## Naturalistic text sources where people write in response to music

The key distinction to hold in mind is between sources where people write _about how a piece sounds_ (semantic description) vs. where they write _the story or scene they imagined_ (narrative response). Both are useful, but the latter is a much stronger signal for your research question.

---

### Tier 1 — Direct narrative responses (what you actually want)

**YouTube comments on ambient/instrumental uploads** This is your richest and most underexplored source. Ambient and instrumental YouTube videos — especially long uploads like "4 hours of Dark Souls ambience" or Brian Eno's _Music for Airports_ — accumulate comments that are overwhelmingly imagistic and narrative. Phrases like "this makes me feel like I'm walking through a fog-covered forest" or "I imagine an abandoned train station at dawn" appear naturally and in volume. Crucially, the _variability_ of these comments across listeners is directly what you want to measure as your intersubjectivity proxy. The YouTube Data API v3 gives you programmatic access to comments at scale with no scraping needed — it's a free, official API with a reasonable quota. You can pull comment threads for any video by ID.

The key methodological step is filtering comments for narrative content. A simple heuristic — comments containing phrases like "I imagine," "sounds like," "makes me think of," "feels like," "reminds me of," "I picture," "I feel like I'm," "this is like being in" — will filter the signal from the noise. You can also use a lightweight zero-shot NLP classifier to separate imagistic/narrative comments from generic praise ("beautiful," "so relaxing").

**Bandcamp album descriptions and listener reviews** Bandcamp is exceptional for this because ambient artists write long, narrative-laden descriptions for their own music ("recorded during a winter walk through the forests of northern Finland, this album evokes..."), and listeners write equally evocative reviews in response. The artist-written descriptions give you the _intended_ narrative context; the listener reviews give you the _received_ narrative response. The divergence between them — or between different listener reviews — is a direct measure of narrative openness. Bandcamp has no official API, but its HTML is clean and consistently structured, making it very scrapable with `requests` + `BeautifulSoup`.

**Reddit: r/ambientmusic, r/postrock, r/soundscapes, r/musicsuggestions** Subreddits like r/ambientmusic are full of posts in the form "looking for music that sounds like [description of a scene or mood]" and recommendation threads where people describe what a track makes them imagine. These are naturalistic narrative responses. Reddit's Pushshift archive (and the official Reddit API, though now rate-limited) lets you pull these at scale. The search query `"sounds like" OR "makes me feel like" OR "I imagine" site:reddit.com/r/ambientmusic` gives you a starting list.

There's also a whole category of YouTube channels that explicitly invite this: some ambience channels include an "Ambience Storyline" — short stories written by the creator to help set the scene — and comments on those videos often contain listeners' own competing or extending narratives. [The Angry Noodle](https://theangrynoodle.com/top-10-ambience-channels-on-youtube-for-writers/) This is a particularly clean signal because the channel's storyline gives you a _ground truth_ intended narrative, and comment divergence tells you how open the music actually is to alternative interpretations.

---

### Tier 2 — Descriptive writing with imagistic content (useful but noisier)

**RateYourMusic (RYM) / Sonemic reviews** RYM has long-form written reviews for hundreds of thousands of albums, including the full ambient/experimental/drone catalog. Reviewers routinely write evocative scene descriptions ("this sounds like the moment before a thunderstorm in a concrete city"), making these reviews a proxy for the imagistic associations a piece affords. The challenge is that RYM reviews are written by enthusiasts with high musical literacy, which may bias toward culturally specific topicality rather than open-ended imagination. RYM has no public API, but its pages are scrapable; just be mindful of rate limits and terms of service.

**Pitchfork, The Wire, Resident Advisor reviews** Music journalism reviews of ambient and experimental albums are rich with scene-setting language that reflects the imagistic content of the music. These are single-author responses (not multi-listener), so you can't use them to measure intersubjectivity directly — but they're useful as a first-pass label of "what kind of imagery does this music typically elicit." They're also cleanly structured HTML.

**Last.fm wiki and artist bios** Last.fm generates a wiki-editable artist page for every artist that has been scrobbled, where users contribute biographical and descriptive information. [Wikipedia](https://en.wikipedia.org/wiki/Last.fm) For ambient artists, these wikis often contain evocative descriptions of the music's imagistic quality. Last.fm has a public API that gives access to artist tags, similar artists, and wiki text — no scraping required.

---

### Tier 3 — Structured but thinner (useful for scale, not depth)

**Last.fm tags** Last.fm's user-generated tag system for ambient music produces tags like "atmospheric," "meditative," "cinematic," "haunting," "dreamlike," "fog," "winter," "oceanic" — which are compressed imagistic labels. These are weaker than full sentences but operate at huge scale (millions of tracks). The Last.fm API gives you tag data directly. Tags can function as a weak proxy: a track tagged "cinematic" and "haunting" has different narrative affordances than one tagged "minimal" and "drone."

**Genius annotations on instrumental/ambient tracks** Genius has an annotation system where listeners write explanations and interpretations of tracks. For instrumental and ambient tracks, these annotations are often imagistic rather than lyric-exegetic. The Genius API is publicly accessible.

---

### The crucial methodological point

The most powerful thing you can do with these sources isn't just to collect descriptions — it's to use **semantic similarity across multiple listeners' comments for the same track** as your NO score. This is exactly the methodology in the 2022 Margulis PNAS paper, but applied to naturally occurring internet text rather than lab-elicited responses. Margulis et al. projected listener narrative descriptions into a pretrained word2vec embedding space and measured within-excerpt vs. across-excerpt semantic similarity using a permutation-based approach. [PubMed Central](https://pmc.ncbi.nlm.nih.gov/articles/PMC8795501/) You can do the same with modern sentence transformers (e.g., `sentence-transformers` from HuggingFace) applied to YouTube comments or Bandcamp reviews. Tracks whose comments cluster tightly in semantic space = low narrative openness; tracks whose comments scatter widely = high narrative openness.

The pipeline in practice:

1. Pull 20–50 narrative comments per ambient track from YouTube
2. Filter for imagistic content with a keyword/classifier heuristic
3. Embed each comment with a sentence transformer
4. Compute mean pairwise cosine similarity across comments for that track → this is your NO score
5. Correlate NO scores with audio features extracted with librosa/Essentia

This is a genuinely novel operationalization of narrative openness from naturalistic data, and the combination of YouTube comments + sentence transformer similarity is something no published study has done in this domain yet.