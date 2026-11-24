# Development Prompts Plan - "Beit HaBalonim" (Excluding AI Assistant)

The product specification lives in `SPEC.md` in the repository root (Hebrew). Every prompt below refers to its sections by number. The AI assistant (SPEC section 5.4 and plan item 15) is not part of this file.

## How to work with this file

1. Keep `SPEC.md`, `TASKS.md` and `CLAUDE.md` in the repository root, and the Stitch design in `design/` (`DESIGN.md` and a `screens/` folder with HTML and PNG files). Task 1 creates `CLAUDE.md`, which points to `SPEC.md`.
2. For each task: start with `/plan`, read the plan, approve it, let Claude implement, run the tests, review the changes with `/diff`, commit, and then `/clear` before the next task.
3. Do not start the next task until the current one meets its "Definition of done".
4. If a task needs a decision that is not in `SPEC.md`, Claude must ask instead of guessing.
5. Decisions already made: customer login is passwordless, with one field for phone or email and a one-time code on every login (WhatsApp for a phone number, email for an email address; no SMS and no phone call); the account is created automatically at the first login and the full name is asked at the first order; the admin keeps a password login; WhatsApp is built from the start next to email; deployment is the last task. The Stitch design is ready in `design/` (see Task 6). Still open: the WhatsApp provider (phone login depends on it before launch).

---

## 1. Project foundations

```
You are an expert full-stack developer. Read SPEC.md sections 1, 3, 4 and 4.1 first.

Set up the foundations of the "beit-habalonim" repository.

Requirements:
1. Create a monorepo with `backend/` (FastAPI, Python) and `frontend/` (Vue 3, TypeScript, Vite).
2. Add a `docker-compose.yml` for local PostgreSQL, and a `.env.example` with every setting the project will need (database URL, secrets, email provider, WhatsApp provider, Cloudflare R2). Real secrets are never committed.
3. Add linting and formatting for both sides, and a test runner (pytest for the backend, Vitest for the frontend).
4. Create `CLAUDE.md` in the root with: the stack, a pointer to SPEC.md and TASKS.md, and these standing rules: all user-facing text is in Hebrew and RTL; no SMS and no phone calls anywhere; customers have no passwords (login is by a one-time code); money is Numeric in ILS and never float; prices are always computed on the server; every client-side validation is repeated on the server; time zone is Asia/Jerusalem; every module and page starts with a Developer Header (two sentences in Hebrew and English: purpose and API links); scan LEARNED_LESSONS.md before complex tasks and add an entry after every non-trivial bug. The design lives in `design/` (see Task 6). Product model: every balloon is its own product and its own cart line, a number balloon is its own product (the customer picks the digit), and there are no bouquets, bundles or upgrades.
5. Create an empty `LEARNED_LESSONS.md` (format: description, cause, solution) and a short `README.md` with run instructions.
6. Make sure a `design/` folder exists in the repository root. The user puts `DESIGN.md` and `screens/` there. Never edit files inside it.

Definition of done: both apps start locally, one trivial test passes on each side, lint passes, and no secret is committed.
```

## 2. Database schema and migrations

```
You are an expert backend developer. Read SPEC.md sections 4.1, 5, 6 and 8 first.

Implement the PostgreSQL schema with SQLAlchemy and Alembic.

Core tables:
- users (id, full_name nullable, email nullable unique, whatsapp_phone nullable unique in E.164 format, preferred_channel set automatically at the first login, created_at, last_login_at). Exactly one of email or whatsapp_phone is set when the account is created. Customers have no password.
- admins (id, email unique, password_hash, failed_attempts, locked_until, created_at)
- login_codes (id, identifier, identifier_type, code_hash, ip, attempts, expires_at, used_at, created_at). Codes are keyed by identifier because the account is created only after the code is verified.
- categories (id, name, display_order, image_url, is_visible)
- products (id, category_id, name, description, base_price, in_stock, allows_custom_text, max_text_words, created_at)
- product_images (id, product_id, r2_url, display_order)
- product_option_groups (id, product_id, group_name, is_required, display_order)
- product_options (id, group_id, option_value, price_delta, display_order)
- orders (id, user_id, status, delivery_type, shipping_address, requested_datetime, shipping_fee_snapshot, total_amount, terms_accepted_at, payment_info, created_at, updated_at)
- order_items (id, order_id, product_id, product_name_snapshot, selected_options_snapshot, custom_text, quantity, unit_price_snapshot)
- order_status_history (id, order_id, from_status, to_status, changed_by, created_at)
- order_messages (id, order_id, sender, message_text, attachment_r2_url, file_type, created_at)
- notification_log (id, user_id, channel, template, status, error, created_at)
- store_settings (id, flat_shipping_fee, opening_hours as weekly JSON, store_name, store_phone, store_address)

Requirements:
1. Use Numeric for all money columns and timezone-aware timestamps.
2. Status is an enum with exactly the values and allowed transitions in SPEC section 6. Put the transition rules in one function that the rest of the code must use.
3. Order creation must save a rigid snapshot of product names, selected options and prices, so that later catalog changes never alter past orders.
4. Add indexes for the common lookups (user email and phone, orders by status and date, products by category).
5. Add a seed script with a few categories, products and options for development.
6. Add the Developer Header at the top of every module.
7. Model a number balloon as a regular product with a required option group for the digit (0 to 9). Every balloon is its own product; there are no bundle, bouquet or upgrade tables.

Definition of done: migrations apply on a clean database and can be rolled back, the seed script runs, and tests cover the status transition function.
```

