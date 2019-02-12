# ref
```
[1]: https://blog.csdn.net/starstar1992/article/details/54913261  
[2]: https://www.cnblogs.com/zhangtianq/p/5839909.html
```

# code
```
int KmpSearch(char* s, char* p)  
{  
    int i = 0;  
    int j = 0;  
    int sLen = strlen(s);  
    int pLen = strlen(p);  
    while (i < sLen && j < pLen)  
    {  
        //①如果j = -1，或者当前字符匹配成功（即S[i] == P[j]），都令i++，j++      
        if (j == -1 || s[i] == p[j])  
        {  
            i++;  
            j++;  
        }  
        else  
        {  
            //②如果j != -1，且当前字符匹配失败（即S[i] != P[j]），则令 i 不变，j = next[j]      
            //next[j]即为j所对应的next值        
            j = next[j];  
        }  
    }  
    if (j == pLen)  
        return i - j;  
    else  
        return -1;  
}  
```

# constant
  from = 'abcbcxgbcbcy'
  to = 'bcbcy'
  
# mv `from` to match `to`
## generic and KMP
1. ①
```
for i in range(len(from)):
  for j in range(len(to)):
    until_if_not_match(from[i], to[j])

# find match part
a b c b c x g b c b c y
  b c b c y
  |-----|
```

2. ② next
```
# generic action
a b c b c x g b c b c y
    b c b c y

# KMP
```
a: 1 2 3 4 5
b: 1 2 3 6
1:  1 = 1
    2 = 2
    3 = 3
    4 != 6
2:  2 ? 1
    3 ? 2  ==> 23=12 ? (4?3):(step2)
    4 ? 3      234=123 ? (5?6):(step2)
    5 ? 6      ...
3: ...

==> 
# find max equal length part of （
  b c b c 's tail
  b c b c 's prefix
）== find max equal length part of to[:i]
  b c b c x
      b c b c y
 as we use it repeat, get it firstly (which is done by `get_next`)
```

## mv `to` to match `from`
### improve KMP: BM
```
a b a b c b c x g b c b c y
b c b c y
        c == y:
        <Y> find next not match?
      b != c : max(align, move a to next pos), 
      None: done
        <N> c in a? align, move a to next pos
  b c b c y
          
```
 
### Sunday算法
```
a b a b c b c x g b c b c y
b c b c y
          b in a:
          <Y> align
      b c b c y
          <N> move a to next pos
            b c b c y
```

 
 
 
