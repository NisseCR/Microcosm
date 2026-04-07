Great instinct — scraping game and film audio is actually a clever path, because both domains have _existing narrative context_ attached to music that was deliberately composed to facilitate imagination. Let me think through this carefully.This is a genuinely strong research direction. Let me lay out the full picture clearly.

---

## Why game and film audio is strategically ideal for this

Your instinct is correct for a specific reason: game and film music are the only domains where **the narrative context of a track is externally documented** — in wikis, script databases, and fan communities — without requiring you to run a listener study. That external documentation can act as a _surrogate label_ for the narrative properties of the music itself.

There are two distinct things you can scrape, and they serve different purposes in your pipeline:

---

### Source 1: Game wiki soundtrack pages (Fandom, wiki.gg)

As you saw in the Horizon Zero Dawn example, game soundtrack pages on wikis like Fandom document exactly where and when each track plays. The Horizon Zero Dawn wiki, for instance, classifies each track as an "ambient track" and documents which specific narrative scenes and zones it accompanies — whether an open-world exploration theme, a cutscene about a character's backstory, or a combat moment. This gives you a **functional label for each track** at no annotation cost.

The key variable you can extract from this is the **narrative specificity** of the context in which a track plays:

- "Plays during the credits" → very low narrative constraint (open to imagination)
- "Plays in open-world exploration zones at night" → low-medium constraint
- "Plays during the cutscene where Aloy confronts Resh" → high constraint (tied to a specific story beat)
- "Plays during combat with the final boss" → very high constraint (clear emotional and narrative function)

This gives you a **context-specificity axis** directly. Tracks tied to named characters, specific dialogue moments, or boss battles have semantically constrained functions. Tracks labeled as "ambient exploration" or "plays during night in open world" have high narrative openness.

**Scrapable sources:**

- `fandom.com` — large number of games with structured soundtrack pages. A published fandom-wiki scraper exists on GitHub (GOLEM-lab/fandom-wiki) specifically designed to extract structured and unstructured data from fandom.com pages.
- `wiki.gg` — newer, cleaner alternative to Fandom with similar structured soundtrack pages
- VGMdb.net — a specialized video game music database with structured metadata, track listings, and game context per album

**Games with especially well-documented ambient soundtrack wikis:** Skyrim, Fallout series, Hollow Knight, Journey, Dark Souls series, Ori and the Blind Forest, Minecraft, Stardew Valley, Final Fantasy series, Horizon Zero Dawn/Forbidden West.

---

### Source 2: VGMIDI + written descriptions

The VGMIDI dataset already exists as 200 MIDI-labeled piano pieces of video game soundtracks, annotated by 30 human subjects with valence-arousal ratings — and crucially, annotators were asked to write two to three sentences describing each piece they listened to. Those sentences are pre-existing free-text narrative descriptions that you can use directly, without any scraping. This is actually very close to what the Margulis PNAS 2022 study did — and you can apply NLP to those descriptions to measure narrative diversity across listeners (your intersubjectivity proxy).

---

### Source 3: Film music with screenplay/script alignment

Film music is even richer than game music for your purposes because screenplays are often publicly available, and the association between a cue and its scene context is documented. Here's the pipeline:

**Audio:** Scrape OST uploads from YouTube (via `yt-dlp`) for films with open or clearly research-permissible soundtracks, or use what's on FMA's "Soundtrack" genre subset.

**Text:** Scrape scene descriptions from:

- `imsdb.com` (Internet Movie Script Database) — thousands of publicly hosted screenplays
- `scripts.com` and `scriptslug.com`
- Subtitle files from `opensubtitles.org` — which give you timestamped dialogue and scene transitions

The alignment idea: if you have a film, its screenplay, and its OST with timestamps, you can associate each cue with the scene text it accompanies. A cue playing over a wordless landscape shot has very different narrative specificity than one playing under a dialogue-heavy confrontation scene. Scene descriptions in screenplays explicitly mark things like "EXT. EMPTY DESERT — DUSK" (low constraint) vs. "INT. INTERROGATION ROOM — Marcus slams the table" (high constraint). You can operationalize this as a **narrative density score for the accompanying scene text** using standard NLP metrics (sentence length, entity count, named character mentions, verb density).

---

### The unified annotation strategy

Combining these three sources gives you a novel labeling scheme that doesn't require any listener study:

|Source|What you scrape|Label derived|
|---|---|---|
|Game wikis|Track context description ("ambient exploration", "boss fight", "named character cutscene")|Functional constraint score|
|VGMIDI descriptions|2-3 sentence listener impressions per track|NLP-derived narrative divergence|
|Film screenplays + OST cues|Scene text at cue timestamp|Scene narrative density score|

The **narrative openness score** for a track becomes a weighted combination of these. Tracks with low functional constraint + high listener description divergence + low scene narrative density = high narrative openness.

---

### Critical methodological caveat

One thing to watch carefully: game and film music is semantically constrained _by its visual and gameplay context_ — but that constraint is partially _external_ to the music itself. A track that sounds ambient and open-ended might still be tightly attached to a specific story beat in its source game. The ideal next step after scraping would be to check whether your derived NO scores correlate with the audio features as expected — if they don't, it may mean the constraint is context-derived rather than musically intrinsic, which is itself an interesting finding worth publishing on.

For the audio itself, `yt-dlp` with a target list of YouTube OST uploads is the most practical scraping route, combined with `librosa` for feature extraction. The main legal consideration is that you'd be working with audio you don't redistribute — only the extracted features — which is standard research practice in MIR.

Would you like help sketching the actual scraping pipeline in code, or thinking through the NLP scoring of scene texts?