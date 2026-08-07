# AGENTS.md

Review guidance for automated code review on this repository.

## What this repo is

TacLinX by Omega Point Solutions — secure field operations & task force coordination for law enforcement. Public capabil

Primary language: HTML.

## Rank findings in this order

1. **Fail-open defects.** A check that returns "permitted", "clean", "safe" or
   "GO" because data was missing, a match failed, a label was unparseable, or an
   exception was swallowed. Silence that renders as approval is the worst defect
   class here and outranks everything below. If a guard cannot evaluate its
   input, it must say so — not pass.
2. **Secret and credential handling.** Secrets on a command line, in a log, in
   an error message, or committed in plaintext. Secrets belong in the SOPS vault
   and reach a process via `sops exec-env` and a script FILE reading `$env:VAR`,
   never an inline quoted one-liner.
3. **Correctness bugs with a concrete failing input.** Name the input. If you
   cannot name it, say the finding is unconfirmed rather than dressing it up.
4. **Tests that assert nothing**, or that were weakened to make a failure pass.

## House rules this repo is held to

- **A claim must not be wider than what was checked.** A finding label, a UI
  string, or a docstring that asserts more than the code establishes is a
  defect, not a wording nit. This has been the single most common real bug here.
- **Deploy, commit, and push are one unit.** Flag anything that would leave the
  tree deployed-but-uncommitted or committed-but-unpushed.
- **Destructive operations verify their targets first.** A regex or filter that
  selects rows for deletion must be shown to select the right ones; a surprising
  count is a reason to stop, not to proceed.

## Stack-specific

- Static sites here are public marketing surfaces. Flag any credential, internal
  hostname, private endpoint, or trade-secret detail in committed markup, JS, or
  comments.
- User-supplied or fetched text rendered into the DOM must be escaped.

## Skip entirely

Style, naming, formatting, import order. Do not praise. If the diff is clean,
say so in one line — a short review is a valid outcome and padding it wastes the
reviewer's attention.
