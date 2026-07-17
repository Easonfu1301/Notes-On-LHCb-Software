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

### 规则总结

1. **有浮动体必有 label** — Figure、Table、Listing 缺一不可
2. **引用带类型前缀** — `Figure.~\ref{}`, `Table.~\ref{}`, `Listing.~\ref{}`, `Section.~\ref{}`, `Eq.~\ref{}`
3. **用 `~` 不可断空格** — 防止 "Figure." 与编号在换行时分离
4. **标签命名小写 + 前缀** — `fig:`, `tab:`, `lst:`, `sec:`, `eq:`
