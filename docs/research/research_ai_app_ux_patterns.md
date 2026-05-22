---
name: ai-app-ux-patterns
description: "UI/UX patterns from consumer AI and creator apps — onboarding, prompting, output, iteration flows for AI-friendly audiences"
metadata: 
  node_type: memory
  type: reference
  originSessionId: 763e5310-03d4-4c7a-ba7a-508b5337f9b5
---

# AI App UX Patterns: Strategic Reference

Research compiled May 2026. Sources: UX Collective, Shape of AI, LogRocket, Lazarev Agency, IntuitionLabs, NxCode, Skywork, and primary product analysis.

---

## 1. Executive Summary — The 7 Patterns That Show Up Everywhere

These are the patterns that cut across every category — conversational, creative, dev tools, audio, productivity, and game AI. Build any AI product without understanding these and you will fight the user at every step.

**1. Radical simplicity at entry.** The most successful AI products ship a text box, not a control panel. ChatGPT's genius was reducing the most powerful AI ever built to a blank input and a blinking cursor. Every additional control you show before the first "wow" increases abandonment. The rule: one input, one CTA, generate.

**2. The gallery as onboarding.** Empty states kill AI products. The fix is always a gallery — curated, community, or algorithmic — that shows what is possible before the user has to imagine it themselves. Suno shows public song creations. Lovable shows fully-built projects. Midjourney's Discord feed is itself the world's largest example gallery. The gallery answers "what can this do?" without a tutorial.

**3. Progressive disclosure of power.** All the best tools layer complexity: Simple Mode first, Custom Mode behind a toggle. Two modes — one for beginners, one for power users — appears in Suno (Simple vs Custom), Pika (basic prompt vs. parameter controls), Runway (basic vs Director Mode), and v0 (description vs. component spec). Never show sliders to someone who hasn't generated their first output yet.

**4. Dual output by default.** Generating two versions of everything — Suno creates two songs per prompt, Midjourney creates a 4-image grid — gives users a comparison without asking them to iterate. It makes the first result feel abundant rather than singular and fragile. Users pick the winner, then refine from there. This is structurally different from single-output tools where the first result feels like a verdict.

**5. Graceful failure and expectation management.** The pattern is universal: ChatGPT warns to verify important information; Claude acknowledges knowledge gaps and caveats; Perplexity shows source links. Every tool that earns trust does so by proactively telling users what it cannot do. Tools that pretend to know everything lose trust the first time they are wrong — which is fast.

**6. Iteration as the core loop, not an afterthought.** The most powerful UX is the one where the user never feels stuck. This means: regenerate button, variation button, inpainting (repaint just one region), and chat-based refinement. Runway's Motion Brush lets you direct motion in specific zones. Claude's Artifacts separate generated content from chat so you can refine code without losing context. The pattern: output lives in a persistent panel; chat drives refinement.

**7. Community feed as retention engine.** Every breakout AI product builds a feed or gallery of what other users have made. Midjourney's Discord, Suno's public feed, Remix AI's discovery gallery, Lovable's remix metrics — these create social proof, demonstrate range, and give users something to aspire to. The feed also solves the "I don't know what to make" problem by surfacing examples.

---

## 2. Per-App Teardowns

### Category A: Conversational AI

#### ChatGPT (OpenAI)

**Onboarding.** Signup, then land on a clean two-column layout: conversation history in the left sidebar, empty chat in the center. Four or five suggested prompt bubbles appear below the input ("Brainstorm startup ideas," "Help me write") to dissolve blank-canvas paralysis immediately. No tutorial screens, no feature tours.

**Input UX.** Single large textarea, send button, file upload icon, voice input toggle. "Custom instructions" persist across conversations (accessed via settings). The interface famously does not expose model parameters to mainstream users — GPT-4o is simply "the one you use."

**Output presentation.** Text streams in real time. Code blocks get monospaced fonts with a copy button. Long outputs use markdown formatting with headers, bullet lists, numbered lists. No separate output panel — everything scrolls in the same thread.

**Progressive disclosure.** The advanced model (o3, reasoning mode) is behind a model picker dropdown — hidden from the default experience. File upload, image generation, canvas mode, and web search are toolbar icons that appear only when relevant.

**Feedback loops.** Thumbs up/down per response. Regenerate button. "Edit message" allows modifying a previous prompt and forking the conversation. ChatGPT's memory settings give users control over what the AI retains.

**Trust signals.** "ChatGPT can make mistakes. Consider checking important information." Footer disclaimer on every session. Enterprise tier explicitly states data won't train models.

**Magic moment timing.** First useful response in under 10 seconds of first prompt. The streamed response itself — watching tokens appear — is the magic moment for many first-time users.

---

#### Claude (Anthropic)

