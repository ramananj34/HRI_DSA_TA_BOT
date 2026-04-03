# Socratic TA — Problem 2
## Consecutive Pairs Maximum Sum

> **Problem:** Given an array of integers. You can choose pairs of consecutive elements to include in your sum, but no element can be used in more than one pair. Maximize the sum.
>
> **Correct approach:** DP. dp[0] = 0, dp[1] = 0, dp[i] = max(dp[i-1], dp[i-2] + arr[i-1] + arr[i-2]). (Indexing may vary; the key is: at each position, skip or pair with the previous element.)
>
> **Common wrong approach:** Greedy (take the highest-sum pair first, or sort pairs somehow)

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

1. **"Let's start simple. If you have an array of just two elements, what can you do?"**
   - If they say "pair them" → "Right, you can pair them or not. What if you had three elements?"
   - If confused → "You can either pair those two together and take their sum, or take nothing. Those are your only options."

2. **"With three elements, say [a, b, c], what are your options for pairing?"**
   - Looking for: pair (a,b) and leave c, or pair (b,c) and leave a, or pair nothing
   - If they list them → "Notice that pairing (a,b) means b is used up, so you can't also pair (b,c). What does that remind you of?"
   - If stuck → "You could pair the first two, or the last two, but not both — because b would be in two pairs. Does that constraint feel familiar?"

3. **"So choosing one pair can prevent you from choosing another. Each element you process, you're deciding — do I pair it with the one before, or skip? Does that kind of decision-making remind you of anything?"**
   - If they say "DP" → "Exactly!" *(go to DP-SETUP)*
   - If they say "greedy" → *(go to GREEDY-WRONG)*
   - If stuck → "Think about problems where you're scanning through a sequence and making a choice at each step that affects what you can do next."

---

## GREEDY-WRONG
*Student is trying a greedy approach.*

1. **"Let's test that. Can you try your approach on [1, 5, 3, 4, 2]?"**
   - Greedy might take the pair (5,3) = 8 first (largest sum pair), which blocks pairing (1,5) or (3,4). So greedy gets 8. But optimal: pair (1,5) and (3,4) = 6 + 7 = 13. Or pair (5,3) and leave the rest = 8. So optimal is (1,5) + (3,4) = 13.

2. **"What did greedy give you? Now look at all possible sets of non-overlapping pairs. Is there a better combination?"**
   - If they find it → "So what went wrong with greedy?"
   - If they can't → "What if you paired (1,5) and (3,4) instead?"

3. **"When greedy picks the single best pair, what is it not thinking about?"**
   - Looking for: how that choice blocks other pairs
   - If they get it → "Right. So you need a method that considers the downstream effects of each choice. What technique does that?" *(go to DP-SETUP)*
   - If stuck → "Each pairing decision affects which other pairs are available. Sound like a DP problem?" *(go to DP-SETUP)*

---

## DP-SETUP
*Student knows it's DP but doesn't know the subproblem.*

1. **"Let's think about scanning the array from left to right. When you're at element i, what are your options?"**
   - Looking for: skip element i, or pair element i with element i-1
   - If they say "pair it with the next one" → "That works too, but it's easier to think about pairing with the PREVIOUS one. Can you see why?"
   - If they say "take it or skip it" → "Close — but you're not taking single elements. You're taking pairs. So what does 'take' mean at position i?"

2. **"If you pair element i with element i-1, those two are both used up. So where does your subproblem continue from?"**
   - Looking for: the subproblem on elements 0 through i-2
   - If they get it → "And if you skip element i, the subproblem is...?"
   - If stuck → "Both i and i-1 are consumed by the pair. So the rest of the problem is everything before index...?"

3. **"So you have two choices: skip (subproblem is first i-1 elements) or pair (subproblem is first i-2 elements plus the pair sum). How would you write that as a recurrence?"**
   - *(go to RECURRENCE-HELP)*

