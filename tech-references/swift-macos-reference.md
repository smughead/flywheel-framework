# Swift/macOS Reference for Flywheel Framework

This document contains Swift and macOS-specific patterns, review agents, and constraints for use with the [Methodology](../Methodology.md).

---

## Stack Differences: Web vs Swift/macOS

| Aspect | Web (JS/TS/Ruby) | Swift/macOS | Implication |
|--------|------------------|-------------|-------------|
| **Build speed** | Fast (hot reload) | Slower (full compilation) | Need explicit build verification |
| **Testing** | Jest/Vitest (fast) | Swift Testing (slower) | Test planning more critical |
| **Architecture** | APIs, microservices | Layered, system constraints | Must respect threading rules |
| **Deployment** | CI/CD, containers | App Store, code signing | Different verification criteria |
| **Memory** | Garbage collected | ARC, ownership model | Review for retain cycles, Sendable |

---

## Build & Test Tools

### XcodeBuildMCP Integration

Use XcodeBuildMCP tools for build operations:

```bash
# Build macOS app
mcp__xcodebuildmcp__build_macos

# Run tests
mcp__xcodebuildmcp__test_macos

# Fallback xcodebuild
xcodebuild -scheme [SchemeName] -destination 'platform=macOS' -quiet build
```

### Build Verification Checkpoints

Swift compilation is slower than interpreted languages. Add explicit verification:

1. **After each logical chunk** — Build to catch type errors early
2. **Before moving to next file** — Ensure changes compile together
3. **Before commit** — Full build + test pass required

---

## Swift-Specific Review Agents (6 Total)

### 1. swift-safety-reviewer

**Purpose:** Memory safety, concurrency compliance, ownership patterns

**Checks:**
- Memory safety (ARC, retain cycles)
- Actor isolation and Sendable conformance
- Swift 6 strict concurrency compliance
- Weak/unowned reference patterns
- Optional handling (force unwraps, implicit unwraps)

**Prompt Template:**
```markdown
Review this Swift code for memory safety and concurrency compliance:

1. **Retain Cycles:** Check for strong reference cycles in closures, delegates,
   and parent-child relationships. Flag missing [weak self] in escaping closures.

2. **Actor Isolation:** Verify @MainActor annotations are explicit where needed.
   Check for data races in async code. Confirm Sendable conformance for cross-actor types.

3. **Swift 6 Compliance:** Flag any patterns that would fail under strict concurrency.

4. **Ownership:** Check for appropriate use of weak vs unowned. Flag force unwraps
   that could crash.

Files to review: [file paths]
```

### 2. audio-performance-analyzer

**Purpose:** Real-time audio constraints, threading, latency

**Checks:**
- No allocations in audio callbacks
- Thread safety (audio thread vs main thread)
- Latency analysis (buffer sizes, processing time)
- Lock-free patterns in signal processing
- Queue priorities (dedicated audio queues)

**Prompt Template:**
```markdown
Review this audio code for real-time performance:

1. **No Allocations:** Audio callbacks must not allocate memory. Flag any:
   - Array/Dictionary creation
   - String operations
   - Object instantiation
   - Closure captures that allocate

2. **Thread Safety:** Audio callbacks run on dedicated queues. Check for:
   - Main thread calls from audio callbacks
   - Lock acquisition (mutexes, semaphores)
   - Blocking operations (I/O, network)

3. **Latency:** Processing must complete within buffer window. Check:
   - Processing time vs buffer size
   - Unnecessary copies
   - Heavy computations in callback path

4. **Queue Configuration:** Verify dedicated audio queues use appropriate QoS
   (.userInitiated or higher).

Files to review: [file paths]
```

### 3. swiftui-patterns-checker

**Purpose:** Modern SwiftUI patterns, state management, observation

**Checks:**
- @Observable vs @ObservableObject (prefer @Observable for iOS 17+)
- @MainActor annotations (explicit vs implicit)
- @ObservationIgnored for high-frequency data
- State management patterns
- View body complexity

**Prompt Template:**
```markdown
Review this SwiftUI code for modern patterns:

1. **Observation:** Check @Observable usage (iOS 17+). Flag @ObservableObject
   that should migrate. Verify @ObservationIgnored for frequently-changing
   properties that don't need view updates.

2. **MainActor:** Verify @MainActor annotations are explicit on view models.
   Check async operations properly dispatch to main.

3. **State Management:** Check @State vs @Binding usage. Flag state that
   should lift to parent. Verify single source of truth.

4. **View Body:** Flag complex logic in body. Check for appropriate use of
   computed properties vs methods.

Files to review: [file paths]
```

### 4. macos-compatibility-reviewer

