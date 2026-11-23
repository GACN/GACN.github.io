# 未完成事项与停止状态

截至 2026-11-23，本轮 ComfyUI / FreeVideo-Pascal 优化研究停止继续扩展。以下项目没有完成，不得在公开介绍中写成已经实现。

## 尚未完成的实验

1. QK INT8 + V FP32 消融。
2. 仅启用 Layer 0、16、32、49 的 QKV 白名单测试。
3. Group-wise Activation Quantization：Group 128 / 256 / 512。
4. 达到 PSNR ≥ 35 dB、SSIM ≥ 0.98 的39帧 INT8 配置。
5. 通过画质验收后的243帧 INT8 长视频测试。
6. NVENC 与 libx264 编码速度、码率、画质对照。
7. SATA SSD 与 NVMe 模型装载时间对照。
8. Nsight 对完整生产推理路径的 Kernel Trace。
9. 更长视频的 latent continuation 接缝与身份一致性系统评估。

## 已经做过但没有收益的尝试

- FREEVIDEO_READ_THREADS 从4增加到8：没有收益，绑定阶段反而变长。
- FREEVIDEO_DECODE_READ_AHEAD=off：总耗时变差。
- 自写 Online Softmax Attention：在测试形状上比 PyTorch SDPA 慢约2.4倍。
- 直接显式构造完整注意力矩阵：长序列 OOM。

## ComfyUI 停止使用说明

用户已决定停止折腾本机 ComfyUI，并计划卸载。

本项目没有远程执行本机卸载，也没有删除用户的模型、工作流、缓存或虚拟环境。

如果手动卸载，建议先区分：
- ComfyUI 程序目录
- 独立 Python / venv
- models 目录
- workflows / outputs / input
- 自定义节点
- 独立 FreeVideo-Pascal 环境与模型缓存

不要因为删除 ComfyUI 主目录，就误删仍希望保留的模型、工作流或独立实验记录。

## 最终状态

当前最可靠的长视频结果仍是原始 FP32 路径：
448×320 / 243帧 / 双轮8+3 / request_seconds 1509.3秒。

INT8 QKV 的39帧测试观察到约10%的端到端耗时改善，但 PSNR 33.55 dB、SSIM 0.9527，未通过预设画质门槛。因此不能宣称它已经成为可替代原始路径的优化版本。

研究暂停，不代表实验失败。保留报告和代码分支，供以后复现。