**Onboarding.** Similar to ChatGPT but with a stronger emphasis on "Projects" as an organizing structure. Projects bundle multiple conversations with uploaded documents into shared knowledge bases — a differentiator aimed at professional users and teams.

**Input UX.** Large textarea with drag-and-drop file support. Supports uploading multiple files simultaneously. Extended thinking mode (for complex reasoning) accessible as a toggle — a progressive disclosure of reasoning power.

**Output presentation.** The "Artifacts" panel is Claude's defining UX innovation: generated code, HTML, and long-form documents appear in a dedicated right-hand panel, separate from the chat thread. This means you can iterate the artifact via chat without the output scrolling away. This pattern (split: conversation left, output right) is now being copied across the category.

**Progressive disclosure.** Two-pane layout only appears when an artifact is generated. Simple prose responses stay in single-column. Users discover the artifact panel naturally.

**Trust signals.** Tone is explicitly calibrated to acknowledge uncertainty: "I'm not certain about this, but..." is a Claude signature. The confidence disclaimer is woven into the voice, not just a footer.

**Distinctive.** Claude's personality and tone are themselves a UX signal — the warmth and self-awareness create a different relationship than ChatGPT's more neutral voice.

---

#### Perplexity AI

**Onboarding.** Source-first design from the first interaction. Unlike ChatGPT or Claude, Perplexity frames itself as a search replacement, not a chat assistant. The landing prompt ("Ask anything") is familiar but the output is radically different.

**Output presentation.** Every answer shows numbered source citations inline and a "Sources" panel with article cards. This is the defining pattern: AI answer + evidence trail. Users see source cards immediately — they can click through and verify. No other major conversational AI does this as its primary mode.

**Progressive disclosure.** "Focus" modes let users constrain to specific sources (Academic, YouTube, Reddit, News). These are available from the input — a one-step scoping mechanism.

**Trust signals.** The source display is itself the trust mechanism. Showing "this claim came from this URL" transfers verification responsibility to the user in a way that builds confidence. Perplexity's unique contribution to the pattern library.

**Magic moment.** Searching for something time-sensitive (stock price, recent news) and getting a sourced, structured answer faster than Google. Perplexity owns the "I need current information" use case.

---

#### Character.ai

**Onboarding.** Character selection is the first step — you choose which AI persona to talk to before you type anything. The character feed (browsable list of AI personalities with names, descriptions, and creator attribution) is the onboarding AND the empty state. Users start by choosing a character, not by typing a prompt.

**Input UX.** Standard chat textarea. The novel element: character persistence. Your character remembers previous conversations. The sensation of continuity is manufactured but effective.

**Emotional UX.** Character.ai's distinctive pattern: AI with persona. The character has a name, an avatar, a personality description, and a writing style. The interface surfaces character identity prominently (avatar, name, stat counts) above every chat. This is the template for any AI where the character IS the product.

**Trust signals.** Characters are user-created and community-rated. Star ratings and conversation counts signal popularity. The community trust model replaces platform trust.

---

#### Pi (Inflection AI)

**Onboarding.** Pi asks for your name immediately. Then offers voice selection — 8 distinct voices labeled "Pi 1" through "Pi 8." This personalization-before-first-message is distinctive: it makes the AI feel like a choice, not a product you're handed.

**Emotional design.** The interface uses warm orange gradients and short, restrained sentences. The design philosophy is "empathy through rhythm, tone, and restraint." Pi communicates warmth through visual and typographic choices, not just words.

**Input UX.** Conversation-only. No file upload, no code interpreter, no image generation. The constraints are the design — this is a relationship chatbot, not a tool.

**Magic moment.** The first time Pi remembers something from a previous session and references it. The "you mentioned" pattern is Pi's hook.

---

### Category B: AI Image / Video Creation

#### Midjourney

**Onboarding (Discord era).** No traditional onboarding — users joined a Discord server and immediately saw a waterfall of other people's generations. The community feed WAS the onboarding. New users learned by watching, reading prompts, and typing `/imagine` in a shared channel. Community osmosis replaced tutorial design.

**Onboarding (Web UI, 2024+).** The web interface launched a proper gallery experience. Users see an "Explore" feed of community generations on first visit. The gallery is browsable, searchable, and remixable. Each image shows the prompt used — prompt transparency is the primary learning mechanism.

**Input UX.** Text prompt + optional reference image. "Style reference" and "character reference" parameters. Aspect ratio selector. Style strength slider (0-100). Parameters are text-based (`--ar 16:9 --stylize 100`) — power-user ergonomics baked into the prompt.

**Output presentation.** 2x2 image grid — four variations by default. Buttons beneath each grid: U1-U4 (upscale individual image), V1-V4 (create variations from that image). This is the canonical AI image grid pattern.

