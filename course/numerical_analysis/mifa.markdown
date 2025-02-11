# 数值分析大作业

**”数值分析“计算实习第三题**

<img src="https://aerozf.oss-cn-beijing.aliyuncs.com/images/numerical_analysis/timu.jpg" width="100%" height="100%" />

### 1 算法和设计方案

​		为解决”数值分析“计算实习第三题，该程序使用了**线形方程组求解的Doolittle分解法算法**、求解矩阵按模最大特征值的**幂法算法**、求解矩阵按模最小特征值的**反幂法算法**这三种数值求解算法。

#### 1.1 Doolittle分解法算法

​	线性方程组的Doolittle分解法算法如下图所示：

<img src="https://aerozf.oss-cn-beijing.aliyuncs.com/images/numerical_analysis/doolittle_-1.jpg" width="100%" height="100%" />

​	Doolittle算法主要用于反幂法中求解特征值向量。

#### 1.2 幂法算法

​	幂法算法用来求解矩阵的**按模最大特征值**（绝对值最大），其算法如下所示：

<img src="https://aerozf.oss-cn-beijing.aliyuncs.com/images/numerical_analysis/mifa.jpg" width="100%" height="100%" />

#### 1.3 反幂法算法

​	反幂法算法用来求解矩阵的**按模最小特征值**（绝对值最小），其算法如下所示：

<img src="https://aerozf.oss-cn-beijing.aliyuncs.com/images/numerical_analysis/fanmifa.jpg" width="100%" height="100%" />

#### 1.4 设计方案

​	程序逻辑和各个算法之间的调用如下图所示：

<img src="https://aerozf.oss-cn-beijing.aliyuncs.com/images/numerical_analysis/suanfajiegou.png" width="100%" height="100%" />



**以上就是”数值分析“计算实习第三题的算法和设计方案**



### 2 源程序

​	**以下是该程序的全部代码（基于C语言）**

```c
/*
个人信息
	学生：张凡
	学号：ZY1904332
	邮箱：zhangfan525@buaa.edu.cn
	完成时间：2019/10/20
程序功能
	完成数值分析计算实习第三大题问题的数值求解
*/

#include<stdio.h>
#include<stdlib.h>
#include<math.h>

#define N 1001
#define Msize 1000
#define eps 1.0e-7

/*定义函数*/
void DoolittleMain();	//Doolittle分解法求解线形方程组函数
double Max(double,double);//最大值函数
double Min(double,double);//最小值函数
double Power_1();		//幂法函数
double Power_2();		//反幂法函数
double dabs(double);	//绝对值函数
void sort_max_cond();	//最大条件数查找函数
void sort_min_cond();	//最小条件数查找函数

/*定义全局变量*/
double max_cond,min_cond,X[N],M[N][N],B[N],Cond[81],Lamda_M[81],Lamda_m[81];
int nk;

/* 主函数*/
void main()
{
	int i,j,k;
	double t,a,b;
	for(k=0;k<=80;k++)
	{
		t=0.05*k;
		for(i=1;i<=Msize;i++)
		{
			for(j=1;j<=Msize;j++)
			{
				if(i==j)
				{
					M[i][j]=(100*(i+0.1))/(i+t);
				}
				else if((Max(j-3,1)<= i && i <j) || (j< i && i<=Min(j+3,1000)))
				{
					M[i][j]=-1*(i+j)/(1+t);
				}
				else
				{
					M[i][j]=0;
				}
			}
		}
		printf("当k=%d时，t_ik= %f：\n",k,(double)k*0.05);
		a=Power_1();
		b=Power_2();
		Lamda_M[k]=a;
		Lamda_m[k]=b;
		printf("    条件数cond= %1.8e\n\n",dabs(a/b));
		Cond[k]=dabs(a/b);
	}
	sort_max_cond();
	printf("当 k= %d 时，t_ik= %f，条件数最大，cond_Max= %1.8e \n\n",nk,nk*0.05,max_cond);
	sort_min_cond();
	printf("    当 k= %d 时，t_ik= %f，条件数最小，cond_min= %1.8e \n",nk,nk*0.05,min_cond);

	for(i=0;i<=80;i++)
	{
		printf("k= %d 时，t_ik= %f， cond = %1.8e \n",i,i*0.05,Cond[i]);
	}
	for(i=0;i<=80;i++)
	{
		printf("k= %d 时，t_ik= %f，最大特征值Lamda_max = %1.8e \n",i,i*0.05,Lamda_M[i]);
	}
	system("pause");
}

/* Doolittle分解法求解线形方程组函数*/
void DoolittleMain()
{
        long  k, i, j,t ;
        double Y[N], temp, A[N][N];
		for(i=1;i<=Msize;i++)
		{
			for(j=1;j<=Msize;j++)
			{
				A[i][j]=M[i][j];
			}
		}
		for(k=1;k<=Msize;k++)
		{
			for(j=k;j<=Msize;j++)
			{
					temp = 0;
					for(t=1;t<=k-1;t++)
					{
						temp+=A[k][t]*A[t][j];
					}
					A[k][j]=M[k][j]-temp;
			}

			for(i=k+1;i<=Msize;i++)
			{
				if(k<Msize)
				{
					temp=0;
					for(t=1;t<=k-1;t++)
					{
						temp+=A[i][t]*A[t][k];
					}
					A[i][k]=(M[i][k]-temp)/A[k][k];
				}
			}
		}
		Y[1]=B[1];
		for(i=2;i<=Msize;i++)
		{
			temp=0;
			for(t=1;t<=i-1;t++)
			{
				temp+=A[i][t]*Y[t];
			}
			Y[i]=B[i]-temp;
		}
		X[Msize]=Y[Msize]/A[Msize][Msize];
		for(i=Msize-1;i>=1;i--)
		{
			temp=0;
			for(t=i+1;t<=Msize;t++)
			{
				temp+=A[i][t]*X[t];
			}
			X[i]=(Y[i]-temp)/A[i][i];
		}
}

/* 最大值函数*/
double Max(double x,double y)
{
	double ma;
	if(x>y) ma=x;
	else ma=y;
	return ma;
}

/*最小值函数 */
double Min(double x,double y)
{
	double mi;
	if(x<y) mi=x;
	else mi=y;
	return mi;
}

/* 绝对值函数*/
double dabs(double x)
{
	if(x>0)return x;
	else if(x<0) return -x;
	else return 0;
}

/* 幂法函数*/
double Power_1()
{
	double miu,beta1,beta2,y[N],temp,s;
	int i,j;
	beta2=0;
	temp=1;
	for(i=1;i<=Msize;i++)
	{
		X[i]=0.1;
	}
	while(temp>=eps)
	{
		s=0;
		for(i=1;i<=Msize;i++)
		{
			s=s+X[i]*X[i];
		}
		miu=sqrt(s);
		for(i=1;i<=Msize;i++)
		{
			y[i]=X[i]/miu;
		}
		for(i=1;i<=Msize;i++)
		{
			s=0;
			for(j=1;j<=Msize;j++)
			{
				s=s+M[i][j]*y[j];
			}
			X[i]=s;
		}
		beta1=beta2;
		s=0;
		for(i=1;i<=Msize;i++)
		{
			s+=y[i]*X[i];
		}
		beta2=s;
		temp=dabs(beta2-beta1)/dabs(beta2);
	}
	printf("    最大特征值 Max=%1.8e",beta2);
	return beta2;
}

/* 反幂法函数*/
double Power_2()
{
	double miu,beta1,beta2,temp,s;
	int i;
	beta2=1;
	temp=1;
	for(i=1;i<=Msize;i++)
	{
		X[i]=0.1;
	}
	while(temp>=eps)
	{
		s=0;
		for(i=1;i<=Msize;i++)
		{
			s=s+X[i]*X[i];
		}
		miu=sqrt(s);
		for(i=1;i<=Msize;i++)
		{
			B[i]=X[i]/miu;
		}
		DoolittleMain();
		beta1=beta2;
		s=0;
		for(i=1;i<=Msize;i++)
		{
			s+=B[i]*X[i];
		}
		beta2=1/s;
		temp=dabs(beta2-beta1)/dabs(beta2);
	}
	printf("    最小特征值 Min=%1.8e",beta2);
	return beta2;
}

/* 最大条件数查找函数*/
void sort_max_cond()
{
	int i;
	max_cond=Cond[0];
	for(i=0;i<=80;i++)
	{
		if(max_cond>Cond[i])
		{
			max_cond=max_cond;
		}
		else
		{
			max_cond=Cond[i];
			nk=i;
		}
	}
}

/*最小条件数查找函数 */
void sort_min_cond()
{
	int i;
	min_cond=Cond[0];
	for(i=0;i<=80;i++)
	{
		if(min_cond>Cond[i])
		{
			min_cond=Cond[i];
			nk=i;
		}
		else
		{
			min_cond=min_cond;
		}
	}
}
```



