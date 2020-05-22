# Exploring_the_type_of_website
'''here, i have combined the web scraping and NLTK to create a model which can visually represent the "what's this about!" of any website (as long as it does want the developers to use their info)'''
# importting all the necesaary modules whenever we will need them
import urllib.request

# now using "request" module, we want to create a request to read the contents of our desired webpage
webpage = urllib.request.urlopen('https://blogs.imf.org/category/global-economy/').read()
# if printed, you will see an html coded content in the webpage, yap, just what we need,
print(webpage)

''' now, that we have the raw html data, we want to polish that by removing unwanted characters like the attributes of
our raw html code. we will do that using BeautifulSoup package.
'''
# I recommend you install the package beforehand if your kernal is fresh as a virgin
pip install BeautifulSoup4
# lets, call the module and use that in our "webpage"
from bs4 import BeautifulSoup
phase1 = BeautifulSoup(webpage)
text = phase1.get_text(strip=True)
# you will notice after printing the text that our html attributes have been wiped out
print(text)
''' cool right! ok, we have a straight plain text, but for future analysis we need something like 'comma separated value'
that's right, since working CSV files and contents are much more fun, so lets dig in by spliting the whole "text"
'''
listed_text = [x for x in text.split()]
print(listed_text)
''' good to see our comma separated values. But! there are words that we are not interested in, like, articles 'a','an','the', common
prepositions, like, 'to', 'at', etc. we don't wanna ruin our final result with those mischiefs, thereby comes the "NLTK" package to the rescue
'''
!pip install nltk
# perticularly we are interested in the stopwords function, because it contains all the common words that we need.
import nltk
nltk.download('stopwords')
from nltk.corpus import stopwords
unwanted_words = stopwords.words('english')
print(unwanted_words)
''' as you can see these are the list of words we are going to avoid in our analysis to get better understanding of the website by 
removing "unwanted_words" from our text
'''
refined_text = listed_text[:]
for item in listed_text:
    if item in unwanted_words:
        refined_text.remove(item)
# we got it!
print(refined_text)
''' to wrap things up quicly we want to count the number of times, iot frequency, each of the words are appearing on the website, then 
create a dictionary like value to give us the words with their frequency.
'''

word_freq = nltk.FreqDist(refined_text)
for word,freq in word_freq.items():
    print(str(word),':',str(freq))

# time to plot and see whats inside

word_freq.plot(20, cumulative=False)

# based on the higher frequent words shown in the plot, basic conclusion can be drawn about the kind of our website 
