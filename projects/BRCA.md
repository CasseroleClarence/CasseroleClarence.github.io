BRCA Analysis Study
Background and Overview
This study investigates the genetic and clinical characteristics of breast cancer, focusing on mutations within BRCA genes. Utilizing data from The Cancer Genome Atlas (TCGA), the research employs unsupervised learning to identify highly transcribed genes and to analyze factors affecting patient survival outcomes. Key insights are gained into pathways critical for tumor growth, such as cell cycle regulation and NK cell-mediated cytotoxicity. This page outlines the study's findings, methodology, and clinical implications for targeted breast cancer therapies.

<div style="display: flex; justify-content: center; align-items: center;"> <img src="/assets/img/BRCA/image1.png" alt="BRCA study overview" style="width: 90%; height: auto;"> </div>

1.0 Study Objectives
The primary aim of this study was to analyze BRCA gene alterations in breast cancer patients to:

- Identify frequently mutated genes.
- Examine the impact of these genes on biochemical pathways.
- Determine patient demographic characteristics influencing survival.
- Explore possible therapeutic targets through pathway dysregulation insights.
Key Pathways Analyzed
- Cell Cycle Regulation: Critical in tumor growth and progression.
- Natural Killer (NK) Cell Cytotoxicity: Known to play a role in the body’s immune response to cancer.
- Antigen Processing and Presentation: Important for immune response activation and therapy response.
2.0 Methodology
Data Collection and Preprocessing
Three TCGA datasets (mutation, clinical, and RNA sequencing data) were utilized, with preprocessing to filter for high-impact mutations. Data analysis steps included:

- Kaplan-Meier Survival Analysis: To assess survival based on patient demographics.
- Mutation Analysis: Examining mutation frequencies, oncoplot generation, and clustering.
- Differential Expression Analysis: Using RNA sequencing data to detect key gene expressions across clusters.

Clustering and Pathway Analysis
Data was clustered into four groups based on mutation profiles. The clusters were compared for demographic, clinical, and pathway characteristics, with differential pathway expression mapped to identify distinct patterns across clusters.

<div style="display: flex; justify-content: center; align-items: center;"> <img src="/assets/img/BRCA/cluster-analysis.png" alt="Cluster Analysis" style="width: 90%; height: auto;"> </div>
3.0 Results
3.1 Clinical Summary
- Patient Demographics: Most patients were White females, with variations across subtypes.
- Survival Outcomes: Differences in survival among subtypes were significant, particularly in African American patients with the Basal subtype.
3.2 Mutation Analysis
Frequent mutations were found in the PIK3CA, TP53, and TTN genes, with missense mutations most common. An oncoplot highlighted four mutation-based clusters, showing gene variation patterns among patient subgroups.

3.3 Pathway Findings
Top pathways affected by gene mutations included:

- Cell Cycle: Downregulated in certain clusters, reflecting impact on cancer progression.
- NK Cell Cytotoxicity: Differentially expressed in clusters, particularly relevant for immunotherapy approaches.
- Antigen Processing: Linked to immune response and potential therapy targets.
4.0 Discussion
- Demographic Patterns: Differences in BRCA subtypes across ethnic groups align with existing literature.
- Therapeutic Implications: Pathways such as NK cytotoxicity and cell adhesion molecules are potential therapeutic targets, with differential expression suggesting varied responses based on subtype.
5.0 Limitations
The study's hierarchical clustering method faced challenges due to overlapping gene activity across breast cancer subtypes. Improved clustering techniques are recommended for future studies to enhance data segmentation and pathway mapping.

6.0 Conclusion
This study provides valuable insights into the genetic landscape of breast cancer, identifying critical pathways for therapeutic intervention. Future research should focus on refining clustering methods and exploring targeted therapies based on pathway dysregulation.

<div style="display: flex; justify-content: center; align-items: center;"> <img src="/assets/img/BRCA/conclusion.png" alt="Study Conclusion" style="width: 90%; height: auto;"> </div>
Contributions
Clinical Analysis: Elena Kovacevic
Mutation and Gene Expression Analysis: Nick Santoso
Report Sections: Introduction by Amine Benhammou, Methods and Results by Nick, and Discussion by Amine and Elena.
References
A comprehensive list of all academic references used in this study.