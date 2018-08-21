---
layout: post
title:  "Count Positon Number"
categories: KeyPoint
tags: KeyPoint
author: goodGid
---
* content
{:toc}


## Problem
Problem URL : **[반복 수열](https://www.acmicpc.net/problem/2331)**

{% capture images %}
    /assets/img/algorithm/2331_1.png
    /assets/img/algorithm/2331_2.png
{% endcapture %}
{% include gallery images=images caption="Screenshots of Problem Explain" cols=2 %}

---


## The Key Point

    자릿수 구하기 

``` cpp

#include <iostream>
#include <cmath>
using namespace std;

int CntPosNumber(int value){
        int pos_number;
        // 방법 1. 로그함수를이용한자리수계산
        pos_number = (int)log10((double)value)+1;
        cout << pos_number << endl;
        
        // 방법 2. 직접계산
        pos_number = 0;
        while (value != 0)
        {
            value /= 10;
            pos_number++;
        }
        
        cout << pos_number << endl;
    
    return pos_number;
}


int main(){
    int value;
    cin >> value;
    CntPosNumber(value);
    
    return 1;
}


```

---


## Code
``` cpp

#include <iostream>
#include <cstring>
#include <cmath>

using namespace std;

int arr[300005];
int cnt;

int main(){
    int a,p;
    cin >> a >> p ;
    
    while (1) {
        if( arr[a] == 2 ) break;
        arr[a]++;
        int number = 0;
        int a_number = a ;
        
        // 방법 1. 로그함수를이용한자리수계산
        number = (int)log10((double)a)+1;
        
        /*
         // 방법 2. 직접계산
         while (a_number != 0)
         {
         a_number /= 10;
         number++;
         }
         */
        
        a_number = a;
        
        int value;
        int tmp = 0;
        for(int i=number-1; i>=0; i--){
            value = a_number / pow(10, i);
            
            tmp += pow(value,p);
            
            a_number = a_number - ( value * pow( 10 , i ) ) ;
        }
        
        a = tmp ;
    }
    
    for(int i=1; i<300005; i++){
        if ( arr[i] == 1) cnt ++;
    }
    
    printf("%d\n",cnt);
    
    return 1; 
}

```
