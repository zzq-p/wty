# LogicSim 逻辑关系图

在线访问：**https://wty.github.io/**

一个纯静态、离线可用的逻辑表达式图形编辑器：输入逆波兰表达式，生成可拖拽的逻辑关系图，编辑节点名称与备注，导入导出 JSON 模型。所有计算均在浏览器本地完成，不联网、不上传数据。

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
    └── README.md               # 详细使用说明与表达式规则
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

重构自 <https://kuangdash.gitlab.io/logicsim/> 的本地副本，`lib/` 内保留各库原有的版权声明。许可条款见 [LICENSE](LICENSE)。
