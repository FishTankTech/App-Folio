# Folio — Project File Tree

```
Folio/
├── Package.swift
├── Package.resolved
│
├── Sources/
│   ├── FolioCore/                      # Shared models — no UI, no side effects
│   │   ├── ClipboardItem.swift         # Core entry model (text/image/file)
│   │   ├── ContentType.swift           # text / rtf / image / file enum
│   │   └── AppMetadata.swift           # Source app bundle ID, name, icon ref
│   │
│   └── Folio/
│       ├── FolioApp.swift              # @main entry, menu bar app scaffold
│       │
│       ├── App/                                        # Milestone 1
│       │   ├── AppDelegate.swift
│       │   ├── LaunchAtLoginManager.swift
│       │   └── Logger.swift
│       │
│       ├── Capture/                                     # Milestone 2
│       │   ├── PasteboardMonitor.swift     # Polling loop, off main thread
│       │   ├── ChangeCountWatcher.swift
│       │   ├── ContentExtractor.swift      # Text / image / file / RTF split
│       │   ├── SourceAppDetector.swift
│       │   ├── ConcealedTypeFilter.swift   # org.nspasteboard.ConcealedType
│       │   └── DuplicateSuppressor.swift
│       │
│       ├── Storage/                                     # Milestone 3
│       │   ├── Database.swift              # SQLite/Core Data wrapper
│       │   ├── Schema/
│       │   │   ├── TextEntry.swift
│       │   │   ├── ImageEntry.swift        # thumbnail + full-res refs
│       │   │   └── Migrations/
│       │   │       └── Migration_0001_InitialSchema.swift
│       │   ├── WriteBatcher.swift
│       │   └── StorageLocationManager.swift
│       │
│       ├── Lifecycle/                                   # Milestone 4
│       │   ├── RetentionManager.swift      # length + time window config
│       │   ├── PruningService.swift        # auto-prune, size cap
│       │   ├── PinnedEntryStore.swift      # exempt from pruning
│       │   └── ExcludeListManager.swift    # apps/domains never recorded
│       │
│       ├── Search/                                      # Milestone 5
│       │   ├── SearchIndex.swift           # incremental, non-blocking
│       │   ├── FuzzyMatcher.swift
│       │   ├── QueryParser.swift           # app / type / date-range filters
│       │   └── RankingEngine.swift         # recency + relevance
│       │
│       ├── Paste/                                       # Milestone 7
│       │   ├── PasteEngine.swift
│       │   ├── AccessibilityPermissionManager.swift
│       │   ├── CmdVInjector.swift          # simulated paste
│       │   ├── FocusRestorer.swift         # return focus to prior app
│       │   └── PasteFallbackHandler.swift  # clipboard fallback + notify
│       │
│       ├── Interface/
│       │   ├── HistoryWindow/                           # Milestone 6
│       │   │   ├── HistoryWindowController.swift
│       │   │   ├── HistoryListView.swift
│       │   │   ├── EntryRowView.swift
│       │   │   ├── KeyboardNavigationHandler.swift  # arrows, enter, esc, 1–9
│       │   │   └── TypeToSearchField.swift
│       │   │
│       │   ├── HotkeyManager.swift                      # Milestone 6
│       │   │
│       │   ├── StatusMenu/                              # Milestone 1, 9
│       │   │   ├── StatusMenuController.swift
│       │   │   └── CaptureIndicator.swift  # menu bar flash on capture
│       │   │
│       │   ├── Previews/                                # Milestone 9
│       │   │   ├── ThumbnailGenerator.swift
│       │   │   ├── ContentPreviewView.swift
│       │   │   └── EntryTypeIconProvider.swift
│       │   │
│       │   └── Preferences/                             # Milestone 8
│       │       ├── PreferencesWindow.swift
│       │       ├── GeneralSettingsView.swift
│       │       ├── AppearanceSettingsView.swift
│       │       ├── HotkeySettingsView.swift
│       │       ├── RetentionSettingsView.swift
│       │       ├── ExcludedAppsView.swift
│       │       ├── PrivacySettingsView.swift
│       │       └── DiagnosticsView.swift   # + DebugOverlay hook
│       │
│       ├── Portability/                                 # Milestone 11
│       │   ├── HistoryExporter.swift
│       │   ├── HistoryImporter.swift
│       │   └── EncryptedArchive.swift
│       │
│       ├── Advanced/                                     # Milestone 13
│       │   ├── SnippetTemplateStore.swift
│       │   ├── TextTransformer.swift       # case, trim, strip formatting
│       │   ├── TaggingStore.swift
│       │   ├── MergePasteController.swift
│       │   ├── ShortcutsProvider.swift     # Shortcuts app + AppleScript
│       │   └── PluginActionAPI.swift       # custom paste transforms
│       │
│       └── Diagnostics/
│           ├── DebugOverlay.swift          # last N captures, timing
│           └── PerformanceMonitor.swift
│
├── Tests/
│   ├── FolioTests/                                      # Milestone 10, 14
│   │   ├── Capture/
│   │   │   ├── CapturePipelineTests.swift
│   │   │   ├── DuplicateSuppressionTests.swift
│   │   │   └── ConcealedTypeTests.swift
│   │   ├── Storage/
│   │   │   ├── StorageLayerTests.swift
│   │   │   ├── MigrationTests.swift
│   │   │   └── CorruptedDatabaseRecoveryTests.swift
│   │   ├── Search/
│   │   │   ├── SearchRelevanceTests.swift
│   │   │   └── FuzzyMatchTests.swift
│   │   ├── Paste/
│   │   │   ├── PasteBehaviorTests.swift
│   │   │   └── PasteBackFidelityTests.swift
│   │   ├── Retention/
│   │   │   └── PruningTests.swift
│   │   ├── Latency/
│   │   │   └── LatencyBenchmarks.swift     # capture→index, hotkey→visible
│   │   ├── EdgeCases/                                   # Milestone 12
│   │   │   ├── LargePayloadTests.swift
│   │   │   ├── RapidSuccessiveCopyTests.swift
│   │   │   ├── ClipboardClearedTests.swift
│   │   │   ├── SleepWakeTests.swift
│   │   │   └── DiskFullTests.swift
│   │   └── Fixtures/
│   │       └── RecordedClipboardEvents/    # replayable event sequences
│   │
│   └── FolioUITests/
│       └── HistoryWindowUITests.swift
│
├── Resources/
│   ├── Assets.xcassets
│   └── Info.plist                          # NSPasteboard/accessibility usage strings
│
└── Reports/                                # Milestone 10 — "Save reports"
    └── (generated test/latency/storage-growth reports land here)
```