**Iteration loop.** The U/V button system is the defining iteration UX: upscale the winner, or generate 4 more variations from a specific image. Vary (Region) adds inpainting — select a specific region to regenerate while preserving the rest.

**Community.** The Explore feed shows trending generations with prompts attached. This is where Midjourney's flywheel lives — every public generation is marketing for the platform.

---

#### Runway

**Onboarding.** Multi-tool creative suite — the landing page leads with "Gen-4" as the flagship product. Onboarding uses G2-confirmed "logically placed, clearly labeled" feature organization. Most users report being able to generate their first video within minutes.

**Input UX.** Text prompt + optional image/video input. Mode selector: Text-to-Video, Image-to-Video, Video-to-Video. Advanced controls appear contextually.

**Director Mode (progressive disclosure at its best).** Camera movement controls (pan, tilt, zoom, dolly, roll) appear as a separate mode — accessible but not the default. Users learn basic generation first, then discover cinematography controls.

**Motion Brush.** The signature differentiator: paint motion directions directly onto specific regions of an image with up to 5 independent motion zones. This is inpainting applied to motion — a pattern no competitor had matched at launch.

**Node-based Workflows (2025+).** Chain multiple models into automated pipelines. This is the power-user tier — discovered only after mastering basic generation. Three layers of disclosure: basic prompt → Director Mode → Workflows.

**Output presentation.** Video player with loop, download, share. Side-by-side comparison not standard but available.

---

#### Pika

**Input UX.** Prompt + aspect ratio selector + motion intensity slider (0-5). The motion intensity slider is the key progressive disclosure — beginners leave it at default, power users dial it down for subtlety.

**Onboarding.** Self-described as "prosumer" — results without needing cinematography knowledge. Default settings are calibrated for good-enough outputs without configuration.

**Pikaframes (keyframe system).** Upload a start frame and end frame, Pika animates between them. This is a different creative model — users control the arc of the animation, not the style.

**Mobile app (late 2025).** Direct publish to TikTok and Instagram from the generation screen — the share pattern embedded into the output.

**Community.** Prompt libraries and collaborative remix threads. The social layer reinforces Pika's prosumer positioning.

---

#### Adobe Firefly

**Onboarding.** Firefly is embedded into Creative Cloud apps (Photoshop, Illustrator) as contextual panels, toolbars, and menus — not a standalone destination. This is the "invisible AI" pattern: the AI appears where users already work, within existing workflows.

**Input UX.** Text prompt + Generative Fill (selection-based inpainting). Style reference via "Style Match." Reference image input.

**Trust signals (brand-specific).** "Style Kits" let teams upload 20 reference images to fine-tune the model on their brand aesthetic — every output stays in-brand. This is the enterprise trust pattern: not "trust the AI" but "control the AI."

**Commercial safety.** Firefly is trained on licensed content — this is prominently communicated as a trust signal for professional/enterprise users who need commercially safe outputs.

---

#### Canva AI (Magic Studio)

**Onboarding.** Canva's AI is embedded in the existing Canva editor as "Magic Media" and related tools. The entry point is within a design workflow — users who already know Canva discover AI as an upgrade, not a new product.

**Input UX.** Single prompt field within the design editor. Style selection from a visual grid of style options. Aspect ratio auto-inherits from the canvas.

**Visual Suite 2.0 (2025).** Unified interface bringing all AI tools under one "Magic" brand within the editor. AI doesn't displace the canvas — it serves it.

**Progressive disclosure.** Basic users use Magic Media as a "fill this with an image" button. Advanced users discover reference image controls, style weights, and negative prompts.

---

#### Leonardo.ai

**Onboarding.** Web-first, standalone. Free tier with credits. Community gallery is the default landing experience — users see what others have generated before creating their own.

**Distinctive features.** Realtime Canvas (generate as you paint — outputs appear live as you draw). Custom model training (fine-tune a model on your art style, then generate from it). These are advanced features surfaced via tabs, not the default entry.

**Input UX.** Prompt + negative prompt (what NOT to include) + model selector + style preset grid. The negative prompt field is standard in image generation tools — a pattern absent from conversational AI but ubiquitous in image tools.

**Output presentation.** Image grid (typically 4). Each image has: like, download, share, "use as reference" buttons. The "use as reference" button feeds the output directly back into the next generation — closing the iteration loop.

---

### Category C: AI Dev / Build Tools

#### v0 (Vercel)

**Onboarding.** Land on a prompt input with example prompts below it. The examples are component-specific ("Build a dashboard with a sidebar and data table") — concrete enough to demonstrate capability without being overwhelming.

