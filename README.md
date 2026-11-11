[![CI](https://github.com/allyourcodebase/libffi/actions/workflows/ci.yml/badge.svg)](https://github.com/allyourcodebase/libffi/actions)

# brightshard's fork

Forked from <https://github.com/allyourcodebase/libffi>. This adds a build step that translates LibFFI C headers to Zig, so Zig code can use LibFFI.

```zig
// build.zig
const libffi = b.dependency("libffi", .{
    .target = target,
    .optimize = optimize,
});

const exe = std.Build.ExecutableOptions{
    .name = "urmom",
    .root_module = b.createModule(.{
        .root_source_file = b.path("src/main.zig"),
        .target = target,
        .optimize = optimize,
        .imports = &.{
            .{ .name = "libffi", .module = libffi.module("libffi") },
        },
    }),
};
exe.root_module.linkLibrary(libffi.artifact("ffi"));

// src/main.zig
const libffi = @import("libffi");
// then can use libffi.whateverFunction()
```

# libffi

This is [libffi](https://sourceware.org/libffi/), packaged for [Zig](https://ziglang.org/).

## Installation

First, update your `build.zig.zon`:

```
# Initialize a `zig build` project if you haven't already
zig init
zig fetch --save git+https://github.com/allyourcodebase/libffi.git
```

You can then import `libffi` in your `build.zig` with:

```zig
const libffi = b.dependency("libffi", .{
    .target = target,
    .optimize = optimize,
});
exe.root_module.linkLibrary(libffi.artifact("ffi"));
```
