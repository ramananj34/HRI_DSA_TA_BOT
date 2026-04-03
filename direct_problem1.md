# Direct TA — Problem 1
## Non-Adjacent Subset Maximum Sum

> **Problem:** Given an array of integers (positive or negative), pick a subset of elements such that no two chosen are adjacent, and maximize the sum.
>
> **Correct approach:** DP. dp[0] = max(0, arr[0]), dp[1] = max(dp[0], arr[1]), dp[i] = max(dp[i-1], dp[i-2] + nums[i])
>
> **Common wrong approach:** Greedy (sort, take largest available, skip neighbors)

---

## GREETING

| Student says | Robot responds |
|---|---|
| "Hi" / "Hey" / waves | "Hey! How's it going? What are you working on?" |
| Jumps straight into question | "Hey! Let me make sure I understand what you're saying..." *(go to DIAGNOSE)* |
| Small talk ("I'm good" / "tired" etc.) | "Glad you came over! So what's giving you trouble?" |

---

## DIAGNOSE — What is the student stuck on?

Listen to what they say and match to the closest entry below.

| Student says something like... | Go to → |
|---|---|
| "I don't know where to start" / "I'm lost" | **START-STUCK** |
| "I think I should sort it" / "I'm taking the biggest ones first" | **GREEDY-WRONG** |
| "I think it's DP but I don't know how to set it up" | **DP-SETUP** |
| "I have a recurrence but it's not working" | **RECURRENCE-HELP** |
| "I don't know what the base case is" | **BASE-CASE** |
| "What about negative numbers?" | **NEGATIVES** |
| "Can you just tell me the answer?" / clearly fishing | **ANSWER-SEEKING** |
| Something else / unclear | **DEFAULT** |

---

## START-STUCK
*Student has no idea where to begin.*

1. **"This is an optimization problem with a constraint — you're picking elements and no two can be next to each other. That means each element either gets picked or skipped, and your choice affects what's available next."**

2. **"When you have a sequence of decisions where each one depends on previous ones, that's a dynamic programming problem. The key insight here is that at each index, you either take it or skip it."**
   - If they seem to follow → "So you want to define a DP array where dp[i] represents the maximum sum you can get from the first i elements." *(go to DP-SETUP)*
   - If they seem confused → "Think of it this way: you walk through the array left to right. At each spot, you decide take or skip. If you take it, you have to skip the next one. DP lets you track the best outcome of those decisions."

---

## GREEDY-WRONG
*Student is trying a greedy approach.*

1. **"Greedy won't work here. The problem is that picking the locally best element can block you from a better combination later."**

2. **"Here's a quick counterexample: try [2, 7, 9, 3, 1]. A greedy approach that takes the largest first picks 9, then is forced to skip 7 and 3, so it takes 2 and maybe 1 — that's 12. But picking elements 1, 3, and 5 gives you 2 + 9 + 1 = 12 too, and picking 7 + 3 = 10 is worse. Actually, the optimal is 2 + 9 + 1 = 12. The point is greedy doesn't systematically find the best — it gets lucky sometimes and fails other times."**

3. **"The reason greedy fails is that each choice affects future choices. You need a technique that considers all possibilities efficiently. That's dynamic programming."**
   - If they accept → *(go to DP-SETUP)*
   - If they push back → "Try running your greedy on a few more examples. You'll find one where it gives the wrong answer. The underlying issue is that the constraint — no two adjacent — creates dependencies between choices that greedy ignores."

---

## DP-SETUP
*Student knows it's DP but doesn't know the subproblem or structure.*

1. **"Define dp[i] as the maximum sum you can achieve using only elements from index 0 through i. At each index i, there are exactly two cases: you skip element i, or you take element i."**

2. **"If you skip element i, then the best answer is just dp[i-1] — whatever was best without this element. If you take element i, then you can't use element i-1, so the best you can combine it with is dp[i-2]. You add arr[i] to that."**
   - If they follow → "So the recurrence is dp[i] = max(dp[i-1], dp[i-2] + arr[i]). That's the whole core of it." *(go to RECURRENCE-HELP if they need to work through it, or BASE-CASE)*
   - If confused → "Think of it in two buckets. Bucket 1: you pretend element i doesn't exist — best answer is dp[i-1]. Bucket 2: you include element i — then you need the best answer that doesn't use its neighbor, which is dp[i-2] plus arr[i]. You take whichever bucket is bigger."