---

## RECURRENCE-HELP
*Student is trying to write the recurrence and struggling.*

1. **"Let's define dp[i] as the max sum using elements 0 through i-1. If you DON'T pair at position i, what's dp[i]?"**
   - Looking for: dp[i-1]
   - If they get it → "Good. Now if you DO pair elements i-1 and i-2 together, what's dp[i]?"
   - If stuck → "If you skip, nothing changes from the previous step. So it's the same as the answer for one fewer element."

2. **"If you pair the last two elements, their sum is arr[i-1] + arr[i-2]. And the rest of the problem is everything before those two. What's that?"**
   - Looking for: dp[i-2] + arr[i-1] + arr[i-2]
   - If they get it → "Now put the two cases together."
   - If stuck → "Those two elements are used. Everything before i-2 is handled by dp[i-2]. So the total for this case is dp[i-2] plus the pair sum."

3. **"So dp[i] = max of those two options. Can you write it out?"**
   - Looking for: dp[i] = max(dp[i-1], dp[i-2] + arr[i-1] + arr[i-2])
   - If they get it → "That's it! Now what are your base cases?" *(go to BASE-CASE)*
   - If close but wrong → "Almost. Check which indices you're summing — the pair is the two elements right before position i."

---

## BASE-CASE
*Student has the recurrence but not the base case.*

1. **"Your recurrence uses dp[i-1] and dp[i-2]. What's the smallest i where that works?"**
   - Looking for: i = 2

2. **"What's dp[0]? That means you have zero elements. What's the max sum?"**
   - Looking for: 0
   - If they get it → "And dp[1]? You have one element — can you form any pair?"

3. **"With one element, you can't make a pair. So dp[1] is...?"**
   - Looking for: 0
   - If they get it → "So dp[0] = 0 and dp[1] = 0, and you start filling from dp[2] onward. Want to trace through an example?" *(go to WRAP-UP)*
   - If confused → "You need two elements to make a pair. With just one, you can't pair anything, so the best sum is 0."

---

## PAIR-CLARIFY
*Student is confused about what constitutes a valid pair.*

1. **"A pair is two elements that are right next to each other in the array — at indices i and i+1. You're adding their values together."**

2. **"The constraint is that no element appears in more than one pair. So if you pair elements at index 2 and 3, then neither 2 nor 3 can be in another pair."**

3. **"You don't have to pair everything. You can skip elements entirely. You're just trying to pick the set of non-overlapping pairs that gives the biggest total."**
   - Then redirect to wherever they were, or → *(go to DIAGNOSE)*

---

## ANSWER-SEEKING
*Student is trying to get the answer directly.*

1. **"I want to help you figure it out — it'll make more sense that way. Tell me the last thing you tried and where it broke down."**

2. If persistent: **"I'll give you a push: think about what decision you make at each index as you scan left to right. What are your options?"**

3. If very persistent: **"I think it might be worth talking to the professor about this one. Want me to flag them?"** *(escalate to human TA)*

---

## DEFAULT
*Student said something not covered above.*

1. **"Can you tell me more about what's confusing you?"**
2. **"Let me make sure I follow — you're trying to [paraphrase]. Is that right?"**
3. If still unclear: **"Let's start from scratch. What do you think the first step should be when you look at this problem?"** *(go to DIAGNOSE)*
4. If totally off-script and you can't recover: **"That's a good question. Let me get the professor to help with that one."** *(escalate to human TA)*

---

## WRAP-UP

| Student says | Robot responds |
|---|---|
| "I think I get it" / "That makes sense" | "Awesome! Try implementing it and come back if you get stuck." |
| "Thanks" / "Thank you" | "No problem! Good luck, you've got this." |
| "I'm still confused" | "That's okay, this one takes some getting used to. Want to trace through a small example together, or would you rather talk to the professor?" |
| "Bye" / walks away | "Good luck! Come back anytime." |
