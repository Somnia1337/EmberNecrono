%%

1. 介绍技术和方法（引言）
2. 编程实验（提交源代码）
3. 分析、讨论实验结果
4. 结论

%%

### 01 引言

#### 图像平滑

目的：减少噪声或细节，使图像更平滑。

- 空间域方法
	- **均值滤波**：利用局部像素的平均值替换中心像素值，适用于去除随机噪声，但可能会模糊图像边缘。
	- **中值滤波**：用局部窗口中像素值的中值替换中心像素，能有效去除椒盐噪声，同时保留边缘信息。
	- **高斯滤波**：对像素值加权平均，权重依据高斯分布分配，适合处理高斯噪声。
	- **自适应滤波**：根据图像局部统计特性动态调整滤波方式，在平滑噪声的同时尽可能保留细节。
- 频率域方法
	- **低通滤波器**：通过削减高频成分（通常代表噪声和细节）实现图像平滑，典型方法包括理想低通滤波、巴特沃斯低通滤波等。

#### 图像锐化

目的：增强边缘和纹理细节，使图像更清晰，常用于边缘检测和特征提取。

- 空间域方法
	- **Robert 交叉梯度算子**：利用对角差分计算梯度，简单快速但对噪声敏感。
	- **Sobel 梯度算子**：结合水平方向和垂直方向的梯度计算，具有一定的抗噪能力。
	- **拉普拉斯算子**：通过计算图像的二阶导数，突出边缘，但可能引入噪声。
	- **LoG 算子**：结合高斯滤波与拉普拉斯算子，兼顾平滑和边缘检测。
- 频率域方法
	- **高通滤波器**：通过削减低频成分（通常代表平滑区域）来增强高频成分，典型方法包括理想高通滤波和巴特沃斯高通滤波。

### 02 编程实验

> [!question] 自己选择一幅图片，加入椒盐噪声，编写 MATLAB 程序分别在空间域采用均值滤波、中值滤波、高斯滤波和自适应滤波以及在频率域采用低通滤波器进行图像平滑对比实验，对实验结果进行分析和讨论，得出相关结论。

```matlab
% 原始图片
img = imread('cameraman.tif');
imshow(img), title('原始');

% 椒盐噪声
noisy_img = imnoise(img, 'salt & pepper', 0.02);
figure, imshow(noisy_img), title('加入椒盐噪声');

% 均值滤波
kernel_avg = fspecial('average', [3 3]);
smoothed_avg = imfilter(noisy_img, kernel_avg, 'replicate');
figure, imshow(smoothed_avg), title('均值滤波');

% 中值滤波
smoothed_median = medfilt2(noisy_img, [3 3]);
figure, imshow(smoothed_median), title('中值滤波');

% 高斯滤波
kernel_gaussian = fspecial('gaussian', [5 5], 1);
smoothed_gaussian = imfilter(noisy_img, kernel_gaussian, 'replicate');
figure, imshow(smoothed_gaussian), title('高斯滤波');

% 自适应滤波
smoothed_adaptive = wiener2(noisy_img, [5 5]);
figure, imshow(smoothed_adaptive), title('自适应滤波');

% 频率域低通滤波
fft_img = fft2(double(noisy_img));
fft_img = fftshift(fft_img);
[M, N] = size(fft_img);
D0 = 20;
[u, v] = meshgrid(-N/2:N/2-1, -M/2:M/2-1);
D = sqrt(u.^2 + v.^2);
low_pass_filter = double(D <= D0);
filtered_fft = fft_img .* low_pass_filter;
filtered_img = ifft2(ifftshift(filtered_fft));
filtered_img = uint8(abs(filtered_img));
figure, imshow(filtered_img), title('频率域低通滤波');
```

<div style="display: flex; justify-content: space-around; align-items: center;">
    <img src="D:\Markdown Files\EmberNecrono\_Attachment\Computer Science\数字图像处理\dip_homework_2_1_1.png" alt="Image 1" style="width: 30%; height: auto;">
    <img src="D:\Markdown Files\EmberNecrono\_Attachment\Computer Science\数字图像处理\dip_homework_2_1_2.png" alt="Image 2" style="width: 30%; height: auto;">