## 3. Notification infrastructure (email and WhatsApp)

```
You are an expert backend developer. Read SPEC.md sections 4, 4.1, 5.6, 5.7 and 7 first.

Build a single notification service used by the whole system.

Requirements:
1. Define one `NotificationService` interface with two adapters: email and WhatsApp (the official WhatsApp Business API, approved message templates). The WhatsApp provider is not chosen yet, so write the adapter against a small provider interface and add a fake provider for development and tests. Do not hardcode a provider.
2. Notifications to a customer always go to the channel of the identifier the customer logged in with (WhatsApp for a phone number, email for an email address). Notifications to the admin go to WhatsApp and email.
3. Define the message templates (in Hebrew): login verification code (a WhatsApp authentication template, and an email), new order request (to admin), order approved with payment link or details, payment received (to admin), order status changed, new chat message (to either side). All messages are transactional, with no marketing content.
4. Send notifications in the background so a slow provider never blocks a request, with a bounded number of retries, and record every attempt in `notification_log`.
5. Read provider keys only from environment variables.
6. Add a Developer Header to every module.

Definition of done: unit tests cover template rendering, channel selection and retry behavior using the fake provider, and a development command can send a test message of each type.
```

## 4. Customer login with one-time codes

```
You are an expert backend developer. Read SPEC.md sections 4.1 and 5.5 first.

Implement customer login in FastAPI as a passwordless flow: one field for phone or email, and a one-time code on every login.

Requirements:
1. One endpoint accepts an identifier: an Israeli mobile phone number or an email address. Normalize it (phone to E.164, email to lowercase) and reject anything else.
2. Send a 6-digit code through the notification service: a phone number gets it on WhatsApp, an email address gets it by email. There is no SMS and no phone call. The code is stored hashed, valid for 10 minutes, single-use, and keyed by identifier.
3. A second endpoint verifies the code. After a correct code: if no account exists for the identifier, create it automatically (full_name stays empty, preferred_channel is set from the identifier type); then start a session and set last_login_at. There is no separate registration and no password for customers.
4. Limit failed attempts per code (the code is cancelled after a small number of wrong tries), limit code requests per identifier and per IP per hour, and add a short cooldown before a resend. Keep all numbers in settings.
5. The response to a code request is identical whether or not an account exists.
6. Session handling: propose the approach and explain it before implementing (recommended: JWT in an httpOnly, secure cookie, with CSRF protection and a configurable lifetime, default 30 days). Add logout, a "current user" endpoint, and an endpoint to save the full name.
7. Add a Developer Header to every module listing its endpoints.

Definition of done: tests cover both identifier types, automatic account creation on the first login and reuse on the next, expired and reused codes, wrong codes and attempt limits, rate limits, the identical response for existing and missing accounts, and logout.
```

## 5. Admin account and login

```
You are an expert backend developer. Read SPEC.md sections 6 and 8 first.

Implement the single admin account.

Requirements:
1. There is no admin registration. Create the admin through a CLI script or environment variables, with a strong password enforced.
2. Admin login uses a separate endpoint and is blocked for 15 minutes after 5 failed attempts.
3. Admin accounts live in the `admins` table, separate from customers. Every admin endpoint requires an authenticated admin, checked on the server. Customers have no passwords and can never become admins.
4. Add an authorization test that proves a regular customer cannot reach any admin endpoint.

Definition of done: tests cover lockout, lockout expiry, and role enforcement.
```

## 6. Frontend foundations, design tokens and homepage

