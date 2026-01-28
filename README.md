# \# RAG Architecture UML Profile (Replication Package)

# 

# This repository contains the \*\*replication package\*\* and \*\*source artifacts\*\* for the paper:

# 

# > \*\*"A UML Profile for Domain-Specific Modelling of Retrieval Augmented Generation (RAG) System Architecture"\*\*

# 

# This artifact provides a domain-specific modelling language (DSML) for RAG systems, implemented as a \*\*UML Profile\*\* in \*\*Eclipse Papyrus\*\*. It includes embedded \*\*Object Constraint Language (OCL)\*\* rules to detect structural integration defects (e.g., dimension mismatches, context overflows) at design time.

# 

# \## 📂 Repository Structure

# 

# \* \*\*`RAG\_UML\_Profile/`\*\*: Contains the source code for the UML Profile.

# \* `\*.profile.uml`: The core metamodel definition and stereotype attributes.

# \* `OCL Constraints`: Embedded validation logic for RAG architectural rules.

# 

# 

# \* \*\*`RAG\_Validation\_Test/`\*\*: Contains the evaluation scenarios described in the paper.

# \* `Scenario A`: Demonstrates "Data Dependency" detection (Dimension Mismatch).

# \* `Scenario C`: Demonstrates "Context Window Overflow" detection.

# \* `Scenario D`: Demonstrates "Interface Protocol Mismatch" detection.

# 

# 

# 

# \## 🛠 Prerequisites

# 

# To run this profile and reproduce the validation results, you need:

# 

# 1\. \*\*Eclipse IDE for Modeling Tools\*\* (Version 2023-09 or later recommended).

# 2\. \*\*Papyrus UML\*\* plugin (usually included in the Modeling package).

# 3\. \*\*OCL (Object Constraint Language)\*\* plugin for Eclipse.

# 

# \## 🚀 How to Reproduce the Results

# 

# \### 1. Installation

# 

# 1\. Clone this repository or download the files.

# 2\. Open Eclipse.

# 3\. Go to `File` -> `Open Projects from File System...`.

# 4\. Select the `RAG\_UML\_Profile` and `RAG\_Validation\_Test` folders.

# 

# \### 2. Loading the Profile

# 

# 1\. Open the `RAG\_Validation\_Test` project in the Project Explorer.

# 2\. Double-click `Component Diagram` to open the canvas.

# 3\. If the stereotypes appear "broken" or missing, re-apply the profile:

# \* Click on the diagram background.

# \* Go to \*\*Properties\*\* -> \*\*Profile\*\*.

# \* Remove the existing profile link and re-add `RAG\_UML\_Profile` using "Map from Workspace".

# 

# 

# 

# \### 3. Running Validation Checks

# 

# To verify the \*\*"Shift-Left"\*\* capabilities described in the paper:

# 

# 1\. Right-click on any component in the diagram (e.g., `JSON\_Prompt` or `OpenAI\_Chunker`).

# 2\. Select \*\*Validation\*\* -> \*\*Validate Model\*\*.

# 3\. Observe the \*\*Model Validation\*\* view at the bottom of the screen.

# 

# \## 📝 Formal Constraint Specification (OCL)

# 

# Per the requirements for formal model verification, the following OCL invariants are implemented in this profile:

# 

# \### \*\*Rule 1: Vector Dimension Compatibility\*\*

# 

# \* \*\*Target:\*\* `<<Chunker>>`

# \* \*\*Description:\*\* Ensures the embedding model's output dimension matches the vector database's index configuration.

# \* \*\*OCL Code:\*\*

# ```ocl

# self.base\_Component.clientDependency.supplier->select(s | s.getAppliedStereotype('RAG\_UML\_Profile::VectorStore') <> null)->forAll(vs |

# &nbsp;   vs.getValue(vs.getAppliedStereotype('RAG\_UML\_Profile::VectorStore'), 'supportedDimension') = self.dimension

# )

# 

# ```

# 

# 

# 

# \### \*\*Rule 2: Interface Protocol Consistency\*\*

# 

# \* \*\*Target:\*\* `<<PromptTemplate>>` (Artifact)

# \* \*\*Description:\*\* Validates that the prompt output format (e.g., JSON) matches the LLM's expected input schema.

# \* \*\*OCL Code:\*\*

# ```ocl

# self.base\_Artifact.clientDependency.supplier->select(s | s.getAppliedStereotype('RAG\_UML\_Profile::LLM\_Reader') <> null)->forAll(llm |

# &nbsp;   llm.getValue(llm.getAppliedStereotype('RAG\_UML\_Profile::LLM\_Reader'), 'expectedInputFormat') = self.outputFormat

# )

# 

# ```

# 

# 

# 

# \### \*\*Rule 3: Context Window Safety\*\*

# 

# \* \*\*Target:\*\* `<<Consolidator>>`

# \* \*\*Description:\*\* Prevents "Silent Truncation" by checking if the consolidated context size exceeds the LLM's hard token limit.

# \* \*\*OCL Code:\*\*

# ```ocl

# self.base\_Component.clientDependency.supplier->select(s | s.getAppliedStereotype('RAG\_UML\_Profile::LLM\_Reader') <> null)->forAll(llm |

# &nbsp;   self.maxContextWindowSize <= llm.getValue(llm.getAppliedStereotype('RAG\_UML\_Profile::LLM\_Reader'), 'contextWindowSize').oclAsType(Integer)

# )

# 

# ```

# 

# 

# 

# \### \*\*Rule 4: Retrieval Volume Validity\*\*

# 

# \* \*\*Target:\*\* `<<Retriever>>`

# \* \*\*Description:\*\* Ensures the `top\_k` parameter is a positive integer to prevent invalid query configurations.

# \* \*\*OCL Code:\*\*

# ```ocl

# self.top\_k >= 1

# 

# ```

# 

# 

# 

# ---

# 

# \## 📄 License

# 

# This artifact is provided under the \*\*MIT License\*\* for academic and research use.

# 

# ---