</div>

<div style="display: flex; justify-content: space-around; align-items: center;">
	<img src="D:\Markdown Files\EmberNecrono\_Attachment\Computer Science\数字图像处理\dip_homework_2_1_3.png" alt="Image 3" style="width: 30%; height: auto;">
    <img src="D:\Markdown Files\EmberNecrono\_Attachment\Computer Science\数字图像处理\dip_homework_2_1_4.png" alt="Image 1" style="width: 30%; height: auto;">
</div>

<div style="display: flex; justify-content: space-around; align-items: center;">
    <img src="D:\Markdown Files\EmberNecrono\_Attachment\Computer Science\数字图像处理\dip_homework_2_1_5.png" alt="Image 2" style="width: 30%; height: auto;">
    <img src="D:\Markdown Files\EmberNecrono\_Attachment\Computer Science\数字图像处理\dip_homework_2_1_6.png" alt="Image 3" style="width: 30%; height: auto;">
    <img src="D:\Markdown Files\EmberNecrono\_Attachment\Computer Science\数字图像处理\dip_homework_2_1_7.png" alt="Image 3" style="width: 30%; height: auto;">
</div>

> [!question] 自己选择一幅图片，编写 MATLAB 程序分别在空间域采用 Robert 交叉梯度算子、Sobel 梯度算子、拉普拉斯算子和 LoG 算子以及在频率域采用高通滤波器进行图像锐化对比实验，对实验结果进行分析和讨论，得出相关结论。

```matlab
% 原始图片
img = imread('cameraman.tif');
imshow(img), title('原始');

% Robert 交叉梯度算子
kernel_roberts_x = [1 0; 0 -1];
kernel_roberts_y = [0 1; -1 0];
roberts_x = imfilter(double(img), kernel_roberts_x);
roberts_y = imfilter(double(img), kernel_roberts_y);
sharpened_roberts = sqrt(roberts_x.^2 + roberts_y.^2);
figure, imshow(uint8(sharpened_roberts)), title('Robert 梯度算子锐化');

% Sobel 梯度算子
sobel_x = fspecial('sobel');
sobel_y = sobel_x';
sobel_x_img = imfilter(double(img), sobel_x);
sobel_y_img = imfilter(double(img), sobel_y);
sharpened_sobel = sqrt(sobel_x_img.^2 + sobel_y_img.^2);
figure, imshow(uint8(sharpened_sobel)), title('Sobel 梯度算子锐化');

% 拉普拉斯算子
kernel_laplacian = fspecial('laplacian', 0.2);
sharpened_laplacian = imfilter(double(img), kernel_laplacian);
figure, imshow(uint8(abs(sharpened_laplacian))), title('拉普拉斯算子锐化');

% LoG 算子
kernel_log = fspecial('log', [5 5], 0.5);
sharpened_log = imfilter(double(img), kernel_log);
figure, imshow(uint8(abs(sharpened_log))), title('LoG 算子锐化');

% 频率域高通滤波
fft_img = fft2(double(img));
fft_img = fftshift(fft_img);
[M, N] = size(fft_img);
D0 = 50;
[u, v] = meshgrid(-N/2:N/2-1, -M/2:M/2-1);
D = sqrt(u.^2 + v.^2);
high_pass_filter = double(D > D0);
filtered_fft = fft_img .* high_pass_filter;
filtered_img = ifft2(ifftshift(filtered_fft));
filtered_img = uint8(abs(filtered_img));
figure, imshow(filtered_img), title('频率域高通滤波');
```

<div style="display: flex; justify-content: space-around; align-items: center;">
    <img src="D:\Markdown Files\EmberNecrono\_Attachment\Computer Science\数字图像处理\dip_homework_2_2_1.png" alt="Image 1" style="width: 30%; height: auto;">
    <img src="D:\Markdown Files\EmberNecrono\_Attachment\Computer Science\数字图像处理\dip_homework_2_2_2.png" alt="Image 2" style="width: 30%; height: auto;">
