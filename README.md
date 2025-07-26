# Multilingual-Retrieval-Augmented-Generation-RAG-System


# Setup Guide:<br>
    1.Open the Notebook 
    2.Install Dependencies 
    3.Upload Your PDF File 
    4.Set Your OpenAI API Key 
    5.Run the RAG Pipeline

# Tools, library and package
  # LangChain Ecosystem
        1. langchain
        2. langchain_community
        3. langchain_core
        4. langchain_openai
  # Used for
        1. Document loading (PyMuPDFLoader)
        2. Chunking (RecursiveCharacterTextSplitter)
        3. Embedding and LLMs (OpenAIEmbeddings, ChatOpenAI)
        4. Vector store (FAISS)
        5. Prompt templates and runnable chains
  #  PDF & OCR Tools
        1. pdf2image – Convert PDF pages to images
        2. pytesseract – OCR text from images (Tesseract)
        3. PyMuPDF (via LangChain) – Load and parse PDF documents
  # Other Python Libraries
        1. os – Environment variable management (for API keys)
        2. re – Regular expressions (text cleaning)
# Sample queries and outputs (Bangla & English)
      main_chain.invoke('বিয়ের সময় কল্যাণীর প্রকৃত বয়স কত ছিল?')
      বিয়ের সময় কল্যাণীর প্রকৃত বয়স পনেরো ছিল।

      main_chain.invoke('হরিশ কোথাই  কাজ করতেন ?')
      হরিশ কানপুরে কাজ করতেন।

      main_chain.invoke('Who is called a good man in Anupams language?')
      The transcript does not provide information on who is called a good man in Anupam's language.

      main_chain.invoke('কন্যাকে আশীর্বাদ করতে কাকে পাঠানো  হইল ?')
      কন্যাকে আশীর্বাদ করতে বিনুদাদা কে পাঠানো হইল।
# Questions


  # 1.	What method or library did you use to extract the text, and why? Did you face any formatting challenges with the PDF content?
      I initially attempted to extract text directly from the PDF using PyMuPDFLoader, but it failed to decode the content correctly.
      This was likely due to the text being encoded in a legacy format like Bijoy rather than standard Unicode. To overcome this 
      limitation, I implemented an alternative approach: first converting the PDF pages to images using pdf2image, then performing 
      OCR on the images with pytesseract to successfully extract the text

  # 2.	What chunking strategy did you choose (e.g. paragraph-based, sentence-based, character limit)? Why do you think it works well for semantic retrieval?
      I implemented a paragraph-based chunking strategy using RecursiveCharacterTextSplitter. This approach optimizes semantic 
      retrieval by intelligently dividing text at hierarchical natural boundaries—paragraphs, lines, sentences, and words—preserving
      contextual coherence within chunks. The structure-aware methodology prevents mid-thought fragmentation, maintains meaningful 
      semantic units, and ensures generated chunks align with embedding model requirements.
  # 3. What embedding model did you use? Why did you choose it? How does it capture the meaning of the text?
      I chose OpenAI's text-embedding-3-small model because it offers the best balance of accuracy, affordability, and speed for 
      understanding text meaning. This tool converts words and sentences into vectors, arranging similar ideas close together in
      a virtual map. For example, it recognizes that 'dog' and 'puppy' are related concepts. Trained on vast amounts of multilingual 
      text, it understands how words connect in different contexts – like knowing 'bark' means something different when talking 
      about trees versus dogs.
  # 4.	How are you comparing the query with your stored chunks? Why did you choose this similarity method and storage setup?
        To compare queries with stored chunks, the system first uses FAISS for high-speed similarity search: it converts the query 
        into an embedding vector via text-embedding-3-small, then scans the vector store to find the top-k chunks with the closest
        cosine similarity to the query vector. This initial candidate set is then refined using Maximal Marginal Relevance (MMR),
        which re-ranks results by balancing semantic relevance against information diversity. I chose this hybrid approach because 
        FAISS delivers millisecond latency at scale—critical for searching large document sets—while MMR overcomes the limitation 
        of naive similarity search . By optimizing for both precision and coverage, this setup ensures retrieved chunks collectively
        address distinct facets of the query, yielding more comprehensive answers in RAG applications.
  # 5.	How do you ensure that the question and the document chunks are compared meaningfully? What would happen if the query is vague or missing context?
        To ensure meaningful comparisons, queries and chunks are converted into the same high-dimensional semantic space using OpenAI's 
        text-embedding-3-small embeddings, which capture contextual relationships beyond keywords. When a query is vague or lacks context,
        the embedding model's deep understanding of linguistic patterns still surfaces topically relevant chunks—but we mitigate ambiguity
        through two key mechanisms: First, MMR diversification ensures retrieved chunks cover multiple semantic facets , compensating for
        query vagueness. Second, low-confidence results trigger interactive clarification, preventing irrelevant retrievals. This dual 
        approach maintains relevance while acknowledging that ambiguous queries require either broader context sampling or user refinement
        to resolve inherent uncertainty.
  # 6.	Do the results seem relevant? If not, what might improve them (e.g. better chunking, better embedding model, larger document)?
        While results show promise, inconsistencies like incorrect answers or "I don't know" responses indicate opportunities for 
        refinement. To improve relevance: first optimize chunking (adjust size/overlap or use semantic-aware splitting); second, 
        test alternative embeddings like LaBSE for multilingual tasks or OpenAI's larger text-embedding-3-large for nuanced Bangla;
        third, expand document coverage with properly decoded Unicode text. If these fail, fine-tune embeddings on domain-specific data. 
      
