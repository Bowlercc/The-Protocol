# THE PROTOCOL

**A response toolkit for the infertility and childlessness community.**

When someone says "just relax" or "have you tried adopting?", you'll have words ready.

---

## What Is This?

The Protocol is an interactive tool that helps people navigating infertility or involuntary childlessness find the right words when faced with well-meaning but hurtful comments.

It provides calibrated responses across:
- **4 tones**: Diplomat, Critic, Absurdist, Realist
- **3 spice levels**: Mild, Medium, Hot
- **2 perspectives**: I (individual) or We (couple)
- **4 contexts**: Stranger, Family, Colleague, Medical

The goal isn't to blow up relationships, it's to educate, set boundaries, and help people feel seen.

---

## Quick Start

1. Open `index.html` in a browser (landing page)
2. Click "I'M READY" to enter the app
3. Type what someone said (or click a common trigger)
4. Choose your voice/tone
5. Get a response you can use, adapt, or just feel validated by
6. Export as an image to share

---

## File Structure

```
the-protocol/
├── index.html      # Landing page
├── app.html        # Main application
├── triggers.json   # Response database (38 triggers, 1400+ responses)
├── admin.html      # Admin panel for editing triggers
└── README.md       # This file
```

---

## Features

### Main App (`app.html`)

- **Smart fuzzy matching**: Matches input to triggers even with typos, filler words, or variations
- **Response variety**: Multiple responses per tone/spice combo with "New Take" rotation
- **Perspective switching**: Toggle between "I" and "We" pronouns
- **Image export**: Share cards in Story (9:16), Square (1:1), or Wide (16:9) formats
- **QR code on exports**: Links back to the site (toggleable)
- **"Why this hurts" export**: Educational companion card explaining the impact (toggleable)
- **Copy to clipboard**: One-click copy of response text
- **Dark/Light themes**: Toggle with the sun/moon button
- **Mobile responsive**: Works on all devices
- **Tutorial**: First-time user walkthrough

### Admin Panel (`admin.html`)

- Browse and search all triggers
- Edit trigger phrases, subtext, and background images
- Edit all responses in a visual grid
- Add new triggers
- Delete triggers
- Export modified JSON

---

## Response Format

Responses use placeholder syntax for perspective switching:

| Placeholder | "I" mode | "We" mode |
|-------------|----------|-----------|
| `{I}` | I | We |
| `{I}'m` | I'm | We're |
| `{I}'ve` | I've | We've |
| `{We}` | I | We |
| `{We're}` | I'm | We're |
| `{my}` | my | our |
| `{My}` | My | Our |
| `{me}` | me | us |
| `{our}` | my | our |
| `{us}` | me | us |

**Example:**
```
"{I}'ve been through IVF. {My} body remembers it vividly."

→ I mode: "I've been through IVF. My body remembers it vividly."
→ We mode: "We've been through IVF. Our body remembers it vividly."
```

---

## Trigger Data Structure

Each trigger in `triggers.json` follows this schema:

```json
{
  "triggers": ["phrase 1", "phrase 2", "..."],
  "subtext": "The hidden meaning behind what they said",
  "image": "https://images.unsplash.com/...",
  "why_it_hurts": "Explanation of the emotional impact (for education card)",
  "what_to_know": "What actually helps instead (for education card)",
  "responses": {
    "diplomat": {
      "mild": ["response 1", "response 2"],
      "medium": ["response 1", "response 2", "response 3"],
      "hot": ["response 1", "response 2", "response 3", "response 4", "response 5"]
    },
    "critic": { ... },
    "absurdist": { ... },
    "realist": { ... }
  }
}
```

### Fields

| Field | Description | Required |
|-------|-------------|----------|
| `triggers` | Array of phrases that match this trigger (first is primary) | ✅ Yes |
| `subtext` | The implied meaning, what they're really saying | ✅ Yes |
| `image` | Unsplash URL for background (grayscale filtered in app) | ✅ Yes |
| `why_it_hurts` | Explanation for the "Why this hurts" education card | ⚠️ Optional* |
| `what_to_know` | What helps instead, for the education card | ⚠️ Optional* |
| `responses` | Object with 4 tones, each containing 3 spice levels | ✅ Yes |

*If not provided, default text is used for the education card export.

---

## Tone Descriptions

| Tone | Description |
|------|-------------|
| **Diplomat** | Graceful deflection. Maintains relationships while setting boundaries. |
| **Critic** | Direct challenge. Factual, pointed, doesn't suffer fools. |
| **Absurdist** | Sardonic humor. Makes the ridiculous obvious through wit. |
| **Realist** | Honest and grounded. No games, just truth. |

---

## Spice Levels

| Level | Description |
|-------|-------------|
| **Mild** | Safe for any audience. Polite but clear. |
| **Medium** | More direct. Some edge, still professional. |
| **Hot** | Full honesty. May cause discomfort. Use wisely. |

---

## Content Gaps (Needs Your Attention)

### "Why This Hurts" Education Cards

The dual-export feature creates a second image explaining why a comment hurts. This requires two new fields per trigger:

| Field | Purpose | Example |
|-------|---------|---------|
| `why_it_hurts` | Explains the emotional impact | "This implies infertility is caused by stress or that the person is somehow responsible..." |
| `what_to_know` | What actually helps instead | "Instead of offering advice, try: 'I'm here for you' or 'That sounds really hard.'" |

**Current status:** Only 3 triggers have this content filled in:
- ✅ "just relax"
- ✅ "why don't you adopt"
- ✅ "everything happens for a reason"

**All other triggers use default fallback text.** To make the education cards more powerful and specific, add these fields to each trigger.

#### How to Add

