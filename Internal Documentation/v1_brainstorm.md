# Internal Documentation/v1_brainstorm.md
# Step 1 – Seed Dataset
Collect a CSV dataset of MIT students in the exact form `(first_name, last_name, class_year)`.  
This becomes the initial input queue for the entire pipeline. No other fields are required at ingest.

# Step 2 – Family Tree Construction (LLM_Policy_1)
For each student in the dataset, invoke LLM_Policy_1 to extract immediate family members and either create a new family_file or merge into an existing one (students sharing a family share the identical family_file on disk).

Secondary iteration: re-apply LLM_Policy_1 to every newly discovered family member to grow the tree outward. This process is fully iterative and can be run multiple times without side effects.

## family_file
Central persistent record for one family.  
Stored as a single JSON/Parquet document keyed by a deterministic family_id (e.g., hash of sorted member names + earliest class year).  
Contains:
- A map of `member_id` → `personal_data`
- A separate `relationship_edges` array for inverse links (parent/child, sibling, etc.)

## LLM_Policy_1 (p1)
Inputs: student name + class_year OR an existing family_file  
Outputs: list of candidate family members (with confidence scores) to merge/append  

Key invariants:
- Functions with or without any personal_data already present
- When personal_data from LLM_Policy_2 is available, it returns higher-recall family members (this differential is the empirical basis for the future graph_df ablation study)
- Strictly append-only: never overwrites, deletes, or corrupts existing family_file or personal_data blocks
- Idempotent: same input always produces the same merge result

# Step 3 – Profile Enrichment (LLM_Policy_2)
Develop LLM_Policy_2 to consume a family_file (and any existing personal_data) and output structured updates in the personal_data schema.

Two supported execution modes (chosen at runtime):
- Per-member mode: run independently on each member (modular, token-efficient, easy parallelization)
- Whole-family mode: feed the entire family_file at once (leverages inter-member context for higher accuracy; theory is that richer connected personal_data will surface more graph nodes)

## LLM_Policy_2
Purpose: generate or expand personal_data for any individual or entire family_file.  
Inputs: family_file (full or partial) + optional target member_id  
Outputs: updated personal_data blocks (delta only) to be merged back idempotently.

## personal_data
Nested structure inside family_file for each family member.  
Supports two distinct linking mechanisms:
1. Shared-asset linking (household-level): updates propagate bidirectionally to all linked members (e.g., same address, shared trusts)
2. Relationship linking (inverse-aware): edges stored separately so parent/child or sibling links remain distinct and non-duplicative

### High-level categories of columns (schema skeleton – not exhaustive)
#### Assets
#### Early Life
- Status acquisition path
#### Relationships
- Siblings
- Parents
- Friends
#### Education
#### Career
- Industry
- Salary range
- Title
- Company
- Employment history
- Internships
#### Affiliations
- Organizations
- Non-profits
- Trusts
- Start-ups
- Projects
#### Internet Accounts
- Social Media (LinkedIn, Instagram, Facebook)
- Emails (Google, Outlook, iCloud, institutional, work)
- GitHub
#### MIT Specific
- Major
- Classes taken
- Research groups / theses

# Future Works
## Graph Theory Representation of a digital footprint (graph_df)
TBD – explicitly out of scope for MVP. Will be built on top of the family_file + relationship_edges once the two policies are stable and idempotent.