# Changes in European Solidarity Before and During COVID-19: Evidence from a Large Crowd- and Expert-Annotated Twitter Dataset

Data and code for our paper

```
@inproceedings{ils-etal-2021-changes,
    title = "Changes in {E}uropean Solidarity Before and During {COVID}-19: Evidence from a Large Crowd- and Expert-Annotated {T}witter Dataset",
    author = "Ils, Alexandra  and
      Liu, Dan  and
      Grunow, Daniela  and
      Eger, Steffen",
    booktitle = "Proceedings of the 59th Annual Meeting of the Association for Computational Linguistics and the 11th International Joint Conference on Natural Language Processing (Volume 1: Long Papers)",
    month = aug,
    year = "2021",
    address = "Online",
    publisher = "Association for Computational Linguistics",
    url = "https://aclanthology.org/2021.acl-long.129",
    doi = "10.18653/v1/2021.acl-long.129",
    pages = "1623--1637",
}
```


## Getting Started


* You can crawl tweets with codes in the folder /tweets_crawl, but please not that not all dependencies for tweet crawling are 
listed in the requirements.txt. 
    *  Get access to Tweepy (The free one is enough)
    *  Unzip GetOldTweets3-20211011T120612Z-001.zip (This is the GetOldTweets3 I used in tweets_crawling.py, please don't install it with pip, because I made some modifications on the basis of the offical one)
    *  Put your hashtags in Hashtags.csv
    *  Change the value of 'year' (e.g. 2021)in tweets_crawling.py, so you can crawl tweets containing your wanted hashtags in 2021
    *  Fill in consumerKey, consumerSecret, accessToken, accessTokenSecret in tweets_crawling.py
    *  If you use Windows, there might be some path issues, just change the path separator according to your own OS



The labels in the dataset mean: 0 is solidarity, 1 is anti-solidarity, 2 is ambivalent and 3 is not applicable.

* You can view the dataset we used in our experiments [here](https://drive.google.com/drive/folders/1zVMPJtQBidFZ9QokR58rjXkibfyVANaG?usp=sharing). Please note that for the sake of privacy, we have omitted the content of the tweets, but we have included the tweet ids, you can crawl tweets with these ids.

## Usage
- An example of further pre_training using masked language modeling task and next sentence prediction task
```python
# train_corpus: path where saves the trained corpus in text file
# output_dir: directory to save the output
# do_lower_case:  whether lowercase text before tokenization
# epochs_to_generate: number of epochs to train for
# max_seq_len: maxinum sequence length

>>> python pregenerate_training_data.py --train_corpus  tweets_LM_6k.txt  --output_dir training_6k/ --do_lower_case --epochs_to_generate 20 --max_seq_len 150

# pregenerated_data: directory where saves the output of pregenerate_training_data.py 
# train_batch_size: batch size for training
# do_lower_case: whether lowercase text before tokenization
# epochs: number of epochs to train for

>>> python pre_training_mlm.py --pregenerated_data training_6k/   --train_batch_size 16  --do_lower_case --output_dir fine_tune/finetuned_lm_6k/ --epochs 20
```
- An example of further pre_training using sentiment classification
```python
# model: you can choose a model name from [bert, xlm]
# weights: you can choose pretrained weights from huggingface transformers ('bert-base-multilingual-cased'or 'xlm-roberta-base'), or self-trained weights
# optional:  
# --data_path: path of the data for sentiment classification  
# --output_dir: directory to save the model 

>>> python pre_training_sentiment_classification.py --model_type bert --pretrained_weights bert-base-multilingual-cased
```
- An example of training
```python
# model_type: you can choose a model from [bert, xlm]
# pretrained_weights: you can choose a pretrained weights from huggingface transformers ('bert-base-multilingual-cased' or 'xlm-roberta-base'), or self-trained weights
# optional:
# --model_path：path where saves the model  
# --oversample_from_train: whether do oversampling from training data  
# --translation: whether add translated data for training  
# --auto_data: whether add auto-labeled data for training  


>>> python train.py --model_type xlm --pretrained_weights xlm-roberta-base --translation --auto_data 
``` 
- An example of predicting
```python
# model_dir：directory where saves models  
# optional:
# --model_name：name of the model,for single model prediction  
# --data_dir： directory where saves tweets to be predicted 
# --output_dir： directory to save the prediction results
# --num_labels： the number of classes 
# --do_lower_case： whether lowercase text before tokenization

>>> python predict.py --model_dir saved_weights --model_name xlm_pytorch_model.bin --data_dir twitter_data --do_lower_case
``` 



