# awesome-agent-rules 扩充计划

> 基于 GitHub 搜索结果，待后续迭代补充的内容

## 已知的竞品/参考项目

| 项目 | Stars | 说明 | 我们的差异化 |
|------|-------|------|-------------|
| [awesome-cursorrules](https://github.com/PatrickJS/awesome-cursorrules) | 高 | Cursor 专用 .cursorrules 合集 | 我们覆盖**所有工具**（CLAUDE.md/AGENTS.md/GEMINI.md），不限 Cursor |
| [agents.md](https://agents.md) | — | AGENTS.md 官方规范网站 | 我们提供**实战规则模板**，不只是规范文档 |
| [OpenSpec](https://github.com/Fission-AI/OpenSpec) | 38k+ | Spec-Driven Development | 定位不同：OpenSpec 管需求，我们管 Agent 行为 |

## 待补充的内容

### 高优先级
- [ ] **CLAUDE.md 最佳实践专栏**：参考搜索结果中的"Rule of Mistake"、Progressive Disclosure、200行上限等原则
- [ ] **Cursor .mdc 规则指南**：`.cursor/rules/` 目录结构、frontmatter 格式、glob 作用域
- [ ] **Windsurf 规则**：`AGENTS.md` 在 Windsurf/Cascade 中的用法（子目录作用域、层级覆盖）
- [ ] **真实项目 CLAUDE.md 示例**：从开源大项目中搜集（需确认许可证）

### 中优先级
- [ ] **按语言/框架分类的规则**：Python 项目最佳实践、TypeScript 项目最佳实践等
- [ ] **按场景分类**：Debug 规则、重构规则、测试规则、部署规则
- [ ] **Aider `.aider.conf.yml` 模板**  
- [ ] **GitHub Copilot 指令文件**

### 低优先级
- [ ] **对比评测**：同一任务用不同规则的效果对比
- [ ] **社区投稿流程自动化**：PR 模板、自动标签

## 搜索结果中的关键洞察（可融入 README）

### CLAUDE.md 专家建议
1. **"Rule of Mistake"**：如果删掉这条规则不会导致 AI 犯错，那就删掉它
2. **渐进式披露**：不要塞所有细节，用 `@` 引用指向详细文档
3. **200 行上限**：每行都消耗 context window 预算
4. **用 `/init` 生成初始版**：Claude Code 的内置命令
5. **子目录 CLAUDE.md**：只在操作该目录时才加载

### Cursor .mdc 最佳实践
1. **模块化文件**：每个 .mdc 不超过 500 行
2. **4 种优先级**：Manual → Auto Attached → Agent Requested → Always
3. **正面指令优于负面指令**：告诉 AI 怎么做，而不是不要怎么做
4. **frontmatter 控制作用域**：`globs` + `alwaysApply` + `description`

### AGENTS.md 规范要点
1. **跨工具标准**：Windsurf、Codex、Cursor、Claude Code 都支持
2. **子目录覆盖**：放在子目录中只对该目录生效
3. **Plain Markdown**：不需要特殊语法
