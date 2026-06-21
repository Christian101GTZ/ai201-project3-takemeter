# Milestone 1 — Community Choice & Label Definitions

## Community: r/Games

For this project, I chose Reddit's r/Games community. r/Games is a large gaming subreddit focused on game announcements, industry news, reviews, technical updates, and community discussion. It is a good fit for a classification task because posts vary widely in purpose: some report industry events, some announce new games or updates, some evaluate game quality, and others focus on open-ended discussion.

After reviewing posts from r/Games, I decided to classify posts by their main discourse purpose rather than by Reddit flair. I do not automatically trust the subreddit flair because a post labeled "Discussion" may actually function as a review, and a post labeled "News" may generate mostly opinion-based discussion. My labels are based on the content and purpose of the post.

## Label 1 — Industry_News

**Definition:** A post about the gaming industry, companies, studios, business decisions, hardware issues, layoffs, acquisitions, legal disputes, labor issues, technology updates, developer interviews, or industry reports.

### Clear Examples

- "Ubisoft closes its Belgrade studio and lays off more than 100 employees."
- "Epic introduces a major Unreal Engine 5 update that improves Lumen performance."

### Ambiguous Example

- "Sony says PS5 is the most successful PlayStation generation ever."

**Decision Rule:** If the post is mainly about business, companies, technology, sales, labor, legal issues, or industry trends, label it **Industry_News** even if the comments include opinions.

---

## Label 2 — Announcement

**Definition:** A post centered on an official announcement, trailer, release date, showcase, DLC reveal, update, patch, launch, season announcement, or promotional reveal.

### Clear Examples

- "Persona 4 Revival Gameplay Broadcast showcases new gameplay and soundtrack updates."
- "Garfield: Escape from Monday Official Announcement Trailer reveals a new Garfield platformer release."

### Ambiguous Example

- "Path of Exile 2 Spirit Walker Ascendancy Showcase reveals a new ascendancy and update features."

**Decision Rule:** If the main purpose is to reveal, promote, launch, or announce new game content, label it **Announcement** even if comments debate the quality of the content.

---

## Label 3 — Review_Critique

**Definition:** A post focused on evaluating the quality of a game through reviews, critiques, retrospectives, performance analysis, user impressions, or gameplay evaluation.

### Clear Examples

- "High on Life 2 Review Thread. OpenCritic score of 75 with critics praising humor and creativity while criticizing technical issues."
- "Bloodborne - Commentary and Critique by Joseph Anderson analyzes Bloodborne's boss design, progression systems, difficulty balance, storytelling, and gameplay mechanics."

### Ambiguous Example

- "Weekly community discussion where players shared impressions of games they recently played."

**Decision Rule:** If the post is mainly evaluating games, discussing strengths and weaknesses, or collecting player/critic impressions, label it **Review_Critique**. If it is mainly open-ended conversation or recommendations, label it **Discussion**.

---

## Label 4 — Discussion

**Definition:** A post focused on community opinions, debates, questions, recommendations, design discussions, gaming culture, genre debates, personal experiences, or general conversation.

### Clear Examples

- "Can anyone recommend turn-based tactics games similar to XCOM that are not too difficult?"
- "What gameplay mechanics have not reached their full potential in gaming?"

### Ambiguous Example

- "The Steam Next Fest June 2026 Edition is live, with players sharing impressions and recommendations from new game demos."

**Decision Rule:** If the main purpose is open-ended conversation, recommendations, debate, or community reflection, label it **Discussion**. If the post primarily evaluates the quality of specific games, label it **Review_Critique**.

---

## Hardest Edge Cases

### Edge Case 1 — Discussion vs Review_Critique

Some weekly discussion threads contain many user impressions that sound like mini-reviews.

**Example:**  
"Players shared detailed impressions of games they recently played, including Hades II, Dead Space Remake, and others."

**Decision Rule:** If the thread is primarily a general community conversation, label it **Discussion**. If the row summary focuses on evaluating game quality, strengths, weaknesses, and recommendations, label it **Review_Critique**.

### Edge Case 2 — Industry_News vs Discussion

Some news posts become opinion-heavy in the comments.

**Example:**  
"PlayStation first-party game sales declining heavily since 2020."

**Decision Rule:** If the post itself reports an industry trend or business issue, label it **Industry_News**. If the post is framed as a community debate or opinion question, label it **Discussion**.

### Edge Case 3 — Announcement vs Review_Critique

Some trailer posts receive many comments judging the game's quality.

**Example:**  
"Dead or Alive 6 Last Round Announcement Trailer reveals an updated definitive edition."

**Decision Rule:** If the post is centered on an official reveal, trailer, update, or launch, label it **Announcement** even if users critique the trailer or game.

---

## Why These Labels Matter

r/Games users interact with many different types of gaming content. A news report, an announcement trailer, a review thread, and a recommendation discussion all serve different purposes. These labels help the model learn meaningful distinctions in gaming discourse instead of simply detecting whether a post is about games.

## Summary

