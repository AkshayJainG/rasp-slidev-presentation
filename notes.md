# Notes: Three-RASP Slidev Deck

## Sources

### Existing talk outline
- Internal long-form outline.
- Key points:
  - Strong 5-step methodology: recon, harden Frida, bypass detection, extract, map logic.
  - Existing talk frames the second engine as RASP B and Protectt.ai as RASP C.
  - Best Protectt.ai hook: GOT monitor log trip-wire on the `G-M` line.

### Promon Shield case study
- Banking app target on `com.snapwork.hdfc`, version 11.2.3.
- Objective: bypass hook/root detection and capture network traffic.
- Protector: Promon Shield, identified by APKiD on the packed native library.
- Java code was not heavily obfuscated, but strings were encrypted and resolved by a single decryptor.
- Native library was packed; analysis came from runtime dumping after `android_dlopen_ext`, followed by SoFixer repair.
- Dynamic tracing covered `dlsym`, `prctl`, `fork`, and direct ARM64 `svc #0` syscalls.
- Detection surface: Frida artifacts, `/proc/self/maps`, `/proc/self/task/*/comm`, root/Xposed/Substrate paths, APK signing reads, and port/thread artifacts.
- Bypass: patch Frida strings, run Frida server on a non-default port, hook the string decryptor, identify Java callbacks, return false for root/ADB checks, and use SSL unpinning scripts to capture traffic.

### RASP B (anonymised)
- Heavyweight ARM64-only Android protection engine.
- Architecture: a small JNI bridge fronts a multi-megabyte native engine (~19 MB).
- Obfuscation: fake C++-style symbol names, ChaCha20-encrypted vendor-tagged strings, control-flow-flattened Java.
- Detection: 37 unique detection codes across root, bootloader, TEE, injection, modules, process anomalies, environment spoofing.
- Bypass theme: zero-libc-hook approach using exception-handler NX redirects, `NativeCallback` wrappers, instrumentation cloaking, and Java hooks.
- Result framed as a reliable analysis window: dumps, string capture, and code mapping completed; direct-syscall termination remained timing-sensitive.

### Protectt.ai
- Equitas Bank `com.equitas.elevate`, Protectt.ai SDK `v2.2.41`.
- Native: `libprotectt-native-lib.so` core engine plus `libapp-protectt-native-lib.so` bridge.
- Many JNI exports are plaintext (`findFridaServer`, `check_maps`, `isHookingTracess`, `CloseApplication`).
- Kill chain has five layers: Java anti-return, Java kill-process, native abort/exit through GOT, native `syscall(exit_group)`, and GOT monitor log + SIGSEGV.
- Final bypass required all layers simultaneously, especially `pthread_exit` code islands and the log-based trip-wire on `AppProtectt` / `G-M`.

## Narrative

1. Define RASP as runtime self-defense.
2. Show a repeatable reversing workflow.
3. Promon Shield: "the production artifact detector" — packing, runtime unpacking, direct syscalls, Frida artifact checks.
4. RASP B: "the black box" — encrypted strings, fake symbols, direct syscalls, prologue scanning.
5. Protectt.ai: "the layered kill chain" — visible functions but more operational resilience.
6. Compare the three: Promon recognises the toolchain; RASP B detects the hook; Protectt.ai composes redundant kills.
7. Close with engineering lessons and a playbook for unknown RASPs.

## Slide Style

- Tone: dark editorial field report, industrial and precise.
- Visual motifs: terminal green, amber warnings, thin borders, monospaced code labels.
- Avoid giant tables on slides; use compact matrices and move detail into speaker notes.
- Use Mermaid diagrams for architecture and kill chains.
