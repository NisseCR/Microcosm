Good — now I have a solid picture of the available dataset landscape. Let me give you a structured, practical breakdown.

---

## Dataset inventory for your research

The core challenge is that no existing dataset was built _for_ narrative openness or narrativization. Your methodological task is to **reuse or combine** existing datasets and derive a proxy for narrative openness from what's already annotated. Here's how the available resources map onto your needs:

---

### Tier 1 — Directly Relevant (emotion/perception annotations + audio)

**Eerola & Vuoskoski Film Soundtrack Dataset (2011)** This is probably your most important starting point. It contains short excerpts (approximately 15 seconds) from film soundtracks, selected to span both discrete emotion categories and dimensional affect space, rated by 12 expert musicologists and validated with 116 university students. Crucially for your work, film music is specifically designed to scaffold narrative imagination, and the tracks are relatively unfamiliar to listeners (reducing topicality bias). The dataset is openly hosted on the Open Science Framework at osf.io/p6vkg. The emotion ratings it contains won't directly give you a narrative openness score, but the variance of emotion ratings across participants — i.e., how much listeners _disagree_ about what emotion a track conveys — is a reasonable proxy for semantic ambiguity and thus narrative openness. High inter-rater agreement = low openness; high disagreement = high openness.

**TROMPA-MER Dataset** This openly available dataset contains diverse categorical annotations accounting for subjectivity, includes additional annotator information (spoken languages, current mood, birthplace), diverse non-Western music, and free-text annotations from listeners in their native languages — and some excerpts have up to 143 individual annotations. The free-text annotation field is particularly valuable: you could mine those responses with NLP to look for narrative content, similar to what Margulis et al. did with story descriptions. The underlying music comes from Muziekweb, the Dutch national music library, which offers 600,000 CDs' worth of material.

**EMMA (Emotion-to-Music Mapping Atlas)** This database contains 364 music excerpts from classical, pop, and hip-hop genres, rated for felt emotion using the Geneva Emotion Music Scale (GEMS) by an average of about 29 participants per excerpt. The GEMS includes dimensions like "wonder," "transcendence," and "peacefulness," which are closely related to the kind of diffuse, imagistic states that precede narrative engagement. Inter-rater variance here is again a useful proxy for your work.

---

### Tier 2 — Audio + Tags (no emotion annotations, but scale and breadth)

**FMA — Free Music Archive** The FMA provides 917 GiB and 343 days of Creative Commons-licensed audio from 106,574 tracks, with pre-computed audio features, metadata, tags, and free-form text such as artist biographies, organized in a hierarchical taxonomy of 161 genres. Critically for your purposes, FMA includes genres like Ambient, Ambient Electronic, Experimental, Electroacoustic, Soundtrack, and Instrumental — which naturally populate the high-narrative-openness end of your spectrum. You can use this corpus to build and test your audio feature model once you've trained it on a smaller labeled set.

**MagnaTagATune** This dataset contains approximately 25,000 clips of 29 seconds each, annotated with 188 tags collected through a crowdsourced game, with an accompanying Echo Nest analysis of rhythm, pitch, and timbre for each track. The 188 tags include perceptual descriptors like "dreamy," "ambient," "calm," "ethereal," "atmospheric," and "no vocals" — tags that cluster toward narrative openness — as well as "fast," "energetic," "drums," "vocals," "singer" — tags that cluster toward the constrained end. You could construct a weak label for narrative openness directly from the tag distributions, without any new listener study.

**MGPHot (Music Genome Project)** Released in 2025, this dataset contains 58 expert musicological attributes per song drawn from the Music Genome Project's methodology, covering rhythm, instrumentation, harmony, vocals, compositional focus, and lyrics. The lyrics-related and melodic-salience attributes in this dataset map directly onto your "semantic constraint" dimension. Attribute scores quantify things like vocal presence, lyrical density, and harmonic complexity — all proxies for where a track sits on your spectrum.

---

### Tier 3 — Adjacent/Complementary

**OpenMIIR (EEG during music perception and imagination)** This public-domain EEG dataset captures brain activity during both music perception and imagination, recorded while participants listened to and then mentally reproduced 12 short familiar music fragments spanning different genres and musical dimensions including meter, tempo, and lyrics presence. This is a niche but theoretically rich resource — the difference in neural signal between _perception_ and _imagination_ conditions is directly relevant to how strongly a track invites mental imagery.

---

### Suggested strategy without a listener study

Given these resources, a viable no-listener-study approach would proceed in three steps:

1. **Construct a proxy Narrative Openness score** using the Eerola & Vuoskoski dataset. For each track, compute the _entropy_ of emotion ratings across participants. Tracks where everyone agrees (low entropy) are semantically constrained; tracks where opinions are highly distributed (high entropy) are more open. Validate this proxy by checking whether it correlates with intuitive properties — ambient instrumentals should score high, marches and lullabies low.
    
2. **Train a feature model** on the entropy-derived scores using MagnaTagATune or FMA audio features. Useful features to experiment with: spectral flatness, chroma variance, MFCC variance over time, onset density, vocal activity detection, and harmonic change rate.
    
3. **Build the recommender** on FMA's full corpus using your trained model to score each track, then apply similarity search within bins of the narrative openness spectrum — so that a user can be recommended tracks at a similar "imagination latitude."
    

The main methodological contribution you'd be making is operationalizing inter-rater emotion entropy as a proxy for narrative openness, which no one has explicitly done before.