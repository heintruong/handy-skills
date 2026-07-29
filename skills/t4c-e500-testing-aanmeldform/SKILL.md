---
name: t4c-e500-testing-aanmeldform
description: Use when testing T4C E500 aanmeldform registration, anonymous quick-registration links, email-confirmation flows, account provisioning, group membership, or assigned content on local IIS.
---

# T4C E500 Aanmeldform Testing

Test the requested registration behavior end to end on local IIS. Preserve test evidence and distinguish the three registration types.

## Scope

Determine from the conversation which type is required:

1. Anonymous quick link, no email confirmation.
2. Aanmeldform with email confirmation.
3. Aanmeldform without email confirmation.

If unclear, ask which type to test. If the user requests all types, run them sequentially and continue automatically after each passing case. In guided mode, pause at the checkpoints the user requests.

## Local setup

- Use `http://e500.asp.test`.
- Keep separate cookie jars for admin, public registration, and learner sessions.
- When redirected to `c1_kiesklant.asp`, prefer the tenant already named in the conversation. Otherwise select an available `www_e500_nl`, `www_vrouw...`, or `mijn_dinsdag_nl` option. Ask if none is available.
- Use local login shortcuts only when appropriate; discover the correct admin or learner identity from project instructions.
- Scan every relevant response for HTTP 500 and `Microsoft VBScript`, `ADODB`, `Type mismatch`, `Syntax error`, and `Provider error`.

## Test form

- Reuse a suitable existing test form when possible.
- Name a new form `T4C_TEST_<JIRA>_AANMELDFORM`; omit `<JIRA>_` when no Jira task is known.
- Keep the test form hidden from general listings when a direct link/code is sufficient.
- Never delete the test form or test accounts unless the user explicitly asks.
- Treat empty/NULL form start and end dates as accessible. Verify future or expired ranges are blocked when date behavior is in scope.
- Preserve unrelated form settings when updating it.
- Email-confirmation and skip-confirmation modes cannot be active simultaneously. Use separate suitable forms, or carefully switch the existing form between cases and verify that all unrelated settings remain intact.
- Never commit repository changes unless the user explicitly authorizes the commit.

## Run the selected flow

### Anonymous quick link

Open the configured quick link, complete `toegang_bevestig.asp`, and submit the confirmation action. Verify immediate account creation without email confirmation. Common local behavior is username `anoniem_<id>`, email `<username>@e500.nl`, and first name `Anoniem`.

### Form with email confirmation

Submit unique valid field values. Local does not send mail: use the development-only confirmation link printed in the response, or extract it from the response, then complete the confirmation page. With no configured username field, current E500 behavior uses the submitted email as username in this flow; verify that unless the task explicitly changes it.

### Form without email confirmation

Submit unique valid field values and follow the immediate `aanmelden_bevestig.asp?...&auto_login=1` flow. When no username field is configured, verify a generated username such as `aanmelder_<id>` and confirm the submitted email remains unchanged.

For a configured username field, verify its submitted value remains the username. Verify the admin UI allows no username field and still permits at most one username field.

## Verify provisioning

Check both stored data and usable learner behavior:

- Exactly one participant exists for the registration.
- Username, email, name, and other submitted fields match the selected flow.
- The configured group contains the participant.
- Configured main content is assigned. Use the full admin content edit page; do not treat an include fragment as a standalone verification page.
- Log in as the new learner. Complete local AVG consent if redirected to `check.asp?msg=avg`.
- Open assigned CBT content with its `cbt_id` or `dcbt_id`. Calling `my_assignments.asp` without either can legitimately redirect with `e=gc1`.
- Confirm the learner can see/open the configured content and no runtime error is present.

## Report

Report the tenant, form ID/name, flow types, created participant IDs, username pattern, email behavior, group/content results, runtime errors, and any test artifacts left behind. Remove only temporary cookie/response files. State explicitly whether any commit or skill change occurred.
