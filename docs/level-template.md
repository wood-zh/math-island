# 关卡生成规范

> 给 Claude 看的**实操模板**。生成新关卡(`level-N.html`)时严格按这个走,沉淀自现有 [catch-up-problems/level-1.html](../catch-up-problems/level-1.html)。
>
> 视觉色板、字体、教学原则在 [CLAUDE.md](../CLAUDE.md) 里,这里只讲**结构**和**HTML 骨架**。

---

## 渐进难度原则

同一岛内的关卡按难度阶梯排列:

- **Lv1**:最简单/最具体的场景,核心概念裸出来
- **Lv2**:加一个变量(比如多一步运算 / 多一个未知量 / 反过来求)
- **Lv3+**:更复杂的场景或综合应用

参考「追及岛」:Lv1 经典追及 → Lv2 时间差(多一步算距离差)→ Lv3 反过来求(同一公式倒着用)。

---

## 一关的 8 个区块

1. **顶部导航**:`← 回到目录` 链接 + `LV N` 徽章
2. **标题区**:关卡名(如「经典追及」)+ 副标题(一句话说核心)
3. **故事卡片**:把题目翻译成生活场景(姐姐妹妹买冰激凌、跑步、零花钱…)
4. **引导思考**:`🤔 先想一想:...`(让她在心里先猜一个数 / 一个方向)
5. **互动可视化**:滑块 / 动画 / 拖拽 / 进度条(必须服务教学,不是装饰)
6. **关键洞察揭示**:在她探索之后才揭示规律,**不要上来就给公式**
7. **完整解题步骤** + **公式总结**(用 `.formula-box`)
8. **变式题(2~3 道)** + **完成庆祝卡片**(`localStorage.setItem('<island>-lvN-done', '1')`)

---

