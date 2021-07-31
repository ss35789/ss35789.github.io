---
layout: single
title: "병합 정렬"
---


#5



분할은 기본으로 하는 정렬<br>
일단 반으로 계속 나누고 합치면서 정렬을 하는 방법  <br>
추가적인 배열을 사용하므로 메모리부분에선 비효율적일 수는 있지만 평균적으로 o(N*logN)을 보장해줄 수 있다.<br>


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
      array[k]=a[i];
      i++;
    }
    else{
      array[k]=a[j];
      j++;
    
    }
    k++;
  }
  
  if(i>middle){
    while(j<=n){
      array[k]=a[j];
      j++;
      k++;
    }

  }
  else{
    while(i<=middle){
      array[k]=a[i];
      i++;
      k++;
    }
  
  }
  //남은 데이터들도 삽입
  
    for(int i=m;i<=n;i++){
        a[i]=array[i];
    }//정렬한 배열을 원래 배열에 삽입
    
}//작게 나눈것들을 합치는 함수

void mergeSort(int *a,int s,int e){
    if(s<e){
        int middle=(s+e)/2;
        mergeSort(a,s,middle);
        mergeSort(a,middle+1,e);
        merge(a,s,middle,e);
    }
  
  
}




int main(void){

  int arr[8]={2,4,5,6,7,1,8,3};
  mergeSort(arr,0,number-1);
  
  for(int i=0;i<number;i++){
    printf("%d",arr[i]);
  }


  return 0;
}


```
