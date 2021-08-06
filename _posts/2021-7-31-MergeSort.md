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
void merge(int *a,int m,int middle,int n){  //m은 시작인덱스, middle은 중간 인덱스, n은 끝인스

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
  } // i와 j를 시작으로 하는 a[]를 임의로 나눈 두 배열의 원소중 작은 것부터 array에 삽입
  
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
  //임의의 두 배열중 어느 한 쪽의 원소가 전부 삽입 되었다면 남은 임의의 배열의 원소들도 마저 삽입
  
    for(int i=m;i<=n;i++){
        a[i]=array[i];
    }//정렬한 배열(array[])을 원래 배열(a[])에 삽입
    
}//작게 나눈것들을 합치는 함수

void mergeSort(int *a,int s,int e){
    if(s<e){
        int middle=(s+e)/2;
        mergeSort(a,s,middle);
        mergeSort(a,middle+1,e);
        merge(a,s,middle,e);
    }
  
  
}// 재귀적으로 일단 임의로 두 배열로 나눈후 정렬하여 합침




int main(void){

  int arr[8]={2,4,5,6,7,1,8,3};
  mergeSort(arr,0,number-1);
  
  for(int i=0;i<number;i++){
    printf("%d",arr[i]);
  }


  return 0;
}


```
