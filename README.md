#  数字图像处理第七次作业——直线检测
### 自动化66史玉康
### 2161800034

## 实验内容

### 1、首先对测试图像（文件名为：test1~test6）进行边缘检测，可采用书上介绍的Sobel等模板或者cann算子方法；

### 1.1 边缘检测基本概念

**边缘的定义**

在数字图像中，边缘是指图像局部变化最显著的部分，边缘主要存在于目标与目标，目标与背景之间，是图像局部特性的不连续性，如灰度的突变、纹理结构的图标、颜色的图标等。尽管图像的边缘点产生的原因各不相同，但他们都是图形上灰度不连续或灰度几句辩护的点，图像边缘分为阶跃状、斜坡状和屋顶状。

**边缘检测的基本方法**

一般图像边缘检测方法主要有如下四个步骤：

图像滤波

传统边缘检测算法主要是基于图像强度的一阶和二阶导数，但导数的计算对噪声很敏感，因此必须使用滤波器来改善与噪声有关的边缘检测器的性能。需要指出的是，大多数滤波器在降低噪声的同时也造成了了边缘强度的损失，因此，在增强边缘和降低噪声之间需要一个折衷的选择。

图像增强

增强边缘的基础是确定图像各点邻域强度的变化值。增强算法可以将邻域(或局部)强度值有显著变化的点突显出来。边缘增强一般是通过计算梯度的幅值来完成的。

图像检测

在图像中有许多点的梯度幅值比较大，而这些点在特定的应用领域中并不都是边缘，所以应该用某种方法来确定哪些点是边缘点。最简单的边缘检测判断依据是梯度幅值。

图像定位

如果某一应用场合要求确定边缘位置，则边缘的位置可在子像素分辨率上来估计，边缘的方位也可以被估计出来。近20多年来提出了许多边缘检测算子，在这里我们仅讨论集中常见的边缘检测算子。

### 1.2常见边缘检测算子分析

**roberts算子**  

Roberts算子采用对角线方向相邻两像素之差近似梯度幅值检测边缘，检测水平和垂直边缘的效果好于斜向边缘，定位精度高，对噪声敏感。当对对角线方向的边缘感兴趣时，一般选择roberts的二位模板。这些模板考虑了中心点两端数据的性质，并携带有关于边缘方向的更多信息。  

**sobel算子**   

 Sobel算子在边缘检测算子扩大了其模版，在边缘检测的同时尽量削弱了噪声。
 其模版大小为3×3，其将方向差分运算与局部加权平均相结合来提取边缘。在求取图像梯度之前，先进行加权平均，然后进行未分，加强了对噪声的一致。其主要用于边缘检测，在技术上它是以离散型的差分算子，用来运算图像亮度函数的梯度的近似值， Sobel算子是典型的基于一阶导数的边缘检测算子，由于该算子中引入了类似局部平均的运算，因此对噪声具有平滑作用，能很好的消除噪声的影响。Sobel算子对于象素的位置的影响做了加权，与Prewitt算子、Roberts算子相比因此效果更好。  
 Sobel算子包含两组3x3的矩阵，分别为横向及纵向模板，将之与图像作平面卷积，即可分别得出横向及纵向的亮度差分近似值。  
 
**prewitt算子**  

Prewitt算子是一种一阶微分算子的边缘检测，利用像素点上下、左右邻点的灰度差，在边缘处达到极值检测边缘，去掉部分伪边缘，对噪声具有平滑作用 。其原理是在图像空间利用两个方向模板与图像进行邻域卷积来完成的，这两个方向模板一个检测水平边缘，一个检测垂直边缘。  
经典Prewitt算子认为：凡灰度新值大于或等于阈值的像素点都是边缘点。即选择适当的阈值T，若P(i,j)≥T，则(i,j)为边缘点，P(i,j)为边缘图像。这种判定是欠合理的，会造成边缘点的误判，因为许多噪声点的灰度值也很大，而且对于幅值较小的边缘点，其边缘反而丢失了。  
Prewitt算子对噪声有抑制作用，抑制噪声的原理是通过像素平均，但是像素平均相当于对图像的低通滤波，所以Prewitt算子对边缘的定位不如Roberts算子。  
因为平均能减少或消除噪声，Prewitt梯度算子法就是先求平均，再求差分来求梯度。  