**Purpose:** Permissions, entitlements, sandbox, API availability

**Checks:**
- Permissions and entitlements required
- Sandbox compliance
- macOS version compatibility (deployment target)
- Deprecated API usage
- Privacy manifest requirements

**Prompt Template:**
```markdown
Review this macOS code for platform compatibility:

1. **Permissions:** Identify required permissions (microphone, camera, screen
   recording, etc.). Verify Info.plist usage descriptions exist.

2. **Entitlements:** Check for capabilities that require entitlements. Verify
   entitlements file matches required capabilities.

3. **Sandbox:** Flag operations that may fail in sandbox (file system access,
   network, inter-process communication).

4. **API Availability:** Check @available annotations. Flag deprecated APIs.
   Verify deployment target compatibility.

5. **Privacy:** Check privacy manifest requirements for tracking, required
   reason APIs.

Files to review: [file paths]
```

### 5. test-coverage-analyzer

**Purpose:** Swift Testing patterns, test quality, coverage gaps

**Checks:**
- Swift Testing patterns (not legacy XCTest)
- Synthetic data usage (hardware-independent tests)
- Test coverage for critical paths
- Test isolation (no shared mutable state)
- Async test patterns

**Prompt Template:**
```markdown
Review test coverage and quality:

1. **Swift Testing:** Check for modern Swift Testing patterns (@Test, #expect).
   Flag XCTest patterns that should migrate.

2. **Synthetic Data:** Tests should not depend on hardware/network. Flag tests
   that require real devices, microphones, network calls.

3. **Coverage:** Identify critical paths not covered. Check edge cases and
   error paths.

4. **Isolation:** Flag shared mutable state between tests. Verify proper
   setup/teardown.

5. **Async:** Check async test patterns. Verify proper expectations for
   concurrent code.

Files to review: [file paths]
```

### 6. architecture-guardian

**Purpose:** Layer boundaries, dependency flow, single responsibility

**Checks:**
- Layer boundary violations (UI → Manager → Processing → Infrastructure)
- Dependency flow (unidirectional, no cycles)
- Single responsibility per component
- Inappropriate coupling
- Protocol/interface design

**Prompt Template:**
```markdown
Review architecture and design:

1. **Layer Boundaries:** Check that UI code doesn't directly access infrastructure.
   Verify proper abstraction layers.

2. **Dependency Flow:** Dependencies should flow one direction (UI → Domain → Data).
   Flag cycles or reverse dependencies.

3. **Single Responsibility:** Each type should have one reason to change. Flag
   types doing too much.

4. **Coupling:** Check for inappropriate tight coupling. Verify use of protocols
   for abstraction.

5. **Naming:** Types and methods should clearly express intent and layer.

Files to review: [file paths]
```

---

## Memory Safety Patterns

### Retain Cycle Prevention

```swift
// CORRECT: Weak reference in escaping closure
someAsyncOperation { [weak self] result in
    guard let self else { return }
    self.handleResult(result)
}

// INCORRECT: Strong reference creates retain cycle
someAsyncOperation { result in
    self.handleResult(result)  // Captured strongly
}
```

### Sendable Conformance

```swift
// Types crossing actor boundaries must be Sendable
actor AudioProcessor {
    func process(_ buffer: AudioBuffer) async {
        // AudioBuffer must be Sendable or copied
    }
}

// Value types are implicitly Sendable
struct AudioBuffer: Sendable {
    let samples: [Float]
}
```

### Actor Isolation

```swift
// Explicit MainActor for UI-related code
@MainActor
final class TranscriptViewModel: Observable {
    var transcripts: [Transcript] = []

    func updateUI() {
        // Guaranteed to run on main thread
    }
}

// Non-isolated async functions
nonisolated func fetchData() async -> Data {
    // Can run on any thread
}
```

---

## Audio Threading Patterns (Example Domain)

### The Rule
Never block, allocate, or acquire locks in audio callbacks.

### Why It Matters
Audio callbacks run every few milliseconds. Blocking causes glitches (pops, dropouts).

### Implementation Pattern

```swift
// CORRECT: Lock-free, pre-allocated
class AudioProcessor {
    private let processingQueue = DispatchQueue(
        label: "audio.processing",
        qos: .userInitiated
    )

    // Pre-allocated buffer
    private var buffer = [Float](repeating: 0, count: 4096)

    func processCallback(_ samples: UnsafePointer<Float>, count: Int) {
        // No allocations
        // No locks
        // No main thread calls
        memcpy(&buffer, samples, count * MemoryLayout<Float>.stride)
    }
}

// INCORRECT: Allocations in callback
func processCallback(_ samples: [Float]) {  // Array copy = allocation
    let filtered = samples.map { $0 * gain }  // New array = allocation
    DispatchQueue.main.async {  // Cross-thread dispatch in callback
        self.updateUI(filtered)
    }
}
```

