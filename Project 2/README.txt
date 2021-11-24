A) Group Members:-

Aayush Kumar Verma (av2955)
Mehul Goel (mg4260)


B) Files submitted:-

craft.py: Main file
query.py
scrape_url.py
spacy_help_functions.py
spanbert.py
w_bring_relations.py
display.py
log.txt : Transcript of our code for the given parameters


C): 

Run the following commands in the VM:

	pip3 install --upgrade google-api-python-client
	pip3 install beautifulsoup4

	sudo apt-get update
	pip3 install -U pip setuptools wheel
	pip3 install -U spacy
	python3 -m spacy download en_core_web_lg

	git clone https://github.com/gkaramanolakis/SpanBERT
	cd SpanBERT
	pip3 install -r requirements.txt
	bash download_finetuned.sh

To run the code, upload all the .py files to the SpanBERT folder and execute craft.py.  Provide the input parameters when prompted by the code.


D):

1. We take user input for the parameters; k = Number of Relations required.
2. Then we start iterating until 'k' relations are found.
3. We query (query entered by the user) Google with our API Key and Search Engine ID.
4. We keep track of seen URLs so that duplicate URLs do not get processed again.
5. Now, we scrape all the fetched URLs for content using urllib and beautifulsoup.
6. We now pre-process the text using replace() and re.sub() functions and truncate the length of characters to 20000 and extract all relations. 
7. If no relations are extracted, we stop the search and print all the relations found till now.
8. Else, we sort the extracted relations in decreasing order of Confidence Score.
9. If number of extracted relations is greater than or equal to 'k', we stop the search ans display all the relations found till now.
10. Else, we display all the relations extracted till now and move on to the next iteration and continue this process until 'k' relations are extracted.


E):

1. We maintain a list of seen URLs.  If a search result URL is not in this list, we process it.
2. We fetch the content from the URLs now.
    2a. First, we open the page for the URL.
    2b. Then, we extract the plain text (content) of the page using Beautiful Soup.
    2c. Now, we remove special characters, namely '\n', '\t', '\xa0', '\' (single backslash) from the text and then return the text.
    2d. We add the text for the URL in the dictionary that we are maintaining.
3. Now, we truncate the length of characters to 20000 for contents of all URLs.
4. We extract the relations from the content now.
    4a. For every URL, we do the following:
    4b. Load SpaCy and split the text into sentences and get named entity pairs. Simultaneously, we keep displaying the progress after processing every fifth sentence.
    4c. Now, we pass these entity pairs to SpanBERT which then predicts the relation for each entity pair.
5. Now, we print these extracted relations.  Simultaneously, we keep checking for duplicate relations and if we find one, we do not append it to the list of final relations.
6. Finally after we get all the relations, we sort them in decreasing order of Confidence Scores and return the final list of all extracted relations.


F):

Google Custom Search Engine JSON API Key   : ENTER YOUR OWN API KEY HERE
Engine ID                                  : ENTER YOUR OWN ENGINE ID HERE
