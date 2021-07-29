---
layout: single
title: "선택 정렬"
---

#First
<code>
#include <stdio.h>
int main(void){

  int i,j,index,tmp,min;
  
  int array[10]={1,4,6,3,7,2,0,9,8,5};
  
  for(i=0;i<10;i++){
    min=999;
    
    for(j=i;j<10;j++){
    
      if(array[j]<min){
        index=j;
        min=array[index];

      }
      
    }//가장 작은 값 비교해가며 위치찾기
    
    tmp=array[i];
    array[i]=array[index];
    array[index]=tmp; //swap
    
  }
  return 0;
}
                        </code>
