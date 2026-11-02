# Zero-loss Tab Vault - Project Scope

Build a browser extension that lets heavy-tab users save work sessions, inspect recovery checkpoints, and restore their tabs from a verified backup. Reliability and recoverability are the core value. The project name expresses a goal; it cannot guarantee survival of device loss without a separate backup.

## Users and problem

Researchers, students, and knowledge workers use tabs as long-running work queues. Browser crashes, extension migrations, and accidental deletion can destroy that context. The first release targets Chrome and Edge desktop users migrating from existing tab managers.

## MVP workflow

1. Grant the permissions needed to capture tabs and choose a backup policy.
2. Save the current window or all windows as a named session.
3. Search sessions and inspect timestamps, window groups, and backup health.
4. Preview a restore, select tabs, and reopen them without deleting the saved session.
5. Export a backup and import it into a clean browser profile.

## Included in the MVP

- Manual capture plus configurable periodic checkpoints; capture URLs, titles, window grouping, pinned state, and tab-group metadata where available.
- Append-only session events, transactional snapshot creation, schema versioning, and checksums for backup integrity.
- A recovery dashboard showing the last successful checkpoint and independent export, with actionable failure messages.
- JSON backup/import, human-readable HTML export, and import of a documented OneTab URL-list format.
- User-approved automatic backup exports, rotation, and a clear distinction between internal checkpoints and independent files.
- Search by title, URL, session name, and capture date; restore previews and duplicate warnings.
- A safety checkpoint before deletion, a recovery bin, and explicit permanent deletion controls.

## Deferred features

Firefox support, user-owned cloud sync, cross-device synchronization, team sharing, AI organization, and paid features follow successful recovery validation. Full page archiving and server-hosted user accounts are outside the MVP.

## Proposed architecture and data

Use TypeScript, React or Preact, Manifest V3, and IndexedDB through Dexie. Separate capture, event storage, snapshot verification, backup/export, and restore planning. Browser API adapters should isolate differences in tab, session, group, storage, and downloads capabilities.

Persist Session, WindowSnapshot, TabSnapshot, SessionEvent, and BackupRecord entities with stable IDs and schema versions. Record capture completion separately from capture start. Never replace a known-good backup before its successor is verified. Retention must preserve the checkpoints needed to recover from an interrupted write.

## Privacy, permissions, and cost

Store browsing data locally, disable private-window capture by default, and request only permissions needed by enabled features. Display export destinations and warn that browser-local storage can disappear with a profile. Offer encrypted portable JSON exports; explain that HTML exports expose their contents. Keep encryption keys out of exports and require successful unlock before import.

Core capture and recovery must work offline with no developer-operated backend, paid inference, or mandatory account. Optional sync must use user-owned storage. Review distribution fees and hosting terms separately before release.

## Acceptance criteria

- A 10,000-tab fixture survives export/import with URLs, grouping, and pinned state intact.
- Interrupted writes and service-worker suspension leave the previous valid checkpoint readable.
- Tampered, truncated, unsupported-version, or incorrectly unlocked backups are rejected before changing stored data.
- Import is transactional; a failed import leaves existing sessions intact.
- Restore opens only the selected tabs, reports unsupported URLs, and preserves the source snapshot.
- Failed automatic exports appear as failed in the dashboard and retain the last valid backup.
- Upgrade, force-kill, accidental deletion, and clean-profile restore scenarios are exercised before release.
- Storage removal is recoverable only from a separately retained export; the interface explains this boundary.

## Delivery and validation

| Phase | Deliverable | Exit condition |
|---|---|---|
| 1 | Capture engine and event store | Representative sessions round-trip without metadata loss. |
| 2 | Verified backups and recovery interface | Failure scenarios preserve a usable recovery path. |
| 3 | Migration, polish, and pilot | Heavy-tab users complete an independent restore. |

Budget 3-5 solo-developer weeks as a planning estimate, excluding store review. Recruit approximately 20 users with previous session loss; measure successful imports, independent restores, trust in backup health, and 14-day retention. Advance only after recovery reliability is demonstrated. Later revenue may come from convenient sync, workspace comparisons, and migration tools while basic backup remains available.

## Scope basis

This scope is derived from the supplied report, "Recent Verified User Pain Points Suitable for Zero-Backend SaaS, Mobile Apps, and Browser Extensions" (research window ending November 3, 2026). Market demand, estimates, and monetization are hypotheses to validate; the report's internal citation markers are not independently verified references. This repository starts with this scope document only; implementation has not started.
