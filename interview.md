# Interview Preparation

## Part A: Technical Questions

### 1. Explain a LEFT JOIN
A LEFT JOIN returns every row from the left table and the matching rows from the right table. Where there is no match, the right-hand columns are NULL.

```sql
SELECT c.customer_id, c.name, o.order_id
FROM customers c
LEFT JOIN orders o ON o.customer_id = c.customer_id;
```
This lists all customers, including those who never ordered. Adding `WHERE o.order_id IS NULL` finds customers with no orders. Use INNER JOIN when you only want rows that match in both tables. A common mistake is filtering on a right-table column in `WHERE`, which silently turns the LEFT JOIN into an INNER JOIN. Put that condition in the `ON` clause instead.

### 2. How do you handle missing data?
1. **Diagnose first:** measure how much is missing per column and ask why (data entry error, system fault, genuinely not applicable). Check whether it is random or related to other variables.
2. **Choose a treatment based on the cause and the amount:**
   - Drop rows or columns if little is missing, or the column is not useful.
   - Impute with the median (skewed numeric data), mean (roughly symmetric data) or mode (categorical data). Group-wise imputation is better when groups differ.
   - Flag it, with an "unknown" category or a `was_missing` indicator, when missingness itself is informative.
3. **Document** every decision and check that it did not change the results materially (for example, compare summaries before and after).

In pandas: `df.isna().mean()`, `df.dropna()`, `df['col'].fillna(df['col'].median())`.

### 3. Mean versus median
The mean is the arithmetic average and uses every value, so it is pulled by outliers. The median is the middle value and is robust to outliers. For skewed data such as income, transaction amounts or house prices, the median better describes a "typical" value. For roughly symmetric data without extreme outliers, the mean is fine and is more useful for further calculation. When the two differ a lot, that itself signals skew or outliers worth investigating. Reporting both is often best.

### 4. What does a p-value mean?
The p-value is the probability of seeing a result at least as extreme as the one observed, **assuming the null hypothesis is true**. A small p-value (commonly below 0.05) means the data would be unusual under the null, so we have evidence against it. It is **not** the probability that the null is true, nor the size or importance of the effect. A very large sample can give a tiny p-value for a trivial effect, so I report effect size and a confidence interval alongside it.

### 5. How would you investigate a sudden drop in sales?
1. **Confirm it is real:** check for data pipeline issues, missing days, changed definitions or duplicate/dropped records.
2. **Size and time it:** when exactly did it start, and is it a step change or a gradual decline? Compare with the same period last week and last year to rule out seasonality.
3. **Segment it:** break down by product, region, channel, customer type, price band and device to see where the drop is concentrated.
4. **Decompose the metric:** sales = traffic × conversion × average order value. Find which component fell.
5. **Look for causes:** price changes, stock-outs, promotions ending, website or app releases, marketing spend, competitor activity, outages, payment failures, holidays.
6. **Test hypotheses and report:** validate the leading cause with data, quantify the impact, recommend actions and set up monitoring or alerts.

---

## Part B: Behavioural Questions (STAR)

> Replace every bracketed prompt with your own real experience. Use your capstone or a weekly assignment if you have no workplace example. Keep each story to about 2 minutes spoken.

### Story 1: Tell me about a time you solved a difficult problem with data

- **Situation:** [Where and when? What was the project and context? e.g. "During my capstone, I was analysing [dataset] to answer [question]."]
- **Task:** [What was your specific responsibility or goal? What made it hard?]
- **Action:** [What did YOU do, step by step? Which tools (Python, SQL, pandas, Excel, Power BI) and why? How did you handle obstacles such as messy data?]
- **Result:** [Quantified outcome: e.g. "cleaned X rows", "found Y% difference", "cut analysis time by Z%". What did you learn?]

### Story 2: Tell me about a time you received feedback or worked under pressure or in a team

- **Situation:** [Context: a group project, a peer review, a mentor's feedback, a tight deadline.]
- **Task:** [What needed to be done and by when? What was your role?]
- **Action:** [How did you respond? What did you change, communicate or organise?]
- **Result:** [What improved? Include a number if possible, and what you would do differently next time.]

**Tips:** use "I" rather than "we" for your own actions, keep the focus on the action step, and end with a measurable result and a lesson.
