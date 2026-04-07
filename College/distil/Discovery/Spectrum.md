This is a rich and genuinely underexplored research intersection. Let me give you a structured overview of where the field currently stands and where your work could fit.

---

## Terminology: What the field calls it

The term you're looking for doesn't have a single settled name yet — which is actually an opportunity for you to contribute definitively. Here's the current landscape:

**Narrative Engagement (NE)** is probably the most established term right now. Research by Margulis et al. developed a Narrative Engagement scale specifically to measure the degree to which listeners imagine a story as wordless music unfolds, and found it was associated with enjoyment across cultures.

**"Semantic affordances"** is another useful framing — research has shown that music has semantic affordances shaped by culture, and that listeners often experience a kind of "distributed attentional focus" where attention rests more on an associated set of multisensory imaginings, including visual imagery, autobiographical memories, or kinesthetic sensations.

**"Topicality"** vs. **"Contrast"** is a key distinction in this literature. Research has investigated whether narrative dimensions of music derive from learned extramusical associations within a culture (Topicality) or from less experience-dependent structural elements like musical contrast — and found that for Western listeners, greater Topicality, rather than greater Contrast, increases narrative engagement. This is highly relevant to your "ambiguity spectrum" idea.

**"Referential ambiguity"** comes from aesthetics research. Ambiguity — the capacity to sustain multiple interpretations — has been identified as a central characteristic of art, with studies showing that moderate levels of ambiguity can elevate artistic appreciation, while music's semantic resonances are described as "famously vague."

Other terms worth exploring in your literature review: _musical narrativity_, _proto-narrative_ (Nattiez), _narrative affordance_, and _imagistic listening_.

---

## The spectrum you're describing

You're essentially looking for a spectrum from **high semantic constraint** (lyrics, strong topical associations, clear programmatic content) to **high semantic openness** (ambient, atonal, or drone-based music with minimal referential anchors). Some useful framing from the research:

- **Lyrics** are the strongest constraint — they semantically "close off" alternative narrativizations.
- **Topicality** (culturally learned associations — e.g., bugle calls → military scenes) constrains narrativization within a culture but is opaque across cultures.
- **Contrast** (structural drama, tension/resolution) appears to scaffold narrativization somewhat independently of culture.
- **Ambient/drone music** sits at the maximally open end — it provides a kind of blank canvas, but may also fail to _initiate_ narrative engagement at all.

One study found that music with a "Nurturing" programme elicits mostly emotional-scenic imagery but doesn't drive narrative plot — suggesting such music creates an emotional-scenic background without having a true narrative structure or function. This is a crucial nuance: there may be an _inverted U-shape_ where highly ambiguous music actually suppresses narrativization, not just unlocks it.

---

## The MIR angle: audio features for narrativization

This is where your work would be genuinely novel — no one has built a recommendation system around a _narrativization score_. Here's what the existing feature literature suggests would be relevant:

**Features likely to correlate with low narrative openness (high constraint):**

- Presence of vocals/lyrics (voice activity detection, vocal melody salience)
- Strong melodic contour (clear intervallic progressions, narrow pitch variance)
- High topicality proxies (genre labels like "march," "lullaby," "hymn" — though these are metadata, not audio features)
- Rhythmic clarity and metric regularity

**Features likely to correlate with high narrative openness:**

- Timbral diffuseness (low spectral centroid variance, high spectral flux)
- Harmonic ambiguity (avoidance of functional harmonic cadences, high chromaticism, modal or atonal material)
- Slow structural change (low onset density, long note durations)
- Minimal melodic salience (ambient pads, lack of salient foreground melody)
- Dynamic plateau (low loudness variation over time)

The MIR literature divides features broadly into low-level frame-based measures (MFCCs, spectral centroid, zero-crossing rate, roughness) and mid-level features with longer analysis windows (beat strength, rhythmic regularity, meter, mode, harmony) — with semantic descriptions like emotional expression sitting at a third level.

---

## How to structure your research

A viable two-phase design might look like this:

**Phase 1 — Construct and validate a "Narrative Openness" (NO) score**

1. Curate a stimulus set spanning the spectrum (ambient drone → programmatic orchestral → pop with lyrics)
2. Run a listener study using the existing NE scale _plus_ free-response story descriptions
3. Use NLP tools (as in the PNAS 2022 paper by Margulis et al.) to measure _intersubjectivity_ — the degree to which listeners generate similar vs. divergent stories. High intersubjectivity = low openness; high story divergence = high openness.
4. This gives you a continuous NO score for each track.

**Phase 2 — Model NO from audio features**

1. Extract low- and mid-level features (using librosa, Essentia) from your tracks
2. Regress your NO scores on audio features to find the best predictors
3. Use this model to score a larger corpus and build a similarity-based recommender

**Key related work to read:**

- Margulis et al. (2019) — _Nature Human Behaviour_ — Narrative Engagement scale
- Margulis et al. (2022) — _PNAS_ — free-response stories and NLP-measured intersubjectivity
- Eerola & Vuoskoski (perceptual features for emotion in MIR)
- Byron Almén (2008), _A Theory of Musical Narrative_ — musicological grounding
- Nattiez — "proto-narrative" in music

---

## The open gap you can fill

As of the most recent large-scale work, no rigorous study had yet specifically focused on the _content_ of stories imagined in response to music, nor on what audio signal properties predict the degree of narrative intersubjectivity. Your proposal — using audio features to predict a track's _position on the narrative openness spectrum_ and building a recommender around that — is genuinely new territory that sits at the intersection of music psychology, MIR, and NLP. The computational operationalization of narrative openness as a continuous, audio-derivable feature is the contribution that doesn't yet exist.

Would you like me to help you draft a literature review structure, identify datasets, or sketch out the experimental design in more detail?