# Direct TA — Problem 2
## Consecutive Pairs Maximum Sum

> **Problem:** Given an array of integers. You can choose pairs of consecutive elements to include in your sum, but no element can be used in more than one pair. Maximize the sum.
>
> **Correct approach:** DP. dp[0] = 0, dp[1] = 0, dp[i] = max(dp[i-1], dp[i-2] + arr[i-1] + arr[i-2]). (At each position, skip or pair with the previous element.)
>
> **Common wrong approach:** Greedy (take the highest-sum pair first, or sort pairs somehow)

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
| "I'm taking the biggest pairs first" / "I sorted the pairs" | **GREEDY-WRONG** |
| "I think it's DP but I don't know the subproblem" | **DP-SETUP** |
| "I have a recurrence but something's off" | **RECURRENCE-HELP** |
| "I don't know the base case" | **BASE-CASE** |
| "I'm confused about what counts as a pair" | **PAIR-CLARIFY** |
| "Can you just tell me the answer?" / clearly fishing | **ANSWER-SEEKING** |
| Something else / unclear | **DEFAULT** |

---

## START-STUCK
*Student has no idea where to begin.*

1. **"This is a DP problem. The reason is that picking a pair of consecutive elements uses both of them up, which affects which other pairs you can pick. That dependency between choices is what makes it DP rather than greedy."**

2. **"The way to think about it: scan the array from left to right. At each position, you have two options — skip this element, or pair it with the element right before it. If you pair them, both are consumed and you continue from two positions back."**
   - If they follow → "So the subproblem is: what's the best sum you can get from the first i elements?" *(go to DP-SETUP)*
   - If confused → "Think of it like you're walking along the array. At each step you either step forward one (skip) or grab two and jump forward two (pair). DP tracks the best outcome at each position."

---

## GREEDY-WRONG
*Student is trying a greedy approach.*

1. **"Greedy doesn't work here because picking the best single pair can block you from picking a better combination of pairs."**

2. **"Counterexample: [1, 5, 3, 4, 2]. Greedy picks the pair (5,3) = 8 because it has the highest sum. But that blocks you from using 5 or 3 in any other pair. The better answer is to pair (1,5) and (3,4) = 6 + 7 = 13."**

3. **"The fundamental issue is that pairs overlap — choosing one pair uses up elements that neighboring pairs need. Greedy doesn't account for these dependencies. DP does."**
   - If they accept → *(go to DP-SETUP)*
   - If they push back → "Run your approach on that example. If it gives 13, great. If not, the greedy is breaking down on the overlap constraint."

---

## DP-SETUP
*Student knows it's DP but doesn't know the subproblem.*

1. **"Define dp[i] as the maximum pair-sum you can get using elements from index 0 through i-1. You're scanning left to right, and at position i you have two choices."**

2. **"Choice 1: skip — you don't pair anything ending here, so dp[i] = dp[i-1]. Choice 2: pair elements i-1 and i-2 together, which uses both of them, so dp[i] = dp[i-2] + arr[i-1] + arr[i-2]."**
   - If they follow → "Take the max of those two cases and that's your recurrence." *(go to RECURRENCE-HELP)*
   - If confused about indexing → "dp[i] represents having processed i elements. The last two elements in that window are arr[i-1] and arr[i-2]. If you pair them, you add their values and fall back to dp[i-2] for everything before them."

---

## RECURRENCE-HELP
*Student is trying to write the recurrence and struggling.*

1. **"The recurrence is: dp[i] = max(dp[i-1], dp[i-2] + arr[i-1] + arr[i-2]). The first term is the skip case. The second term is the pair case."**

2. If their recurrence is close but wrong:
   - **Only adding one element in the pair case:** "A pair is two elements. You need to add both arr[i-1] and arr[i-2] when you take the pair."
   - **Using dp[i-1] in the pair case:** "If you pair elements i-1 and i-2, element i-1 is used — so you can't include dp[i-1]. You jump to dp[i-2]."
   - **Wrong indices on arr:** "Make sure your array indices match. If dp[i] covers i elements, the last element is arr[i-1] and the one before it is arr[i-2]."
   - **Missing the max:** "You want the better of skip vs pair. Wrap both cases in max()."

3. Once correct → "Now you need the base cases." *(go to BASE-CASE)*

---

## BASE-CASE
*Student has the recurrence but not the base case.*

1. **"Your recurrence uses dp[i-1] and dp[i-2], so you need dp[0] and dp[1] defined."**

2. **"dp[0] = 0. Zero elements means no pairs, so the sum is 0."**

3. **"dp[1] = 0. One element means you can't form a pair — you need at least two elements for that. So the sum is still 0."**

4. **"From dp[2] onward, use the recurrence. dp[2] = max(dp[1], dp[0] + arr[0] + arr[1]) = max(0, arr[0] + arr[1]). That's just deciding whether to pair the first two elements or not."**
   - If they follow → "That's the complete solution. dp[n] is your final answer." *(go to WRAP-UP)*
   - If confused → "Both base cases are 0 because you need at least two elements to form any pair. Once you have two, you start deciding."

---

## PAIR-CLARIFY
*Student is confused about what constitutes a valid pair.*

1. **"A pair means two elements that are next to each other in the array — indices i and i+1. You add their values together."**

2. **"The constraint is that each element can be in at most one pair. So if you pair indices 2 and 3, neither of those can appear in any other pair."**

3. **"You're picking a set of non-overlapping consecutive pairs to maximize the total sum. Elements that aren't in any pair are just ignored."**
   - Then redirect to wherever they were, or → *(go to DIAGNOSE)*

---

## ANSWER-SEEKING
*Student is trying to get the answer directly.*

1. **"I can help you work through it, but I'm not going to hand you the answer. Tell me what you've tried and I'll point you in the right direction."**

2. If persistent: **"Here's a concrete starting point: it's DP, and at each position you either skip or pair with the previous element. Build the recurrence from there."**

3. If very persistent: **"I think you should talk to the professor about this one. Want me to flag them?"** *(escalate to human TA)*

---

## DEFAULT
*Student said something not covered above.*

1. **"Can you say a bit more about where you're stuck?"**
2. **"Let me make sure I understand — you're trying to [paraphrase]. Is that right?"**
3. If still unclear: **"Here's the big picture: this is a DP problem. You scan left to right, and at each index you either skip or pair with the previous element. The recurrence tracks the best sum so far."** *(go to DIAGNOSE)*
4. If totally off-script and you can't recover: **"Good question — let me get the professor to help with that one."** *(escalate to human TA)*

---

## WRAP-UP

| Student says | Robot responds |
|---|---|
| "I think I get it" / "That makes sense" | "Great! Try coding it up and come back if anything breaks." |
| "Thanks" / "Thank you" | "No problem! Good luck with it." |
| "I'm still confused" | "That's okay, this one is tricky. Want me to go over any specific part again, or would you rather talk to the professor?" |
| "Bye" / walks away | "Good luck! Come back anytime." |
