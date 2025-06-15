# Prompt Engineering for Review Summarization

This project evaluates different **prompt engineering techniques** for summarizing real customer reviews using OpenAI’s GPT-3.5-turbo model.

Part of **CISC 692 – Next-Gen AI System Design Patterns**  
Assignment A03-D: *Prompt Flow & Evaluation*  
Author: **Darshika Verma**

---

## Project Objective

To **compare prompting strategies** for summarizing product reviews and evaluate them based on:
- **Accuracy**
- **Tone**
- **Task Success (length + clarity)**

---

## Project Workflow

1. **Data Collection:**  
   - Fetched 140 real customer reviews for *Sony WH-CH520 Wireless Headphones* using the [Unwrangle API](https://data.unwrangle.com/).

2. **Prompting Techniques Tested:**
   - `Zero-shot`
   - `Few-shot`
   - `Chain-of-Thought (CoT)`
   - `Tree-of-Thoughts`
   - `ReAct`

3. **Summary Generation:**  
   - Used `openai.chat.completions.create()` with GPT-3.5-turbo

4. **Analysis:**  
   - Word count comparison
   - Sentiment analysis (`TextBlob`)
   - Manual rubric scoring

5. **Refinement:**  
   - Refined top-performing prompt (`Few-shot`) to improve tone and fluency

---

## Prompting Strategies Explained

| Prompt Type      | Description                                                            | Goal                                |
|------------------|------------------------------------------------------------------------|-------------------------------------|
| **Zero-shot**    | Direct summary request without any examples                            | Test model's base ability           |
| **Few-shot**     | Provide 1–2 example reviews + summaries before asking                   | Guide LLM using context             |
| **Chain-of-Thought** | Step-by-step breakdown (sentiment → main point → summary)         | Encourage structured reasoning      |
| **Tree-of-Thoughts** | Generate 2 summaries → pick best → justify                        | Explore diverse reasoning paths     |
| **ReAct**        | Combine "think" (sentiment) + "act" (summary) in one prompt             | Simulate real-time decisions        |

---

## Tech Stack

- **Language Model:** `GPT-3.5-turbo` (via OpenAI API)
- **Prompt Templates:** `jinja2`
- **Data Handling:** `pandas`
- **Sentiment Analysis:** `TextBlob`
- **Visualization:** `matplotlib`, `seaborn`
- **Data Fetching:** `requests`, `Unwrangle API`

---

## Results: Prompt Performance (Avg. Scores)

| Prompt Type        | Word Count | Sentiment Score | Total Score (Accuracy + Tone + Task) |
|--------------------|------------|------------------|---------------------------------------|
| **Few-shot**       | 16.7       | 0.379            | ⭐ **6.0**                             |
| **Zero-shot**      | 17.9       | 0.391            | ⭐ **6.0**                             |
| **ReAct**          | 21.7       | 0.371            | ⭐ **6.0**                             |
| Chain-of-Thought   | 30.0       | 0.304            | 5.4                                   |
| Tree-of-Thoughts   | 92.6       | 0.327            | 4.6                                   |

**Top performers**: ReAct, Few-shot, Zero-shot  
**Tree-of-Thoughts** was insightful but too verbose

---

## Evaluation Rubric

Each prompt was rated on:

| Metric          | Description                                     | Score Range |
|------------------|--------------------------------------------------|-------------|
| `accuracy_score` | Does the summary reflect the review correctly?   | 0–2         |
| `tone_score`     | Is the tone polite, human-like, and readable?    | 0–2         |
| `task_success`   | Is it a concise 1–2 sentence summary as asked?   | 0–2         |
| `total_score`    | Sum of the above (max 6)                         | 0–6         |

---

## Refined Few-shot Prompt (Best Performing)

```jinja2
Examples:
Review: "Battery dies too fast. Not usable for travel."
Summary: "User was unhappy with the short battery life and travel performance."

Review: "Very comfortable and amazing clarity! Would buy again."
Summary: "User praised the comfort and clarity, and expressed willingness to repurchase."

Now write a polite and concise summary (1–2 sentences):
"{{ review }}"
Summary:
Refined Summary: User enjoys these headphones for yard work due to their great quality.
Original Summary: The reviewer found the headphones great for yard work

