# vscode 中 C/C++ 的配置

> c, cpp, c++, vscode

[toc]

# &sect; vscode under linux

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