### 3 结果

#### 3.1 各小题答案

##### 3.1.1 第一小问答案

​		**当`k=3, t_ik=0.15`时，矩阵得到最大的条件数，为`cond=5.91626350e+005`**。

##### 3.1.2 第二小问答案

​		当`k=71, t_ik=3.55`时，矩阵得到最小的条件数，为`cond=1.33496465e+003`。

##### 3.1.3 第三小问答案

​	该矩阵系列在`k=0,...,80`时对应的最大特征值如下所示：

```c
k= 0 时，t_ik= 0.000000，最大特征值Lamda_max = -1.15485823e+004
k= 1 时，t_ik= 0.050000，最大特征值Lamda_max = -1.09938926e+004
k= 2 时，t_ik= 0.100000，最大特征值Lamda_max = -1.04896297e+004
k= 3 时，t_ik= 0.150000，最大特征值Lamda_max = -1.00292161e+004
k= 4 时，t_ik= 0.200000，最大特征值Lamda_max = -9.60716993e+003
k= 5 时，t_ik= 0.250000，最大特征值Lamda_max = -9.21888870e+003
k= 6 时，t_ik= 0.300000，最大特征值Lamda_max = -8.86047565e+003
k= 7 时，t_ik= 0.350000，最大特征值Lamda_max = -8.52861209e+003
k= 8 时，t_ik= 0.400000，最大特征值Lamda_max = -8.22045344e+003
k= 9 时，t_ik= 0.450000，最大特征值Lamda_max = -7.93354745e+003
k= 10 时，t_ik= 0.500000，最大特征值Lamda_max = -7.66576888e+003
k= 11 时，t_ik= 0.550000，最大特征值Lamda_max = -7.41526666e+003
k= 12 时，t_ik= 0.600000，最大特征值Lamda_max = -7.18042116e+003
k= 13 时，t_ik= 0.650000，最大特征值Lamda_max = -6.95980902e+003
k= 14 时，t_ik= 0.700000，最大特征值Lamda_max = -6.75217436e+003
k= 15 时，t_ik= 0.750000，最大特征值Lamda_max = -6.55640483e+003
k= 16 时，t_ik= 0.800000，最大特征值Lamda_max = -6.37151167e+003
k= 17 时，t_ik= 0.850000，最大特征值Lamda_max = -6.19661301e+003
k= 18 时，t_ik= 0.900000，最大特征值Lamda_max = -6.03091981e+003
k= 19 时，t_ik= 0.950000，最大特征值Lamda_max = -5.87372396e+003
k= 20 时，t_ik= 1.000000，最大特征值Lamda_max = -5.72438815e+003
k= 21 时，t_ik= 1.050000，最大特征值Lamda_max = -5.58233673e+003
k= 22 时，t_ik= 1.100000，最大特征值Lamda_max = -5.44705043e+003
k= 23 时，t_ik= 1.150000，最大特征值Lamda_max = -5.31805675e+003
k= 24 时，t_ik= 1.200000，最大特征值Lamda_max = -5.19492665e+003
k= 25 时，t_ik= 1.250000，最大特征值Lamda_max = -5.07726923e+003
k= 26 时，t_ik= 1.300000，最大特征值Lamda_max = -4.96472756e+003
k= 27 时，t_ik= 1.350000，最大特征值Lamda_max = -4.85697512e+003
k= 28 时，t_ik= 1.400000，最大特征值Lamda_max = -4.75371257e+003
k= 29 时，t_ik= 1.450000，最大特征值Lamda_max = -4.65466503e+003
k= 30 时，t_ik= 1.500000，最大特征值Lamda_max = -4.55957959e+003
k= 31 时，t_ik= 1.550000，最大特征值Lamda_max = -4.46822320e+003
k= 32 时，t_ik= 1.600000，最大特征值Lamda_max = -4.38038070e+003
k= 33 时，t_ik= 1.650000，最大特征值Lamda_max = -4.29585321e+003
k= 34 时，t_ik= 1.700000，最大特征值Lamda_max = -4.21445655e+003
k= 35 时，t_ik= 1.750000，最大特征值Lamda_max = -4.13601995e+003
k= 36 时，t_ik= 1.800000，最大特征值Lamda_max = -4.06038485e+003
k= 37 时，t_ik= 1.850000，最大特征值Lamda_max = -3.98740378e+003
k= 38 时，t_ik= 1.900000，最大特征值Lamda_max = -3.91693948e+003
k= 39 时，t_ik= 1.950000，最大特征值Lamda_max = -3.84886360e+003
k= 40 时，t_ik= 2.000000，最大特征值Lamda_max = -3.78305745e+003
k= 41 时，t_ik= 2.050000，最大特征值Lamda_max = -3.71940904e+003
k= 42 时，t_ik= 2.100000，最大特征值Lamda_max = -3.65781397e+003
k= 43 时，t_ik= 2.150000，最大特征值Lamda_max = -3.59817446e+003
k= 44 时，t_ik= 2.200000，最大特征值Lamda_max = -3.54039885e+003
k= 45 时，t_ik= 2.250000，最大特征值Lamda_max = -3.48440110e+003
k= 46 时，t_ik= 2.300000，最大特征值Lamda_max = -3.43010040e+003
k= 47 时，t_ik= 2.350000，最大特征值Lamda_max = -3.37742077e+003
k= 48 时，t_ik= 2.400000，最大特征值Lamda_max = -3.32629069e+003
k= 49 时，t_ik= 2.450000，最大特征值Lamda_max = -3.27664279e+003
k= 50 时，t_ik= 2.500000，最大特征值Lamda_max = -3.22841355e+003
k= 51 时，t_ik= 2.550000，最大特征值Lamda_max = -3.18154302e+003
k= 52 时，t_ik= 2.600000，最大特征值Lamda_max = -3.13597458e+003
k= 53 时，t_ik= 2.650000，最大特征值Lamda_max = -3.09165474e+003
k= 54 时，t_ik= 2.700000，最大特征值Lamda_max = -3.04853286e+003
k= 55 时，t_ik= 2.750000，最大特征值Lamda_max = -3.00656104e+003
k= 56 时，t_ik= 2.800000，最大特征值Lamda_max = -2.96569387e+003
k= 57 时，t_ik= 2.850000，最大特征值Lamda_max = -2.92588804e+003
k= 58 时，t_ik= 2.900000，最大特征值Lamda_max = -2.88710328e+003
k= 59 时，t_ik= 2.950000，最大特征值Lamda_max = -2.84930053e+003
k= 60 时，t_ik= 3.000000，最大特征值Lamda_max = -2.81244299e+003
k= 61 时，t_ik= 3.050000，最大特征值Lamda_max = -2.77649562e+003
k= 62 时，t_ik= 3.100000，最大特征值Lamda_max = -2.74142515e+003
k= 63 时，t_ik= 3.150000，最大特征值Lamda_max = -2.70719987e+003
k= 64 时，t_ik= 3.200000，最大特征值Lamda_max = -2.67378960e+003
k= 65 时，t_ik= 3.250000，最大特征值Lamda_max = -2.64116557e+003
k= 66 时，t_ik= 3.300000，最大特征值Lamda_max = -2.60930036e+003
k= 67 时，t_ik= 3.350000，最大特征值Lamda_max = -2.57816779e+003
k= 68 时，t_ik= 3.400000，最大特征值Lamda_max = -2.54774290e+003
k= 69 时，t_ik= 3.450000，最大特征值Lamda_max = -2.51800183e+003
k= 70 时，t_ik= 3.500000，最大特征值Lamda_max = -2.48892178e+003
k= 71 时，t_ik= 3.550000，最大特征值Lamda_max = -2.46048096e+003
k= 72 时，t_ik= 3.600000，最大特征值Lamda_max = -2.43265853e+003
k= 73 时，t_ik= 3.650000，最大特征值Lamda_max = -2.40543455e+003
k= 74 时，t_ik= 3.700000，最大特征值Lamda_max = -2.37878990e+003
k= 75 时，t_ik= 3.750000，最大特征值Lamda_max = -2.35270608e+003
k= 76 时，t_ik= 3.800000，最大特征值Lamda_max = -2.32716599e+003
k= 77 时，t_ik= 3.850000，最大特征值Lamda_max = -2.30215261e+003
k= 78 时，t_ik= 3.900000，最大特征值Lamda_max = -2.27764981e+003
k= 79 时，t_ik= 3.950000，最大特征值Lamda_max = -2.25364211e+003
k= 80 时，t_ik= 4.000000，最大特征值Lamda_max = -2.23011468e+003
```

