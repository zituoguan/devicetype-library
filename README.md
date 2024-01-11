# NetBox 设备类型库

## 关于此库

此库旨在用于填充 [NetBox](https://github.com/netbox-community/netbox) 中的设备类型。
它包含一组用 YAML 表达的设备类型定义，按制造商排列。每个文件代表一个独立的物理设备类型（例如，制造商和型号）。这些定义可以加载到 NetBox 中，而不是手动创建新的设备类型定义。

如果您想为此库做出贡献，请在提交内容之前阅读我们的[贡献指南](CONTRIBUTING.md)。

**注意：截至 2023 年 3 月，Netbox-Device-Type-Library-Import 已并入 NetBox 社区组织。我们将尽快完全支持它。**
如果您想自动导入这些设备类型模板文件，有一个 NetBox 社区 Python 脚本
可以检查重复项，允许您选择性导入供应商等，可在此处获取 [netbox-community/Device-Type-Library-Import](https://github.com/netbox-community/Device-Type-Library-Import)。

## 设备类型定义

每个定义**必须**至少包括以下字段：

- `manufacturer`: 生产此设备类型的制造商名称。
  - 类型：字符串
- `model`: 设备类型的型号。每个制造商必须是唯一的。
  - 类型：字符串
- `slug`: 型号的 URL 友好表示。像型号一样，这也必须对每个制造商是唯一的。所有 slug 应该在前面加上制造商的名称和一个破折号，请参见下面的示例。
  - 类型：字符串
  - 模式：`"^[-a-zA-Z0-9_]+$"`。必须匹配以下字符：`-`、`_`、大写或小写的 `a` 到 `z`，数字 `0` 到 `9`。

>:test_tube: **有效示例**:
>```
>manufacturer: Dell
>model: PowerEdge R6515
>slug: dell-poweredge-r6515
>```

以下字段可以**可选**声明：

- `part_number`: 型号的另一种表示（例如 SKU）。(**默认值：无**)
  - 类型：字符串
> :test_tube: **示例**: `part_number: D109-C3`
- `u_height`: 设备类型在机架单位中的高度。支持 0.5U 的增量。(**默认值：1**)
  - 类型：数字（最小值 `0`，倍数 `0.5`）
> :test_tube: **示例**: `u_height: 12.5`
- `is_full_depth`: 一个布尔值，表示设备类型是否占据机架的前后面。(**默认值：true**)
  - 类型：布尔值
> :test_tube: **示例**: `is_full_depth: false`
- `airflow`: 设备的气流模式声明。(**默认值：无**)
  - 类型：字符串
  - 选项：
    - `front-to-rear`
    - `rear-to-front`
    - `left-to-right`
    - `right-to-left`
    - `side-to-rear`
    - `passive`
> :test_tube: **示例**: `airflow: side-to-rear`
- `front_image`: 表示此设备在 [elevation-images](elevation-images/) 文件夹中有一个正面立面视图。(**默认值：无**)
  - 注意：立面视图文件夹需要与此设备相同的文件夹名称。文件名还必须遵守 <VALUE_IN_SLUG>.front.<acceptable_format>
  - 类型：布尔值
> :test_tube: **示例**: `front_image: True`
- `rear_image`: 表示此设备在 [elevation-images](elevation-images/) 文件夹中有一个背面立面视图。(**默认值：无**)
  - 注意：立面视图文件夹需要与此设备相同的文件夹名称。文件名

还必须遵守 <VALUE_IN_SLUG>.rear.<acceptable_format>
  - 类型：布尔值
> :test_tube: **示例**: `rear_image: True`
- `subdevice_role`: 表明这是一个 `parent`（父）或 `child`（子）设备。(**默认值：无**)
  - 类型：字符串
  - 选项：
    - `parent`
    - `child`
> :test_tube: **示例**: `subdevice_role: parent`
- `comments`: 允许向设备添加注释的字符串字段。(**默认值：无**)
  - 类型：字符串
> :test_tube: **示例**: `comments: This is a comment that will appear on all NetBox devices of this type`
- `weight`: 表示数字重量值的数字。必须是 0.01 的倍数（2 位小数）。(**默认值：无**)
    - 类型：数字
    - 值：必须是 0.01 的倍数
- `weight_unit`: 定义测量单位的字符串。它必须是受支持的值之一。(**默认值：无**)
  - 类型：字符串
  - 值：枚举选项
    - kg
    - g
    - lb
    - oz
>:test_tube: **示例**:
>```
>weight: 12.21
>weight_unit: lb
>```
- `is_powered`: 一个布尔值，表示设备类型不使用电源。这主要用作对非设备（例如，用于安装桌面设备的机架安装套件）的验证测试的解决方法 (**默认值：True**)
  - 类型：布尔值
> :test_tube: **示例**: `is_powered: false`

有关这些属性的更多详细信息以及下面列出的属性，请参考
[架构定义](schema/) 和下面的 [组件定义](#component-definitions)。

### 组件定义

下面列出了有效的组件类型。每种类型的组件必须声明要添加的单个组件模板的列表。

- [`console-ports`](#console-ports "在 NetBox 2 及以后版本中可用")
- [`console-server-ports`](#console-server-ports "在 NetBox 2.2 及以后版本中可用")
- [`power-ports`](#power-ports "在 NetBox 1.7 及以后版本中可用")
- [`power-outlets`](#power-outlets "在 NetBox 2 及以后版本中可用")
- [`interfaces`](#interfaces "在所有版本的 NetBox 中可用")
- [`front-ports`](#front-ports "在 NetBox 2.5 及以后版本中可用")
- [`rear-ports`](#rear-ports "在 NetBox 2.5 及以后版本中可用")
- [`module-bays`](#module-bays "在 NetBox 3.2 及以后版本中可用")
- [`device-bays`](#device-bays "在所有版本的 NetBox 中可用")
- [`inventory-items`](#inventory-items "在 NetBox 3.2 及以后版本中可用")

每种类型的组件的可用字段如下所示。

#### 控制台端口

- `name`: 名称
- `label`: 标签
- `type`: 端口类型别名（数组）
- `poe`: 此端口是否访问/提供 POE？（布尔值）

#### 控制台服务器端口

- `name`: 名称
- `label`: 标签
- `type`: 端口类型别名（数组）

#### 电源端口

- `name`: 名称
- `label`: 标签
- `type`: 端口类型别名（数组）
- `maximum_draw`: 端口的最大功率消耗，以瓦特为单位（可选）
- `allocated_draw`: 端口的分配功率消耗，以瓦特为单位（可选）

#### 电源插座

- `name`: 名称
- `label`: 标签
- `type`: 端口类型别名（数组）
- `power_port`：电源插口的名称，此插口为设备提供电源（可选）
- `feed_leg`：此插口映射的电源相（腿）；A、B 或 C（可选）

#### 接口

- `name`：名称
- `label`：标签
- `type`：接口类型缩写（数组）
- `mgmt_only`：一个布尔值，指示此接口是否仅用于管理目的（默认：false）

#### 前端端口

- `name`：名称
- `label`：标签
- `type`：端口类型缩写（数组）
- `rear_port`：此前端端口映射到的设备后端端口的名称
- `rear_port_position`：映射到的后端端口上的相应位置（默认：1）

#### 后端端口

- `name`：名称
- `label`：标签
- `type`：端口类型缩写（数组）
- `positions`：可以映射到此后端端口的前端端口数量（默认：1）
- `poe`：此端口是否访问/提供 POE？（布尔值）

#### 模块托架

- `name`：名称
- `label`：标签
- `position`：此模块托架在父设备中的字母数字位置。创建模块组件时，组件名称中的 `{module}` 字符串将被替换为模块槽的 `position`。有关更多详细信息，请参见 [NetBox 文档](https://docs.netbox.dev/en/stable/models/dcim/moduletype/#automatic-component-renaming)。

#### 设备托架

- `name`：名称
- `label`：标签

#### 库存项目

- `name`：名称
- `label`：标签
- `manufacturer`：生产此物品的制造商的名称
- `part_id`：制造商分配的部件 ID

## 数据验证 / 提交质量检查

此仓库专注于保持高质量设备类型定义的两种方式：

- **预提交检查** - 可选，但**强烈推荐**，以帮助在提交前识别简单问题。（尾随空白、文件末尾修复器、检查 YAML、YAML 格式化、YAML 检查）
  - 安装
    - 虚拟环境路线
      - 建议为您的仓库创建虚拟环境（`python3 -m venv venv`）
      - 安装所需的 pip 包（`pip install -r requirements.txt`）
      - 继续到“安装 `pre-commit` 钩子”
    - 仅 `pre-commit` 路线
      - [安装 pre-commit](https://pre-commit.com/#install)（`pip install pre-commit`）
    - 安装 `pre-commit` 钩子
      - 安装预提交脚本：`pre-commit install`
  - 使用及有用的 `pre-commit` 命令
    - 使用 `git` 阶段化文件后，运行更改文件的预提交脚本：`pre-commit run`
    - 运行所有文件的预提交脚本：`pre-commit run --all`
    - 卸载预提交脚本：`pre-commit uninstall`
  - 了解更多关于 [pre-commit](https://pre-commit.com/)
- **GitHub 动作** - 在 PR 合并之前自动运行。重复执行 yamllint 并根据 NetBox 设备类型模式进行验证。