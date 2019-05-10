# A Point Cloud Registration Algorithm Based on Feature Extraction and Matching

https://www.hindawi.com/journals/mpe/2018/7352691/

기존 문제점 : 양이 늘어 나면 느리고 정확도 떨어짐. `The existing registration algorithms suffer from low precision and slow speed when registering a large amount of point cloud data. `

본 논문에서는 특징추출과 매칭을 통한 registration 알고리즘 제안 `In this paper, we propose a point cloud registration algorithm based on feature extraction and matching; the algorithm helps alleviate problems of precision and speed.`


절차 
- In the rough registration stage, 
    - the algorithm extracts feature points based on the judgment of retention points and bumps, which improves the speed of feature point extraction. 

- In the registration process, 
    - FPFH features and Hausdorff distance are used to search for corresponding point pairs, 
    - and the RANSAC algorithm is used to eliminate incorrect point pairs, thereby improving the accuracy of the corresponding relationship. 

- In the precise registration phase, 
    - the algorithm uses an improved normal distribution transformation (INDT) algorithm. 

실험 결과 속도와 정확도 좋음 `Experimental results show that given a large amount of point cloud data, this algorithm has advantages in both time and precision.`


## 1. Introduction

3D 포인트 클라우드 정합(Registration)은 중요하다. `The continuous development of three-dimensional (3D) scanning equipment has promoted the application of 3D point cloud reconstruction technology in the fields of mechanical manufacturing, medicine, robot navigation, and other industries. Registration technology plays an important role in 3D point cloud reconstruction. Due to the influence of the measuring equipment, object shape, and environment, a complete point cloud data model must be measured from multiple perspectives. It is an important step to integrate the point cloud in different coordinate systems into a unified coordinate system to obtain the complete data model of the object being tested.`

방대한양의 데이터를 속도와 정확도를 만족시키는 것이 중요한 연구 주제이다. `With a large amount of point cloud data, the existing registration algorithms either improve the registration accuracy at the cost of time or increase the registration speed. How to achieve a balance between time and accuracy when dealing with a large volume of cloud data is a problem worth studying. `

본 논문에서는 빠르고 정확도 높은 특징 추출/매칭 기반 정합 방법을 제안한다. `In this paper, a point cloud registration algorithm based on feature extraction and matching is proposed to improve the speed while maintaining the accuracy of the registration of a large amount of cloud data. `

제안 알고리즘 설명 
-  The algorithm uses the fast point feature histogram (FPFH) descriptor [1] and Hausdorff distance [2] to extract and locate the corresponding feature points in order to minimize the number of registration points, thereby improving its speed. 
- We propose an improved normal distribution transform (INDT) algorithm for precise registration and a mixed probability density function (PDF) to replace the single PDF, which further improves the accuracy of the algorithm.

논문의 구성 `The remainder of this article is organized as follows. `
- The research situation is introduced and analyzed in Section 2. 
- Section 3 introduces our rough registration algorithm. 
- Section 4 provides the improved algorithm of the normal distribution transformation. Section 5 verifies the speed and accuracy of the registration algorithm based on experimental simulation data.


## 2. Related Works

1960년대부터 시작된 연구로 2D에서 3D로 확장되고 있다. `The research on registration technology started in 1960, and it experienced a transition from the two-dimensional image registration to a 3D model registration. `

포인트 클라우드 정합 방법론 3가지가 있다. `Automatic registration is one of the three types of point cloud registration methods, which includes instrument-based and manual registration. `
- Automatic registration
- instrument-based registration
- manual registration

자동화 정합 방법론이 주 연구 분야 이다. `Automatic registration has greater flexibility and does not require human intervention and auxiliary equipment. Therefore, most researchers study automatic registration. `

정합은 두 파트로 나누어진다. `Point cloud registration technology mainly has two parts:`
- rough registration 
- and accurate registration. 


The registration algorithm in this paper is also composed of two parts, rough registration and precise registration, while registration of point clouds is mainly divided into rigid registration and nonrigid registration. 

연구는 강체 정합에 초점을 두고 있다. `The deformation of point clouds, which will directly affect the extraction of feature points, must be considered in nonrigid registration. This study focuses mainly on rigid registration.`

### 2.1. Initial Registration of Point Cloud

러프한 정합은 4가지 방법이 있다. `The four current types of rough registration of point clouds are based on the following:  `
- the measurement equipment;  
- human-computer interaction;  
- exhaustive thinking; and  
- features. 


첫 2개는 의존성/제약이 있어 마지막 2개가 주 연구 분야 이다. `The first two methods rely on the limitations of the auxiliary equipment and the need for manual intervention. The latter two constitute the main research direction.`


#### A. exhaustive thinking

분류 `The registration method based on exhaustive thinking can be divided into `
- methods of maximizing the point of rigid body transformation to obtain registration parameters and  
- finding the minimum transformation of the error function by traversing the transformation space quasi-parameter. 

These include the 
- voting method [3], 
- geometric hashing, 
- search transformation space method [4], 
- and RANSAC (RANdom SAmple Consensus) algorithm. 

##### 가. 기존 연구들 

