# 💭 Reflection: Game Glitch Investigator

Answer each question in 3 to 5 sentences. Be specific and honest about what actually happened while you worked. This is about your process, not trying to sound perfect.

## 1. What was broken when you started?

- What did the game look like the first time you ran it?
The game asks me to guess a number between 1 and 100 within 7 attempts. I see a space to enter my guess, start a new game, and a checkbox to give a hint. I see on the side that I
can play on easy, normal, or hard. 

- List at least two concrete bugs you noticed at the start  
  (for example: "the hints were backwards").
I guessed 100 and the hint told me to go higher, even though the target number should be between 1 and 100. When I guess 101 it tells me to go lower, so there may be an issue with
edge cases. Similarly, inputting a negative number outputs a hint that tells me to go lower. Another bug is the caption of "Guess a number between # and #" doesn't change when the difficulty
is adjusted.

**Bug Reproduction Log**

Document at least 3 bugs you found. Add rows as needed.

| Input | Expected Behavior | Actual Behavior | Console Output / Error |
|-------|-------------------|-----------------|------------------------|
|  1000 | Out of bounds!    | Go HIGHER!      | parse_guess never checks that the guess is between the low and high| 
| –1    | out of bounds!    | Go LOWER!       | same as bug 1, but also if guess < secret, it outputs go lower instead of higher|
| new game button| start a new game| game locks and doesn't allow guesses| new game handler doesn't reset the session status|

---

## 2. How did you use AI as a teammate?

- Which AI tools did you use on this project (for example: ChatGPT, Gemini, Copilot)?
I only used Claude on this project.

- Give one example of an AI suggestion that was correct (including what the AI suggested and how you verified the result).
One suggestion that was correct was the inverted hints. The suggested fix was to flip the message so that when the guess < secret, it would output go HIGHER instead of go LOWER.
I verified the result by implementing the fix and seeing that the game outputs the correct hint.

- Give one example of an AI suggestion you did not accept as written (including what the AI suggested, why you rejected or changed it, and how you verified your version). It does not have to be a suggestion that was wrong: over-engineered, out of scope, harder to read, or a poor fit for this codebase all count.
One AI suggestion I did not accept was when Claude attempted to rebuild the submit flower so feedback is saved in st.session_state.feedback. It ended every sumit with st.rerun() and reset_game() on difficulty change. It was quite difficult to read and understand, along with the fact that I only asked Claude to fix the attempt range and not overextend on the diffulcty-change resets.

---

## 3. Debugging and testing your fixes

- How did you decide whether a bug was really fixed?
I would implement the change then play-test it myself to see if it would output the expected output. After the fix, I also tried nearby cases. For the out of range bug, the original issue was from 1000, but I also tried 0, 101, and edge cases like 1 and 100 to make sure the boundary was right and I didn't just fix the case of 1000. Sometimes the bug would be fixed, other times, I would find more bugs stemming from the fix.

- Describe at least one test you ran (manual or using pytest)  
  and what it showed you about your code.
A main test I ran was the attempts counter. I played a full game on Normal, guessing until I ran out of attemps. I noticed that counter did not stop at 0 as expected and went into the negatives. I looked into the attemps logic and, with help from Claude, saw that the attemps logic and new game reset were tied together. I had claude run the same scenario with Streamlit's built-in test to confirm that expending all guesses lead to a "lost" status.

- Did AI help you design or understand any tests? How?
Yes, Claude pointed me to where the bug came from in the code. For example, it explained that the odd hints came from two bugs stacking. It also suggested what to try when I tested, like guessing above and below the secret. The cherry on top is it running a simulated game test for me.

---

## 4. What did you learn about Streamlit and state?

- How would you explain Streamlit "reruns" and session state to a friend who has never used Streamlit?
Streamlit essentially reruns the whole Python file everytime a piece of code is changed. As a result, the variables in memory gets wiped out each rerun, so the game would forget the secret numer and attempts everytime a guess is inputted. Session state is the state of memory where variable values like secret, attempts, and score are stored.
---

## 5. Looking ahead: your developer habits

- What is one habit or strategy from this project that you want to reuse in future labs or projects?
  - This could be a testing habit, a prompting strategy, or a way you used Git.
One strategy I would reuse is testing the fix with nearby cases. This ensures I can catch more bugs and test edge cases.

- What is one thing you would do differently next time you work with AI on a coding task?
I would be more specific with what I want Claude to do. Many of the fixes came with overextension where Claude implemented "fixes" that I didn't ask for. It didn't bug out the game but it bloats the app.

- In one or two sentences, describe how this project changed the way you think about AI generated code.
This project showed me how buggy AI code can be. There were so many logic errors that showed up immediately when a real human play-tested it.