**Community:** r/Games  
**Goal:** Classify r/Games posts by their main content purpose.  
**Labels:**

- Industry_News
- Announcement
- Review_Critique
- Discussion

These labels are mutually exclusive, cover most r/Games posts, and reflect common categories that users encounter in the community.

# Milestone 2 — Project Planning

## Data Collection Plan

I collected examples from the r/Games subreddit using publicly available posts and discussion threads. Rather than focusing on comments, I chose to classify posts because they provided clearer distinctions between different types of gaming content.

The dataset was collected manually and stored in a CSV file using the following format:

```text
text,label,notes
```

Each row contains:

- text: The title or summary of the post
- label: One of the four classification categories
- notes: A brief explanation of why the example belongs in that category

My target was to create a balanced dataset with 200 total examples:

- Industry_News: 50 examples
- Announcement: 50 examples
- Review_Critique: 50 examples
- Discussion: 50 examples

During collection I reviewed each example manually and assigned labels based on the definitions established in Milestone 1. Duplicate entries were removed to improve dataset quality and maintain consistency.

### Final Dataset Distribution

| Label | Count |
|---------|---------:|
| Industry_News | 50 |
| Announcement | 50 |
| Review_Critique | 50 |
| Discussion | 50 |
| **Total** | **200** |

The final dataset is perfectly balanced, which reduces the likelihood of the model favoring a particular class during training.

---

## Difficult Labeling Cases

### Example 1

**Post:** Weekly community discussion where players shared impressions of games they recently played.

**Possible Labels:** Discussion or Review_Critique

**Decision:** Review_Critique

**Reason:** Although it originated from a discussion thread, the content primarily consisted of game evaluations, recommendations, strengths, and weaknesses.

---

### Example 2

**Post:** PlayStation first-party game sales declining heavily since 2020.

**Possible Labels:** Industry_News or Discussion

**Decision:** Discussion

**Reason:** The post was framed around community debate and interpretation rather than simply reporting factual business news.

---

### Example 3

**Post:** Dead or Alive 6 Last Round Announcement Trailer reveals an updated definitive edition.

**Possible Labels:** Announcement or Review_Critique

**Decision:** Announcement

**Reason:** The purpose of the post was the official reveal of the product. Community opinions about the game's quality were secondary.

---

## Evaluation Metrics

### Accuracy

Accuracy measures the percentage of correctly classified examples across the entire test set.

### Precision

Precision measures how often predictions for a particular label are correct.

### Recall

Recall measures how many examples from a true label are successfully identified.

### F1 Score

F1 Score combines precision and recall and will serve as the primary performance metric because all four classes are equally important.

### Confusion Matrix

A confusion matrix will be used to identify which labels the model struggles to separate.

Particular attention will be paid to:

- Industry_News vs Discussion
- Announcement vs Industry_News
- Review_Critique vs Discussion
- Announcement vs Review_Critique

These boundaries represent the most likely classification challenges.

---

## Definition of Success

The classifier will be considered successful if it achieves:

- Accuracy of at least 75%
- Macro F1 score of at least 0.70
- No individual class below 0.60 F1
- Clear separation between all four categories in the confusion matrix

For a real-world moderation or analytics tool, I would consider:

- 80%+ Accuracy
- 0.75+ Macro F1

to be strong performance.

---

## AI Tool Plan

### Label Stress-Testing

Before finalizing the dataset, ChatGPT was used to generate borderline examples that sat between multiple labels. This helped refine decision rules and reduce ambiguity.

### Annotation Assistance

ChatGPT assisted with formatting Reddit posts into CSV rows and occasionally suggested possible labels. Every label included in the final dataset was manually reviewed and corrected when necessary.

### Failure Analysis

After training, ChatGPT will be used to help identify patterns in incorrect predictions.

Questions to investigate include:

- Which labels are confused most frequently?
- Does the model rely too heavily on keywords such as "review" or "trailer"?
- Does the model struggle with short posts?
- Does the model confuse discussions with critiques?

Any patterns identified by AI will be verified manually before inclusion in the final report.

---

## Project Success Summary

This project aims to classify r/Games posts into four categories:

- Industry_News
- Announcement
- Review_Critique
- Discussion

The final dataset contains 200 manually labeled examples with an even distribution across all categories. The next phase of the project is to compare a zero-shot Groq baseline against a fine-tuned DistilBERT classifier and evaluate how well the model learns these distinctions. 


# Milestone 4 — Baseline Results

The zero-shot baseline achieved 80% accuracy on the test set.

The model performed best on **Announcement** and **Industry_News**, showing that these categories have clear and recognizable language patterns.

The model struggled most with **Discussion**, which had the lowest F1-score. Many Discussion posts were incorrectly classified as other categories. This may be because Discussion posts cover a wide variety of topics and writing styles, making them harder to identify consistently.

**Hypothesis:** Fine-tuning DistilBERT on my labeled r/Games dataset will improve performance, especially for the Discussion category, by helping the model learn community-specific language patterns and distinctions between labels.

These baseline results will be used as the benchmark for evaluating the fine-tuned model. 