**Option 1: Edit triggers.json directly**
```json
{
  "triggers": ["have you tried IVF"],
  "subtext": "...",
  "image": "...",
  "why_it_hurts": "Your explanation here...",
  "what_to_know": "Your helpful alternative here...",
  "responses": { ... }
}
```

**Option 2: Use the admin panel** (if updated to support these fields)

#### Writing Guidelines

When writing `why_it_hurts`:
- Explain the emotional weight, not just why it's factually wrong
- Consider the cumulative effect (they've heard this dozens of times)
- Keep it 2-3 sentences

When writing `what_to_know`:
- Focus on what actually helps
- Offer concrete alternatives ("Instead of X, try Y")
- Keep the tone educational, not accusatory

---

### Fuzzy Matching

The matching system now handles variations like:
- "you just need to relax" → matches "just relax"
- "have you ever tried IVF?" → matches "have you tried IVF"
- Typos: "everthing hapens for a reson" → matches "everything happens for a reason"

**To improve matching for a specific trigger**, add more variations to its `triggers` array:

```json
{
  "triggers": [
    "just relax",
    "need to relax",
    "stop stressing",
    "stress causes infertility",
    "you're too stressed",
    "relax and it will happen"
  ],
  ...
}
```

The more variations you add, the better the matching. The fuzzy system will handle minor differences, but explicit variations help with completely different phrasings of the same idea.

---

## Editing Triggers

### Using the Admin Panel

1. Open `admin.html`
2. Select a trigger from the sidebar (or search)
3. Edit:
   - **Trigger phrases**: Add/remove matching phrases
   - **Subtext**: The hidden meaning
   - **Image URL**: Background for export cards
   - **Responses**: One per line in each textarea
4. Click "Save Trigger" to save in memory
5. Click "Export JSON" to download
6. Replace `triggers.json` with the downloaded file

### Manual Editing

You can also edit `triggers.json` directly in any text editor. Ensure valid JSON syntax.

---

## Adding New Triggers

### Via Admin Panel

1. Click "+ NEW TRIGGER"
2. Fill in the trigger phrases
3. Add subtext and image URL
4. Add responses for each tone/level
5. Export and replace `triggers.json`

### Manually

Add a new object to the `triggers.json` array:

```json
{
  "triggers": ["your new phrase", "alternate phrasing"],
  "subtext": "What they really mean",
  "image": "https://images.unsplash.com/photo-xxx",
  "responses": {
    "diplomat": {
      "mild": ["Response here"],
      "medium": ["Response here"],
      "hot": ["Response here"]
    },
    "critic": {
      "mild": ["Response here"],
      "medium": ["Response here"],
      "hot": ["Response here"]
    },
    "absurdist": {
      "mild": ["Response here"],
      "medium": ["Response here"],
      "hot": ["Response here"]
    },
    "realist": {
      "mild": ["Response here"],
      "medium": ["Response here"],
      "hot": ["Response here"]
    }
  }
}
```

---

## Deployment

The Protocol is static HTML/CSS/JS, no server required.

### Options

1. **Local**: Just open `index.html` in a browser
2. **GitHub Pages**: Push to repo, enable Pages
3. **Netlify/Vercel**: Drag and drop the folder
4. **Any static host**: Upload all files

### Requirements

- Modern browser (Chrome, Firefox, Safari, Edge)
- JavaScript enabled
- Internet connection (for fonts and images)

---

## Customization

### Theming

Edit CSS variables in `app.html`:

```css
:root {
  --void: #080808;        /* Dark background */
  --charcoal: #121212;    /* Secondary dark */
  --bone: #E8E4DF;        /* Primary text */
  --cream: #F5F2ED;        /* Bright text */
  --alert: #C41E1E;       /* Hot/warning color */
  --gold: #B8956A;        /* Accent color */
}
```

### Common Triggers Display

Edit the `COMMON_TRIGGERS` array in `app.html`:

```javascript
const COMMON_TRIGGERS = [
  "Just relax",
  "Why don't you adopt?",
  // ... add or remove as needed
];
```

---

## Philosophy

### Educate, Don't Detonate

The responses are designed to:
- Help the speaker understand why their comment hurts
- Set boundaries without destroying relationships
- Validate the recipient's experience
- Provide options from gentle to firm

### The Subtext Matters

Every trigger includes a "subtext", the hidden meaning. This helps users understand *why* the comment stings, not just *that* it does.

### Multiple Valid Responses

There's no single "right" answer. Different situations call for different approaches. A comment from a stranger at a party needs a different response than the same comment from your mother.

---

## Contributing

This project welcomes contributions from the community.

### Ways to Help

1. **Add responses**: More variety helps everyone
2. **Add triggers**: What comments are we missing?
3. **Improve matching**: Better fuzzy matching logic
4. **Translations**: Adapt for other languages/cultures
5. **Accessibility**: Improve screen reader support

### Guidelines

- Keep responses constructive, not cruel
- Aim to educate, not just vent
- Test perspective switching (`{I}`, `{We}`, etc.)
- Consider all contexts (stranger vs. family)

---

## Credits

Created by **Craig & Courtney** as part of the [Matterhood](https://matterhood.ghost.io/) project.

### Resources

- Fonts: [Google Fonts](https://fonts.google.com) (Playfair Display, Space Mono)
- Images: [Unsplash](https://unsplash.com)
- Export: [html2canvas](https://html2canvas.hertzen.com)

---

## License

This project is shared with love for the infertility and childlessness community.

Use it, adapt it, share it. Just don't be a jerk about it.

---

## Support

If this helped you, consider:
- Sharing with someone who needs it
- Contributing new responses
- Supporting [Matterhood](https://matterhood.ghost.io/)

---

*"You don't owe anyone an explanation. But if you want one ready, we've got you."*