##### 3.1.4 计算得到的全部的条件系数

​	该矩阵系列在`k=0,...,80`时对应的条件系数如下所示：

```c
k= 0 时，t_ik= 0.000000， cond = 4.75415976e+004
k= 1 时，t_ik= 0.050000， cond = 5.78346708e+003
k= 2 时，t_ik= 0.100000， cond = 9.46240187e+003
k= 3 时，t_ik= 0.150000， cond = 5.91626350e+005
k= 4 时，t_ik= 0.200000， cond = 1.10551910e+004
k= 5 时，t_ik= 0.250000， cond = 1.73267964e+004
k= 6 时，t_ik= 0.300000， cond = 6.23422326e+003
k= 7 时，t_ik= 0.350000， cond = 1.40154156e+005
k= 8 时，t_ik= 0.400000， cond = 1.64267463e+004
k= 9 时，t_ik= 0.450000， cond = 4.94718782e+003
k= 10 时，t_ik= 0.500000， cond = 5.02913595e+004
k= 11 时，t_ik= 0.550000， cond = 6.78091340e+003
k= 12 时，t_ik= 0.600000， cond = 1.40855295e+004
k= 13 时，t_ik= 0.650000， cond = 6.21196080e+003
k= 14 时，t_ik= 0.700000， cond = 9.25757807e+003
k= 15 时，t_ik= 0.750000， cond = 3.25825736e+003
k= 16 时，t_ik= 0.800000， cond = 1.00232077e+004
k= 17 时，t_ik= 0.850000， cond = 3.69503897e+003
k= 18 时，t_ik= 0.900000， cond = 2.08529450e+004
k= 19 时，t_ik= 0.950000， cond = 9.16623591e+003
k= 20 时，t_ik= 1.000000， cond = 3.26840520e+003
k= 21 时，t_ik= 1.050000， cond = 2.71895732e+004
k= 22 时，t_ik= 1.100000， cond = 9.48701738e+003
k= 23 时，t_ik= 1.150000， cond = 3.23515248e+003
k= 24 时，t_ik= 1.200000， cond = 1.90469457e+003
k= 25 时，t_ik= 1.250000， cond = 9.02089772e+003
k= 26 时，t_ik= 1.300000， cond = 3.30066161e+003
k= 27 时，t_ik= 1.350000， cond = 1.79371198e+003
k= 28 时，t_ik= 1.400000， cond = 6.96056954e+003
k= 29 时，t_ik= 1.450000， cond = 3.70560270e+003
k= 30 时，t_ik= 1.500000， cond = 1.63401012e+003
k= 31 时，t_ik= 1.550000， cond = 5.04041052e+003
k= 32 时，t_ik= 1.600000， cond = 4.65290305e+003
k= 33 时，t_ik= 1.650000， cond = 1.59247131e+003
k= 34 时，t_ik= 1.700000， cond = 3.64832393e+003
k= 35 时，t_ik= 1.750000， cond = 7.19085186e+003
k= 36 时，t_ik= 1.800000， cond = 1.81167130e+003
k= 37 时，t_ik= 1.850000， cond = 2.70073089e+003
k= 38 时，t_ik= 1.900000， cond = 2.33695353e+004
k= 39 时，t_ik= 1.950000， cond = 2.19479035e+003
k= 40 时，t_ik= 2.000000， cond = 2.05527215e+003
k= 41 时，t_ik= 2.050000， cond = 1.35953554e+004
k= 42 时，t_ik= 2.100000， cond = 2.94775607e+003
k= 43 时，t_ik= 2.150000， cond = 1.60600135e+003
k= 44 时，t_ik= 2.200000， cond = 4.76889806e+003
k= 45 时，t_ik= 2.250000， cond = 4.92370516e+003
k= 46 时，t_ik= 2.300000， cond = 1.62414874e+003
k= 47 时，t_ik= 2.350000， cond = 2.73679821e+003
k= 48 时，t_ik= 2.400000， cond = 2.10859890e+004
k= 49 时，t_ik= 2.450000， cond = 2.17354506e+003
k= 50 时，t_ik= 2.500000， cond = 1.85029882e+003
k= 51 时，t_ik= 2.550000， cond = 7.83766520e+003
k= 52 时，t_ik= 2.600000， cond = 3.50714395e+003
k= 53 时，t_ik= 2.650000， cond = 1.43326091e+003
k= 54 时，t_ik= 2.700000， cond = 3.10774170e+003
k= 55 时，t_ik= 2.750000， cond = 1.09850868e+004
k= 56 时，t_ik= 2.800000， cond = 1.98524624e+003
k= 57 时，t_ik= 2.850000， cond = 1.86917125e+003
k= 58 时，t_ik= 2.900000， cond = 8.19125408e+003
k= 59 时，t_ik= 2.950000， cond = 3.43968269e+003
k= 60 时，t_ik= 3.000000， cond = 1.42162529e+003
k= 61 时，t_ik= 3.050000， cond = 2.82415306e+003
k= 62 时，t_ik= 3.100000， cond = 1.69912962e+004
k= 63 时，t_ik= 3.150000， cond = 2.11998484e+003
k= 64 时，t_ik= 3.200000， cond = 1.65320906e+003
k= 65 时，t_ik= 3.250000， cond = 5.21392188e+003
k= 66 时，t_ik= 3.300000， cond = 4.52045485e+003
k= 67 时，t_ik= 3.350000， cond = 1.57692053e+003
k= 68 时，t_ik= 3.400000， cond = 2.16858004e+003
k= 69 时，t_ik= 3.450000， cond = 2.08557851e+004
k= 70 时，t_ik= 3.500000， cond = 2.73845974e+003
k= 71 时，t_ik= 3.550000， cond = 1.33496465e+003
k= 72 时，t_ik= 3.600000， cond = 2.97867120e+003
k= 73 时，t_ik= 3.650000， cond = 1.28882464e+004
k= 74 时，t_ik= 3.700000， cond = 2.03729408e+003
k= 75 时，t_ik= 3.750000， cond = 1.55754209e+003
k= 76 时，t_ik= 3.800000， cond = 4.37582473e+003
k= 77 时，t_ik= 3.850000， cond = 5.40715054e+003
k= 78 时，t_ik= 3.900000， cond = 1.67121286e+003
k= 79 时，t_ik= 3.950000， cond = 1.80787206e+003
k= 80 时，t_ik= 4.000000， cond = 7.17089099e+003
```

