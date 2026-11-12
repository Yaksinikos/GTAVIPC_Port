# GTAVIPC_Port
Grande Tricksters’ Automated Victorious Individualization Project Computer Port

# Full Individual Research Project Plan — Legacy Game Modernization & Full Rust Reimplementation

Project specification · Technical research plan · Architecture · Reverse engineering · Implementation · Testing · Deployment

The project should be treated as a complete modernization and controlled reimplementation of the legacy game, rather than a conventional source-code port.

The central objective is to understand the existing game's functionality, document its systems, reproduce the required gameplay behavior, eliminate unnecessary legacy dependencies, and establish a modern Rust-based architecture that performs exceptionally well on Linux and through the specified Wine environment.

The plan below uses several fundamental principles:

1. Read-only legacy source: The original source code is never modified. All implementation takes place in a separate, newly created repository.
2. Offline-first research: No source code, game assets, behavioral traces, system information, or development telemetry may leave the local environment.
3. Mock-first development: Dependencies are mocked before real implementations are introduced.
4. Rust-first architecture: New systems are implemented in Rust. C++ is retained only when a documented technical justification exists.
5. Performance-driven engineering: Correctness comes first, followed by profiling-driven CPU, GPU, memory, and latency optimization.
6. Complete documentation: Every analyzed legacy subsystem receives its own documentation, and every replacement links back to its corresponding analysis.
7. Strict testing: Every feature requires unit tests and system tests, with offline behavioral testing in isolated Podman containers.
8. Modular architecture: Network functionality is not required for the initial game experience, but future network integration must remain architecturally possible.

## 1. Important architectural decisions

Before implementation, establish these boundaries:

| Area | Decision |
| --- | --- |
| Primary language | Rust |
| Legacy language | C++ for analysis and exceptional compatibility cases |
| Assembly | Selective, measured optimizations |
| Linux compatibility | musl and glibc |
| Primary GPU target | AMD / Mesa / Vulkan |
| NVIDIA | Compatibility where practical |
| Windows execution | Wine + Vulkan |
| Multiplayer | Removed from initial implementation |
| Networking | Offline mocks and replaceable interfaces |
| Frame generation | Removed |
| Telemetry and device tracking | Removed |
| Licensing | Simplified only within authorized ownership and redistribution boundaries |
| Localization | Centralized multilingual asset architecture |
| Source repository | Read-only legacy reference |
| New repository | Independent Rust implementation |
| Testing | Offline, mock-first, Podman-isolated |
| AI | Local models exclusively |

### 1.1 Recommended implementation strategy

I recommend a Rust-first rewrite, not an automatic translation of every C++ source file.

The preferred approach is:

- Reimplement the game engine and core systems in Rust.
- Preserve only the behavior that is necessary for the intended game experience.
- Reconstruct interfaces instead of preserving obsolete dependencies.
- Replace platform-specific and hardware-specific functionality with portable abstractions.
- Remove unnecessary anti-copy, telemetry, device-identification, and licensing subsystems only where ownership and applicable legal permissions authorize doing so.
- Introduce assembly only after profiling demonstrates a meaningful benefit.

For the initial implementation, use a modular monolith rather than dozens of independent services. This avoids unnecessary IPC, synchronization, memory duplication, and runtime overhead.

## 2. Project phases and execution order

1. Phase 00 — Project authorization, scope and preservation
   Establish source ownership, source-read-only guarantees, asset permissions, repository boundaries, and technical acceptance criteria.
2. Phase 01 — Development environment and offline infrastructure
   Build the Podman environment, local AI integration, reproducible toolchains, source inventory, and test infrastructure.
3. Phase 02 — Legacy source code analysis
   Analyze the complete C++ project, dependencies, build system, runtime behavior, data structures, and execution paths.
4. Phase 03 — Behavioral reconstruction and system documentation
   Establish behavior specifications, test scenarios, dependency maps, mock interfaces, and subsystem documentation.
5. Phase 04 — Target architecture and Rust foundation
   Define crate boundaries, engine interfaces, memory management, error handling, threading, GPU interfaces, and platform abstractions.
6. Phase 05 — Core game reimplementation
   Rebuild the game loop, world simulation, rendering, input, audio, resources, persistence, and gameplay systems.
7. Phase 06 — Offline conversion
   Remove multiplayer and external service dependencies, replace them with deterministic local implementations and mocks.
8. Phase 07 — Localization and accessibility
   Build the complete multilingual UI, dialogue, subtitle, font, text-layout, and voice-over infrastructure.
9. Phase 08 — Performance engineering
   Optimize CPU, GPU, memory, I/O, compilation, startup, frame pacing, and latency.
10. Phase 09 — Linux and Wine compatibility
    Validate Void Linux, musl/glibc, Vulkan, AMD graphics, Wine, NTSync, Wayland, and the specified gaming environment.
11. Phase 10 — Final validation and release
    Complete regression testing, offline security audits, performance qualification, documentation, packaging, and reproducible release builds.

## 3. Critical technical considerations

### 3.1 The 500 GB source-code requirement

A 500 GB repository must not be handled as one enormous source-analysis task.

The analysis system should use:

- Incremental file indexing.
- Content hashes and immutable manifests.
- Symbol databases.
- Dependency graphs.
- Translation-unit-level analysis.
- Bounded concurrent processing.
- Memory-mapped reads where appropriate.
- Disk-backed indexes.
- Incremental C++ compilation databases.
- Cached AST and intermediate analysis results.
- Prioritized analysis based on runtime relevance.

A strict memory budget must be enforced independently of available system RAM.

Importantly, a 500 GB source tree does not imply that every source file must be loaded into memory or that all files should be compiled simultaneously.

### 3.2 Rust coding standards

The Google Rust Style Guide must be a mandatory engineering standard, not merely a recommendation.

