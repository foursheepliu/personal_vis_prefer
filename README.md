# 我的界面偏好与修改记录

记录软件界面怎么改、为什么改，以及哪些方案已经被否定。下一次在其他软件里做相似的功能时，直接带上这些记录，减少重复解释。

这里的“复用”主要是复用交互和视觉原则，不是把某个项目的代码直接搬到另一个项目。

## 从哪里开始

- 看当前偏好：[preferences/interface.json](preferences/interface.json)。只收录对话中明确提出的要求，并标明适用范围。
- 做媒体卡片：[patterns/media-card.md](patterns/media-card.md)。包含描述区、播放器、快捷键和缩放柄的要求。
- 看具体项目：[FlowTracker](projects/flowtracker/README.md)。从项目目录进入修改记录、截图和实现说明。
- 新增修改：[修改记录模板](templates/change-record.md)；新增软件：[项目模板](templates/project.md)。

## 在别的软件里怎么用

可以把下面这段话连同仓库链接交给协作的人或 AI：

> 请先阅读这个仓库的 README、preferences/interface.json，以及与当前功能相关的 patterns。再看目标项目的记录。优先参考我明确提出的要求；已经否定的版本不要重新套用。根据当前软件调整实现，不要照搬其他技术栈的代码。修改后补充新版本、对比截图和验证结果。

第一次接入一个软件时，在 `projects/<项目名>/` 下建立文件夹。先写清项目地址、功能范围和当前版本，再记录具体变化。

## 一次修改怎么记录

1. 写下用户的原话或明确要求，区分“用户要求”和“实现时自行选择的细节”。
2. 保存修改前的截图；公开仓库中使用独立样例或不含私人内容的截图。
3. 每个有明显变化的中间版都留一条记录，写清被保留或被否定的原因。
4. 保存当前版截图、行为说明和验证结果。实现完成不等于用户已经认可。
5. 只把用户明确确认的偏好提炼到 `preferences/` 或 `patterns/`，不要把临时方案升级成通用规则。
6. 用 Git 提交本次变化。旧截图不覆盖，旧决定不删除；用新记录说明替代关系。

## 目录约定

```text
preferences/                 有证据的当前偏好，适合其他工具读取
patterns/                    可跨软件复用的交互、视觉说明
projects/<project>/README.md 项目入口和当前状态
projects/<project>/history/  按时间保留修改过程
projects/<project>/decisions/当前决定、原因与替代关系
projects/<project>/assets/   按功能保存各版截图
projects/<project>/implementation/ 与具体技术栈有关的实现说明
scripts/                    可选的采集或检查工具
templates/                  新项目和新修改的模板
```

## 当前收录

| 项目 | 功能 | 当前进度 |
| --- | --- | --- |
| [FlowTracker](projects/flowtracker/README.md) | 视频卡片、描述、播放器、缩放柄 | 已实现本轮要求并验证；等待用户继续使用后的反馈 |

截图是证据的一部分。FlowTracker 历史截图来自保留的本地构建，统一使用独立生成的视频样例重新采集；它们展示各版 UI，不是用户研究项目的原始截图，也不证明历史版本的媒体读取已经正常。

## 可选：重新采集 FlowTracker 的对比图

仓库附带 `scripts/capture-flowtracker.mjs`。需要本地 Node.js、Electron 和各版应用的 `dist` 目录；这些构建与运行时不放进偏好仓库。

在本机建立一个不提交的 `local-stages.json`：

```json
[
  {"id": "06-selected-space", "build": "/path/to/FlowTracker/dist"}
]
```

执行：

```sh
node scripts/capture-flowtracker.mjs \
  --electron /path/to/Electron \
  --stages local-stages.json \
  --out projects/flowtracker/assets/video-player
```

采集器启动隔离工作区，生成中性的视频样例，不读取真实笔记；截图只截播放器卡片。每版使用新 id，并同步更新截图 manifest 和历史说明。旧构建缺失时明确标注，不用推测出来的示意图冒充当时界面。
