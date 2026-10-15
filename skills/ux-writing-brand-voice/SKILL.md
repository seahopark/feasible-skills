---
name: ux-writing-brand-voice
description: |
  Use when writing or reviewing product UI copy (buttons, error messages, notifications, empty
  states), or when a product has no brand voice yet and you need to define one. Gives a review
  procedure with 7 baseline guidelines for Korean UI copy, and a workshop procedure for defining
  voice and tone with the team.

  Use this when:
  - Writing or editing UI text and you want it consistent, plain, and warm
  - Reviewing existing copy against written guidelines and returning before/after fixes
  - A product has no voice document and you need to build one with the team
  - Different channels (exchange referral, search, in-app) need different messages for the same product
---

# UX Writing and Brand Voice

Every piece of text in a product is user experience, and together they make the brand voice. A PM who hands all copy to design or marketing ends up with a product whose voice is split. Use this skill for two jobs: (A) writing or reviewing copy when a voice exists, (B) defining the voice when it does not.

For customer-facing documents (FAQ, help center, release notes) built from internal sources, use a docs skill instead. For multi-language keys, glossaries, and translation rules, use `localization-system`.

## A. Write or review copy (a voice exists)

1. Confirm whether the request is writing or review. Ask which screen and element it is (button, instruction, error, empty state). Get the service's voice document first if there is one. If there is none, go to B, or use the baseline guidelines below and say the result is a draft.
2. Write or check the copy against the guidelines.
3. Return review results as a table: current text / suggested text / reason. Number the guideline each issue violates.
4. If one concept appears under several labels (for example "delete" and "remove"), propose fixing on one. Once a wording is chosen, do not change it without a strong reason.

### Seven baseline guidelines (Korean UI copy)

Examples are Korean UI strings with English glosses.

1. **No hard terms.** Replace technical jargon with words anyone knows. "디폴트 설정값" (default setting value) becomes "처음 정해진 설정" (the setting it starts with).
2. **Explain unavoidable jargon in everyday speech.** "백엔드에서 처리돼요" (handled on the backend) becomes "보이지 않는 곳에서 자동으로 작동해요" (it works automatically behind the scenes).
3. **Explain complex ideas with a concrete scene.** "서비스 장애 시 대처 방안" (response plan for outages) becomes "앱이 갑자기 꺼지거나 화면이 안 뜰 때는 이렇게 해보세요" (if the app suddenly closes or the screen won't load, try this).
4. **Turn problems toward a solution, warmly.** "접속 불가" (cannot connect) becomes "지금은 잠시 접속이 어려워요. 조금 뒤에 다시 시도해 주세요" (connecting is hard right now; please try again shortly).
5. **Avoid formal, cold register.** "문의해 주시기 바랍니다" (please make an inquiry) becomes "궁금한 점이 있다면 언제든지 물어보세요" (ask anytime if you're curious).
6. **Prefer native words to loanwords.** "리셋" (reset) becomes "처음으로 되돌리기" (return to the start); "에러" (error) becomes "문제가 생겼어요" (a problem came up). A service glossary, or a loanword that has become standard, overrides this.
7. **Use the active voice.** "설정이 변경되었습니다" (the setting was changed) becomes "설정을 바꿨어요" (we changed the setting).

Also: start action prompts with a verb, and prefer positive phrasing over negative.

These are defaults for a friendly, plain-spoken product. For a product where trust and precision come first (finance, legal, security), adjust the strength of guidelines 5 and 7 to the voice document and say you did.

## B. Define the voice (no voice document yet)

A brand voice is not made by the design team alone. It forms when the whole team agrees on who the company is.

1. **Pick a benchmark.** Choose a service in the same register and see what core message builds its trust. Borrow the structure and restate your own core value in one sentence. Original case: a plain news app was redefined as an investment-intelligence product that explains *why* prices move, with the core value "volatility needs a standard".
2. **Interview every key person on the team.** Use the 28 questions from the book *Microcopy* as the backbone. Use interviews, not surveys: the point is to draw out how each person sees the company, not to have them pick an answer.
3. **Derive 4 to 5 core values from the interviews.** Original case: reliability, customer focus, gentleness, expertise, personalization.
4. **Write the persona in a line.** Age range, character, relationship with the user (for example administrator and personal assistant), and attitude under pressure (for example staying calm in a falling market).
5. **Differentiate messages by entry channel.** The same product says different things to different people: for exchange referrals, "reliable information comes to you first"; for search visitors, "stop hopping between sites, find it in one place"; for a ticker-based entry, "looking for more on {ticker}?".
6. **Collect everything in one voice document:** core values, persona, speech principles, banned expressions, channel messages, approved example copy. From then on, A defers to this document.

## Rules of thumb

- Guidelines cover user-visible text only. Leave placeholders and variables untouched when editing copy.
- If the user wants a tone that differs from the guidelines, respect it as the service's choice, but flag once if it would split the tone within one product.
