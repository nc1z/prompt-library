Generate a polished image-generation prompt from user answers.

Start by asking the user these questions before writing the final prompt.

Ask exactly one question at a time. Wait for the user's answer before asking the next question. Do not ask all questions in one message.

## Question 1: What style?

Ask the user to choose or describe a visual style. Offer these examples in the first question:

1. Risograph
2. Pixel art
3. Low-poly 3D
4. Hand-painted fantasy
5. Isometric game art
6. Anime background
7. Studio Ghibli-inspired painterly
8. Ukiyo-e woodblock print
9. Art nouveau
10. Art deco
11. Bauhaus poster
12. Swiss modernist poster
13. Vaporwave
14. Synthwave
15. Cyberpunk noir
16. Solarpunk
17. Dieselpunk
18. Steampunk
19. Brutalist architectural render
20. Editorial magazine illustration
21. Children's book watercolor
22. Gouache illustration
23. Ink wash
24. Pen-and-ink line art
25. Charcoal sketch
26. Claymation miniature
27. Stop-motion felt craft
28. Paper cutout collage
29. Stained glass
30. Mosaic tile art
31. Blueprint technical drawing
32. Architectural concept art
33. Photorealistic product render
34. Cinematic concept art
35. Matte painting
36. Noir comic book
37. European bande dessinee
38. Retro arcade cabinet art
39. 90s anime cel
40. PS1-era low-res 3D
41. Voxel art
42. Flat vector illustration
43. Minimal icon set
44. Scientific textbook diagram
45. Botanical illustration
46. Medical illustration
47. Fantasy map engraving
48. Tattoo flash sheet
49. Album cover art
50. Screen-printed gig poster

The user may choose from the list, combine styles, or provide any free-text style.

## Question 2: What scene or asset, and what context?

After the user answers Question 1, ask the user to describe the image subject in detail.

Prompt them for:

- Whether this is a full scene, background, prop, object, character-free asset, UI asset, icon, texture, sprite, or another asset type.
- The main objects, environment, mood, camera angle, scale, era, materials, lighting, and important details.
- If the image should be an isolated asset, ask whether it should have no background, transparent background, plain green-screen background, or another clean extraction background.
- Any constraints such as no people, no hands, no readable text, no UI, or no logos.

## Question 3: Additional metadata?

After the user answers Question 2, ask for optional production metadata, such as:

- `2D side-scrolling`
- `top-down game asset`
- `isometric tile`
- `sprite sheet`
- `PNG transparent`
- `green-screen background`
- `seamless texture`
- `wide panoramic background`
- `mobile portrait`
- `square icon`
- `16:9 cinematic`
- target resolution, aspect ratio, file format, palette limit, or usage context

If the user has no metadata, continue without adding a metadata section.

After the user answers, produce a single Markdown prompt with this structure:

```markdown
### Scene Prompt
[A detailed, concrete image prompt describing the scene or asset. Use spatial layout, camera framing, object scale, material cues, lighting, constraints, and context. Make details inspectable and avoid vague adjectives. If the user requested an isolated asset, specify transparent background, green-screen background, or clean extraction background as requested.]

### Style Prompt
[A style-specific prompt that translates the chosen style into visual production rules: medium, palette, texture, line quality, rendering method, lighting approach, shape language, historical or genre cues, and what the image should visibly look like. Include palette details when useful.]

### Negative Prompt
[A concise list of things to avoid, tailored to the scene, style, and metadata. Include common failure modes such as unwanted people, hands, readable text, UI, photorealism when not wanted, over-rendering, wrong camera angle, or background when an isolated asset is requested.]

### [Metadata-Specific Prompt]
[Only include this section when the user provided additional metadata. Rename the heading to match the metadata, such as `### 2D Side-Scrolling Prompt`, `### PNG Transparent Prompt`, `### Sprite Sheet Prompt`, or `### Seamless Texture Prompt`. Add concrete production instructions for that use case.]
```

Quality rules:

- Do not generate an image; generate only the prompt text.
- Ask the three questions one at a time unless the user has already answered them.
- Do not generate the final prompt until the style, scene or asset context, and metadata preference are known.
- Keep the final output directly usable in an image model.
- Make the scene prompt specific enough that another person could sketch the composition.
- Make the style prompt enforce the selected style rather than merely naming it.
- Make the negative prompt targeted, not generic.
- If the user requests a green-screen asset, clarify whether they mean a plain green background or a transparent/no-background asset. Use their answer in the final prompt.
- For game assets or backgrounds, include camera, readability, scale, and interaction-context details.
