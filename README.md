
# WhiskMeUp

Embedded Rust firmware for RP-series microcontrollers (rp235x / rp2040 targets).

**Status:** Prototype — buildable via Cargo and loadable with pico tools or the VS Code tasks included in this workspace.

**Overview**
- Purpose: lightweight firmware repository targeting RP-series chips. Contains board linker scripts and build configuration for embedded Rust development.
- Supported targets: rp235x (RISC-V variant) and RP2040 (Raspberry Pi Pico family). Linker scripts in the repo: `rp2040.x`, `rp2350.x`, `rp2350_riscv.x`.

**Repository layout**
- `src/` — main firmware source (`main.rs`).
- `build.rs` — build script used during compilation.
- `Cargo.toml` — project manifest and dependencies.
- `rp2040.x`, `rp2350.x`, `rp2350_riscv.x` — linker scripts for different targets.
- `LICENSE-APACHE`, `LICENSE-MIT` — licensing files.

**Prerequisites**
- Install Rust (rustup): https://rustup.rs/
- Add any target toolchains/targets you need (examples below).
- For flashing to Raspberry Pi Pico or compatible boards, install `picotool` (or use the VS Code Raspberry Pi Pico extension configured in tasks).

Common target examples:

```bash
# Cortex-M (example)
rustup target add thumbv6m-none-eabi

# RISCV or other rp235x target (example name may vary)
rustup target add riscv32imc-unknown-none-elf
```

**Build**
- From the repository root, build a release firmware with:

```bash
cargo build --release
```

- Or use the provided VS Code task `Compile Project` / `Build + Generate SBOM (release)` which runs a release build and emits an SBOM.

**Flash / Run**
- The workspace provides a `Run Project` task that depends on the release build and uses the configured `picotool` path to load firmware.

Manual flashing (example with picotool):

```bash
# Replace <path-to-elf> with target/release/<your-binary>.elf
picotool load -x <path-to-elf> -t elf
```

If you use a different flashing tool (openocd, probe-rs, etc.), substitute the appropriate command.

**Development notes**
- This project is no-std embedded Rust; expect to work with hardware abstraction crates and PACs in `Cargo.toml`.
- Use `defmt` / RTT or semihosting as configured in the Cargo dependencies for debug logging if present.

**Contributing**
- Open an issue to discuss features or file a pull request for bug fixes and enhancements. Keep PRs scoped and testable on supported hardware.

**License**
- This project is dual-licensed under Apache-2.0 and MIT. See `LICENSE-APACHE` and `LICENSE-MIT`.
