# 数学冒险岛 (Math Island)

爸爸为女儿做的小学数学学习工具。她现在四年级,数学是相对薄弱的学科,需要在家针对性补漏。

托管在 GitHub Pages,在 iPad 上访问。

---

## 项目定位:补漏工具,不是教材替代

学校已经在系统教数学。**这个项目的唯一目的**是把她在学校"没掌握好的部分"在家通过可视化、互动的方式重新讲透。

- ❌ 不追求覆盖全教材
- ❌ 不预先排满学习路径
- ❌ 不做"做完就毕业"的体系
- ✅ 围绕**真实错题**驱动内容
- ✅ 一次只补一个具体的卡点
- ✅ 长期陪她走完整个上学生涯,但每一步只做眼前最该做的

---

## 内容来源:两种触发方式

新关卡/新岛的发起,**两种路径都可以**,不必拘泥于一种:

### A. 错题驱动(被动累积)

```
爸爸拍照(微信/任意渠道)
   ↓
Claude 转写到 docs/mistake-log.md(题目 / 她的做法 / 错在哪 / 标签 / 根因猜测)
   ↓
docs/tags.md 标签按需新增,累计错题数
   ↓
某个标签 ≥ 3 条 → 提议生成新关卡或新岛
   ↓
Claude 生成 → 本地预览 → push → iPad 看到
```

### B. 主题点名(主动观察)

```
爸爸在对话里直接描述:"她最近 XX 类型题做得不好,我们做个 XX 岛吧"
   ↓
Claude 和爸爸讨论:分几关 / 难度阶梯 / 用什么生活类比
   ↓
Claude 生成 → 本地预览 → push → iPad 看到
   ↓
事后在 docs/tags.md 加一行记录(状态 ✅,来源标"爸爸点名")
```

**两种路径汇合**:她做完后,爸爸反馈("懂了"/"还卡 X")→ 决定是迭代当前岛还是去做下一个。

**节奏目标**:她每周用 2~3 次,每次 15~25 分钟。Claude 每周生产 1~2 关足够。

详见:
- [docs/student-profile.md](docs/student-profile.md) — 女儿的学情档案
- [docs/mistake-log.md](docs/mistake-log.md) — 错题池
- [docs/tags.md](docs/tags.md) — 标签表
- [docs/level-template.md](docs/level-template.md) — 关卡生成规范

---

## 教学原则(写每一关都要遵守)

1. **永远不要直接给最终答案**。用引导式提问把孩子带到答案那里
2. **多用生活类比**:披萨、糖果、积木、零花钱、家人(姐姐妹妹妈妈爸爸)
3. **语言简单**:避免「因此」「即」「显然」「也即」等书面词
4. **一次只讲一个核心概念**,不要塞太多
5. **鼓励要具体**:不说「真棒」,说「你刚才注意到 X,这一步特别关键!」
6. **每关结尾必须有 2~3 道变式题**——做对才算真懂、才解锁通关
7. **错了不要直接说「错了」**,说「这是个好想法,我们再看看…」
8. **难度渐进**:同一岛内,Lv1 用最简单场景/最基础概念,逐级加深(像现有「追及岛」那样)

---

## 项目结构

```
math-island/
├── index.html                  ← 总目录(所有岛屿入口)
├── CLAUDE.md                   ← 本文件
├── README.md
├── .gitignore
│
├── docs/                       ← 项目文档(给 Claude 和爸爸看)
│   ├── student-profile.md      ← 学情档案
│   ├── mistake-log.md          ← 错题池
│   ├── tags.md                 ← 标签表
│   └── level-template.md       ← 关卡生成规范
│
├── catch-up-problems/          ← 追及问题专题(已上线)
│   ├── index.html
│   ├── level-1.html
│   ├── level-2.html
│   └── level-3.html
│
├── [其他专题文件夹/]            ← 见下方命名规范
└── ...
```

### 命名规范(重要)

- **文件夹和文件名一律用英文**,不用拼音
  - ✅ `fractions/`, `geometry/`, `decimals/`, `word-problems/`
  - ❌ `fenshu/`, `jihe/`, `xiaoshu/`
- **localStorage key 前缀也用英文**
  - ✅ `catch-up-lv1-done`, `fractions-lv1-done`
  - ❌ `zhuiji-lv1-done`
- **页面内容(标题、按钮文案、讲解)** 仍然用中文 — 只有路径和标识符是英文

### 文件命名

- 岛屿目录:`<topic>/index.html` 当目录页
- 关卡:`level-1.html`、`level-2.html` ...

---

## 技术要求

每个 HTML 文件必须:

- **单文件、自包含**:CSS 和 JS 内联,不依赖外部 CDN(以防离线/网络差)
- **iPad Safari 兼容**:避免冷门 API,验证 -webkit- 前缀
- **viewport meta**:`<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">`
- **字体大**:正文 ≥ 15px,关键内容 18px+
- **触摸友好**:按钮 ≥ 44px 高度,滑块 thumb ≥ 28px
- **相对路径**:链接其他页用相对路径,不要绝对路径或写死 URL
- **不用 localStorage 之外的存储**(GitHub Pages 静态站,没后端)

## 视觉规范(沿用现有专题的设计语言)

色板:

