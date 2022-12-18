# Problem Statement

Given a string, find the length of the longest palindromic subsequence in it.

e.g  
ABA  -> 3  (centered around B)   
ABC  -> 1  (each Character is a palindrome in itself)   
ABAXABAXABB -> 9 (centered around the second B)   

### Solutions 

We can of'ourse go with the brute force approach and  
iterate over all indices in the string and check the length of palindromes around it

or

### Use Dynamic Programming to solve it

Lets consider a string

|B|A|B|C|B|A|B|
|---|---|---|---|---|---|---|
|0|1|2|3|4|5|6|

We notice the length of the longest palindromic subsequence L(0,6) = 
* L(1,5) + 2,  IFF str[0] == str[6]
* max of L(0,5) and L(1,6) if str[0] != str[6]

Similarly, for L(1,5) =  
* L(2,4) + 2, IFF str[1] == str[5]
* max of L(1,4) and L(2,5) if str[1] != str[5]

and so on..

So each problem can be sub-divided into sub-problems.

Hence, it displays the two necessary properties to be solvable by dynamic programming
1. Overlapping subproblems
2. Optimal Substructure

Now lets solve this using Tabulation (Bottom Up approach) 

We also noticed above, that the for longest palindrome of length 6, we need to first find the palindromes of length 5 and so on

We will take two pointers i and j such that they represent two indices on the string

|B|A|B|C|B|A|B|
|---|---|---|---|---|---|---|
|i|j||||||

and for each value of i we have a row, and for each value of j we have a column   
and **we will move i and j, such that they are form string of length 1,2 and so on**


|i \ j| 0 | 1 | 2 | 3 | 4 | 5 | 6 |
|-----|---|---|---|---|---|---|---|
| 0   |   |   |   |   |   |   |   |
| 1   |   |   |   |   |   |   |   |
| 2   |   |   |   |   |   |   |   |
| 3   |   |   |   |   |   |   |   |
| 4   |   |   |   |   |   |   |   |
| 5   |   |   |   |   |   |   |   |
| 6   |   |   |   |   |   |   |   |


For length = 1 we have i=j and dp array as    

|i \ j| 0 | 1 | 2 | 3 | 4 | 5 | 6 |
|-----|---|---|---|---|---|---|---|
| 0   | 1 |   |   |   |   |   |   |
| 1   |   | 1 |   |   |   |   |   |
| 2   |   |   | 1 |   |   |   |   |
| 3   |   |   |   | 1 |   |   |   |
| 4   |   |   |   |   | 1 |   |   |
| 5   |   |   |   |   |   | 1 |   |
| 6   |   |   |   |   |   |   | 1 |

For length = 2 we have,
* dp[0][1] = max (dp[0][0], dp[1][1]) since str[0] != str[1]
* dp[1][2] = max (dp[1][1], dp[2][2]) since str[1] != str[2]  
and so on,

Hence dp array becomes
|i \ j| 0 | 1 | 2 | 3 | 4 | 5 | 6 |
|-----|---|---|---|---|---|---|---|
| 0   | 1 | 1 |   |   |   |   |   |
| 1   |   | 1 | 1 |   |   |   |   |
| 2   |   |   | 1 | 1 |   |   |   |
| 3   |   |   |   | 1 | 1 |   |   |
| 4   |   |   |   |   | 1 | 1 |   |
| 5   |   |   |   |   |   | 1 | 1 |
| 6   |   |   |   |   |   |   | 1 |

For length = 3 we have
* dp[0][2] = dp[1,1] + 2 since str[0] == str[2]
* dp[1][3] = max (dp[1][2], dp[2][3]) since str[1] != str[3] 
* dp[2][4] = max (dp[2][3], dp[3][4]) since str[2] == str[4] 

and so on,

Hence, dp array becomes  

|i \ j| 0 | 1 | 2 | 3 | 4 | 5 | 6 |
|-----|---|---|---|---|---|---|---|
| 0   | 1 | 1 | 1+2  |   |   |   |   |
| 1   |   | 1 | 1 | 1 |   |   |   |
| 2   |   |   | 1 | 1 | 1+2  |   |   |
| 3   |   |   |   | 1 | 1 | 1 |   |
| 4   |   |   |   |   | 1 | 1 | 1+2  |
| 5   |   |   |   |   |   | 1 | 1 |
| 6   |   |   |   |   |   |   | 1 |


Continuing on, we for length 4, dp array becomes

|i \ j| 0 | 1 | 2 | 3 | 4 | 5 | 6 |
|-----|---|---|---|---|---|---|---|
| 0   | 1 | 1 | 3 | 3 |   |   |   |
| 1   |   | 1 | 1 | 1 | 3 |   |   |
| 2   |   |   | 1 | 1 | 3 | 3 |   |
| 3   |   |   |   | 1 | 1 | 1 | 3 |
| 4   |   |   |   |   | 1 | 1 | 3 |
| 5   |   |   |   |   |   | 1 | 1 |
| 6   |   |   |   |   |   |   | 1 |

and for length 5, dp array becomes

|i \ j| 0 | 1 | 2 | 3 | 4 | 5 | 6 |
|-----|---|---|---|---|---|---|---|
| 0   | 1 | 1 | 3 | 3 | 3 |   |   |
| 1   |   | 1 | 1 | 1 | 3 | 5 |   |
| 2   |   |   | 1 | 1 | 3 | 3 | 3  |
| 3   |   |   |   | 1 | 1 | 1 | 3 |
| 4   |   |   |   |   | 1 | 1 | 3 |
| 5   |   |   |   |   |   | 1 | 1 |
| 6   |   |   |   |   |   |   | 1 |

and for length 6, dp array becomes

|i \ j| 0 | 1 | 2 | 3 | 4 | 5 | 6 |
|-----|---|---|---|---|---|---|---|
| 0   | 1 | 1 | 3 | 3 | 3 | 5 |   |
| 1   |   | 1 | 1 | 1 | 3 | 5 | 5 |
| 2   |   |   | 1 | 1 | 3 | 3 | 3 |
| 3   |   |   |   | 1 | 1 | 1 | 3 |
| 4   |   |   |   |   | 1 | 1 | 3 |
| 5   |   |   |   |   |   | 1 | 1 |
| 6   |   |   |   |   |   |   | 1 |



and finally for length 7, dp array becomes

|i \ j| 0 | 1 | 2 | 3 | 4 | 5 | 6 |
|-----|---|---|---|---|---|---|---|
| 0   | 1 | 1 | 3 | 3 | 3 | 5 | 7 |
| 1   |   | 1 | 1 | 1 | 3 | 5 | 5 |
| 2   |   |   | 1 | 1 | 3 | 3 | 3 |
| 3   |   |   |   | 1 | 1 | 1 | 3 |
| 4   |   |   |   |   | 1 | 1 | 3 |
| 5   |   |   |   |   |   | 1 | 1 |
| 6   |   |   |   |   |   |   | 1 |


Hence, the result is 7

Obviously, this solution is O(N^2) since we have 2 loops over i and j


### We can also use Manacher's Algorithm to solve it
https://en.wikipedia.org/wiki/Longest_palindromic_substring   
https://www.youtube.com/watch?v=V-sEwsca1ak