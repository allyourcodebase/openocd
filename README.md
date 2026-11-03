[![CI](https://github.com/allyourcodebase/openocd/actions/workflows/ci.yml/badge.svg)](https://github.com/allyourcodebase/openocd/actions)

# openocd

This is [openocd](https://openocd.org/), packaged for [Zig](https://ziglang.org/).

## Usage

Safe builds are triggering ubsan, so fast/small needs to be used. 

To use the binary directly you can just do:
```
zig build -Doptimize=fast run -- -f interface/cmsis-dap.cfg -f target/rp2350.cfg
```

To use in `build.zig.zon` you first need to add it via:

```
zig fetch --save git+https://github.com/allyourcodebase/openocd.git
```

And after that you can just use the `openocd` artifact and `tcl` named lazy path in your `build.zig` with:

```zig
const openocd_dep = b.dependency("openocd", .{
    .optimize = .fast,
});
const openocd_run = b.addRunArtifact(openocd_dep.artifact("openocd"));
openocd_run.addArg("-s");
openocd_run.addDirectoryArg(openocd_dep.namedLazyPath("tcl"));
```
