# Twitter_Sentiment_Classification

## Introduction
Online shopping platforms, social media platforms, and other internet applications or interactions have become part of people's daily lives. Consequently, customer reviews and community comments are readily available all over the internet, reflecting consumers' emotions and satisfaction with products or services.

From a business perspective, mining insights from the vast volume of user comments can uncover pain points and areas for improvement in products or services. As the data volume of comments continues to grow, manually reviewing each comment for sentiment analysis becomes inefficient. Therefore, automating the process to quickly analyze the emotional responses to a particular product and gain a precise understanding of pain points is essential to harness the benefits of big data effectively.

This is precisely the angle from which this project approaches the issue. Methodologically, we aim to build a deep learning architecture based on `BERT`（Bidirectional Encoder Representations from Transformers）. This model can automatically conduct sentiment analysis on user comments, categorizing them as positive, neutral, or negative.

## Table of contents
* [Model Selection and Building](#model-selection-and-building)
* [Data Collection and Preparation](#Data-Collection-and-Preparation)
* [Model Training](#Model-Training)
* [Results Presentation](#Results-presentation)

## Model Selection and Building
We consider to use BERT based models to handle this project.  

### BERT
As the Transformer architecture presented by the paper [Attention is all you need](#Attention-is-all-you-need). You can comprehense BERT is the Encoder of Transformer.
Regarding the pre-training process of BERT, there are two techniques worth to mention, Masked Language Modeling (Masked LM) and Next Sentence Prediction (NSP).

- **Masked Language Modeling（Masked LM）**  
BERT was trained by masking 15% of the tokens with the goal to guess them.  
For example, if there's a sentence:  
That's `[mask]` she `[mask]`.  
BERT model need to be trained to predict the mask tokens which is:  
That's `[mask]` she `[mask]`. -> That's what she said.  

- **Next Sentence Prediction（NSP）**
Given a pair of two sentences, the pre-training process is to help BERT to judge whether or not the second follows the first, which you can realize as a binary classification task.  
For example:  
Input = `[CLS]` That's `[mask]` she `[mask]`. `[SEP]` Haha, that's ridiculous! `[SEP]`  
Label = True（Means the second sentence indeed following the previous one）  
Input = `[CLS]` That's `[mask]` she `[mask]`. `[SEP]` Dwight, you ignorant `[mask]`! `[SEP]`  
Label = False  

In more detail, actually we only extract the vector represent `[CLS]` token to do the prediction, like the picture below.  
<p align="center">
  <img width="450" alt="截圖 2023-09-21 下午4 56 51" src="https://github.com/Shawus/Text_Sentiment_Classification/assets/104006335/bf9b672d-3de3-46b1-a769-d7608098faf1">
</p>  

After repeated fine-tuning processes, it was observed that utilizing the Roberta model led to a slight improvement in prediction performance.  

### BERT vs RoBERTa  
Regarding the dataset used to train the model, RoBERTa was trained on a dataset of 160GB of text, which is more than 10 times larger than the dataset used to train BERT.  
Besides trained on bigger dataset, there are few changes in order to improve the results on BERT architecture.  

- **Removing the Next Sentence Prediction (NSP) objective**  
The NSP method had been explained above, and the authors experimented with removing/adding of NSP loss to different versions and concluded that removing the NSP loss matches or slightly improves downstream task performance.  

- **Dynamically changing the masking pattern**  
In BERT architecture, the masking is performed once during data preprocessing, resulting in a single static mask. Instead, data used to train RoBERTa is duplicated and masked 10 times, each time with a different mask strategy over 40 epochs, thus having 4 epochs with the same mask.

## Data Collection and Preparation  
We extracted the csv file of dataset which is mounted at Google Drive, and make each dataset（train / test / validation） to "DataLoader" type in result.  
The sentence output after using tokenizer function can refer to the picture below.  
<p align="center">
  <img width="800" alt="截圖 2023-10-16 下午5 26 27" src="https://github.com/Shawus/Text_Sentiment_Classification/assets/104006335/842e423f-cf0c-4b08-80bf-c99a11d8ae95">
</p>  

The training set, testing set, and validation set consist of 10248, 2928, and 1461 data rows, respectively.  

## Model Training  
Some of the function and hyperparameter settings are shown in the picture below.  
<p align="center">
  <img width="680" alt="截圖 2023-10-16 下午5 40 06" src="https://github.com/Shawus/Text_Sentiment_Classification/assets/104006335/96d942a3-34cb-4dbf-89fa-b57318605035">
</p>  

## Results Presentation  
After trained in 10 epochs with learning rate decay in steps, the performance of the model are shown below.  

<p align="center">
  <img width="520" alt="截圖 2023-10-16 下午5 36 30" src="https://github.com/Shawus/Text_Sentiment_Classification/assets/104006335/754ae986-e736-4b97-bb73-e3f422544579">
</p>  

During the process of experiment BERT and RoBERTa architecture, we discover that the finetuned RoBERTa model can classify slightly better than BERT model.  

