---

## RECURRENCE-HELP
*Student is trying to write the recurrence and struggling.*

1. **"The recurrence has two parts. The 'skip' case gives you dp[i-1]. The 'take' case gives you dp[i-2] + arr[i]. You want the max of those two."**

2. **"So it's: dp[i] = max(dp[i-1], dp[i-2] + arr[i]). The first term is 'don't use index i,' the second is 'use index i and combine it with the best answer that ends at or before i-2.'"**

3. If their recurrence is close but wrong:
   - **Using dp[i-1] + arr[i]:** "That would mean you're taking element i right after element i-1 — but they're adjacent. You need to jump back to dp[i-2] to avoid that."
   - **Missing the max:** "You need to pick the better of the two options. Wrap both cases in a max."
   - **Other error:** "Let me restate it clearly: dp[i] = max(dp[i-1], dp[i-2] + arr[i]). Compare what you have to that."

4. Once they have it → "Now you need base cases." *(go to BASE-CASE)*

---

## BASE-CASE
*Student has the recurrence but not the base case.*

1. **"Your recurrence references dp[i-1] and dp[i-2], so you need to define dp[0] and dp[1] manually."**

2. **"dp[0] is the best sum from just the first element. You either take it or take nothing, so dp[0] = max(0, arr[0]). The 0 matters because if the element is negative, you're better off picking nothing."**

3. **"dp[1] is the best sum from the first two elements. You can take the first, take the second, or take neither. So dp[1] = max(dp[0], arr[1]). Or equivalently, max(0, arr[0], arr[1])."**
   - If they follow → "That's the complete solution. You fill the array left to right and dp[n-1] is your answer." *(go to WRAP-UP)*
   - If confused about the 0 → "The 0 handles the case where all elements are negative. You're not forced to pick anything — an empty subset with sum 0 is valid."

---

## NEGATIVES
*Student is confused about negative numbers.*

1. **"You're allowed to pick an empty subset. That means if everything is negative, the answer is 0 — you pick nothing."**

2. **"This affects your base case. dp[0] should be max(0, arr[0]), not just arr[0]. The 0 represents choosing nothing."**
   - If they follow → redirect to wherever they were before this came up
   - If confused → "Imagine arr = [-5, -3, -8]. The best move is to pick no elements. Your sum is 0. So the base case has to allow for that."

---

## ANSWER-SEEKING
*Student is trying to get the answer directly.*

1. **"I can point you in the right direction, but I'm not going to give you the final answer. Tell me where you're stuck and I'll help you get past that specific thing."**

2. If persistent: **"Here's what I can tell you: it's a DP problem, and at each index you either take or skip. Work from there and come back if you hit a wall."**

3. If very persistent: **"I think you should talk to the professor about this one. Want me to flag them?"** *(escalate to human TA)*

---

## DEFAULT
*Student said something not covered above.*

1. **"Can you say a bit more about where you're stuck?"**
2. **"Just so I'm on the same page — you're trying to [paraphrase]. Is that right?"**
3. If still unclear: **"Here's the high-level picture: this is a DP problem where at each index you decide to take or skip the element. The constraint is no two adjacent. Does that help frame it?"** *(go to DIAGNOSE)*
4. If totally off-script and you can't recover: **"That's a good question — let me get the professor to help with that one."** *(escalate to human TA)*

---

## WRAP-UP

| Student says | Robot responds |
|---|---|
| "I think I get it" / "That makes sense" | "Great! Try coding it up and come back if anything breaks." |
| "Thanks" / "Thank you" | "No problem! Good luck with it." |
| "I'm still confused" | "That's okay, this one is tricky. Want me to go over any part again, or would you rather talk to the professor?" |
| "Bye" / walks away | "Good luck! Come back anytime." |
