# 个人网站仓库上手指南

这个仓库会自动生成并发布 <https://felixzhangmingzhe.github.io/>。网页使用 [Academic Pages](https://github.com/academicpages/academicpages.github.io) 模板；绝大多数文件负责主题和构建，日常更新通常只需要改少数几个 Markdown/YAML 文件。

## 先认识最常用的文件

| 想修改的内容 | 文件或目录 | 说明 |
| --- | --- | --- |
| 首页正文 | _pages/about.md | 这是网站首页；不是根目录的 README.md |
| 左侧姓名、简介、地点、单位、社交链接 | _config.yml | 修改后本地预览时要重启 Jekyll |
| 顶部菜单 | _data/navigation.yml | 调整标题、顺序或链接 |
| 简历页 | _pages/cv.md | 当前顶栏中的 CV 页面 |
| 个人兴趣页 | _pages/interests.md | 当前放置围棋等非学术内容 |
| 头像和网页图片 | images/ | 当前头像文件是 images/profile.png |
| 可下载文件 | files/ | 例如 CV PDF、论文或 slides |
| 论文 | _publications/ | 每篇论文一个 Markdown 文件 |
| 报告与讲座 | _talks/ | 每次报告一个 Markdown 文件 |
| 教学经历 | _teaching/ | 每项教学经历一个 Markdown 文件 |
| 项目 | _portfolio/ | 每个项目一个 Markdown/HTML 文件 |
| 博客 | _posts/ | 文件名通常为 YYYY-MM-DD-title.md |
| 数据版 CV | _data/cv.json | 供 /cv-json/ 页面读取；当前没有放在顶栏 |

_includes/、_layouts/、_sass/、assets/ 主要控制页面结构、样式和脚本。除非要改版式，一般不要动；这样以后同步上游时冲突更少。

## 最简单的日常修改

### 修改首页介绍

打开 _pages/about.md。最上面的两条三横线之间是页面设置，下面是普通 Markdown。修改正文后提交到 master，GitHub Pages 会自动重新构建。

### 修改侧栏信息

打开 _config.yml，在 author: 下修改 bio、location、employer、email 和 personal_email 等字段。留空的社交账号不会显示。Bluesky、ORCID 和 PubMed 字段目前已注释，因此侧栏不会出现这些链接。

Google Scholar 目前通过 googlescholar_placeholder: true 显示为“coming soon”。创建主页后，把主页地址填入 googlescholar 字段即可自动变成可点击链接。

### 更换头像

用新的正方形图片替换 images/profile.png，尽量保持文件名不变。建议使用清晰、裁剪到头肩范围的 JPG 或 PNG。

### 新增一个菜单页面

1. 在 _pages/ 新建 Markdown 文件，例如 _pages/research.md。
2. 在文件最上方加入以下页面设置：

       ---
       permalink: /research/
       title: "Research"
       author_profile: true
       ---

3. 在 _data/navigation.yml 的 main: 下加入：

       - title: "Research"
         url: /research/

### 新增论文、报告或项目

对应目录里保留了 Academic Pages 的示例文件，但这些示例已设置为 published: false，不会出现在网站上。复制最接近的示例，改成自己的内容、修改文件名，并删除复制文件中的 published: false 即可发布。

## 在 WSL 中本地预览

在仓库根目录运行：

    bundle config set --local path 'vendor/bundle'
    bundle install
    bundle exec jekyll serve -l -H 0.0.0.0

然后在 Windows 浏览器打开 <http://localhost:4000>。如果改了 _config.yml，请按 Ctrl+C 停止服务后重新运行最后一条命令。

提交前建议至少运行：

    git status
    git diff --check
    bundle exec jekyll build

## 从 Academic Pages 同步更新

第一次设置上游仓库：

    git remote add upstream https://github.com/academicpages/academicpages.github.io.git

以后同步：

    git checkout master
    git pull origin master
    git fetch upstream
    git merge upstream/master
    bundle exec jekyll build
    git push origin master

最常见的冲突文件是 _config.yml：通常应同时保留上游新增的设置和自己的姓名、网址、author 信息。合并前先提交自己的工作，发生冲突后用 git status 查看文件；不要直接丢弃整个个人版本或整个上游版本。

## 推荐的安全工作方式

- 每次只做一类修改，并写清楚 commit message；出错时更容易回退。
- 推送前先预览首页、CV、移动端菜单和所有新增链接。
- 这个仓库是公开的。不要提交密码、恢复码、学生证号、住址、未公开研究数据或任何不希望被搜索引擎收录的内容。
- 如果某个内容还没准备好公开，可在页面的 YAML 区域加入 published: false。
- 上游同步主要更新主题代码；个人内容尽量集中在 _config.yml、_data/navigation.yml、_pages/ 和内容目录中。
