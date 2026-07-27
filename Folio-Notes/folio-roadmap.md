# Folio — Development Roadmap

Folio is a lightweight, native macOS clipboard manager. It maintains a persistent local history of copied text and images, exposed through a keyboard-first, minimal interface — the clipboard history macOS still doesn't ship with.

The goal is reliability first, features second. Every milestone should improve confidence that clipboard history is captured accurately, stored safely, and retrievable instantly.

---

# Milestone 1 — Project Foundation

## Project

- [ ] Swift Bundler project
- [ ] SwiftUI application
- [ ] Menu bar application
- [ ] Launch at login support
- [ ] Background operation
- [ ] Preferences window
- [ ] Status menu
- [ ] Logging system

### Notes

- Keep clipboard polling and storage off the main thread.
- Build around a modular pipeline (capture → store → index → present) rather than one large clipboard manager.
- Every subsystem should be independently testable.

---

# Milestone 2 — Clipboard Capture

## Monitoring

- [ ] NSPasteboard polling loop
- [ ] Change-count detection
- [ ] Text capture
- [ ] Image capture
- [ ] File reference capture
- [ ] RTF/plain-text distinction
- [ ] Source application detection
- [ ] Duplicate suppression

### Notes

Poll at the lowest interval that still feels instantaneous.

Never capture from apps flagged as private (e.g. password managers) — respect `org.nspasteboard.ConcealedType` and similar markers.

---

# Milestone 3 — Local Storage

## Persistence

- [ ] SQLite (or Core Data) backing store
- [ ] Text entry schema
- [ ] Image entry schema (thumbnail + full)
- [ ] Timestamp + source app metadata
- [ ] Write batching
- [ ] Storage location configuration
- [ ] Database migration support

### Notes

Avoid writing every keystroke-adjacent event to disk — only finalized clipboard changes.

All storage stays local; no network calls, ever.

---

# Milestone 4 — History Management

## Data Lifecycle

- [ ] Configurable history length
- [ ] Configurable retention window
- [ ] Manual delete (single entry)
- [ ] Clear all history
- [ ] Pin/favorite entries
- [ ] Auto-prune old entries
- [ ] Storage size cap
- [ ] Exclude-list (apps/domains never recorded)

### Notes

Pinned entries should be exempt from auto-pruning and size caps.

Deletion should be immediate and irreversible — no silent soft-deletes lingering on disk.

---

# Milestone 5 — Search & Indexing

## Retrieval

- [ ] Full-text search index
- [ ] Fuzzy matching
- [ ] Search by source app
- [ ] Search by content type (text/image/file)
- [ ] Search by date range
- [ ] Incremental indexing on capture
- [ ] Ranked results (recency + relevance)

### Notes

Indexing should never block capture — new entries must be recorded even if indexing is behind.

Search should feel instant at every history size up to the configured cap.

---

# Milestone 6 — Keyboard-First Interface

## Interaction

- [ ] Global hotkey to open history
- [ ] Arrow-key navigation
- [ ] Type-to-search
- [ ] Enter to paste
- [ ] Escape to dismiss
- [ ] Numeric quick-select (1–9)
- [ ] Paste-and-restore-focus to prior app
- [ ] Optional mouse interaction (secondary, not required)

### Notes

Every action reachable by mouse must also be reachable by keyboard. The reverse is not required.

Opening the history window should never steal focus longer than necessary to make a selection.

---

# Milestone 7 — Paste Behavior

## Output

- [ ] Paste as plain text
- [ ] Paste as original format
- [ ] Paste image
- [ ] Simulated Cmd+V injection
- [ ] Accessibility permission handling
- [ ] Paste failure fallback (copy to active clipboard)

### Notes

If simulated paste fails or is blocked by the target app, fall back to placing the item on the system clipboard and notifying the user, rather than failing silently.

---

# Milestone 8 — User Interface

## Preferences

