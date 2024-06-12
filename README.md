# Minitutorial LLM+RAG+Ontologies for LDAC 2024

A tiny tutorial for starting to find similarities between building related concepts. 
Created for the Summer School https://linkedbuildingdata.net/ldac2024/ in Bochum, Germany

This is work in progress to pick up some of the work done in DURAARK, particularly the awesome ["Interlink"]( https://github.com/DURAARK/interlink/)  system by @aothms to align different vocabularies like DBPedia, Getty AAT and bSDD.


Back then, "all we had" were some similarity searches like Levenshtein and Jaccard distances to guess the relation between nodes. Let's find out what LLMs can do fo us this time. 

## Help during the workshop
temporary etherpad (will be deleted in 30 days) https://yopad.eu/p/LDAC2024 

## Results

```Python


from langchain_community.llms import Ollama
#question="what is the URL of the material that is created of isocyanate and polyol resin"
question="""using the SKOS vocabulary, creata a turtle triple {subject} {predicate} {object} 
use a SKOS relation that describes the similarity to a subject from another vocabulary in the namespace odb: with a label 'Solid wood' / 'Glue-laminated timber board''
write a valid line usng turtle syntax.
do not write anything else"""
#question="what is the URL of the material that closest fitting to the category 'Wood' / 'Derived timber products' / '3- and 5-ply wood'? Only give the URL and a single digit that indicates how sure you are between 0.0 and 1.0"
ollama = Ollama(
    base_url='http://localhost:11434',
    model="mistral"
)
docs = vectorstore.similarity_search(question)
from langchain.chains import RetrievalQA
qachain=RetrievalQA.from_chain_type(ollama, retriever=vectorstore.as_retriever())
res = qachain.invoke({"query": question})
print(res['result'])
```

which spits out

```
<http://dbpedia.org/resource/Beaverboard> skos:broader odb:Solid_wood .
```

>[!CAUTION]
>This is a clear example of how hallucinations can pollute your data. Beaverboards are not more general than "Solid Wood". If anything this should have been `skos:narrower` . Treat all LLM results with care, calibrate, test with human in the loop!

Which is a valid line in a turtle file. 
Now, enjoy drawing the rest of the owl! 

If you would like to collaborate, get in touch!


IANACS - I am not a computer scientist 


There is a staggering stack of technology and modules involved here most of which I have only a very, very rough understanding if any at all. Take it with a grain of salt, send issuses an PRs


## Understaning Linked Data

To do the DBPedia excersise, some background knowledge is handy.
Use either
- [Pieters LDAC 2024 notebooks ](https://github.com/SSoLDAC2024/handson-querying-and-interaction)
- [LBD notebooks in the ifcopenshell-turial I made for LDAC 2022](https://github.com/jakob-beetz/ifcopenshell-notebooks/tree/ldac-2022)    
- [Mine from LDAC 2019](https://github.com/jakob-beetz/SummerSchoolOfLDAC/)    

## Installing prerequisites

### Conda/Python
Based on Python, so conda/miniconda is what you need. **I highly recommend Mamba. Like Conda, but on speed** go fetch it [here](https://mamba.readthedocs.io/en/latest/installation/micromamba-installation.html)


```
conda env create -f environment.yml
conda activate LDAC2024
```

### LLMs with OLLAMA

Download from [Ollama Github](https://github.com/ollama/ollama)

Download models. Let's go with some smaller ones. The 7 Billion LLama3 should do

```
ollama pull llama3
ollama pull nomic-embed-text
```

If you have a particularly weak machine, you might look into even smaller ones


The langchain-Chroma part has been adapted from [here](https://github.com/ollama/ollama/blob/main/docs/tutorials/langchainpy.md)


