# vscode 中 C/C++ 的配置

> c, cpp, c++, vscode

# &sect; vscode on linux

## &para; manjaro deepin

```bash
$ tree .vscode
```

.vscode 文件夹结构

```bash
.vscode/
├── c_cpp_properties.json
├── launch.json
├── settings.json
└── tasks.json
```

C/C++ 配置 c_cpp_properties.json

```json
{
    "configurations": [
        {
            "name": "Linux",
            "includePath": [
                "${workspaceFolder}/include",
                "/usr/lib/gcc/x86_64-pc-linux-gnu/9.1.0/include"
            ],
            "defines": [],
            "compilerPath": "/usr/bin/gcc",
            "cStandard": "c11",
            "cppStandard": "c++17",
            "intelliSenseMode": "gcc-x64"
        }
    ],
    "version": 4
}
```

# &sect; vscode on Windows

## &para; Problem

1. "std" 没有成员 "cout"

> 在 `Setting` -> `C_Cpp: Intelli Sense Engine` 中，改成 `Tag Parser`