# Max Profit with k Transactions

You are given an array of integers representing the prices of a single stock on various days (each index in the array represents a different day). You are also given an integer k, which represents the number of transactions you are allowed to make. One transaction consists of buying the stock on a given day and selling it on another, later day. 

Find the maximum profit that you can make buying and selling the stock, given k transactions. Note that you can only hold 1 share of the stock at a time; in other words, you cannot buy more than 1 share of the stock on any given day, and you cannot buy a share of the stock if you are still holding another share. Note that you also don't need to use all k transactions that you're allowed.

**e.g.**

Input: [5, 11, 3, 50, 60, 90], k = 2

Output: 93 (Buy: 5, Sell: 11; Buy: 3, Sell: 90)


# Related Question
https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/


## Dynamic Programming Implementation

```java
public static int maxProfitWithKTransactions(int[] prices, int k) {
	if(prices == null || prices.length == 0 || k == 0){
            return 0;
        }
	int[][] dpTable = new int[k+1][prices.length];
	    
	for(int i = 1; i <= k; i++) {
	    for(int j = 1; j < prices.length; j++) {
	    	  int profit = Integer.MIN_VALUE;
	    	  for(int x = 0; x < j; x++) {
	    		profit = Math.max(profit, prices[j] - prices[x] + dpTable[i-1][x]) ;
	    	  }
	    	  dpTable[i][j] = Math.max(dpTable[i][j-1], profit);
	    }
	}
	    
      return dpTable[k][prices.length-1];
}
```

Above dynamic programming implementation have runtime complexity of O(n<sup>2</sup>k) and space complexity of O(nk)

```
Time Complexity  = O(n^2 k)
Space Complexity = O(nk)
```

## Dynamic Programming Implementation - Runtime Optimization O(nk)

```java
public static int maxProfitWithKTransactions(int[] prices, int k) {
	if(prices == null || prices.length == 0 || k == 0){
            return 0;
        }
	int[][] dpTable = new int[k+1][prices.length];
	for(int i = 1; i <= k; i++) {
	    int profit = Integer.MIN_VALUE;
	    int x = 0;
	    for(int j = 1; j < prices.length; j++) {
	    	  profit = Math.max(profit,  - prices[x] + dpTable[i-1][x]) ;
	    	  dpTable[i][j] = Math.max(dpTable[i][j-1], profit + prices[j]);
	    	  x++;
	    }
        }
	    
     return dpTable[k][prices.length-1];
}
```

Above dynamic programming implementation have both runtime and space complexity of O(nk)

```
Time Complexity  = O(nk)
Space Complexity = O(nk)
```

## Dynamic Programming Implementation - Space Optimization O(n)

```java
public static int maxProfitWithKTransactions(int[] prices, int k) {
	if(prices == null || prices.length == 0 || k == 0){
            return 0;
        }
		
	int[] currentProfits = new int[prices.length];
	int[] previousProfits = new int[prices.length];
	    
	for(int i = 1; i <= k; i++) {
	    int profit = Integer.MIN_VALUE;
	    int x = 0;
	    for(int j = 1; j < prices.length; j++) {
	    	 profit = Math.max(profit,  - prices[x] + previousProfits[x]) ;
	    	 currentProfits[j] = Math.max(currentProfits[j-1], profit + prices[j]);
	    	 x++;
	    }
	    previousProfits = Arrays.copyOf(currentProfits, prices.length);
	    Arrays.fill(currentProfits, 0);
	}
	    
     return previousProfits[prices.length-1];
}
```

Above dynamic programming implementation have runtime complexity of O(nk) and space complexity of O(n)

```
Time Complexity  = O(nk)
Space Complexity = O(n)
```
