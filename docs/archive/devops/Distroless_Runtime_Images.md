# Distroless Runtime Images

`gcr.io/distroless/cc-debian13:nonroot` 是由 Google 提供的 **Distroless runtime image**，設計用於 **執行已編譯的 native binary**，而不是作為完整 Linux 環境。

Distroless 的核心理念：

> 容器只包含「執行程式所需的最小 runtime」，不包含系統工具。

---

# Distroless 的設計原則

Distroless（distribution-less）刻意移除所有非必要組件：

不存在：

```
bash
sh
apt
apk
curl
wget
git
package manager
```

只保留：

```
glibc
dynamic loader
runtime libraries
CA certificates
minimal filesystem
```

這樣做的結果：

```
更小的 image
更少的 CVE
更小的 attack surface
```

Distroless 不是 Linux 發行版，而是：

> **production runtime environment for compiled applications**

---

# `cc` Image 的含義

`distroless/cc` 提供 **glibc-based C/C++ runtime**。

包含：

```
glibc
libstdc++
dynamic linker
TLS / CA certificates
```

適用於所有 **動態連結 glibc 的 native binary**：

```
C
C++
Rust
Zig
Go (CGO enabled)
```

其中 Rust 經常使用此 image，因為 Rust 的設計目標之一就是：

> memory-safe systems language replacing C/C++

如果 Rust binary 依賴 glibc，runtime image 通常選擇：

```
distroless/cc
```

---

# `nonroot` Tag

`nonroot` 表示容器預設使用 **非 root 使用者**：

```
UID 65532
```

目的：

```
避免 root privilege
符合 Kubernetes security policy
降低 container breakout 風險
```

這是 production container 的常見安全配置。

---

# Distroless 不包含 `apt`

Distroless **不提供 package manager**。

因此以下操作不會成功：

```
apt install
apt-get update
apk add
```

這是刻意設計，而不是缺失。

原則：

> runtime image 不應該在執行時安裝任何東西。

---

# 正確的使用模式

Distroless 必須與 **multi-stage build** 一起使用。

所有編譯與套件安裝都發生在 **builder stage**。

```
build stage
    ↓
install dependencies
compile binary
    ↓
runtime stage
    ↓
copy artifacts
run binary
```

---

# 典型 Rust Dockerfile

```dockerfile
FROM rust:1.93 AS builder

RUN apt-get update && \
    apt-get install -y pkg-config libssl-dev

WORKDIR /app
COPY . .

RUN cargo build --release

FROM gcr.io/distroless/cc-debian13:nonroot

COPY --from=builder /app/target/release/app /app/app

ENTRYPOINT ["/app/app"]
```

關鍵原則：

```
apt → builder stage only
distroless → runtime only
```

runtime image 只包含：

```
application binary
runtime libraries
```

---

# 如果需要額外 runtime library

某些 library 仍可能需要存在於 runtime image。

處理方式：

```
apt install (builder)
↓
copy required libraries
↓
runtime image
```

例如：

```dockerfile
COPY --from=builder /usr/lib/x86_64-linux-gnu/libsqlite3.so* /usr/lib/
```

這樣 runtime 仍然保持 minimal。

---

# Debug 與 Distroless

Distroless 不適合 debugging。

官方提供 debug 變體：

```
distroless:debug
```

例如：

```
gcr.io/distroless/cc-debian13:debug
```

這些 image 會包含：

```
busybox
sh
```

但仍然不提供 `apt`。

---

# Runtime Image 選擇

常見三種 runtime strategy：

| runtime image | 用途                    |
| ------------- | --------------------- |
| scratch       | fully static binary   |
| distroless    | dynamic glibc runtime |
| alpine        | minimal Linux distro  |

選擇邏輯：

```
static binary        → scratch
dynamic glibc binary → distroless
debuggable runtime   → alpine
```

---

# 心法

Distroless 的核心思想可以用一句話總結：

```
build everything
copy artifacts
run minimal runtime
```

如果你的 runtime image 需要：

```
apt
shell
package manager
```

那麼：

> 你大概率不應該使用 Distroless。
