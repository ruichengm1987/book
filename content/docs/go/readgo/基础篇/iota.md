---
title: iota
weight: 2
---

# iota
* const (

    a = 1 << iota  // a==1 (iota == 0)  
    b = 1 << iota  // b==2 （iota == 1）  
    c = 3          // c == 3 (iota==2, unused)  
    d = 1 << iota  // d==8 (iota == 3)     
)
    
* const x = iota  // x==0
* const y = iota  // y==0
分开的const语句,iota每次都从0开始