#### 3.2 程序运行结果

​		**如下是程序运行后在窗口打印出的运行结果：**

```c
当k=0时，t_ik= 0.000000：
    最大特征值 Max=-1.15485823e+004    最小特征值 Min=2.42915317e-001    条件数cond= 4.75415976e+004

当k=1时，t_ik= 0.050000：
    最大特征值 Max=-1.09938926e+004    最小特征值 Min=1.90091729e+000    条件数cond= 5.78346708e+003

当k=2时，t_ik= 0.100000：
    最大特征值 Max=-1.04896297e+004    最小特征值 Min=1.10855888e+000    条件数cond= 9.46240187e+003

当k=3时，t_ik= 0.150000：
    最大特征值 Max=-1.00292161e+004    最小特征值 Min=-1.69519430e-002    条件数cond= 5.91626350e+005

当k=4时，t_ik= 0.200000：
    最大特征值 Max=-9.60716993e+003    最小特征值 Min=-8.69018898e-001    条件数cond= 1.10551910e+004

当k=5时，t_ik= 0.250000：
    最大特征值 Max=-9.21888870e+003    最小特征值 Min=-5.32059618e-001    条件数cond= 1.73267964e+004

当k=6时，t_ik= 0.300000：
    最大特征值 Max=-8.86047565e+003    最小特征值 Min=1.42126377e+000    条件数cond= 6.23422326e+003

当k=7时，t_ik= 0.350000：
    最大特征值 Max=-8.52861209e+003    最小特征值 Min=-6.08516530e-002    条件数cond= 1.40154156e+005

当k=8时，t_ik= 0.400000：
    最大特征值 Max=-8.22045344e+003    最小特征值 Min=5.00431021e-001    条件数cond= 1.64267463e+004

当k=9时，t_ik= 0.450000：
    最大特征值 Max=-7.93354745e+003    最小特征值 Min=1.60364792e+000    条件数cond= 4.94718782e+003

当k=10时，t_ik= 0.500000：
    最大特征值 Max=-7.66576888e+003    最小特征值 Min=-1.52427156e-001    条件数cond= 5.02913595e+004

当k=11时，t_ik= 0.550000：
    最大特征值 Max=-7.41526666e+003    最小特征值 Min=1.09354983e+000    条件数cond= 6.78091340e+003

当k=12时，t_ik= 0.600000：
    最大特征值 Max=-7.18042116e+003    最小特征值 Min=5.09772895e-001    条件数cond= 1.40855295e+004

当k=13时，t_ik= 0.650000：
    最大特征值 Max=-6.95980902e+003    最小特征值 Min=-1.12038843e+000    条件数cond= 6.21196080e+003

当k=14时，t_ik= 0.700000：
    最大特征值 Max=-6.75217436e+003    最小特征值 Min=7.29367261e-001    条件数cond= 9.25757807e+003

当k=15时，t_ik= 0.750000：
    最大特征值 Max=-6.55640483e+003    最小特征值 Min=-2.01224277e+000    条件数cond= 3.25825736e+003

当k=16时，t_ik= 0.800000：
    最大特征值 Max=-6.37151167e+003    最小特征值 Min=6.35675911e-001    条件数cond= 1.00232077e+004

当k=17时，t_ik= 0.850000：
    最大特征值 Max=-6.19661301e+003    最小特征值 Min=1.67700884e+000    条件数cond= 3.69503897e+003

当k=18时，t_ik= 0.900000：
    最大特征值 Max=-6.03091981e+003    最小特征值 Min=-2.89211899e-001    条件数cond= 2.08529450e+004

当k=19时，t_ik= 0.950000：
    最大特征值 Max=-5.87372396e+003    最小特征值 Min=-6.40799999e-001    条件数cond= 9.16623591e+003

当k=20时，t_ik= 1.000000：
    最大特征值 Max=-5.72438815e+003    最小特征值 Min=1.75143160e+000    条件数cond= 3.26840520e+003

当k=21时，t_ik= 1.050000：
    最大特征值 Max=-5.58233673e+003    最小特征值 Min=-2.05311672e-001    条件数cond= 2.71895732e+004

当k=22时，t_ik= 1.100000：
    最大特征值 Max=-5.44705043e+003    最小特征值 Min=-5.74158370e-001    条件数cond= 9.48701738e+003

当k=23时，t_ik= 1.150000：
    最大特征值 Max=-5.31805675e+003    最小特征值 Min=1.64383496e+000    条件数cond= 3.23515248e+003

当k=24时，t_ik= 1.200000：
    最大特征值 Max=-5.19492665e+003    最小特征值 Min=-2.72743291e+000    条件数cond= 1.90469457e+003

当k=25时，t_ik= 1.250000：
    最大特征值 Max=-5.07726923e+003    最小特征值 Min=-5.62834142e-001    条件数cond= 9.02089772e+003

当k=26时，t_ik= 1.300000：
    最大特征值 Max=-4.96472756e+003    最小特征值 Min=1.50416133e+000    条件数cond= 3.30066161e+003

当k=27时，t_ik= 1.350000：
    最大特征值 Max=-4.85697512e+003    最小特征值 Min=-2.70777871e+000    条件数cond= 1.79371198e+003

当k=28时，t_ik= 1.400000：
    最大特征值 Max=-4.75371257e+003    最小特征值 Min=-6.82948794e-001    条件数cond= 6.96056954e+003

当k=29时，t_ik= 1.450000：
    最大特征值 Max=-4.65466503e+003    最小特征值 Min=1.25611551e+000    条件数cond= 3.70560270e+003

当k=30时，t_ik= 1.500000：
    最大特征值 Max=-4.55957959e+003    最小特征值 Min=-2.79042310e+000    条件数cond= 1.63401012e+003

当k=31时，t_ik= 1.550000：
    最大特征值 Max=-4.46822320e+003    最小特征值 Min=-8.86480015e-001    条件数cond= 5.04041052e+003

当k=32时，t_ik= 1.600000：
    最大特征值 Max=-4.38038070e+003    最小特征值 Min=9.41429609e-001    条件数cond= 4.65290305e+003

当k=33时，t_ik= 1.650000：
    最大特征值 Max=-4.29585321e+003    最小特征值 Min=2.69760163e+000    条件数cond= 1.59247131e+003

当k=34时，t_ik= 1.700000：
    最大特征值 Max=-4.21445655e+003    最小特征值 Min=-1.15517608e+000    条件数cond= 3.64832393e+003

当k=35时，t_ik= 1.750000：
    最大特征值 Max=-4.13601995e+003    最小特征值 Min=5.75178022e-001    条件数cond= 7.19085186e+003

当k=36时，t_ik= 1.800000：
    最大特征值 Max=-4.06038485e+003    最小特征值 Min=2.24123705e+000    条件数cond= 1.81167130e+003

当k=37时，t_ik= 1.850000：
    最大特征值 Max=-3.98740378e+003    最小特征值 Min=-1.47641655e+000    条件数cond= 2.70073089e+003

当k=38时，t_ik= 1.900000：
    最大特征值 Max=-3.91693948e+003    最小特征值 Min=1.67608788e-001    条件数cond= 2.33695353e+004

当k=39时，t_ik= 1.950000：
    最大特征值 Max=-3.84886360e+003    最小特征值 Min=1.75363611e+000    条件数cond= 2.19479035e+003

当k=40时，t_ik= 2.000000：
    最大特征值 Max=-3.78305745e+003    最小特征值 Min=-1.84066011e+000    条件数cond= 2.05527215e+003

当k=41时，t_ik= 2.050000：
    最大特征值 Max=-3.71940904e+003    最小特征值 Min=-2.73579390e-001    条件数cond= 1.35953554e+004

当k=42时，t_ik= 2.100000：
    最大特征值 Max=-3.65781397e+003    最小特征值 Min=1.24088082e+000    条件数cond= 2.94775607e+003

当k=43时，t_ik= 2.150000：
    最大特征值 Max=-3.59817446e+003    最小特征值 Min=-2.24045544e+000    条件数cond= 1.60600135e+003

当k=44时，t_ik= 2.200000：
    最大特征值 Max=-3.54039885e+003    最小特征值 Min=-7.42393485e-001    条件数cond= 4.76889806e+003

当k=45时，t_ik= 2.250000：
    最大特征值 Max=-3.48440110e+003    最小特征值 Min=7.07678665e-001    条件数cond= 4.92370516e+003

当k=46时，t_ik= 2.300000：
    最大特征值 Max=-3.43010040e+003    最小特征值 Min=2.11193736e+000    条件数cond= 1.62414874e+003

当k=47时，t_ik= 2.350000：
    最大特征值 Max=-3.37742077e+003    最小特征值 Min=-1.23407738e+000    条件数cond= 2.73679821e+003

当k=48时，t_ik= 2.400000：
    最大特征值 Max=-3.32629069e+003    最小特征值 Min=1.57748858e-001    条件数cond= 2.10859890e+004

当k=49时，t_ik= 2.450000：
    最大特征值 Max=-3.27664279e+003    最小特征值 Min=1.50751086e+000    条件数cond= 2.17354506e+003

当k=50时，t_ik= 2.500000：
    最大特征值 Max=-3.22841355e+003    最小特征值 Min=-1.74480657e+000    条件数cond= 1.85029882e+003

当k=51时，t_ik= 2.550000：
    最大特征值 Max=-3.18154302e+003    最小特征值 Min=-4.05929947e-001    条件数cond= 7.83766520e+003

当k=52时，t_ik= 2.600000：
    最大特征值 Max=-3.13597458e+003    最小特征值 Min=8.94167627e-001    条件数cond= 3.50714395e+003

当k=53时，t_ik= 2.650000：
    最大特征值 Max=-3.09165474e+003    最小特征值 Min=2.15707742e+000    条件数cond= 1.43326091e+003

当k=54时，t_ik= 2.700000：
    最大特征值 Max=-3.04853286e+003    最小特征值 Min=-9.80947953e-001    条件数cond= 3.10774170e+003

当k=55时，t_ik= 2.750000：
    最大特征值 Max=-3.00656104e+003    最小特征值 Min=2.73694793e-001    条件数cond= 1.09850868e+004

当k=56时，t_ik= 2.800000：
    最大特征值 Max=-2.96569387e+003    最小特征值 Min=1.49386701e+000    条件数cond= 1.98524624e+003

当k=57时，t_ik= 2.850000：
    最大特征值 Max=-2.92588804e+003    最小特征值 Min=-1.56533974e+000    条件数cond= 1.86917125e+003

当k=58时，t_ik= 2.900000：
    最大特征值 Max=-2.88710328e+003    最小特征值 Min=-3.52461693e-001    条件数cond= 8.19125408e+003

当k=59时，t_ik= 2.950000：
    最大特征值 Max=-2.84930053e+003    最小特征值 Min=8.28361449e-001    条件数cond= 3.43968269e+003

当k=60时，t_ik= 3.000000：
    最大特征值 Max=-2.81244299e+003    最小特征值 Min=1.97832931e+000    条件数cond= 1.42162529e+003

当k=61时，t_ik= 3.050000：
    最大特征值 Max=-2.77649562e+003    最小特征值 Min=-9.83125053e-001    条件数cond= 2.82415306e+003

当k=62时，t_ik= 3.100000：
    最大特征值 Max=-2.74142515e+003    最小特征值 Min=1.61342909e-001    条件数cond= 1.69912962e+004

当k=63时，t_ik= 3.150000：
    最大特征值 Max=-2.70719987e+003    最小特征值 Min=1.27699021e+000    条件数cond= 2.11998484e+003

当k=64时，t_ik= 3.200000：
    最大特征值 Max=-2.67378960e+003    最小特征值 Min=-1.61733302e+000    条件数cond= 1.65320906e+003

当k=65时，t_ik= 3.250000：
    最大特征值 Max=-2.64116557e+003    最小特征值 Min=-5.06560250e-001    条件数cond= 5.21392188e+003

当k=66时，t_ik= 3.300000：
    最大特征值 Max=-2.60930036e+003    最小特征值 Min=5.77220753e-001    条件数cond= 4.52045485e+003

当k=67时，t_ik= 3.350000：
    最大特征值 Max=-2.57816779e+003    最小特征值 Min=1.63493831e+000    条件数cond= 1.57692053e+003

当k=68时，t_ik= 3.400000：
    最大特征值 Max=-2.54774290e+003    最小特征值 Min=-1.17484384e+000    条件数cond= 2.16858004e+003

当k=69时，t_ik= 3.450000：
    最大特征值 Max=-2.51800183e+003    最小特征值 Min=-1.20733974e-001    条件数cond= 2.08557851e+004

当k=70时，t_ik= 3.500000：
    最大特征值 Max=-2.48892178e+003    最小特征值 Min=9.08876528e-001    条件数cond= 2.73845974e+003

当k=71时，t_ik= 3.550000：
    最大特征值 Max=-2.46048096e+003    最小特征值 Min=-1.84310570e+000    条件数cond= 1.33496465e+003

当k=72时，t_ik= 3.600000：
    最大特征值 Max=-2.43265853e+003    最小特征值 Min=-8.16692535e-001    条件数cond= 2.97867120e+003

当k=73时，t_ik= 3.650000：
    最大特征值 Max=-2.40543455e+003    最小特征值 Min=1.86637846e-001    条件数cond= 1.28882464e+004

当k=74时，t_ik= 3.700000：
    最大特征值 Max=-2.37878990e+003    最小特征值 Min=1.16762225e+000    条件数cond= 2.03729408e+003

当k=75时，t_ik= 3.750000：
    最大特征值 Max=-2.35270608e+003    最小特征值 Min=-1.51052488e+000    条件数cond= 1.55754209e+003

当k=76时，t_ik= 3.800000：
    最大特征值 Max=-2.32716599e+003    最小特征值 Min=-5.31823402e-001    条件数cond= 4.37582473e+003

当k=77时，t_ik= 3.850000：
    最大特征值 Max=-2.30215261e+003    最小特征值 Min=4.25760776e-001    条件数cond= 5.40715054e+003

当k=78时，t_ik= 3.900000：
    最大特征值 Max=-2.27764981e+003    最小特征值 Min=1.36287236e+000    条件数cond= 1.67121286e+003

当k=79时，t_ik= 3.950000：
    最大特征值 Max=-2.25364211e+003    最小特征值 Min=-1.24657169e+000    条件数cond= 1.80787206e+003

当k=80时，t_ik= 4.000000：
    最大特征值 Max=-2.23011468e+003    最小特征值 Min=-3.10995479e-001    条件数cond= 7.17089099e+003

当 k= 3 时，t_ik= 0.150000，条件数最大，cond_Max= 5.91626350e+005

当 k= 71 时，t_ik= 3.550000，条件数最小，cond_min= 1.33496465e+003
k= 0 时，t_ik= 0.000000， cond = 4.75415976e+004
k= 1 时，t_ik= 0.050000， cond = 5.78346708e+003
k= 2 时，t_ik= 0.100000， cond = 9.46240187e+003
k= 3 时，t_ik= 0.150000， cond = 5.91626350e+005
k= 4 时，t_ik= 0.200000， cond = 1.10551910e+004
k= 5 时，t_ik= 0.250000， cond = 1.73267964e+004
k= 6 时，t_ik= 0.300000， cond = 6.23422326e+003
k= 7 时，t_ik= 0.350000， cond = 1.40154156e+005
k= 8 时，t_ik= 0.400000， cond = 1.64267463e+004
k= 9 时，t_ik= 0.450000， cond = 4.94718782e+003
k= 10 时，t_ik= 0.500000， cond = 5.02913595e+004
k= 11 时，t_ik= 0.550000， cond = 6.78091340e+003
k= 12 时，t_ik= 0.600000， cond = 1.40855295e+004
k= 13 时，t_ik= 0.650000， cond = 6.21196080e+003
k= 14 时，t_ik= 0.700000， cond = 9.25757807e+003
k= 15 时，t_ik= 0.750000， cond = 3.25825736e+003
k= 16 时，t_ik= 0.800000， cond = 1.00232077e+004
k= 17 时，t_ik= 0.850000， cond = 3.69503897e+003
k= 18 时，t_ik= 0.900000， cond = 2.08529450e+004
k= 19 时，t_ik= 0.950000， cond = 9.16623591e+003
k= 20 时，t_ik= 1.000000， cond = 3.26840520e+003
k= 21 时，t_ik= 1.050000， cond = 2.71895732e+004
k= 22 时，t_ik= 1.100000， cond = 9.48701738e+003
k= 23 时，t_ik= 1.150000， cond = 3.23515248e+003
k= 24 时，t_ik= 1.200000， cond = 1.90469457e+003
k= 25 时，t_ik= 1.250000， cond = 9.02089772e+003
k= 26 时，t_ik= 1.300000， cond = 3.30066161e+003
k= 27 时，t_ik= 1.350000， cond = 1.79371198e+003
k= 28 时，t_ik= 1.400000， cond = 6.96056954e+003
k= 29 时，t_ik= 1.450000， cond = 3.70560270e+003
k= 30 时，t_ik= 1.500000， cond = 1.63401012e+003
k= 31 时，t_ik= 1.550000， cond = 5.04041052e+003
k= 32 时，t_ik= 1.600000， cond = 4.65290305e+003
k= 33 时，t_ik= 1.650000， cond = 1.59247131e+003
k= 34 时，t_ik= 1.700000， cond = 3.64832393e+003
k= 35 时，t_ik= 1.750000， cond = 7.19085186e+003
k= 36 时，t_ik= 1.800000， cond = 1.81167130e+003
k= 37 时，t_ik= 1.850000， cond = 2.70073089e+003
k= 38 时，t_ik= 1.900000， cond = 2.33695353e+004
k= 39 时，t_ik= 1.950000， cond = 2.19479035e+003
k= 40 时，t_ik= 2.000000， cond = 2.05527215e+003
k= 41 时，t_ik= 2.050000， cond = 1.35953554e+004
k= 42 时，t_ik= 2.100000， cond = 2.94775607e+003
k= 43 时，t_ik= 2.150000， cond = 1.60600135e+003
k= 44 时，t_ik= 2.200000， cond = 4.76889806e+003
k= 45 时，t_ik= 2.250000， cond = 4.92370516e+003
k= 46 时，t_ik= 2.300000， cond = 1.62414874e+003
k= 47 时，t_ik= 2.350000， cond = 2.73679821e+003
k= 48 时，t_ik= 2.400000， cond = 2.10859890e+004
k= 49 时，t_ik= 2.450000， cond = 2.17354506e+003
k= 50 时，t_ik= 2.500000， cond = 1.85029882e+003
k= 51 时，t_ik= 2.550000， cond = 7.83766520e+003
k= 52 时，t_ik= 2.600000， cond = 3.50714395e+003
k= 53 时，t_ik= 2.650000， cond = 1.43326091e+003
k= 54 时，t_ik= 2.700000， cond = 3.10774170e+003
k= 55 时，t_ik= 2.750000， cond = 1.09850868e+004
k= 56 时，t_ik= 2.800000， cond = 1.98524624e+003
k= 57 时，t_ik= 2.850000， cond = 1.86917125e+003
k= 58 时，t_ik= 2.900000， cond = 8.19125408e+003
k= 59 时，t_ik= 2.950000， cond = 3.43968269e+003
k= 60 时，t_ik= 3.000000， cond = 1.42162529e+003
k= 61 时，t_ik= 3.050000， cond = 2.82415306e+003
k= 62 时，t_ik= 3.100000， cond = 1.69912962e+004
k= 63 时，t_ik= 3.150000， cond = 2.11998484e+003
k= 64 时，t_ik= 3.200000， cond = 1.65320906e+003
k= 65 时，t_ik= 3.250000， cond = 5.21392188e+003
k= 66 时，t_ik= 3.300000， cond = 4.52045485e+003
k= 67 时，t_ik= 3.350000， cond = 1.57692053e+003
k= 68 时，t_ik= 3.400000， cond = 2.16858004e+003
k= 69 时，t_ik= 3.450000， cond = 2.08557851e+004
k= 70 时，t_ik= 3.500000， cond = 2.73845974e+003
k= 71 时，t_ik= 3.550000， cond = 1.33496465e+003
k= 72 时，t_ik= 3.600000， cond = 2.97867120e+003
k= 73 时，t_ik= 3.650000， cond = 1.28882464e+004
k= 74 时，t_ik= 3.700000， cond = 2.03729408e+003
k= 75 时，t_ik= 3.750000， cond = 1.55754209e+003
k= 76 时，t_ik= 3.800000， cond = 4.37582473e+003
k= 77 时，t_ik= 3.850000， cond = 5.40715054e+003
k= 78 时，t_ik= 3.900000， cond = 1.67121286e+003
k= 79 时，t_ik= 3.950000， cond = 1.80787206e+003
k= 80 时，t_ik= 4.000000， cond = 7.17089099e+003
k= 0 时，t_ik= 0.000000，最大特征值Lamda_max = -1.15485823e+004
k= 1 时，t_ik= 0.050000，最大特征值Lamda_max = -1.09938926e+004
k= 2 时，t_ik= 0.100000，最大特征值Lamda_max = -1.04896297e+004
k= 3 时，t_ik= 0.150000，最大特征值Lamda_max = -1.00292161e+004
k= 4 时，t_ik= 0.200000，最大特征值Lamda_max = -9.60716993e+003
k= 5 时，t_ik= 0.250000，最大特征值Lamda_max = -9.21888870e+003
k= 6 时，t_ik= 0.300000，最大特征值Lamda_max = -8.86047565e+003
k= 7 时，t_ik= 0.350000，最大特征值Lamda_max = -8.52861209e+003
k= 8 时，t_ik= 0.400000，最大特征值Lamda_max = -8.22045344e+003
k= 9 时，t_ik= 0.450000，最大特征值Lamda_max = -7.93354745e+003
k= 10 时，t_ik= 0.500000，最大特征值Lamda_max = -7.66576888e+003
k= 11 时，t_ik= 0.550000，最大特征值Lamda_max = -7.41526666e+003
k= 12 时，t_ik= 0.600000，最大特征值Lamda_max = -7.18042116e+003
k= 13 时，t_ik= 0.650000，最大特征值Lamda_max = -6.95980902e+003
k= 14 时，t_ik= 0.700000，最大特征值Lamda_max = -6.75217436e+003
k= 15 时，t_ik= 0.750000，最大特征值Lamda_max = -6.55640483e+003
k= 16 时，t_ik= 0.800000，最大特征值Lamda_max = -6.37151167e+003
k= 17 时，t_ik= 0.850000，最大特征值Lamda_max = -6.19661301e+003
k= 18 时，t_ik= 0.900000，最大特征值Lamda_max = -6.03091981e+003
k= 19 时，t_ik= 0.950000，最大特征值Lamda_max = -5.87372396e+003
k= 20 时，t_ik= 1.000000，最大特征值Lamda_max = -5.72438815e+003
k= 21 时，t_ik= 1.050000，最大特征值Lamda_max = -5.58233673e+003
k= 22 时，t_ik= 1.100000，最大特征值Lamda_max = -5.44705043e+003
k= 23 时，t_ik= 1.150000，最大特征值Lamda_max = -5.31805675e+003
k= 24 时，t_ik= 1.200000，最大特征值Lamda_max = -5.19492665e+003
k= 25 时，t_ik= 1.250000，最大特征值Lamda_max = -5.07726923e+003
k= 26 时，t_ik= 1.300000，最大特征值Lamda_max = -4.96472756e+003
k= 27 时，t_ik= 1.350000，最大特征值Lamda_max = -4.85697512e+003
k= 28 时，t_ik= 1.400000，最大特征值Lamda_max = -4.75371257e+003
k= 29 时，t_ik= 1.450000，最大特征值Lamda_max = -4.65466503e+003
k= 30 时，t_ik= 1.500000，最大特征值Lamda_max = -4.55957959e+003
k= 31 时，t_ik= 1.550000，最大特征值Lamda_max = -4.46822320e+003
k= 32 时，t_ik= 1.600000，最大特征值Lamda_max = -4.38038070e+003
k= 33 时，t_ik= 1.650000，最大特征值Lamda_max = -4.29585321e+003
k= 34 时，t_ik= 1.700000，最大特征值Lamda_max = -4.21445655e+003
k= 35 时，t_ik= 1.750000，最大特征值Lamda_max = -4.13601995e+003
k= 36 时，t_ik= 1.800000，最大特征值Lamda_max = -4.06038485e+003
k= 37 时，t_ik= 1.850000，最大特征值Lamda_max = -3.98740378e+003
k= 38 时，t_ik= 1.900000，最大特征值Lamda_max = -3.91693948e+003
k= 39 时，t_ik= 1.950000，最大特征值Lamda_max = -3.84886360e+003
k= 40 时，t_ik= 2.000000，最大特征值Lamda_max = -3.78305745e+003
k= 41 时，t_ik= 2.050000，最大特征值Lamda_max = -3.71940904e+003
k= 42 时，t_ik= 2.100000，最大特征值Lamda_max = -3.65781397e+003
k= 43 时，t_ik= 2.150000，最大特征值Lamda_max = -3.59817446e+003
k= 44 时，t_ik= 2.200000，最大特征值Lamda_max = -3.54039885e+003
k= 45 时，t_ik= 2.250000，最大特征值Lamda_max = -3.48440110e+003
k= 46 时，t_ik= 2.300000，最大特征值Lamda_max = -3.43010040e+003
k= 47 时，t_ik= 2.350000，最大特征值Lamda_max = -3.37742077e+003
k= 48 时，t_ik= 2.400000，最大特征值Lamda_max = -3.32629069e+003
k= 49 时，t_ik= 2.450000，最大特征值Lamda_max = -3.27664279e+003
k= 50 时，t_ik= 2.500000，最大特征值Lamda_max = -3.22841355e+003
k= 51 时，t_ik= 2.550000，最大特征值Lamda_max = -3.18154302e+003
k= 52 时，t_ik= 2.600000，最大特征值Lamda_max = -3.13597458e+003
k= 53 时，t_ik= 2.650000，最大特征值Lamda_max = -3.09165474e+003
k= 54 时，t_ik= 2.700000，最大特征值Lamda_max = -3.04853286e+003
k= 55 时，t_ik= 2.750000，最大特征值Lamda_max = -3.00656104e+003
k= 56 时，t_ik= 2.800000，最大特征值Lamda_max = -2.96569387e+003
k= 57 时，t_ik= 2.850000，最大特征值Lamda_max = -2.92588804e+003
k= 58 时，t_ik= 2.900000，最大特征值Lamda_max = -2.88710328e+003
k= 59 时，t_ik= 2.950000，最大特征值Lamda_max = -2.84930053e+003
k= 60 时，t_ik= 3.000000，最大特征值Lamda_max = -2.81244299e+003
k= 61 时，t_ik= 3.050000，最大特征值Lamda_max = -2.77649562e+003
k= 62 时，t_ik= 3.100000，最大特征值Lamda_max = -2.74142515e+003
k= 63 时，t_ik= 3.150000，最大特征值Lamda_max = -2.70719987e+003
k= 64 时，t_ik= 3.200000，最大特征值Lamda_max = -2.67378960e+003
k= 65 时，t_ik= 3.250000，最大特征值Lamda_max = -2.64116557e+003
k= 66 时，t_ik= 3.300000，最大特征值Lamda_max = -2.60930036e+003
k= 67 时，t_ik= 3.350000，最大特征值Lamda_max = -2.57816779e+003
k= 68 时，t_ik= 3.400000，最大特征值Lamda_max = -2.54774290e+003
k= 69 时，t_ik= 3.450000，最大特征值Lamda_max = -2.51800183e+003
k= 70 时，t_ik= 3.500000，最大特征值Lamda_max = -2.48892178e+003
k= 71 时，t_ik= 3.550000，最大特征值Lamda_max = -2.46048096e+003
k= 72 时，t_ik= 3.600000，最大特征值Lamda_max = -2.43265853e+003
k= 73 时，t_ik= 3.650000，最大特征值Lamda_max = -2.40543455e+003
k= 74 时，t_ik= 3.700000，最大特征值Lamda_max = -2.37878990e+003
k= 75 时，t_ik= 3.750000，最大特征值Lamda_max = -2.35270608e+003
k= 76 时，t_ik= 3.800000，最大特征值Lamda_max = -2.32716599e+003
k= 77 时，t_ik= 3.850000，最大特征值Lamda_max = -2.30215261e+003
k= 78 时，t_ik= 3.900000，最大特征值Lamda_max = -2.27764981e+003
k= 79 时，t_ik= 3.950000，最大特征值Lamda_max = -2.25364211e+003
k= 80 时，t_ik= 4.000000，最大特征值Lamda_max = -2.23011468e+003
```



