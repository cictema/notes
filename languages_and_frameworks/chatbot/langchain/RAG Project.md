
## Steps 
- Set up the environment and import the relevant libraries like `google.generativeai`, `pathlib`, `textwrap` and `IPython`.
- Start with asking questions to the Gemini Pro model and save the chat history for the conversation.
- Experiment with the different configuration parameters like `temperature`, `maximum output tokens`, `top_k`, `top_p`, and `candidate_count`.
- Evaluate the quality of the responses.
- Perform retrieval-augmented generation to improve the quality of responses (on a particular topic) using a document, website, or any knowledge corpus of your choice.

## Import
- `generativeai`**:** This is used for AI-powered applications using Google’s Generative AI models like the Gemini Pro.
- `genai`**:** This is used to create the model object using the `GenerativeModel()` function with `gemini-pro` as the parameter (model).
- `textwrap`**:** This library is used to create a function that will show the result in a systematic indented format.
- `IPython`**:** This library is used in Jupyter Notebooks to display different kinds of outputs that are beyond simple text, e.g., images, audio, video, HTML, etc.
- `Markdown`**:** This library converts the text into the Markdown format.

Use the LangChain library to import the following functions for retrieval-augmented generation (RAG):
- `PyPDFLoader`**:** The `langchain_community` library has a lot of document loaders. The `PyPDFLoader` function is used to load text content from PDF files.
- `RecursiveCharacterTextSplitter`**:** LangChain uses this function to split the documents into smaller chunks for efficient processing.
- `GoogleGenerativeAIEmbeddings`**:** This function integrates Google’s embedding models with LangChain. Embeddings convert text into numbers that capture semantic meaning, making them suitable for tasks like similarity search in RAG.
- `ChatGoogleGenerativeAI`**:** This function is an extension of LangChain that helps interact with Google’s generative AI chat models, such as the Gemini Pro model.
- `Chroma`**:** This is a vector database designed for faster and more efficient similarity search.
- `RetrievalQA`**:** This function is tailored to build question-answering systems using RAG. It combines retrieval (finding relevant context) with generation (generating an answer based on the retrieved context).

Use the API key inside the `genai.configure()` for the model to work.

Instantiate a Google Generative AI object using the `gemini-pro` model.


## Get Conversation History While Prompting Gemini
1. Use the model to start the chat.
```python
#Start chat and store it in the history

hist = model.start_chat()
```
2. Ask a query to the model and generate the response text.
```python
#Get Response
response = hist.send_message("Hi! Proivide a recipe to make a margeritta pizza from scratch")

#Get Markdown Output
Markdown(response.text)
```
3. Iterate over the history to print each sentence of the conversation. This way, you can isolate and find a specific sentence for observation.
```python
#Get all items from history
#Contains parts, and role objects
#role can be user or model
for item in hist.history:
	print(item)
	print("\n\n")

#First object in item.parts
item.parts[0].text
```
4. Find and print the number of tokens in the query.
```python
# Count the number of tokens
model.count_tokens("Now provide the location of the nearest supermarket where I can buy the ingredients from.")
```

## Configuration Parameters
### `temperature`
- The `temperature` parameter in the Gemini Pro model is imperative in the response generation process. It is used during the sampling phase, mainly when the `top_p` and `top_k` parameters are being used. `temperature` influences the randomness in token selection:
- Low temperatures are best for prompts that demand to-the-point, accurate, and concise responses. For example, if you ask what is the capital of France, there is only _one_ answer, or when you ask to compute the square root of 16.
- High temperatures lead to diverse and creative outcomes, such as generating a roadmap for learning machine learning or writing a poem about AI. You’ll get varied responses, which you can also customize by asking specific questions about them.

```python
genai.types.GenerationConfig(temperature= <parameter_value>)
```

#### Range of Values:
- The range of values for the temperature parameter is 0.0–1.0.
- Its default value is 0.9 in `gemini-pro` and 0.4 in `gemini-pro-vision`.

```python
def get_response(prompt, generation_config={}):
    response = model.generate_content(contents=prompt, 
    generation_config=generation_config)
    return response

for temp in [0.0, 0.25, 0.5, 0.75, 1.0]:
  config = genai.types.GenerationConfig(temperature=temp)
  result = get_response("Explain the concepts of XGBoost and Random Forest with real-life use cases", generation_config=config)
  print(f"\n\nFor temperature value {temp}, the results are: \n\n")
  display(Markdown(result.text))
```