- 主色:`#FF6B6B` (珊瑚粉,标题/按钮)
- 辅红:`#FFB7B2` (浅粉,渐变)
- 强调绿:`#B5EAD7` (薄荷,完成/正确)
- 蓝灰:`#7AB8FF` (引导/提示)
- 暖底:`#FFF5E1` → `#FFE5E1` (页面渐变)
- 文字:`#5D4954` (深暖灰)

字体:`-apple-system, "PingFang SC", "Hiragino Sans GB", "Helvetica Neue", sans-serif`

组件:

- 卡片:`background: white; border-radius: 18px; box-shadow: 0 4px 14px rgba(255,154,162,0.15);`
- 按钮:`border-radius: 12px; font-weight: 700; padding: 13px 16px;`
- 输入框:`border: 2px solid #DDD; border-radius: 8px;`
- 滑块:`#FF6B6B` thumb,`#FFD3D3` track

---

## 关卡结构(简版,详见 [docs/level-template.md](docs/level-template.md))

每个 `level-N.html` 包含 8 个区块:

1. **顶部导航**:`← 回到目录` + 关卡徽章(LV N)
2. **标题区**:关卡名 + 副标题
3. **故事卡片**:把题目翻译成生活场景
4. **引导思考**:`🤔 先想一想:...`
5. **互动可视化**:滑块 / 动画 / 拖拽(必须服务教学,不是装饰)
6. **关键洞察揭示**:在她探索之后,而不是上来就给
7. **完整解题步骤** + **公式总结**(用 .formula-box 突出)
8. **变式题(2~3 道)** + **完成庆祝卡片**(`localStorage.setItem('xxx-lvN-done', '1')`)

## 进度追踪规范

每个专题用统一的 key 前缀(英文):

- 追及问题:`catch-up-lv1-done`、`catch-up-lv2-done` ...
- 分数运算(假设):`fractions-lv1-done` ...

值统一用 `'1'` 表示通关。

顶层 [index.html](index.html) 通过遍历这些 key 计算总进度。**新增专题时,记得在 `ISLANDS` 数组里加一项**(folder / keyPrefix / totalLevels / availableLevels)。

---

## 工作流:增加一个新专题

触发条件(任一即可):
- (A) 某个标签在 [docs/tags.md](docs/tags.md) 累积 ≥ 3 条错题
- (B) 爸爸在对话中直接点名某类题目较弱,描述清楚后开始讨论

1. **先确认设计**(对话讨论):
   - 这个主题分几关?难度怎么阶梯化?
   - 每一关的核心概念是什么?
   - 用什么生活类比?
2. **建文件夹**:`mkdir <english-name>/`(英文,不用拼音)
3. **生成文件**:`index.html` + `level-1.html` ... 按 [docs/level-template.md](docs/level-template.md)
4. **更新顶层** [index.html](index.html):在 `ISLANDS` 数组加一项
5. **本地预览**:`python3 -m http.server 8000`,浏览器打开 `localhost:8000` 检查
6. **提交**:`git add . && git commit -m "🏝️ 新增 X 岛" && git push`
7. **30 秒后** GitHub Pages 自动部署,iPad 刷新即可看到

## 工作流:修改/优化已有关卡

1. 直接在对应 `level-N.html` 里改
2. 本地浏览器预览验证
3. `git commit` + `git push`

## 工作流:转写一张错题照片

爸爸发来照片后:

1. 在 [docs/mistake-log.md](docs/mistake-log.md) **追加**(不要重写整个文件)一条新条目,按文件里的格式
2. 给这道题打标签;如果是新标签,同时在 [docs/tags.md](docs/tags.md) 加一行
3. 如果某标签累计 ≥ 3 条,主动提议:"`tag-X` 已经积累了 N 条错题,要不要做一个专题?"
4. 转写时如果发现根因可能是另一个更基础的概念(比如分数加减错根源是约分),写在 `根因猜测` 字段——这是关键的增量价值

---

## 当前进度(更新内容时同步)

### 已上线

- **追及岛** ([catch-up-problems/](catch-up-problems/))
  - Lv1 经典追及 ✓
  - Lv2 时间差追及 ✓
  - Lv3 反过来求 ✓

### 待开发

由 [docs/mistake-log.md](docs/mistake-log.md) 和 [docs/tags.md](docs/tags.md) 共同决定。**不预先排路线图**——等真实错题积累后再决定下一个做哪个。

### 当前学习上下文

- 年级:四年级
- 教材版本:(待补充)
- 学习状态、卡点、偏好详见 [docs/student-profile.md](docs/student-profile.md)

---

## 重要的「不要」清单

- ❌ 不要在讲解里直接说「答案是 X」
- ❌ 不要用太抽象的数学术语(四年级听不懂)
- ❌ 不要做无用的炫技动画(动画必须服务教学)
- ❌ 不要让单个页面长到要滚动 5 屏以上
- ❌ 不要用绝对路径(GitHub Pages 部署后路径会变)
- ❌ 不要引入复杂依赖(保持纯 HTML/CSS/JS)
- ❌ 不要用粗暴的「错了!」反馈,要温和
- ❌ 不要用拼音命名文件/文件夹
- ❌ 不要主动建"全教材覆盖"的知识图谱——这是补漏工具,不是教材
- ❌ 不要预先生成"未来要做的岛"的占位——等真实错题驱动

## 常用命令

```bash
# 本地预览(在 repo 根目录跑)
python3 -m http.server 8000

# 提交并推送
git add . && git commit -m "..." && git push

# 看 GitHub Pages 部署状态
gh run list  # 如果装了 gh CLI
```
