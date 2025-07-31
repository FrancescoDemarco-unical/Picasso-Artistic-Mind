## Project Overview

---

# Capturing Picasso’s Artistic Mind: Mathematical Modeling of Cognitive Processes

## Description

This project adopts a computational approach to analyze Pablo Picasso's artistic work by extracting visual metrics, employing clustering techniques, and conducting scientific analysis.

The goal is to identify recurring patterns, complex structures, and potential visual attractors within the metric space.

## Main Objectives

- Data cleaning and management
- Calculation of visual metrics
- Clustering based on these metrics
- Identification of attractors using Mean-Shift
- Reduced visualization with UMAP and t-SNE
- Dynamic exploration through bifurcations

## Calculated Metrics

1. **Fractal dimension**
2. **Entropy**
3. **Birkhoff measure**
4. **Euclidean distance**

## Analysis Phases

1. Counting artworks (downloaded images + elements in JSON)
   *(Dataset: [Google Drive link](https://drive.google.com/drive/u/1/folders/1oBE6t9TOheAfzASu3EZP70e6tsHTNr3I))*
2. Organization by style, according to JSON metadata
3. Verification and counting for each stylistic category
4. Removal of folders with fewer than 10 artworks (not significant)
5. New artwork count post-cleaning
6. Calculation of visual metrics

## Analyses Performed

- ANOVA and MANOVA
- Clustering with Mean-Shift (bandwidth = 0.25 × median)
- Kernel Distribution (univariate and bivariate)
- Contextual Fit
- Hausdorff Measure
- Aggregate statistics per cluster (means per metric)
- Comparative Histograms
- Dimensionality Reduction (UMAP and t-SNE)
- Detection of possible visual attractors
- Social interactions of Picasso's artworks

## Included Visualizations

- Histograms by cluster and metric
- Dendrogram
- Word cloud
- 2D and 3D scatter plots
- 2D projections (UMAP and t-SNE)
- Bifurcation diagrams (adapted to the visual domain)

---

## Section 1: Artwork Counting and Preliminary Data

* **Purpose:** To establish the total number of available artworks and metadata for analysis.
* **Input:** Image files (`.jpg`, `.jpeg`, `.png`) from various subfolders and a JSON file containing metadata (`pablo-picasso.json`).
* **Methods:** Recursive directory scanning for image counting; loading and inspecting the JSON file for element counting.
* **Output:** Total number of images counted in the folders and number of elements/records in the JSON file.
* **Insights:** A total of 1170 artworks (images) confirmed. The JSON file count indicates the size of the associated metadata dataset, essential for future analyses.
* **Key Visualizations:** No direct visualizations, numerical outputs.

---

## Section 2: Data Normalization and Organization

* **Purpose:** To clean and standardize artwork metadata, resolving homonymy issues and categorizing artworks by stylistic period.
* **Input:** JSON file with artwork metadata (`pablo-picasso.json`) and directories containing artwork images.
* **Methods:**
    * **"Untitled" Renaming:** Scanning the JSON for artworks titled "Untitled" and assigning a progressive numeric suffix (e.g., "Untitled_1").
    * **Organization by Period:** Creating a new directory structure (`pablo-picasso-period`) where artworks are copied into subfolders based on their stylistic "period." Artworks without a defined "period" are removed.
    * **Duplicate Title Numbering:** Identifying and renaming artworks with identical titles (case-insensitive) by adding a numeric suffix (e.g., "Les Demoiselles d'Avignon_1"). The first occurrence retains its original title.
* **Output:** Updated JSON file with normalized titles; new image directories organized by stylistic period.
* **Insights:** Normalizing "Untitled" titles and removing incomplete records improve dataset consistency. Organization by period facilitates future stylistic analyses and corpus navigation. Numbering duplicate titles ensures unique identification for each artwork.
* **Key Visualizations:** No direct visualizations; the output is a file structure and a modified JSON file.

---

## Section 3: Verification and Consolidation of Organized Artworks

* **Purpose:** To confirm the outcome of cleaning and organization operations, re-checking the total number of artworks after categorization by period.
* **Input:** The new period-organized folder (`.\\DATA\\pablo-picasso-period`) containing artwork images.
* **Methods:** Recursive scanning of the `pablo-picasso-period` directory to count the total number of image files present.
* **Output:** The final count of artworks organized by period.
* **Insights:** The final count of **1105 artworks** confirms that artworks without a defined "period" have been removed and that the organization process was correctly executed. This consolidated dataset is now ready for subsequent analysis phases.
* **Key Visualizations:** No direct visualizations; the output is a numerical count.

---

## Section 4: Image Metric Calculation

* **Purpose:** To quantify the visual characteristics of each artwork using mathematical metrics, preparing them for stylistic and comparative analyses.
* **Input:** Artworks images organized by period (`.\\DATA\\pablo-picasso-period`).
* **Methods:** For each image, converted to grayscale, the following metrics are calculated:
    * **Fractal Dimension (Box-Counting):** Measures the complexity and roughness of the image's contour.
    * **Shannon Entropy:** Quantifies the amount of information or randomness of pixel distribution.
    * **Birkhoff Measure:** Calculated as the ratio of the total perimeter to the square root of the total area of a binary shape derived from the image. It indicates contour complexity relative to object size.
    * **Euclidean Norm (Frobenius):** Measures the "magnitude" or overall intensity of the image, normalized between 0 and 1.
* **Output:** A CSV file (`.\\CSV\\metrics.csv`) containing for each artwork its period, name, and the calculated values of the four metrics.
* **Insights:** These metrics provide a numerical representation of Picasso's painting styles. For example, a high fractal dimension might indicate artworks with complex details and irregular shapes (e.g., cubism), while entropy could reveal tonal variability. The Birkhoff measure and Euclidean Norm offer further angles on structural complexity and visual intensity of compositions. These quantitative data are fundamental for an objective analysis of stylistic evolutions.
* **Key Visualizations:** No direct visualizations at this stage; the output is a tabular dataset.

---

## Section 5: Integration and Cleaning of Additional Data

* **Purpose:** To enrich the metrics dataset with temporal and stylistic information and remove any incomplete records.
* **Input:** CSV file with calculated metrics (`.\\CSV\\metrics.csv`) and the original metadata JSON file (`.\\DATA\\pablo-picasso.json`).
* **Methods:**
    * **Data Merging:** The 'Year', 'Period', and 'Style' columns are extracted from the JSON and added to the CSV, using the artwork name as the matching key. Normalization is applied to artwork names to ensure robust matching (removal of special characters, conversion to lowercase).
    * **Missing Value Handling:** All rows in the resulting dataset with null values in the 'Year', 'Period', or 'Style' columns are removed to ensure data integrity for subsequent analyses.
* **Output:** A new CSV file (`.\\CSV\\metrics_with_details.csv`) containing the image metrics enriched with the completion year, period, and style of each artwork, with all incomplete rows removed.
* **Insights:** Adding the year and period is crucial for conducting temporal analyses of Picasso's style evolution. Removing rows with null values ensures that subsequent analyses (e.g., predictive modeling or statistical analyses) are based on a clean and complete dataset, improving result reliability.
* **Key Visualizations:** No direct visualizations; the output is an updated tabular dataset.

---

## Section 6: Outlier Removal and Post-Cleaning Count

* **Purpose:** To identify and eliminate anomalous values (outliers) from the calculated metrics, ensuring that subsequent analyses are based on robust and representative data. Subsequently, artworks are recounted to confirm the final dataset.
* **Input:** The CSV file containing artwork metrics and details (`.\\CSV\\metrics_with_details.csv`).
* **Methods:**
    * **Data Loading and Preparation:** The CSV is loaded, and numerical columns (`Fractal dimension`, `Entropy`, `Birkhoff measure`, `Euclidean norm`) are converted to numeric type, handling any conversion errors and removing rows with NaN values in these columns.
    * **IQR Outlier Removal:** For each stylistic "Period" and for each numerical metric, the First Quartile (Q1), Third Quartile (Q3), and Interquartile Range (IQR) are calculated. Artworks whose values fall outside the range $[Q1 - 1.5 \times IQR, Q3 + 1.5 \times IQR]$ are considered outliers and removed. This process is iterative and can be repeated until no more visible outliers exist.
    * **Outlier Visualization:** Box plots are generated for each metric, grouped by period, to visualize data distribution and outlier removal effectiveness. These plots are saved in the `.\\PLOT\\BOXPLOT\\` directory.
    * **Final Count:** After outlier removal, the total number of artworks and the count of artworks for each stylistic period are recalculated.
* **Output:** A new CSV file (`.\\CSV\\metrics_with_details_no_outlier.csv`) containing the outlier-cleaned dataset. Several visualizations (box plots) are produced showing the distribution of metrics by period, without outliers. A summary of artwork counts for each period after cleaning is also provided.
* **Insights:** Outlier removal is fundamental to prevent extreme values from distorting statistical analyses and predictive models. The post-cleaning count indicates that the final dataset comprises **852 artworks**, with a distribution by period that now more accurately reflects the central corpus of Picasso's works after anomalous data removal. Box plot visualizations confirm that the data is now more concentrated and ready for meaningful comparative analyses between different periods.
* **Key Visualizations:** Box plots of "Fractal dimension," "Entropy," "Birkhoff measure," and "Euclidean norm" grouped by "Period," showing distributions after outlier removal.

---

## Section 7: Metric Normalization

* **Purpose:** To bring all calculated metrics to a common scale (between 0 and 1) to eliminate differences in magnitude between them, making them comparable and suitable for algorithms sensitive to data scale (e.g., many machine learning algorithms).
* **Input:** The outlier-cleaned metric CSV file (`.\\CSV\\metrics_with_details_no_outlier.csv`).
* **Methods:**
    * **Data Loading:** The dataset is loaded.
    * **Checking and Cleaning:** Metric columns ("Fractal dimension", "Entropy", "Birkhoff measure", "Euclidean distance") are explicitly converted to numeric type. Any non-numeric values (resulting from errors or previous conversions) are transformed to `NaN`, and rows containing these `NaN`s are removed from the dataset.
    * **Min-Max Normalization:** Scikit-learn's **MinMaxScaler** algorithm is applied, scaling each metric's values so that the minimum value becomes 0 and the maximum value becomes 1, while preserving relative relationships between data.
* **Output:** A new CSV file (`.\\CSV NORMALIZED\\normalized_metrics_with_details_no_outlier.csv`) where all metric columns have been normalized to the [0, 1] range.
* **Insights:** Normalization is a crucial pre-processing step that improves the performance of many analytical and machine learning algorithms, ensuring that no metric dominates the analysis due to its intrinsic scale. This makes the data ready for tasks such as clustering, classification, or regression analysis, where comparability between features is fundamental.
* **Key Visualizations:** No direct visualizations; the output is a tabular dataset with scaled values.

---

## Section 8: Normalized Statistical Analysis by Period

* **Purpose:** To examine statistical differences in normalized metrics among Picasso's different artistic periods, identifying which metrics show significant variations over time and between consecutive periods.
* **Input:** The CSV file with normalized metrics and artwork details (`.\\CSV NORMALIZED\\normalized_metrics_with_details_no_outlier.csv`).
* **Methods:**
    * **Calculation of Mean and Standard Deviation:** For each period ("Early Years", "Blue Period", "Rose Period", "African Period", "Cubist Period", "Neoclassicist & Surrealist Period", "Later Years"), the mean and standard deviation are calculated for each of the four metrics ("Fractal dimension", "Entropy", "Birkhoff measure", "Euclidean distance"). Results are saved in a CSV (`.\\CSV NORMALIZED\\normalized_metrics_by_period.csv`).
    * **MANOVA (Multivariate Analysis of Variance):** A MANOVA is performed to simultaneously compare the means of all metrics between chronologically consecutive pairs of periods. MANOVA determines if statistically significant differences exist between groups when considering multiple dependent variables together.
    * **Univariate ANOVA (post-hoc) Analysis:** If MANOVA indicates a significant difference (p-value < 0.05), a univariate ANOVA is conducted for each metric individually, again between consecutive period pairs. This helps identify which specific metrics contribute to the overall significance found with MANOVA.
    * **Trend Visualization:** Line graphs are created showing the trend of each metric's mean across chronologically ordered periods. These graphs are saved as PNG images in the `.\\PLOT NORMALIZED\\TREND\\` folder.
* **Output:**
    * A CSV file (`.\\CSV NORMALIZED\\normalized_metrics_by_period.csv`) with the means and standard deviations of metrics for each period.
    * A CSV file (`.\\CSV NORMALIZED\\normalized_manova_analysis_by_period.csv`) summarizing MANOVA p-values for each pair of consecutive periods and indicating which metrics were significantly different in the univariate ANOVA.
    * PNG images (`.\\PLOT NORMALIZED\\TREND\\normalized_metrics_trend.png`) showing metric trends over time.
* **Insights:** Statistical analysis will reveal if and how the visual characteristics of Picasso's works (measured by metrics) significantly changed from one period to another. Trend graphs will visualize these evolutions. MANOVA and subsequent ANOVAs will provide a statistical basis for asserting that transitions between periods are not just nominal but reflect quantifiable changes in the visual style of the works. This step is crucial for understanding Picasso's artistic evolution from a data-driven perspective.

---

## Section 8.1: Statistical Analysis with Respect to Period (Normalized Data)

* **Purpose:** To deepen the analysis of metrics extracted from Picasso's works, focusing on statistical differences among various artistic periods, using the **normalized** data obtained in the previous step. The goal is to quantify and visualize the evolution of Picasso's style throughout his career phases.
* **Input:** The CSV file containing normalized metrics and artwork details (`.\\CSV NORMALIZED\\normalized_metrics_with_details_no_outlier.csv`).
* **Methods:**
    * **Calculation of Mean and Standard Deviation by Period:** For each defined artistic period ("Early Years", "Blue Period", "Rose Period", "African Period", "Cubist Period", "Neoclassicist & Surrealist Period", "Later Years"), the mean and standard deviation are calculated for each of the four metrics ("Fractal dimension", "Entropy", "Birkhoff measure", "Euclidean distance"). This provides an initial overview of central tendencies and metric dispersion within each phase. Results are saved in a CSV (`.\\CSV NORMALIZED\\normalized_metrics_by_period.csv`).
    * **MANOVA (Multivariate Analysis of Variance):** A MANOVA is conducted to simultaneously compare the means of all metrics between chronologically consecutive pairs of periods in Picasso's artistic timeline. MANOVA is used to determine if there are statistically significant differences between groups (periods) when considering multiple dependent variables (metrics) simultaneously. This helps understand if the evolution between periods has a collective impact on artwork characteristics.
    * **Univariate ANOVA (Post-Hoc) Analysis:** If MANOVA reveals an overall significant difference between two consecutive periods (p-value < 0.05), univariate ANOVA analyses are performed for each metric individually. This post-hoc step serves to identify which specific metrics contribute most to the overall significance observed by MANOVA, indicating which aspects of Picasso's style changed most markedly between periods.
    * **Visualization of Mean Trends:** Line graphs are generated illustrating the trend of each metric's mean across artistic periods, ordered chronologically. These visualizations allow for immediate understanding of Picasso's stylistic variations and evolutions over time, making quantifiable changes in his works' properties evident.
* **Output:**
    * A CSV file (`.\\CSV NORMALIZED\\normalized_metrics_by_period.csv`) containing the means and standard deviations of metrics for each artistic period.
    * A CSV file (`.\\CSV NORMALIZED\\normalized_manova_analysis_by_period.csv`) summarizing MANOVA p-values for each pair of consecutive periods and indicating which metrics were significantly different in the univariate ANOVA.
    * PNG images (`.\\PLOT NORMALIZED\\TREND\\normalized_metrics_trend.png`) showing metric trends over time.
* **Insights:** This phase of statistical analysis is **fundamental** for quantitatively validating the transitions between Picasso's artistic periods. Results will indicate not only if there were significant differences in style, but also which specific metrics (and thus which visual properties) were most influential in these changes. Trend graphs will provide an intuitive representation of these evolutions, complementing numerical analysis with visual context.

---

## Section 8.2: Analysis of Mean Change in Metrics

* **Purpose:** To quantify the magnitude of changes in Picasso's visual artwork metrics between consecutive artistic periods, providing a direct measure of stylistic evolution.
* **Input:** The CSV file containing the means of metrics for each period (`.\\CSV\\metrics_by_period.csv`).
* **Methods:**
    * **Data Loading and Preparation:** The dataset of period means is loaded. Numeric columns are converted to float format, handling commas as decimal separators.
    * **Chronological Ordering:** Picasso's artistic periods are ordered chronologically ("Early Years", "Blue Period", "Rose Period", "African Period", "Cubist Period", "Neoclassicist & Surrealist Period", "Later Years").
    * **Calculation of Consecutive Differences:** For each metric (Fractal Dimension, Entropy, Birkhoff Measure, Euclidean Norm), the difference between the mean of the next period and the mean of the current period is calculated. This highlights the increase or decrease of each visual characteristic when transitioning from one artistic phase to another.
* **Output:** A new CSV file (`.\\CSV\\metric_variations_by_period.csv`) listing each metric, the transition between periods (e.g., "Early Years to Blue Period") and the value of the observed mean difference.
* **Insights:** This analysis provides a **direct and quantifiable measure** of Picasso's stylistic evolution. For example, a significant positive difference in "Fractal Dimension" between the Blue Period and Cubism would indicate an increase in complexity and fragmentation of forms. These data are essential for understanding not only *if* the style changed, but also *how* and *to what extent* each visual characteristic changed throughout his career.
* **Key Visualizations:** No direct visualizations at this stage; the output is a tabular dataset quantifying the changes.

---

## Section 9: 3D Visualization and Clustering (Normalized Data)

* **Purpose:** To visualize the relationships between Picasso's different artistic periods in the normalized metric space and identify stylistic groupings through hierarchical clustering analysis. This helps understand stylistic evolution from a multidimensional perspective.
* **Input:** The CSV file containing normalized metrics and artwork details (`.\\CSV NORMALIZED\\normalized_metrics_with_details_no_outlier.csv`).
* **Methods:**
    * **Data Loading and Preparation:** The normalized dataset is loaded. Ensure the 'Period' column is categorical and chronologically ordered. Metric columns are converted to numeric, and rows with missing values in metrics or period are removed.
    * **Calculate Means by Period:** Means of all metrics are calculated for each artistic period.
    * **Dendrogram (Hierarchical Clustering):**
        * **Hierarchical clustering** ('ward' method) is applied using the means of 'Fractal Dimension' and 'Entropy' for each period.
        * A **dendrogram** is generated visualizing the similarity relationships between periods. An optional horizontal threshold line can help identify potential clusters.
        * The dendrogram is saved as a PNG image (`.\\PLOT NORMALIZED\\DENDOGRAM\\dendrogram_fractal_entropy.png`).
    * **3D Scatter Plots with Chronological Trajectory:**
        * For all possible combinations of three metrics (e.g., (Fractal Dimension, Entropy, Birkhoff Measure), (Entropy, Birkhoff Measure, Euclidean Norm), etc.), 3D scatter plots are created.
        * Each point in the plot represents an artistic period, positioned based on the mean values of the three selected metrics.
        * **Chronological arrows** are added connecting successive period points, visualizing Picasso's "evolutionary path" in the metric space.
        * Each 3D plot is saved as a PNG image (`.\\PLOT NORMALIZED\\3D DISPLAY\\Evolutionary Path of Picasso's Styles (Space of ...).png`).
* **Output:**
    * A PNG image of the dendrogram (`.\\PLOT NORMALIZED\\DENDOGRAM\\dendrogram_fractal_entropy.png`).
    * Several PNG images of 3D scatter plots, one for each combination of three metrics, showing the chronological trajectory of periods in the three-dimensional metric space (`.\\PLOT NORMALIZED\\3D DISPLAY\\...png`).
* **Insights:** The **dendrogram** will help identify which stylistic periods are most similar to each other in terms of visual complexity and randomness, suggesting possible natural groupings in Picasso's style. The **3D plots** will offer an intuitive and dynamic representation of his style's evolution, showing how artworks shifted in the visual feature space throughout his career. Chronological trajectories will highlight the main directions of stylistic change.

---

## Section 9.1: 3D Visualization and Clustering (Unnormalized Data)

* **Purpose:** To visualize the relationships between Picasso's different artistic periods in the **unnormalized** metric space and identify stylistic groupings through hierarchical clustering analysis. This serves as a comparison with the analysis on normalized data, to observe the impact of metric scale on relationships.
* **Input:** The CSV file containing original (unnormalized) metrics and artwork details (`.\\CSV\\metrics_with_details_no_outlier.csv`).
* **Methods:**
    * **Data Loading and Preparation:** The dataset is loaded. Ensure the 'Period' column is categorical and chronologically ordered. Metric columns are converted to numeric, and rows with missing values in metrics or period are removed.
    * **Calculate Means by Period:** Means of all metrics are calculated for each artistic period.
    * **Dendrogram (Hierarchical Clustering):**
        * **Hierarchical clustering** ('ward' method) is applied using the means of 'Fractal Dimension' and 'Entropy' for each period.
        * A **dendrogram** is generated visualizing the similarity relationships between periods. An example horizontal threshold line can help identify potential clusters.
        * The dendrogram is saved as a PNG image (`.\\PLOT\\DENDOGRAM\\dendrogram_fractal_entropy.png`).
    * **3D Scatter Plots with Chronological Trajectory:**
        * For all possible combinations of three metrics, 3D scatter plots are created.
        * Each point in the plot represents an artistic period, positioned based on the mean values of the three selected metrics.
        * **Chronological arrows** are added connecting successive period points, visualizing Picasso's "evolutionary path" in the metric space.
        * Each 3D plot is saved as a PNG image (`.\\PLOT\\3D DISPLAY\\Evolutionary Path of Picasso's Styles (Space of ...).png`).
* **Output:**
    * A PNG image of the dendrogram (`.\\PLOT\\DENDOGRAM\\dendrogram_fractal_entropy.png`).
    * Several PNG images of 3D scatter plots, one for each combination of three metrics, showing the chronological trajectory of periods in the three-dimensional metric space (`.\\PLOT\\3D DISPLAY\\...png`).
* **Insights:** This section replicates the analysis of Section 9 using unnormalized data. The comparison between the results of the two steps (9 and 9.1) is crucial: the dendrogram and 3D plots on unnormalized data might show **different groupings and trajectories** compared to those obtained with normalized data. This will highlight how metric scale can influence the interpretation of stylistic relationships and the importance of normalization for scale-sensitive algorithms.

---

## Section 10: Box Plot Visualization (Normalized Data)

* **Purpose:** To visualize the distribution of key metrics (mean ± standard deviation) for consecutive artistic periods of Picasso, using **normalized** data. This helps understand stylistic overlap and separation between the phases of his career.
* **Input:** The CSV file containing the means and standard deviations of normalized metrics for each period (`.\\CSV NORMALIZED\\normalized_metrics_by_period.csv`).
* **Methods:**
    * **Data Loading:** The dataset of period means and standard deviations is loaded.
    * **Generate 2D Box Plots (with standard deviation rectangles):** A series of 2D plots are created, each focusing on two consecutive periods and a specific pair of normalized metrics.
        * For each period, a rectangle is drawn centered on the mean of the two metrics and with sides equal to twice the standard deviation of each metric (mean ± standard deviation). This rectangle represents the most common variability range of artworks in that period with respect to those metrics.
        * A point at the center of the rectangle indicates the exact mean.
        * The analyzed periods and visualized metrics are:
            * **Early Years vs Blue Period:** Entropy (X-axis) and Euclidean Distance (Y-axis).
            * **Blue Period vs Rose Period:** Fractal Dimension (X-axis) and Euclidean Distance (Y-axis).
            * **Rose Period vs African Period:** Fractal Dimension (X-axis) and Euclidean Distance (Y-axis).
            * **African Period vs Cubist Period:** Fractal Dimension (X-axis) and Euclidean Distance (Y-axis).
            * **Cubist Period vs Neoclassicist & Surrealist Period:** Birkhoff Measure (X-axis) and Euclidean Distance (Y-axis).
            * **Neoclassicist & Surrealist Period vs Later Years:** Entropy (X-axis) and Fractal Dimension (Y-axis).
        * Each plot is saved as a PNG image in the `.\\PLOT NORMALIZED\\BOX DISPLAY\\` directory.
* **Output:** Several PNG image files (`.\\PLOT NORMALIZED\\BOX DISPLAY\\... .png`), each depicting standard deviation rectangles for a pair of consecutive periods, visualizing their overlap or separation in the selected metric space.
* **Insights:** This visualization is fundamental for understanding the **distinction and overlap** between Picasso's different stylistic periods in quantitative terms. Overlapping rectangles suggest continuity or similarity between styles, while separated ones indicate sharper, more distinctive transitions. The size of the rectangle (determined by the standard deviation) also provides an indication of the **stylistic variability** within each period. Using normalized data ensures that the comparison is not influenced by different metric scales.

---

## Section 10.1: Box Plot Visualization (Unnormalized Data)

* **Purpose:** To visualize the distribution of key metrics (mean ± standard deviation) for consecutive artistic periods of Picasso, using **unnormalized** data. This section is crucial for comparing the impact of normalization on the interpretation of stylistic relationships.
* **Input:** The CSV file containing the means and standard deviations of original (unnormalized) metrics for each period (`.\\CSV\\metrics_by_period.csv`).
* **Methods:**
    * **Data Loading:** The dataset of period means and standard deviations is loaded.
    * **Generate 2D Box Plots (with standard deviation rectangles):** A series of 2D plots are created, each focusing on two consecutive periods and a specific pair of unnormalized metrics.
        * For each period, a rectangle is drawn centered on the mean of the two metrics and with sides equal to twice the standard deviation of each metric (mean ± standard deviation). This rectangle visualizes the most common variability range of artworks in that period.
        * A point at the center of the rectangle indicates the exact mean.
        * The analyzed periods and visualized metrics are:
            * **Early Years vs Blue Period:** Euclidean Distance (X-axis) and Entropy (Y-axis).
            * **Blue Period vs Rose Period:** Fractal Dimension (X-axis) and Euclidean Distance (Y-axis).
            * **Rose Period vs African Period:** Fractal Dimension (X-axis) and Euclidean Distance (Y-axis).
            * **African Period vs Cubist Period:** Fractal Dimension (X-axis) and Euclidean Distance (Y-axis).
            * **Cubist Period vs Neoclassicist & Surrealist Period:** Birkhoff Measure (X-axis) and Euclidean Distance (Y-axis).
            * **Neoclassicist & Surrealist Period vs Later Years:** Entropy (X-axis) and Fractal Dimension (Y-axis).
        * Each plot is saved as a PNG image in the `.\\PLOT\\BOX DISPLAY\\` directory (different from normalized data) with "(Unnormalized Data)" in the title and filename for clarity.
* **Output:** Several PNG image files (`.\\PLOT\\BOX DISPLAY\\... (Unnormalized).png`), each depicting standard deviation rectangles for a pair of consecutive periods, based on unnormalized data.
* **Insights:** This section allows for a **direct comparison** with Section 10. The analysis of box plots with unnormalized data will reveal how differences in metric scales can alter the appearance of distributions and overlaps between periods. It is expected that magnitude differences between metrics may make some separations more or less evident, emphasizing the importance of data preprocessing (normalization) for robust and interpretable clustering results, especially when metrics have different scales.

---

## Section 11: K-Means Clustering (Normalized Data)

* **Purpose:** To apply the K-Means clustering algorithm to normalized visual metric data to identify natural groupings in Picasso's artworks, based on their intrinsic characteristics rather than chronological order. 7 clusters are used, consistent with the number of defined stylistic periods.
* **Input:** The CSV file containing normalized metrics for each artwork (`.\\CSV NORMALIZED\\normalized_metrics_with_details_no_outlier.csv`).
* **Methods:**
    * **Data Loading:** The normalized dataset is loaded, ensuring decimals are interpreted correctly.
    * **Configuration:** The four metrics ('Fractal dimension', 'Entropy', 'Birkhoff measure', 'Euclidean distance') and the number of clusters ($k=7$) are defined.
    * **Iteration over Metric Pairs:** The K-Means algorithm is executed for each unique combination of two metrics.
        * For each metric pair, data is selected, and the K-Means algorithm is initialized and trained. `random_state=42` ensures reproducibility of results.
        * Cluster labels assigned to each data point (artwork) are added to the DataFrame.
        * Cluster centroids are calculated and printed, providing the mean coordinates of each group in the metric space.
    * **Cluster Visualization:** For each analyzed metric pair, a scatter plot is generated:
        * Points represent individual artworks, colored according to the K-Means assigned cluster.
        * Cluster centroids are visualized as red and black 'X' markers, highlighting the "center" of each group.
        * The plot title indicates the metrics used and the number of clusters.
        * Each plot is saved as a PNG image in the `.\\PLOT NORMALIZED\\CLUSTER\\` directory for subsequent review and analysis.
* **Output:**
    * Printout of centroids for each analyzed metric pair.
    * Several PNG image files (`.\\PLOT NORMALIZED\\CLUSTER\\KMeans_Cluster_... .png`), each showing the distribution of K-Means clusters for a specific pair of normalized metrics.
    * The original DataFrame enriched with new columns indicating cluster assignment for each artwork in relation to each considered metric pair.
* **Insights:** This section aims to discover if *emergent* stylistic groupings exist from the intrinsic properties of artworks, independent of historical period classification. By comparing these clusters with predefined periods, the consistency of metrics in capturing stylistic transitions can be evaluated. Plots will visually show how artworks group based on their quantifiable characteristics, and centroid positions will indicate the average characteristics of each stylistic cluster identified by the algorithm.

---

## Section 11.1: K-Means Clustering (Unnormalized Data)

* **Purpose:** To apply the K-Means clustering algorithm to **unnormalized** visual metric data to identify natural groupings in Picasso's artworks. This analysis serves as a direct comparison with Section 11 to evaluate the impact of normalization on clustering results. $k=7$ clusters are maintained for consistency.
* **Input:** The CSV file containing original (unnormalized) metrics for each artwork (`.\\CSV\\metrics_with_details_no_outlier.csv`).
* **Methods:**
    * **Data Loading:** The unnormalized dataset is loaded, ensuring decimals are interpreted correctly.
    * **Configuration:** The four metrics ('Fractal dimension', 'Entropy', 'Birkhoff measure', 'Euclidean distance') and the number of clusters ($k=7$) are defined.
    * **Iteration over Metric Pairs:** The K-Means algorithm is executed for each unique combination of two metrics.
        * For each metric pair, data is selected, and the K-Means algorithm is initialized and trained. `random_state=42` ensures reproducibility.
        * Cluster labels assigned to each data point (artwork) are added to the DataFrame.
        * Cluster centroids are calculated and printed, providing the mean coordinates of each group in the metric space.
    * **Cluster Visualization:** For each analyzed metric pair, a scatter plot is generated:
        * Points represent individual artworks, colored according to the K-Means assigned cluster.
        * Cluster centroids are visualized as red and black 'X' markers, highlighting the "center" of each group.
        * The plot title indicates the metrics used and the number of clusters.
        * Each plot is saved as a PNG image in the `.\\PLOT\\CLUSTER\\` directory (different from normalized data) to clearly distinguish results.
* **Output:**
    * Printout of centroids for each analyzed metric pair.
    * Several PNG image files (`.\\PLOT\\CLUSTER\\KMeans_Cluster_... .png`), each showing the distribution of K-Means clusters for a specific pair of unnormalized metrics.
    * The original DataFrame enriched with new columns indicating cluster assignment for each artwork in relation to each considered metric pair.
* **Insights:** This section is fundamental for comparing the effectiveness of K-Means clustering with and without normalization. It is expected that clusters formed with unnormalized data may be strongly influenced by the scale of individual metrics, leading to less significant groupings or those dominated by the metric with the widest value range. This comparison will help highlight the importance of data preprocessing (normalization) for obtaining more robust and interpretable clustering results, especially when metrics have different scales.

---

## Section 12: Kernel Density Estimation (KDE) (Normalized Data)

* **Purpose:** To analyze the probability distribution of Picasso's visual metrics using Kernel Density Estimation (KDE) on **normalized** data. This section aims to visualize the density of data points in the metric space, identifying areas of higher concentration and potential overlaps between stylistic periods.
* **Input:** The CSV file containing normalized metrics for each artwork (`.\\CSV NORMALIZED\\normalized_metrics_with_details_no_outlier.csv`).
* **Methods:**
    * **Data Preparation:**
        * The normalized dataset is loaded.
        * Metric columns ('Fractal dimension', 'Entropy', 'Birkhoff measure', 'Euclidean distance') are identified and converted to a numeric format. Rows with NaN values in the metrics are removed to ensure calculation integrity.
    * **Normalization (Min-Max Scaling):** Although the data is already loaded from a "normalized" file, Min-Max re-normalization (between 0 and 1) is performed on the metrics to ensure all values fall within a consistent range, which is crucial for Mahalanobis Distance calculation and KDE visualization.
    * **Mahalanobis Distance Calculation:**
        * For each artistic period (defined by the 'Period' column), the Mahalanobis distance of each artwork from its period's centroid is calculated.
        * Mahalanobis distance accounts for the correlation between metrics and their variance, providing a more robust measure of "distance" than simple Euclidean distance, especially in multidimensional spaces.
        * Potential singularity of the covariance matrix is handled by adding a small "jitter" to ensure its invertibility.
        * The calculated distances are added to the DataFrame in a new column 'Mahalanobis_Distance_Normalized'.
    * **Contextual Fit Calculation:**
        * A "Contextual Fit" metric is introduced, defined as the inverse of the Mahalanobis Distance (1 / (Mahalanobis_Distance + epsilon), where epsilon is a small value to prevent division by zero).
        * A higher "Contextual Fit" value indicates that an artwork better fits the average characteristics of its period.
        * This metric is added to the DataFrame in a new column 'Contextual_Fit'.
    * **Summary Statistics Calculation:** The means and standard deviations of the normalized Mahalanobis Distance and "Contextual Fit" are calculated and printed for each artistic period, providing a numerical summary of the internal homogeneity of each period.
    * **KDE Plot Generation:**
        * **Univariate KDE (2x2 Subplot):** A single plot containing four subplots is generated, each showing the probability density distribution (KDE) for a single metric (Fractal Dimension, Entropy, Birkhoff Measure, Euclidean Distance). This provides an overview of the distribution of each metric across the entire dataset.
        * **Bivariate KDE (for Metric Pairs):** Separate KDE plots are generated for each unique combination of two metrics. These plots show the joint density of the two metrics, highlighting areas where artworks are more concentrated and the relationships between the metrics. Individual data points are also added to provide context.
        * All plots are saved as PNG images in the `.\\PLOT NORMALIZED\\KDE PLOT\\` directory.
* **Output:**
    * The DataFrame enriched with 'Mahalanobis_Distance_Normalized' and 'Contextual_Fit' columns.
    * Printout of the means and standard deviations of Mahalanobis distances and "Contextual Fit" per period.
    * A PNG image file (`.\\PLOT NORMALIZED\\KDE PLOT\\all_univariate_kde_2x2_subplot.png`) showing the univariate distributions of all metrics.
    * Several PNG image files (`.\\PLOT NORMALIZED\\KDE PLOT\\kde_bivariate_... .png`), each depicting the joint density distribution for a pair of normalized metrics.
* **Insights:** This section provides an in-depth understanding of the distribution of metrics and the internal cohesion of the periods. KDE plots will visualize Picasso's stylistic "fingerprints," showing how metrics cluster and overlap. Mahalanobis distances and "Contextual Fit" will quantify how individual artworks deviate from or conform to their period's prototype, providing a basis for evaluating the fluidity or rigidity of stylistic transitions.

---

## Section 12.1: Kernel Density Estimation (KDE) (Unnormalized Data)

* **Purpose:** To analyze the probability distribution of Picasso's visual metrics using Kernel Density Estimation (KDE) on **unnormalized** data. This section is crucial for comparing the impact of normalization on the interpretation of stylistic relationships and the shape of distributions.
* **Input:** The CSV file containing original (unnormalized) metrics for each artwork (`.\\CSV\\metrics_with_details_no_outlier.csv`).
* **Methods:**
    * **Data Preparation:**
        * The original (unnormalized) dataset is loaded.
        * Metric columns ('Fractal dimension', 'Entropy', 'Birkhoff measure', 'Euclidean distance') are identified and converted to a numeric format. Rows with NaN values in the metrics are removed.
    * **Normalization (Min-Max Scaling):** Unlike Section 12, here the original data is Min-Max normalized (between 0 and 1) *within this section* before calculating the Mahalanobis Distance. This step is intentional to allow the Mahalanobis calculation on scaled data, even if the focus is on KDE of unnormalized data. For KDEs, `df_cleaned_for_kde` which contains the original (unnormalized) values is used.
    * **Mahalanobis Distance Calculation (on normalized data):**
        * For each period, the Mahalanobis distance of each artwork from its period's centroid is calculated, but **on normalized versions of the data within this phase**. This calculation is functional to provide the 'Contextual_Fit' metric, which is based on scaled data to be interpretable across different metrics.
        * Cases of insufficient data points or singular covariance matrices are handled by adding jitter for invertibility.
        * The calculated distances are added to the DataFrame in a new column 'Mahalanobis_Distance_Normalized'.
    * **Contextual Fit Calculation (on normalized data):**
        * A new column 'Contextual_Fit' is created.
        * For each artwork, the "Contextual Fit" is calculated as the inverse of its 'Mahalanobis_Distance' (i.e., $1 / (\text{Mahalanobis_Distance})$) based on the Mahalanobis distances calculated on normalized data.
        * This metric is added to the DataFrame in a new column 'Contextual_Fit'.
    * **Summary Statistics Calculation:** The means and standard deviations of the normalized Mahalanobis Distance and "Contextual Fit" are calculated and printed for each artistic period.
    * **KDE Plot Generation (on original UNnormalized data):**
        * **Univariate KDE (2x2 Subplot):** A single plot containing four subplots is generated, each showing the probability density distribution (KDE) for a single metric (Fractal Dimension, Entropy, Birkhoff Measure, Euclidean Distance) **using the original unnormalized data** (`df_cleaned_for_kde`).
        * **Bivariate KDE (for Metric Pairs):** Separate KDE plots are generated for each unique combination of two metrics. These plots show the joint density of the two metrics **using the original unnormalized data**, highlighting areas of higher concentration. Individual data points are also added to provide context.
        * All plots are saved as PNG images in the `.\\PLOT\\KDE PLOT\\` directory (different from the normalized data directory) to clearly distinguish results.
* **Output:**
    * The DataFrame enriched with 'Mahalanobis_Distance_Normalized' and 'Contextual_Fit' columns (whose values are derived from metrics normalized internally in this phase).
    * Printout of the means and standard deviations of Mahalanobis distances and "Contextual Fit" per period.
    * A PNG image file (`.\\PLOT\\KDE PLOT\\all_univariate_kde_2x2_subplot.png`) showing the univariate distributions of all **unnormalized** metrics.
    * Several PNG image files (`.\\PLOT\\KDE PLOT\\kde_bivariate_... .png`), each depicting the joint density distribution for a pair of **unnormalized** metrics.
* **Insights:** The comparison between the KDE plots generated here (on unnormalized data) and those in Section 12 (on normalized data) is crucial. KDE distributions on unnormalized data are expected to show very different peaks and dispersions due to the intrinsic scales of each metric, potentially obscuring relationships or creating the illusion of separations where none are significant after normalization. This step reinforces the importance of normalization for fair comparative analysis and for the validity of techniques like Mahalanobis Distance, which depend on the scales and correlations between variables.

---

## Section 13: Hausdorff Measure (Normalized Data)

* **Purpose:** To calculate the **multidimensional Hausdorff Distance** between consecutive artistic periods of Picasso, using **normalized** visual metrics. This measure quantifies the "similarity" or "maximum distance" between two sets of points (artistic periods) in a multidimensional space, offering an indication of transition and distinction between styles.
* **Input:** The CSV file containing normalized metrics for each artwork (`.\\CSV NORMALIZED\\normalized_metrics_with_details_no_outlier.csv`).
* **Methods:**
    * **Definition of Multidimensional Hausdorff Function:** A `hausdorff_multidimensional(A, B)` function is implemented that calculates the Hausdorff distance between two multidimensional sets of points ($A$ and $B$).
        * The function uses Euclidean distance to calculate point-to-point distances between the sets.
        * It calculates the two direct Hausdorff distances: $h(A, B)$ (the maximum distance of a point in A to its closest point in B) and $h(B, A)$ (the maximum distance of a point in B to its closest point in A).
        * The final Hausdorff distance is the maximum of these two direct distances, representing the "worst-case" distance between the two sets.
        * It handles the case of empty sets, returning $0.0$.
    * **Data Loading and Preparation:**
        * The normalized metrics dataset is loaded.
        * A sequential order for artistic periods (`style_order`) is defined to analyze consecutive transitions.
        * Key numerical metric columns are selected: 'Fractal dimension', 'Entropy', 'Birkhoff measure', 'Euclidean distance'.
        * Column presence is verified, and explicit conversion to numeric type is performed. Rows with NaN values in metric columns are removed to ensure calculation validity.
    * **Data Normalization:** Metric data is further normalized to a $[0, 1]$ range using `MinMaxScaler`. Even if the input file is already "normalized," this step ensures consistency and robustness of the range for specific Hausdorff distance calculations, especially if the previous normalization process used a different method or range.
    * **Distance Calculation for Consecutive Pairs:**
        * The code iterates through pairs of consecutive periods defined in `style_order`.
        * For each pair, it extracts the subsets of data corresponding to the artworks of each period.
        * It calls the `hausdorff_multidimensional` function to calculate the distance between the two subsets.
        * Results are printed and collected in a DataFrame.
* **Output:**
    * A tabular printout showing the multidimensional Hausdorff Distance calculated for each transition between consecutive artistic periods (e.g., "Early Years" to "Blue Period", "Blue Period" to "Rose Period", etc.).
    * A CSV file (`.\\CSV NORMALIZED\\normalized_hausdorff4D.csv`) containing the results of these distances.
* **Insights:** This section provides a direct quantification of the "separation" between Picasso's different stylistic periods in the multidimensional space of normalized visual metrics. Higher Hausdorff Distance values will indicate greater discontinuity or a sharper transition between the two styles, suggesting that the artworks of those periods occupy more distinct regions of the feature space. Conversely, lower values might suggest a more gradual transition or greater overlap between styles. These results are crucial for understanding Picasso's stylistic evolution from a computational perspective.

---

## Section 13.1: Hausdorff Measure (Unnormalized Data)

* **Purpose:** To calculate the **multidimensional Hausdorff Distance** between consecutive artistic periods of Picasso, using **unnormalized** visual metrics. This section serves as a counterpart to Section 13, to investigate how the lack of normalization affects the perception of distances and transitions between styles.
* **Input:** The CSV file containing original (unnormalized) metrics for each artwork (`.\\CSV\\metrics_with_details_no_outlier.csv`).
* **Methods:**
    * **Definition of Multidimensional Hausdorff Function:** The same `hausdorff_multidimensional(A, B)` function as in Section 13 is used, which calculates the Hausdorff distance between two multidimensional sets of points. The function relies on Euclidean distance and considers the two maximum direct distances between the sets.
    * **Data Loading and Preparation:**
        * The original (unnormalized) dataset is loaded.
        * The sequential order for artistic periods (`style_order`) is defined.
        * Key numerical metric columns are selected: 'Fractal dimension', 'Entropy', 'Birkhoff measure', 'Euclidean distance'.
        * Column presence is verified, and explicit conversion to numeric type is performed. Rows with NaN values in metric columns are removed.
    * **Data Normalization (Important):** Despite the objective being to analyze the impact of *unnormalized* data, the `hausdorff_multidimensional` function (which uses `cdist` with 'euclidean' as metric) is scale-sensitive. Therefore, **within this section**, the metric data is still normalized to a $[0, 1]$ range using `MinMaxScaler` *before* calculating the distances. This is a technical compromise: although starting from "unnormalized" data in the CSV file, calculating Hausdorff distance (which is a metric distance like Euclidean) often requires scaled data to prevent a single metric with a very wide value range from dominating the overall distance. The crucial point is that the *input file* was not pre-normalized like the one in Section 13, but normalization is applied *here* for the Hausdorff calculation.
    * **Distance Calculation for Consecutive Pairs:**
        * The code iterates through pairs of consecutive periods defined in `style_order`.
        * For each pair, it extracts the subsets of data corresponding to the artworks of each period.
        * It calls the `hausdorff_multidimensional` function to calculate the distance between the two subsets of data **normalized in this step**.
        * Results are printed and collected in a DataFrame.
* **Output:**
    * A tabular printout showing the multidimensional Hausdorff Distance calculated for each transition between consecutive artistic periods.
    * A CSV file (`.\\CSV\\hausdorff4D.csv`) containing the results of these distances.
* **Insights:** The comparison between the Hausdorff Distance results calculated here (with normalization applied *locally* before calculation) and those in Section 13 (where the input data was already normalized) is subtle but important. Both sections use normalized data for the actual Hausdorff distance calculation. The main difference lies in the origin point of normalization and the term "unnormalized" which refers to the input CSV file. If the results are very similar, this would indicate the robustness of the normalization method used. However, Section 13.1, while normalizing, relies on a CSV file that has not undergone a "global" pre-normalization like the file used in Section 13. This could lead to slight differences if the previous cleaning or normalization process was not identical. The analysis in this section aims to ensure that, even starting from raw data, applying normalization before calculating Hausdorff distance is a necessary step to obtain meaningful measures of stylistic separation.

---

## Section 14: Contextual Fit (Normalized Data)

* **Purpose:** To calculate the "Contextual Fit" for each artwork, based on the Mahalanobis Distance calculated within its own stylistic period, using **normalized** visual metric data. This metric provides an indication of how well an artwork "fits" the statistical characteristics of its belonging style, with higher values indicating a better fit. Subsequently, summary statistics (mean and standard deviation) of these measures will be calculated for each period.
* **Input:** The CSV file containing normalized metrics for each artwork (`.\\CSV NORMALIZED\\normalized_metrics_with_details_no_outlier.csv`).
* **Methods:**
    * **Data Loading:** The normalized metrics dataset is loaded, paying attention to the decimal separator (comma).
    * **Numerical Feature Definition:** The four key metrics ('Fractal dimension', 'Entropy', 'Birkhoff measure', 'Euclidean distance') are identified. Robust column validation is performed, converting them to numeric and removing rows with NaN values in these columns to ensure calculation integrity.
    * **Mahalanobis Distance Calculation:**
        * The DataFrame is grouped by the 'Period' column.
        * For each period:
            * The centroid (mean) of the metrics for that period is calculated.
            * The covariance matrix of the metrics is calculated.
            * A check for covariance matrix singularity (zero determinant) is performed, and if necessary, a small diagonal term (jitter) is added to make it invertible.
            * The inverse covariance matrix is calculated.
            * For each artwork within the period, the Mahalanobis distance between the artwork itself and the centroid of its period is calculated, using the period-specific inverse covariance matrix.
            * The calculated distances are mapped back to the original DataFrame's index in the new 'Mahalanobis_Distance' column.
    * **Contextual Fit Calculation:**
        * A new column 'Contextual_Fit' is created.
        * For each artwork, the "Contextual Fit" is calculated as the inverse of its 'Mahalanobis_Distance' (i.e., $1 / (\text{Mahalanobis_Distance})$). A small epsilon value is implicitly considered or handled to prevent division by zero or infinite values for distances close to zero (although the code explicitly handles the case of distance 0 by setting `np.inf`). NaN values in Mahalanobis distance also result in NaN for Contextual Fit.
    * **Saving Intermediate Data:** The complete DataFrame with the new 'Mahalanobis_Distance' and 'Contextual_Fit' columns is saved to a new CSV file (`.\\CSV NORMALIZED\\normalized_contextual_fit.csv`).
    * **Calculation and Saving of Summary Statistics:**
        * The mean and standard deviation of 'Mahalanobis_Distance' and 'Contextual_Fit' are calculated for each 'Period'.
        * The columns of the statistics DataFrame are flattened for better readability.
        * These aggregated statistics are saved to another CSV file (`.\\CSV NORMALIZED\\normalized_mahalanobis_contextual_fit.csv`).
* **Output:**
    * The original DataFrame extended with 'Mahalanobis_Distance' and 'Contextual_Fit' columns, with the first 10 rows displayed.
    * A CSV file (`.\\CSV NORMALIZED\\normalized_contextual_fit.csv`) containing the entire updated DataFrame with the new metrics for each artwork.
    * A tabular printout of the means and standard deviations of 'Mahalanobis_Distance' and 'Contextual_Fit' for each period.
    * A CSV file (`.\\CSV NORMALIZED\\normalized_mahalanobis_contextual_fit.csv`) containing these aggregated statistics per period.
* **Insights:** This section provides crucial quantification of stylistic homogeneity within each period. A high "Contextual Fit" value for an artwork indicates that it is very representative of its period. By analyzing the means and standard deviations of these metrics for each period, it's possible to identify which periods are more "compact" (artworks very similar to each other) and which are more "diverse" or "fluid" (artworks more dispersed around the centroid), providing insights into the rigidity of stylistic conventions at different times in Picasso's career. This data is fundamental for discussing stylistic evolution and period coherence.

---

## Section 14.1: Contextual Fit (Unnormalized Data)

* **Purpose:** To calculate the "Contextual Fit" for each artwork, based on the Mahalanobis Distance calculated within its own stylistic period, using **unnormalized** visual metric data. This section compares the results with Section 14, highlighting the impact of normalization on Mahalanobis Distance and, consequently, on "Contextual Fit".
* **Input:** The CSV file containing original (unnormalized) metrics for each artwork (`.\\CSV\\metrics_with_details_no_outlier.csv`).
* **Methods:**
    * **Data Loading:** The original metrics dataset is loaded, paying attention to the decimal separator.
    * **Numerical Feature Definition:** The four key metrics are identified. Robust column validation is performed, converting them to numeric and removing rows with NaN values.
    * **Mahalanobis Distance Calculation:**
        * The DataFrame is grouped by the 'Period' column.
        * For each period:
            * The centroid (mean) of the metrics is calculated.
            * The covariance matrix of the metrics is calculated.
            * Covariance matrix singularity is handled by adding a small diagonal term (jitter) to make it invertible.
            * The inverse covariance matrix is calculated.
            * For each artwork within the period, the Mahalanobis distance between the artwork itself and the centroid of its period is calculated, using the period-specific inverse covariance matrix. **It is crucial to note that here the Mahalanobis Distance is calculated directly on the original (unnormalized) metric values loaded from the CSV.** This is the main divergence point from Section 14.
            * The calculated distances are mapped back to the original DataFrame's index in the new 'Mahalanobis_Distance' column.
    * **Contextual Fit Calculation:**
        * A new column 'Contextual_Fit' is created.
        * For each artwork, the "Contextual Fit" is calculated as the inverse of its 'Mahalanobis_Distance'.
    * **Saving Intermediate Data:** The complete DataFrame with the new 'Mahalanobis_Distance' and 'Contextual_Fit' columns is saved to a new CSV file (`.\\CSV\\contextual_fit.csv`).
    * **Calculation and Saving of Summary Statistics:**
        * The mean and standard deviation of 'Mahalanobis_Distance' and 'Contextual_Fit' are calculated for each 'Period'.
        * The columns of the statistics DataFrame are flattened.
        * These aggregated statistics are saved to another CSV file (`.\\CSV\\mahalanobis_contextual_fit.csv`).
* **Output:**
    * The original DataFrame extended with 'Mahalanobis_Distance' and 'Contextual_Fit' columns, with the first 10 rows displayed.
    * A CSV file (`.\\CSV\\contextual_fit.csv`) containing the entire updated DataFrame with the new metrics for each artwork.
    * A tabular printout of the means and standard deviations of 'Mahalanobis_Distance' and 'Contextual_Fit' for each period.
    * A CSV file (`.\\CSV\\mahalanobis_contextual_fit.csv`) containing these aggregated statistics per period.
* **Insights:** This section is fundamental for demonstrating the impact of normalization on Mahalanobis Distance and the "Contextual Fit" metric. Without normalization, metrics with wider value ranges (e.g., 'Euclidean distance' if its values are much larger than others) will tend to dominate the distance calculation, potentially masking the contributions of other metrics and leading to a less significant "Contextual Fit." It is expected that the values of 'Mahalanobis_Distance' and 'Contextual_Fit' will be very different from Section 14, and the interpretation of periods as more or less "compact" may change drastically. This underscores the importance of normalization as a critical pre-processing step for multivariate analysis.

---

## Section 15: Bifurcation Maps (Normalized Data)

* **Purpose:** To visualize the distribution of Picasso's "Fractal Dimension" in relation to his artistic periods and creation years, using **normalized** visual metric data. These "Bifurcation Maps" help identify stylistic transitions and the variability of fractal dimension within and between periods.
* **Input:** The CSV file containing normalized metrics for each artwork (`.\\CSV NORMALIZED\\normalized_metrics_with_details_no_outlier.csv`).
* **Methods:**
    * **Data Loading:** The normalized metrics dataset is loaded. `FileNotFoundError` is handled.
    * **Period Definition and Order:** The chronological order of Picasso's artistic periods (`ordered_styles`) is defined. The 'Period' column of the DataFrame is converted to a categorical type with this order to ensure graphs are displayed correctly in temporal sequence.
    * **Stripplot Generation (Bifurcation Maps):** An `sns.stripplot` is used for each plot. A stripplot is a good choice for visualizing the distribution of one-dimensional data points for different categories, allowing to see data density and individual points. `jitter` adds a slight random displacement to points to prevent overlap and show data density.
        * **"Fractal Dimension" vs. "Artistic Period" (Horizontal):**
            * Y-axis: 'Artistic Period' (with the defined order).
            * X-axis: 'Fractal Dimension'.
            * Orientation: Horizontal (`orient='h'`).
            * The plot visualizes the spread of fractal dimension for each period.
            * Saved as `.\\PLOT NORMALIZED\\BIFURCATION MAP\\fractal_dimension_by_artistic_period_h.png`.
        * **"Fractal Dimension" vs. "Artistic Period" (Vertical):**
            * X-axis: 'Artistic Period' (with the defined order).
            * Y-axis: 'Fractal Dimension'.
            * Orientation: Vertical.
            * X-axis labels rotated for readability.
            * Saved as `.\\PLOT NORMALIZED\\BIFURCATION MAP\\fractal_dimension_by_artistic_period_v.png`.
        * **"Fractal Dimension" vs. "Year" (Horizontal):**
            * Y-axis: 'Year of creation'.
            * X-axis: 'Fractal Dimension'.
            * Orientation: Horizontal (`orient='h'`).
            * The plot shows the evolution of fractal dimension over time.
            * Saved as `.\\PLOT NORMALIZED\\BIFURCATION MAP\\fractal_dimension_by_year_horizontal.png`.
        * **"Fractal Dimension" vs. "Year" (Vertical):**
            * X-axis: 'Year of creation'.
            * Y-axis: 'Fractal Dimension'.
            * Orientation: Vertical (`orient='v'`).
            * X-axis labels rotated for readability.
            * Saved as `.\\PLOT NORMALIZED\\BIFURCATION MAP\\fractal_dimension_by_year_vertical.png`.
    * **Saving Plots:** All plots are saved in a dedicated subdirectory `.\\PLOT NORMALIZED\\BIFURCATION MAP\\`.
* **Output:** Four PNG image files, each depicting a bifurcation map of fractal dimension in relation to artistic periods or creation years, in horizontal and vertical orientations.
* **Insights:** Bifurcation maps offer a detailed and intuitive visualization of fractal dimension value distributions. They allow observing:
    * **Cohesion/Variability within Periods:** If points for a given period are tightly clustered, it indicates greater stylistic consistency for that metric. If they are widely scattered, it suggests greater variability.
    * **Transitions between Periods:** How the ranges and densities of points change between consecutive periods. This can highlight "bifurcations" or sudden shifts in the fractal dimension distribution, suggesting distinct stylistic passages.
    * **Temporal Trends:** Plots by year can reveal a gradual or abrupt evolution of fractal dimension throughout Picasso's career, potentially correlated with the introduction of new styles or techniques.
    * Since data is normalized, comparison between periods is fairer and not influenced by original metric scale differences.

---

## Section 15.1: Bifurcation Maps (Unnormalized Data)

* **Purpose:** To visualize the distribution of Picasso's "Fractal Dimension" in relation to his artistic periods and years of creation, using **unnormalized** visual metric data. This section replicates the analysis of Section 15 but with the original data, allowing a direct comparison of the impact of normalization on the visualizations.
* **Input:** The CSV file containing the original (unnormalized) metrics for each artwork (`.\\CSV\\metrics_with_details_no_outlier.csv`).
* **Methods:**
    * **Data Loading:** The original metrics dataset is loaded. File absence is handled.
    * **Period Definition and Order:** The chronological order of artistic periods (`ordered_styles`) is defined. The 'Period' column of the DataFrame is converted to a categorical type with this order to ensure chronological visualization.
    * **Stripplot Generation (Bifurcation Maps):** For each plot, an `sns.stripplot` is used, which is ideal for showing the distribution of one-dimensional data points for different categories, revealing density and individual point positions. `Jitter` is applied to spread overlapping points.
        * **"Fractal Dimension" vs. "Artistic Period" (Horizontal):**
            * Y-axis: 'Artistic Period'.
            * X-axis: 'Fractal Dimension'.
            * Orientation: Horizontal (`orient='h'`).
            * Saving: `.\\PLOT\\BIFURCATION MAP\\fractal_dimension_by_artistic_period_h.png`.
        * **"Fractal Dimension" vs. "Artistic Period" (Vertical):**
            * X-axis: 'Artistic Period'.
            * Y-axis: 'Fractal Dimension'.
            * Orientation: Vertical.
            * X-axis labels are rotated for better readability.
            * Saving: `.\\PLOT\\BIFURCATION MAP\\fractal_dimension_by_artistic_period_v.png`.
        * **"Fractal Dimension" vs. "Year" (Horizontal):**
            * Y-axis: 'Year of creation'.
            * X-axis: 'Fractal Dimension'.
            * Orientation: Horizontal (`orient='h'`).
            * Saving: `.\\PLOT\\BIFURCATION MAP\\fractal_dimension_by_year_horizontal.png`.
        * **"Fractal Dimension" vs. "Year" (Vertical):**
            * X-axis: 'Year of creation'.
            * Y-axis: 'Fractal Dimension'.
            * Orientation: Vertical (`orient='v'`).
            * X-axis labels are rotated for readability.
            * Saving: `.\\PLOT\\BIFURCATION MAP\\fractal_dimension_by_year_vertical.png`.
    * **Saving Plots:** All plots are saved in a dedicated subdirectory `.\\PLOT\\BIFURCATION MAP\\`, distinct from the one used for normalized data.
* **Output:** Four PNG image files, each representing a bifurcation map of fractal dimension in relation to artistic periods or years of creation, in horizontal and vertical orientations, based on **unnormalized data**.
* **Insights:** The visual comparison between the bifurcation maps in this section (unnormalized data) and those in Section 15 (normalized data) is crucial. It is expected that the graphs on unnormalized data may show very different value ranges, potentially influenced by the intrinsic scale of fractal dimension and not by its relative variation. This direct comparison will help understand how normalization affects the perception of variability and transitions between periods, and reinforce the argument for using normalized data for meaningful comparative analysis between metrics.

---

## Section 16: Attractor Detection

* **Purpose:** This section aims to identify "clusters" or "attractors" in the multidimensional space of Picasso's visual metrics (Fractal Dimension, Entropy, Birkhoff Measure, Euclidean Distance). These attractors represent recurrent or prevalent morphological and structural configurations in his artworks, regardless of the formally assigned stylistic period. The approach is based on the **Mean-Shift** clustering algorithm, followed by visualizations using PCA, t-SNE, and UMAP to explore data structure, and finally an analysis of metric distributions per cluster.
* **Input:** The CSV file `normalized_metrics.csv` containing normalized visual metrics for each artwork.
* **Methods:**
    * **Data Loading and Preparation:**
        * The dataset is loaded.
        * Metric columns ("Fractal dimension", "Entropy", "Birkhoff measure", "Euclidean distance") are cleaned of any non-numeric values and converted to `float` type.
        * An array `X` containing only the values of the four metrics for clustering is created.
    * **Mean-Shift Clustering:**
        * An **adaptive bandwidth** for the Mean-Shift algorithm is estimated. Bandwidth is a crucial parameter that determines the scale of cluster detection. A value of `0.39 * median_bw` is used as specified, suggesting a slightly more permissive approach to cluster definition than a pure median bandwidth.
        * The Mean-Shift algorithm is applied to the data `X`. This algorithm does not require a predefined number of clusters and identifies them autonomously based on data point density.
        * Cluster labels (`labels`) assigned to each data point are added to the original DataFrame in a new 'Cluster' column.
        * The number of clusters found is printed.
    * **Cluster Analysis and Visualization:**
        * **Dimensionality Reduction (PCA):** Principal Component Analysis (PCA) is applied to reduce the four metric dimensions to two principal components (`X_pca`), facilitating visualization.
        * **PCA Scatter Plot:** A scatter plot of the data projected onto PCA 1 and PCA 2 is generated, with points colored according to the cluster assigned by Mean-Shift. This plot helps visualize cluster separation in the principal component space.
        * **Statistics per Cluster:** The means of the four metrics for each identified cluster are calculated and printed. This provides a numerical characterization of each attractor.
        * **Bar Plots of Means per Metric and Cluster:** Separate bar plots are generated for each of the four metrics, showing the average value of that metric for each cluster. This helps understand which metrics best define each cluster.
        * **Scaled Means Bar Plot per Metric and Cluster:** The means of the metrics for each cluster are further scaled (normalized between 0 and 1) using `MinMaxScaler`. This allows a direct comparison of the relative importance of each metric within a cluster, visualized in a single bar plot grouped by cluster and metric.
        * **Dimensionality Reduction (t-SNE):** t-Distributed Stochastic Neighbor Embedding (t-SNE) is applied to reduce the data to two dimensions (`X_tsne`). t-SNE is particularly effective at preserving local proximity relationships and visualizing the intrinsic structure of data in high-dimensional spaces.
        * **t-SNE Scatter Plot:** A scatter plot of the data projected by t-SNE is created, colored by cluster. This plot offers an alternative visualization to PCA, often revealing more distinct groupings if clusters have complex shapes.
        * **Dimensionality Reduction (UMAP):** Uniform Manifold Approximation and Projection (UMAP) is applied to reduce the data to two dimensions (`embedding`). UMAP is similar to t-SNE but often faster and more suitable for large datasets, aiming to preserve both local and global structure.
        * **UMAP Scatter Plot:** A scatter plot of the data projected by UMAP is generated, colored by cluster. This plot provides another perspective on the arrangement of clusters in the feature space.
        * **"Fractal Dimension" Bifurcation Map per Cluster:** A stripplot is generated that visualizes the distribution of "Fractal Dimension" values for each cluster. This plot acts as a "bifurcation map," showing the spread of the metric within each attractor and the transitions between them.
* **Output:**
    * The number of clusters found by the Mean-Shift algorithm.
    * Printouts of the average statistics for each metric within each cluster.
    * Plots:
        * Scatter plot of clusters projected onto PCA.
        * Four separate bar plots, one for each metric, showing average values per cluster.
        * A unified bar plot of scaled metric averages per cluster (with scaled metrics).
        * Scatter plot of clusters projected onto t-SNE.
        * Scatter plot of clusters projected onto UMAP.
        * A "bifurcation" type plot of Fractal Dimension per cluster.
* **Insights:** This analysis allows the identification and characterization of "attractors" in Picasso's style based on objective metrics. These attractors may or may not correspond to traditional stylistic periods, offering a data-driven perspective on the evolution of his visual language. Clusters represent stable states or morphological configurations that the artist explored. PCA, t-SNE, and UMAP visualizations help understand the separation and structure of these attractors in the feature space, while the analysis of metric averages per cluster quantifies the distinctive characteristics of each attractor. The bifurcation map of fractal dimension (or other metrics) per cluster can show how a specific metric is distributed within each attractor and whether there are jumps or continuities between them, suggesting points of change or consolidation in style.

---