Reference: [Google Rust Style Guide](<https://google.github.io/styleguide/rustguide.html?utm_source=chatgpt.com>).

The project should additionally enforce:

- `rustfmt --check`
- `clippy` with strict warnings.
- `#![deny(unsafe_op_in_unsafe_fn)]`
- Explicit documentation of unsafe code.
- No undocumented unsafe blocks.
- No unexplained unwraps.
- No unnecessary allocations or cloning.
- No glob imports.
- Small, purpose-specific crates.
- Explicit ownership and borrowing.
- Consistent error handling.
- Review of every dependency addition.

The Google guide should take precedence where its recommendations differ from generic Rust conventions, while compatible official Rust conventions remain applicable.

### 3.3 REA analysis integration

[morluto/rea — Reverse Engineer Anything](<https://github.com/morluto/rea?utm_source=chatgpt.com>) can be integrated as an optional local analysis tool.

Use it for evidence-backed examination of:

- Native binaries.
- Symbols and references.
- Assembly and pseudocode.
- Application behavior.
- Runtime execution traces.
- Dependency relationships.

REA findings must be treated as hypotheses supported by evidence, not automatically accepted as ground truth.

Its agent/model integration must be configured for a local model and verified to have no external data transmission. REA's local analysis capabilities alone do not guarantee that every connected AI provider is local.

GitHub

+1

## 4. Complete project work breakdown structure

The following is the proposed implementation specification. It uses hierarchical identifiers so that individual functions, tests, decisions, and documentation can be traced throughout the project.

The hierarchy supports expansion to 99 levels, but this does not mean artificially creating 99 layers of abstraction. Each level should represent a real technical decomposition.

For example:

`P05.03.02.04.01.03`

- `P05` — Core game implementation.
- `03` — Rendering subsystem.
- `02` — GPU abstraction.
- `04` — Resource lifecycle.
- `01` — Resource destruction.
- `03` — Device-loss recovery test.

Every leaf-level task must have an owner, prerequisites, implementation specification, test specification, documentation reference, and completion criteria.

### Phase 00 — Authorization, preservation and project definition

P00.01 — Source preservation

- P00.01.01 Establish the original source directory.
- P00.01.02 Mount or expose the legacy source as read-only.
- P00.01.03 Verify filesystem permissions.
- P00.01.04 Calculate source-file hashes.
- P00.01.05 Generate an immutable source manifest.
- P00.01.06 Detect symbolic links and external references.
- P00.01.07 Identify generated and third-party files.
- P00.01.08 Establish an independent destination repository.
- P00.01.09 Verify that build processes cannot write into the source tree.
- P00.01.10 Establish backups and recovery procedures.

P00.02 — Project boundaries

- P00.02.01 Define supported gameplay.
- P00.02.02 Define excluded legacy functionality.
- P00.02.03 Define local-only execution.
- P00.02.04 Define permitted assets.
- P00.02.05 Define redistribution permissions.
- P00.02.06 Define platform compatibility.
- P00.02.07 Define performance targets.
- P00.02.08 Define localization requirements.
- P00.02.09 Define security requirements.
- P00.02.10 Define release acceptance criteria.

P00.03 — Technical decision framework

- P00.03.01 Rust as the primary implementation language.
- P00.03.02 C++ only for documented exceptions.
- P00.03.03 ASM only after profiling.
- P00.03.04 Vulkan as the primary graphics API.
- P00.03.05 AMD-first graphics validation.
- P00.03.06 No mandatory network services.
- P00.03.07 Mock-first dependency architecture.
- P00.03.08 Modular monolith architecture.
- P00.03.09 Deterministic test environment.
- P00.03.10 Decision approval and documentation process.

### Phase 01 — Development environment and infrastructure

P01.01 — Repository setup

Recommended initial structure:

text

```text

game-modernization/
├── .git/
├── .gitignore
├── .gitattributes
├── rust-toolchain.toml
├── Cargo.toml
├── Cargo.lock
├── rustfmt.toml
├── clippy.toml
├── deny.toml
├── README.md
├── CHANGELOG.md
├── LICENSE
├── crates/
│   ├── game_core/
│   ├── game_runtime/
│   ├── game_platform/
│   ├── game_renderer/
│   ├── game_audio/
│   ├── game_input/
│   ├── game_world/
│   ├── game_assets/
│   ├── game_localization/
│   ├── game_persistence/
│   ├── game_ui/
│   ├── game_network_mock/
│   ├── game_diagnostics/
│   └── game_launcher/
├── assets/
│   ├── common/
│   ├── languages/
│   ├── fonts/
│   ├── audio/
│   ├── textures/
│   ├── shaders/
│   └── configuration/
├── docs/
│   ├── architecture/
│   ├── decisions/
│   ├── legacy_analysis/
│   ├── systems/
│   ├── interfaces/
│   ├── testing/
│   ├── security/
│   ├── performance/
│   ├── localization/
│   ├── compatibility/
│   ├── deployment/
│   ├── research/
│   └── project_management/
├── tests/
│   ├── unit/
│   ├── integration/
│   ├── system/
│   ├── behavioral/
│   ├── regression/
│   ├── performance/
│   ├── stress/
│   ├── security/
│   ├── compatibility/
│   ├── localization/
│   ├── mocks/
│   ├── fixtures/
│   └── test_data/
├── tools/
│   ├── analysis/
│   ├── profiling/
│   ├── asset_pipeline/
│   ├── validation/
│   └── documentation/
├── containers/
│   ├── analysis/
│   ├── testing/
│   ├── building/
│   └── performance/
├── scripts/
│   ├── build/
│   ├── test/
│   ├── analysis/
│   └── release/
└── build/
```

P01.02 — Git configuration

- P01.02.01 Configure LF line endings.
- P01.02.02 Configure `.gitattributes`.
- P01.02.03 Configure `.gitignore`.
- P01.02.04 Exclude build artifacts.
- P01.02.05 Exclude local AI model files.
- P01.02.06 Exclude source-analysis databases where appropriate.
- P01.02.07 Exclude runtime caches and temporary data.
- P01.02.08 Verify binary asset handling.
- P01.02.09 Establish commit conventions.
- P01.02.10 Establish changelog conventions.

Recommended `.gitattributes`:

gitattributes

```gitattributes
* text=auto eol=lf

*.rs text eol=lf
*.toml text eol=lf
*.md text eol=lf
*.json text eol=lf
*.yaml text eol=lf
*.yml text eol=lf
*.sh text eol=lf

*.png binary
*.jpg binary
*.jpeg binary
*.ogg binary
*.wav binary
*.flac binary
*.mp3 binary
*.pak binary
*.bin binary
```

P01.03 — Rust toolchain

- P01.03.01 Pin Rust toolchain.
- P01.03.02 Configure Cargo.
- P01.03.03 Configure rustfmt.
- P01.03.04 Configure Clippy.
- P01.03.05 Configure dependency auditing.
- P01.03.06 Establish offline dependency cache.
- P01.03.07 Establish lockfile validation.
- P01.03.08 Establish build reproducibility.
- P01.03.09 Establish release profiles.
- P01.03.10 Establish separate development and production configurations.

P01.04 — Podman infrastructure

- P01.04.01 Create rootless container environment.
- P01.04.02 Disable networking by default.
- P01.04.03 Use read-only root filesystems.
- P01.04.04 Create temporary writable mounts.
- P01.04.05 Restrict Linux capabilities.
- P01.04.06 Disable privileged containers.
- P01.04.07 Restrict process counts.
- P01.04.08 Restrict memory consumption.
- P01.04.09 Restrict CPU consumption.
- P01.04.10 Restrict filesystem access.
- P01.04.11 Separate analysis and test containers.
- P01.04.12 Create immutable container images.
- P01.04.13 Verify network isolation.
- P01.04.14 Verify no host-source modification.
- P01.04.15 Capture test artifacts locally.

Create dedicated container profiles:

| Profile | Purpose | Network |
| --- | --- | --- |
| Analysis | Static source analysis | Disabled |
| Behavioral | Deterministic behavior tests | Disabled |
| Unit | Isolated unit tests | Disabled |
| System | Integrated local system tests | Disabled |
| Build | Reproducible compilation | Disabled |
| Performance | Controlled benchmarks | Disabled |
| GPU | Dedicated Vulkan testing | Disabled |

GPU testing may require explicitly passing a device node into the container. It must never result in granting the container unrestricted host access.

P01.05 — Local AI infrastructure

- P01.05.01 Select local model runtime.
- P01.05.02 Download and verify model artifacts before offline operation.
- P01.05.03 Configure local inference endpoints.
- P01.05.04 Disable cloud fallback.
- P01.05.05 Disable external telemetry.
- P01.05.06 Configure offline agent execution.
- P01.05.07 Create restricted agent workspaces.
- P01.05.08 Create task-specific agent permissions.
- P01.05.09 Validate output against source evidence.
- P01.05.10 Require human approval for architectural changes.

### Phase 02 — Legacy source analysis

This is one of the most important phases. Implementation must not begin before sufficient behavioral understanding exists.

P02.01 — Repository inventory

- P02.01.01 Enumerate all files.
- P02.01.02 Classify C++ source files.
- P02.01.03 Classify header files.
- P02.01.04 Classify assembly files.
- P02.01.05 Identify scripting languages.
- P02.01.06 Identify configuration files.
- P02.01.07 Identify binary assets.
- P02.01.08 Identify generated code.
- P02.01.09 Identify third-party dependencies.
- P02.01.10 Identify duplicate source files.
- P02.01.11 Identify obsolete code.
- P02.01.12 Generate source inventory reports.

P02.02 — Large-scale source indexing

- P02.02.01 Implement streaming file discovery.
- P02.02.02 Implement incremental hashing.
- P02.02.03 Implement source classification.
- P02.02.04 Implement C++ translation-unit discovery.
- P02.02.05 Build symbol indexes.
- P02.02.06 Build include dependency graphs.
- P02.02.07 Build function-call graphs.
- P02.02.08 Build class inheritance graphs.
- P02.02.09 Build module dependency graphs.
- P02.02.10 Build data-flow indexes.
- P02.02.11 Build configuration dependency graphs.
- P02.02.12 Store analysis results on disk.
- P02.02.13 Implement interrupted-analysis recovery.
- P02.02.14 Implement incremental reanalysis.
- P02.02.15 Benchmark indexing at 500 GB scale.

P02.03 — C++ static analysis

- P02.03.01 Detect compiler requirements.
- P02.03.02 Detect language standards.
- P02.03.03 Identify compiler-specific extensions.
- P02.03.04 Analyze preprocessor directives.
- P02.03.05 Analyze macro dependencies.
- P02.03.06 Analyze template-heavy code.
- P02.03.07 Analyze ownership patterns.
- P02.03.08 Analyze memory allocation.
- P02.03.09 Analyze pointer arithmetic.
- P02.03.10 Analyze thread synchronization.
- P02.03.11 Analyze exception handling.
- P02.03.12 Analyze undefined-behavior risks.
- P02.03.13 Analyze platform-specific APIs.
- P02.03.14 Analyze CPU instruction requirements.
- P02.03.15 Analyze compiler optimization dependencies.

P02.04 — Runtime behavior analysis

Only execute the legacy application in a controlled environment with authorized assets and no external network access.

- P02.04.01 Identify startup sequence.
- P02.04.02 Identify initialization order.
- P02.04.03 Identify runtime states.
- P02.04.04 Identify state transitions.
- P02.04.05 Identify event dispatch.
- P02.04.06 Identify update-loop behavior.
- P02.04.07 Identify rendering behavior.
- P02.04.08 Identify input behavior.
- P02.04.09 Identify audio behavior.
- P02.04.10 Identify save/load behavior.
- P02.04.11 Identify shutdown behavior.
- P02.04.12 Record deterministic traces.
- P02.04.13 Record error handling.
- P02.04.14 Record failure recovery.
- P02.04.15 Identify hardware-dependent behavior.

P02.05 — Dependency analysis

For every dependency, determine:

- Purpose.
- API surface.
- Initialization requirements.
- Runtime lifecycle.
- Memory ownership.
- Threading requirements.
- External side effects.
- Network requirements.
- Platform dependencies.
- Replacement strategy.
- Required behavioral compatibility.
- Test and mock strategy.

P02.06 — Security and privacy analysis

- P02.06.01 Inventory telemetry.
- P02.06.02 Inventory device-identification code.
- P02.06.03 Inventory tracking functionality.
- P02.06.04 Inventory external endpoints.
- P02.06.05 Inventory crash-reporting services.
- P02.06.06 Inventory licensing dependencies.
- P02.06.07 Inventory update mechanisms.
- P02.06.08 Inventory anti-tamper dependencies.
- P02.06.09 Identify data collection and storage.
- P02.06.10 Establish authorized removal specifications.
- P02.06.11 Verify absence of tracking in the replacement.
- P02.06.12 Verify absence of external communication.

### Phase 03 — Behavioral specifications and documentation

Every individual subsystem must have a separate documentation file.

P03.01 — Documentation template

Each legacy system document should contain:

1. System identification.
2. Purpose.
3. Legacy source locations.
4. Relevant classes and functions.
5. Entry points.
6. Exit points.
7. Initialization sequence.
8. Runtime lifecycle.
9. Input parameters.
10. Output values.
11. Internal state.
12. State transitions.
13. Dependencies.
14. Memory behavior.
15. CPU behavior.
16. GPU interactions.
17. Threading behavior.
18. Error conditions.
19. Failure behavior.
20. External side effects.
21. Network dependencies.
22. Platform dependencies.
23. Observed behavior.
24. Static analysis findings.
25. Runtime analysis findings.
26. Uncertainties.
27. Behavioral test cases.
28. Mock requirements.
29. Replacement architecture.
30. Replacement interfaces.
31. Rust crate mapping.
32. Performance requirements.
33. Security requirements.
34. Acceptance criteria.
35. Documentation links.

P03.02 — Behavioral test construction

- P03.02.01 Identify normal execution paths.
- P03.02.02 Identify edge cases.
- P03.02.03 Identify invalid inputs.
- P03.02.04 Identify boundary conditions.
- P03.02.05 Identify timing-dependent behavior.
- P03.02.06 Identify concurrency behavior.
- P03.02.07 Identify resource exhaustion behavior.
- P03.02.08 Identify error recovery.
- P03.02.09 Identify state persistence.
- P03.02.10 Identify deterministic output expectations.

P03.03 — Mock architecture

- P03.03.01 Mock filesystem.
- P03.03.02 Mock clock.
- P03.03.03 Mock random number generator.
- P03.03.04 Mock input devices.
- P03.03.05 Mock audio device.
- P03.03.06 Mock graphics device.
- P03.03.07 Mock network transport.
- P03.03.08 Mock multiplayer sessions.
- P03.03.09 Mock player synchronization.
- P03.03.10 Mock external services.
- P03.03.11 Mock platform APIs.
- P03.03.12 Mock persistence.
- P03.03.13 Mock resource loading.
- P03.03.14 Mock failures and timeouts.
- P03.03.15 Mock hardware limitations.

Every mock must define its expected behavior, deterministic output, failure modes, and verification strategy.

P03.04 — REA evidence management

- P03.04.01 Register analysis targets.
- P03.04.02 Record tool versions.
- P03.04.03 Record analysis configuration.
- P03.04.04 Store relevant evidence locally.
- P03.04.05 Link evidence to subsystem documents.
- P03.04.06 Distinguish observation from inference.
- P03.04.07 Record confidence levels.
- P03.04.08 Revalidate uncertain findings.
- P03.04.09 Prevent unsupported AI-generated conclusions.
- P03.04.10 Review final documentation.

### Phase 04 — Rust architecture

P04.01 — Core foundation

- P04.01.01 Define application lifecycle.
- P04.01.02 Define initialization interfaces.
- P04.01.03 Define shutdown interfaces.
- P04.01.04 Define global configuration.
- P04.01.05 Define error types.
- P04.01.06 Define diagnostics.
- P04.01.07 Define logging.
- P04.01.08 Define resource management.
- P04.01.09 Define threading model.
- P04.01.10 Define scheduling model.

P04.02 — Crate boundaries

Each crate must have:

- A clearly defined responsibility.
- A minimal public API.
- Explicit dependencies.
- No unnecessary circular dependencies.
- Unit tests.
- Integration tests.
- Documentation.
- Performance benchmarks where relevant.

P04.03 — Platform abstraction

- P04.03.01 Operating-system interfaces.
- P04.03.02 Filesystem interfaces.
- P04.03.03 Memory interfaces.
- P04.03.04 Thread interfaces.
- P04.03.05 Clock interfaces.
- P04.03.06 Input interfaces.
- P04.03.07 Audio interfaces.
- P04.03.08 GPU interfaces.
- P04.03.09 Window interfaces.
- P04.03.10 Process interfaces.

P04.04 — Error architecture

- P04.04.01 Recoverable errors.
- P04.04.02 Fatal errors.
- P04.04.03 Initialization failures.
- P04.04.04 Resource failures.
- P04.04.05 Device-loss errors.
- P04.04.06 I/O failures.
- P04.04.07 Asset validation errors.
- P04.04.08 Serialization errors.
- P04.04.09 Localization errors.
- P04.04.10 User-facing error messages.

### Phase 05 — Full game reimplementation

The following is the target subsystem inventory. Actual subsystems must be confirmed against the legacy analysis.

P05.01 — Application and game lifecycle

- P05.01.01 Launcher.
- P05.01.02 Configuration loading.
- P05.01.03 Initialization.
- P05.01.04 Main game loop.
- P05.01.05 State machine.
- P05.01.06 Pause handling.
- P05.01.07 Resume handling.
- P05.01.08 Shutdown.
- P05.01.09 Crash recovery.
- P05.01.10 Restart handling.

P05.02 — World simulation

- P05.02.01 World representation.
- P05.02.02 Entity management.
- P05.02.03 Component management.
- P05.02.04 Spatial queries.
- P05.02.05 Collision detection.
- P05.02.06 Physics.
- P05.02.07 Animation state.
- P05.02.08 Game rules.
- P05.02.09 Time progression.
- P05.02.10 Deterministic simulation.

P05.03 — Rendering

- P05.03.01 Vulkan initialization.
- P05.03.02 Physical-device selection.
- P05.03.03 Logical-device creation.
- P05.03.04 Queue management.
- P05.03.05 Swapchain management.
- P05.03.06 Command buffers.
- P05.03.07 Descriptor management.
- P05.03.08 Buffer management.
- P05.03.09 Texture management.
- P05.03.10 Shader management.
- P05.03.11 Pipeline management.
- P05.03.12 Render graph.
- P05.03.13 Synchronization.
- P05.03.14 Frame pacing.
- P05.03.15 Resolution scaling.
- P05.03.16 Presentation.
- P05.03.17 Device-loss recovery.
- P05.03.18 GPU resource destruction.
- P05.03.19 AMD-specific validation.
- P05.03.20 NVIDIA compatibility validation.

P05.04 — Input

- P05.04.01 Keyboard.
- P05.04.02 Mouse.
- P05.04.03 Gamepad.
- P05.04.04 Input mapping.
- P05.04.05 Input configuration.
- P05.04.06 Key rebinding.
- P05.04.07 Controller profiles.
- P05.04.08 Focus handling.
- P05.04.09 Input latency.
- P05.04.10 Input recording for tests.

P05.05 — Audio

- P05.05.01 Audio device discovery.
- P05.05.02 Audio backend.
- P05.05.03 Audio mixer.
- P05.05.04 Voice playback.
- P05.05.05 Music playback.
- P05.05.06 Positional audio.
- P05.05.07 Audio channels.
- P05.05.08 Volume management.
- P05.05.09 Audio streaming.
- P05.05.10 Audio localization.
- P05.05.11 Audio synchronization.
- P05.05.12 Device disconnection recovery.

P05.06 — Assets

- P05.06.01 Asset registry.
- P05.06.02 Asset metadata.
- P05.06.03 Asset validation.
- P05.06.04 Asset loading.
- P05.06.05 Asset streaming.
- P05.06.06 Asset caching.
- P05.06.07 Resource lifetime.
- P05.06.08 Asset versioning.
- P05.06.09 Compression.
- P05.06.10 Corruption handling.

P05.07 — UI

- P05.07.01 UI rendering.
- P05.07.02 UI layout.
- P05.07.03 UI components.
- P05.07.04 Menus.
- P05.07.05 Settings.
- P05.07.06 HUD.
- P05.07.07 Dialogues.
- P05.07.08 Text input.
- P05.07.09 Accessibility.
- P05.07.10 Dynamic localization.

P05.08 — Persistence

- P05.08.01 Save files.
- P05.08.02 Load files.
- P05.08.03 Save schema.
- P05.08.04 Versioned serialization.
- P05.08.05 Atomic writes.
- P05.08.06 Corruption detection.
- P05.08.07 Backup handling.
- P05.08.08 Configuration persistence.
- P05.08.09 Local player state.
- P05.08.10 Recovery testing.

### Phase 06 — Multiplayer and network dependency elimination

The objective is to make the game fully playable without networking.

P06.01 — Network dependency inventory

- P06.01.01 Identify network initialization.
- P06.01.02 Identify connection requirements.
- P06.01.03 Identify session management.
- P06.01.04 Identify player synchronization.
- P06.01.05 Identify matchmaking.
- P06.01.06 Identify server validation.
- P06.01.07 Identify network-dependent game logic.
- P06.01.08 Identify online persistence.
- P06.01.09 Identify external APIs.
- P06.01.10 Identify offline startup blockers.

P06.02 — Local gameplay conversion

- P06.02.01 Replace remote player state with local state.
- P06.02.02 Replace server-controlled progression with local progression.
- P06.02.03 Replace remote world state with local simulation.
- P06.02.04 Replace matchmaking with local initialization.
- P06.02.05 Replace online configuration with local configuration.
- P06.02.06 Replace external persistence with local storage.
- P06.02.07 Remove mandatory connection checks.
- P06.02.08 Validate startup without network interfaces.
- P06.02.09 Validate gameplay without external endpoints.
- P06.02.10 Validate shutdown without network activity.

P06.03 — Future extension interfaces

Create mock interfaces for:

- `NetworkTransport`
- `SessionProvider`
- `RemotePlayerProvider`
- `WorldReplication`
- `MessageSerializer`
- `ConnectionState`
- `NetworkClock`
- `RemotePersistence`

These must be inert by default. They should not contain hidden network initialization, DNS lookup, socket creation, or background connection attempts.

The local game must not depend on them.

### Phase 07 — Full localization architecture

Mandatory systemLocalization must be implemented as a core architectural system rather than a late-stage translation feature.

P07.01 — Language infrastructure

- P07.01.01 Language registry.
- P07.01.02 Locale identification.
- P07.01.03 Language selection.
- P07.01.04 Default-language fallback.
- P07.01.05 Per-language asset loading.
- P07.01.06 Language change without restart where feasible.
- P07.01.07 Missing translation detection.
- P07.01.08 Translation validation.
- P07.01.09 Unicode normalization.
- P07.01.10 Language-specific formatting.

P07.02 — Arabic support

- P07.02.01 Right-to-left layout.
- P07.02.02 Bidirectional text.
- P07.02.03 Arabic shaping.
- P07.02.04 Contextual glyph selection.
- P07.02.05 Arabic punctuation.
- P07.02.06 Mixed Arabic and Latin text.
- P07.02.07 Numeral formatting.
- P07.02.08 UI mirroring.
- P07.02.09 Font fallback.
- P07.02.10 Arabic subtitle validation.

P07.03 — Cyrillic support

- P07.03.01 Extended Cyrillic character coverage.
- P07.03.02 Font fallback.
- P07.03.03 Language-specific glyph coverage.
- P07.03.04 Case conversion.
- P07.03.05 Unicode normalization.
- P07.03.06 Text measurement.
- P07.03.07 Subtitle layout.
- P07.03.08 UI overflow tests.

P07.04 — Mandarin Chinese

- P07.04.01 Simplified Chinese.
- P07.04.02 Traditional Chinese.
- P07.04.03 CJK font coverage.
- P07.04.04 Glyph fallback.
- P07.04.05 Line breaking.
- P07.04.06 Punctuation.
- P07.04.07 Character spacing.
- P07.04.08 Subtitle timing.
- P07.04.09 Text rendering.
- P07.04.10 Font memory optimization.

P07.05 — Voice-over

- P07.05.01 Language-specific audio directories.
- P07.05.02 Voice asset registry.
- P07.05.03 Voice metadata.
- P07.05.04 Dialogue-to-audio mapping.
- P07.05.05 Missing voice fallback.
- P07.05.06 Audio format validation.
- P07.05.07 Audio streaming.
- P07.05.08 Voice selection.
- P07.05.09 Subtitle synchronization.
- P07.05.10 Voice-over regression tests.

P07.06 — Central language asset structure

text

```text

assets/
└── languages/
    ├── en/
    │   ├── manifest.toml
    │   ├── ui/
    │   │   ├── common.json
    │   │   ├── menus.json
    │   │   └── settings.json
    │   ├── dialogue/
    │   ├── subtitles/
    │   ├── voice/
    │   └── metadata/
    ├── ar/
    │   ├── manifest.toml
    │   ├── ui/
    │   ├── dialogue/
    │   ├── subtitles/
    │   ├── voice/
    │   └── metadata/
    ├── ru/
    │   ├── manifest.toml
    │   ├── ui/
    │   ├── dialogue/
    │   ├── subtitles/
    │   ├── voice/
    │   └── metadata/
    ├── zh-Hans/
    │   ├── manifest.toml
    │   ├── ui/
    │   ├── dialogue/
    │   ├── subtitles/
    │   ├── voice/
    │   └── metadata/
    └── zh-Hant/
        ├── manifest.toml
        ├── ui/
        ├── dialogue/
        ├── subtitles/
        ├── voice/
        └── metadata/
```

Every language should be independently installable, verifiable, and replaceable.

### Phase 08 — Performance engineering

P08.01 — Baseline measurement

- P08.01.01 Establish startup baseline.
- P08.01.02 Establish average frame time.
- P08.01.03 Establish P95 frame time.
- P08.01.04 Establish P99 frame time.
- P08.01.05 Establish CPU usage.
- P08.01.06 Establish GPU utilization.
- P08.01.07 Establish peak memory.
- P08.01.08 Establish allocation rates.
- P08.01.09 Establish disk I/O.
- P08.01.10 Establish input latency.

P08.02 — Rust optimization

- P08.02.01 Reduce unnecessary allocations.
- P08.02.02 Reduce deep copies.
- P08.02.03 Optimize data layouts.
- P08.02.04 Improve cache locality.
- P08.02.05 Optimize hot loops.
- P08.02.06 Reduce synchronization overhead.
- P08.02.07 Improve thread scheduling.
- P08.02.08 Reduce lock contention.
- P08.02.09 Optimize asset loading.
- P08.02.10 Optimize resource lifetime.

P08.03 — C++ and assembly exceptions

The preferred implementation is Rust.

If a legacy C++ component must temporarily remain:

- Document why it cannot yet be replaced.
- Isolate it behind a stable FFI boundary.
- Compile with Clang.
- Use explicit ABI definitions.
- Validate memory ownership.
- Run sanitizer tests.
- Measure its actual runtime cost.
- Establish a replacement deadline.

For ASM:

- Benchmark the Rust implementation first.
- Identify an actual hot path.
- Implement only a narrowly scoped routine.
- Document calling conventions.
- Document register and memory assumptions.
- Test CPU feature detection.
- Provide a portable fallback.
- Benchmark across supported AMD CPUs.
- Run correctness and differential tests.

Never introduce handwritten assembly simply because it appears faster.

P08.04 — Peak resource testing

- P08.04.01 Maximum source-indexing workload.
- P08.04.02 Maximum asset-loading workload.
- P08.04.03 Maximum entity count.
- P08.04.04 Maximum texture workload.
- P08.04.05 Maximum audio workload.
- P08.04.06 Maximum concurrent tasks.
- P08.04.07 CPU saturation testing.
- P08.04.08 GPU saturation testing.
- P08.04.09 Memory pressure testing.
- P08.04.10 Combined resource pressure testing.

### Phase 09 — Platform compatibility

P09.01 — Void Linux

- P09.01.01 Validate glibc build.
- P09.01.02 Validate musl build where supported.
- P09.01.03 Validate dynamic dependencies.
- P09.01.04 Validate Vulkan loader.
- P09.01.05 Validate AMD Mesa drivers.
- P09.01.06 Validate audio backend.
- P09.01.07 Validate input devices.
- P09.01.08 Validate Wayland.
- P09.01.09 Validate filesystem paths.
- P09.01.10 Validate clean installation.

P09.02 — Wine environment

Target:

`Windows 11 + Wine + Vulkan + NTSync + AMDGPU + Wayland`

Subtasks:

- P09.02.01 Define Wine prefix requirements.
- P09.02.02 Validate executable compatibility.
- P09.02.03 Validate Vulkan translation.
- P09.02.04 Validate synchronization behavior.
- P09.02.05 Validate audio.
- P09.02.06 Validate input.
- P09.02.07 Validate window management.
- P09.02.08 Validate fullscreen.
- P09.02.09 Validate controller support.
- P09.02.10 Validate save directories.
- P09.02.11 Validate shutdown.
- P09.02.12 Validate crash recovery.

P09.03 — Gaming environment integration

Validate each component independently before combined testing:

- AMD `amdgpu`.
- Wayland compositor.
- Wine.
- Vulkan.
- NTSync.
- LatencyFleX.
- MangoHUD.
- Feral GameMode.
- CPU scheduler behavior.
- Frame-rate limiter.

P09.04 — 244 FPS target

A 244 FPS cap corresponds to approximately 4.098 ms per frame.

The acceptance target should be:

- Stable 244 FPS where hardware permits.
- Frame pacing without unnecessary oscillation.
- No forced frame generation.
- No unnecessary CPU/GPU synchronization.
- No uncontrolled busy-wait loops.
- No avoidable input buffering.
- Correct behavior when the GPU cannot sustain the target.

The game must not force maximum CPU utilization continuously. Instead, it should use available CPU resources efficiently, with configurable worker limits and minimal idle overhead.

### Phase 10 — Testing, verification and quality assurance

This phase is continuous throughout development, not something that happens only before release.

P10.01 — Unit testing

Every public function and every important internal behavior must have appropriate unit tests.

Test categories:

- Normal execution.
- Empty inputs.
- Invalid inputs.
- Boundary values.
- Overflow conditions.
- Error handling.
- Resource exhaustion.
- Memory ownership.
- State transitions.
- Concurrent access.
- Deterministic output.
- Recovery behavior.

P10.02 — Integration testing

Test combinations of subsystems:

- Core + world simulation.
- World + physics.
- Renderer + asset manager.
- Renderer + UI.
- Audio + localization.
- Input + gameplay.
- Persistence + game state.
- Localization + UI.
- Platform + renderer.
- Game runtime + all core systems.

P10.03 — Behavioral testing

Compare the original game's documented behavior against the new implementation.

Use:

- Recorded input sequences.
- Deterministic random seeds.
- Controlled clocks.
- Fixed configuration.
- Mock hardware.
- Mock filesystem.
- Reference outputs.
- State snapshots.
- Differential comparisons.

Where exact visual or numerical equivalence is not required, document the acceptable differences.

P10.04 — System testing

Each complete feature must pass:

1. Initialization.
2. Normal execution.
3. Invalid operation.
4. Resource exhaustion.
5. Failure handling.
6. Recovery.
7. Shutdown.
8. Restart.
9. Offline execution.
10. Regression verification.

P10.05 — Security testing

- P10.05.01 Verify network isolation.
- P10.05.02 Verify no external DNS requests.
- P10.05.03 Verify no outbound sockets.
- P10.05.04 Verify no telemetry.
- P10.05.05 Verify no device tracking.
- P10.05.06 Verify no external AI services.
- P10.05.07 Verify no hidden background services.
- P10.05.08 Verify filesystem boundaries.
- P10.05.09 Verify dependency integrity.
- P10.05.10 Verify container isolation.
- P10.05.11 Verify asset parsing against malformed inputs.
- P10.05.12 Verify safe handling of corrupted save files.

P10.06 — Performance testing

- P10.06.01 Cold startup.
- P10.06.02 Warm startup.
- P10.06.03 Scene transitions.
- P10.06.04 Asset streaming.
- P10.06.05 Long-duration gameplay.
- P10.06.06 CPU saturation.
- P10.06.07 GPU saturation.
- P10.06.08 Memory pressure.
- P10.06.09 Frame-time distribution.
- P10.06.10 Input-to-display latency.
- P10.06.11 Thread contention.
- P10.06.12 Memory fragmentation.
- P10.06.13 Resource cleanup.
- P10.06.14 Performance regression.

### Phase 11 — Release engineering

P11.01 — Build qualification

- P11.01.01 Reproducible release build.
- P11.01.02 Rust dependency verification.
- P11.01.03 Offline build verification.
- P11.01.04 musl qualification.
- P11.01.05 glibc qualification.
- P11.01.06 Vulkan validation.
- P11.01.07 AMD GPU validation.
- P11.01.08 Wine validation.
- P11.01.09 Asset integrity verification.
- P11.01.10 Binary dependency audit.

P11.02 — Release packaging

- P11.02.01 Linux package.
- P11.02.02 Portable Linux package.
- P11.02.03 musl-compatible package where feasible.
- P11.02.04 glibc-compatible package.
- P11.02.05 Wine-compatible executable/package.
- P11.02.06 Language asset packages.
- P11.02.07 Configuration defaults.
- P11.02.08 Local documentation.
- P11.02.09 Release manifest.
- P11.02.10 Checksums.

P11.03 — Final release acceptance

A release is permitted only when:

- All required core features are implemented.
- All required features have unit tests.
- All required features have system tests.
- All critical behavioral tests pass.
- Offline operation is verified.
- No unwanted external communication occurs.
- Localization works across all required scripts.
- AMD Vulkan validation passes.
- Void Linux compatibility passes.
- Wine compatibility passes.
- Performance targets are measured and documented.
- No known critical data-corruption issues remain.
- All architectural decisions are documented.

## 5. Required testing architecture

I strongly recommend a unified test contract for every feature.

text

```text

Feature
│
├── Specification
│   ├── Requirements
│   ├── Expected behavior
│   └── Failure conditions
│
├── Mock implementation
│   ├── Deterministic inputs
│   ├── Deterministic outputs
│   ├── Failure injection
│   └── Resource simulation
│
├── Unit tests
│   ├── Normal cases
│   ├── Boundary cases
│   └── Error cases
│
├── Integration tests
│   ├── Interface compatibility
│   ├── State consistency
│   └── Dependency behavior
│
├── System tests
│   ├── Initialization
│   ├── Execution
│   ├── Recovery
│   └── Shutdown
│
├── Performance tests
│   ├── CPU
│   ├── GPU
│   ├── Memory
│   └── Latency
│
└── Documentation
    ├── Legacy analysis reference
    ├── Replacement architecture
    ├── Test evidence
    └── Acceptance result
```

### 5.1 Test isolation rules

Every test container should follow these rules:

| Restriction | Requirement |
| --- | --- |
| Network | `--network=none` |
| Privileged mode | Prohibited |
| Root filesystem | Read-only |
| Capabilities | Drop all unless specifically required |
| Source directory | Read-only mount |
| Writable directories | Explicit temporary mounts |
| Memory | Explicit cgroup limit |
| CPU | Explicit CPU quota |
| Processes | PID limit |
| External endpoints | None |
| AI inference | Local only |
| Test results | Local artifacts |

One important distinction: a GPU system test cannot be completely hardware-independent. Keep ordinary behavioral tests GPU-mocked, and run dedicated Vulkan hardware tests in a separately restricted environment.

### 5.2 Testing levels

| Level | Scope | Required |
| --- | --- | --- |
| L1 | Function | Yes |
| L2 | Module | Yes |
| L3 | Crate | Yes |
| L4 | Subsystem | Yes |
| L5 | Integrated system | Yes |
| L6 | Complete game | Yes |
| L7 | Platform compatibility | Yes |
| L8 | Long-duration stress | Yes |
| L9 | Release qualification | Yes |

## 6. Documentation and decision management

All project documentation must live inside `/docs`.

The two most important files are:

- `/CHANGELOG.md`
- `/docs/decisions/`

Use individual decision records, for example:

`/docs/decisions/ADR-0001-rust-first-architecture.md`

Each decision record should contain:

markdown

```markdown
# ADR-0001: Rust-First Architecture

## Status
Accepted

## Context
The legacy game is predominantly written in C++.

## Decision
New game systems will be implemented in Rust.

## Alternatives
- Complete C++ modernization.
- Hybrid C++ and Rust.
- Complete Rust reimplementation.

## Reasoning
Maintainability, memory safety, performance,
platform compatibility and architectural simplicity.

## Consequences
- New crate architecture.
- Explicit FFI boundaries where necessary.
- Increased initial reimplementation effort.

## Related Documentation
- /docs/architecture/
- /docs/legacy_analysis/
- /docs/systems/
```

### 6.1 Mandatory documentation relationships

Every implementation must reference its legacy analysis:

text

```text
Legacy source
    ↓
Legacy system documentation
    ↓
Behavior specification
    ↓
Replacement architecture
    ↓
Rust implementation
    ↓
Unit tests
    ↓
System tests
    ↓
Performance verification
```

No major subsystem should be implemented without this traceability.

### 6.2 Changelog rules

Every meaningful change must document:

- Change identifier.
- Date.
- Affected subsystem.
- Reason for change.
- Previous behavior.
- New behavior.
- Performance implications.
- Security implications.
- Tests performed.
- Documentation references.

## 7. Agent and subagent organization

Use local AI agents to divide independent research and implementation work.

However, agents should not be allowed to independently modify shared architectural interfaces.

Recommended organization:

Project Coordinator

Requirements · Scheduling · Final approval

Legacy Analysis

Source, dependencies, REA

Behavior Research

Traces, specifications, mocks

Rust Architecture

Crates, interfaces, standards

Graphics

Vulkan, AMD, frame pacing

Gameplay

World, physics, input

Audio & UI

Localization, dialogue, voice

Testing

Unit, system, regression

Performance

CPU, GPU, memory

Platform

Void, musl, glibc, Wine

Security

Offline, privacy, isolation

### Agent rules

1. Agents receive only the source directories necessary for their task.
2. All legacy source mounts remain read-only.
3. Agents cannot independently change public crate interfaces.
4. Every generated implementation requires tests.
5. Every finding requires source references or behavioral evidence.
6. No agent may introduce an external network dependency.
7. No agent may install unapproved dependencies.
8. All decisions are reviewed centrally.
9. Agents must produce concise completion reports.
10. Human approval is required before merging major changes.

## 8. Performance targets and qualification criteria

These are initial engineering targets, not claims about what the hardware will achieve. Establish a real baseline before fixing final thresholds.

| Metric | Initial target |
| --- | --- |
| Frame-rate cap | 244 FPS |
| Frame budget | 4.098 ms |
| Frame generation | None |
| GPU API | Vulkan |
| GPU priority | AMD |
| CPU utilization | Efficient, scalable |
| Memory | Bounded and profiled |
| Startup | Measured and optimized |
| Input latency | Minimized and measured |
| Network activity | Zero |
| Runtime telemetry | Zero |
| External AI requests | Zero |

For peak-performance analysis, capture:

- Average FPS.
- 1% low FPS.
- 0.1% low FPS.
- P50/P95/P99 frame times.
- CPU time per subsystem.
- GPU execution time.
- GPU memory consumption.
- System memory consumption.
- Allocation frequency.
- Cache misses.
- Thread scheduling delays.
- Input-to-display latency.

Important: A 244 FPS cap does not mean that every CPU core should remain at 100%. Unnecessary saturation can increase latency and reduce frame stability.

## 9. Additional technical decisions I recommend

There are several areas where the initial requirements need a more precise engineering definition.

### 9.1 musl and glibc

Treat these as separate build and compatibility targets.

Rust can target both environments, but native dependencies, Vulkan loader availability, audio backends, and system libraries must be independently validated.

Do not assume that a musl binary will automatically work in every Void Linux installation.

### 9.2 AMD-first Vulkan implementation

Use a vendor-neutral Vulkan backend with AMD as the primary validation platform.

Avoid hardcoding AMD-only functionality into the renderer.

This makes NVIDIA compatibility substantially easier while keeping the primary optimization effort focused on AMD.

### 9.3 GPU and CPU utilization

Implement separate worker pools for:

- Game simulation.
- Asset processing.
- Rendering preparation.
- Audio processing.
- Background I/O.

Avoid allowing background asset processing to interfere with the frame-critical execution path.

### 9.4 Network removal

Removing multiplayer must not accidentally remove essential local gameplay logic.

Before eliminating any network-related component, establish whether it performs additional functions such as world initialization, progression, configuration, or save validation.

Replace those functions locally before removing their dependency.

### 9.5 Legacy security and licensing functionality

Document every component before removal. Keep the modernization limited to functionality you own or are authorized to modify. Do not bypass third-party DRM or external access controls. The new implementation should have its own transparent local integrity and asset-validation model without unnecessary user tracking.

## 10. Final project execution gates

The project should proceed through the following gates.

| Gate | Required deliverable | Approval condition |
| --- | --- | --- |
| G00 | Project foundation | Source preservation verified |
| G01 | Offline infrastructure | Container isolation verified |
| G02 | Legacy inventory | Source coverage documented |
| G03 | Behavioral analysis | Core behavior understood |
| G04 | System specifications | Required documents completed |
| G05 | Rust foundation | Architecture and coding standards enforced |
| G06 | Core gameplay | Local gameplay operational |
| G07 | Offline conversion | No multiplayer requirement |
| G08 | Localization | All required languages functional |
| G09 | Performance | Targets measured and qualified |
| G10 | Platform compatibility | Void and Wine validated |
| G11 | Release | Full acceptance suite passes |

Написание текста

# 11\. Master implementation rules

These rules apply to every task, crate, subsystem, test, and release.

## Source code

- Legacy source code is strictly read-only.
- New source code belongs exclusively to the new repository.
- No modifications to original source files.
- Every reused component must have documented ownership and licensing permission.
- No undocumented third-party code imports.

## Rust

- Google Rust Style Guide is mandatory.
- All code must pass formatting and lint checks.
- Unsafe Rust must be minimized and justified.
- All unsafe blocks require precise safety comments.
- Prefer explicit ownership and borrowing.
- Avoid unnecessary allocations.
- Avoid unnecessary abstractions.
- Keep modules and crates focused.
- Every public API must have appropriate documentation.

## Architecture

- Rust-first implementation.
- Modular monolith.
- Explicit interfaces.
- Mock-first development.
- No unnecessary networking.
- No unnecessary background services.
- No unnecessary runtime dependencies.
- No hidden external communication.
- Deterministic behavior wherever practical.

## Testing

- Every feature requires unit tests.
- Every feature requires system tests.
- Integration tests are mandatory for subsystem boundaries.
- Behavioral tests must operate offline.
- Podman containers must be isolated.
- All test fixtures must be local.
- Every test failure must be reproducible.
- Performance changes must be benchmarked.

## Documentation

- All documentation belongs in `/docs`.
- Every analyzed legacy subsystem gets its own Markdown document.
- Every replacement references its analysis document.
- Complex code receives short, meaningful comments.
- Architectural decisions belong in `/docs/decisions`.
- Meaningful changes belong in `/CHANGELOG.md`.
- No undocumented architectural changes.

## Privacy and network isolation

- No user tracking.
- No device identification.
- No telemetry.
- No external AI services.
- No external analytics.
- No external crash reporting.
- No mandatory online services.
- No unexpected DNS or socket activity.
- No source-code uploads.
- No network dependency in ordinary gameplay.

## Release requirements

- Reproducible builds.
- Verified dependencies.
- Offline execution.
- Verified localization.
- Verified Void Linux compatibility.
- Verified AMD Vulkan compatibility.
- Verified Wine compatibility.
- Complete test reports.
- Complete documentation.

**Definition of Done:** A subsystem is complete only when its implementation, tests, documentation, offline behavior, performance characteristics, and acceptance criteria have all been verified.

## 12. Recommended implementation order and prioritization

Although the work breakdown contains many parallel tasks, I would not begin implementing the renderer, gameplay, localization, and persistence simultaneously.

Use these priorities:

Priority 1 — Foundation

Critical

Source preservation, offline infrastructure, indexing, dependency analysis, and documentation.Priority 2 — Behavior

Critical

Runtime analysis, gameplay specifications, deterministic traces, and mock interfaces.Priority 3 — Core Rust engine

Critical

Application lifecycle, world simulation, rendering, input, audio, persistence.Priority 4 — Offline conversion

Critical

Local-only gameplay and elimination of mandatory online dependencies.Priority 5 — Localization

High

Multilingual UI, Arabic, Cyrillic, Mandarin, voice-over, and subtitles.Priority 6 — Optimization

High

Vulkan, AMD, CPU scheduling, memory, ASM profiling, and 244 FPS qualification.Priority 7 — Compatibility and release

High

Void Linux, musl, glibc, Wine, NTSync, and release qualification.

## 13. What I would establish before starting the actual project

There are five particularly important unknowns that determine the size and order of the implementation:

1. Legacy engine architecture: Whether the game uses a custom engine, a modified commercial engine, or a recognizable open-source engine.
2. Actual source composition: Whether the 500 GB includes assets, generated files, build artifacts, multiple revisions, or 500 GB of actual source.
3. Gameplay dependency: Which game features currently require server-side validation or multiplayer synchronization.
4. Graphics architecture: Whether the current renderer is DirectX, OpenGL, Vulkan, or a proprietary abstraction.
5. Asset ownership: Which existing assets, voice recordings, music, fonts, and scripts can legally be carried into the new implementation.

These questions should be resolved during the first analysis stage, not guessed during architecture design.

My strongest recommendation: Make Phase 02 and Phase 03 the most thoroughly executed parts of the entire project. A well-documented legacy game with deterministic behavioral specifications is considerably easier to reimplement correctly than a massive collection of C++ files analyzed only through source translation.

The next practical deliverable should be a complete Phase 00–03 execution specification, including individual task IDs, agent assignments, source-analysis workflows, Podman profiles, documentation templates, test contracts, and completion checklists. That would turn this master plan into an executable research and development program.
