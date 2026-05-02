# fsociety vpn documentation

This repository contains the canonical user-facing legal documents for [fsociety vpn](https://github.com/t3rmynal/fsociety-vpn). The client application fetches these files as raw markdown on every launch (with a one-hour local cache and an embedded fallback for offline use), so the version you see here is always the version every user is reading.

## Files

| file | what | rendered in client |
|------|------|--------------------|
| [`privacy.md`](privacy.md) | Privacy policy. What is collected, what is not, retention, third-party processors, data subject rights. | F1 from the home screen |
| [`terms.md`](terms.md) | Terms of service. Acceptable use, prohibited activities, refunds, liability, dispute resolution. | F2 from the home screen |

## Editing

These documents are read by every user automatically on every launch. Treat changes with the same care as production deployments.

- Open a pull request rather than committing directly. Material changes should be tagged `material` and announced in the project Telegram channel before merge.
- Keep the `Last updated` date at the top of each file in sync with the merge date.
- Avoid breaking changes to section headings without a transition period (the client renders these as part of the navigation).
- Both documents are written in plain English. Translations are welcome as separate files (`privacy.de.md`, `privacy.ru.md`), but the English version is authoritative.

## Versioning

These files are not versioned independently. The git commit hash and last-updated date in each file constitute the version identifier. The client surfaces the last-updated date in its header.

## License

These documents are public and may be reused as templates for similar projects under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Attribution to fsociety vpn is appreciated but not required.