</div>

<div style="display: flex; justify-content: space-around; align-items: center;">
	<img src="D:\Markdown Files\EmberNecrono\_Attachment\Computer Science\数字图像处理\dip_homework_2_2_3.png" alt="Image 3" style="width: 30%; height: auto;">
    <img src="D:\Markdown Files\EmberNecrono\_Attachment\Computer Science\数字图像处理\dip_homework_2_2_4.png" alt="Image 1" style="width: 30%; height: auto;">
</div>

<div style="display: flex; justify-content: space-around; align-items: center;">
    <img src="D:\Markdown Files\EmberNecrono\_Attachment\Computer Science\数字图像处理\dip_homework_2_2_5.png" alt="Image 2" style="width: 30%; height: auto;">
    <img src="D:\Markdown Files\EmberNecrono\_Attachment\Computer Science\数字图像处理\dip_homework_2_2_6.png" alt="Image 3" style="width: 30%; height: auto;">
</div>

### 03 结果分析

#### 实验 1：图像平滑

- **均值滤波**
	- 优点：对椒盐噪声有一定去除效果。
	- 缺点：由于采用简单平均，导致图像整体模糊，细节和边缘明显损失。
- **中值滤波**
	- 优点：对椒盐噪声效果显著，能保留较多图像边缘和细节。
	- 缺点：可能在某些场景中引入轻微平滑伪影（例如边缘的局部波动）。
- **高斯滤波**
	- 优点：对高斯噪声有良好效果，同时相对保留图像边缘。
	- 缺点：对椒盐噪声的去除效果较弱。
- **自适应滤波**
	- 优点：自适应调整窗口参数，能在平滑噪声的同时更好地保留图像细节。
	- 缺点：计算复杂度较高。
- **低通滤波**
	- 优点：能有效去除高频噪声。
	- 缺点：由于削弱了所有高频分量，导致边缘信息丢失，图像模糊严重。

#### 实验 2：图像锐化

- **Robert 交叉梯度算子**
	- 优点：计算简单，对对角线边缘增强效果较好。
	- 缺点：对噪声非常敏感，可能导致边缘不连续，且对非对角方向的边缘检测能力较弱。
- **Sobel 梯度算子**
	- 优点：结合水平和垂直方向的梯度，增强边缘细节的能力较强，噪声敏感性比 Robert 算子稍低。
	- 缺点：仍会放大图像中的噪声，在高噪声场景中效果不佳。
- **拉普拉斯算子**
	- 优点：通过计算图像的二阶导数，可以很好地突出边缘和细节。
	- 缺点：对噪声特别敏感，可能在噪声较多的情况下放大噪声导致伪边缘出现。
- **LoG 算子**
	- 优点：结合高斯滤波和拉普拉斯算子的特点，先平滑噪声再增强边缘，具有一定的抗噪能力。
	- 缺点：对高频细节可能存在一定的损失，计算复杂度较高。
- **高通滤波（理想高通滤波器）**
	- 优点：能够显著增强图像的边缘细节。
	- 缺点：简单的理想高通滤波器可能导致振铃效应，并对噪声较为敏感。

### 04 实验总结

图像平滑实验结论：

- **中值滤波** 的效果最佳，能够显著去除噪声而保留边缘。
- **均值滤波** 和 **高斯滤波** 适合处理随机噪声，但对椒盐噪声效果有限。
- **自适应滤波** 兼顾去噪和细节保留，适用于复杂场景。
- **频率域低通滤波** 虽然有效去除高频噪声，但对细节敏感的场景并不适合。

图像锐化实验结论：

- **Robert 算子** 和 **Sobel 算子** 对边缘增强能力较强，但 Sobel 算子更全面。
- **拉普拉斯算子** 适合增强全局边缘，**LoG 算子** 则兼顾了抗噪能力。
- **频率域高通滤波器** 增强了所有高频成分，但易引入伪影。