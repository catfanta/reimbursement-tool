# 报销 PDF 合并打印

## Web 版（GitHub Pages）
- `index.html` — 纯前端 PDF 合并工具，拖拽上传 → 预览 → 下载
- `lib/pdf-lib.min.js` — 本地 vendor，无外部 CDN 依赖
- `lib/pdf.min.js` + `lib/pdf.worker.min.js` + `lib/cmaps/` — pdf.js 3.11.174 本地 vendor，自动裁剪时渲染首页检测内容边界
- 直接打开 `index.html` 或部署到 GitHub Pages
- 功能：上传、配置布局、自动裁剪、合并、预览、下载

## 自动裁剪空白边缘
- 合并前用 pdf.js 以 0.5 倍渲染首页，扫描非空白像素得到内容边界，`embedPage` 按边界裁剪嵌入
- 零散内容剔除：与主内容间隔 ≥30pt 且墨迹 ≤10%、尺寸 ≤25% 的孤立块（页脚/页码）不参与边界
- 检测失败或整页空白时自动回退整页；界面提供开关（默认开启）
- file:// 直接打开时无法创建 Worker，改以 `<script>` 加载 worker 走主线程（`ensurePdfjsWorker`）
