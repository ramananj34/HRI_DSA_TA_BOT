# Socratic TA — Problem 1
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
| Jumps straight into question | "Hey! Let me make sure I understand what you're asking..." *(go to DIAGNOSE)* |
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

1. **"Let's think small. If you only had one element in the array, what would you do?"**
   - If they say "take it" → "Okay, what if there were two elements?"
   - If they say "take it if it's positive" → "Good thinking. Now what about two elements?"
   - If confused → "There's just one number. Would you pick it or skip it?"

2. **"Now say you have three elements. You pick one — what does that prevent you from picking?"**
   - If they say "the neighbors" → "Right. So at each element, what decision are you making?"
   - If confused → "If you pick the first one, can you also pick the second?"

3. **"So at every index you have a choice: take it or skip it. Does that remind you of any technique from class?"**
   - If they say "DP" or "dynamic programming" → "Exactly. So what would your subproblem look like?" *(go to DP-SETUP)*
   - If they say "recursion" → "You're close. How could you avoid recomputing the same thing?" *(go to DP-SETUP)*
   - If they say "greedy" → *(go to GREEDY-WRONG)*
   - If still stuck → "Think about problems where you made a sequence of decisions and each one affected the next."

---

## GREEDY-WRONG
*Student is trying a greedy approach.*

1. **"Interesting idea. Can you walk me through what happens on a specific example? Try [3, 7, 2, 8, 1]."**
   - Let them work through it. Greedy would take 8 then 3 = 11, but optimal is 7 + 8 = 15 (or 3 + 8 = 11... actually let me use a better example). Use **[2, 10, 3, 12, 1]**. Greedy takes 12 then 2 = 14. Optimal is 10 + 12 = 22... wait those are adjacent. Use **[5, 1, 4, 2, 3]**. Greedy: take 5, skip 1, take 4, skip 2, take 3 = 12. Actually optimal: 5 + 4 + 3 = 12. Let me use **[2, 7, 9, 3, 1]**. Greedy takes 9, then 2 = 11. Optimal is 7 + 3 = 10 or 2 + 9 + 1 = 12. Yes. Greedy gives 11, optimal gives 12.
   - **Use this example: [2, 7, 9, 3, 1]**

2. **"What did greedy give you? Is there a combination that does better?"**
   - If they find the counterexample → "So what went wrong with greedy? Why didn't it work?"
   - If they can't find it → "What if you took the first, third, and fifth elements?"

3. **"When greedy picks the best-looking option right now, what is it not considering?"**
   - Looking for: "how the choice affects future choices"
   - If they get it → "So you need a way to account for how each choice affects the rest. What technique handles that?" *(go to DP-SETUP)*
   - If stuck → "Each decision you make changes what's available later. Is there a technique that handles sequences of decisions?" *(go to DP-SETUP)*

---

## DP-SETUP
*Student knows it's DP but doesn't know the subproblem or structure.*

1. **"What does the subproblem need to capture? After looking at the first i elements, what do you need to know?"**
   - If they say "the best sum so far" → "Good. Does it matter whether you took the i-th element or not?"
   - If they say "whether I took the last one" → "Exactly. Why does that matter for the next decision?"
   - If confused → "Think about it this way — when you're at element i, what do you need to know about what already happened?"

2. **"So if dp[i] is the best sum considering the first i elements, what are your two options at index i?"**
   - Looking for: skip i (take dp[i-1]) or take i (take dp[i-2] + arr[i])
   - If they say "take it or skip it" → "Right! If you skip it, what's the best you can do? If you take it, what do you need?"
   - If confused → "You said each element is either taken or skipped. If you skip element i, the answer is the same as what?" *(go to RECURRENCE-HELP)*

---

## RECURRENCE-HELP
*Student is trying to write the recurrence and struggling.*

1. **"Let's think about the two cases. If you DON'T take element i, then the best answer is... what?"**
   - Looking for: dp[i-1]
   - If they get it → "Good. Now if you DO take element i, what's the most recent element you're allowed to have also taken?"
   - If stuck → "If you skip element i, the problem is the same as the first i-1 elements, right?"

2. **"If you take element i, you can't take element i-1. So the best you can combine it with is... ?"**
   - Looking for: dp[i-2] + arr[i]
   - If they get it → "So what's dp[i]?"
   - If stuck → "You took i, you can't take i-1, so you jump back to the best answer through element... ?"

3. **"Now put the two cases together. What's dp[i]?"**
   - Looking for: dp[i] = max(dp[i-1], dp[i-2] + arr[i])
   - If they get it → "That's it. Now what are your base cases?" *(go to BASE-CASE)*
   - If close but wrong → "You're almost there. Remember, you want the BEST of the two options."

---

## BASE-CASE
*Student has the recurrence but not the base case.*

1. **"Your recurrence uses dp[i-1] and dp[i-2]. What's the smallest i where that works?"**
   - Looking for: i = 2, so we need dp[0] and dp[1]

2. **"If you only have one element, what's the best you can do?"**
   - Looking for: max(0, arr[0]) — take it if positive, otherwise take nothing
   - If they say just arr[0] → "What if that element is negative? Are you forced to take something?"
   - If they get it → "And dp[1]?"

3. **"What about dp[1] — you're looking at the first two elements, and you can only take one or neither."**
   - Looking for: max(dp[0], arr[1]) or max(0, arr[0], arr[1])
   - If they get it → "You've got the full solution. Want to trace through an example to check?" *(go to WRAP-UP)*

---

## NEGATIVES
*Student is confused about negative numbers.*

1. **"If every element in the array is negative, what should you pick?"**
   - Looking for: nothing / empty subset / sum of 0
   - If they say "the least negative" → "Are you required to pick at least one? Re-read the problem."
   - If they get it → "So your base case needs to account for picking nothing. How does that change dp[0]?"

---

## ANSWER-SEEKING
*Student is trying to get the answer directly.*

1. **"I want to help you get there yourself — it'll stick better that way. Let me ask you this instead..."** *(redirect to wherever they are in the flow)*

2. If persistent: **"Tell me what you've tried so far and where exactly you got stuck, and I'll ask you something that should help."**

3. If very persistent: **"I think it might be worth talking to the professor about this one. Want me to flag them?"** *(escalate to human TA)*

---

## DEFAULT
*Student said something not covered above.*

1. **"Can you say a bit more about what's tripping you up?"**
2. **"Let me make sure I understand — you're trying to [paraphrase]. Is that right?"**
3. If still unclear: **"Let's start from the beginning. When you look at this problem, what's your first instinct for how to solve it?"** *(go to DIAGNOSE)*
4. If totally off-script and you can't recover: **"Hmm, that's a good question. Let me get the professor to help with that one."** *(escalate to human TA)*

---

## WRAP-UP

| Student says | Robot responds |
|---|---|
| "I think I get it" / "That makes sense" | "Awesome! Try coding it up and come back if you get stuck again." |
| "Thanks" / "Thank you" | "No problem! Good luck, you've got this." |
| "I'm still confused" | "That's okay, this is a tough one. Want to trace through an example together, or would you rather talk to the professor?" |
| "Bye" / walks away | "Good luck! Come back anytime." |
