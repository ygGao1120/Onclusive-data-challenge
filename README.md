# Onclusive-data-challenge

There are 2 major parts in this data assignment: data preprocessing (preprocessing.ipynb) and model building (Final_model.ipynb). 

The preprocessing part includes : 
1. Removing missing values; 
2. Transferring labels into numerical numbers; 
3. Selecting top k evidence sentences in ‘main_text’, based on sentence transformer mode, generates a new column called ‘top_k’;
4. Data cleaning for column ‘claim’, ‘explanation’ and ‘top_k’: removing new-line characters,symbols and numbers, and turning all words into lowercase;
5. Tokenization;
6. Stemming;
7. Removing stop words;
8. Removing outliers;
The datasets after preprocessing are ‘cleaned_train.xlsx’, ‘cleaned_valid.xlsx’, and ‘cleaned_test.xlsx’.

For model building, I fine-tuned pre-trained bert based models to do the prediction. The inputs are columns ‘clean_claim’ and ‘clean_explanation’, and the output is the predicted labels. I choose 3 pre-trained models: ‘bert-base-uncased’, ‘microsoft/BiomedNLP-PubMedBERT-base-uncased-abstract-fulltext’, and ‘monologg/biobert_v1.1_pubmed’.  I optimized my prediction model on cross entropy loss with Adam optimizer, and used prediction accuracy to evaluate the model's performance.

Because of the limitation of time and the computational resources I have, I set hyperparameter ‘epochs’ equal to 1 and ‘max_length’ of the Tokenizer encoder equal to 100. And I performed hyper-parameter grid search as part of validation for models from the 3 models listed above, batch sizes from {32,64,128}, learning rate from {1e-5, 2e-5, 3e-5, 4e-5}. The candidate models and hyperparameters are first trained on the training set and tested on the validation set to find the best combination. 

The result shows that the best model is ‘monologg/biobert_v1.1_pubmed’ with batch size 64 and learning rate 4e-5. Then I re-trained this model on training set and validation set and tested it on test set, getting a prediction accuracy of 0.6829.  

The result shows that training veracity prediction on in-domain data improves the accuracy of veracity prediction. A possible improvement for current work is to increase the value of ‘max_len’, then test the performance of BiomedNLP and Biobert. BiomedNLP is trained on the abstract and full-text articles from PubMed and PubMedCentral, while Biobert is only trained on the abstract. Intuitively BiomedNLP should contain more med-related information. I think the reason why Biobert has a better performance is that the ‘max_len’ limits the amount of information that can be entered into the models.
