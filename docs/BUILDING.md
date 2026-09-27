# Building

Run from the repository root with Python 3.10+:

```sh
python -B scripts/check.py
python -B scripts/build.py
```

Install LuaJIT and/or Lua to run the policy suite. With both installed, both are tested.
The installable package is `dist/HD2-G60-Smart-Targeting-0.1-beta.zip`.
The builder uses only Python's standard library and never installs the addon or connects to a game process.

`src/g60` holds authored modules. `addon/entry.lua.in` supplies the loader entry and runtime checks.
`compat` holds the reviewed binding, signatures, IDs, priority catalog and model-derived profiles used by this release.
The public assembler freezes those inputs instead of requiring private executable captures or local asset databases.
It rebuilds the original 0.5.19 entry and its three deployment files byte for byte; the public version applies to the
package and release metadata. Existing log/version strings are deliberately retained.

`scripts/check.py` verifies exact release parity. After intentional runtime changes, that check will fail until
new native tests and gameplay validation justify updating the expected release evidence. Do not merely update hashes to silence it.
`scripts/build.py` can still build a changed checkout, but reports `matches_tested_payload: false`.

## Optional Windows fixture tests

The source includes a C fixture DLL and Windows LuaJIT integration suites. The fixture is built locally and is
never placed in the installable package. To run under Wine, supply a Windows x64 C compiler, a local compatible
`lua-tool.exe` runner and a Windows LuaJIT `lua51.dll`:

```sh
python -B scripts/test_windows.py --compiler /path/to/x86_64-w64-mingw32-clang \
  --runner /path/to/lua-tool.exe --lua-dll /path/to/lua51.dll
```

The runner contract is `lua-tool.exe <lua51.dll> <script.lua> <unused-output> run`.
Set `WINEPREFIX` to an isolated test prefix. These optional tools and proprietary game DLLs are not distributed here.
Tests call only the fixture and synthetic world observations, never a running game's native functions.
