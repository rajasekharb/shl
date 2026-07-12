- What percent of your context window is currently used, and how much of that is irreplaceable working state versus reconstructable history?

- If my prompt is ambiguous or has multiple interpretations, list assumption(s), ask for multiple cohesive questions for clarification(s). And then proceed.

- Reason rigorously step by step, weigh alternative approaches, and challenge your first instinct before committing. After weighing, commit to a recommendation with its rationale and your confidence.

- Think step by step and ground your responses to only authoritative information from trusted sources in the respective domain and not just from training data.

-   Ambiguity:
    If my prompt is ambiguous or has multiple interpretations, list your assumptions and ask focused clarifying questions — then proceed.

    Reasoning:
    Think step by step and ground your responses in authoritative information from trusted domain sources, not just training data.

## Claude Context Window List:

#: 1
File (~ relative): ~/.claude/CLAUDE.md
What it is: Global "STEM Expert Mode" instructions

#: 2
File (~ relative): ~/Claude/Projects/LadybugDBStudy/SyneCompiler/CLAUDE.md
What it is: Project (Syne) instructions

#: 3
File (~ relative): ~/.claude/projects/-Users-phoenix-Claude-Projects-LadybugDBStudy-SyneCompiler/memory/MEMORY.md
What it is: Auto-memory index

#: 4
File (~ relative): ~/Claude/Projects/LadybugDBStudy/SyneCompiler/.remember/recent.md
What it is: Remember buffer (7-day) — content injected by SessionStart hook

#: 5
File (~ relative): ~/Claude/Projects/LadybugDBStudy/SyneCompiler/.remember/archive.md
What it is: Remember archive — content injected by SessionStart hook

#: 6
File (~ relative): ~/.claude/plugins/cache/claude-plugins-official/superpowers/6.0.3/skills/using-superpowers/SKILL.md
What it is: Superpowers bootstrap skill — full text injected by hook

Referenced/named in those files but not loaded (only pointers are in context):

- Memory entries behind MEMORY.md: adr-governance.md, syne-roadmap.md, why-javaparser-not-spoon-yet.md, style-tooling-setup.md, guidelines-compliance-pass.md — all under ~/.claude/projects/-Users-phoenix-Claude-Projects-LadybugDBStudy-SyneCompiler/memory/
- Other .remember/ files merely named by the hook: now.md (empty), today-2026-06-22…27.done.md, core-memories.md, plus logs/
- Untracked working-tree items named in git status only: ~/Claude/Projects/LadybugDBStudy/SyneCompiler/Board.md and com/


## Gemini Context Window List:

### Workspace & Configuration Directories
- `~/Claude/Projects/LadybugDBStudy/SyneCompiler` (Active Workspace)
- `~/Claude/Projects/LadybugDBStudy/SyneCompiler/.agents` (Workspace Customizations Root)
- `~/.gemini/antigravity-ide` (App Data Directory)
- `~/.gemini/config` (Global Customizations Root)
- `~/.gemini/antigravity-ide/brain/4e1b5fff-3107-4c8c-b45f-1c30a11b21e4` (Conversation Artifacts Directory)
- `~/.gemini/antigravity-ide/brain/4e1b5fff-3107-4c8c-b45f-1c30a11b21e4/scratch` (Scratch Scripts/Files Directory)
- `~/.gemini/antigravity-ide/knowledge` (Knowledge Items Directory)

### Framework & Knowledge Files
- `~/Claude/Projects/LadybugDBStudy/SyneCompiler/.agents/AGENTS.md` (Workspace-scoped Rules)
- `~/Claude/Projects/LadybugDBStudy/SyneCompiler/.agents/skills.json` (Workspace-scoped Skills Registration)
- `~/.gemini/config/AGENTS.md` (Global Rules)
- `~/.gemini/config/skills.json` (Global Skills Registration)
- `~/.gemini/antigravity-ide/knowledge/metadata.json` (Knowledge Items Metadata)

### System Logs & Conversation Artifacts (Available upon creation)
- `~/.gemini/antigravity-ide/brain/4e1b5fff-3107-4c8c-b45f-1c30a11b21e4/.system_generated/logs/transcript.jsonl` (Compact Transcript)
- `~/.gemini/antigravity-ide/brain/4e1b5fff-3107-4c8c-b45f-1c30a11b21e4/.system_generated/logs/transcript_full.jsonl` (Full Transcript)
- `~/.gemini/antigravity-ide/brain/4e1b5fff-3107-4c8c-b45f-1c30a11b21e4/task.md` (Execution Checklist)
- `~/.gemini/antigravity-ide/brain/4e1b5fff-3107-4c8c-b45f-1c30a11b21e4/implementation_plan.md` (Design/Implementation Plan)
- `~/.gemini/antigravity-ide/brain/4e1b5fff-3107-4c8c-b45f-1c30a11b21e4/walkthrough.md` (Summary of Accomplished Work)

