---
layout: post
title: 全排列算法（Java实现）
categories: [algorithm]
tags: [permutation]
---

设一组数p = {r1, r2, r3, ... ,rn}, 全排列为perm(p)，pn = p - {rn}。

因此perm(p) = r1perm(p1), r2perm(p2), r3perm(p3), ... , rnperm(pn)。当n = 1时perm(p} = r1。


**实现java代码如下：**
{% highlight java %}
    public class Permutate {
        public static int total = 0;
        public static void swap(String[] str, int i, int j)
        {
	        String temp = new String();
	        temp = str[i];
	        str[i] = str[j];
	        str[j] = temp;
        }
        public static void arrange (String[] str, int st, int len)
        {
                if (st == len - 1)
                {
                        for (int i = 0; i < len; i ++)
                        {
                                System.out.print(str[i]+ "  ");
                        }
                        System.out.println();
                        total++;
                }
                else
                {
                        for (int i = st; i < len; i ++)
                        {
                                swap(str, st, i);
                                arrange(str, st + 1, len);
                                swap(str, st, i);
                        }
                }
		
        }
	/**
	 * @param args
	 */
        public static void main(String[] args) {
                String str[] = {"a", "b", "c", "d", "e"};
                arrange(str, 0, str.length);
                System.out.println(total);
        }
    }
{% endhighlight %}