**LOG算子**
LOG算子基本思想是：先在一定的范围内做平滑滤波，然后再利用差分算子来检测在相应尺度上的边缘。滤波器的选择要考虑以下两个因素：其一是滤波器在空间上要求平稳，即要求空间位置误差 Δ x要小；其二是平滑滤波器本身要求是带通滤波器，并且在有限的带通内是平稳的，即要求频域误差 Δω 要小。根据信号处理中的测不准原理， Δx 和 Δ ω是相互矛盾的，而达到测不准下限的滤波器就是高斯滤波器。Marr 和 Hildreth 提出的这种差分算子是各向同性的拉普拉斯二阶差分算子。该边缘检测器的基本特征是：

（1） 所用的平滑滤波器是高斯滤波器

（2） 增强步骤采用的是二阶导数（即二维拉普拉斯函数）

（3） 边缘检测的判据是二阶导数过零点并且对应一阶导数的极大值

该方法的特点是先用高斯滤波器与图像进行卷积，既平滑了图像又降低了噪声，使孤立的噪声点和较小的结构组织被滤除。然而由于对图像的平滑会导致边缘的延展，因此只考虑那些具有局部梯度极大值的点作为边缘点，这可以用二阶导数的零交叉来实现。拉普拉斯函数可用作二维二阶导数的近似，因为它是一种标量算子。为了避免检测出非显著的边缘，所以应该选择一阶导数大于某一阈值的零交叉点来作为边缘点。

**canny算子**  

该算子功能比前面几种都要好，但是它实现起来较为麻烦，Canny算子是一个具有滤波，增强，检测的多阶段的优化算子.

利用Canny算子检测边缘的土体算法如下:

（1）用式所示的高斯函数h(r)对图像进行平滑滤波，去除图像中的噪声。

（2）在每一点计算出局部梯度和边缘方向，可以利用Sobel算子、Roberts算子等来计算。边缘点定义为梯度方向上其强度局部最大的点。

（3）对梯度进行“非极大值抑制”。在第二步中确定的边缘点会导致梯度幅度图像中出现脊。然后用算法追踪所有脊的顶部，并将所有不在脊的顶部的像素设为零，以便在输出中给出一条细线。

（4）双阐值化和边缘连接。脊像素使用两个闽值Tl和竹做阂值处理，其中Tl<T2.值大于竹的脊像素称为强边缘像素，Tl和T2之间的脊像素称为弱边缘像素。由于边缘阵列孔是用高闽值得到的，因此它含有较少的假边缘，但同时也损失了一些有用的边缘信息。而边缘阵列Tl的闽值较低，保留了较多信息。因此，可以以边缘阵列几为基础，用边缘阵列Tl进行补充连接，最后得到边缘图像。

Canny算子也存在不足之处:

（1）为了得到较好的边缘检测结果，它通常需要使用较大的滤波尺度，这样容易丢失一些细节

（2）Canny算子的双阈值要人为的选取，不能够自适应

### 1.3 实验结果

以下分别为test1~test6的边缘检测结果图。每张图中分别包含原图，以及各种梯度算子作用下的边缘检测结果图。

<img src="https://github.com/YukiShiyk/final/blob/master/image/T1.jpg"  div align=center />
<img src="https://github.com/YukiShiyk/final/blob/master/image/T2.jpg"  div align=center />
<img src="https://github.com/YukiShiyk/final/blob/master/image/T3.jpg"  div align=center />
<img src="https://github.com/YukiShiyk/final/blob/master/image/T4.jpg"  div align=center />
<img src="https://github.com/YukiShiyk/final/blob/master/image/T5.jpg"  div align=center />
<img src="https://github.com/YukiShiyk/final/blob/master/image/T6.jpg"  div align=center />



### 2. 在边缘检测的基础上，用hough变换检测图中直线；

### 2.1 问题分析

Hough变换是一种使用表决原理的参数估计技术。其原理是利用图像空间和Hough参数空间的点－线对偶性，把图像空间中的检测问题转换到参数空间。

Hough直线检测的基本原理在于利用点与线的对偶性，在我们的直线检测任务中，即图像空间中的直线与参数空间中的点是一一对应的，参数空间中的直线与图像空间中的点也是一一对应的。这意味着我们可以得出两个非常有用的结论：   

