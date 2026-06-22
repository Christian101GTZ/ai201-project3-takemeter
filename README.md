# TakeMeter: Discourse Classification in r/Games

## Community Choice and Reasoning

I selected r/Games because it is one of the largest gaming discussion communities on Reddit and contains a wide variety of discourse styles. Users regularly post game announcements, industry news, reviews, critiques, and discussion topics.

After reviewing approximately 40 posts from the community, I found that most content naturally fell into one of four recurring communication styles. This made r/Games a strong candidate for a classification task because the distinctions are meaningful to community members and frequently appear in everyday discussions.

---

## Label Taxonomy

### Announcement

**Definition:** Posts primarily intended to announce a new game, update, trailer, release date, expansion, or event.

**Examples:**

* Persona 4 Revival Gameplay Broadcast showcases new gameplay and soundtrack updates.
* Garfield: Escape from Monday Official Announcement Trailer reveals a new Garfield platformer release.

### Industry_News

**Definition:** Posts reporting factual information about the gaming industry, companies, developers, business decisions, legal actions, technology, or market trends.

**Examples:**

* Nintendo suing the U.S. government over tariffs affecting its business operations.
* Epic introduces a major Unreal Engine 5 update that improves Lumen performance and can run up to twice as fast on Nintendo Switch 2.

### Review_Critique

**Definition:** Posts evaluating, reviewing, critiquing, or analyzing a game, feature, mechanic, or design decision.

**Examples:**

* High on Life 2 Review Thread.
* Tomodachi Life: Living the Dream Review Thread.

### Discussion

**Definition:** Posts centered on community conversation, recommendations, opinions, questions, or debate.

**Examples:**

* Best traveling NPCs and merchants in video games.
* Can anyone recommend turn-based tactics games similar to XCOM that are not too difficult?

---

## Data Collection and Labeling Process

Data was collected from public posts and discussion threads within r/Games.

The final dataset contains 200 manually labeled examples. Each example was reviewed and assigned one of four labels: Announcement, Industry_News, Review_Critique, or Discussion.

The dataset was intentionally balanced to reduce class imbalance during training.

### Label Distribution

| Label           | Count |
| --------------- | ----- |
| Announcement    | 50    |
| Industry_News   | 50    |
| Review_Critique | 50    |
| Discussion      | 50    |

### Dataset File

The labeled dataset is included in this repository as:

`rgames_labeled_posts.csv`

---

## Difficult-to-Label Examples

### Example 1

**Text:** The Steam Next Fest June 2026 Edition is live, with players sharing impressions and recommendations from new game demos.

**Potential Labels:** Discussion or Review_Critique

**Final Label:** Discussion

**Reasoning:** The focus is on community conversation and recommendations rather than a formal review or critique.

### Example 2

**Text:** Hideo Kojima disappointed with the state of the industry, believing the most interesting work is happening among indies while big budget studios are producing safe projects.

**Potential Labels:** Industry_News or Review_Critique

**Final Label:** Industry_News

**Reasoning:** The post reports comments made by a major industry figure rather than presenting the author's own critique.

### Example 3

**Text:** PlayStation first-party game sales declining heavily since 2020.

**Potential Labels:** Industry_News or Discussion

**Final Label:** Discussion

**Reasoning:** The post primarily encourages community debate about the topic rather than reporting new information.

---

## Fine-Tuning Approach

The model was fine-tuned using Hugging Face's DistilBERT model:

`distilbert-base-uncased`

Training was performed in Google Colab using a free T4 GPU.

### Dataset Split

* Training Set: 140 examples
* Validation Set: 30 examples
* Test Set: 30 examples

### Training Configuration

* Model: distilbert-base-uncased
* Epochs: 3
* Learning Rate: 2e-5
* Batch Size: 16

These settings were kept at the notebook defaults because the dataset was relatively small.

---

## Baseline Description

The baseline model used Groq's `llama-3.3-70b-versatile` in a zero-shot classification setting.

The prompt included definitions for all four labels and instructed the model to return only one label for each post.

The baseline was evaluated on the same test set as the fine-tuned model, allowing for a direct comparison between the two approaches.

---

## Demo Video

Loom Video: https://www.loom.com/share/22062a609c144d88af94e1554c878d14

# Evaluation Report

## Results Comparison

| Model                                   | Accuracy |
| --------------------------------------- | -------- |
| Zero-shot Baseline (Groq Llama 3.3 70B) | 80.0%    |
| Fine-tuned DistilBERT                   | 53.3%    |

The zero-shot baseline outperformed the fine-tuned DistilBERT model by 26.7 percentage points. While I expected fine-tuning to improve performance, the relatively small dataset and overlap between labels likely made it difficult for DistilBERT to learn reliable decision boundaries.

---

## Per-Class Metrics

### Fine-Tuned DistilBERT

| Label           | Precision | Recall | F1-Score |
| --------------- | --------- | ------ | -------- |
| Industry_News   | 0.60      | 0.38   | 0.46     |
| Announcement    | 0.75      | 0.43   | 0.55     |
| Review_Critique | 0.44      | 0.88   | 0.58     |
| Discussion      | 0.60      | 0.43   | 0.50     |

