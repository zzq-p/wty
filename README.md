# LogicSim 逻辑关系图

> ## ⚠️ 来源说明
>
> 本项目是 **[https://kuangdash.gitlab.io/logicsim/](https://kuangdash.gitlab.io/logicsim/)** 的**修改版**。
>
> - 原版地址：**<https://kuangdash.gitlab.io/logicsim/>**
> - 本仓库以该站的本地副本为基础，做了**代码重构**与**界面改版**，源码与样式均已修改
> - 这是**非官方版本**，与原站及原作者没有隶属关系，也不代表原站立场
> - 原版的版权与许可归原作者所有；第三方库版权见 [LICENSE](LICENSE) 与 `logicsim-refactored/lib/` 内的原始声明

在线访问：**https://zzq-p.github.io/wty/**

一个纯静态、离线可用的逻辑表达式图形编辑器：输入逆波兰表达式，生成可拖拽的逻辑关系图，编辑节点名称与备注，导入导出 JSON 模型。所有计算均在浏览器本地完成，不联网、不上传数据。

## 相对原版的改动

| 方面 | 改动 |
| --- | --- |
| 界面视觉 | 重写 `style.css`：卡片化布局、渐变顶栏与品牌标记、按钮/输入框/折叠面板/表格/状态栏重做、窄屏断点、`prefers-reduced-motion` 支持 |
| 画布外观 | 调整画布底色、连线颜色、节点圆角与选中描边（仅外观参数） |
| 表达式解析 | 改为逐词元检查栈深度，错误定位更准确；单个变量与常量不再被误判为错误 |
| 图形生成 | 改用有序二元决策图，对相同分支去重、对逻辑运算缓存，只绘制可达的选择节点 |
| 模型与校验 | 统一 `memo` 字段、保存节点位置、载入前校验（重复编号、悬空连线、循环连接等），校验失败保留旧图 |
| 规模保护 | 限制词元数、变量数、节点数、连线数与文件大小，避免无界增长 |
| 测试 | 新增 Node.js 原生回归测试 `logicsim-refactored/tests/core.test.cjs` |

完整的问题清单、算法说明与验证记录见 [logicsim-refactored/重构说明.md](logicsim-refactored/重构说明.md)。

## 目录结构

```
.
├── index.html                  # 入口页，自动跳转到编辑器
├── .nojekyll                   # 关闭 GitHub Pages 的 Jekyll 处理
└── logicsim-refactored/        # 编辑器本体（无构建步骤）
    ├── index.html              # 界面结构
    ├── style.css               # 视觉样式
    ├── src/core.js             # 表达式解析、决策图生成、模型校验（无 DOM 依赖）
    ├── src/app.js              # 画布、布局、缩放、节点属性、导入导出
    ├── lib/                    # JointJS、jQuery、lodash、Backbone、dagre、graphlib
    ├── assets/                 # 节点图标
    ├── tests/core.test.cjs     # 核心逻辑回归测试
    ├── README.md               # 详细使用说明与表达式规则
    └── 重构说明.md             # 重构清单、算法说明与验证记录
```

## 本地运行

编辑器无需安装依赖或构建，直接打开 `logicsim-refactored/index.html` 即可；也可用任意静态服务器（如 `npx serve .`）在本地预览。

运行核心逻辑测试（需要 Node.js）：

```sh
cd logicsim-refactored
npm test
```

## 部署

仓库根目录即为站点根目录，推送到 `main` 分支后由 GitHub Pages 直接提供服务。`.nojekyll` 保证 `lib/` 等目录以原样发布。

## 表达式速查

| 运算 | 输入 |
| --- | --- |
| 与 ∧ | `a b .` |
| 或 ∨ | `a b ,` |
| 非 ¬ | `a <` |
| 推出 → | `a b >` |
| 等价 ↔ | `a b =` |

变量与运算符之间用空格分隔，`0` / `1` 表示假 / 真。例如 `a b . fe >` 表示 (a ∧ b) → fe。详细规则见 [logicsim-refactored/README.md](logicsim-refactored/README.md)。

## 来源与许可

- **原版**：[LogicSim](https://kuangdash.gitlab.io/logicsim/) — <https://kuangdash.gitlab.io/logicsim/>
- **本仓库**：基于原版本地副本的修改版（derivative work），非官方，仅作学习与自用
- **第三方依赖**：`logicsim-refactored/lib/` 为原站使用的 JointJS、jQuery、Lodash、Backbone、Graphlib、Dagre 等库，均保留库文件内的原有版权声明，本次未升级版本
- **许可**：见 [LICENSE](LICENSE)
