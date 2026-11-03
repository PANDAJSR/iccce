# 未合并 PR 集成版本（2026-10-30）

本目录用于编译、验证和插件联调；功能修复仍回到对应 PR 的独立 worktree。

- 工作目录：`D:\Code\iccce-integration`
- 分支：`integration/all-open-prs-2026-10-30`
- 上游：`InkCanvasForClass/community`，`net10`
- 基线：`db9f8e9d20d893b693e6d13a74ebfda2d8da482a`
- 旧集成快照保留在 `integration/all-open-prs-2026-10-25`。

## PR 与合入顺序

| PR | 内容 | HEAD | 功能 worktree |
| --- | --- | --- | --- |
| [#716](https://github.com/InkCanvasForClass/community/pull/716) | 工具栏手动添加与设置图标 | `a9500741bbe0f581bd691e03b6ee028d51999108` | `D:\Code\iccce-plugin-toolbar-pr` |
| [#731](https://github.com/InkCanvasForClass/community/pull/731) | 文件插图接口 | `863151073a6826bf6412e0449b2bd064b057ff92` | `D:\Code\iccce-plugin-image-fix` |
| [#737](https://github.com/InkCanvasForClass/community/pull/737) | 放映套索选择 | `244f37218d5b765577294a4d4d43ce607026aa04` | `C:\Users\PANDA-JSR\AppData\Local\Temp\opencode\iccce-playback-selection-pr` |
| [#738](https://github.com/InkCanvasForClass/community/pull/738) | 保持应用主题 | `d746a65d8f6e0324ae9928afafd2e5e9ef6470c2` | `C:\Users\PANDA-JSR\AppData\Local\Temp\opencode\iccce-theme-pr` |
| [#740](https://github.com/InkCanvasForClass/community/pull/740) | 外部批注与输入捕获 | `5aeaa47cafee7b4004540b0272509934e40de896` | `D:\Code\iccce-external-annotation-pr` |
| [#747](https://github.com/InkCanvasForClass/community/pull/747) | 白板退出恢复翻页条 | `f35b7dddb8303e1c7f35500f54bc656998c51130` | `D:\Code\iccce-navigation-fix` |

#743、#744 已在上游合并，不再单独聚合；#742 已关闭，其捕获修复包含于 #740。

## 本次冲突解决

- #737 在自己的 worktree 合并最新上游，`UpdateCurrentToolMode` 同时保留退出课件选择和上游漫游结束/接触点清理。
- #740 合并更新后的 #737。两者原先携带不同 ID 的套索前置提交，五处冲突逐项保留外部批注路由、能力接口及生命周期，同时带入上游漫游清理。
- 两个 PR 分支均已推送，无强制推送；GitHub 均为 MERGEABLE，Lint、AnyCPU、x86 检查全部通过。
- 本集成分支基于最新上游新建，以上六个 PR 依次自动合并，没有额外冲突解决代码。
- 完整启动发现 #716 原 head 在同一插件注册首个组件时立刻写标记，后续自动组件被跳过。新增独立功能 worktree 修复：Initialize 期间固定首次状态，成功完成后才写标记，日志输出实际 autoAdd 判定。已推送并合入集成版。
- 2026-10-31 用户人工发现删除自动组件后重启又加回。日志确认正常退出 UnloadAll 会无条件删除首次标记。#716 `a9500741` 进一步将布局/标记清理限于真正删除插件，删除时在注销前获取本插件组件归属，避免误清其他插件布局。已推送并合入集成版。

## 验证记录

本轮工具、源码与日志：`C:\Users\PANDA-JSR\AppData\Local\Temp\opencode\iccce-pr-validation-20261030`。

- #737：Debug x64 解决方案、Release 宿主构建通过；31 项运行时检查通过，含新增白板漫游切画笔/套索。
- #740：Debug x64 解决方案、Release 宿主构建通过；39 项运行时检查及 36 项 InputCapture 检查通过。
- 原 head 复跑：#716 33 项、#731 40 项、#738 28 项运行时检查通过；#716 Debug x64 解决方案构建通过。
- #716 修复后 Debug x64/Release 构建及原有 33 项通过。真实插件 LoadPlugin → Initialize 的多组件测试：旧 head 两项失败，修复后 9 项全部通过；故意抛出初始化异常的回滚检查 5 项通过，标记不写入且组件注册清理。
- 2026-10-31 退出/重载修复：独立 #716 Debug x64/Release 构建通过；31 项生命周期检查通过，覆盖卸载/重载/UnloadAll/再次加载/真正删除及重装，并检查保留其他插件。另三个独立进程共享隔离配置共 18 项通过；原有 33 项与失败回滚 5 项通过。新人工副本 `pr716-retest-20261031` 等待用户反馈，旧 `pr716` 保留原样。
- 集成版本本次重新通过 Debug x64/Release 构建及 31 项生命周期检查，人工 integration 副本已更新，配置保留。
- 修复后另做完整 App 启动 smoke：宿主窗口正常响应，助手实际加载成功，日志确认两种工具栏的手动组件 autoAdd=False、自动组件 autoAdd=True。最终集成版本重新通过 Debug x64/Release 构建及 9 项真实多组件加载检查。
- 最终集成副本完整启动 smoke 通过：真实 ENBX 插件与验证助手均成功加载，打开菜单桥接完成，ENBX 两类工具栏组件维持手动添加。
- #747：Debug x64 解决方案构建通过，54 项 PresentationNavigation 检查通过；新增真实 MainWindow/演示服务/白板按钮/异步展开回归 29 项通过。
- 集成版本：Debug x64 解决方案、Release 宿主构建通过，0 错误；已有区域设置/未使用字段等警告。InputCapture 36 项和 PresentationNavigation 54 项通过。
- 同一集成 DLL 顺序复跑 #716/#731/#737/#738/#740/#747 场景：33/40/31/28/39/29 项，合计 200 项全部通过。
- 当前工作区 ENBX 插件显式以 `IccCeRoot=D:/Code/iccce-integration` 构建通过；WebEasiNote 当前工作区 `pnpm build` 通过。真实 WebView2/插件入口的批注集成测试通过（笔迹、历史、冻结、矩形荧光、激光及消退），该测试先针对独立 #740 SDK 构建执行。

运行时反射工具使用真实构造/XAML/生产方法和 WPF Dispatcher，隔离配置并跳过完整 App/Window_Loaded；演示回调为测试替身。WebView2 测试驱动真实播放器，但不代表完整宿主交互或触摸/手写笔验收。

## 人工验证

独立副本：`D:\Code\iccce-pr-manual-20261030`，操作清单见该目录 `MANUAL-TEST.md`。

`integration` 副本包含本集成分支、当前 ENBX 插件工作区构建和验证助手；`pr716` 等副本仅包含对应 PR 原 head / 更新 head 与验证助手，用于区分集成结果与单 PR 结果。一次只启动一份；配置在各副本独立目录。

#716 用户已确认新版移除自动、加入手动后退出重启保持正常，结合此前截图确认浮动工具栏首次行为；已回复[维护者](https://github.com/InkCanvasForClass/community/pull/716#issuecomment-6537831879)，明确自动与人工覆盖范围。白板完整人工流程、热重载及图标最终人工验收未推断通过，其他 PR 仍等待人工结果。#738 要求迁移 net6 的事项仍单独待处理，本轮未改 PR 目标分支。

#731 用户确认独立副本中延迟后台插图后切鼠标，图片正常出现、自动进入选择并可拖动缩放；已[回复维护者](https://github.com/InkCanvasForClass/community/pull/731#issuecomment-6537866970)。用户明确限定为这组步骤，其余人工项目仍待反馈。

#731 追加人工通过：撤销/重做、实际拖动浮动工具栏后连续插图保持位置、小图不明显放大；已[补充回复](https://github.com/InkCanvasForClass/community/pull/731#issuecomment-6539784434)。竖排、白板切页/冻结、PPT、多屏/DPI 仍待人工结果。

2026-11-03 #731 用户确认白板两页图片隔离、位置/大小保持、冻结拒绝插图及解冻恢复，已[补充回复](https://github.com/InkCanvasForClass/community/pull/731#issuecomment-6600063656)。竖排、真实 PPT、多屏/DPI 仍待人工结果。

同日 #731 用户确认竖排工具栏实际拖动后连续插图，位置及按钮排列正常，图片可选择/拖动；已[补充回复](https://github.com/InkCanvasForClass/community/pull/731#issuecomment-6600440975)。剩余未确认人工项为真实 PPT、多屏/不同 DPI。
