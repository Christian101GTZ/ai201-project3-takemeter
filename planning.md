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