**Input UX.** Text prompt describing UI. The ideal prompt includes: specific component list, user context (who uses this, how), and desired stack. v0's "How to prompt v0" guide is itself a form of prompt scaffolding — the documentation teaches the input pattern.

**Output presentation.** Generated code appears in a right-hand panel with a live preview. Left: chat thread. Right: code + preview. This is the Artifacts pattern (pioneered by Claude) applied to dev tools.

**Iteration loop.** Chat-based refinement: "Make the sidebar collapsible," "Change the primary color to blue." Each iteration updates the right-hand panel while preserving the conversation thread.

**Progressive disclosure.** No explicit modes. Power comes through prompt specificity — the more precise the input, the more controlled the output.

**Distinctive.** v0 is optimized for the React + Tailwind + shadcn/ui stack. This opinionated default stack means outputs look coherent — a design decision that trades flexibility for consistency.

---

#### Lovable

**Onboarding.** "Describe your app" in a large textarea. The landing page shows example apps built with Lovable as social proof. First generation produces a functional full-stack app — backend, auth, database, UI — not just a UI mockup.

**Input UX.** Natural language description. No settings before first generation. Lovable infers stack, design system, and architecture from the description.

**Output presentation.** Live preview of the running app in a browser window, alongside the conversation panel. Users see a working app, not code.

**Visual Editor (2025).** Click on any element in the preview to enter a Figma-like editing mode — change colors, text, layout without typing. This is progressive disclosure of design control: start with words, graduate to visual editing.

**Iteration loop.** Chat for functional changes, Visual Editor for design tweaks. Two iteration modes for two types of users (developers vs. designers).

---

#### bolt.new

**Onboarding.** Prompt input with example apps as starting points. Interface "feels like ChatGPT" — familiar chat paradigm applied to app building. This deliberate familiarity lowers the cognitive cost of switching.

**Input UX.** Chat-first. Bolt supports Supabase integration and full-stack generation from the same chat interface as Lovable.

**Distinctive.** Bolt imports GitHub repos — users can paste a GitHub URL and Bolt loads the codebase for editing. This bridges the gap between AI prototyping and existing codebases.

---

#### Cursor

**No natural language onboarding.** Cursor's onboarding is aimed at existing developers — it's a VS Code fork. The setup assumes familiarity with code editors. AI features are discovered through keyboard shortcuts (Cmd+K for inline edits, Cmd+L for chat) — power-user ergonomics from day one.

**Distinctive.** Cursor is the "I bring my own stack" tool. Unlike Lovable or v0, Cursor does not make decisions for you — it assists decisions you are already making. The UX is IDE-first, AI-second.

**Codebase awareness.** Cursor indexes the entire codebase and uses it as context. "@codebase" in a prompt means "search the whole repo." This is the Context Retention pattern applied to software.

---

### Category D: AI Audio / Music

#### ElevenLabs

**Onboarding.** Voice synthesis tool with professional-grade controls. Entry: choose a voice from the library (hundreds of voices with audio previews), type text, generate. The voice library IS the onboarding — browsing voices is inherently exploratory.

**Input UX.** Text input + voice selector + style controls (stability, clarity, exaggeration sliders). The slider UX is standard for audio — users understand "more/less" for audio controls in ways they don't for image generation.

**Output presentation.** Audio player with waveform visualization. Download, share, add to project. Projects bundle voice + audio into a persistent workspace.

**ElevenMusic (2026).** Standalone music generation iOS app. "Think Spotify meets Suno" — generation + discovery in one interface.

**Trust signals.** Prominent voice cloning consent flows. ElevenLabs has invested more than competitors in consent and attribution UI — the legal and ethical context of voice generation makes trust signals load-bearing.

---

#### Suno

**Onboarding.** Login → land on a public feed of AI-generated songs. You see what others have made before making your own. Feed → Create is the onboarding flow.

**Input UX.** Two modes: Simple Mode (one description field) and Custom Mode (separate Style + Lyrics fields). Simple Mode generates two songs simultaneously — dual output by default. Custom Mode separates genre/mood/instrumentation from lyrical content.

**Progressive disclosure.** Simple → Custom is the explicit disclosure path. The toggle is prominent and labeled. Custom Mode reveals: separate Lyrics field with structural tags ([Verse], [Chorus]), Style field for genre descriptors, and generation sliders.

**Output.** Two songs per generation. Audio player with waveform. Like, share, remix, download buttons. The public feed means every song is a potential community artifact.

**Suno Studio (V5, 2025+).** DAW-like editor with 12 generative stems (isolated tracks), timeline editor, vocal personas. This is the third tier of disclosure — Studio mode, revealed after users are comfortable with Custom Mode.

---

#### Udio

**Positioning.** "For people who know what they want." Udio's interface prioritizes granular control over immediacy. Where Suno prioritizes the accessible path, Udio targets musicians and producers who want specific results.

