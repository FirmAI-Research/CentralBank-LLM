# **CentralBank-LLM (ChatGPT, Langchain)**
Discover what top institutions <ins>**really think</ins>** about current economic conditions (price inflation, unemployment, interest rates). Plotly Dash App is built to extract main highlights and query latest central banking publications from:
- [Federal Reserve](https://www.federalreserve.gov/monetarypolicy.htm)
- [Bank of England](https://www.bankofengland.co.uk/monetary-policy-report/monetary-policy-report)
- [European Central Bank](https://www.ecb.europa.eu/pub/economic-bulletin/html/index.en.html)

The RAG-LLM retriever is trained to answer user prompts and generate plots **<ins>exclusively using textual data downloaded directly from central banks<ins>**. 
It also provides citations to the original publications (source, author, and page information).

## **Tutorial:**
![](https://github.com/viczommers/CentralBank-LLM/blob/main/Tutorial.gif)
### **How to install & run the app**:
1. ```git clone https://github.com/viczommers/CentralBank-LLM.git```
2.  change directory to cloned repository ```cd your-folder-path```
3.  ```pip install -r requirements.txt ```
4.  ```python app.py ```
5. open the app at http://localhost:8050/


## **Notes & Known Issues:**
### Langchain Chroma vectorstore deletion
ChromaDB is known to have issues in Streamlit/Dash apps when trying to delete collections or reset the database (if ChromaDB was used in a running callback). This behaviour results in new embeddings being appended to old db. Every Vectordb instance including as.retriever() needs to be set to None i.e. connection to Chroma terminated for the files to be released for deletion, once vectordb instance is set to None there is no way to reinitiate the database connection. This was somewhat addressed in old `delete_downloads()` function:
```python
    if os.path.exists(persist_directory):
        shutil.rmtree(persist_directory, ignore_errors=True)
```
However, *chroma.sqlite3* file will still exist and requires a Dash app restart to terminate the callback and release the file for deletion (or it can be deleted manually).
The `langchain.vectorstores` module does not include functionality to perform `client.reset()` or track the collection, therefore I suggest using the `chromadb` library directly for similar projects. Those are the main reasons why webscraping, text splitting and embedding should be done using FastAPI backend server in MongoDB or Pinecone, and not locally.
### 400 Bad Request:
Frequent requests to Bank of England server will sometimes return less files than actually available due to cooldown. This can be remediated by changing IP Address or modifying the for-loop to request only reports from February, May, August, November (at expense of poentially missing emergency publications).

## [**Book a Demo | Order a Custom Dashboard**](mailto:vic.dashboards@icloud.com?subject=[GitHub]%20LLM%20Dashboard)
Interested in adding NLP tools to your proprietary strategy? Get your own pocket macro strategist today - I offer boutique services for building tailored dashboards and JSON FastAPIs from custom data sources (local virtual machine or private cloud).

## **Disclosures:**
THIS DOCUMENT DOES NOT CONTAIN INSIDE INFORMATION FOR THE PURPOSES OF ARTICLE 7 OF THE MARKET ABUSE REGULATION (EU) 596/2014 (“MAR”).
THE MATERIAL GENERATED BY THE APP IS PROVIDED FOR INFORMATIONAL PURPOSES ONLY AND SHOULD NOT BE CONSTRUED AS INVESTMENT ADVICE OR SOLICITATION TO BUY OR SELL SECURITIES. 
THE MATERIAL IS NOT INTENDED TO BE USED AS A GENERAL GUIDE TO INVESTING, OR AS A SOURCE OF ANY SPECIFIC INVESTMENT RECOMMENDATIONS, AND MAKES NO IMPLIED OR EXPRESS RECOMMENDATIONS CONCERNING THE MANNER IN WHICH ANY CLIENT’S ACCOUNT SHOULD BE HANDLED.
