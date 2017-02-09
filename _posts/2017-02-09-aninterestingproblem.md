---
layout: post
title: Feb. 9th, An Interesting Problem
tags: [diary]
feature:
  background-image: ac.jpg
---

I came across a very interesting, or at least worthy of being dug into, problem on Leetcode, [Problem 32. Longest Valid Parentheses](https://leetcode.com/problems/longest-valid-parentheses/). Cannot wait to share it.

I suppose some of you have already done it, though it is at a level of hard. According to the problem description, we should write a problem so that we could find the length of longest valid parentheses. The difficult part of the problem lies in the time cost. It is rather easy to think of using stack to solve the problem, however, I tried something else.

At first, I tried to scan the whole string once to get the answer without using stack. It is feasible that, from the head, every time you get a '(', you start to track the numbers of '(' and ')'. If the number of '(' is smaller than the other, you should stop and update the answer. This could pass the TLE (time limit exceed) examination, but it just beat 5% of the submission, which is probably not accepted by most of the programmers. The main reason is this algorithm does too much repeated work on counting the numbers. To ensure it does not miss any correct answer, every '(' should be examined, even if it has been counted before (for example, in s = "(((())", we have already scanned s[2] when examining s[0] and s[1]).

~~~ cpp
int longestValidParentheses(string s) {
    int left = 0;
    int right = 0;
    int max = 0;
    for(int i = 0;i<s.size();i++){
        if(s[i]=='('){
            left = 0;
            right = 0;
            int p = i;
            while(p<=s.size()-(left-right)){
                if(s[p]=='(') left++;
                else if(s[p]==')') right++;
                if(left<=right){
                    if(left*2>max) max = left*2;
                    if(left<right) break;
                }
                p++;
            }
        }
    }
    return max;
}
~~~

To avoid the repeated work, I wanted to make every examination to produce an independent result, that is, the examination result of each '(' would somehow be recorded or directly be updated to the answer. So I tried to iteratively call an independent function, which would evaluate the parentheses matching and return a value representing the result of certain part of the string. At the same time, the position point would move forward, no need to move backward.

~~~ cpp
int f(int &p, int &max, string s){
    int c = 0;
    p++;
    while(p<s.size()){
        if(s[p]=='('){
            int x = f(p, max, s);
            if(x==-1) return -1;
            else{
                c+=x;
                if(c>max) max = c;
            }
        }
        else{
            c+=2;
            p++;
            if(c>max) max = c;
            return c;
        }
    }
    return -1;
}

int longestValidParentheses(string s){
    int p = 0;
    int max = 0;
    int cur = 0;
    while(p<s.size()){
        if(s[p]=='('){
            int x = f(p, max, s);
            if(x==-1){
                cur = 0;
            }
            else{
                cur+=x;
                if(cur>max) max = cur;
            }
        }
        else{
            p++;
            cur = 0;
        }
    }
    return max;
}
~~~

In this implementation, if several continuous parts are all valid matching, we add the length up to get the possible answer. However, this pass all but one test cases, which is a long string with too many '(' in the head, so this algorithm leads to MLE (memory limit exceed, really rare to me).

Then I am thinking of using dynamic programming to solve the problem. The tricky part is that when you consider the transition equation from dp(i, j) to dp(i-1, j+1) (using dp(x, y) to represent longest valid parentheses length from x to y), there are two cases. One of them is s[i-1] and s[j+1] is '(' and ')' with taking i to j as a whole. The other is dp(i-1, j+1) can be a combination of dp(i-1, k) and dp(k+1, j+1). But not surprisingly, the simple implementation led to TLE. Pruning is certainly necessary. Sadly, I cannot find too many regulations for pruning.

~~~ cpp
int longestValidParentheses(string s){
    int** dp = new int*[s.size()];
    for(int i = 0;i<s.size();i++) dp[i] = new int[s.size()];
    int max = 0;
    for(int d = 1;d<s.size();d+=2){
        for(int i = 0;i<s.size()-d;i++){
            if(d==1){
                if(s[i]=='(' && s[i+d]==')'){
                    dp[i][i+d] = 2;
                    max = 2;
                }
                else{
                    dp[i][i+d] = 0;
                }
            }
            else{
                dp[i][i+d] = 0;
                if(s[i]=='(' && s[i+d]==')'){
                    if(dp[i+1][i+d-1]!=0 && dp[i+1][i+d-1]+2>dp[i][i+d]){
                        dp[i][i+d] = dp[i+1][i+d-1]+2;
                    }
                    for(int j = i+1;j<i+d-1;j+=2){
                        if(dp[i][j]!=0 && dp[j+1][i+d]!=0 && dp[i][j]+dp[j+1][i+d]>dp[i][i+d]){
                            dp[i][i+d] = dp[i][j]+dp[j+1][i+d];
                            if(dp[i][i+d]==d+1) break;
                        }
                    }
                    if(dp[i][i+d]>max) max = dp[i][i+d];
                }
            }
        }
    }
    return max;
}
~~~

After two days of intermittent work on this problem, I guess the best way to solve if is still stack. Every time it is not valid, push the index, then we could find the longest length using the indices in the stack.

There is solution of dynamic programming on the website, which is rather amazing. You could go and see yourself if interested. Leave a comment if you find any optimization!

I also took my examination of driving on the road yesterday morning, so glad to pass (only around 5 passed out of 17 in my group)!
