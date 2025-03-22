# [IlliniPlan.com](https://www.illiniplan.com/)

### Demo:

[![IlliniPlan Demo](https://img.youtube.com/vi/_niGGKMig3s/0.jpg)](https://www.youtube.com/watch?v=_niGGKMig3s)

## Motivation

The primary motivation is to streamline the degree planning process. Students often face complexity when ensuring they meet all institutional requirements and prerequisites, aligning courses with their personal interests, and maintaining a legal schedule (no prerequisite violations, no duplicates, no courses placed in unavailable semesters). This project addresses these challenges by providing automated checks, intelligent recommendations, and a visual map to reduce the overhead and confusion often associated with manual academic planning.

---

## Key Features

### Complete Degree Planning

- Students can map every course needed for their entire degree.
- Tracks all general education, major-specific, and nuanced requirements (e.g., partial credit-hour categories).

### Tree-Like Data Structure

- Courses are represented as nodes in a tree-like structure.
- Captures relationships such as prerequisites and dependent courses.
- Hovering over a node highlights its prerequisite chain and dependent nodes.

### Graduation Readiness Tracking

- Continuously evaluates the student’s plan against graduation requirements.
- Indicates when a student will be ready to graduate.
- Specifies unmet conditions and how to address them.

### Nuanced Requirement Management

- Handles intricate requirements like partial credit distributions (e.g., "5/6 credit hours of Non-Western Cultural Studies").
- Recommends courses that fulfill specialized requirements effectively.

### Automated Schedule Validation

- Enforces rules to prevent the creation of invalid schedules:
  - No enrolling in courses before completing prerequisites.
  - No duplicate courses.
  - No placing courses in semesters where they are not offered.
  - Warns users about courses requiring special approval.
- Ensures schedules align with institutional policies.

### Dynamic Reorganization

- Automatically reorganizes schedules if requirements cannot be met in the current plan.
- Uses an integrated LLM to suggest course swaps or replacements while respecting user preferences and institutional rules.

### Personalized Course Recommendations

- Recommends courses based on user-defined criteria, such as:
  - Courses satisfying multiple requirements simultaneously.
  - Highest average GPA courses in a specific category.
  - Electives aligned with user interests (e.g., AI or game development for CS students).

### Data Integration for Decision-Making

- Displays external metrics, such as average GPA data for individual courses (e.g., UIUC grade disparity dataset), to help users make informed decisions.

### Conflict and Time Management

- Detects and prevents scheduling conflicts (e.g., overlapping course times).
- Ensures time constraints are respected during course planning.

### Exporting Capability

- Users can export fully validated course schedules to Google Sheets.
- Facilitates easy sharing with advisors and other stakeholders.

---

## LLM Integration

The app integrates a conversational LLM assistant to provide:

- Interpretation of degree requirements and user preferences.
- Suggestions for course additions or replacements to meet missing requirements.
- Reorganized course maps when no standard solutions fit.
- Personalized recommendations for electives and general education based on user-defined criteria.

---

## Usage Scenarios

- **Graduation Readiness:** Determine if a student is ready to graduate and identify specific missing requirements.
- **Requirement Fulfillment:** Find courses that satisfy multiple requirement categories simultaneously.
- **Dynamic Adjustments:** Recommend course swaps or reorganized schedules when gaps or conflicts arise.
- **Personalized Course Selection:** Tailor course recommendations to user interests, academic goals, or performance metrics (e.g., higher GPA courses).

---

## Data Flow

1. **Course Catalog Integration:**
   - The system ingests the institution’s complete course catalog.
2. **Requirement Analysis:**
   - Matches degree requirements against planned courses.
3. **Constraint Enforcement:**
   - Tree-like structures and logic ensure prerequisites, eligibility, and scheduling rules are not violated.
4. **Real-Time Adjustments:**
   - An LLM and rules engine optimize schedules dynamically upon user input or requirement changes.

This robust data flow ensures a seamless experience, building intelligent, constraint-driven academic plans that adapt to user needs while maintaining institutional compliance.