### Plugin Manifests (`plugin.json`)
- `~/.gemini/config/plugins/android-cli-plugin/plugin.json`
- `~/.gemini/config/plugins/chrome-devtools-plugin/plugin.json`
- `~/.gemini/config/plugins/firebase/plugin.json`
- `~/.gemini/config/plugins/flutter/plugin.json`
- `~/.gemini/config/plugins/google-antigravity-sdk/plugin.json`
- `~/.gemini/config/plugins/modern-web-guidance-plugin/plugin.json`
- `~/.gemini/config/plugins/science/plugin.json`

### Skill Instructions (`SKILL.md`)
**General / Core Skills**
- `~/.gemini/config/skills/context-introspection/SKILL.md`
- `~/.gemini/config/skills/deep-thinking/SKILL.md`
- `~/.gemini/config/skills/just-refine/SKILL.md`
- `~/.gemini/config/skills/refine-prompt/SKILL.md`
- `~/.gemini/config/plugins/google-antigravity-sdk/skills/google-antigravity-sdk/SKILL.md`

**Web & Chrome DevTools**
- `~/.gemini/config/plugins/chrome-devtools-plugin/skills/a11y-debugging/SKILL.md`
- `~/.gemini/config/plugins/chrome-devtools-plugin/skills/chrome-devtools/SKILL.md`
- `~/.gemini/config/plugins/chrome-devtools-plugin/skills/debug-optimize-lcp/SKILL.md`
- `~/.gemini/config/plugins/chrome-devtools-plugin/skills/memory-leak-debugging/SKILL.md`
- `~/.gemini/config/plugins/chrome-devtools-plugin/skills/troubleshooting/SKILL.md`
- `~/.gemini/config/plugins/modern-web-guidance-plugin/skills/chrome-extensions/SKILL.md`
- `~/.gemini/config/plugins/modern-web-guidance-plugin/skills/modern-web-guidance/SKILL.md`

**Firebase**
- `~/.gemini/config/plugins/firebase/skills/firebase_ai_logic_basics/SKILL.md`
- `~/.gemini/config/plugins/firebase/skills/firebase_app_hosting_basics/SKILL.md`
- `~/.gemini/config/plugins/firebase/skills/firebase_auth_basics/SKILL.md`
- `~/.gemini/config/plugins/firebase/skills/firebase_basics/SKILL.md`
- `~/.gemini/config/plugins/firebase/skills/firebase_crashlytics/SKILL.md`
- `~/.gemini/config/plugins/firebase/skills/firebase_data_connect_basics/SKILL.md`
- `~/.gemini/config/plugins/firebase/skills/firebase_firestore/SKILL.md`
- `~/.gemini/config/plugins/firebase/skills/firebase_hosting_basics/SKILL.md`
- `~/.gemini/config/plugins/firebase/skills/firebase_remote_config_basics/SKILL.md`
- `~/.gemini/config/plugins/firebase/skills/firebase_security_rules_auditor/SKILL.md`
- `~/.gemini/config/plugins/firebase/skills/xcode_project_setup/SKILL.md`

**Flutter / Dart / Android**
- `~/.gemini/config/plugins/android-cli-plugin/skills/SKILL.md`
- `~/.gemini/config/plugins/flutter/skills/dart-add-unit-test/SKILL.md`
- `~/.gemini/config/plugins/flutter/skills/dart-build-cli-app/SKILL.md`
- `~/.gemini/config/plugins/flutter/skills/dart-collect-coverage/SKILL.md`
- `~/.gemini/config/plugins/flutter/skills/dart-fix-runtime-errors/SKILL.md`
- `~/.gemini/config/plugins/flutter/skills/dart-generate-test-mocks/SKILL.md`
- `~/.gemini/config/plugins/flutter/skills/dart-migrate-to-checks-package/SKILL.md`
- `~/.gemini/config/plugins/flutter/skills/dart-resolve-package-conflicts/SKILL.md`
- `~/.gemini/config/plugins/flutter/skills/dart-run-static-analysis/SKILL.md`
- `~/.gemini/config/plugins/flutter/skills/dart-setup-ffi-assets/SKILL.md`
- `~/.gemini/config/plugins/flutter/skills/dart-use-ffigen/SKILL.md`
- `~/.gemini/config/plugins/flutter/skills/dart-use-pattern-matching/SKILL.md`
- `~/.gemini/config/plugins/flutter/skills/flutter-add-integration-test/SKILL.md`
- `~/.gemini/config/plugins/flutter/skills/flutter-add-widget-preview/SKILL.md`
- `~/.gemini/config/plugins/flutter/skills/flutter-add-widget-test/SKILL.md`
- `~/.gemini/config/plugins/flutter/skills/flutter-apply-architecture-best-practices/SKILL.md`
- `~/.gemini/config/plugins/flutter/skills/flutter-build-responsive-layout/SKILL.md`
- `~/.gemini/config/plugins/flutter/skills/flutter-fix-layout-issues/SKILL.md`
- `~/.gemini/config/plugins/flutter/skills/flutter-implement-json-serialization/SKILL.md`
- `~/.gemini/config/plugins/flutter/skills/flutter-setup-declarative-routing/SKILL.md`
- `~/.gemini/config/plugins/flutter/skills/flutter-setup-localization/SKILL.md`
- `~/.gemini/config/plugins/flutter/skills/flutter-use-http-package/SKILL.md`

