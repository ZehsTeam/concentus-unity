# Concentus-Unity: Opus for Unity C#

This project is a fork of [Concentus](https://github.com/lostromb/concentus) v2.2.2, retargeted to compile against **.NET Standard 2.1**, fixing a `Span<T>` runtime incompatibility on Unity's modern Mono/CLR.

Concentus-Unity is an independent, unofficial fork. It is not affiliated with or endorsed by the original Concentus maintainers or by Unity Technologies; "Unity" in the name refers to compatibility with the Unity engine runtime.

This is a minimal retargeting fork, with no functional or API changes beyond what was required to compile under .NET Standard 2.1. Namespaces have been renamed from `Concentus` to `ConcentusUnity` to avoid assembly/namespace collisions when both the original Concentus and this fork are present in the same process.

## Why this fork exists

If you've hit `MissingMethodException` referencing a `Span<T>`-based method when using Concentus in Unity, this is why.

The original Concentus targets .NET Standard 2.0, which has no native `Span<T>` support, so it relies on the `System.Memory` NuGet polyfill package instead. On runtimes that supply `Span<T>` natively (such as Unity's modern Mono/CLR), this causes a type-identity mismatch between the polyfilled `Span<T>` Concentus was compiled against and the native `Span<T>` actually present at runtime, resulting in a `MissingMethodException` for any `Span`-based method, even though the method clearly exists in the assembly's metadata.

Retargeting to .NET Standard 2.1 resolves this: `Span<T>` is supplied natively by the runtime with no polyfill involved, so the type identity matches consumers running on modern .NET/Mono runtimes.

## Project status

This fork inherits its Opus implementation directly from Concentus v2.2.2, which itself is based on libopus master 1.1.2 (FIXED_POINT, with optional floating-point analysis) and includes a port of the libspeexdsp resampler. See the [original Concentus repository](https://github.com/lostromb/concentus) for details on the upstream implementation, testing methodology, and performance characteristics. None of that work has been redone or re-verified independently for this fork; only the target framework and namespaces have changed.

## License

This project is licensed under the same BSD-style license as the original Concentus project. See [LICENSE](./LICENSE) for the full text and original copyright holders.
