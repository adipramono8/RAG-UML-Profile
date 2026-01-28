# RAG Architecture UML Profile (Replication Package)
This repository contains the replication package and source artifacts for the paper: 

**"A UML Profile for Domain-Specific Modelling of Retrieval Augmented Generation (RAG) System Architecture"**

This artifact provides a domain-specific modelling language (DSML) for RAG systems, implemented as a UML Profile in Eclipse Papyrus. It includes embedded Object Constraint Language (OCL) rules to detect structural integration defects (e.g., dimension mismatches, context overflows) at design time.

## 📂 Repository Structure
`RAG_UML_Profile/`: Contains the source code for the UML Profile.

`*.profile.uml`: The core metamodel definition and stereotype attributes.

`OCL Constraints`: Embedded validation logic for RAG architectural rules.

`RAG_Validation_Test/`: Contains the evaluation scenarios described in the paper.

### Testing Scenario
`Scenario A`: Demonstrates "Data Dependency" detection (Dimension Mismatch).

`Scenario B`: Demonstrates "Context Window Overflow" detection.

`Scenario C`: Demonstrates "Interface Protocol Mismatch" detection.

## 🛠 Prerequisites
To run this profile and reproduce the validation results, you need:

1. Eclipse IDE for Modeling Tools (Version 2023-09 or later recommended).

2. Papyrus UML plugin (usually included in the Modeling package).

3. OCL (Object Constraint Language) plugin for Eclipse.

## 🚀 How to Reproduce the Results
### 1. Installation
- Clone this repository or download the files.

- Open Eclipse.

- Go to File -> Open Projects from File System...

- Select the RAG_UML_Profile and RAG_Validation_Test folders.

### 2. Loading the Profile
-  Open the RAG_Validation_Test project in the Project Explorer.

- Double-click Component Diagram to open the canvas.

- If the stereotypes appear "broken" or missing, re-apply the profile:

- Click on the diagram background.

- Go to Properties -> Profile.

- Remove the existing profile link and re-add RAG_UML_Profile using "Map from Workspace".

### 3. Running Validation Checks
- Right-click on any component in the diagram (e.g., JSON_Prompt or OpenAI_Chunker).

- Select Validation -> Validate Model.

- Observe the Model Validation view at the bottom of the screen.

## 📝 Formal Constraint Specification (OCL)
Per the requirements for formal model verification, the following OCL invariants are implemented in this profile:

### Rule 1: Vector Dimension Compatibility
#### Target: Chunker

Description: Ensures the embedding model's output dimension matches the vector database's index configuration.

*OCL Code*:
```
self.base_Component.clientDependency.supplier->select(s | s.getAppliedStereotype('RAG_UML_Profile::VectorStore') <> null)->forAll(vs |
    vs.getValue(vs.getAppliedStereotype('RAG_UML_Profile::VectorStore'), 'supportedDimension') = self.dimension
)
```
### Rule 2: Interface Protocol Consistency
#### Target: PromptTemplate (Artifact)

Description: Validates that the prompt output format (e.g., JSON) matches the LLM's expected input schema.

*OCL Code*:

```
self.base_Artifact.clientDependency.supplier->select(s | s.getAppliedStereotype('RAG_UML_Profile::LLM_Reader') <> null)->forAll(llm |
    llm.getValue(llm.getAppliedStereotype('RAG_UML_Profile::LLM_Reader'), 'expectedInputFormat') = self.outputFormat
)
```

### Rule 3: Context Window Safety
#### Target: Consolidator

Description: Prevents "Silent Truncation" by checking if the consolidated context size exceeds the LLM's hard token limit.

*OCL Code*:

```
self.base_Component.clientDependency.supplier->select(s | s.getAppliedStereotype('RAG_UML_Profile::LLM_Reader') <> null)->forAll(llm |
    self.maxContextWindowSize <= llm.getValue(llm.getAppliedStereotype('RAG_UML_Profile::LLM_Reader'), 'contextWindowSize').oclAsType(Integer)
)
```

### Rule 4: Retrieval Volume Validity
#### Target: Retriever

Description: Ensures the top_k parameter is a positive integer to prevent invalid query configurations.

*OCL Code*:
```
self.top_k >= 1
```

## 📄 License
This artifact is provided under the MIT License for academic and research use.