**Input UX.** Detailed prompt field + inpainting (regenerate specific sections of a song). The inpainting concept from image tools applied to music — you can regenerate the chorus while preserving the verse.

**Distinctive.** The inpainting pattern for music is Udio's differentiator — section-level regeneration, not whole-song regeneration.

---

### Category E: AI Productivity

#### Notion AI

**Onboarding.** Notion's onboarding asks function (Marketing, Engineering, Personal) during signup, then serves a personalized selection of 5 templates. The personalization reduces choice paralysis and demonstrates value immediately via templates with demo data.

**Input UX.** AI is embedded as a "/" command within any page — a contextual input that appears where the user is working, not in a separate panel. Type "/" and the AI command menu appears inline.

**Context-awareness.** "Summarize this page," "Brainstorm ideas about this document" — Notion AI's inputs are always relative to the current document context. The AI knows what you are looking at.

**Progressive disclosure.** AI features are discoverable through the "/" command menu — users encounter them as they type, not through a tutorial. The feature reveals itself at the moment of need.

**Notion 3.0 / AI Agents.** Autonomous agents that execute multi-step tasks across the workspace. This is the top tier of disclosure — visible only after users are deeply embedded in the product.

---

#### Gamma

**Onboarding.** Asks: "What's your main reason for using Gamma?" "Which industry?" "What will you use it for?" — intent routing that personalizes the template and theme recommendations before any generation.

**Input UX.** Three generation paths: Paste in text/URL → Gamma structures it. Write a prompt → Gamma creates an outline. Start from a template. The outline step is explicit — users review and edit an AI-generated outline before slide generation. This "review before generate" pattern is distinctive and reduces wasted generation.

**Progressive disclosure.** Generate → see slides → edit in the Gamma editor. The Gamma Agent (2025) adds a chat panel alongside the editor for mid-edit AI assistance.

**Output presentation.** Slides rendered as web-native cards — not PowerPoint files. Embeddable, shareable via URL. The output format itself is the UX innovation.

---

#### Tome

**Positioning.** AI presentation tool oriented toward storytelling and narrative structure. Tome generates from a prompt with a focus on visual hierarchy and narrative flow, not just bullet points.

**Input UX.** One prompt → Tome generates an entire narrative arc with suggested visuals. Less structured than Gamma's outline review step — more "generate and see."

**Output presentation.** Web-native slides with integrated AI image generation per slide. Users iterate on visuals and text within the slide editor.

---

### Category F: AI Game / Avatar

#### Ready Player Me

**Onboarding.** Avatar creation wizard: choose body type → customize face, hair, clothing → generate. The "selfie to avatar" path uses a phone photo to seed the avatar creation — the magic moment is seeing your face on a 3D character.

**Progressive disclosure.** Basic customization is visual (click to change). Advanced customization (body proportions, accessories) appears as you go deeper. Generative AI (2025+) allows text-to-avatar: "A cyberpunk warrior with red hair" → avatar generation.

**Cross-platform.** The avatar is the same across any game that integrates Ready Player Me. The value proposition is portability — make it once, use it everywhere. The UX reinforces this by showing which games your avatar can be used in.

---

#### Inworld AI

**For developers, not end users.** Inworld's primary UX is a developer console for configuring AI NPC characters — defining personality, memory, goals, and speech patterns. The "onboarding" is API documentation and quickstart guides.

**Character creation UX.** Define: name, personality traits, core memory (backstory), goals (what the character wants), speech patterns (how they talk). This is the template for character-as-product design — structured fields that define an AI persona.

**Integration with game engines.** SDKs for Unity and Unreal. The designed output is an NPC that responds to player inputs in character, consistently, forever. The UX challenge is the configuration experience, not the conversation experience.

---

#### Scenario.gg

**Onboarding.** Upload training images (your existing art style) → train a custom model (a few clicks, no technical skill required) → generate assets in that style. Three-step: train → generate → export.

**Distinctive.** Scenario solves the style consistency problem — every asset looks like it came from the same artist. The custom model IS the product, not just the interface.

**Input UX.** Prompt + composition control + pixel-perfect inpainting. The advanced controls (composition, inpainting) are discoverable but not the default entry point.

**API-first.** Scenario's architecture enables integration into existing pipelines (Unity, Unreal, design tools). The UX is dual-mode: web interface for human users, API for automated workflows.

---

## 3. Pattern Library

Named, reusable UX patterns with descriptions and which apps use them.

### Input Patterns

**P1: The Blank Slate Prompt**
A large, centered text input as the primary (sometimes only) interactive element on the page. Signals: "just start typing." No onboarding required.
Used by: ChatGPT, Claude, Perplexity, v0, Lovable, Gamma, Suno

