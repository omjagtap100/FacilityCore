# FacilityCore – starting prompt for Cursor Agent

Put FacilityCore_BRD_v2.docx and FacilityCore_Gap_Analysis.docx in a `docs/` folder of a new, empty project folder, open that folder in Cursor, then paste the prompt below into Agent mode.

---

You're building FacilityCore, a building/housing-society maintenance app. The full requirements are in docs/FacilityCore_BRD_v2.docx, and the open gaps are in docs/FacilityCore_Gap_Analysis.docx. Read both fully, including the tables, before writing any code.

1. Summarise the BRD into docs/REQUIREMENTS.md (REQ IDs, roles, screens, build phases) so it's easy to reference later.
2. Propose a tech stack that fits Section 10 (Technical Assumptions) and Section 9 (NFRs): a mobile app for residents, an admin web dashboard, a backend API, and a database. Wait for my approval before scaffolding.
3. Follow Section 12 (Build Phases): build one phase at a time, write tests for its "check", and stop for my review after each phase.
4. Wherever Section 11 (Decisions Needed) is unresolved, use the suggested answer, make it configurable, and record it in docs/DECISIONS.md.
5. Keep the optional add-ons (gate security / face checks, paid services) behind feature flags, switched off by default, and build them last, as the BRD says.
6. Keep a README with setup steps, and keep docs/PROJECT_NOTES.md updated so a teammate can pick up the work.

Start with steps 1 and 2.
