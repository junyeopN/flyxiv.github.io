# CLAUDE.md

## Work posts on this blog are public: keep company-specific detail out

Posts about work (`category: General Dev` + `Work History`, ex) SPORTER, BestCrop, image service posts) must stay
high level. Write what the problem was, what kind of approach we took, what was learned, and the outcome. Do not write:

- **Internal names**: service, engine, repo, topic or pod names (ex) `image-index`, `image-bestcrop`, `esports-chat`).
  Use generic names: "our image analysis service", "a separate crop service", "the reaction engines".
- **Model names and sizes**: no model family/version (Qwen, Gemini, SigLIP, YOLO, ...) or parameter counts (9B, 27B).
  Say "a fine-tuned VLM", "an open-weight LLM with LoRA", "a commercial VLM", "a small vision backbone".
- **Business logic**: how a score or decision is computed, signals we use and why (ex) chat speed as the impact
  signal), windows, thresholds, weights, retry/temperature schedules, title or filter rules, priority rules.
  Say "we added logic to avoid cutting off text", not the rule itself.
- **Algorithms and model architecture**: what the model predicts, heads/queries/resamplers, post-processing rules.
- **System architecture**: engine lists, data flow details, polling intervals, serving stack (Triton, KServe,
  SGLang, Redis, Kafka), replica/GPU counts per service. Architecture diagrams stay at "source -> models -> output".
- **Real frames and screenshots**: no broadcast/stream frames, app screenshots of real content, internal dashboards
  or demo pages. Covers and figures are hand-made diagrams (render the SVG to `cover.png` with headless Chrome).
- **Internal data and operations**: business analysis numbers (view counts, label counts), incident counts and
  details, internal dates of rollouts.

Generic public techniques are fine (ex) DeepSpeed ZeRO-3, multi-LoRA serving, "3 GPUs -> 1"), and so are headline
results the post is about (ex) "text cuts 23% -> 7%", "x3.7 throughput per GPU").

Before finishing a work post, grep the markdown **and** SVG/image text for internal names, model names, `\d+[BM]`
sizes, framework names and threshold-like numbers, and look at every image.
