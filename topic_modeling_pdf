from pdfminer.pdfinterp import PDFResourceManager, PDFPageInterpreter
from pdfminer.pdfpage import PDFPage
from pdfminer.converter import TextConverter
from pdfminer.layout import LAParams

from io import StringIO

def pdf_to_text(pdfname):

    # PDFMiner boilerplate
    rsrcmgr = PDFResourceManager()
    sio = StringIO()
    codec = 'utf-8'
    laparams = LAParams()
    device = TextConverter(rsrcmgr, sio, laparams=laparams)
    interpreter = PDFPageInterpreter(rsrcmgr, device)

    # Extract text
    fp = open(pdfname, 'rb')
    for page in PDFPage.get_pages(fp):
            interpreter.process_page(page)
    fp.close()

    # Get text from StringIO
    text = sio.getvalue()

    # Cleanup
    device.close()
    sio.close()

    return text


handbook_string = pdf_to_text('Vendor Handbook 2015 FINAL (3).pdf')
clean_chars = [",", ".", "'", ";", "\n"]

def clean_text(text_string, special_characters):
    cleaned_string = text_string
    cleaned_string = cleaned_string.lower()
    for string in special_characters:
        cleaned_string = cleaned_string.replace(string, "")
    cleaned_string = cleaned_string.lower()
    return(cleaned_string)

def tokenize(text_string, special_characters):
    cleaned_handbook = clean_text(handbook_string, clean_chars)
    handbook_tokens = cleaned_handbook.split(" ")
    return(handbook_tokens)

tokenized_handbook = tokenize(handbook_string, clean_chars)


# #### Remove Stop Words 


# remove stop words
from stop_words import get_stop_words
# create English stop words list
en_stop = get_stop_words('en')

# remove stop words from tokens
stopped_tokenized_handbook = [i for i in tokenized_handbook if not i in en_stop]


# #### Stemming the Words



from nltk.stem.porter import PorterStemmer

# Create p_stemmer of class PorterStemmer
p_stemmer = PorterStemmer()


# stem token
stemmed_stopped_tokenized_handbook = [p_stemmer.stem(i) for i 
                                      in stopped_tokenized_handbook]
texts = stemmed_stopped_tokenized_handbook


# from collections import Counter
# import operator
# from pprint import pprint
# 
# counts = dict(Counter(texts))
# sorted_counts =sorted(counts.items(), key=operator.itemgetter(1),reverse=True)
# pprint(sorted_counts[:30])

# #### Constructing a document-term matrix


from gensim import corpora, models
import gensim

dictionary = corpora.Dictionary([texts])



# convert the dictionary into a bag-of-words
corpus = [dictionary.doc2bow([text]) for text in texts]


# #### Applying the LDA model


ldamodel = gensim.models.ldamodel.LdaModel(corpus, num_topics=5, 
                                           id2word = dictionary, passes=20)


# #### Examining the results


print(ldamodel.print_topics(num_topics=5, num_words=3))





