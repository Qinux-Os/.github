# .github

Organization-wide GitHub configuration. Anything in this repository applies to
**every** Qinux repository automatically.

## What lives here

| Path | Purpose |
| --- | --- |
| [`ISSUE_TEMPLATE/`](ISSUE_TEMPLATE/) | Issue forms used by all repositories |
| [`PULL_REQUEST_TEMPLATE/`](PULL_REQUEST_TEMPLATE/) | Pull request template |
| [`workflows/`](workflows/) | Checks that run on every repository |
| [`.markdownlint.yml`](.markdownlint.yml) | Markdown lint rules |

## Issue templates

| Form | Use for |
| --- | --- |
| [`bug_report.yml`](ISSUE_TEMPLATE/bug_report.yml) | Something does not work |
| [`feature_request.yml`](ISSUE_TEMPLATE/feature_request.yml) | Suggest new behaviour |
| [`question.yml`](ISSUE_TEMPLATE/question.yml) | Ask how to do something |

[`ISSUE_TEMPLATE/config.yml`](ISSUE_TEMPLATE/config.yml) disables blank issues
and links out to security advisories, discussions, and the RFC process.

Blank issues are disabled deliberately. A form takes thirty seconds and saves
the maintainers ten minutes of asking for the same information.

## Organization checks

[`workflows/repository-checks.yml`](workflows/repository-checks.yml) runs on
every push and pull request in every repository:

- **Markdown** lint, using the rules in `.markdownlint.yml`
- **LICENSE** must exist and must not be empty
- **EditorConfig** no CRLF, no trailing whitespace, newline at end of file

These are deliberately boring. They catch the mistakes that generate noisy
review comments.

Repository-specific workflows do the real work: building the kernel, compiling
packages, producing ISOs.

## Adding a check

Prefer a composite action over copy-pasting a workflow into every repository.
If a check genuinely applies organization-wide, add it here and it runs
everywhere.

A new check must:

- Pass on all 51 repositories as they currently stand.
- Fail with a message that says what to fix.
- Not require secrets in order to run on a fork.

## Local testing

```sh
pip install pymarkdownlnt
pymarkdownlnt --config .github/.markdownlint.yml scan

# what CI checks for line endings and trailing whitespace
git ls-files -z | xargs -0 grep -IlP '\r$|\s+$'
```

## Notes

- Templates here are inherited by every repository in the organization.
- A repository can override an inherited template by defining its own.
- Do not add secrets to workflows in this repository. It is public.