###  `max_output_tokens`
- The parameter `max_output_tokens` is the upper limit of tokens generated in a response. Lower output token values lead to shorter responses, while higher token values give longer responses. 
- A token approximates to four characters, translating to about 60–80 words for 100 tokens. You can adjust the `max_output_tokens` parameter based on the required response length.

```python
genai.types.GenerationConfig(max_output_tokens= <parameter_value>)
```

#### Range of Values
- The range of values for `max_output_tokens` is 1–8192 for `gemini-pro` model, and the default value is 8192.
- Similarly, the range of values for the `gemini-pro-vision` model is 1–2048, and the default value is 2048.


```python

def get_response(prompt, generation_config={}):
    response = model.generate_content(contents=prompt, generation_config=generation_config)
    return response

for max_out_tok in [1, 50, 100, 150, 200]:
    config = genai.types.GenerationConfig(max_output_tokens=max_out_tok)
    result = get_response("Explain the concepts of XGBoost and Random Forest with real-life use cases", generation_config=config)
    print(f"\n\nFor max output token value {temp}, the results are: \n\n")
    display(Markdown(result.text))```


###  `top_k`
- The `top_k` parameter is a measure of how many of the most probable tokens are considered at each step. 
- It affects the model’s token selection strategy for generating outputs. 
- A higher `top_k` value increases the diversity, leading to more creative responses. 
- A `top_k` of 1 implies a deterministic approach, choosing the most likely token.

```python
config = genai.types.GenerationConfig(top_k=k)
```

#### Range of Values
- The range of values for the `top_k` parameter is 1–40.
- Its default value is unspecified in `gemini-pro` and 32 in `gemini-pro-vision` model.

```python
def get_response(prompt, generation_config={}):
    response = model.generate_content(contents=prompt, 
    generation_config=generation_config)
    return response

for k in [1, 4, 16, 32, 40]:
    config = genai.types.GenerationConfig(top_k=k)
    result = get_response("Explain the concepts of XGBoost and Random Forest with real-life use cases", generation_config=config)
    print(f"\n\nFor top k value {temp}, the results are: \n\n")
    display(Markdown(result.text))
```





###  `top_p`
- The `top_p` parameter controls how the AI model chooses words when generating text. 
- It looks at words from most to least likely, adding up their probabilities until reaching the `top_p` value. 
- Then, it picks the next word from this group, with some influence from the `temperature` parameter. 
- A lower `top_p` produces more focused and predictable text, while a higher `top_p` results in less predictable answers.
```python
config = genai.types.GenerationConfig(top_p=p)
```

#### Range of Values
- The range of values for the `top_p` parameter is 0.0–1.0.
- The default value is 1.0.


```python
def get_response(prompt, generation_config={}):
    response = model.generate_content(contents=prompt, 
    generation_config=generation_config)
    return response

for p in [0, 0.2, 0.4, 0.8, 1]:
    config = genai.types.GenerationConfig(top_p=p)
    result = get_response("Explain the concepts of XGBoost and Random Forest with real-life use cases", generation_config=config)
    print(f"\n\nFor top p value {temp}, the results are: \n\n")
    display(Markdown(result.text))
```


###  `candidate_count`
- The `candidate_count` config parameter determines the number of potential responses the model generates internally before selecting the best one to present. 
- A higher value of `candidate_count` means the model explores more possibilities, thus leading to more creative or diverse responses. 
- However, this also increases the computational resources required for generation.
- You can only set `candidate_count` to 1 for the `config` parameter since the Gemini Pro model is designed to focus on generating the single best possible response rather than creating a variety of options. This could be due to the underlying architecture or training data used for the model.

```python
genai.types.GenerationConfig(candidate_count = <parameter_value>)
```

```python
config = genai.types.GenerationConfig(candidate_count=1)

result = get_response("Explain the concepts of XGBoost and Random Forest with real-life use cases", generation_config=config)

Markdown(result.text)
```


## Retrieval Augmented Generation
**Retrieval-augmented generation (RAG)**, is where a model extracts specific information to generate more relevant responses. The source of information can be a corpus, a document, a database, a website, or anything related.

There are some strong advantages of using RAG, like:
- Enhanced contextual responses, increasing the LLM capabilities.
- Retrieval of up-to-date information since it retrieves information from the website, document, or corpus.
- Reduced hallucinations, which is a problem with the LLMs.

