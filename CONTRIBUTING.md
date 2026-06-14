# Contributing

欢迎贡献新的实验模板、指标类型、脚本改进和文档修订。

## 开发流程

1. Fork 或 clone 仓库。
2. 在本地修改 `plugins/algorithm-experiment-manager/skills/algorithm-experiment-manager/` 下的 `SKILL.md`、`templates/`、`examples/` 或 `scripts/`。
3. 运行发布校验：

```bash
python3 scripts/validate_marketplace.py
```

4. 若修改了 Python 脚本，确保仍然只依赖标准库，除非有非常明确的理由。
5. 提交 PR 时说明：
   - 修改了哪些实验管理场景。
   - 是否影响已有模板字段。
   - 是否新增或改变脚本命令行参数。
   - 是否影响 marketplace 或 plugin manifest。

## 内容原则

- 不针对某一个项目、模型或算法过度定制。
- 缺失实验信息写“待补充”或“用户未提供”，不要鼓励编造。
- 保持增量记录、失败实验归档、公平对比检查和论文证据分级这四条主线。
- 新指标应说明适用实验类型，不要强制所有实验使用所有指标。
