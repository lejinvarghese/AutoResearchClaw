# AutoResearchClaw - Draft Extension Fork

**Fork Purpose:** Extend existing research paper drafts instead of generating from scratch.

This fork adds the ability to **preserve and extend** your existing research paper drafts with literature citations and section expansions, rather than generating completely new papers.

## 🎯 Key Feature: Draft Extension Mode

When you provide an existing draft, this fork:
- ✅ **Preserves** your paper's structure, titles, and mathematical framework
- ✅ **Extracts** TODOs automatically from your draft
- ✅ **Adds** citations from literature search to address gaps
- ✅ **Extends** incomplete sections with relevant research
- ✅ **Maintains** your writing style and notation

## 🚀 Quick Start

```bash
# 1. Clone this fork
git clone https://github.com/lejinvarghese/AutoResearchClaw.git
cd AutoResearchClaw
git checkout relationship-economics-extension

# 2. Install
python3.11 -m venv .venv
source .venv/bin/activate
pip install -e .

# 3. Configure (copy and edit config)
cp config.researchclaw.example.yaml config.arc.yaml

# 4. Add your draft path to config.arc.yaml:
#    research:
#      input_draft: "/path/to/your/draft.md"
#      topic: "Your paper topic"
#      domains: ["domain1", "domain2"]

# 5. Run
export OPENAI_API_KEY="sk-..."
researchclaw run --config config.arc.yaml --auto-approve
```

## 📖 Full Documentation

See **[DRAFT_EXTENSION_GUIDE.md](DRAFT_EXTENSION_GUIDE.md)** for:
- Complete setup instructions
- Configuration reference
- Best practices
- Troubleshooting
- Example workflows

## ✨ What's Different from Original?

### Original AutoResearchClaw:
- Generates research papers from scratch
- Starts with topic → finds literature → writes complete paper

### This Fork:
- **Extends existing drafts** with your content preserved
- Reads your paper → extracts TODOs → adds citations → fills gaps
- Perfect for theoretical papers that need literature support

## 📊 Example Result

**Input:**
- Paper: "Relationship Economics" (36KB, 16 sections, 16 TODOs)
- TODOs: Missing citations for thermodynamic foundations, network theory, etc.

**Output:**
- ✅ Original title & structure preserved
- ✅ 9 TODOs addressed with proper citations
- ✅ Added 200+ relevant citations from 5 domains
- ✅ 18KB of extensions maintaining original style

**Time:** ~45 minutes

## 🔧 Technical Changes

**Modified Files:**
1. `researchclaw/config.py` - Added `input_draft` config field
2. `researchclaw/pipeline/executor.py` - Draft reading & extension logic with strong prompts

**Key Innovation:** Strongly-worded visual prompts that force LLMs to extend rather than regenerate.

## 📁 Branch

Main extension work: `relationship-economics-extension`

## 🙏 Credits

- **Original**: [aiming-lab/AutoResearchClaw](https://github.com/aiming-lab/AutoResearchClaw)
- **Fork**: Draft extension feature for theoretical research papers
- **Author**: lejinvarghese

## 📝 License

Same as original AutoResearchClaw repository.

---

**For detailed usage instructions, see [DRAFT_EXTENSION_GUIDE.md](DRAFT_EXTENSION_GUIDE.md)**
