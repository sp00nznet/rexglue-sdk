> [!NOTE]
> **This is a personal fork of [rexglue/rexglue-sdk](https://github.com/rexglue/rexglue-sdk), kept for the
> [Saints Row](https://github.com/sp00nznet/saintsrow) and [Vigilante 8 Arcade](https://github.com/sp00nznet/vig8) recompilations.** The changes here were made with an
> AI coding assistant. The ReXGlue maintainers have said they don't want AI-written pull requests, which
> is fair, so these stay in this fork instead of going upstream. We don't want to add work for their
> project, and nothing here is meant to be offered back unless they ask for it.
>
> Use [upstream](https://github.com/rexglue/rexglue-sdk) for anything else. Please don't report problems
> with this fork to the ReXGlue project.
>
> **Branch `saintsrow`** = upstream `development` at `bd833a2` plus these commits, and nothing else:
>
> | Change | Why |
> | --- | --- |
> | Skip zero-length indirect buffers and packet streams | A count of 0 builds a `RingBuffer` with capacity 0 and `set_write_offset` divides by zero. Hardware treats an empty IB as a no-op. |
> | Don't reset the image heap when reading the XEX | `Reset()` clears the page table for the whole `0x8xxxxxxx` heap, including the function dispatcher allocation at `image_end`. A static recomp loads one module. |
> | Re-attach native objects when the guest rewrites a dispatch header | Saints Row re-initialises its IO event and semaphore headers inline between waits. Each call minted a new native object (201 for one event), so waiters and signallers never met. |
> | Log unregistered indirect call targets instead of aborting | A development aid: one run lists every missing manifest entry. **Not** a behaviour you want in a finished game. |
> | Build libmspack from its real sources, not the symlinks (#1) | `cabextract/mspack/` is git symlinks; a Windows clone without symlink support gets one-line text files and `lzxd.c` doesn't compile. |
> | Build the `XUsbcam` exports (#2) | `xboxkrnl_usbcam.cpp` was left out of the build, so a title importing the Vision camera API (Vigilante 8 Arcade) doesn't link. |
>
> It does **not** add: new features, API changes, platform support, or anything Saints Row-specific beyond the
> dispatch-header workaround above. The rest of this README is upstream's, unchanged. The licence is
> upstream's BSD-3-Clause ([LICENSE](LICENSE)).

> [!CAUTION]
> This project is in early development. Expect things to not work quite right and there to be significant changes and breaking public API updates as development progresses. Contributions and feedback are welcome, but please be aware that the codebase is still evolving rapidly.

<h1 align="center">
  <br>
  <a href="https://github.com/rexglue/rexglue-sdk">
    <img src="https://github.com/rexglue/rexglue-media/blob/main/ReX_Banner.png" alt="ReXGlue banner">
  </a>
  <br>
  <br>
  <a href="https://discord.gg/CNTxwSNZfT">
    <img src="https://img.shields.io/badge/Discord-Join%20Server-5865F2?logo=discord&logoColor=white" alt="Discord">
  </a>
  <a href="https://github.com/rexglue/rexglue-sdk/stargazers">
    <img src="https://img.shields.io/github/stars/rexglue/rexglue-sdk" alt="rexglue-sdk stargazers">
  </a>
</h1>

ReXGlue converts Xbox 360 PowerPC code into portable C++ that runs natively on modern platforms.

ReXGlue is heavily rooted on the foundations of [Xenia](https://github.com/xenia-project), the Xbox 360 emulator. Rather than interpreting or JIT-compiling PPC instructions at runtime, ReXGlue takes a different path: it generates C++ source code ahead of time, an approach inspired by [XenonRecomp](https://github.com/hedge-dev/XenonRecomp) and [rexdex's recompiler](https://github.com/rexdex/recompiler).

Latest SDK builds and releases are published on [GitHub Releases](https://github.com/rexglue/rexglue-sdk/releases). Join the [Discord server](https://discord.gg/CNTxwSNZfT) for updates and share what you have created.

## Builds

| Channel | CI | Download |
| --- | --- | --- |
| Release | [![win-amd64](https://github.com/rexglue/rexglue-sdk/actions/workflows/build-win-amd64.yaml/badge.svg)](https://github.com/rexglue/rexglue-sdk/actions/workflows/build-win-amd64.yaml) [![linux-amd64](https://github.com/rexglue/rexglue-sdk/actions/workflows/build-linux-amd64.yaml/badge.svg)](https://github.com/rexglue/rexglue-sdk/actions/workflows/build-linux-amd64.yaml) [![linux-arm64](https://github.com/rexglue/rexglue-sdk/actions/workflows/build-linux-aarch64.yaml/badge.svg)](https://github.com/rexglue/rexglue-sdk/actions/workflows/build-linux-aarch64.yaml) | [Latest stable](https://github.com/rexglue/rexglue-sdk/releases/latest) |
| Nightly | [![nightly](https://github.com/rexglue/rexglue-sdk/actions/workflows/nightly.yaml/badge.svg)](https://github.com/rexglue/rexglue-sdk/actions/workflows/nightly.yaml) | [Latest pre-release](https://github.com/rexglue/rexglue-sdk/releases?q=prerelease%3Atrue) |

## Quickstart

For quick start guide, full CLI reference, and config file options, see the [wiki](https://github.com/rexglue/rexglue-sdk/wiki).

# **Disclaimer**
ReXGlue is not affiliated with nor endorsed by Microsoft or Xbox. It is an independent project created for educational and development purposes. All trademarks and copyrights belong to their respective owners. 

This project is not intended to promote piracy nor unauthorized use of copyrighted material. Any misuse of this software to endorse or enable this type of activity is strictly prohibited.


# Credits

## ReXGlue
- [Tom (crack)](https://github.com/tomcl7) - Project Founder
- [Loreaxe](https://github.com/Loreaxe) - Linux Contributor
- [mystixor](https://github.com/Mystixor) - Windows Contributor
- [Graine25](https://github.com/Graine25) - Project Support
- [Carlos Estrague (mrcmunir)](https://github.com/mrcmunir) - Linux / ARM64 Contributor
- [sanjay900](https://github.com/sanjay900) - Linux / SDL Contributor
- [Toby](https://github.com/TbyDtch) - Project Support
- [Roxxsen](https://github.com/Roxxsen) - CI/CD Contributor

The list above is not exhaustive. Thanks to everyone in the ReXGlue community who contributes code, files issues, tests builds, and keeps the project moving.

## Very Special Thank You:
- [Project Xenia](https://github.com/xenia-project/xenia/tree/master/src/xenia) - Their invaluable work on Xbox 360 emulation laid the groundwork for ReXGlue's development. This project (and numerous others) would not exist without their hard work and dedication.
- [XenonRecomp](https://github.com/hedge-dev/XenonRecomp) - For pioneering the modern static recompilation approach for Xbox 360. A lot of the codegen analysis logic and instruction translations are based on their work. Thank you!
- [rexdex's recompiler](https://github.com/rexdex/recompiler) - The OG static recompiler for Xbox 360. 
- Many others in the Xbox 360 homebrew and modding communities whose work and research have contributed to the collective knowledge that makes projects like this possible.
