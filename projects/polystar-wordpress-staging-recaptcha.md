# Polystar WordPress Staging reCAPTCHA Setup

Date: 2026-05-26

Context:
- Staging site: `https://polystar.zestdigital.com/`
- Production site: `https://www.polystar.co.uk/`
- Staging SSH user/site path used during testing: `polystar-staging`, `/home/polystar-staging/webapps/polystar-staging`
- Do not update or change production until staging steps are intentionally reproduced.

Confirmed staging setup:
- Gravity Forms contact forms work with reCAPTCHA v3 through `gravityformsrecaptcha`.
- WooCommerce checkout works with reCAPTCHA v2 through `recaptcha-woo` version `1.4.7`.
- WooCommerce account/login/register forms are covered by `recaptcha-woo` v2.
- Wordfence reCAPTCHA v3 can cover WP login/admin login style flows, but did not protect checkout account creation.

Important findings:
- Gravity Forms v3 stores score on GF entries, for example `gravityformsrecaptcha_score = 0.9`.
- `recaptcha-woo` v2 is pass/fail and does not store a score on Woo orders.
- Wordfence stores CAPTCHA scores on users only for its own protected flows; it did not add score/order meta for checkout-created accounts.
- Wordfence WooCommerce integration appears to target My Account registration/login, not checkout `createaccount` during order placement.
- Test order `#36891` proved checkout + Stripe test payment + `recaptcha-woo` v2 worked on staging.

Current staging `recaptcha-woo` options after successful test:
- `rcfwc_tested = yes`
- `rcfwc_woo_checkout = on`
- `rcfwc_woo_checkout_pos = beforepay`
- `rcfwc_woo_login = on`
- `rcfwc_woo_register = on`
- `rcfwc_guest_only = off`
- v2 site/secret keys present for `polystar.zestdigital.com`

Recommended production reproduction:
1. Create production v2 reCAPTCHA key for `www.polystar.co.uk`.
2. Install/activate `recaptcha-woo` on production only after backup.
3. Configure Woo checkout, Woo login, and Woo registration protection.
4. Keep Gravity Forms v3 for forms using production v3 key.
5. Keep Wordfence v3 for WP login/admin login protection if desired.
6. Avoid double-protecting Woo account forms with both Wordfence and `recaptcha-woo` unless tested; prefer `recaptcha-woo` for Woo flows.
7. Test checkout with account creation, My Account registration/login, lost password if enabled, and all public Gravity Forms.

Related staging plugin update caveats from same rollout:
- Keep `jquery-updater` at `3.7.1.3`; version `4.0.0` broke Woo checkout JS.
- Keep `uk-address-postcode-validation` at `3.3.0`; version `4.3.0` broke Ideal Postcodes lookup.
- Stripe plugin worked at `woocommerce-gateway-stripe 10.4.0`; `10.7.0` caused Stripe `clover`/`apiVersion` JS failure.

Production rollout note, 2026-05-27:
- Full prod backup made at `/home/polystar/update-backups/prod-rollout-20260527T024110Z`.
- Safe plugin batch applied to production; known holds kept: `jquery-updater 3.7.1.3`, `uk-address-postcode-validation 3.3.0`, `woocommerce-gateway-stripe 10.4.0`.
- Contact form submit and test order were verified after safe batch.
- Gravity Forms updates are blocked by expired client license: `gravityforms 2.8.18 -> 2.10.2` and `gravityformsrecaptcha 1.6.0 -> 2.2.2` both fail with `Update package not available`.
- GF console errors may remain until the license is renewed, but forms still submitted during production smoke test.
