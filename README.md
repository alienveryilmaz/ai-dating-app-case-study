# TwoCan — AI-Powered Social Connection & Matching App

**A production React Native application built on temperament-based compatibility, with a fine-tuned LLM and RAG system underneath it.**

- 🎨 **Figma design & interactive prototype:** https://www.figma.com/proto/dotxElrUfZauIvt1yHfIsr/TwoCan
- 📱 **Google Play:** https://play.google.com/store/apps/details?id=com.twocanapp *(limited release)*

> **Source code notice**
> The source code is private due to product confidentiality and NDA constraints.
> This repository is a case study of the product, its architecture, and my role in building it.

---

## What TwoCan does

Most matching apps optimise for volume: photos, swipes, and shallow preference filters. TwoCan starts from a different assumption — that compatibility is a property of how two people think and respond, not what they list on a profile.

Every user completes a temperament and personality assessment before they can access the app. Nobody browses first and answers questions later; the assessment is the gate. What comes out of it drives everything downstream: the profile model, the compatibility scoring, and how the in-app AI talks to that person.

The product also includes a **Partner Mode** for people already in a relationship, which reframes the same temperament model around an existing pair rather than around discovery.

---

## Tech stack

| Layer | Technology |
|---|---|
| Mobile | React Native, Expo, TypeScript |
| State & data | Firestore, Firebase Realtime Database |
| Backend | Firebase Cloud Functions, Node.js |
| Auth | Firebase Authentication |
| AI | Fine-tuned LLMs, RAG, LangChain, vector retrieval |
| Release | App Store Connect, Google Play Console, TestFlight |

---

## Mobile application

The app is React Native across iOS and Android, built and shipped by one engineer. The parts that took the most work:

**Assessment onboarding.** A long, multi-step psychometric assessment sits between signup and first use — structurally the riskiest thing you can put in an onboarding funnel. It was built to be resumable across sessions, to preserve partial answers, and to keep perceived length down through pacing and progress feedback. Getting people to finish it is a product problem as much as an engineering one, and it is where most of the iteration went.

**Real-time messaging and presence.** Live chat and online/offline presence on Firebase Realtime Database, with the usual hard parts: message ordering under poor connectivity, optimistic sends that have to reconcile, and presence that degrades honestly instead of showing users as online when they are not.

**Compatibility-ranked discovery.** The discovery surface is ordered by compatibility score rather than by swipe mechanics, which meant designing an interface where the *reason* for a match is visible and legible, not hidden behind a percentage.

**AI assistant interface.** Streaming responses inside a mobile chat UI, conversation state that survives backgrounding and app restarts, and graceful handling of slow or failed generations.

**Push notifications and background behaviour.** Notification handling across both platforms, deep-linked into the right conversation or match, with the platform-specific permission and lifecycle differences that come with it.

---

## Compatibility & matching system

The matching layer is a custom algorithm built on temperament and character data rather than declared preferences.

It does not produce a binary match or a single percentage. It produces a structured compatibility profile — which dimensions align, which conflict, and what that combination tends to look like in practice. The interface then has to make that legible to someone who has never heard of temperament theory, which is arguably the harder half of the problem.

I designed and implemented both the scoring model and the data model it runs on.

---

## AI architecture

The AI layer is a system rather than a prompt.

**Dispatcher.** Before the conversational model responds, a dispatcher decides which workflow the message belongs to and which reasoning path to take. This keeps behaviour bounded and predictable instead of relying on one long system prompt to hold everything together.

**Retrieval.** RAG over a curated domain corpus, scoped to the user's temperament profile — so retrieval changes not only what information surfaces but how it is framed for that specific person.

**Fine-tuning.** Models fine-tuned for temperament-aligned relationship guidance, so persona differences are learned rather than instructed.

**Memory.** Long-term conversation memory, including retention of emotionally significant moments a user has shared, so the assistant is not starting from zero each session.

**Quality.** In a domain this personal, the interesting problem is not generation — it is what the interface does when a generation is confidently wrong and a user is sitting in front of it. Output evaluation and fallback behaviour were treated as product requirements.

### Data provenance

All AI knowledge and training data is manually prepared by a professional team and grounded in academic work.

- No scraped internet content
- No automatically generated datasets
- Fully original, domain-specific corpus

For a product operating in an emotionally sensitive domain, where the underlying material came from is a correctness requirement, not a compliance checkbox.

---

## Backend & cloud architecture

Built on Firebase, event-driven throughout.

- **Authentication** — account lifecycle and session management
- **Realtime Database** — live chat, presence, ephemeral state
- **Firestore** — structured user, assessment and match data
- **Cloud Functions** — triggered on assessment completion; runs scoring and matching workflows, plus backend automation

Serverless throughout, so the matching and scoring pipeline scales with usage rather than with provisioned capacity.

---

## Release & operations

I own the full release lifecycle on both platforms: build signing, TestFlight external beta, staged rollout, store listing and metadata, and App Review compliance.

That last part is not a footnote. Getting a consumer app in a saturated category through review is its own engineering and product problem, and it shaped real decisions about the product — including the move away from swipe mechanics toward compatibility-ranked discovery.

---

## My role

Co-founder and lead engineer. I own the product end to end:

- **Mobile** — React Native application across iOS and Android
- **Backend** — Firebase architecture, Cloud Functions, data modelling
- **Matching** — compatibility algorithm and scoring model
- **AI systems** — dispatcher logic, RAG pipeline, fine-tuning, assistant behaviour
- **Product** — concept, onboarding design, discovery mechanics, Partner Mode
- **Release** — store submissions, beta programme, review compliance on both platforms

---

## Related work

**Self Finder** — temperament-based AI guidance platform built on the same foundation, in Next.js. Live at [selffinder.ai](https://selffinder.ai)

---

**Ali Enver Yilmaz** · Mobile & AI Systems Engineer
[LinkedIn](https://www.linkedin.com/in/alienveryilmaz) · [GitHub](https://github.com/alienveryilmaz) · alienveryilmaz@gmail.com
