# Assignment 4. Patient Zero

Student ID: 250300
Name: Nariman Serikkali
Group: SE-2536

This file is the report. Fill it in as you go. How to start: `START_HERE.md`. The task: `docs/Assignment4.pdf`.

## Program output

The 9 lines your program prints on your data (`java -cp out Main`):

    Mutations: 60
    Shared core: 1012
    Marker hits: 5
    First hit: 4741
    Border length: 3
    Shared words: 510
    Tests needed: 32
    Cheapest testing: 115
    Longest chain: 24

My outbreak report (a screenshot of `output/outbreak.html`):

![outbreak](output/outbreak.png)

## Questions

Answer in your own words, two or three sentences each.

Mission 1. What does the cell d[i][j] of your table mean, and how is it computed from other cells?
d[i][j] is the smallest number of changes (replace, insert, delete) needed to turn the first i letters of A into the first j letters of B. If the last letters are equal, it comes from d[i-1][j-1]. If not, I take the best of three cells: d[i-1][j-1]+1 (replace), d[i-1][j]+1 (delete), d[i][j-1]+1 (insert).
Mission 2. How is the longest common subsequence different from the longest common substring? My Shared core is bigger than the number of letters that are the same at the same place: why?
A substring must be a continuous block, but a subsequence may skip letters, so it can match letters in the same order with gaps between them. My Shared core is bigger than the number of letters equal at the same position because after an insertion or a deletion the strains shift, and the subsequence can still match the letters after the shift.
Mission 3. What does the prefix function tell you, and why does the pointer in the text never go back in KMP? What is the Border length of my marker and what does it mean?
The prefix function pi[i] is the length of the longest proper prefix of the marker that is also a suffix of its first i+1 letters. In KMP, on a mismatch the pattern pointer jumps back by pi, but the text pointer only moves forward, because the letters we already matched are known, and the border tells us how many of them still fit. My marker is CGGGGCGG and its Border length is 3: the prefix CGG is equal to the suffix CGG, so after a full match the next match can overlap with the previous one by 3 letters.
Mission 4. Why must you compare the letters after the hashes are equal? What happens to the cost of Rabin-Karp if many windows have the same hash?
Different words can have the same hash (a collision), so equal hashes do not prove equal words, and I compare the letters to be sure. If many windows have the same hash, I have to compare letters for each of them, and the time goes from about O(n) towards O(n·k) or worse.
Mission 5. What are the two states of a person in your DP, and how do you get the answer for a person from its children? Why does a greedy "test the person with most links first" not always give the cheapest answer?
Each person has two states: tested, in[v], and not tested, out[v]. If v is tested, in[v] = cost[v] + sum of min(in[u], out[u]) over children u, because every child is free to choose. If v is not tested, out[v] = sum of in[u], because every child must be tested to cover its link. The greedy “most links first” can fail because a person with many links can be expensive, and a few cheaper people around can cover all links with a smaller total cost.
## AI use
I used Claude (Anthropic) to explain the ideas of the algorithms (edit distance, LCS, KMP, Rabin-Karp, DP on a tree), to review my code, and to help with git and the structure of the README. I read and understood every line of the code that I submitted.
The syllabus requires you to say which AI tools you used and for what. Write the tool (ChatGPT, Copilot, other) and what you asked it: explain an idea, find an error, review code. Write "none" if you did not use any. If you took code or text from a book or a website, give the source here.

## Timing (Mission 7)

Milliseconds, average of 3 runs, from `java -cp out Bench`.

| test | size | time                  |
|---|---|-----------------------|
| naive search | 20 000 / 40 000 / 80 000 | 654.1 / 273.8 / 486.0 |
| KMP | 20 000 / 40 000 / 80 000 | 7.3 / 10.2 / 8.8 |
| edit distance | 500 / 1000 / 2000 / 4000 | 18.8 / 44.9 / 323.4 / 1225.7 |

What do the numbers say about how the time grows? When you double the size, by how much does each time grow? Two or three sentences.
KMP stays at about 7-10 ms when the text grows from 20 000 to 80 000, so its time is almost linear and does not depend much on the length of the text. The naive search is much slower (hundreds of ms) and its numbers jump between runs, but it is far above KMP. Edit distance grows about 4 times when the length is doubled (for example from 2000 to 4000: 323 ms to 1226 ms), which fits the quadratic O(n·m) table.
## Complexity

n is the length of the text (or of a strain), m the length of the pattern, k the length of the window, v the number of people. Fill in every algorithm you wrote.

| algorithm | time | space | assumptions |
|---|---|--|-------------|
| Edit distance | O(n·m) | O(n·m) | full table (n+1)×(m+1)            |
| LCS | O(n·m) | O(n·m) | full table (n+1)×(m+1)            |
| Prefix function and KMP | O(n + m) | O(m) | the text pointer never goes back            |
| Rabin-Karp (shared words) | O(n + m + n·k) in the worst case, about O(n + m) on average | O(m) | k = 12, worst case when many hashes collide            |
| Tree DP | O(v) | O(v) | one pass over the tree, no recursion            |

## Before you submit

- The four files in `data` and the screenshot `output/outbreak.png` are in the repository.
- `java tools/Check.java` shows all examples right.
- This README is filled in, the program output and the section AI use included.
- The work is pushed to main (at least one commit per mission) and the repository is public.
- The link is attached in Moodle.