- [ ] History window layout
- [ ] Appearance (light/dark, compact/comfortable)
- [ ] Hotkey customization
- [ ] Retention & storage settings
- [ ] Excluded apps list
- [ ] Privacy settings (concealed-type handling)
- [ ] Diagnostics panel

---

# Milestone 9 — Visualization & Feedback

## Live Feedback

- [ ] Capture indicator (menu bar flash/subtle cue)
- [ ] Entry type icons (text/image/file)
- [ ] Image thumbnail previews
- [ ] Content preview on hover/focus
- [ ] Character/word count for text entries
- [ ] Debug overlay (last N captures, timing)

### Notes

Feedback should be subtle — Folio stays out of the way unless summoned.

---

# Milestone 10 — Evaluation

## Testing

- [ ] Capture accuracy test (text/image/file across common apps)
- [ ] Duplicate suppression test
- [ ] Search relevance test
- [ ] Latency test (capture-to-index, hotkey-to-visible)
- [ ] Storage growth test over extended use
- [ ] Paste-back fidelity test (formatting preserved)
- [ ] Save reports

### Notes

A build should not be considered "stable" until it passes an extended real-world capture session without missed or corrupted entries.

---

# Milestone 11 — Profiles & Sync (Local Scope Only)

## Data Portability

- [ ] Export history (encrypted archive)
- [ ] Import history
- [ ] Multiple storage locations (e.g. external volume)
- [ ] Per-Mac independent histories

### Notes

No cloud sync in scope. If sync is ever pursued, it must be opt-in, end-to-end encrypted, and a separate milestone entirely — not a default behavior.

---

# Milestone 12 — Reliability

## Edge Cases

- [ ] Large clipboard payloads (multi-MB images/files)
- [ ] Rapid successive copies
- [ ] Clipboard cleared by another app
- [ ] App quits mid-copy
- [ ] Sleep/wake clipboard state
- [ ] Concealed/private pasteboard types
- [ ] Corrupted database recovery
- [ ] Disk-full handling

### Notes

Every skipped or degraded capture is preferable to a corrupted history or a crash.

---

# Milestone 13 — Advanced Features

## Nice-to-Have

- [ ] Snippet templates (static reusable text)
- [ ] Text transformation on paste (case, trim, strip formatting)
- [ ] Tagging/organizing entries
- [ ] Multi-item merge/combine paste
- [ ] Shortcuts app integration
- [ ] AppleScript support
- [ ] Plugin action API for custom paste transforms

---

# Milestone 14 — Automated Tests

## Testing

- [ ] Capture pipeline unit tests
- [ ] Storage layer tests
- [ ] Search/index tests
- [ ] Paste-behavior tests
- [ ] Retention/pruning tests
- [ ] UI tests
- [ ] Performance benchmarks

### Notes

Replay recorded clipboard event sequences through the capture pipeline.

Tests should produce identical stored entries every run.

---

# Future Research

- [ ] Cross-device sync (opt-in, end-to-end encrypted)
- [ ] Smart de-duplication across similar (not identical) entries
- [ ] OCR on copied images for searchability
- [ ] Clipboard content classification (URL, code, address, etc.)
- [ ] Adaptive suggestions based on usage patterns
- [ ] iCloud-free multi-Mac history merge
- [ ] Encrypted at-rest storage by default

---

# Design Principles

- Local-first
- No cloud processing
- No network calls
- Deterministic capture behavior
- Missed captures are preferable to corrupted or leaked data
- Keyboard-first, mouse-optional
- Reliability over feature count
- Native macOS APIs whenever possible

---

# Definition of Done

A release candidate should satisfy all of the following:

- Clipboard capture is reliable across common macOS apps and content types.
- Search returns relevant results instantly at typical history sizes.
- Paste-back preserves original formatting and fidelity.
- Private/concealed clipboard content is never recorded.
- History is portable and reproducible via export/import.
- All storage, capture, and search tests pass.
- The application can remain running continuously with minimal CPU and memory usage.
