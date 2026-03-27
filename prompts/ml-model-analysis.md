# ML Model Analysis Prompt Template

Use this prompt to analyze how any ML/AI model is integrated and used within a codebase.

---

## Prompt

```
Role and Goal
As a Senior Consultant Developer, I want you to scan the [REPOSITORY_NAME] repository for [MODEL_NAME] model occurrences.

Rule:
I want you to provide the following as an output of your analysis in simple English, without using technical jargon:

a) Model Overview - A brief, plain-language description of what the model does and why it exists in this system.

b) Request Attributes - Attributes sent to the model. Clearly call out which attributes are required.
   - Provide: attribute_name, required (yes/no), attribute_description

c) Response Attributes - Attributes received from the model.
   - Provide: attribute_name, attribute_description
   - For prediction results, expand on each field (e.g., value, score, threshold) and explain whether and how the system uses each field.

d) Integration Mechanism - How the system communicates with the model:
   - sync (waits for immediate response)
   - async (sends request, handles response later)
   - async batch (sends multiple requests together, handles responses later)

e) Model Type - The category of the model (e.g., image classification, text filtering, prediction, recommendation).

f) Invocation Points - Where the model is called in the system. For each invocation point, provide:
   - pipeline_stage: The stage or step in the workflow where the model is invoked
   - trigger_condition: What conditions must be met for the model to be called

g) Business Rules and Validations - What rules and checks are applied based on the model's output.
   - Provide: rule_name, description

h) Grounding Sources - List the code files that support each finding.

Output Language:
The output must be suitable for a non-technical audience.

Exclusions (do NOT cover):
- Modifying pipeline logic or model outputs
- Defining or changing business rules/validations
- Runtime performance or SLAs
- Infrastructure or deployment details

Important:
- DO NOT hallucinate or assume anything
- Every line item in your output must be grounded in actual code
- If a field is received but not used, explicitly state that

Provide the crisp, succinct output in [MODEL_NAME]_analysis.md
```

---

## Placeholder Reference

| Placeholder | Description | Example |
|-------------|-------------|---------|
| `[REPOSITORY_NAME]` | Name of the repository being analyzed | 3p-pipeline |
| `[MODEL_NAME]` | Name of the ML model to analyze | NSFW, Clean, Guardian, MTA Prediction |

---

## Example Usage

### Analyzing Profanity Detection Model in Item Launch Pipeline
```
Role and Goal
As a Senior Consultant Developer, I want you to scan the 3p-pipeline repository for NSFW model occurrences.
...
Provide the crisp, succinct output in NSFW_analysis.md
```

### Analyzing Fraud Detection Model in Payments Service
```
Role and Goal
As a Senior Consultant Developer, I want you to scan the payments-service repository for Fraud Detection model occurrences.
...
Provide the crisp, succinct output in FraudDetection_analysis.md
```

---

## Follow-Up Prompts

After the initial analysis, you may want to ask:

1. **Deep-dive on predictions:**
   > "Expand the analysis on predictedValues in the [MODEL_NAME] response and share whether and how the system uses score and threshold."

2. **Add model description:**
   > "Add a short description of this model before the Request Attributes table."

3. **Compare with another model:**
   > "How does the [MODEL_NAME] integration differ from [OTHER_MODEL_NAME] in terms of request/response handling?"

4. **Error handling:**
   > "What happens when the [MODEL_NAME] model returns an error or times out?"

5. **Data flow:**
   > "Trace the data flow from when [MODEL_NAME] is invoked to when results are persisted."

---

## Expected Output Structure

The generated analysis file should follow this structure:

```markdown
# [MODEL_NAME] Model Analysis – [REPOSITORY_NAME]

## Model Overview
[Plain-language description of the model]

## a) Request Attributes Sent to the Model
| Attribute Name | Required | Description |
|----------------|----------|-------------|
| ... | ... | ... |

## b) Response Attributes Received from the Model
| Attribute Name | Description |
|----------------|-------------|
| ... | ... |

### Expanded: [Key Response Field] Structure
[Detailed breakdown of complex response fields]

### How the System Uses These Fields
| Field | Used? | Explanation |
|-------|-------|-------------|
| ... | ... | ... |

## c) Integration Mechanism
[sync / async / async batch with explanation]

## d) Model Type
[Category of the model]

## e) Invocation Points
| Pipeline Stage | Trigger Condition |
|----------------|-------------------|
| ... | ... |

## f) Business Rules and Validations
| Rule Name | Description |
|-----------|-------------|
| ... | ... |

---

## Grounding Sources (Code References)
- [List of files that support the findings]
```

---

## Tips for Best Results

1. **Be specific about the model name** – Use the exact name as it appears in the codebase (e.g., "NSFW" not "content moderation").

2. **Run iteratively** – Start with the main prompt, then use follow-ups to drill deeper.

3. **Verify grounding** – Always check that findings reference actual code files.

4. **Request clarification on unused fields** – If the model returns data the system doesn't use, it's important to document that explicitly.

5. **Ask about edge cases** – Error handling and fallback behavior are often important but not covered in the initial analysis.
