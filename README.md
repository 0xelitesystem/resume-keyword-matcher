# Resume Keyword Matcher

Paste your resume and a job description, get an honest keyword-overlap analysis. One HTML file, no server, no tracking, no external dependencies. Works offline.

## Live demo

https://0xelitesystem.github.io/resume-keyword-matcher/

## Features

- Two inputs: resume text and job description text. Analyze with a click or Ctrl+Enter / Cmd+Enter.
- Keyword-overlap score with the formula stated right under the number, no black box.
- Matched list: terms and phrases that appear in both, with how often the job description mentions them.
- Missing list: terms in the job description but not your resume, ranked by frequency (repeated terms usually matter more), with counts.
- Requirements flag: missing terms that appear in lines that look like hard requirements (lines containing "required", "must", "you have", "qualifications") are flagged so you review those first.
- Phrase matching: 2 to 3 word skills like "machine learning" or "customer service" are matched as phrases, not just single words.
- Variant handling: simple plural folding (apis / api) and a small alias map (js / javascript, ts / typescript, k8s / kubernetes, postgres / postgresql, ci/cd, node.js, front end / frontend, and so on).
- Stopword filtering: common English words plus resume boilerplate ("experience", "team", "strong") are ignored. The lists are plain constants in the source, edit them to taste.
- Load example button with a short sample resume and job description that shows both matched and missing terms.
- Light and dark theme, keyboard-usable, single file.

## How it works

1. Both texts are lowercased and tokenized on non-word boundaries. Punctuation breaks phrases, so a phrase never spans a comma or sentence boundary.
2. Tokens are normalized: aliases are folded (k8s becomes kubernetes), simple plurals are stripped, and multi-part spellings like ci/cd or node.js become single tokens.
3. From the job description, the tool collects every significant single word plus every 2 to 3 word phrase made entirely of significant words. Stopwords and boilerplate are dropped. Redundant subterms are pruned (if "machine learning" is kept, a "machine" entry that only ever appears inside that phrase is dropped, unless your resume matches the single word but misses the phrase, in which case the partial credit stays visible).
4. Each term is checked against the resume's normalized word sequences.

The score, stated plainly:

```
score = (unique significant JD terms found in the resume) / (total unique significant JD terms) x 100
```

Each unique term or phrase counts once toward the score regardless of how often it repeats. Frequency is used only to rank the missing list.

## What it is not

- **Not an ATS simulator.** Real applicant tracking systems parse, weight, and rank resumes in very different ways, and most rejections are humans skimming, not robots filtering. This tool measures one thing: keyword overlap.
- **Not a guarantee.** A high score does not mean you pass any screen, and a low score does not mean you fail one.
- **Not an invitation to keyword-stuff.** Adding terms you cannot back up in an interview is counterproductive. Use the missing list as a checklist of things to mention only if they truthfully describe work you have done.

## Privacy

Everything runs in your browser. Your resume and the job description never leave your machine: no requests, no analytics, no storage. This matters more than usual here, because resumes contain your name, address, phone number, and work history. Verify by opening DevTools and watching the network tab while you analyze.

## License

MIT. Copyright 0xelitesystem 2026.
