# luci-app-honk

OpenWrt 上的 honk（eBPF 透明代理引擎，dae 兼容）LuCI 管理界面与二进制包。

本仓库包含两个包：

- `honk`：从 [daeuniverse/honk](https://github.com/daeuniverse/honk) 的
  GitHub Release 按目标架构下载预编译静态 `honk-core`，不在本地编译。
- `luci-app-honk`：参考 `luci-app-dae` 实现的 LuCI 配置界面。

## 使用

把本仓库放入 OpenWrt 源码树的 `package/honk`：

```bash
git clone https://github.com/QiuSimons/luci-app-honk package/honk
```

然后：

```bash
make menuconfig
# Network -> Web Servers/Proxies -> luci-app-honk
make package/honk/luci-app-honk/compile V=s
```

honk 目前只提供 `x86_64` 与 `aarch64` 的静态 musl 二进制，因此包通过
`@(x86_64||aarch64)` 限制架构。

## 配置

默认配置在 `/etc/honk/config.dae`，拆分配置位于
`/etc/honk/config.d/{node,route,dns,api}.dae`。首次使用前请替换示例节点与订阅。

## 控制面板 (Doona)

> [!NOTE]
> 当前 **Doona** 控制面板仍在积极开发中。如需尝试体验相关功能（如 Native API、配置在线编辑与 GeoData 规则集更新等），需要自行替换 debug 版本的 `honk-core` 核心（替换路径为 `/usr/bin/honk-core`）。

## 版本号

`scripts/update_honk_version.sh` 会读取 daeuniverse/honk 最新 Release，
把 tag（如 `v0.0.1.beta.49`）清洗成同时满足 ipk 与 apk 的版本
（`0.0.1_beta49`），并写回 `honk/Makefile`。

