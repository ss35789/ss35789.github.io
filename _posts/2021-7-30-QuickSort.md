---
layout: single
title: "퀵 정렬"
---



#4


분할을 베이스로 하는 정렬 O(N*logN)

기준이 되는 피벗값에 따라 O(N*N) 을 갖을 수도 있음

```c
#include <stdio.h>
int n=10;
int array[10]={4,5,3,6,2,0,1,7,9,8};


void quickSort(int *data, int start,int end){
    if(start>=end)return;//원소가 하나인 경우
    
    int i=start+1;
    int j=end;
    int tmp;
    int key=start;
    
  while(i<=j){
    
    
    while(data[i]<=data[key]){
      i++;
    }
    while(data[j]>=data[key] && j>start){
      j--;
    }
    if(i>j){
      tmp=data[j];
      data[j]=data[key];
      data[key]=tmp;
      
    }//엇갈린 경우
    else{
      tmp=data[i];
      data[i]=data[j];
      data[j]=tmp;
    
    }//엇갈리지 않은 경우
  }
  quickSort(data,0,j-1);
  quickSort(data,j+1,end);
}


int main(void){
  quickSort(array,0,n);
  
  for(int i=0;i<10;i++){
    printf("%d",array[i]);
  }


  return 0;
}

```