### Checklist
- [ ] Audio callbacks use dedicated queues
- [ ] No allocations in callback code
- [ ] No locks acquired in callback path
- [ ] Processing time < buffer duration
- [ ] Pre-allocated buffers for processing

---

## macOS Permissions Reference

### Required Entitlements

| Feature | Entitlement Key |
|---------|-----------------|
| Microphone | `com.apple.security.device.audio-input` |
| Camera | `com.apple.security.device.camera` |
| Screen Recording | `com.apple.security.cs.disable-library-validation` |
| Network Client | `com.apple.security.network.client` |
| File Access | `com.apple.security.files.user-selected.read-write` |

### Info.plist Usage Descriptions

```xml
<key>NSMicrophoneUsageDescription</key>
<string>[Your app] needs microphone access to [reason].</string>

<key>NSCameraUsageDescription</key>
<string>[Your app] needs camera access to [reason].</string>
```

### Permission Request Pattern

```swift
func requestMicrophoneAccess() async -> Bool {
    let status = AVCaptureDevice.authorizationStatus(for: .audio)

    switch status {
    case .authorized:
        return true
    case .notDetermined:
        return await AVCaptureDevice.requestAccess(for: .audio)
    case .denied, .restricted:
        return false
    @unknown default:
        return false
    }
}
```

---

## XcodeBuildMCP Tool Reference

### Available Tools

| Tool | Purpose |
|------|---------|
| `build_macos` | Build macOS app |
| `test_macos` | Run macOS tests |
| `build_run_macos` | Build and run app |
| `clean` | Clean build products |
| `list_schemes` | List available schemes |
| `show_build_settings` | Show build configuration |
| `session-set-defaults` | Set project/scheme defaults |

### Session Configuration

```javascript
// Set project defaults for session
mcp__xcodebuildmcp__session-set-defaults({
    workspacePath: "/path/to/Project.xcworkspace",
    scheme: "ProjectName",
    suppressWarnings: true  // Reduce output noise
})
```

---

## Directory Structure (Swift/macOS)

```
.claude/
├── knowledge/
│   ├── patterns/
│   │   ├── audio-threading.md          # Audio callback constraints
│   │   ├── swiftui-observation.md      # @Observable best practices
│   │   ├── testing-strategies.md       # Synthetic data approaches
│   │   ├── memory-safety.md            # ARC, Sendable, actor isolation
│   │   └── macos-permissions.md        # Entitlements, sandbox rules
│   │
│   ├── learnings/
│   │   └── [date]-[feature].md         # Per-feature insights
│   │
│   └── reviews/
│       ├── swift-safety-checklist.md
│       ├── audio-performance-rubric.md
│       └── macos-compatibility.md
│
├── agents/
│   ├── swift-safety-reviewer.md
│   ├── audio-performance-analyzer.md
│   ├── swiftui-patterns-checker.md
│   ├── macos-compatibility-reviewer.md
│   ├── test-coverage-analyzer.md
│   └── architecture-guardian.md
│
└── skills/
    ├── build/
    ├── test/
    ├── review-swift/
    └── compound/
```

---

## Common Swift Pitfalls

### 1. Force Unwraps in Production Code
```swift
// AVOID in production
let value = optionalValue!

// PREFER explicit handling
guard let value = optionalValue else {
    logger.error("Missing expected value")
    return
}
```

### 2. Implicit Main Thread Assumptions
```swift
// AVOID: Assuming main thread
func updateUI() {
    label.text = newValue  // May crash if not main thread
}

// PREFER: Explicit annotation
@MainActor
func updateUI() {
    label.text = newValue  // Compiler-enforced main thread
}
```

### 3. Closure Retain Cycles
```swift
// AVOID: Timer retains self
timer = Timer.scheduledTimer(withTimeInterval: 1, repeats: true) { _ in
    self.tick()  // Strong reference
}

// PREFER: Weak capture
timer = Timer.scheduledTimer(withTimeInterval: 1, repeats: true) { [weak self] _ in
    self?.tick()
}
```

### 4. Blocking Main Thread with Sync Operations
```swift
// AVOID: Blocking main thread
let data = try Data(contentsOf: url)  // Blocks until complete

// PREFER: Async operation
let data = try await URLSession.shared.data(from: url).0
```

---

## Related Resources

- **Apple Developer Documentation:** Use `apple-docs` MCP for framework APIs
- **Swift Language Reference:** Use `/programming-swift` skill for syntax questions
- **WWDC Sessions:** Search with `mcp__apple-docs__search_wwdc_content`
