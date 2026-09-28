# Camping Menu — October 3

Static GitHub Pages app, retaining the existing Firebase Realtime Database and menu identifiers.

- Original autumn picnic cover, October 3 date, responsive chat/menu layout.
- General chat at `generalChat`; menu conversations remain at `menus/{id}/comments`.
- Legacy string comments remain readable. New messages contain text, user, timestamp and optional image.
- Photos are converted to JPEG, resized to at most 1000px and capped at 300000 data-URL characters (about 225 KB). Recent 100 messages are loaded per room. HEIC is supported only when the browser can decode it; otherwise the UI requests JPEG/PNG.
- Likes live at `menus/{id}/likes/{username}`. Transactions preserve concurrent writes and avoid resurrecting deleted menus.
- Organizer UI can remove all menus/messages; regular UI exposes own-item deletion only. Confirmation precedes deletion.

## Important existing security limitation

The original app uses client-side username/PIN verification and a directly accessible Realtime Database. This revision preserves compatibility. Organizer login uses a strong password verified through PBKDF2-SHA256 (210000 iterations); only its salted verifier is in source. **This is an organizer UI convenience, not server-enforced authorization.** Client code and requests can be changed by a determined visitor. No Firebase Authentication identity, privileged server, or enforced database rules were provisioned in this change. Before wider public use, migrate legacy users to Firebase Authentication and deploy database rules enforcing ownership and an admin claim/UID. Do not deploy restrictive rules independently of the compatible authentication migration.

Passwords are no longer persisted in browser localStorage. The existing users database still follows the legacy PIN scheme. Administrator credential was provided privately to the project owner, not committed.

## Validation

Syntax check and isolated DOM/in-memory Firebase integration checks: legacy login, invalid PIN, like/unlike, general and menu messaging, legacy comments, image-only messages, text escaping, ownership UI, organizer verification/deletion, no stale-menu resurrection, no password persistence. These checks do not verify production Firebase authorization rules. Existing live menu content is retained.
