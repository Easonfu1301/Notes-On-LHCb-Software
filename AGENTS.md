# AGENTS.md — AI 协作者规范

## 交叉引用规范

### 基本原则
- 所有浮动体（Figure、Table、Listing 等）**必须**带有 `\label{}`
- 引用时使用 **完整名称 + `~\ref{}`** 格式，例如 `Figure.~\ref{fig:xxx}`

### 标签命名规则

| 浮动体类型 | 标签前缀 | 示例 |
|-----------|---------|------|
| Figure（图） | `fig:` | `\label{fig:overview}` |
| Table（表） | `tab:` | `\label{tab:tv-z}` |
| Listing（代码） | `lst:` | `\label{lst:example}` |
| Section（节） | `sec:` | `\label{sec:background}` |
| Equation（公式） | `eq:` | `\label{eq:mass}` |

### 引用格式

引用时必须带类型前缀，使用不可断空格 `~`：

```latex
% ✅ 正确写法
Figure.~\ref{fig:overview}
Table.~\ref{tab:tv-z}
Listing.~\ref{lst:example}
Section.~\ref{sec:background}
Eq.~\ref{eq:mass}

% ❌ 错误写法
\ref{tab:tv-z}              % 缺少 "Table." 前缀
Table. \ref{tab:tv-z}       % 普通空格，可能导致换行断开
Fig.~\ref{fig:overview}     % 缩写不统一，统一用完整拼写
```

### 完整示例

```latex
% --- 定义 ---
\begin{figure}[htbp]
    \centering
    \includegraphics[width=0.8\textwidth]{Background/Overview.png}
    \caption{LHCb 升级二期子探测器布局示意图。}
    \label{fig:overview}
\end{figure}

\begin{table}[htbp]
    \centering
    \caption{TV 各 station 的 z 坐标。}
    \label{tab:tv-z}
    \begin{tabular}{cc}
        \toprule
        Station & $z$ [mm] \\
        \midrule
        0 & $-281.25$ \\
        \bottomrule
    \end{tabular}
\end{table}

% --- 引用 ---
探测器布局如 Figure.~\ref{fig:overview} 所示。
TV 的 z 坐标如 Table.~\ref{tab:tv-z} 所示。
```

### 图的强制引用规范

每个 Figure（`\label{fig:...}`）**必须**在正文中至少被引用一次。如果 AI 协作者在代码中发现了未被引用的 Figure，应主动找到最合适的正文位置，添加对该图的引用。

**AI 协作者检查流程：**

1. 扫描所有 `\label{fig:...}`，统计每个标签在正文中被 `\ref{}` 引用的次数。
2. 若某个 Figure 的引用次数为 0：
   - 阅读该 Figure 的 `\caption{}`，理解其内容。
   - 在正文中找到与该图内容最相关的段落或句子。
   - 在该位置添加引用，格式为 `Figure.~\ref{fig:xxx}`。
   - 必要时可微调上下文语句，使引用自然融入。
3. 新增 Figure 时，必须在创建的同时添加至少一处引用。

### 规则总结

1. **有浮动体必有 label** — Figure、Table、Listing 缺一不可
2. **每个 Figure 至少被引用一次** — 新增或现有图均需满足；缺失时 AI 应主动找到最合适位置补上引用
3. **引用带类型前缀** — `Figure.~\ref{}`, `Table.~\ref{}`, `Listing.~\ref{}`, `Section.~\ref{}`, `Eq.~\ref{}`
4. **用 `~` 不可断空格** — 防止 "Figure." 与编号在换行时分离
5. **标签命名小写 + 前缀** — `fig:`, `tab:`, `lst:`, `sec:`, `eq:`

---

## 公式排版规范

### 公式环境选择

优先使用 `eqnarray` 环境排版多行公式：

```latex
% ✅ 正确写法
\begin{eqnarray}
    x &=& a + b \\
    y &=& c + d
\end{eqnarray}

% ❌ 避免使用
\begin{align}
    x &= a + b \\
    y &= c + d
\end{align}
```

### 规则总结

1. **多行公式优先用 `eqnarray`** — 对齐方式统一用 `&=&` 格式
2. **单行公式可用 `equation`** — 简单情况不强求 `eqnarray`

---

## 单位与数字排版规范

### 基本原则
- 所有物理单位必须用 `\mathrm{}` 包裹
- 数字与单位之间必须有一个空格（用 `~` 不可断空格）

### 正确示例

```latex
% ✅ 正确写法
$12.5~\mathrm{mm}$
$3.0~\mathrm{GeV}/c^2$
$299\,792\,458~\mathrm{m/s}$

% ❌ 错误写法
$12.5mm$                    % 单位未用 \mathrm 包裹，会被渲染为斜体变量
$12.5\mathrm{mm}$           % 数字和单位间缺少间隔空格
$12.5 mm$                   % 普通空格，可能换行断开
```

### 规则总结

1. **单位用 `\mathrm{}`** — 防止被渲染为斜体变量
2. **数字与单位间用 `~`** — 不可断空格，防止换行分离
3. **复合单位整体包裹** — 如 `$\mathrm{GeV}/c^2$` 或 `$\mathrm{m/s}$`

---

## Listing 代码块文件路径规范

### 基本原则
- 每个 `\begin{lstlisting} ... \end{lstlisting}` 代码块的**第一行**必须写出该代码所属的源文件路径。

### 正确示例

```latex
% ✅ 正确写法 — 第一行标注了文件路径
\begin{lstlisting}[label=lst:example, caption={some code}]
// path/to/file.cpp
int main() {
    return 0;
}
\end{lstlisting}

% ❌ 错误写法 — 缺少文件路径
\begin{lstlisting}[label=lst:example, caption={some code}]
int main() {
    return 0;
}
\end{lstlisting}
```

### AI 协作者检查流程

在修改或新增任何 `lstlisting` 代码块时，**必须**执行以下检查：

1. 确认 `lstlisting` 内部的第一行是否包含文件路径（通常以注释形式，如 `// path/to/file` 或 `# path/to/file`）
2. **如果缺少文件路径**，使用 `vscode_askQuestions` 工具向用户询问该代码的来源文件路径，**不要自行猜测或编造路径**。
3. 用户提供路径后，再将其添加到 `lstlisting` 的第一行。

调用 `vscode_askQuestions` 的示例：
```json
{
  "questions": [
    {
      "header": "missing-file-path",
      "question": "Listing \\label{lst:xxx} 的第一行缺少源文件路径，请提供该代码的来源文件路径：",
      "allowFreeformInput": true
    }
  ]
}
```

### 规则总结

1. **每个 listing 第一行必须有文件路径** — 以注释形式标注
2. **缺失时用 `vscode_askQuestions` 询问用户** — 绝不自行猜测路径
3. **路径格式** — 根据语言选择注释符号：C/C++ 用 `//`，Python 用 `#`，等等