**P2: Suggestion Bubbles**
3-6 pre-written example prompts displayed as clickable chips or cards below the input. Dissolves blank-canvas paralysis. Disappear once the user types.
Used by: ChatGPT, Notion AI, Meta AI, Gamma

**P3: Simple Mode / Custom Mode Toggle**
Two labeled modes: a beginner path (minimal options) and a power-user path (full controls). The toggle is explicit and always accessible.
Used by: Suno, Pika, Runway (Director Mode), v0 (implicit via prompt specificity)

**P4: Negative Prompt Field**
A secondary input for specifying what the AI should NOT generate. Standard in image generation; rare in conversational AI.
Used by: Midjourney, Leonardo.ai, Stable Diffusion-based tools

**P5: Style Picker Grid**
A visual grid of style thumbnails the user can click to set an aesthetic direction before generating. Replaces the need to describe style in text.
Used by: Canva AI, Adobe Firefly, Gamma (themes), ElevenLabs (voice library)

**P6: Intent Routing Questions**
Onboarding questions that route users to personalized template sets or feature subsets based on their stated role or goal.
Used by: Notion (function selector), Gamma (industry + use case), Pi (name + voice selection), Lovable (implicit)

**P7: Prompt Enhancer**
AI that automatically improves a low-quality user prompt before generating. Often presented as "Enhance prompt" toggle.
Used by: Midjourney (stylize parameter), various image tools

### Output Patterns

**P8: Dual Output by Default**
Generate two versions simultaneously without requiring the user to ask for a comparison. Users pick the winner.
Used by: Suno (2 songs per prompt), Midjourney (4-image grid), many image tools

**P9: Split Panel (Conversation Left, Output Right)**
Chat/conversation thread on the left; generated artifact (code, doc, image) in a persistent right panel that doesn't scroll away.
Used by: Claude (Artifacts), v0, Lovable (live preview), Cursor

**P10: Result Grid**
Multiple generations displayed in a uniform grid. Each item has secondary actions (like, download, use as reference, vary).
Used by: Midjourney, Leonardo.ai, Midjourney Web UI, Sora

**P11: Streaming Output**
Generated content appears token-by-token (text) or frame-by-frame in real time. Creates engagement and implies responsiveness.
Used by: ChatGPT, Claude, Perplexity, most LLM chat interfaces

**P12: Waveform Audio Player**
Generated audio with a visible waveform, playback controls, and download. Makes audio output tangible and scrub-able.
Used by: ElevenLabs, Suno, Udio

### Iteration Patterns

**P13: Inpainting**
Select a specific region of an image (or section of audio) and regenerate only that part while preserving the rest.
Used by: Midjourney (Vary Region), Runway, Adobe Firefly (Generative Fill), Udio (section regeneration)

**P14: Upscale + Vary Buttons**
After a result grid, each item has buttons to either upscale (higher resolution, this exact image) or vary (4 new variations derived from this image).
Used by: Midjourney (U1-U4, V1-V4 buttons)

**P15: Chat-Based Refinement**
Users type refinement instructions in a chat thread, and the output panel updates. Iteration is conversational, not form-based.
Used by: Claude, v0, Lovable, Gamma (Gamma Agent), Cursor

**P16: Branch / Version History**
Previous generations are preserved and accessible. Users can return to any prior state.
Used by: Claude (conversation branches), Midjourney (all generations saved), Lovable (version history)

### Community Patterns

**P17: Public Feed as Default Landing**
First-time and returning users land on a feed of what others have generated. The community's output is the homepage.
Used by: Suno, Midjourney (Explore), Remix AI, Leonardo.ai

**P18: Prompt Transparency**
Generated content in the gallery shows the prompt that created it. Teaches new users how to prompt; rewards good prompt writers.
Used by: Midjourney, Scenario.gg community gallery, Shape of AI examples

**P19: Remix with One Click**
"Start from this" button on any community-generated item. Copies the prompt and settings into the user's input, ready to modify.
Used by: Lovable (project remix), Remix Camera, Pika community threads

**P20: Creator Attribution**
Community gallery items show creator name and avatar. Incentivizes sharing; creates proto-social network effects.
Used by: Midjourney, Leonardo.ai, Character.ai (character creators), Suno

### Trust Patterns

**P21: Inline Uncertainty**
AI expresses uncertainty within its response ("I'm not certain about this," "you may want to verify"). Not a footer — embedded in the answer.
Used by: Claude (signature pattern), ChatGPT (contextually)

**P22: Source Cards**
AI response includes clickable cards showing the sources used. Each card shows headline, domain, date.
Used by: Perplexity (primary pattern), Bing/Copilot, you.com

