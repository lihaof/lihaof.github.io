---
layout: post
title: 素数筛法
subtitle: 
description:
author: 扬涛
header-img: 
category: []
tags: [Algorithm, Number Theroy]
---

### Eratosthenes筛法:  
先用第一个质数2去筛，2留下，把2的倍数剔除掉；再用下一个素数，也就是3筛，把3留下，把3的倍数剔除掉；接下去用下一个素数5筛，把5留下，把5的倍数剔除掉；不断重复下去......最后剩下来的都是质数。  

```  
//判断前N个质数  
bool IsPrime[N];
int prime[N/2],tot = 0;
memset(IsPrime,true,sizeof(IsPrime));
for (int i = 2;i <= N;i++)
	if (IsPrime[i]){
		prime[tot++] = i;
		for (int j = i*2;j <= N;j += i)
			IsPrime[j] = false;
		}
```  
*时间复杂度：O(NloglogN)*   



### Euler筛法  

```  
bool IsPrime[N];
int prime[N/2],tot = 0;
memset(IsPrime,true,sizeof(IsPrime));
for (int i = 2;i <= N;i++){
	if (IsPrime[i])
		prime[tot++] = i;
	for (int j = 0;j < tot;j++){
		if (i*prime[j] > N)//超过最大范围，跳出 
			break;
		IsPrime[i*prime[j]] = false;//将倍数筛除 
		if (i%prime[j] == 0)//保证只筛到以prime[j]为最小质因数的数 
			break;
	}
}
```  
*时间复杂度：O(N)*  

[文章来源](http://wenku.baidu.com/link?url=0YEYx5S3alaN_SxQX06YJ4BAbSZn0oKbskZMv8zdTnt1yZOTnX45Aes1c_AxtXW3uKAGSlZZ7c0j3lvzrN_77cHbJMT6Cb14GIRizTFlTyq)  

  
  

  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  


  [1]: http://www.vonlion.cn/myblog/img/game_site.JPG