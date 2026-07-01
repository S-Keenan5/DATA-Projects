Dual-Input 1D CNN and AAC Antimicrobial Predictor

A dual-input model combining 1D CNN and Amino Acid Composition (AAC) branches was developed to predict antimicrobial activity, enhancing a baseline 1D CNN that failed to consistently exceed 0.985 mean ROC AUC. The model uses the AAC branch to provide global contextual information that the local 1D CNN branch misses, enabling consistent performance above 0.985 mean ROC AUC across all random seeds, including seed 2.

Project Structure

- Repository Files: Contains the primary model notebook, dataset preprocessing scripts, and training validation documentation.
- Core Frameworks: Built using Python, TensorFlow, Keras, and Scikit-Learn.

Model Architecture

1D CNN Branch (Local Patterns)
- Sequence Encoding: Input biological sequences are one-hot encoded using a matrix where rows represent sequence positions and columns represent the 20 standard amino acids.
- Convolutional Layers: 
  - Block 1: 128 filters with a kernel size of 3 to extract localized, short residue patterns.
  - Block 2: 256 filters with a kernel size of 5 to process medium 5-residue structural patterns.
  - Block 3: 256 filters without pooling to further refine the sequence representation. Pooling was stopped at this stage due to the sequence compression reaching a 50 to 12 ratio.
- Pooling Layer: Concatenates GlobalMaxPooling1D and GlobalAveragePooling1D. Max pooling identifies the presence of specific peak residue sequences, while average pooling captures the broad activation levels across all positions to improve Precision-Recall (PR) AUC.

AAC Branch (Global Context)
- Feature Engineering: Computes the absolute frequency of each of the 20 standard amino acids directly from the source FASTA data column without external dependencies.
- Dense Networks: Processes the frequency vectors through two small dense layers of 64 and 32 neurons to learn structural frequency patterns associated with antimicrobial traits.
- Data Integrity: A StandardScaler is fitted strictly on the training partition only and applied independently to the validation and testing sets to prevent data leakage.

Training and Optimization Workflow

- Input Management: Dual branch arrays are supplied dynamically as a list argument straight into the standard fit and predict functions.
- Dynamic Learning Rates: Utilizes the ReduceLROnPlateau callback with a patience setting of 5 epochs to split the learning rate in half when navigating flat loss surfaces.
- Early Stopping: Configured with a patience setting of 20 epochs to provide the architecture sufficient training space to recover from temporary plateaus before executing a hard halt.

Future Explorations

- Evaluate the implementation of Term Frequency-Inverse Document Frequency (TF-IDF) feature text embeddings.
- Experiment with integrating these tokenized embeddings into Recurrent Neural Networks (RNN) or ensemble structures to treat amino acid residues as distinct text characters.

Development Assistance

- Artificial intelligence tools including ChatGPT, Claude, and Gemini were utilised for technical consultation and assistance during the construction of this project.