### Zero-Shot Baseline

| Label           | Precision | Recall | F1-Score |
| --------------- | --------- | ------ | -------- |
| Industry_News   | 1.00      | 0.75   | 0.86     |
| Announcement    | 0.78      | 1.00   | 0.88     |
| Review_Critique | 0.70      | 0.88   | 0.78     |
| Discussion      | 0.80      | 0.57   | 0.67     |

The baseline model performed better across all categories. The fine-tuned model struggled most with distinguishing Discussion posts from Review_Critique posts.

---

## Confusion Matrix

![Confusion Matrix](Assets/confusion_matrix.png)

| True \ Predicted | Industry_News | Announcement | Review_Critique | Discussion |
| ---------------- | ------------- | ------------ | --------------- | ---------- |
| Industry_News    | 5             | 0            | 3               | 0          |
| Announcement     | 0             | 7            | 0               | 0          |
| Review_Critique  | 3             | 0            | 5               | 0          |
| Discussion       | 1             | 0            | 5               | 1          |

### Analysis

The model correctly classified all Announcement posts.

The largest source of error occurred between Discussion and Review_Critique. Five Discussion posts were incorrectly classified as Review_Critique. This suggests the model associated evaluation language with reviews even when the post was primarily a discussion.

Industry_News was also commonly confused with Review_Critique. News posts containing opinions or statements from developers often resembled review-style language.

---

## Analysis of Wrong Predictions

### Example 1

**Text:** Nintendo suing the U.S. government over tariffs affecting its business operations.

**True Label:** Industry_News

**Predicted Label:** Discussion

**Confidence:** 0.27

**Analysis:** This post reports a real business event involving Nintendo and government policy. The model likely interpreted the topic as a community discussion rather than industry reporting because the post is short and lacks obvious news-reporting language.

### Example 2

**Text:** Garfield: Escape from Monday Official Announcement Trailer reveals a new Garfield platformer release.

**True Label:** Announcement

**Predicted Label:** Review_Critique

**Confidence:** 0.28

**Analysis:** The phrase "Official Announcement Trailer" clearly signals an announcement. The model appears to have focused on the game description rather than the announcement-related wording.

### Example 3

**Text:** Players debated whether Planetside 2 should have been delayed, discussing optimization issues, game balance, launch expectations, and the challenges of releasing large-scale online games.

**True Label:** Discussion

**Predicted Label:** Review_Critique

**Confidence:** 0.26

**Analysis:** This post focuses on community discussion and debate. However, because it mentions optimization, balance, and quality concerns, the model incorrectly associated it with Review_Critique.

---

## Sample Classifications

| Example Text                                                                        | Predicted Label | Confidence |
| ----------------------------------------------------------------------------------- | --------------- | ---------- |
| Persona 4 Revival Gameplay Broadcast showcases new gameplay and soundtrack updates. | Announcement    | High       |
| Nintendo suing the U.S. government over tariffs affecting its business operations.  | Discussion      | 0.27       |
| Players debated whether Planetside 2 should have been delayed.                      | Review_Critique | 0.26       |

The first example demonstrates a correct prediction because the post contains clear announcement language related to a gameplay reveal and promotional showcase.

---

## Reflection: What the Model Learned vs. What I Intended

My goal was to classify posts according to their primary discourse style within r/Games.

The model struggled with all categories to varying degrees, although Announcement remained one of the more recognizable categories. The largest challenge was distinguishing Discussion, Review_Critique, and Industry_News because these categories frequently shared similar vocabulary and topics.

The results suggest that the model relied heavily on keywords rather than understanding the broader purpose of a post. Additional training examples and clearer distinctions between Discussion and Review_Critique would likely improve performance.

Although the fine-tuned model did not outperform the baseline, the project provided valuable insight into the challenges of dataset design, annotation consistency, and text classification.

# Spec Reflection

The project specification helped guide the overall structure of the project, especially during the dataset collection and evaluation stages. Having clear requirements for label definitions, dataset size, baseline comparison, and error analysis made it easier to organize the workflow and ensure that every stage of the project had a specific purpose.

One way my implementation differed from the original plan was during data collection. I initially focused on classifying different types of gaming content, but while reviewing r/Games posts I realized that some categories overlapped significantly. As a result, I refined the labels and decision rules throughout the annotation process to better reflect the actual discourse patterns found within the community.

# AI Usage

## Example 1: Label Design and Planning

I used ChatGPT during the planning phase to help evaluate and refine my label taxonomy. By discussing potential edge cases and ambiguous examples, I was able to create clearer decision rules for distinguishing between Industry_News, Announcement, Review_Critique, and Discussion posts.

## Example 2: Dataset Review

I used ChatGPT to review portions of the dataset and identify examples that were difficult to classify. The AI helped highlight potential overlaps between categories, but all final labeling decisions were made manually after reviewing each example.

## Example 3: Evaluation and Error Analysis

After training the model, I used ChatGPT to help analyze incorrect predictions and identify patterns in the errors. This helped me recognize that the model frequently confused Discussion and Review_Critique posts. The final interpretations and conclusions included in the report were reviewed and written by me.
