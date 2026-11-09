> **ℹ️ Unofficial build for ownCloud 11.**
> This is a fork of [owncloud/impersonate](https://github.com/owncloud/impersonate),
> which was archived by ownCloud in July 2026. All credit belongs to the original
> authors (Jörn Friedrich Dreyer, Sujith Haridasan and the ownCloud contributors).
> Licensed under the AGPL-3.0, like the original.

## Why this fork exists

Before archiving the repository, ownCloud prepared version 0.7.0 for ownCloud 11
on `master` ([#251](https://github.com/owncloud/impersonate/pull/251),
[#254](https://github.com/owncloud/impersonate/pull/254)), but never published it.
The latest official release, 0.6.1, declares `max-version="10"`, so ownCloud 11
refuses to enable it, and the marketplace no longer offers it.

This fork only publishes a ready-to-install tarball of that unreleased upstream code.

**What is different from upstream:**

- **The app code is unchanged.** PHP, JavaScript, CSS, templates and translations
  are byte-for-byte the upstream `master` (commit `8b137cc`), which in turn is
  identical to 0.6.1 apart from `appinfo/info.xml`.
- This README and a CHANGELOG entry.
- **The tarball is not signed.** Only ownCloud can sign apps, so the tarball ships
  without `appinfo/signature.json`. ownCloud skips the integrity check for
  non-shipped apps that have no signature file, so no integrity warning appears.

**Tested on:** ownCloud 11.1.0.1 (official `owncloud/server` Docker image),
PHP 8, MariaDB 10.11. The same code has been in daily production use there
since November 2026.

## Disclaimer

This is **not** an official ownCloud release, and it is **not actively maintained**.
I publish it in the hope that it is useful to other ownCloud 11 administrators.
No new features or security fixes should be expected. Impersonate lets
administrators act as any user, so review the code before installing it, like
any other app with that level of access.

## Installation

1. Download `impersonate.tar.gz` from the
   [latest release](https://github.com/DigiSoulBcn/impersonate/releases/latest).
2. Extract it into your apps directory. With the official Docker image, that is
   the custom apps directory `/mnt/data/apps`:

   ```sh
   tar -xzf impersonate.tar.gz -C /mnt/data/apps
   chown -R www-data:www-data /mnt/data/apps/impersonate
   ```

3. Enable it:

   ```sh
   occ app:enable impersonate
   ```

**Upgrading from a patched 0.6.1** (with `max-version` edited by hand): remove the
old folder and extract the new one in its place. If you kept the original
`appinfo/signature.json` of 0.6.1, delete it, otherwise the integrity check will
report the edited `info.xml`.

---

# Impersonate

The Impersonate application allows administrators, and group admins, to log in as another user within an ownCloud instance. It provides a helpdesk-like experience and can be useful to help users with configuration issues, to get a better understanding of what they see when they use their ownCloud account, or to perform actions in legacy accounts.

Once Impersonate is installed, a new column will be available in the user management panel. Click on the icon next to the user that you want to impersonate and you will be logged in as that user. Your current session will be temporarily suspended, and you will see a notification at the top of the page reminding you that you’re impersonating another user. Once you’re finished, log out, and you will return to your previous user session.

As a security measure, the application lets ownCloud administrators restrict the ability to impersonate users in specific groups. When enabled and configured, only a group’s administrator can impersonate members of their group. Administrators can find configuration options in the "User Authentication" section of the "Admin settings" panel.

## Building from source

For development, execute `make build-dep; make js-templates`
To create distribution tar file, execute `make dist`

# Known limitations
- If you impersonate a user that has never logged in, the filesystem cannot be initialized (that requires a proper login). As a result you will only see an error page, no matter what app you try to use. You have to kill the cookie to log out. Maybe add a logout link to error pages?
