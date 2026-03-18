# Draft Extension Guide for AutoResearchClaw

This fork of AutoResearchClaw adds the ability to **extend existing research paper drafts** rather than generating papers from scratch. Perfect for theoretical papers with TODOs that need literature citations and section expansions.

## 🎯 What This Fork Does

Instead of generating a completely new paper, this fork:
- ✅ **Preserves** your existing paper structure, titles, and content
- ✅ **Extracts** TODOs from your draft automatically
- ✅ **Adds** citations from literature search to fill gaps
- ✅ **Extends** incomplete sections with relevant research
- ✅ **Maintains** your mathematical notation and writing style

## 📋 Prerequisites

- Python 3.11+
- OpenAI API key (or OpenRouter/other LLM provider)
- An existing research paper draft in Markdown format

## 🚀 Quick Start

### 1. Clone This Fork

```bash
git clone https://github.com/lejinvarghese/AutoResearchClaw.git
cd AutoResearchClaw
git checkout relationship-economics-extension
```

### 2. Install

```bash
python3.11 -m venv .venv
source .venv/bin/activate
pip install -e .
```

### 3. Configure

Copy the example config and edit it:

```bash
cp config.researchclaw.example.yaml config.arc.yaml
```

Edit `config.arc.yaml`:

```yaml
project:
  name: "my-research"
  mode: "full-auto"

research:
  topic: "Brief description of your paper's topic"
  domains:
    - "domain-1"  # e.g., network-science
    - "domain-2"  # e.g., behavioral-economics
    - "domain-3"  # Add relevant domains
  daily_paper_count: 15
  quality_threshold: 4.5
  input_draft: "/absolute/path/to/your/draft.md"  # ← KEY: Your draft path

llm:
  provider: "openai"
  base_url: "https://api.openai.com/v1"
  api_key_env: "OPENAI_API_KEY"
  api_key: ""  # Or use environment variable

  primary_model: "gpt-4o"
  fallback_models:
    - "gpt-4o-mini"
```

### 4. Prepare Your Draft

Your draft should be in Markdown with clear TODO markers:

```markdown
# My Paper Title

## Section 1: Introduction

Content here...

## Section 2: Related Work

> **TODO**: Add citations to Smith (2020), Jones (2021) on topic X.

Content here...

### Global TODOs
- [ ] Add worked example
- [ ] Create simulation architecture
```

**Supported TODO patterns:**
- `> **TODO**: Description`
- `- [ ] Description`

### 5. Run

```bash
export OPENAI_API_KEY="sk-..."
researchclaw run --config config.arc.yaml --auto-approve
```

### 6. Get Results

Extended draft will be at:
```bash
artifacts/rc-YYYYMMDD-HHMMSS-*/stage-17/paper_draft.md
```

## 📖 How It Works

### Stage 17: Paper Draft Extension

When AutoResearchClaw reaches Stage 17 (PAPER_DRAFT), our modifications:

1. **Read your draft** from `config.research.input_draft`
2. **Extract TODOs** using regex patterns
3. **Activate EXTENSION MODE** with strong prompts
4. **Generate extensions** that address TODOs with citations
5. **Preserve** all existing complete sections

### The Extension Prompt

The key innovation is a **strongly-worded prompt** that forces the LLM to extend rather than replace:

```
╔══════════════════════════════════════════════════════════════╗
║ 🚨 DRAFT EXTENSION MODE - NOT NEW PAPER GENERATION 🚨 ║
╚══════════════════════════════════════════════════════════════╝

STRICT RULES:
❌ DO NOT generate new paper
❌ DO NOT rewrite existing sections
✅ DO add citations to fill TODOs
✅ DO preserve all mathematical notation

[Shows: N sections, M axioms, K TODOs]
[Includes: First 2000 chars of your draft]
```

This visual, explicit format successfully forces GPT-4o to extend rather than regenerate.

### Output Format

The extended draft includes:

```markdown
# Your Original Title

[Your original table of contents preserved]

## EXTENDED SECTIONS - Building on Existing Draft

## Addressing TODO [Line X]: Topic Name
[New content with citations from literature search]

## Addressing TODO [Line Y]: Another Topic
[More extensions...]

## Method
[Additional content if needed]
```

## 🎯 Best Practices

### For Best Results:

**1. Clear TODO Markers**
```markdown
✅ Good:
> **TODO**: Add citations to Prigogine (1977) on dissipative structures.

❌ Less Good:
Note: Need more citations here.
```

**2. Specific Research Domains**
```yaml
domains:
  - "network-science"        # Specific
  - "behavioral-economics"   # Clear

# Avoid vague domains like "general" or "science"
```

**3. Complete Existing Sections**