**P23: Action Preview**
Before executing a multi-step or destructive action, the AI shows what it will do and asks for confirmation.
Used by: Lovable (showing planned code changes), Cursor (showing diff before applying)

**P24: Commercial Safety Signal**
Explicit notice that content is safe for commercial use (trained on licensed content). For professional/enterprise trust.
Used by: Adobe Firefly (primary differentiator), Getty AI tools

---

## 4. Anti-Patterns

Things that look reasonable but hurt AI product UX.

**A1: Controls before the first magic moment**
Showing sliders, parameter panels, or mode selectors before the user has generated anything. Users don't know what the controls do until they've seen a generation. The fix: generate first with defaults, then reveal controls in context.

**A2: Single output with no variation path**
Generating one result with no regenerate, vary, or alternative button. The first output is almost never the final output. Single-output UX treats the first generation as a verdict — users feel pressure to accept or abandon. Always show at least two outputs or provide a visible "generate again" path.

**A3: Linear wizard workflows for creative tools**
Forcing users through a fixed sequence of steps (Step 1 → Step 2 → Step 3) when creativity is non-linear. Creative users need to jump back, skip ahead, and re-enter at any stage. The fix: continuous loop workflows with multiple entry points.

**A4: Empty containers with prominent real estate**
Large visual panels or dashboard sections that are empty by default, shown to new users. Empty space signals "broken" or "nothing here yet" — it does not inspire action. The fix: pre-populate with demo data, templates, or examples. Never show empty.

**A5: Vague error and limitation messaging**
"Something went wrong" or "Unable to generate" without explaining why or what to do next. AI failures are frequent enough that the error state is part of the core experience, not an edge case. The fix: explain the constraint ("This content may violate our guidelines — try adjusting your prompt to...") and offer a path forward.

