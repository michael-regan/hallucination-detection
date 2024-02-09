# hallucination-detection

Data and script (OpenAI API) for the task of Knowledge Base Question Answering (KBQA) **semantic hallucination detection**, a novel task we describe in a recent paper (currently under review).

In a few words, we show that Abstract Meaning Representation (AMR) can help detect erroneous generated SPARQL queries using Large Language Models (LLMs) for multilingual question answering (QA). Both AMR and SPARQL are semantic structures representing the meaning of a question; however, AMR graph coverage is greater than that of SPARQL (for a given knowledge base like DBPedia). Can we use a joint text-to-structure model to generate AMRs and SPARQL queries and examine the ranked n-best list to estimate the model's confidence in one versus the other? Basically, using semantic representations in a multi-task setup for model `interpretability` or `QA confidence estimation`!

A sample prompt: `data/gpt4-prompts-joint-amr-sparql-qald9-no-oracle_2023-10.json` 

Jupyter notebook: `src/gpt4-1106-amr-sparql-joint-parsing-prompts.ipynb` (works with above data)

To get the main idea behind how we define `hallucination` and `hallucination detection` in our experiments, see line 22 in the prompt, or search for the key `sparql_allowed_relations`. This is the part of the prompt where we constrain the LLM to a small set of SPARQL relations; in our case, all relations observed in the QALD-9 training data (Perevalov et al, 2022) mapped to AMRs in a resource created by the amazing IBM team, QALD-9-AMR: 

[(https://github.com/IBM/AMR-annotations)] 

and described in Lee et al (2022): 

[(https://aclanthology.org/2022.naacl-main.393/)].

When we use the full set of relations, LLMs usually behave well (no hallucinations). As soon as we start removing relations from this set, however, LLM **honesty** starts to deteriorate, that is, performance in checking set membership degrades.

The sample generated results (bottom of notebook) show how we can catch GPT-4 red-handed being shifty. This happens in at least **84%** of cases we test with GPT-4 (Oct, 2023), and is nearly just as frequent with GPT-4-1106, what we call `SPARQL semantic hallucination`.