Create a concise presentation overview before any slides are produced.

First ask the user for:

1. Presentation length.
   - Ask how long the presentation should be, such as `5 minutes`, `15 minutes`, `30 minutes`, or `1 hour`.
2. Topic.
   - Ask what the presentation is about.
3. Theme.
   - Ask for the desired theme, style, audience, or tone.

If the user already provided any of these, do not ask again for that item. Ask only for missing inputs.

After all inputs are known:

- Estimate an appropriate slide count from the requested duration.
- Generate a concise slide-by-slide overview in bullet points.
- Keep each slide description to one short line.
- Include the intended purpose of each slide, not full slide copy.
- Keep the structure practical for Codex to turn into slides later.
- Do not generate slide visuals, speaker notes, images, PPT files, PDFs, or Google Slides yet.

Use this output format:

```markdown
# Slide Overview

- Length: [requested length]
- Estimated slides: [number]
- Topic: [topic]
- Theme: [theme]

## Outline

1. [Slide title] - [one-line purpose]
2. [Slide title] - [one-line purpose]
3. [Slide title] - [one-line purpose]
```

End by asking the user whether they want to revise the overview or continue to slide design.
