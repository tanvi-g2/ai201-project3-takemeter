# ai201-project3-takemeter

# TakeMeter: r/books Post Classifier

## Community
r/books — Book fandoms are some of the largest communities and there are many different types of topics that are covered within this community. Readers come from a wide variety of backgrounds, read many different types of books, and share many things related to the community. This makes it a strong fit for classification and provides a large enough dataset. 

## Labels
**reaction** — a post sharing a personal response to a book or reading 
experience. No external link. Feeling or experience-forward.
- Example 1: Thoughts On Micro By Michael Crichton
Micro like other of Crichton’s work I thought I’d get bored to death by and simply wanted to get through to eventually sell and free up space on my bookshelves, is something I actually got quite invested for some time. Whatever scientific jargon/bullshit he makes up and includes in his writing somehow always manages to intrigue me since my first non-Jurassic Park title, Prey.

It’s about a group of grad students who due to accidentally uncovering the dark secrets of the head (Vin Drake) of a company called, Nanigen are forcibly shrunk down to microscopic size and forced into in a whole, new world where carnivorous insects and small animals like birds are dangerous, terrifying apex predators. Their journey trying to deal with the micro wildlife and surviving was honestly way more interesting than the actual plot about tiny, killer robots armed with toxic missiles and the technology used to create them being eventually sold to foreign parties.

A minor character near the beginning of the novel (some engineer who helped make the tensor field shrinking machines or whatever) is brutally killed off by being torn apart by ants and it’s described in graphic detail how one rips off his scalp while the rest carry off his intestines and other internal organs into their nest. Which only now made me realize how dark Crichton’s novels can surprisingly get (reminds me of Nedry’s death in Jurassic Park). Heck, even in Congo the trained attack gorillas are portrayed as unsettlingly intelligent and explicitly stated to cave intruders’ faces in with blunt weapons when they ambush.

Most of the main cast die in increasingly worse and tragic ways. Notably, a female student who’s jumped by a flock of mynah birds and ripped in pieces midair as they fight over her fresh corpse. This isn’t too different from some of the dinosaur kills in The Lost World. How surprisingly fitting, since birds are dinosaurs after all lol.

It’s actually impressive, because in latest cinema like Ant-Man you don’t really get to see the truly scary side of being a micro-sized person. If Micro got a movie adaptation, a PG-13 or R rating would be very appropriate.

Forget the robot/micro-technology lessons. Micro, in my opinion is best enjoyed for its survival-horror segments set in a dimension where you’re nothing more than prey to things you can just step on as a normal-sized human.
- Example 2: Just finished the princess bride!
I’m so stupid and never even realized that there was a book, having grown up on solely the movie. The book grabbed my heart strings and PULLED!!! Fezzik is my favorite by FAR!! He truly was the backbone of the entire story-without him they couldn’t have done anything.

Buttercup was kinda useless but had good moments, and everything else was honestly just fantastic. It gripped me in a way that I don’t even want to move on to other books cause I want to clutch onto that one for forever. The ending with fezzik broke my heart and I’m so glad that wasn’t in the movie.

I also have to give so much credit to Goldman for the whole Morgenstern stuff cause it had me looking up the original book to try to find it lol. It IS the original!! Just such an amazing book, I wish it lasted longer. I’m definitely going to be reading it again. It’s absolutely my favorite book now!!

**news** — a post linking to an externally published article about the 
book world (author news, adaptations, libraries, publishing industry).
- Example 1: Judy Blume says she's done writing: '50 years is enough!'
https://www.npr.org/2026/06/20/nx-s1-5825725/judy-blume-scott-simon
- Example 2: The Massachusetts House has passed a bill that would shield schools from attempted book bans https://www.masslive.com/politics/2026/06/mass-moves-to-head-off-library-school-book-bans-3-big-things-to-know-bay-state-briefing.html

