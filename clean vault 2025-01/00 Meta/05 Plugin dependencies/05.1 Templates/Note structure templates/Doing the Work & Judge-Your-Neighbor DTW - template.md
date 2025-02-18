<%*
tp.hooks.on_all_templates_executed(async () => {
    const file = tp.file.find_tfile(tp.file.path(true));
    
    // process Frontmatter
    await app.fileManager.processFrontMatter(file, (frontmatter) => { 
        frontmatter["up"] = "[[Doing The Work]]";
        frontmatter["type"] = "my/reflections/dtw";
        frontmatter["publish"] = false;
        frontmatter["banner"] = "https://i.imgur.com/k7VqUM7.jpg";
    });
});-%>
![[<% tp.file.title %>#^Summary]]

> [!instructions] 
> - Think of a stressful situation with someone, for example, an argument. 
> - As you meditate on that specific time and place and begin to feel what that felt like, fill in the blanks below. 
> - Use short, simple sentences.

1. In this situation, who angers, confuses, hurts, saddens, or disappoints you, and why? 
    - I am **<%tp.file.cursor(0)%>** with ** <% tp.file.cursor(2) %>** because ** <% tp.file.cursor(3) %>** 
2. In this situation, how do you want him/her to change? What do you want him/her to do? (WANTS)
    - I want ** <% tp.file.cursor(2) %>** to ** <% tp.file.cursor(4) %>**  
3. In this situation, what advice would you offer him/her? “He/she should/shouldn’t…” (ADVICE) 
    - ** <% tp.file.cursor(2) %>** should/shouldn’t ** <% tp.file.cursor(5) %>**
4. In order for you to be happy in this situation, what do you need him/her to think, say, feel, or do? (NEEDS)
    - I need ** <% tp.file.cursor(2) %>** to ** <% tp.file.cursor(6) %>**  
5. What do you think of him/her in this situation? Make a list. (It’s okay to be petty and judgmental.) (COMPLAINTS)
    - ** <% tp.file.cursor(2) %>** is ** <% tp.file.cursor(7) %>** 
6. What is it about this person and situation that you don’t ever want to experience again? ^statement6
    - I don’t ever want ** <% tp.file.cursor(8) %>**

---
> [!NOTE]+ applying the work
> - **Now question each of your statements, using the four questions of The Work, below**. 
> 1. Is it true? (Yes or no. If no, move to question 3.)
> 2. Can you absolutely know that it’s true? (Yes or no.) 
> 3. How do you react, what happens, when you believe that thought? 
> 4. Who or what would you be without the thought? 
> - **Turn the thought around** (give at least 3 example turn arounds for each belief)

---
- ** <% tp.file.cursor(2) %>**, ** <% tp.file.cursor(3) %>**
    1. 
    2. no
    3. 
    4. 
    5. **turn around**:
        - 
- I want ** <% tp.file.cursor(2) %>** to ** <% tp.file.cursor(4) %>**
    1. 
    2. no
    3. 
    4. 
    5. **turn around**:
        - 
- ** <% tp.file.cursor(2) %>** should/shouldn’t ** <% tp.file.cursor(5) %>**
    1. 
    2. no
    3. 
    4. 
    5. **turn around**:
        - 
- I need ** <% tp.file.cursor(2) %>** to ** <% tp.file.cursor(6) %>**
    1. 
    2. no
    3. 
    4. 
    5. **turn around**:
        - 
- ** <% tp.file.cursor(2) %>** is ** <% tp.file.cursor(7) %>**
    1. 
    2. no
    3. 
    4. 
    5. **turn around**:
        - 
> [!Question] For the turnaround to **statement 6**, replace the words *I don’t ever want…* with *I am willing to…* and *I look forward to…* 
> ![[<% tp.file.title %>#^statement6]]
- I don’t ever want the above things again
    1. 
    2. no
    3. 
    4. 
    5. **turn around**:
        - I am willing to and I look forward to


---

- **Summary**::  What did I learn from this exercise?

^Summary