# OpenCurVe Resume

[![License: MIT](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
[![LaTeX Template](https://img.shields.io/badge/LaTeX-template-008080?logo=latex&logoColor=white)](resume.tex)
[![Compiler](https://img.shields.io/badge/compiler-XeLaTeX%20%7C%20LuaLaTeX-2F6F9F)](#环境要求)
[![CurVe Based](https://img.shields.io/badge/CurVe-based-4F94C4)](https://ctan.org/pkg/curve)
[![One Page](https://img.shields.io/badge/resume-one--page-173A86)](#效果预览)
[![AI Editable](https://img.shields.io/badge/AI-editable-7C3AED)](#为什么使用-latex)

制作简历有好的想法，但是在Word难以体现出来？排版总是有莫名的缩进，字体总是对不齐？想使用AI辅助但是修改总是有偏差？Html可以生成漂亮的简历，但是人工调整却难以下手？

<p align="center"><strong>OPENCURVE RESUME IS ALL YOU NEED !</strong></p>

> OpenCurVe Resume是一个基于 [CurVe](https://ctan.org/pkg/curve) 的可高度定制单页 LaTeX 简历模板。

## 效果预览

<p align="center">
  <img src="./assets/resume-preview.png" alt="OpenCurVe Resume 示例预览" width="820">
</p>

## 为什么使用 LaTeX

当前市面上简历形式主要是**Word**，人工编辑直观，但是复杂排版对操作者要求较高，同时AI难以精确修改内容；一部分是**HTML/CSS 转 PDF**（可以参考[VibeResume](https://github.com/LiuMengxuan04/vibe-resume)，本项目开源想法也来自于此），适合AI生成和修改，但人工维护困难。

**LaTeX** 可以让AI精确修改内容，人工编辑也仅需少量命令，且排版结果稳定。


## 环境要求

推荐使用完整安装的 TeX Live 或 Overleaf，并选择以下编译器之一：

- XeLaTeX；
- LuaLaTeX。

模板使用的主要 LaTeX 包包括：

- `curve`、`ctex`、`fontspec`；
- `fontawesome5`、`simpleicons`；
- `graphicx`、`xcolor`、`hyperref`；
- `tcolorbox` 及其 `skins` 库。

`tcolorbox` 会间接加载 PGF/TikZ，用于公司 Banner 的圆角、裁切和 Logo 独立居中。章节标题本身使用原生 TeX 绘制。

## 项目结构

```text
OpenCurVe Resume/
├── README.md
├── LICENSE
├── .gitignore
├── resume.tex                 # 编译入口与页眉信息
├── open-curve.sty             # 全局字体、颜色、间距和通用组件
├── resume.pdf                 # 编译后的示例
├── assets/
│   ├── bytedance-color.png    # 图片 Logo 示例
│   └── resume-preview.png     # README 效果预览
└── sections/
    ├── education.tex          # 教育背景
    ├── experience.tex         # 实习经历与公司Banner
    ├── projects.tex           # 项目经历
    └── skills.tex             # 个人技能
```

## 使用方法

> [!TIP]
> 如果你懒得自己修改可以全程与AI进行交互，以下内容可以跳过

### 1. 修改个人信息

在 `resume.tex` 的 `\leftheader` 中替换姓名、电话、邮箱、出生年月、GitHub 和额外说明：

```latex
\leftheader{%
  {\LARGE\sffamily\bfseries 你的姓名}\par\smallskip
  \normalsize
  % 电话、邮箱、出生年月、GitHub 和额外说明
}
```

当前页眉左右比例为 `74/26`，定义在 `open-curve.sty`：

```latex
\headerscale{0.74}
```

### 2. 修改正文

直接编辑 `sections/` 中对应文件：

- 教育经历：`sections/education.tex`；
- 实习经历：`sections/experience.tex`；
- 项目经历：`sections/projects.tex`；
- 个人技能：`sections/skills.tex`。

模块顺序由 `resume.tex` 决定，可以删除、增加或重新排序：

```latex
\input{sections/education}
\input{sections/experience}
\input{sections/projects}
\input{sections/skills}
```

### 3. 编译

进入项目目录后运行：

```bash
latexmk -xelatex resume.tex
```

也可以使用 LuaLaTeX：

```bash
latexmk -lualatex resume.tex
```

清理辅助文件：

```bash
latexmk -c resume.tex
```

在 Overleaf 中上传整个目录，将 Compiler 设置为 **XeLaTeX** 或 **LuaLaTeX**，主文件选择 `resume.tex`。

## 定制页眉图片

所有图片建议放入 `assets/`。PNG、JPG 和 PDF 可以直接使用；SVG 建议先转换为 PDF。

修改 `resume.tex` 中完整的 `\rightheader` 区块即可。

### 单个校徽

```latex
\rightheader{%
  \centering
  \includegraphics[width=0.72\linewidth]{assets/school-logo.pdf}%
}
```

### 两个校徽

```latex
\rightheader{%
  \includegraphics[width=0.48\linewidth]{assets/school-a.pdf}\hspace{0.8mm}%
  \includegraphics[width=0.48\linewidth]{assets/school-b.pdf}%
}
```

如果图片宽度不同，可以分别调整两个 `width` 数值。

### 头像

```latex
\rightheader{%
  \centering
  \includegraphics[
    width=2.5cm,
    height=2.5cm,
    keepaspectratio
  ]{assets/avatar.jpg}%
}
```


## 定制全局颜色

全局颜色位于 `open-curve.sty`：

```latex
\definecolor{OpenCurvePrimary}{HTML}{2F6F9F}
\definecolor{OpenCurveAccent}{HTML}{4F94C4}
\definecolor{OpenCurveHeading}{HTML}{173A86}
\definecolor{OpenCurveMuted}{HTML}{4B5563}
```

| 颜色 | 默认用途 |
| --- | --- |
| `OpenCurvePrimary` | “教育背景”等章节标题 |
| `OpenCurveAccent` | 章节横线、页眉占位框边框 |
| `OpenCurveHeading` | 项目名称、公司项目名称等二级标题 |
| `OpenCurveMuted` | 日期、职位、辅助说明 |

将 HTML 色值替换即可。例如：

```latex
\definecolor{OpenCurvePrimary}{HTML}{1F4E79}
```

链接颜色在 `open-curve.sty` 的 `hyperref` 配置中，默认全部为黑色：

```latex
\RequirePackage[colorlinks=true,allcolors=black,breaklinks=true]{hyperref}
```

## 定制公司 Banner

公司专属配置没有放入全局样式，而是集中在 `sections/experience.tex`，便于直接复制、删除或替换。

### 公司颜色

```latex
\definecolor{ByteDanceBlue}{HTML}{3C8CFF}
\definecolor{ByteDanceTint}{HTML}{EEF5FF}
\definecolor{HuaweiRed}{HTML}{CF0A2C}
\definecolor{HuaweiTint}{HTML}{FFF1F3}
```

每家公司通常需要：

- 一个品牌主色，用于左侧边条、Logo 和 Bullet；
- 一个浅色背景，用于 Banner 背景。

### Banner 参数

```latex
\companybanner[上方间距]
  {背景色}
  {品牌色}
  {Logo}
  {公司及部门名称}
  {职位与日期}
```

例如：

```latex
\companybanner[0.8em]{HuaweiTint}{HuaweiRed}{Logo 内容}{%
  华为技术有限公司\,·\,研发中心
}{软件工程师实习生 \headerdivider 2024.07--2024.10}
```

Banner 的固定高度、圆角、左右内边距和品牌色左边条都定义在同一个文件的 `\companybanner` 中：

```latex
arc=1.5mm,
height=22pt,
left=6pt,
right=6pt
```

左边条宽度在 overlay 中设置：

```latex
([xshift=3pt]frame.north west)
```

### 使用图片 Logo

字节跳动示例从 `assets/` 加载透明 PNG：

```latex
\includegraphics[height=20pt]{assets/bytedance-color.png}
```

替换图片文件或调整 `height` 即可。图片由独立 overlay 节点居中，不需要额外使用 `\raisebox`。

### 使用 Simple Icons

华为示例使用 `simpleicons`：

```latex
\textcolor{HuaweiRed}{%
  \fontsize{18}{18}\selectfont\simpleicon{huawei}%
}
```

更换图标时，只需将 `huawei` 换成 Simple Icons 支持的名称，并调整颜色或字号。例如：

```latex
\simpleicon{github}
```

因此同一个 `\companybanner` 同时支持本地图片、Simple Icons，也可以传入普通文字或其他 LaTeX 图标。

## 定制 Bullet

### 通用 Bullet

`open-curve.sty` 中的 `\cvitem` 默认使用黑色 Bullet：

```latex
\cvitem{普通内容}
```

通过可选参数指定颜色：

```latex
\cvitem[OpenCurveAccent]{彩色 Bullet 内容}
```

其续行缩进按照“Bullet＋一个空格”的实际宽度自动计算。

### 主要贡献 Bullet

实习模块中的“项目背景”“主要贡献”“项目成果”标签不带 Bullet；只有“主要贡献”下面的具体分点使用 `\companyitem`：

```latex
\companyitem{ByteDanceBlue}{字节跳动项目的具体贡献。}
\companyitem{HuaweiRed}{华为项目的具体贡献。}
```

`\companyitem` 位于 `sections/experience.tex`，当前首行缩进为 `0.65em`，续行悬挂缩进为 `1.6em`：

```latex
\hspace{0.65em}
\hangindent=1.6em
```

## 定制字体

字体配置位于 `open-curve.sty`。

| 类型 | 首选字体 | 回退字体 |
| --- | --- | --- |
| 英文正文及无衬线文字 | Times New Roman | TeX Gyre Termes |
| 中文正文及无衬线文字 | Microsoft YaHei | FandolHei |
| 英文等宽字体 | TeX Gyre Cursor | — |

首选字体存在时自动使用，不存在时回退：

```latex
\IfFontExistsTF{Times New Roman}{
  \setmainfont{Times New Roman}
  \setsansfont{Times New Roman}
}{
  \setmainfont{TeX Gyre Termes}
  \setsansfont{TeX Gyre Termes}
}
```

更换中文字体时，同步修改以下命令：

```latex
\setCJKmainfont{你的中文字体}
\setCJKsansfont{你的中文字体}
\setCJKmonofont{你的中文字体}
```

建议保留 `\IfFontExistsTF` 回退逻辑，以便在本机、GitHub Actions 和 Overleaf 上获得更稳定的编译结果。

## 定制版式和间距

### 页边距

位于 `open-curve.sty`：

```latex
\RequirePackage[
  a4paper,
  left=1.2cm,
  right=1.2cm,
  top=1cm,
  bottom=1cm
]{geometry}
```

### 全局行距

```latex
\linespread{1.3}\selectfont
```

### 章节标题前间距

每个章节可单独设置：

```latex
\cvsection[0.8]{教育背景}
\cvsection[0.45]{实习经历}
```

可选参数以当前 `\baselineskip` 为单位。表格等前置内容会影响视觉间距，因此不同章节可以使用不同数值。

### 章节标题和横线

`\cvsection` 位于 `open-curve.sty`，当前设置包括：

- 标题：`15pt / 16.8pt`；
- 标题到横线：`3.5pt`；
- 横线粗细：`1.5pt`。

### 项目标题间距

`\cvheading` 的可选参数控制标题上方间距：

```latex
\cvheading{第一个项目}{技术栈}
\cvheading[0.8em]{后续项目}{技术栈}
```

## 友链

[LINUX DO](https://linux.do/)

## 许可与致谢

OpenCurVe Resume 的原创代码和文档采用 [MIT License](./LICENSE)。

本项目使用 Didier Verna 的 CurVe 文档类，并基于 LianTze Lim 的 [A Customised CurVe CV](https://www.overleaf.com/latex/templates/a-customised-curve-cv/mvmbhkwsnmwv) 示例进行重构。