The extension works best when:
- Your draft has clear structure (## headers)
- Complete sections are fully written
- TODOs are specific about what's missing

**4. Choose Right LLM Provider**

For theoretical papers:
- **OpenAI GPT-4o**: Best for following complex instructions
- **Claude (via OpenRouter)**: Good for mathematical content
- **GPT-4o-mini**: Faster but may not follow extension instructions as well

## 📊 Configuration Reference

### Key Fields for Draft Extension

| Field | Description | Example |
|-------|-------------|---------|
| `research.input_draft` | **Required.** Path to your draft | `/home/user/papers/draft.md` |
| `research.topic` | Brief topic description | `Mathematical framework for relationships` |
| `research.domains` | Relevant research domains | `["network-science", "game-theory"]` |
| `research.daily_paper_count` | Papers to retrieve per domain | `15` (recommended) |
| `research.quality_threshold` | Minimum citation count filter | `4.5` |

### LLM Provider Options

**OpenAI (Recommended):**
```yaml
llm:
  provider: "openai"
  base_url: "https://api.openai.com/v1"
  api_key_env: "OPENAI_API_KEY"
  primary_model: "gpt-4o"
```

**OpenRouter (Claude/others):**
```yaml
llm:
  provider: "openrouter"
  base_url: "https://openrouter.ai/api/v1"
  api_key_env: "OPENROUTER_API_KEY"
  primary_model: "anthropic/claude-3.5-sonnet"
```

## 🔧 Troubleshooting

### Issue: Extension mode not activating

**Symptom:** Output has different title, doesn't preserve structure

**Solution:**
1. Verify `input_draft` path is correct and file exists
2. Check draft file is UTF-8 encoded
3. Ensure package is installed: `pip install -e .`
4. Check logs for "🔧 EXTENSION MODE ACTIVATED" message

### Issue: TODOs not being addressed

**Symptom:** Extension generated but doesn't address specific TODOs

**Solutions:**
1. Use supported TODO formats: `> **TODO**:` or `- [ ]`
2. Make TODOs specific (mention authors, topics, years)
3. Ensure literature search found relevant papers

### Issue: Generated paper too short

**Symptom:** Extended draft only a few hundred lines

**Check:**
- Literature search succeeded (check `stage-04/`)
- Experiment stages completed (can fail for theoretical papers - OK)
- Draft had clear incomplete sections

### Issue: Citations not relevant

**Solution:**
- Refine `research.domains` to be more specific
- Increase `research.daily_paper_count`
- Adjust `research.quality_threshold`

## 📁 Output Files

After completion, check these directories:

```
artifacts/rc-YYYYMMDD-HHMMSS-*/
├── stage-01/goal.md                    # Research goal extracted
├── stage-04/papers.json                # Literature search results
├── stage-17/paper_draft.md            # ⭐ Your extended draft
├── stage-17/references_preverified.bib # All citations found
├── stage-19/paper_revised.md          # Peer-reviewed version
├── stage-22/paper_final.md            # Final polished version
├── stage-22/references.bib            # Clean references
├── stage-22/paper.tex                 # LaTeX version
└── deliverables/                      # Complete publication package
```

**Key files:**
- **stage-17/paper_draft.md** - Main extended draft
- **stage-17/references_preverified.bib** - All citations to review
- **stage-22/paper_final.md** - Polished final version

## 🎓 Example Workflow

### Real Example: "Relationship Economics" Paper

**Original Draft:**
- 36KB, 16 sections, 8 axioms, 16 TODOs
- Theoretical framework with mathematical formulations
- Missing citations for thermodynamic foundations

**Configuration:**
```yaml
research:
  topic: "Mathematical framework for relationships as resource exchange networks"
  domains:
    - "network-science"
    - "behavioral-economics"
    - "evolutionary-biology"
    - "complex-systems"
    - "game-theory"
  input_draft: "/path/to/relationship_economics.md"
```

**Result:**
- ✅ Original title and structure preserved
- ✅ 9 TODOs addressed with proper citations
- ✅ Added: Prigogine (1977), Kauffman (1993), England (2013)
- ✅ Added: Gottman (relationships), Sutton & Barto (RL)
- ✅ 18KB of extensions with 200+ citations

**Time:** ~45 minutes end-to-end

## 🤝 Contributing

Improvements welcome! Key areas:

1. **Better TODO extraction** - Support more formats
2. **Diagram handling** - Preserve/reference figures
3. **Custom prompt templates** - Let users customize extension prompts
4. **Selective extension** - Only extend specific sections

## 📚 Technical Details

### Modified Files

1. **`researchclaw/config.py`**
   - Added `input_draft: str` field to `ResearchConfig`
   - Config validation includes draft path

2. **`researchclaw/pipeline/executor.py`**
   - `_execute_paper_draft()`: Reads draft, extracts TODOs
   - `_write_paper_sections()`: Modified prompts for extension mode
   - Strong visual prompts force LLM compliance

### Extension Logic Flow

```python
def _execute_paper_draft(...):
    # 1. Read draft if configured
    if config.research.input_draft:
        draft_content = Path(config.research.input_draft).read_text()
        todos = extract_todos(draft_content)

    # 2. Pass to paper writing
    draft = _write_paper_sections(
        ...,
        input_draft_content=draft_content,
        input_draft_todos=todos
    )

def _write_paper_sections(..., input_draft_content, input_draft_todos):
    # 3. Activate extension mode if draft provided
    if input_draft_content:
        extension_instruction = build_strong_prompt(
            draft_content, todos, sections, axioms
        )

    # 4. Modify task from "Write" to "EXTEND"
    task = "EXTEND" if extension_mode else "Write"

    # 5. Generate with extension-aware prompts
    result = llm.chat(system=system, user=extension_instruction + ...)
```

### Why It Works

**Key insight:** LLMs need **very explicit, visual, repeated instructions** to override their default behavior of generating from scratch.

The prompt engineering includes:
- Visual warning boxes (╔══╗ borders)
- Multiple DO/DON'T rule statements
- Shows existing structure to preserve
- Requires specific output format
- Repeats "NOT NEW PAPER" 3+ times

This forces GPT-4o to actually extend rather than regenerate.

## 📖 Related Resources

- [AutoResearchClaw Original](https://github.com/aiming-lab/AutoResearchClaw)
- [This Fork](https://github.com/lejinvarghese/AutoResearchClaw)
- [Branch](https://github.com/lejinvarghese/AutoResearchClaw/tree/relationship-economics-extension)

## 🙏 Acknowledgments

This fork builds on [AutoResearchClaw](https://github.com/aiming-lab/AutoResearchClaw) by the aiming-lab team.

Draft extension feature developed for extending theoretical research papers with literature citations.

---

**Questions or issues?** Open an issue on the fork repository!