### 4 讨论分析

##### 4.1 程序是如何实现幂法和反幂法等矩阵矩阵运算的？

​	答：在C语言中，是通过一维数组和二维数组来实现幂法和反幂法等矩阵矩阵运算的，在开始运算时，把相关的初值储存在数组中，通过for循环遍历和四则运算来实现矩阵和向量的运算，最后再把结果储存在数组中。

##### 4.2 线性方程组计算中为什么要使用Doolittle算法，而不使用别的算法？

​	答：我在之前用vb.NET分析过列主元Gauss算法、Doolittle算法、Crout算法、列主元Doolittle算法计算相同阶数随机矩阵的耗时，分析结果显示Doolittle算法计算速度最快（Crout算法耗时和Doolittle算法耗时接近），列主元Gauss算法最慢。综合考虑，选用了Doolittle算法。

​	备注：关于耗时分析报告，请参考网址：http://zhangfan525.top/course/numerical_analysis/Linear_equation_analysis/

##### 4.3 在编程过程中遇到了那些困难？是如何解决的？

​	答：之前用习惯了python、vb.NET、matlab等相对更加高级的语言后，用C语言编写程序是各种不适应，例如函数中数组参数传递的问题，在别的高级语言中，数组可以直接通过形参来传递，而在C语言中这样是行不通的，不过有两种方法可供选择：通过全局变量传质、通过指针传值，最后选择用全局变量传值解决的数组参数传递的问题。当然了还有一些零零散散的小bug，例如粗心打错字母了、累加中初值没清零、for循环中循环条件不小心打错了等。

##### 4.4 幂法和反幂法在求解过程中有什么区别？

​	答：幂法和反幂法其实在本质上没啥区别，反幂法相对于幂法由于多了一步线性方程组求解，所以反幂法的耗时要远大于幂法。程序的主要耗时就在反幂法求解过程中。

##### 4.5 有何心得

​	答：通过这次编程，对数值分析中的算法有了更加深入的了解，同时也理解了如何在计算机中实现相应的算法和矩阵运算等，获益匪浅。抖个机灵：编程挺有趣的，就是有点费头发 )：)：)：。





​																													张凡

​																												ZY1904332
