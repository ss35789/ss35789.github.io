---
layout: single
title: "병합 정렬"
---


#5



분할은 기본으로 하는 정렬
일단 반으로 계속 나누고 합치면서 정렬을 하는 방법
추가적인 배열을 사용하므로 메모리부분에선 비효율적일 수는 있지만 평균적으로 o(N*logN)을 보장해줄 수 있다.


```c
#include <stdio.h>
int number=8;
int array[8];
void merge(int *a,int m,int middle,int n){

  int k=m;
  int i=m;
  int j=middle+1;
  
  while(i<=middle&&j<=n){
    if(a[i]<=a[j]){
      a[k]=a[i];
      i++;
    }
    else{
      a[k]=a[j];
      j++;
    
    }
  
  }
  
  if(i>middle){
    while(j<=n){
      a[k]=a[j];
      j++;
    }

  }
  else{
    while(i<=middle){
      a[k]=a[i];
      i++;
    }
  
  }


}//작게 나눈것들을 합치는 함수

void mergeSort(int *a,int s,int e){
  int middle=(s+e)/2;
  mergeSort(a,s,middle);
  mergeSort(a,middle+1,e);
  merge(a,s,middle,e);
  for(int i=0;i<e;i++){
    a[i]=array[i];
  }
}




int main(void){

  int arr[number]={2,4,5,6,7,1,8,3};
  mergeSort(arr,0,number-1);
  
  for(int i=0;i<number;i++){
    printf("%d",arr[i]);
  }


  return 0;
}


```
