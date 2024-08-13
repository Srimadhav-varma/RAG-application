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
<br/>
<strong>Install dependencies:</strong><br/>
Use the first 4 cells to install all the dependencies needed for the application.<br/>
<br/>
<strong>General libraries installation</strong><br/>
We are going to install:<br/>
<ul>
  <li>Langchain for prompt templates, building our Vector DB and generating embeddings from a HuggingFace model.</li>
  <li>FAISS for vector database.</li>
  <li>PyTorch for Automatic Speech Recognition.</li>
  <li>PyPDF for processing PDF data.</li>
  <li>Gradio for application development.</li>
</ul>
<strong>Install IndicASR dependencies</strong><br/>
<img width="873" alt="download" src="https://github.com/user-attachments/assets/06c88708-45cd-4925-b7e1-bdf7ab604c5e"><br/>
Reference: https://www.slideshare.net/slideshow/build-your-own-asr-engine/117762678#4<br/>
<br/>
Working of an Automatic Speech Recognition (ASR) Model:<br/>
<ul>
  <li><strong>Audio Input: </strong>The process starts when a microphone captures the sound of your voice and converts it into a digital audio signal. This digital signal is essentially a wave form that represents the variations in air pressure created by your speech.</li>
  <li><strong>Preprocessing: </strong>The digital audio signal is cleaned up to remove background noise and other distortions. This step often involves techniques like filtering and normalization to ensure the signal is clear and consistent.</li>
  <li><strong>Feature Extraction: </strong>The cleaned audio signal is analyzed to extract key features that are important for recognizing speech. This typically involves breaking the audio into small chunks (called frames) and analyzing these chunks for patterns.</li>
  <li><strong>Acoustic Modeling: </strong>The extracted features are then compared against acoustic models, which are statistical representations of different speech sounds (phonemes). These models have been trained on large datasets of recorded speech and corresponding transcriptions, allowing the system to predict which phonemes match the features of the audio signal.</li>
  <li><strong>Language Modeling and Decoding: </strong>Finally, the recognized phonemes are put together using a language model that understands the probabilities of different word sequences. This helps in forming coherent and grammatically correct sentences. The system then decodes the best match for the spoken input, converting the series of phonemes into a text output that represents what was said.</li>
</ul>
We will be using the NeMo framework released by NVIDIA for using the IndicASR models.<br/>
Reference: https://docs.nvidia.com/nemo-framework/user-guide/latest/overview.html<br/>
<br/>
<strong>Install IndicTrans2 dependencies</strong><br/>
Training a Machine Translation Model involves mainly 3 steps:<br/>
<ul>
  <li><strong>Data Collection: </strong>First, we gather a large amount of parallel texts, which are sentences translated from one language to another. For example, a sentence in English and its corresponding translation in Spanish.</li>
  <li><strong>Learning Language Patterns: </strong>The model examines these pairs of sentences to understand the patterns and rules of how words and phrases in one language correspond to those in another language.</li>
  <li><strong>Training with Pairs: </strong>During training, the model learns by looking at many pairs of sentences (source and target). It tries to minimize the difference between the predicted translation and the actual translation. This is done using a technique called backpropagation, where the model adjusts its parameters to improve accuracy.</li>
</ul>

![IndicTrans2 dependencies](https://github.com/user-attachments/assets/bec4e3eb-c74f-475f-bb38-23e623d6788d)<br/>
<br/>
IndicTrans2 Supported list of languages along with language codes:<br/>
<br>


<table>
<tbody>
  <tr>
    <td>Assamese (asm_Beng)</td>
    <td>Kashmiri (Arabic) (kas_Arab)</td>
    <td>Punjabi (pan_Guru)</td>
  </tr>
  <tr>
    <td>Bengali (ben_Beng)</td>
    <td>Kashmiri (Devanagari) (kas_Deva)</td>
    <td>Sanskrit (san_Deva)</td>
  </tr>
  <tr>
    <td>Bodo (brx_Deva)</td>
    <td>Maithili (mai_Deva)</td>
    <td>Santali (sat_Olck)</td>
  </tr>
  <tr>
    <td>Dogri (doi_Deva)</td>
    <td>Malayalam (mal_Mlym)</td>
    <td>Sindhi (Arabic) (snd_Arab)</td>
  </tr>
  <tr>
    <td>English (eng_Latn)</td>
    <td>Marathi (mar_Deva)</td>
    <td>Sindhi (Devanagari) (snd_Deva)</td>
  </tr>
  <tr>
    <td>Konkani (gom_Deva)</td>
    <td>Manipuri (Bengali) (mni_Beng)</td>
    <td>Tamil (tam_Taml)</td>
  </tr>
  <tr>
    <td>Gujarati (guj_Gujr)</td>
    <td>Manipuri (Meitei) (mni_Mtei)</td>
    <td>Telugu (tel_Telu)</td>
  </tr>
  <tr>
    <td>Hindi (hin_Deva)</td>
    <td>Nepali (npi_Deva)</td>
    <td>Urdu (urd_Arab)</td>
  </tr>
  <tr>
    <td>Kannada (kan_Knda)</td>
    <td>Odia (ory_Orya)</td>
    <td></td>
  </tr>
</tbody>
</table>
<br/>
<strong>Install IndicTTS dependencies</strong><br/>
Basic components of a TTS (Text-to-Speech) Model:<br/>
<ul>
  <li><strong>Text Input: </strong>The system receives a text input, which can be a sentence, paragraph, or any other form of written language.</li>
  <li><strong>Text Analysis: </strong>This stage breaks down the written text into its basic components. This may involve tasks like splitting sentences into words, identifying parts of speech, and performing other forms of linguistic analysis.</li>
  <li><strong>Linguistic Features Extraction: </strong>Here, the system extracts features from the analyzed text that are relevant to speech production. These features might include things like phoneme identities (the basic units of speech), stress patterns, and intonation.</li>
  <li><strong>Acoustic Model: </strong>This component uses the linguistic features to predict the acoustic features of speech. Acoustic features include things like pitch, volume, and spectral envelope (the frequency distribution of the sound).</li>
  <li><strong>Vocoder: </strong>Finally, the vocoder uses the predicted acoustic features to generate an audio waveform that corresponds to the spoken version of the input text.</li>
</ul>

<img width="1021" alt="IndicTTS dependencies" src="https://github.com/user-attachments/assets/083f96d3-02e5-41ca-9cdb-7950e33c8715"><br/>

<strong>Restart the session:</strong><br/>
In order for some library imports to take effect, we will need to restart the session.<br/>
<em>WARNING: The cells might lead to import errors if the session is not restarted.</em><br/>
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
    <li>FAISS is an open-source vector database by Meta AI, to store embeddings of text at a scale and process queries in miliseconds.</li>
</ul>