```
You are an expert frontend developer. Read SPEC.md sections 1, 2, 3 and 5.1 first.

Build the Vue 3 (TypeScript) client foundations with full RTL support.

Requirements:
1. Set up Vite, Vue Router, Pinia, an API client, SCSS or Bootstrap, and the RTL layout (`dir="rtl"`, Hebrew only).
2. The design is ready in `design/`: read `DESIGN.md` fully and look at the PNG files in `design/screens/`. Define all colors, fonts, spacing, radii and shadows as CSS variables in one design-tokens file, with values taken from the prose sections of `DESIGN.md` (Colors, Typography, Layout, Elevation, Shapes, Components). Do NOT use the machine-generated color list at the top of the file where it conflicts with the prose (for example its lavender page background): the page background is the warm cream `#FFFBF7`, and primary buttons are `#C93F5B`. Use `#5F6170` for out-of-stock text. Use the screen HTML only as a visual reference (it is Tailwind) and never copy it. Ignore anything in `DESIGN.md` or in the screens that is not in SPEC.md (bouquets and bundles, a greeting-card component, bestseller, new and quality badges). If the design and SPEC.md disagree, SPEC.md wins, and if unsure, ask me. Components use only the token variables.
3. Build the main layout: header (logo linking home, search bar, account, cart), category navigation menu, footer (store details, opening hours, links to privacy and terms, "by Created RACHEL" credit).
4. Build the homepage: main banner and category grid.
5. Add the floating feedback and bug-report bubble (bottom left) with the "site in development" message. For the AI button (bottom right), leave only an empty placeholder slot and do not implement it.
6. Build the login modal described in SPEC section 5.5: a title, a one-line subtitle, a single field labeled "phone or email" (Israeli phone numbers and emails, with client-side validation), a "send code" button, then a code-entry step with a resend button after a short cooldown, and the privacy-policy note with a link. Open it from the header account button, and connect it to the endpoints from Task 4 (use a fake API if Task 4 is not done yet).
7. Create a Developer Header component shown only in development mode, and use it on every page and component.
8. Mobile-first and responsive.

Definition of done: the app runs, the homepage renders in RTL on mobile and desktop widths, and tokens are the only source of design values.
```

## 7. Catalog, search and product page

```
You are an expert full-stack developer. Read SPEC.md sections 5.2 and 5.3 and 4.1 first.

Implement the product catalog, backend and frontend.

Requirements:
1. Backend endpoints: list categories, list products with free-text search and filters by category and budget, product detail with option groups and options.
2. One pricing function on the server computes the unit price: base price plus the price deltas of the selected options. All other code (cart, orders) must use it.
3. Product page: show images (a placeholder illustration when there is no image), required and optional option groups, and a flat list of independent options with price deltas (size, color, number, font, letter color). A number balloon is its own product page with a digit picker (0 to 9) plus color and size; it is never an add-on inside another product. Every balloon is a separate cart line.
4. Custom text mailers: up to 4 words, validated on the client and again on the server. Only the size option affects the price.
5. Out-of-stock products are hidden from customers (or marked, according to SPEC 5.2) and cannot be added to the cart.
6. Products without options can be added to the cart directly.

Definition of done: tests cover the pricing function (including options and custom text), search and filters, and out-of-stock behavior.
```

## 8. Cart, order request and order history

```
You are an expert full-stack developer. Read SPEC.md sections 4.1, 5.5 and 5.6 first.

Implement the cart, the checkout and the order request.

Requirements:
1. Only logged-in customers can place an order. If the customer is not logged in, show the login modal in place and continue the checkout afterwards.
2. The cart holds product ids, selected options, custom text and quantity. It never holds prices. On every view and on submit, the server recomputes all prices with the pricing function.
3. Checkout: delivery type (local pickup or flat-rate local delivery, with the fee from `store_settings`), a free requested date and time (must be in the future, Asia/Jerusalem), the customer's full name (asked here if the account has none yet, then saved on the account), contact details, and a mandatory checkbox for terms and privacy policy (save the time of acceptance).
4. Submitting creates an order with status "pending approval" without payment, saves the full snapshot of items, options, custom text and prices, writes the first status history row, and notifies the admin (new order request).
5. Show the customer the confirmation message from SPEC 5.6 (request sent for availability approval; call the store if there is no response).
6. Order history page for the customer: list and detail of their own orders only.

