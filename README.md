Documentation: https://pdollar.github.io/toolbox/

这个工具箱一共包含七大部分： 
## channels 
Robust image features, including HOG, for fast object detection.   
## classify 
Fast clustering, random ferns, RBF functions, PCA, etc.   
## detector 
Aggregate Channel Features (ACF) object detection code.   
## filters 
Routines for filtering images.   
## images 
Routines for manipulating and displaying images.   
## matlab 
General Matlab functions that should have been a part of Matlab.   
## videos 
Routines for annotating and displaying videos.   
# 添加路径
addpath(genpath(‘D:\piotr_toolbox\toolbox’)); 
savepath;   
（1）channel里的hog的使用：   
注意：由于要用到部分C++程序，所以我们需要编译一下：   
在MATLAB命令窗口输入：toolboxCompile;   
用到的是： 
H = hog( I, binSize, nOrients, clip, crop )   

INPUTS 

% I - [hxw] color or grayscale input image (must have type single) 
% binSize - [8] spatial bin size 
% nOrients - [9] number of orientation bins 
% clip - [.2] value at which to clip histogram bins 
% crop - [0] if true crop boundaries（对边界特别处理） 
% 
% OUTPUTS 
% H - [h/binSize w/binSize nOrients*4] computed hog features   
调用方法： 
I=imResample(single(imread(‘peppers.png’)),[480 640])/255; 读取图像，统一图像大小，类型为Single 
H=hog(I,8,9); 返回的是60*80*36的多维矩阵（考虑边界的8个像素）   
e=hog(I,8,9,0.2,1);返回的是58*78*36的多维矩阵（考虑边界的8个像素）   
（2）channel里的hogDraw的使用：   
用到的是： V = hogDraw( H, w, fhog ) 
作用是画出hog的示意图。 
INPUTS   
% H - [m n oBin*4] computed hog features   
% w - [15] width for each glyph   
% fhog - [0] if true draw features returned by fhog   
% 
% OUTPUTS 
% V - [m*w n*w] visualization of hog features 
调用方法: 
V=hogDraw(e,25); 
im(V); 
(3)channel里的gradientMag的使用：      
用到的是：[M,O] = gradientMag( I, channel, normRad, normConst, full ) 
作用是求出每个像素的梯度以及幅值。 
% INPUTS 
% I - [hxwxk] input k channel single image 
% channel - [0] if>0 color channel to use for gradient computation 
% normRad - [0] normalization radius (no normalization if 0) 
% normConst - [.005] normalization constant 
% full - [0] if true compute angles in [0,2*pi) else in [0,pi) 
% 
% OUTPUTS 
% M - [hxw] gradient magnitude at each location 
% O - [hxw] approximate gradient orientation modulo PI 
调用： 
a=rgbConvert(imread(‘peppers.png’),’gray’); 
[M,O]=gradientMag(a); 
（4）channel里的gradientHist的使用：    
作用是求每一个patch的梯度统计。 
用到的是：H = gradientHist( M, O, varargin ) 
% INPUTS 
% M - [hxw] gradient magnitude at each location (see gradientMag.m) 
% O - [hxw] gradient orientation in range defined by param flag 
% binSize - [8] spatial bin size 
% nOrients - [9] number of orientation bins 
% softBin - [1] set soft binning (odd: spatial=soft, >=0: orient=soft) 
% useHog - [0] 1: compute HOG (see hog.m), 2: compute FHOG (see fhog.m) 
% clipHog - [.2] value at which to clip hog histogram bins 
% full - [false] if true expects angles in [0,2*pi) else in [0,pi) 
% 
% OUTPUTS 
% H - [w/binSize x h/binSize x nOrients] gradient histograms 
调用： 
H1=gradientHist(M,O,2,6,0); 
（5）channel里的J = rgbConvert( I, colorSpace, useSingle )的使用：   
作用是实现不同的图像空间的相互转换。 
% INPUTS 
% I - [hxwx3] input rgb image (uint8 or single/double in [0,1]) 
% colorSpace - [‘luv’] other choices include: ‘gray’, ‘hsv’, ‘rgb’, ‘orig’ 
% useSingle - [true] determines output type (faster if useSingle) 
% 
% OUTPUTS 
% J - [hxwx3] single or double output image (normalized to [0,1]) 
使用方法： 
I = imread(‘peppers.png’); 
J = rgbConvert( I, ‘luv’ ); 
J2=rgbConvert( I, ‘hsv’ ); 
（6）image里的varargout = montage2( IS, prm )的使用：   
作用是显示各个通道的图像。 
使用方法：   
figure(2),montage2(J);  
