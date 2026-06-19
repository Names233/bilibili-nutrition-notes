# bilibili-nutrition-notes

每天自动获取 B站历史记录，筛选有营养的内容，通过 BiliNote 生成笔记，上传到笔记仓库。

## 流程

```
B站历史记录 API
      ↓
筛选"营养"内容（标题+分类+标签+AI判断）
      ↓
BiliNote 本地部署，生成 Markdown 笔记
      ↓
上传到笔记仓库（NAMESPACE-NOTE）
```

## 技术方案

### 1. 获取历史记录

- API: `GET https://api.bilibili.com/x/web-interface/history/cursor`
- 参数: max, view_at, business
- 需要登录 cookie

### 2. 筛选"营养"内容

- 标题关键词
- 视频分类（如：知识、科技、教育等）
- 标签
- AI 判断内容价值（可选）

### 3. 生成笔记

- 本地部署 BiliNote: https://github.com/JefferyHcool/BiliNote
- 输入视频链接，输出 Markdown 笔记

### 4. 上传笔记

- git push 到 NAMESPACE-NOTE 仓库

## 相关项目

- Nemo2011/bilibili-api - B站 API Python 库
- JefferyHcool/BiliNote - AI 视频笔记生成
- 2977094657/BilibiliHistoryFetcher - 历史记录获取工具