**Science / Biology / Bioinformatics**
- `~/.gemini/config/plugins/science/skills/alphafold_database_fetch_and_analyze/SKILL.md`
- `~/.gemini/config/plugins/science/skills/alphagenome_single_variant_analysis/SKILL.md`
- `~/.gemini/config/plugins/science/skills/chembl_database/SKILL.md`
- `~/.gemini/config/plugins/science/skills/clinical_trials_database/SKILL.md`
- `~/.gemini/config/plugins/science/skills/clinvar_database/SKILL.md`
- `~/.gemini/config/plugins/science/skills/dbsnp_database/SKILL.md`
- `~/.gemini/config/plugins/science/skills/embl_ebi_ols/SKILL.md`
- `~/.gemini/config/plugins/science/skills/encode_ccres_database/SKILL.md`
- `~/.gemini/config/plugins/science/skills/ensembl_database/SKILL.md`
- `~/.gemini/config/plugins/science/skills/foldseek_structural_search/SKILL.md`
- `~/.gemini/config/plugins/science/skills/gnomad_database/SKILL.md`
- `~/.gemini/config/plugins/science/skills/gtex_database/SKILL.md`
- `~/.gemini/config/plugins/science/skills/human_protein_atlas_database/SKILL.md`
- `~/.gemini/config/plugins/science/skills/interpro_database/SKILL.md`
- `~/.gemini/config/plugins/science/skills/jaspar_database/SKILL.md`
- `~/.gemini/config/plugins/science/skills/literature_search_arxiv/SKILL.md`
- `~/.gemini/config/plugins/science/skills/literature_search_biorxiv/SKILL.md`
- `~/.gemini/config/plugins/science/skills/literature_search_europepmc/SKILL.md`
- `~/.gemini/config/plugins/science/skills/literature_search_openalex/SKILL.md`
- `~/.gemini/config/plugins/science/skills/ncbi_sequence_fetch/SKILL.md`
- `~/.gemini/config/plugins/science/skills/openfda_database/SKILL.md`
- `~/.gemini/config/plugins/science/skills/opentargets_database/SKILL.md`
- `~/.gemini/config/plugins/science/skills/pdb_database/SKILL.md`
- `~/.gemini/config/plugins/science/skills/protein_sequence_msa/SKILL.md`
- `~/.gemini/config/plugins/science/skills/protein_sequence_similarity_search/SKILL.md`
- `~/.gemini/config/plugins/science/skills/pubchem_database/SKILL.md`
- `~/.gemini/config/plugins/science/skills/pubmed_database/SKILL.md`
- `~/.gemini/config/plugins/science/skills/pymol/SKILL.md`
- `~/.gemini/config/plugins/science/skills/quickgo_database/SKILL.md`
- `~/.gemini/config/plugins/science/skills/reactome_database/SKILL.md`
- `~/.gemini/config/plugins/science/skills/scienceskillscommon/SKILL.md`
- `~/.gemini/config/plugins/science/skills/string_database/SKILL.md`
- `~/.gemini/config/plugins/science/skills/ucsc_conservation_and_tfbs/SKILL.md`
- `~/.gemini/config/plugins/science/skills/unibind_database/SKILL.md`
- `~/.gemini/config/plugins/science/skills/uniprot_database/SKILL.md`
- `~/.gemini/config/plugins/science/skills/uv/SKILL.md`
- `~/.gemini/config/plugins/science/skills/workflow_skill_creator/SKILL.md`

