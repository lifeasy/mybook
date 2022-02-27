# 编译命令

## 编译命令swiftc

```bash
# 生成语法树
swiftc -dump-ast main.swift
# 生成最简洁的SIL代码
swiftc -emit-sil main.swift
# 生成LLVM IR代码
swiftc -emit-ir main.swift -o main.ll
# 生成汇编代码
swiftc -emit-assembly main.swift -o main.s
```