**discussion** — a question or conversation prompt to the r/books 
community. No external link.
- Example 1: A potential reading challenge idea?
I keep thinking about this idea I've had! What is the longest chain of books (WITHOUT repeating an author) you could make where each time, the next book was written by an author that had an endorsement quote on the cover of the previous one?

E.g. Prince of Thorns (Mark Lawrence) has a quote on the front by Robin Hobb, so the next read is Assassin's Apprentice, which has a quote on the front by Melanie Rawn, so the next read is Dragon Prince, which has a quote on by Anne McCaffrey, so we choose Dragonflight etc etc.

I feel like there are definitely some cliques of authors that you see recommending each others' things all the time, so it would be easy to read yourself into a dead-end where your only options were repeats, unless you chose editions and next reads very tactically!

There's definitely the potential here for some kind of year long reading challenge or something. Mostly posting to see if anybody thinks it's an interesting idea! What's the longest chain you think you could make?
- Example 2: Buying used books on Amazon
I keep wanting to buy used books from Amazon, but whenever I start searching with that intention, I find that a new copy is usually only $1 or $2 more than the used copies being sold. And the used copies usually have slower shipping. I feel like it should be more enticing to buy a used book. If new and used are practically the same cost, it seems like the only reason to buy used is ethical (avoid giving amazon money, avoid excess manufacturing of new books). I'm not even sure which is a more ethical choice on the whole. If the new book is sitting in an Amazon warehouse 2 miles from my home and the used book is flying on a plane to me across the continent, maybe used is worse for the world? Does buying new books put more money into the industry and somehow help authors and publishers? How do you choose?


## Data Collection
- **Source:** r/books, manually collected by browsing the front page
- **Labeling process:** manual
- **Label distribution:** reaction: 65, news: 72, discussion: 63 (200 total)

### Difficult-to-label examples
1. "Respect for Friend Drops After Reading Book They Recommended" 
[Onion link] — has a link but is humorous/social, not serious book news. 
Decision: labeled **news** because link present = news, structural rule wins.

2. "What's the last book you read that was so bad it made you angry?" — 
reads like a personal reaction but is structured as a question to the 
community. Decision: labeled **discussion** because soliciting community 
input is the primary purpose.

3. A post mixing a personal book response with a question at the end 
("has anyone else felt this way?"). Decision: labeled **reaction** because 
the personal response was the primary content, the question was incidental.

## Fine-Tuning Approach
- **Base model:** distilbert-base-uncased
- **Training setup:** 3 epochs, learning rate 2e-5, batch size 16
- **Hyperparameter decision:** kept default learning rate of 2e-5, which 
is standard for fine-tuning DistilBERT on small datasets. Increasing it 
risked overshooting on only 200 examples.
- **Split:** 70% train / 15% validation / 15% test (automated by notebook)

## Baseline
- **Model:** Groq llama-3.3-70b-versatile, zero-shot
- **Prompt:** provided label definitions and one example per label, 
instructed model to return only the label name
- **Results collected:** 30/30 parseable responses

## Evaluation Report

### Overall Accuracy
| Model | Accuracy |
|-------|----------|
| Zero-shot baseline (Groq) | 93.3% |
| Fine-tuned DistilBERT | 76.7% |
| Difference | -16.7% (regression) |

### Per-Class Metrics

**Baseline:**
| Label | Precision | Recall | F1 |
|-------|-----------|--------|----|
| reaction | 0.82 | 0.90 | 0.86 |
| news | 1.00 | 1.00 | 1.00 |
| discussion | 0.88 | 0.78 | 0.82 |

**Fine-tuned:**
| Label | Precision | Recall | F1 |
|-------|-----------|--------|----|
| reaction | 0.60 | 0.90 | 0.72 |
| news | 1.00 | 1.00 | 1.00 |
| discussion | 0.75 | 0.33 | 0.46 |

### Confusion Matrix (Fine-Tuned Model)
| | Predicted: reaction | Predicted: news | Predicted: discussion |
|---|---|---|---|
| **True: reaction** | 9 | 0 | 1 |
| **True: news** | 0 | 11 | 0 |
| **True: discussion** | 6 | 0 | 3 |

