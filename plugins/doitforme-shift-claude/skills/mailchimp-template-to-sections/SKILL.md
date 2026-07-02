---
name: mailchimp-template-to-sections
description: Convert a full email template image into sectioned, editable Mailchimp HTML with CTA extraction. Use when users want image-based campaign reconstruction into maintainable markup.
---

# Mailchimp Template To Sections

Use this skill whenever a user wants campaign content built from a full image template in Mailchimp.

## Goal

Turn one full template image into clean, maintainable campaign HTML by:

1. Splitting the template into visual sections.
2. Detecting CTA regions and converting them into real HTML buttons.
3. Keeping sections separately linkable (one link per section when needed).
4. Producing HTML that is easy for humans to read and edit.

## When To Trigger

Trigger this skill if the user asks for any of the following:

- "use image template"
- "split template into sections"
- "recognize CTAs/buttons"
- "make content from this image"
- "add links per section"

## Required Inputs

- Full template image (`image_path` or direct image URL).
- Campaign id (if updating an existing draft).
- Optional brand constraints:
  - Background color
  - Button style
  - Max content width

## Default Output Rules

- Use table-based, email-safe HTML.
- Keep structure simple and readable (indented blocks, one section per row).
- Keep image blocks separate (`<tr><td><img ... /></td></tr>`).
- Use real CTA buttons (`<a ...>`) instead of image-only buttons when possible.
- Keep buttons visually consistent unless the user asks otherwise.
- Do not ship placeholder links silently; ask for URLs first or use `#` and clearly mark them.

## Workflow

1. Get the full template image.
2. Identify sections top-to-bottom.
3. Detect CTAs and replace visual button zones with HTML buttons.
4. Upload section images and keep section-to-url mapping.
5. Ask for missing URLs.
6. Build readable, sectioned HTML.
7. Apply to draft with `mailchimp_set_campaign_content`.

## HTML Pattern

```html
<table role="presentation" width="100%" ...>
  <tr>
    <td align="center">
      <table role="presentation" width="100%" style="max-width:620px;">
        <tr><td><img src="SECTION_URL" ... /></td></tr>
        <tr><td align="center"><a href="CTA_URL" ...>CTA Text</a></td></tr>
        <tr><td><img src="SECTION_URL" ... /></td></tr>
      </table>
    </td>
  </tr>
</table>
```

If a section needs its own link:

```html
<a href="SECTION_LINK"><img src="SECTION_URL" ... /></a>
```

## Link prompt template

- `Contact us button URL:`
- `See more button URL:`
- `Section links (optional):`
  - `section-01:`
  - `section-02:`
  - `section-03:`

## Quality checklist

- Background color matches creative.
- Buttons use consistent style.
- No accidental white gaps between slices.
- Mobile scales properly (`width:100%; max-width:...; height:auto;`).
- HTML remains readable and clearly sectioned.
