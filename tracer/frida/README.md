# Frida tracer for Tenet

This tracer is adapted from [Synacktiv's Frinet project](https://github.com/synacktiv/frinet) so it can emit text traces that match the Tenet format used by this repository. It hooks a function entry with Frida's Stalker and writes the trace to `tracer/frida/traces/`.

## Requirements
- Python 3
- [Frida](https://frida.re/) (installable with `pip install frida`)

## Usage
Run `trace.py` from this directory. The interface mirrors the upstream Frinet tool:

```bash
python3 trace.py spawn <binary_or_package> <module_name> <function_address> [options]
python3 trace.py attach <process_or_pid> <module_name> <function_address> [options]
```

Common options:
- `-a/--args` to pass comma-separated arguments when spawning (for example `"/bin/ls,-la"`).
- `-m/--multirun` to keep the hook active for multiple executions.
- `-e/--exclude` to exclude other modules from stalking (faster but memory tracing may be inaccurate).
- `-s/--slow` to use the multi-architecture JavaScript tracer when x86/ARMv7 support is required.
- `-E/--end` to stop at a specific end address instead of the function return.

The tracer will create files named like `<process>_<epoch>_<tid>.log` inside `tracer/frida/traces/`. Each new trace begins with a base comment such as `# SO: libfoo.so @ 0x1234000`, matching the Tenet text trace expectations (`demo/log.txt`).

## Notes
- Fast mode supports `arm64` and `x64`; other architectures fall back to the slower JavaScript implementation.
- Module map summaries are written when tracing every module using `--traced-module '*'`.
