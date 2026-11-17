# Agent Note: 鸿蒙未签名构建使用 GitHub 托管 runner

Status: implemented

## Problem

现有 HAP 工作流仅使用自托管 runner，新的 fork 没有这类 runner 时无法完成未签名编译。

## Decision

手动未签名构建使用临时 Ubuntu runner，复用固定提交的 `ErBWs/setup-ohos` 与 API 26 工具链。工作流校验 SDK 发布引用，使用现有签名脚本移除签名引用，不加载签名 Secrets。

标签及手动签名构建保留自托管 runner 与既有凭据注入边界；不增加 PR 触发器。两种构建均组装完整 APP，验证包结构与应用身份，附带校验值及来源信息。自托管 runner 因包校验新增 Python 3 前提。

## Alternatives considered

- 继续仅使用自托管 runner：无需安装 SDK，但 fork 需要额外机器和 runner 运维，不能直接完成本次构建。
- 新建独立未签名工作流：不触碰原工作流，但会重复依赖安装、签名配置与上传步骤，因此沿用单一入口。

## Consequences

fork 可以直接手动生成未签名 HAP 和 APP；不自动构造签名材料，也不表示真机安装或运行已通过。SDK 下载依赖固定第三方 action 和镜像，发布引用检查失败时停止，不静默切换版本。

未发现已有 `.agents/notes/` 决策记录；本笔记不归档或替代其他决定。
