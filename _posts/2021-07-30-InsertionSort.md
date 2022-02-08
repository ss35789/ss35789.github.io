---
layout: single
title: "삽입 정렬"
---

#Third


A그룹과 B의 그룹으로 나누어 B그룹에서 앞에서 부터 정렬되있는 A그룹으로 정렬하면서 이동 
A그룹은 이미 정렬이 되어 있기 때문에 B->A 의 과정에서만 정렬하면 된다.



```c
#include <stdio.h>

int main(void){
  int array[10]={4,6,3,1,0,8,9,7,5,2};
  int i,j,tmp;
  
  for(i=1;i<10;i++){
    for(j=i-1;j>=0;j--){
      if(array[j]<array[j+1]){
        break;
      }
      tmp=array[j];
      array[j]=array[j+1];
      array[j+1]=tmp;//swap
    }
  
  }


    for(i=0;i<10;i++){
        printf("%d",array[i]);
    }
return 0;
}



```
출처 : https://blog.naver.com/ndb796/221226806398