## HTML 骨架

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>{关卡名} 🏝️</title>
<style>
  /* === 公共重置 === */
  * { box-sizing: border-box; -webkit-tap-highlight-color: transparent; }
  body {
    font-family: -apple-system, "PingFang SC", "Hiragino Sans GB", "Helvetica Neue", sans-serif;
    background: linear-gradient(180deg, #FFF5E1 0%, #FFE5E1 100%);
    color: #5D4954;
    margin: 0; padding: 16px;
    line-height: 1.6; font-size: 18px;
    min-height: 100vh;
  }
  .container { max-width: 720px; margin: 0 auto; }

  /* === 1. 顶部导航 === */
  .nav { display: flex; align-items: center; justify-content: space-between; padding: 8px 0 16px; }
  .nav a { color: #FF6B6B; text-decoration: none; font-weight: 600; }
  .nav .badge { background: #FF6B6B; color: white; padding: 4px 12px; border-radius: 12px; font-size: 14px; font-weight: 700; }

  /* === 2. 标题区 === */
  .header { text-align: center; padding: 16px 0; }
  .header h1 { color: #FF6B6B; font-size: 28px; margin: 0 0 6px; }
  .header .subtitle { color: #888; font-size: 15px; margin: 0; }

  /* === 通用卡片 === */
  .card {
    background: white;
    border-radius: 18px;
    padding: 18px 20px;
    margin: 14px 0;
    box-shadow: 0 4px 14px rgba(255,154,162,0.15);
  }

  /* === 4. 引导思考 === */
  .think-card { background: #E1F0FF; border-left: 4px solid #7AB8FF; }

  /* === 6. 关键洞察 === */
  .insight-card { background: #FFF9E1; border-left: 4px solid #FFC857; }

  /* === 7. 公式 === */
  .formula-box {
    background: linear-gradient(135deg, #FFB7B2, #FF6B6B);
    color: white;
    padding: 16px 20px;
    border-radius: 14px;
    text-align: center;
    font-size: 20px;
    font-weight: 700;
    margin: 16px 0;
  }

  /* === 按钮 === */
  .btn {
    background: #FF6B6B; color: white; border: none;
    border-radius: 12px; font-weight: 700; padding: 13px 16px;
    font-size: 16px; cursor: pointer; font-family: inherit;
    min-height: 44px;
  }
  .btn:active { transform: scale(0.97); }
  .btn-secondary { background: #FFB7B2; }

  /* === 输入框 === */
  input[type=number], input[type=text] {
    border: 2px solid #DDD; border-radius: 8px;
    padding: 8px 12px; font-size: 18px; font-family: inherit;
    width: 80px;
  }
  input:focus { outline: none; border-color: #FF6B6B; }

  /* === 滑块(若用) === */
  input[type=range] { -webkit-appearance: none; width: 100%; height: 8px; background: #FFD3D3; border-radius: 4px; }
  input[type=range]::-webkit-slider-thumb {
    -webkit-appearance: none; width: 28px; height: 28px;
    border-radius: 50%; background: #FF6B6B; cursor: pointer;
    box-shadow: 0 2px 6px rgba(255,107,107,0.4);
  }

  /* === 8. 完成庆祝 === */
  .complete-card {
    background: linear-gradient(135deg, #B5EAD7, #4CC76C);
    color: white;
    text-align: center;
    padding: 24px;
    border-radius: 18px;
    margin: 20px 0;
    display: none;  /* 默认隐藏,通关后 JS 显示 */
  }
  .complete-card h2 { margin: 0 0 8px; font-size: 24px; }
</style>
</head>
<body>
<div class="container">

  <!-- 1. 顶部导航 -->
  <div class="nav">
    <a href="index.html">← 回到目录</a>
    <span class="badge">LV {N}</span>
  </div>

  <!-- 2. 标题区 -->
  <div class="header">
    <h1>{关卡名}</h1>
    <p class="subtitle">{一句话说明这关核心}</p>
  </div>

  <!-- 3. 故事卡片 -->
  <div class="card">
    <p>{用生活场景把题目讲一遍。妹妹/姐姐/爸爸/妈妈 + 冰激凌/玩具/跑步...}</p>
  </div>

  <!-- 4. 引导思考 -->
  <div class="card think-card">
    <p>🤔 <strong>先想一想:</strong>{让她猜一个数,或猜一个方向}</p>
  </div>

  <!-- 5. 互动可视化(滑块/动画/拖拽,服务教学) -->
  <div class="card">
    <!-- 这里是关卡的核心交互 -->
  </div>

  <!-- 6. 关键洞察(在她玩过 5 之后才出现/亮起) -->
  <div class="card insight-card">
    <p>💡 <strong>发现没?</strong>{揭示规律}</p>
  </div>

  <!-- 7. 解题步骤 + 公式 -->
  <div class="card">
    <h3>怎么算?</h3>
    <ol>
      <li>{第 1 步}</li>
      <li>{第 2 步}</li>
      <li>{第 3 步}</li>
    </ol>
    <div class="formula-box">{公式}</div>
  </div>

  <!-- 8. 变式题 + 完成庆祝 -->
  <div class="card">
    <h3>🎯 试试看(做对 2 题就通关!)</h3>
    <!-- 题目 1 -->
    <!-- 题目 2 -->
    <!-- 题目 3 -->
  </div>

  <div class="complete-card" id="complete-card">
    <h2>🎉 通关啦!</h2>
    <p>你掌握了 {核心概念}!继续挑战下一关吧 →</p>
    <a href="index.html" class="btn btn-secondary" style="display:inline-block;text-decoration:none;margin-top:8px;">回到目录</a>
  </div>

</div>

<script>
  // === 通关逻辑 ===
  // 关卡 ID 用英文,与 index.html 的 keyPrefix 对应
  // 例:catch-up-lv1-done, fractions-lv2-done
  const LEVEL_KEY = '{island-id}-lv{N}-done';

  let correctCount = 0;
  function markCorrect() {
    correctCount++;
    if (correctCount >= 2) {
      localStorage.setItem(LEVEL_KEY, '1');
      const card = document.getElementById('complete-card');
      card.style.display = 'block';
      card.scrollIntoView({behavior: 'smooth', block: 'center'});
    }
  }

  // 判题函数(每个变式题调用 markCorrect 当判对)
  // function check1() { ... if (correct) markCorrect(); }
</script>
</body>
</html>
```

---

## 命名约定

- 文件路径:`<island-english-name>/level-N.html`(如 `catch-up-problems/level-1.html`)
- localStorage key:`<island-english-name>-lvN-done`(如 `catch-up-lv1-done`)
- key 前缀必须和 [index.html](../index.html) `ISLANDS` 数组里的 `keyPrefix` 完全一致

---

## 检查清单(生成完一关后自检)

- [ ] 8 个区块齐全
- [ ] 没有直接给最终答案——是引导式的
- [ ] 互动可视化是服务教学,不是装饰
- [ ] 变式题至少 2~3 道,做对 ≥ 2 才通关
- [ ] 完成庆祝调用了 `localStorage.setItem`,key 拼对
- [ ] 顶部导航的"回到目录"链接是相对路径(`index.html`)
- [ ] 全部内联(没引外部 CDN)
- [ ] 字号 ≥ 15px,按钮 ≥ 44px 高
- [ ] 在浏览器开 `localhost:8000` 实际点过一遍,变式题答对真的会显示通关
