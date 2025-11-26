# ME249 Project 3 – Part 1 记录

本节记录我在 `CodeP3.2F25.ipynb` 中为 Task 3.1.1 所做的改动、参数设置与输出产物，便于报告撰写与复现。

## 代码改动（均在 `CodeP3.2F25.ipynb`）
- 填入完整原始数据：输入 `[T_air, I_D, R_L]` 与输出 `[V_L, Power]` 列表。
- (a) 计算并打印中位数：`x_median = [10, 800, 6.696]`，`y_median = [26.45, 100.1]`；按中位数归一化得到 `x_norm`、`y_norm`。
- (b) 数据划分：使用 `rng = np.random.default_rng(7)` 随机打乱，2/3 训练、1/3 验证。
- (c) 模型结构：Keras Sequential，RandomUniform 初始化范围改为 `[-0.1, 0.1]`；层次 `[6, 12, 6, 输出2]`，激活 `elu`（输出层无激活），输入形状 `[3]`。
- (d) 训练超参调整：`RMSprop(lr=0.010)`，epochs=6000，batch_size=8，EarlyStopping 监控 loss，`patience=1200`，恢复最佳权重。
- (e)(f) 反归一化后计算训练/验证 MAE，并保存指标到 `outputs/part1/metrics.json`。
- (e)(f) 绘制功率预测 vs. 数据的对数-对数散点图（训练、验证），带 1:1 参考线。
- (g) 生成 T_air=20°C、`R_L∈[4,8]`、`I_D∈[500,1200]` 的功率曲面图。

## 主要超参
- 初始化范围：`[-0.1, 0.1]`
- 学习率：`0.010`
- 批大小：`8`
- 训练轮次：`6000`（带 EarlyStopping，patience=1200）
- 数据划分随机种子：`7`

## 训练结果（一次代表性运行）
- Train MAE (V_L, Power): ~[0.69 V, 2.74 W]
- Train MAE overall: ~1.71
- Val MAE (V_L, Power): ~[0.75 V, 3.03 W]
- Val MAE overall: ~1.89
- 归一化 loss 最佳值约 0.00948（低于 0.025 目标）

## 输出文件
- `outputs/part1/train_power_loglog.png`：训练集功率对数-对数图（含 1:1 线）
- `outputs/part1/val_power_loglog.png`：验证集功率对数-对数图（含 1:1 线）
- `outputs/part1/power_surface.png`：T_air=20°C 时功率曲面图
- `outputs/part1/metrics.json`：保存训练/验证 MAE 指标（物理单位）

## 使用说明
按顺序运行 notebook 的 cell0 → cell5，即可复现上述数据划分、模型训练与图像/指标输出。若需继续调参，可在 cell2 调整学习率、epochs、patience 或去掉 EarlyStopping 后重跑。***
