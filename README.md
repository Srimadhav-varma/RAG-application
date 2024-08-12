# RAG-application
<strong>The summary of the  work entire flow of the application:</strong><br/> 
<ul>
  <li>Transcribing Indic audio to Indic text using IndicASR.<br/></li> 
  <li>Translating Indic text to english text using IndicTrans2.<br/></li> 
  <li> Loading PDF data into a vector database (FAISS) using LangChain.<br/></li> 
  <li>Querying the vector dataabase for the most similar document.<br/></li> 
  <li>Using Gemini 1.5 API to process the retrieved information from vectorDB according to the user's query.<br/></li> 
  <li>Translating english information back to origin language using IndicTrans2.<br/></li> 
  <li>Converting the processed information to audio in original language using IndicTTS.<br/></li> 
</ul>
<strong>Work flow of an end-to-end RAG powered voice assistant</strong>
<img width="1390" alt="Rag workflow" src="https://github.com/user-attachments/assets/96296727-d7f4-4f94-a0df-55c35d481409">
The code is modified to use the free version of gemini api instead of openAI's API.<br/>
<strong>Install dependencies</strong><br/>
Use the first 4 cells to install all the dependencies needed for the application.<br/>
<strong>Restart session</strong><br/>
In order for some library imports to take effect, we will need to restart the session.<br/>
<em>WARNING: The cells below might lead to import errors if the session is not restarted.</em><br/>
<strong>Creating a Vector database using FAISS + langchain<strong/>






