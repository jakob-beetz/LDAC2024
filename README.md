# Minitutorial LLM+RAG+Ontologies for LDAC 2024

A tiny tutorial for starting to find similarities between building related concepts. 
Created for the Summer School https://linkedbuildingdata.net/ldac2024/ in Bochum, Germany

This is work in progress to pick up some of the work done in DURAARK, particularly the awesome ["Interlink"]( https://github.com/DURAARK/interlink/)  system by @aothms to align different vocabularies like DBPedia, Getty AAT and bSDD.

Back then, "all we had" where some similarity searches like Levenshtein and Jaccard distances to guess the similarities between nodes. Let's find out what LLMs can do fo us this time. 


IANACS - I am not a computer scientist 

There is a staggering stack of technology and modules involved here most of which I have only a very, very rough understanding if any at all. Take it with a grain of salt, send issuses an PRs


The langchain-Chroma part has been adapted from https://github.com/ollama/ollama/blob/main/docs/tutorials/langchainpy.md
