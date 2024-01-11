# 贡献

此库由 NetBox 用户社区维护。因此，准确和完整的设备类型定义的定期贡献对其成功至关重要。

## 编写定义文件

每个 NetBox 设备类型定义都作为一个单独的 YAML 文件存在，按制造商（供应商）排列。文件通过将定义的型号名称与 `.yaml` 扩展名连接来命名。例如：

```no-highlight
device-types/Acme/BFR-1000.yaml
device-types/Acme/BFR-2000.yaml
```

编写新定义时，有一些重要的准则需要遵循：

- 每个独特的型号号码都需要一个单独的定义文件，即使组件集合是相同的。
- 定义文件必须以 `.yaml` 或 `.yml` 结尾。
- 创建制造商目录时使用适当的、对人类友好的名称（例如，使用 `Alcatel-Lucent` 而不是 `alcatel`）。
- 仅包括固定在机箱上的组件。对于所有可以在 NetBox 中建模的模块化组件（网络模块、电源等），应包括模块槽和相关模块类型。
- 按照设备操作系统中显示的方式准确命名组件（与物理机箱标签不同的情况下）。
- 在适用的情况下使用接口名称的完整形式。例如，使用 `TenGigabitEthernet1/2/3` 而不是 `Te1/2/3`。

此外，请确保遵守以下风格指南：

- 使用两个空格进行缩进。
- 在列出其组件之前指定设备类型的属性。
- 避免在不需要避免语法错误的情况下用引号封装 YAML 值。
- 每个定义文件以空行结束。

## 贡献工作流程

向库提交新定义的过程如下：

1. 验证所提出的定义是否与现有定义重复或冲突。（如果不确定，请在提交 PR 之前提出问题寻求澄清。）
2. [Fork](https://guides.github.com/activities/forking/) GitHub 项目并创建一个新分支来保存您的建议更改。如果添加新定义，分支应命名为大致遵循格式 `<manufacturer>-<series>`（例如，`cisco-c9300`）。
3. 安装并运行 [README.md](README.md) 中描述的预提交脚本（可选，但建议）
4. 确切地介绍新内容，就像它被接受后应该显示的那样。
5. 创建一个 [草案拉取请求](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/about-pull-requests#draft-pull-requests)，以将您的新分支合并到 `master` 分支。在 PR 中包含对引入的更改的简要描述。
6. GitHub 将自动针对您的草案 PR 运行测试以验证它。如果测试失败，请对分支进行必要的更改，以便它们通过。
7. 提交 [拉取请求](https://github.com/netbox-community/devicetype-library/compare?expand=1) 进行审查。请注意，未通过验证的提交 PR 将被关闭，必须撤回。
8. 维护者将审查您的 PR 并采取以下三种操作之一：
   - 接受并合并它
   - 请求修订
   - 引用原因（例如，验证失败或与库不适用）关闭 PR