**A6: Hiding the prompt used**
Generated content in a gallery or history with no way to see what prompt created it. This prevents learning, prevents remixing, and makes the output feel magical in a bad way (you can't reproduce it). The fix: always show the prompt. Transparency is a feature.

**A7: Static forms as the AI input**
Fixed input fields that force the user to fill pre-defined slots. AI's strength is handling free-form language — forcing users into structured forms defeats the purpose and increases friction. The fix: free-form prompt input with optional structured fields as progressive disclosure.

**A8: Overconfident outputs without caveats**
AI responses that assert facts without uncertainty markers. Users eventually discover the AI was wrong — and the lack of hedging makes the failure feel like deception. The fix: calibrate confidence language ("likely," "based on available data," "I'd recommend verifying this") at the model/prompt level.

**A9: Memory without user control**
AI that remembers everything about a user but provides no way to view, edit, or delete what it knows. Creates discomfort ("what does it know about me?"). The fix: a visible memory panel with edit and delete per item.

**A10: Conversation-as-navigation**
Products where the only way to find previous work is to scroll through chat history. History should have its own organized surface (sidebar with named sessions, project folders, searchable gallery) separate from the active conversation.

---

## 5. Screenshot / Visual Reference Index

These URLs contain good screenshots, demos, or visual documentation of the patterns described above. (Images cannot be embedded in markdown but the URLs are live sources.)

### UX Pattern Libraries

- **Shape of AI — Pattern Gallery**: https://www.shapeof.ai/patterns/gallery
  The canonical pattern library for AI UX. Browse named patterns with screenshots from real products.

- **AI UX Design Guide — Progressive Disclosure**: https://www.aiuxdesign.guide/patterns/progressive-disclosure
  Detailed progressive disclosure pattern with before/after screenshots.

- **AI UX Design Guide — Conversational UI**: https://www.aiuxdesign.guide/patterns/conversational-ui
  Conversational interface patterns with implementation examples.

### Product-Specific

- **UX Collective — Time to Magic Moment (ChatGPT, Claude, Perplexity)**: https://uxdesign.cc/time-to-magic-moment-claude-chatgpt-perplexity-7df7ec3a4fe6
  Side-by-side onboarding flow comparison with screenshots.

- **Lazarev Agency — 33 Chatbot UI Examples**: https://www.lazarev.agency/articles/chatbot-ui-examples
  33 real chatbot/AI interfaces with annotated screenshots.

- **IntuitionLabs — Conversational AI UI Comparison 2025**: https://intuitionlabs.ai/articles/conversational-ai-ui-comparison-2025
  Side-by-side UI comparison table for ChatGPT, Gemini, Claude, Poe.

- **LogRocket — AI-Driven UX Design Patterns**: https://blog.logrocket.com/ux-design/ai-driven-ux-design-patterns/
  Pattern descriptions with implementation examples and screenshots.

### Design Analysis

- **Pencil & Paper — Generative AI UX Examples 2024**: https://www.pencilandpaper.io/articles/generative-ai-examples
  Case studies of AI UX with screenshots from production products.

- **Koru UX — 14 AI Patterns for Designers**: https://www.koruux.com/ai-patterns-for-ui-design/
  14 named patterns with visual examples from real AI products.

- **AlterSquare — UI Patterns That Don't Work for AI**: https://altersquare.io/ui-patterns-dont-work-ai-powered-interfaces/
  Anti-pattern analysis with design alternatives.

- **Goodux/Appcues — Notion's Lightweight Onboarding**: https://goodux.appcues.com/blog/notions-lightweight-onboarding
  Annotated Notion onboarding flow with screenshots.

### Video / Demo References

- **Midjourney Web UI walkthrough** (Midjourney's official Discord remains the best live demo of the community feed pattern)
- **Runway Director Mode demo**: https://runwayml.com/ (feature tour videos on the homepage)
- **Suno — Custom Mode guide**: https://undetectr.com/blog/suno-ai-custom-mode-guide
- **Lovable — live preview UX**: https://lovable.dev/ (homepage shows split panel with live app preview)
- **v0 — prompt guide**: https://vercel.com/blog/how-to-prompt-v0

---

## 6. Recommendations for an AI Game Design Wizard

The `/game-design` skill is an AI wizard that helps game designers build games through Justin Gary's 6-stage design process. Here is what to steal from the above research for that specific context.

### What the game design wizard context shares with these apps

- Creative output (like image/music generators) — the user is making something, not just consuming information
- Multi-stage workflow (like Gamma's outline → slides flow) — the process has structure
- Expert audience with domain knowledge (like Cursor's IDE-first UX) — game designers know their vocabulary
- Iterative refinement (like Claude's Artifacts) — the design evolves across sessions
- High stakes output (like enterprise trust patterns) — the user will stake real work on the output

### Steal these patterns

**From Gamma:** The "review the outline before generating" pattern. For game design, this maps to: show the user the structure (stage, questions, decisions) before deep-diving into any stage. Let them edit the outline — which mechanics to focus on, which stages to skip — before committing to a generation path. Reduces wasted effort.

**From Claude's Artifacts:** The split panel. Game design output (a GDD section, a mechanic description, a playtest protocol) should live in a persistent artifact panel, not buried in the chat thread. The chat drives refinement; the artifact is the deliverable.

**From Suno's Simple/Custom toggle:** Two modes for game design sessions. Quick mode: wizard asks the minimum questions needed to generate a first draft of the stage deliverable. Expert mode: all questions, full depth. The toggle should be explicit and available at any point.

**From Character.ai:** Character as the product. The wizard has a voice and a methodology (Justin Gary's framework). That personality — structured, experienced, builder-focused — should be consistent and recognizable. The wizard is not a generic chat interface; it is a specific expert with a name and a method.

**From Midjourney's V/U buttons:** Explicit variation vs. refinement paths after any output. "Generate 3 variations of this mechanic" vs. "Deepen this specific mechanic" are distinct actions that should be distinct buttons or commands, not inferred from chat.

**From Notion AI's "/" command:** Stage transitions should feel like navigating with intent, not scrolling through a linear flow. "Move to Stage 3" or "/playtest" as an explicit navigation command gives the user a sense of agency over the process.

**From Perplexity's source cards:** After any recommendation or example in the wizard output, show "games that use this pattern" as clickable, scannable cards. The game designer equivalent of source attribution is example games — showing the pattern in the wild.

**From the empty state research:** Never show the wizard asking "What would you like to do?" Start with something: a short example of a Stage 1 output from a real or fictional game, a prompt like "Tell me about the game you're designing — or pick an example to explore," or a gallery of 3-4 game concepts generated from previous sessions.

**From the trust pattern research:** The wizard should be explicit about what the framework CAN and CANNOT do. "This process helps you make design decisions — it doesn't playtest for you, and it doesn't guarantee the game is fun. What it does is make your design choices visible and deliberate." Stage setting is trust-building.

**From Lovable's Visual Editor:** After generating a GDD section or stage deliverable, offer a structured editing mode where the user can click specific fields (core loop, win condition, feedback system) and edit them in place, rather than going back through chat to change one element.

### The one pattern the game design wizard doesn't have yet but should

**The "show me a game that did this" pattern.** When the wizard recommends a design decision ("Consider using a resource conversion loop"), it should immediately follow with 2-3 real examples (Dominion's card economy, Wingspan's engine builder). This is the Perplexity source card pattern applied to game design knowledge. It validates the recommendation, teaches by example, and grounds abstract advice in concrete, recognizable games. This requires a curated "game mechanics + examples" knowledge base baked into the system prompt — not a web search, but a reliable, fast in-context library of "X mechanic → games that use it" mappings.
