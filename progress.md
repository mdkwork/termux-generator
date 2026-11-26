# Progress

## Task: Fix `--display-name` patch on branch `V3`

Status: **implemented and locally verified; CI validation triggered**

- [x] Diagnosed latest workflow run `45749718371`:
  - workflow passed `--display-name "Termux MoMo"` as a single argument;
  - `patch_apps()` ran, but patched the wrong `strings.xml` because
    `grep "termux-app"` matched the whole `termux-apps-main/` prefix;
  - the patch ran before `replace_termux_name()`, which rewrote the
    `TERMUX_APP_NAME` entity value to the custom package name.
- [x] `scripts/termux_generator_steps.sh`: move the display-name patch after
  the package rename, target the exact app `strings.xml` for both `f-droid`
  and `play-store`, and replace the exact `<!ENTITY TERMUX_APP_NAME "...">`
  declaration with an XML-escaped value.
- [x] `.github/workflows/build-termux.yml`: pass all workflow inputs through
  job-level `env`, remove redundant `Set --*` steps, and build the argument
  array with quoted `"$VAR"` values.
- [x] Local fixture tests: f-droid `MDK Termux`, play-store `Termux MoMo`, and
  XML special characters — all passed. `bash -n` and `git diff --check` clean.
- [ ] Await result of the triggered `workflow_dispatch` run on `V3`.
