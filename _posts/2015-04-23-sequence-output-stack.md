---
layout: post
title: 根据入栈序列输出所有可能的出栈序列
categories: [algorithm]
tags: [Stack]
---

###Input：
------
4  
1 2 3 4

###Output:
-------
4 3 2 1   
3 4 2 1   
3 2 4 1   
3 2 1 4   
2 4 3 1   
2 3 4 1   
2 3 1 4   
2 1 4 3   
2 1 3 4   
1 4 3 2   
1 3 4 2   
1 3 2 4   
1 2 4 3   
1 2 3 4   
14  

**代码：**
{% highlight java %}
    public class Main {
        private static int number;

    	public static void main(String[] args) {
    		Scanner cin = new Scanner(System.in);
    		int T = cin.nextInt();
    		int[] A = new int[T];
    		int k = 0;
    		while (T-- > 0) {
    			A[k++] = cin.nextInt();
    		}
    		int[] count = { 0, 0 };// count[0]: 进栈的次数；count[1]:出栈的次数
    		ArrayList<Integer> stack = new ArrayList<Integer>();
    		func(stack, count, k, A);
    		System.out.println(number);
    		cin.close();
    	}
    
    	public static void func(List<Integer> stack, int[] count, int n, int[] A) {
    		// 1代表进栈；0代表出栈
    		if (count[0] < n) {
    			stack.add(1);
    			count[0]++;
    			func(stack, count, n, A);
    			count[0]--;
    			stack.remove(stack.size() - 1);
    		}
    
    		if (count[1] < count[0]) {
    			stack.add(0);
    			count[1]++;
    			func(stack, count, n, A);
    			count[1]--;
    			stack.remove(stack.size() - 1);
    		}
    
    		if (stack.size() == 2 * n) {
    			Iterator<Integer> it = stack.iterator();
    			Stack<Integer> stk = new Stack<Integer>();
    			int j = 0;
    			while (it.hasNext()) {
    				int out = it.next();
    				if (out == 1) {
    					stk.push(A[j++]);
    				} else {
    					System.out.print(stk.pop() + " ");
    				}
    			}
    			number++;
    			System.out.println();
    		}
    	}
    }
{% endhighlight %}

##变种：
---

有2n个人排成一队进入剧场。入场费5元。其中只有n个人有一张5元钞票，另外n人只有一张10元钞票，剧院无其它钞票可找零，问有多少种方法是的只要10元的人买票，售票处就有5元的钞票找零？

**解法**：
将持有5元的人买票视作5元入栈，持有10元的人买票视作栈中5元出栈。

**卡特兰数**：
-----

![QQ图片20150503145847.png][1]

C(2n, n)/(n+1)
--------------

  [1]: /album/2015/05/607658847.png