###### 랜색 기반

In 1981, Fischler proposed the RANSAC algorithm, 
    - which is a method of fitting a mathematical model based on a sample. 

######  랜색 개선 

Meng [5] proposed the application of a sampling sphere and improved the original registration algorithm based on it; 
    - this method is based on the RANSAC idea. 
    - By setting up a sampling sphere model, the searching efficiency of the corresponding point pair is improved, and the time complexity of the algorithm is reduced. 


###### 특징 기반 

In 2015, Zhang Yonging [6] proposed a point cloud rough registration method based on the feature descriptors of a point feature histogram and FPFH, along with the principle of sampling consistency.

####  B. Feature 

특징 기반 방식은 특징쌍을 찾아 변환행렬을 추출하여 정합을 수행 한다. `The feature-based registration method obtains the coordinate transformation parameters by performing a corresponding point pair search on the overlapping regions of the point clouds, thereby completing the point cloud registration.` 

특징쌍을 찾을때 most similar geometry를 선택 한다. `When looking for a corresponding point, the method chooses the point with the most similar geometry.`

##### 나. 기존 연구들 

###### Chua [7] 

- In 2000, Chua [7] and others proposed the point signature (PS) method. 

- This method constructs a point signature through extensive calculation; 

- it is a complex process that is sensitive to noise.

###### Gelfand [8]

In 2005, through the unremitting efforts of Gelfand [8] and others, feature descriptors based on integral invariants were applied to the field of 3D registration, thereby ensuring the reliability of the registration results. 

###### Yan Jianfeng [9]

Yan Jianfeng [9] proposed a method of rough registration using the curvature of the point cloud to extract feature points and using similarity to find pairs of points; 

this shortened the time of registration, but the accuracy remained low. 


###### Yang Xiaoqing [10]

Yang Xiaoqing [10] improved the point cloud coarse registration algorithm based on the normal vector based on a point geometric feature extraction algorithm in 3D space. 

The key points are selected by comparing the characteristic degree of each point in the point cloud data (i.e., the angle between the normal points and adjacent points) and the threshold value. 

The main curvature is then calculated only for the key points, thereby increasing the speed of calculating the curvature and the efficiency of the corresponding point pair search. 

However, this method, facing the registration of a large volume of point cloud data, has a long running time or low precision. 

To solve this problem, we propose a rough point cloud registration algorithm to ensure a shorter running time and more accurate calculations.


### 2.2. Point Cloud Accurate Registration

ICP가 고전적인 알고리즘 이다. `The most classic point cloud accurate registration technology is the iterative closest point (ICP) algorithm. `

ICP는 단점이 있음, NDP추천 `However, because ICP is used to find the corresponding point using the nearest neighbor search, the operation is too long, and the initial position of the point cloud is too high, so the normal distributions transform (NDT) algorithm is the main precision Quasi-technical research.`

#### A. History 

- The NDT algorithm was proposed in 2003 by Biber et al. [11]. 

- In 2006, Martin Magnusson [12] summarized 2D-NDT and extended it to the registration of 3D data through 3D-NDT. 
    - Magnusson’s algorithm is faster than the current standard for 3D registration and is often more accurate. 

- In 2011, Cihan [13] proposed a multilayered normal distribution transformation algorithm called MLNDT. 
    - The algorithm divides a point cloud into 8n cells, where n is the number of layers, 
    - replaces the original Gaussian probability function with the Mahalanobis distance function as a fractional function, 
    - and uses the Newton and Levenberg-Marquardt (LM) optimization method to optimize the fractional function, with better registration speed than the previous NDT algorithm. 
    
- In 2013, Cihan [14] further explained the MLNDT algorithm. 

- In 2015, Hyunki Hong and B.H. Lee [15] proposed the key-layered normal distributions transform algorithm (KLNDT) based on the key layer normal distribution transformation; it has a good success rate and accuracy. 

- In 2016, Hyunki Hong and B.H. Lee [16] proposed to convert the reference point cloud into a disk-like distribution suitable for the point cloud structure to improve the registration accuracy of the NDT algorithm. 

- Zhang Xiao [17] performed a feature point search based on the speeded up robust features (SURF) algorithm, an improvement of the 3D-NDT algorithm. 

- Hu Xiuxiang [18] proposed the normal aligned radial feature (NARF) algorithm, which improved NDT. 

- Chen Qingyan [19, 20] study is based on curvature feature of NDT registration method; Zheng Fen [21] et al. 

- improved 3D scale invariant feature transform (3DSIFT) algorithm and 3D-NDT algorithm. 

- 위 여러 연구들은 모두 NDT알고리즘을 정확도와 속도 측면에서 향상 시키는 방안 들이다. `The above methods all improve the original NDT algorithm in different ways, with various accuracy and time advantages. `
    - 이둘은 항상 트레이드 오프다 `However, it has always been a challenge to obtain a balance between time and accuracy when registering large amounts of point cloud data. `

- 본 논문의 제안 `The registration algorithm in this paper improves the normal distribution transformation INDT algorithm in the accurate registration stage, improves the speed of the registration algorithm, and ensures good registration accuracy.`









