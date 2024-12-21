> [!question] 简述图像复原与图像增强的区别和各自的主要方法。

#### 区别

- 定义
    - *图像复原*：通过数学模型和算法，逆转图像在获取、传输和存储过程中受到的退化，以恢复接近真实场景的图像。图像复原强调真实性。
    - *图像增强*：通过对图像的某些特性进行增强，使其更适合人眼观察或机器处理。图像增强强调视觉效果或特定应用需求。
- 目标
    - *图像复原*：还原图像原貌，减小噪声、模糊等退化现象，提高图像质量。
    - *图像增强*：提高图像的视觉效果或特定特征，便于后续分析或应用。

#### 主要方法

- *图像复原*
    - 去噪
        - 空间域滤波：如均值滤波、中值滤波、高斯滤波、自适应滤波。
        - 频率域滤波：如维纳滤波。
        - 现代方法：基于小波变换或深度学习的图像去噪算法。
    - 去模糊
        - 逆滤波：适合已知退化函数的线性模糊。
        - 维纳滤波：结合噪声功率和信号功率最优复原模糊图像。
        - 盲去卷积：在退化函数未知的情况下进行模糊恢复。
    - 运动复原
        - 恢复因相机运动或物体运动导致的模糊，采用运动估计模型。
- *图像增强*
    - 灰度变换
        - 对比度拉伸、伽马校正、直方图均衡化等。
    - 空间域增强
        - 点运算：如亮度调整、伽马变换。
        - 区域运算：如边缘检测、锐化。
    - 频率域增强
        - 高通滤波：突出高频分量，增强边缘和细节。
    - 彩色增强
        - 色彩平衡、白平衡、伪彩色增强。
    - 多尺度增强
        - 利用小波变换或金字塔分解方法对细节进行不同尺度增强。

> [!question] 自己选择一幅图片，先对该图像进行退化处理，再添加高斯噪声，最后用逆滤波、维纳滤波、约束最小二乘方法、L-R 算法和盲去卷积方法进行图像复原仿真实验，对实验结果进行分析和讨论，得出相关结论。

#### 代码

```MATLAB
% 载入 MATLAB 内置图像
original_img = im2double(imread('cameraman.tif'));

% 1. 图像退化处理（模糊）
PSF = fspecial('motion', 21, 11); % 运动模糊点扩散函数 (PSF)
blurred_img = imfilter(original_img, PSF, 'conv', 'circular'); % 模糊图像

% 2. 添加高斯噪声
noise_variance = 0.001; % 噪声方差
noisy_blurred_img = imnoise(blurred_img, 'gaussian', 0, noise_variance);

% 3. 逆滤波复原
% 直接使用逆滤波（不考虑噪声）
inverse_filtered_img = deconvwnr(noisy_blurred_img, PSF, 0);

% 4. 维纳滤波复原
estimated_nsr = noise_variance / var(original_img(:)); % 估计的噪声与信号比 (NSR)
wiener_filtered_img = deconvwnr(noisy_blurred_img, PSF, estimated_nsr);

% 5. 约束最小二乘方法（CLS）
regularization_param = 0.005; % 正则化参数
cls_filtered_img = deconvreg(noisy_blurred_img, PSF, regularization_param);

% 6. L-R 迭代算法
num_iterations = 20; % 迭代次数
lr_filtered_img = deconvlucy(noisy_blurred_img, PSF, num_iterations);

% 7. 盲去卷积复原
[blind_filtered_img, estimated_psf] = deconvblind(noisy_blurred_img, PSF, 10);

% 对比显示所有结果
figure;
subplot(3, 3, 1), imshow(original_img), title('原始');
subplot(3, 3, 2), imshow(blurred_img), title('退化');
subplot(3, 3, 3), imshow(noisy_blurred_img), title('退化+高斯噪声');
subplot(3, 3, 4), imshow(inverse_filtered_img), title('逆滤波复原');
subplot(3, 3, 5), imshow(wiener_filtered_img), title('维纳滤波复原');
subplot(3, 3, 6), imshow(cls_filtered_img), title('约束最小二乘复原');
subplot(3, 3, 7), imshow(lr_filtered_img), title('L-R 算法复原');
subplot(3, 3, 8), imshow(blind_filtered_img), title('盲去卷积复原');
```

#### 结果

![[dip_homework_3.png|500]]

#### 分析与讨论

- **退化图像**：经过运动模糊退化并加入高斯噪声后，图像中边缘信息变得模糊，细节丢失严重。
- **逆滤波**：对模糊有一定的复原效果，但噪声显著放大。
- **维纳滤波**：在去噪和模糊复原之间取得了良好平衡，效果优于逆滤波。
- **约束最小二乘法**：进一步增强了边缘细节，同时抑制了一些噪声。
- **L-R 算法**：效果较好，能够逐步恢复细节，但迭代次数较多时可能引入伪影。
- **盲去卷积**：估计 PSF 后，图像细节恢复效果较好，但对噪声敏感。

#### 结论

- **已知退化模型**：在已知退化模型的情况下，*维纳滤波* 和 *约束最小二乘法* 提供了较好的复原效果，能够有效去除噪声并恢复图像细节。
- **未知退化模型**：在退化模型未知的情况下，*盲去卷积* 方法能够较好地恢复图像细节，但对噪声较为敏感，复原效果受噪声估计的影响较大。
- **实际应用**：不同复原方法的选择应根据具体场景决定。在应用中通常需要对退化函数和噪声参数进行合理估计，以确保复原效果最佳。