### Analysis of Wrong Predictions
--- #1 ---
Text:      What's the last book you read that was so bad that it made you angry?
I read The Rebel and the Final Blood War by K.A. Linde and I just hated everything about it! I don't know if the other two books i...
True:      discussion
Predicted: reaction  (confidence: 0.41)
- Why it failed: the word "you" and emotional framing ("made you angry") 
pattern-matches to reaction. The model learned that emotional language = 
reaction, and can't detect that the post is soliciting community responses 
rather than sharing a personal one. Low confidence (0.41) shows the model 
was uncertain.

--- #2 ---
Text:      Reading exclusively on phone
I've been a long time kindle user for digital books. At least 13 years or so now I think. But the last year or so I've been so busy, so it's been hard to read.
True:      discussion
Predicted: reaction  (confidence: 0.39)
- Why it failed: the post opens with a personal experience, which the 
model reads as reaction. The community-facing question comes later. The 
model weighted the opening framing too heavily.

--- #3 ---
Text:      Where are the most unexpected places you’ve found your favorite authors?
I have been reading about the history of nuclear weapons for about a decade. Some of the books I enjoyed most were by the autho...
True:      discussion
Predicted: reaction  (confidence: 0.36)
- Why it failed: despite being a clear question, the personal anecdote 
in the body confused the model. All three wrong predictions have very 
low confidence, suggesting the model knew it was uncertain but defaulted 
to reaction anyway.

### Sample Classifications
| Post (truncated) | Predicted Label | Confidence |
|------------------|----------------|------------|
| "Just finished The Princess Bride and I'm devastated..." | reaction | 0.91 |
| "Judy Blume says she's done writing https://www.npr.org/2026/06/20/nx-s1-5825725/judy-blume-scott-simon" | news | 0.99 |
| "Buying used books on Amazon — how do you choose?" | discussion | 0.68 |
| "What's the last book that made you angry?" | reaction ❌ | 0.41 |
The reaction prediction for "Just finished The Princess Bride" is 
reasonable — the post is entirely personal and emotional with no 
question or external link, which is exactly what the reaction label captures.


### What the Model Learned vs. What I Intended
I intended the model to distinguish posts by their primary purpose: 
sharing a feeling (reaction), linking to news (news), or prompting 
a coversation or question responses (discussion). The model learned this well for news (perfect score) because the signal is structural with a link being present or not. For reaction vs discussion, this is where thel ine got blurry. My intention was for the model to detect the intent of the post, whether it was simply to share reaction and feelings or to prompt a discussion of some sort.  Since many discussion posts open with a personal anecdote before asking a question, the model consistently misread them as reactions. The decision boundary it learned is narrower than what I intended.

### Reflection on Fine-Tuning Regression
The fine-tuned model performed worse than the zero-shot baseline (76.7% 
vs 93.3%). This is probably because 200 examples is a small dataset so the discussion/reaction overlap ending up confusing the model and giving the model inconsistent signal. The larger Groq model handles the discussion/reaction boundary better because it understands communicative intent from pretraining on vast text, whereas DistilBERT had only 140 labeled examples to learn from.

## Spec Reflection
The spec helped most during label design. Having to clearly describe each label and provide examples for it forced me to actually differentiate between the 3 labels and what would fit within each one. This helped prevent a lot of confusion later on when i was actually classifying everything. My implementation was different because the fine-tuned model did worse than the baseline. In the future, I would collect more discussion examples and actually put in notes to give the model cleaner signal 
on the boundaries.

## AI Usage
1. **Label stress-testing:** used Claude to test boundary cases between 
reaction and discussion during planning, which led to the decision rule 
about posts that mix personal response with a community question.
2. **Failure analysis:** pasted misclassified examples into Claude to 
identify patterns. Claude identified that all discussion errors involved 
posts opening with personal anecdotes — verified this manually by 
re-reading each wrong prediction.

Video Link: 
https://youtu.be/MXD7GZGweok