Definition of done: tests cover server-side price recomputation (a tampered client price is ignored), the snapshot staying unchanged after a product edit, the future-date validation, and access control (a customer cannot read another customer's order).
```

## 9. Order chat and attachments

```
You are an expert full-stack developer. Read SPEC.md sections 4, 5.7 and 8 first.

Implement the chat inside each order.

Requirements:
1. Each order has a message thread between the customer and the admin. The customer reaches it from the order history, and the admin from the order page.
2. Messages can contain text and attachments (images and PDF only, up to 5MB). Verify the real MIME type on the server, not just the file extension, and reject anything else.
3. Store attachments in Cloudflare R2 and serve them only through authorized access (no public listing of files). Only the order's customer and the admin can read them.
4. When one side writes, notify the other side through the notification service (customer by the channel they logged in with, admin by WhatsApp and email).
5. Rate limit message sending.

Definition of done: tests cover MIME and size validation, access control to messages and files, and notification triggering.
```

## 10. Admin panel: catalog, images and settings

```
You are an expert full-stack developer. Read SPEC.md sections 5.2 and 6 first.

Implement the catalog management part of the admin panel (admin only).

Requirements:
1. Full CRUD for categories and products (name, description, price, category, multiple images, display order), plus ordering and a show/hide switch for each category.
2. Product option groups and options with price deltas, and a setting for products with custom text (allowed, maximum number of words).
3. For each product the admin chooses which parameters the customer must select.
4. An "In stock / Out of stock" toggle for each product.
5. Image upload to Cloudflare R2 with type and size validation.
6. A settings page: flat shipping fee, weekly opening hours and store details.
7. Deleting a product that appears in past orders must not break those orders (the snapshot keeps them intact).

Definition of done: tests cover authorization, CRUD, the stock toggle and deletion of a product that exists in a past order.
```

## 11. Admin panel: orders, statuses and payment

```
You are an expert full-stack developer. Read SPEC.md sections 5.6, 6 and 7 first.

Implement order management in the admin panel (admin only).

Requirements:
1. Orders dashboard sorted by date and urgency (requested date), with the full order details and snapshot, and the order chat.
2. Status changes go only through the transition function from Task 2 (SPEC section 6). Every change writes a row to `order_status_history` and sends a status notification to the channel the customer logged in with.
3. Approve: the admin pastes the Bit payment link or the payment details (the system does not generate a Bit link). Save them on the order (`payment_info`), move the order to "approved, awaiting payment", and send the customer a message with them. Local pickup customers may pay cash in the store.
4. Reject: moves the order to "cancelled" with an explanation that is also posted in the order chat.
5. Manual payment confirmation: after the admin confirms the Bit transfer (or cash), the order moves to "paid, in preparation". The admin is notified of payment events.
6. Later statuses: ready for pickup or out for delivery, then completed. Unpaid orders are never cancelled automatically; the admin cancels manually.

Definition of done: tests cover every allowed transition, rejection of every forbidden transition, history rows, and notification triggers.
```

## 12. Legal pages and data retention

```
You are an expert full-stack developer. Read SPEC.md section 8 first.

Requirements:
1. Create the privacy policy and terms pages with placeholder Hebrew text and a visible note in the code that the final wording must be reviewed by a legal professional.
2. Link both pages from the footer and from the checkout consent checkbox.
3. Add the delete function for customer data: a button for the customer (own account and orders) and for the admin (any customer), with confirmation.
4. Add a retention setting in the configuration and a scheduled cleanup job structure. The AI conversation cleanup is out of scope for now.

Definition of done: pages are reachable, deletion removes personal data and is covered by tests, and the retention settings are documented in the README.
```

## 13. Testing and security hardening

```
You are an expert full-stack developer and security reviewer. Read SPEC.md sections 4.1, 8 and 10 first.

Requirements:
1. Fill gaps in the test suite: pricing, order and status flow, validation, notifications, chat access control, and rate limits. Report overall coverage.
2. Security review of the whole project: CORS, CSRF, secure headers, cookie flags, input validation, file upload handling, authorization on every endpoint (customer vs admin vs anonymous), and secrets handling. List findings by severity and fix the high ones.
3. Confirm that no card data is stored and that there is no SMS code or dependency anywhere.
4. Add the discovered problems and fixes to LEARNED_LESSONS.md.
5. Run an accessibility pass on the main screens: color contrast (WCAG AA, 4.5:1), focus order in RTL, minimum touch target of 44px, and labels on all form fields. Fix what fails.

Definition of done: all tests pass, the findings list is written to a file, and high-severity items are fixed.
```

## 14. Deployment and operations

```
You are an expert DevOps-minded developer. Read SPEC.md sections 4 and 9 first.

Hosting, domain and backup choices are not decided. Before writing anything, present two or three realistic options with approximate monthly cost, setup effort and trade-offs, recommend one for a small store run by one person, and wait for my decision.

After the decision, implement:
1. Production configuration and a deployment procedure (documented step by step in the README).
2. Domain and HTTPS setup instructions.
3. Production secrets management through environment variables, never committed.
4. Automatic database backups with a documented restore test.
5. Basic logging, error monitoring and a health endpoint.
6. A simple CI pipeline that runs lint and tests on every push.
7. A pre-launch checklist: WhatsApp provider and templates approved (including the authentication template for login codes, since phone login depends on it), email sending domain verified, Bit payment details configured, admin account created, legal pages reviewed, and a full end-to-end test order.

Definition of done: the site runs on the chosen host over HTTPS, a backup was restored successfully at least once, and the checklist is complete.
```
