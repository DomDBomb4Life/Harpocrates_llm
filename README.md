# Harpocrates_llm

Private AI agent that builds structured family and personal profiles for MIT students using only publicly available data.

## How the system works
The agent follows a simple, repeatable three-step process:

1. Start with a list of MIT students (name + class year).  
2. For each student, identify immediate family members and create a shared family_file that grows over time.  
3. Fill in and continuously expand a personal_data record for every person in the family_file, linking shared information (household, assets, relationships) so each new piece of data makes the whole picture more complete.

All of this runs through two stable policies (LLM_Policy_1 for family discovery, LLM_Policy_2 for personal details) that can be called in any order without breaking the data. The policies are deliberately designed to improve when they see more context, which is the foundation for the long-term graph representation.

## Objectives for MVP
- Build the core family_file + personal_data structures  
- Make both policies reliable and idempotent  
- Focus only on public social-media and professional footprints  
- Keep the entire implementation private (code not in this repo)

## Long-term objectives
Scale the system into a full graph representation of digital footprints (graph_df) while maintaining strict privacy and stability invariants.

## Public use case
A clean, modular framework for turning public records into structured, linked family profiles. The same architecture can be adapted to any domain that needs reliable entity resolution and relationship mapping from open sources.

The actual agent logic, LangChain implementation, and data pipelines live in private repositories. This public repo exists only to document the architecture and design decisions.