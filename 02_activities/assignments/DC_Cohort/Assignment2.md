# DC Assignment 2: Design a Logical Model and Advanced SQL

🚨 **Please review our [Assignment Submission Guide](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md)** 🚨 for detailed instructions on how to format, branch, and submit your work. Following these guidelines is crucial for your submissions to be evaluated correctly.

#### Submission Parameters:
* Submission Due Date: `April 07, 2026`
* Weight: 70% of total grade
* The branch name for your repo should be: `assignment-two`
* What to submit for this assignment:
    * This markdown (Assignment2.md) with written responses in Section 1 and 4
    * Two Entity-Relationship Diagrams (preferably in a pdf, jpeg, png format).
    * One .sql file 
* What the pull request link should look like for this assignment: `https://github.com/<your_github_username>/sql/pulls/<pr_id>`
    * Open a private window in your browser. Copy and paste the link to your pull request into the address bar. Make sure you can see your pull request properly. This helps the technical facilitator and learning support staff review your submission easily.

Checklist:
- [ ] Create a branch called `assignment-two`.
- [ ] Ensure that the repository is public.
- [ ] Review [the PR description guidelines](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md#guidelines-for-pull-request-descriptions) and adhere to them.
- [ ] Verify that the link is accessible in a private browser window.

If you encounter any difficulties or have questions, please don't hesitate to reach out to our team via our Slack. Our Technical Facilitators and Learning Support staff are here to help you navigate any challenges.

***

## Section 1:
You can start this section following *session 1*, but you may want to wait until you feel comfortable wtih basic SQL query writing. 

Steps to complete this part of the assignment:
- Design a logical data model
- Duplicate the logical data model and add another table to it following the instructions
- Write, within this markdown file, an answer to Prompt 3


###  Design a Logical Model

#### Prompt 1
Design a logical model for a small bookstore. 📚

At the minimum it should have employee, order, sales, customer, and book entities (tables). Determine sensible column and table design based on what you know about these concepts. Keep it simple, but work out sensible relationships to keep tables reasonably sized. 

Additionally, include a date table. 
A date table (also called a calendar table) is a permanent table containing a list of dates and various components of those dates. Some theory, tips, and commentary can be found [here](https://www.sqlshack.com/designing-a-calendar-table/), [here](https://www.mssqltips.com/sqlservertip/4054/creating-a-date-dimension-or-calendar-table-in-sql-server/) and [here](https://sqlgeekspro.com/creating-calendar-table-sql-server/). 
Remember, you don't actually need to run any of the queries in these articles, but instead understand *why* date tables in SQL make sense, and how to situate them within your logical models.

There are several tools online you can use, I'd recommend [Draw.io](https://www.drawio.com/) or [LucidChart](https://www.lucidchart.com/pages/).

**HINT:** You do not need to create any data for this prompt. This is a conceptual model only. 

#### Prompt 2
We want to create employee shifts, splitting up the day into morning and evening. Add this to the ERD.

#### Prompt 3
The store wants to keep customer addresses. Propose two architectures for the CUSTOMER_ADDRESS table, one that will retain changes, and another that will overwrite. Which is type 1, which is type 2? 

**HINT:** search type 1 vs type 2 slowly changing dimensions. 

```
Architecture A - SCD Type 1: Keeps only the current address. When a customer updates their address, the old one is overwritten and lost forever.
Architecture B - SCD Type 2: Keeps every version of the customer’s address by creating a new row each time the address changes.
```

***

## Section 2:
You can start this section following *session 4*.

Steps to complete this part of the assignment:
- Open the assignment2.sql file in DB Browser for SQLite:
	- from [Github](./02_activities/assignments/assignment2.sql)
	- or, from your local forked repository  
- Complete each question, by writing responses between the QUERY # and END QUERY blocks


### Write SQL

#### COALESCE
1. Our favourite manager wants a detailed long list of products, but is afraid of tables! We tell them, no problem! We can produce a list with all of the appropriate details. 

Using the following syntax you create our super cool and not at all needy manager a list:
```
SELECT 
product_name || ', ' || product_size|| ' (' || product_qty_type || ')'
FROM product
```

But wait! The product table has some bad data (a few NULL values). 
Find the NULLs and then using COALESCE, replace the NULL with a blank for the first column with nulls, and 'unit' for the second column with nulls. 

**HINT**: keep the syntax the same, but edited the correct components with the string. The `||` values concatenate the columns into strings. Edit the appropriate columns -- you're making two edits -- and the NULL rows will be fixed. All the other rows will remain the same.

<div align="center">-</div>

#### Windowed Functions
1. Write a query that selects from the customer_purchases table and numbers each customer’s visits to the farmer’s market (labeling each market date with a different number). Each customer’s first visit is labeled 1, second visit is labeled 2, etc. 

You can either display all rows in the customer_purchases table, with the counter changing on each new market date for each customer, or select only the unique market dates per customer (without purchase details) and number those visits. 

**HINT**: One of these approaches uses ROW_NUMBER() and one uses DENSE_RANK().

Filter the visits to dates before April 29, 2022.

2. Reverse the numbering of the query so each customer’s most recent visit is labeled 1, then write another query that uses this one as a subquery (or temp table) and filters the results to only the customer’s most recent visit. 
**HINT**: Do not use the previous visit dates filter.

3. Using a COUNT() window function, include a value along with each row of the customer_purchases table that indicates how many different times that customer has purchased that product_id.

You can make this a running count by including an ORDER BY within the PARTITION BY if desired.
Filter the visits to dates before April 29, 2022.

<div align="center">-</div>

#### String manipulations
1. Some product names in the product table have descriptions like "Jar" or "Organic". These are separated from the product name with a hyphen. Create a column using SUBSTR (and a couple of other commands) that captures these, but is otherwise NULL. Remove any trailing or leading whitespaces. Don't just use a case statement for each product! 

| product_name               | description |
|----------------------------|-------------|
| Habanero Peppers - Organic | Organic     |

**HINT**: you might need to use INSTR(product_name,'-') to find the hyphens. INSTR will help split the column. 

2. Filter the query to show any product_size value that contain a number with REGEXP. 

<div align="center">-</div>

#### UNION
1. Using a UNION, write a query that displays the market dates with the highest and lowest total sales.

**HINT**: There are a possibly a few ways to do this query, but if you're struggling, try the following: 1) Create a CTE/Temp Table to find sales values grouped dates; 2) Create another CTE/Temp table with a rank windowed function on the previous query to create "best day" and "worst day"; 3) Query the second temp table twice, once for the best day, once for the worst day, with a UNION binding them. 

***

## Section 3:
You can start this section following *session 5*.

Steps to complete this part of the assignment:
- Open the assignment2.sql file in DB Browser for SQLite:
	- from [Github](./02_activities/assignments/assignment2.sql)
	- or, from your local forked repository  
- Complete each question, by writing responses between the QUERY # and END QUERY blocks

### Write SQL

#### Cross Join
1. Suppose every vendor in the `vendor_inventory` table had 5 of each of their products to sell to **every** customer on record. How much money would each vendor make per product? Show this by vendor_name and product name, rather than using the IDs.

**HINT**: Be sure you select only relevant columns and rows. Remember, CROSS JOIN will explode your table rows, so CROSS JOIN should likely be a subquery. Think a bit about the row counts: how many distinct vendors, product names are there (x)? How many customers are there (y). Before your final group by you should have the product of those two queries (x\*y). 

<div align="center">-</div>

#### INSERT
1. Create a new table "product_units". This table will contain only products where the `product_qty_type = 'unit'`. It should use all of the columns from the product table, as well as a new column for the `CURRENT_TIMESTAMP`.  Name the timestamp column `snapshot_timestamp`.

2. Using `INSERT`, add a new row to the product_unit table (with an updated timestamp). This can be any product you desire (e.g. add another record for Apple Pie). 

<div align="center">-</div>

#### DELETE 
1. Delete the older record for the whatever product you added.

**HINT**: If you don't specify a WHERE clause, [you are going to have a bad time](https://imgflip.com/i/8iq872).

<div align="center">-</div>

#### UPDATE
1. We want to add the current_quantity to the product_units table. First, add a new column, `current_quantity` to the table using the following syntax.
```
ALTER TABLE product_units
ADD current_quantity INT;
```

Then, using `UPDATE`, change the current_quantity equal to the **last** `quantity` value from the vendor_inventory details. 

**HINT**: This one is pretty hard. First, determine how to get the "last" quantity per product. Second, coalesce null values to 0 (if you don't have null values, figure out how to rearrange your query so you do.) Third, `SET current_quantity = (...your select statement...)`, remembering that WHERE can only accommodate one column. Finally, make sure you have a WHERE statement to update the right row, you'll need to use `product_units.product_id` to refer to the correct row within the product_units table. When you have all of these components, you can run the update statement.
*** 

## Section 4:
You can start this section anytime.

Steps to complete this part of the assignment:
- Read the article
- Write, within this markdown file, between 250 and 1000 words. No additional citations/sources are required.

### Ethics

Read: Boykis, V. (2019, October 16). _Neural nets are just people all the way down._ Normcore Tech. <br>
    https://vicki.substack.com/p/neural-nets-are-just-people-all-the

**What are some of the ethical issues important to this story?**

Consider, for example, concepts of labour, bias, LLM proliferation, moderating content, intersection of technology and society, ect. 

```
Vicki Boykis’s 2019 essay “Neural nets are just people all the way down” delivers a deceptively simple but profound reminder that behind every impressive neural network lies layer upon layer of invisible human labor, choices, and biases. What appears to be pure machine intelligence is actually a vast, often precarious assembly of people. The ethical stakes are high and remarkably relevant to today’s AI landscape.
The article’s central metaphor, clothes are still stitched by human hands because robots can’t handle fabric’s variability, extends directly to AI training. ImageNet, the foundational dataset that powered the deep-learning revolution in 2012, required millions of images to be manually labeled. Stanford’s Fei-Fei Li first paid Princeton students $10 an hour; when that proved too slow and expensive, the project turned to Amazon Mechanical Turk. Low-paid crowdworkers performed the repetitive, cognitively demanding task of classifying thousands of images daily.
This pattern has only intensified. Modern large language models (LLMs) and multimodal systems rely on even larger armies of data labelers, content moderators, and “AI trainerswho annotate toxic text, rate model outputs, or generate preference data for reinforcement learning. The ethical problem is twofold: exploitation and invisibility. Workers endure repetitive strain, psychological harm from viewing disturbing content, and precarious gig-economy contracts with no benefits or job security. Yet their labour is erased in marketing narratives that celebrate “artificial” intelligence. As Boykis shows, even the linguistic backbone rests on uncredited graduate student and clerical work from decades earlier. This is not a one-off historical quirk. It is structural. AI’s economic model externalizes human costs while privatizing the gains.
Every taxonomy is political. Boykis traces how ImageNet drew on WordNet’s synsets, which are human-created groupings of concepts that inevitably reflect the cultural assumptions, blind spots, and prejudices of their creators. When those taxonomies are used to label millions of images, biases become baked in at scale. The essay highlights ImageNet Roulette, an experiment that exposed wildly offensive or absurd labels for people. The ImageNet team later acknowledged that 1,593 synsets in the “person” subtree were problematic or unsafe and began a manual cleanup process.
This is an example of representational harm. Classification systems are never neutral; they encode power. Who decides what counts as a “normal” family photo, a “professional” hairstyle, or an “angry” facial expression? When these datasets train today’s LLMs and vision models, the same biases propagate, thus disproportionately harming marginalized groups in hiring algorithms, facial recognition, content moderation, and medical AI. 
Boykis notes that Mechanical Turk workers were monitored with control images to catch cheating. In 2025, content moderators for major platforms and AI companies still perform the same invisible work, flagging hate speech, violence, and exploitation so that recommendation engines and safety filters can function. The ethical tension is acute. Moderation is essential for safer AI, yet it is chronically underpaid, under-supported, and psychologically damaging. The very systems that claim to “learn” from human feedback depend on this hidden workforce while simultaneously automating away the jobs of the people who built them.
Boykis wrote before the ChatGPT era, but her argument is even more urgent now. The explosive proliferation of LLMs has multiplied the demand for human-labeled data by orders of magnitude. Reinforcement learning from human feedback (RLHF), synthetic data generation, and red-teaming all require massive human input. Yet the dominant narrative remains “bigger models = better intelligence.” This obscures the reality that scaling is only possible because of an ever-larger, often exploited underclass of data workers.
At its heart, the essay challenges the myth of technological neutrality. Neural nets do not float above society. They are society which is compressed, encoded, and amplified. The choices made in 1964 (Brown Corpus), 2007 (ImageNet labeling), and today (preference datasets for LLMs) shape what AI “knows” and how it behaves. When those choices reflect historical inequalities, the resulting systems entrench them. This is not an engineering problem that can be solved with better algorithms alone. It is a sociotechnical one requiring transparency, accountability, fair compensation for data workers, and democratic oversight of foundational datasets.
Boykis leaves us with a powerful image that every neural net is “just people all the way down.” Recognizing this does not diminish the technical achievements of AI. It demands we treat the humans behind it with dignity, confront the politics embedded in our data infrastructures, and design systems that are honest about their origins rather than pretending to be magically autonomous.
In an era of trillion-parameter models and breathless claims of artificial general intelligence, this reminder is essential. The ethical path forward requires more than technical fixes. It calls for labour rights for data workers, rigorous bias audits, public accountability for training data, and a cultural shift that values the human infrastructure of AI as much as the silicon. Until we acknowledge that neural nets are people all the way down, we risk building ever more powerful systems on foundations that are ethically precarious.
```
