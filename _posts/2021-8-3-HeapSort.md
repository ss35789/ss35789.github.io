---
layout:single
title:"힙 정렬"
---

#6

완전 이진트리를 힙구조로 바꾼이후 끝인덱스를 하나씩 root와 바꾸며 재귀적으로 힙구조를 만들어 정렬을 만듦<br>
배열에 트리를 넣어 이용<br>



```c
#include <stdio.h>

int main() {
    int number=8;
    int heap[8]={3,4,5,3,6,7,6,4};
    
    for(int i=1;i<number;i++){//  최대힙만들기
        int c=i;
        do{
            int root=(c-1)/2;
            
            if(heap[root]<heap[c]){//자손이 부모보다 크다면
                int tmp=heap[root];
                heap[root]=heap[c];
                heap[c]=tmp;
                //swap
            }
            
            c=root;
            
            
        }while(c!=0);
        
        
    }
    
    
    //힙정렬
    
    
    for(int i=number-1;i>=0;i--){
        int tmp=heap[i];
        heap[i]=heap[0];
        heap[0]=tmp;// root와 끝 수를 교환
        
        //int root=0;
        //int c=1;
        // do{
        //     c=root*2+1;
        //     if(heap[c]<heap[c+1]&& c<i-1){
        //         c++;
        //     }
            
        //     if(heap[root]<heap[c]&&c<i){
        //         int tmp=heap[root];
        //         heap[root]=heap[c];
        //         heap[c]=tmp;
        //     }
            
        //     root=c;
        // }while(c<i); // 위에서 아래로
        
        int c=i-1;
        do{
            int root=(c-1)/2;
            if(heap[root]<heap[c]){//자손이 부모보다 크다면
                int tmp=heap[root];
                heap[root]=heap[c];
                heap[c]=tmp;
                //swap
            }
            
            c=root;
        }while(c>0);
        
        //아래에서 위로
    }
    for(int i=0;i<number;i++){
        printf("%d ",heap[i]);
    }
    return 0;
}

```