## Langchain + RAG
LangChain’s modular design helps integrate various document loaders, embedding models, vector stores, and LLM providers. In the coming tasks, you’ll use the tools provided by LangChain in the order specified below:
1. Load the PDF document (from where you will retrieve the specific data) using the `PyPDFLoader`.
2. Extract the texts using `RecursiveCharacterTextSplitter` to have meaningful chunks of text for further processing.
3. Use `GoogleGenerativeAIEmbeddings` to generate the embeddings for the extracted texts that will be used for similarity search in RAG.
4. Use `Chroma` (a vector database) to store the created embeddings so that they can be used to retrieve the relevant information later when needed.
5. Use the `RetrievalQA` function to build the question-answering system for retrieval (finding relevant context) with generation (generating an answer based on the retrieved context). This function will use `ChatGoogleGenerativeAI` with the Gemini Pro model to generate responses from the retrieved context.

## Load the PDF and Extract the Text
1. Create a text splitter using the function `RecursiveCharacterTextSplitter()` to divide the text into chunks of a specific size and with some overlap.
	- The chunk size sets the maximum desired length of each extracted text. It is better to keep the chunk size between 500 and 1000 characters to have better retrieval accuracy, lesser computational costs, and also to keep it within the models’ limits.
	- Chunk overlap is the number of characters that overlap between consecutive chunks. Overlapping text chunks helps to retain context between the adjoining text chunks, where the answer is connected to the nearby text chunks. Here, you’ll keep it between 50 and 100 characters.
3. Combine the content of all the pages into a single string.
4. Create the text splitter to break this large string into smaller, manageable pieces.

#### Steps:
- Use the `PyPDFLoader()` function to load the data.
```python
CHUNK_SIZE = 700

CHUNK_OVERLAP = 100

pdf_path = "https://www.analytixlabs.co.in/assets/pdfs/Data_Engineering%20&_Other_Job_Roles-AnalytixLabs.pdf"

pdf_loader = PyPDFLoader(pdf_path)
```
- Use the `load_and_split()` function to split the data.
```python
split_pdf_document = pdf_loader.load_and_split()
```
- Use the `RecursiveCharacterTextSplitter()` to create a text-splitter. This function takes the following parameters:
    - `chunk_size`**:** This defines the maximum length of each text chunk.
    - `chunk_overlap`**:** This defines the number of overlapping characters between consecutive chunks.
```python
text_splitter = RecursiveCharacterTextSplitter(chunk_size=CHUNK_SIZE, chunk_overlap=CHUNK_OVERLAP)

context = "\n\n".join(str(p.page_content) for p in split_pdf_document)

texts = text_splitter.split_text(context)
```

## Gemini Model and Embeddings
-  Use the `ChatGoogleGenerativeAI()` function to instantiate a Gemini Pro model. Pass appropriate values for `google_api_key` and the `temperature` parameters.
```python
gemini_model = ChatGoogleGenerativeAI(model='gemini-pro', google_api_key="", temperature=0.8)
```

- For creating the embeddings, use the `GoogleGenerativeAIEmbeddings()` function to instantiate the `models/embedding-001` model (also pass the `google_api_key`).
```python
embeddings = GoogleGenerativeAIEmbeddings(model="models/embedding-001", google_api_key="")
```
- Use the `Chroma()` function with the split text and the embedding model to generate the embeddings. The texts will be converted into dense vector representations. These vectors capture the semantic meaning of each text chunk and will be used to find similarities between queries and the texts.
```python
vector_index = Chroma.from_texts(texts, embeddings)
```
- Transform the Chroma index into a retriever object to retrieve texts for the questions asked. Pass `"search_kwargs={"k": 5}"` as the parameter to return the top 5 most similar documents or chunks to your queries.
```python
retriever = vector_index.as_retriever(search_kwargs={"k" : 5})
```

## RAG Question Answering Chain

- Use the `RetrievalQA` library to create the RAG question answering chain.
    - `RetrievalQA.from_chain_type()` function to create the RAG question answering chain. This function will take the following parameters:
	- The language model used for answering questions.
	- `retriever`**:** The retriever used for fetching relevant documents.
	- `return_source_documents`**:** The flag to include source documents in the result.
```python
qa_chain = RetrievalQA.from_chain_type(gemini_model, retriever=retriever, return_source_documents=True)
```
- After the question answering chain is created, ask the question where the question is a value to the key `"query"` in a dictionary format.
	- Use the `qa_chain()` function to get the response specific to the document (using a query).
```python
# Example usage 
question = "Which tools do Data Engineers primarily work with?"
result = qa_chain.invoke({"query": question})
print("Answer:", result["result"])
```