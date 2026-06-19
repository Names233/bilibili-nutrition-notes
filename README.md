# bilibili-nutrition-notes

每天自动获取 B站历史记录，筛选有营养的内容，通过 BiliNote 生成笔记，上传到笔记仓库。

## 流程

```
B站历史记录 API
      ↓
关键词粗筛（标题+标签）
      ↓
AI 精筛（DeepSeek，批量调用）
      ↓
数据指标排序（收藏比、投币比、时长）
      ↓
BiliNote 本地部署，生成 Markdown 笔记
      ↓
上传到笔记仓库（NAMESPACE-NOTE）
```

## 筛选逻辑

### 第一层：关键词粗筛

正向关键词（命中任一进入下一层）：
- 教程、原理、解析、深入、干货、科普、学习、入门、进阶
- 讲、为什么、怎么做、是什么、如何、区别、对比
- 实战、项目、案例、手把手

负向关键词（命中直接排除）：
- 挑战、合集、名场面、高能、笑、哭、翻车
- vlog、日常、开箱、吃播、探店

### 第二层：AI 精筛

- 模型：DeepSeek（成本低）
- 批量处理：一次 10-20 个视频，减少 API 调用
- 输入：标题 + 简介 + 标签
- 输出：1-10 分，>= 7 通过
- 判断标准：是否有学习/思考价值

### 第三层：数据指标排序

通过筛选后，按以下指标排序决定优先级：
- 收藏/播放比
- 投币/播放比
- 时长权重

## 技术方案

### 1. 登录与 Cookie

- 扫码登录获取 cookie
- cookie 本地存储，过期后重新扫码

### 2. 获取历史记录

- API: `GET https://api.bilibili.com/x/web-interface/history/cursor`
- 参数: max, view_at, business
- 已处理记录：本地记录已生成笔记的视频 ID，避免重复

### 3. 生成笔记

- 本地部署 BiliNote: https://github.com/JefferyHcool/BiliNote
- 输入视频链接，输出 Markdown 笔记

### 4. 上传笔记

- 目录结构：`B站视频/YYYYMMDD/分类/标题.md`
- 笔记元数据（YAML front matter）：
  - 原视频链接
  - UP 主
  - 发布日期
  - 观看日期
  - 分区/分类
  - 标签
- git push 到 NAMESPACE-NOTE 仓库

### 5. 定时任务

- 每天自动执行
- 不限数量，处理所有符合条件的视频

## 相关项目

- Nemo2011/bilibili-api - B站 API Python 库
- JefferyHcool/BiliNote - AI 视频笔记生成
- 2977094657/BilibiliHistoryFetcher - 历史记录获取工具
