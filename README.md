# RAG-application
<strong>The summary of the entire work flow of the application:</strong><br/> 
<ul>
  <li>Transcribing Indic audio to Indic text using IndicASR.<br/></li> 
  <li>Translating Indic text to english text using IndicTrans2.<br/></li> 
  <li> Loading PDF data into a vector database (FAISS) using LangChain.<br/></li> 
  <li>Querying the vector dataabase for the most similar document.<br/></li> 
  <li>Using Gemini 1.5 API to process the retrieved information from vectorDB according to the user's query.<br/></li> 
  <li>Translating english information back to origin language using IndicTrans2.<br/></li> 
  <li>Converting the processed information to audio in original language using IndicTTS.<br/></li> 
</ul>
<strong>Work flow of an end-to-end RAG powered voice assistant:</strong>
<img width="1390" alt="Rag workflow" src="https://github.com/user-attachments/assets/96296727-d7f4-4f94-a0df-55c35d481409">
The code is modified to use the free version of gemini api instead of openAI's API.<br/>
<strong>Install dependencies:</strong><br/>
Use the first 4 cells to install all the dependencies needed for the application.<br/>
<strong>Restart the session:</strong><br/>
In order for some library imports to take effect, we will need to restart the session.<br/>
<em>WARNING: The cells below might lead to import errors if the session is not restarted.</em><br/>
<strong>Creating a Vector database using FAISS + langchain:</strong><br/>

![Creating a Vector database using FAISS + langchain](https://github.com/user-attachments/assets/74e3854d-8794-4fe6-beba-3766568a1187)

<strong>Vector DB in a nutshell:</strong><br/>

<ul>
  <li>First, we use an embedding model to create vector embeddings for the text.<br/></li> 
  <li>Then the vector embeddings are inserted into the vector database, with a reference to the original text.<br/></li>  
  <li>When the application receives a query, we use the embedding model to create embeddings for the query and use those embeddings to query the database for similar vector embeddings.<br/></li> 
  <li>The most similar embeddings are extracted from the Database.<br/></li> 
</ul>

<strong>Here, we will use FAISS (Facebook AI Similarity Search) to build a vector DB initialized from a PDF document containing all details of the PM-Kisan Yojna:</strong><br/>
<ul>
    <li>We use Langchain's functions for initializing our vector DB, reading a PDF document directly and for using an embedding model from HuggingFace to embed our PDF text.</li> 
    <li>Langchain is an Open-source framework designed to integrate LLMs into applications, utilizing the powerful capabiltiies of langchain wrappers.</li>  
    <li>FAISS is an open-source vector database by Meta AI, to store embeddings of text at a scale and process queries in miliseconds.</li></ul>

