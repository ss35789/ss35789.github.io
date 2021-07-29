---
layout: single
title: "버블 정렬"
---

#Second


바로 옆자리의 숫자와 비교해 순서를 바꾸는 정렬(가장 비효율)

```c
#include <stdio.h>

int main(void){
  int array[10]={4,5,3,0,1,2,6,8,9,7};
  int i,j,tmp;
  for(i=0;i<10;i++){
    for(j=0;j<9-i;j++){
      if(array[j]>array[j+1]){
        tmp=array[j];
        array[j]=array[j+1];
        array[j+1]=tmp;//swap
      }
    
    }
  
  }
  
  for(i=0;i<10;i++){
    printf("%d",array[i]);
  }//
  return0;
}


```
