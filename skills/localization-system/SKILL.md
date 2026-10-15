---
name: localization-system
description: |
  Use when designing a multi-language UI system (key naming, glossary, per-language rules) or
  when translating or reviewing UI strings. Covers the structure/rules/glossary split, an L10N key
  formula, baseline English and Japanese UI rules, and a translate / review / Q&A operating mode
  with a 5-item review checklist.

  Use this when:
  - Setting up localization for a product for the first time
  - Translating or reviewing UI strings in a source language and target languages
  - Defining or fixing a key naming convention and translation sheet layout
  - Running a translation bot or agent that must stay consistent across requests
---

# Localization System

Multi-language is not translation, it is localization. Do not move the meaning of the source text across; rewrite it so a user of that language reads it naturally. Use this skill to (1) design the system, and (2) translate, review, and answer questions inside it. For the tone of the source-language copy, use `ux-writing-brand-voice`.

## Separate three layers

| Layer | Content | Managed |
|---|---|---|
| Structure | placeholders, tags, variables | shared across languages, never changed |
| Rules | register, casing, punctuation, length | independent per language |
| Glossary | key terms and banned expressions | independent per language, wins over rules |

Mixing the layers makes it unclear what the standard is. Consistency comes from the glossary, not from rules: fix "Login" as ログイン and ban ログオン. The PM or UX writer defines the glossary and rules first; the translator works inside them. If the order is reversed, mixed usage accumulates and is hard to undo.

## Designing the system

1. Scope the system to static UI text (app and web). Dynamic content such as articles and comments is outside the key system.
2. Build the glossary first. Columns: source term, per-language terms (`term_en`, `term_ja`), banned expressions (`ng_*`), notes.
3. Write a rule document per language (baselines below).
4. Define the key convention (below).
5. Use two sheets. The final sheet holds category, key (primary key), and per-language strings. The management sheet holds the inputs for keys: status, module, page, UI, description, per-language strings, a note with translation context, and the key.

### L10N key formula

`[module]-[page]-[ui]-[content]_[ui type]`

- Depths 1 to 3 are location (module, page, UI). The fourth, content identifier, may be omitted.
- The key must end with a UI type identifier: `_title`, `_subtitle`, `_description`, `_link_label`, `_info`, `_label` (most general), `_placeholder`, `_toast`, `_message` (guidance, success, failure, error), `_button_label`.
- Lowercase letters and digits only. `-` separates depths; `_` separates words and the type.
- Examples: `common-gnb-menu-news_label`, `login-form-email_placeholder`, `login-main_title`.
- Variables use braces such as `{value}`, `{year}`; tags use `<one></one>`; line breaks use `\n`.
- The key generator joins module, page, UI, and description with `-`, then reads the note or the source text to pick the right UI type identifier.

## Rules for every language

- Keep `{name}`, `%s`, `{0}`, `{count}`, `%(count)d` and tags such as `<b>`, `<br>`, `\n` exactly as they are. Adjust only the word order and particles around them.
- Keep numbers, units, and symbols in the same position as the source.
- Use glossary terms before anything the context suggests.
- Keep buttons, tabs, and menus as short as possible, and similar in length within a group.
- Use one expression per concept, and one register per message type on a screen. Do not change a chosen wording without a strong reason.
- Do not add meaning that is not in the source.
- If the source is ambiguous, do not guess. Pick the safest, shortest wording common in that language's UI, and leave a comment asking for confirmation when it cannot be settled. Short beats guessed.
- Do not judge marketing copy, advertising, legal, or medical text. Escalate to a person.

## Baseline rules by language (the service's own rules win)

**English**
- Neutral, friendly product tone. No slang, idioms, or culture-specific jokes. American spelling by default.
- Buttons and labels: concise imperative verbs or short noun phrases (Save, Cancel, Log in, Sign up). Do not mix Remove and Delete for the same action.
- Instructions and errors: short sentences, polite but not apologetic or emotional. Say what the user should do next, not the system's internal story.
- No period on buttons or short labels; a period on full sentences. At most one exclamation mark, usually none. Pick Title Case or Sentence case and keep it product-wide.

**Japanese**
- Default to the です・ます polite form. Short labels may use a noun ending or a short verb form. Keep one register per screen and message type; no casual endings or emoticons.
- Buttons: short and direct (保存, キャンセル, 削除, 設定, ログイン, 新規登録). Prefer expressions common in Japanese products over literal English. Do not mix 削除 and 消去 for one meaning.
- Prefer kanji, but keep katakana where it is the UI norm. Fix one katakana spelling per concept.
- Errors and guidance: one or two polite sentences, for example 保存に失敗しました。しばらくしてからもう一度お試しください。
- When unsure, take vocabulary and length from large Japanese web services, without copying specific sentences. For news and portal screens, avoid casual or literary phrasing.

For another language, write its rules with the same frame: tone, buttons, messages, terms, spelling, length, placeholders, ambiguity, consistency. Do not map the source language's rules one to one.

## Operating as a translation bot or agent

Classify each request first: **translate** (source to target), **review** (existing translation), or **question** (about a word, expression, or rule). Input can be a line list, a pasted spreadsheet range, or a natural-language question. If intent or target language is unclear, ask the minimum and continue.

- Translate output: a markdown table with source, EN, JA columns.
- Review output: a problem summary list plus a fix table (current / suggested / reason). End with a short table of key terms used in this text.
- Question output: explain from the rules and glossary.
- If the user looks up a term, search the glossary rows that contain it and reuse them.

### Review checklist (5 items)

1. Are required glossary terms used correctly, and are banned expressions absent?
2. Are placeholders and tags identical to the source?
3. Is the tone right? (EN: not harsh or too casual. JA: polite form held, no casual endings.)
4. Are buttons, tabs, and menus kept short?
5. Within the same request, is each concept expressed the same way?

## Rules of thumb

- Do not hand localization 100% to translators. The result can be correct in the language and awkward in the product.
- If you cannot tell whether a string is a `_label` or a `_message`, ask for context in the note column first.