（1）图像空间中的每条直线在参数空间中都对应着单独一个点来表示；   
（2）图像空间中的直线上任何一部分线段在参数空间对应的是同一个点。   

因此Hough直线检测算法就是把在图像空间中的直线检测问题转换到参数空间中对点的检测问题，通过在参数空间里寻找峰值来完成直线检测任务。      
如前所述，霍88夫直线检测就是把图像空间中的直线变换到参数空间中的点，通过统计特性来解决检测问题。具体来说，如果一幅图像中的像素构成一条直线，那么这些像素坐标值（x, y）在参数空间对应的曲线一定相交于一个点，所以我们只需要将图像中的所有像素点（坐标值）变换成参数空间的曲线，并在参数空间检测曲线交点就可以确定直线了。    
在理论上，一个点对应无数条直线或者说任意方向的直线，但在实际应用中，我们必须限定直线的数量（即有限数量的方向）才能够进行计算。    
因此，我们将直线的方向θ离散化为有限个等间距的离散值，参数ρ也就对应离散化为有限个值，于是参数空间不再是连续的，而是被离散量化为一个个等大小网格单元。将图像空间（直角坐标系）中每个像素点坐标值变换到参数空间（极坐标系）后，所得值会落在某个网格内，使该网格单元的累加计数器加1。当图像空间中所有的像素都经过霍夫变换后，对网格单元进行检查，累加计数值最大的网格，其坐标值（ρ0, θ0）就对应图像空间中所求的直线。   

### 2.2 代码实现

见附录code文档

3. 比较不同边缘检测算法（2种以上）、不同hough变换参数对直线检测的影响；

### 3.1 代码实现

见附录code文档

### 3.2 实验结果

**Roberts算子**

<img src="https://github.com/YukiShiyk/final/blob/master/image/Roberts3.jpg"  div align=center />
<img src="https://github.com/YukiShiyk/final/blob/master/image/Roberts5.jpg"  div align=center />
<img src="https://github.com/YukiShiyk/final/blob/master/image/Roberts7.jpg"  div align=center />

**sobel算子**

<img src="https://github.com/YukiShiyk/final/blob/master/image/sobel3.jpg"  div align=center />
<img src="https://github.com/YukiShiyk/final/blob/master/image/sobel5.jpg"  div align=center />
<img src="https://github.com/YukiShiyk/final/blob/master/image/sobel7.jpg"  div align=center />

**prewitt算子**

<img src="https://github.com/YukiShiyk/final/blob/master/image/prewitt3.jpg"  div align=center />
<img src="https://github.com/YukiShiyk/final/blob/master/image/prewitt5.jpg"  div align=center />
<img src="https://github.com/YukiShiyk/final/blob/master/image/prewitt7.jpg"  div align=center />

**LOG算子**

<img src="https://github.com/YukiShiyk/final/blob/master/image/log3.jpg"  div align=center />
<img src="https://github.com/YukiShiyk/final/blob/master/image/log5.jpg"  div align=center />
<img src="https://github.com/YukiShiyk/final/blob/master/image/log7.jpg"  div align=center />

**canny算子**

<img src="https://github.com/YukiShiyk/final/blob/master/image/canny3.jpg"  div align=center />
<img src="https://github.com/YukiShiyk/final/blob/master/image/canny5.jpg"  div align=center />
<img src="https://github.com/YukiShiyk/final/blob/master/image/canny7.jpg"  div align=center />






### 3.3 结果分析

本次作业中主要对比了test1的在分别以Roberts算子，sobel算子，prewitt算子，LOG算子以及canny算子为梯度算子的边缘检测算法的基础上不同hough变换参数对直线检测的影响。  

由实验结果图发现，在hough变换参数不变的情况下，由于robert算子的边缘检测效果弱于sobel算子，sobel算子的边缘检测效果弱于canny算子，所以在直线检测中，以roberts算子为基础的直线检测结果更好，线段更贴近于图像原本就有的直线，而最为复杂的canny算子由于边缘检测结果更加精细复杂，因而在hough变换参数不变的情况下，直线检测具有更多干扰，于是实验结果较不理想。  

在边缘检测算法不变的情况下，改变hough变换参数，及改变极值点的个数，发现随着极值点的个数增加，实验结果图中的直线检测结果更多且更准确。  



