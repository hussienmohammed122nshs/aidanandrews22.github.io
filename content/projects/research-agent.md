Built a system that uses both local and cloud LLMs to generate high-quality reports efficiently; the architecture emphasizes modularity and concurrency to ensure scalability — pretty much exactly what you'd want in a production environment. I built this for research that I do on a day-to-day basis and to run on my local computer which uses a 4090 so a lot of the code is getting around the resource constraints, but you can use all api's if you want. Simple booleans control which models are being used (uses 2 at a time).

## Core Components
1. **Model Setup**
- Using LocalDeepSeekLLM for local inference and GPT-4 in the cloud
- Writer module leverages Claude 3.5 Sonnet for generation
- Implemented GPU optimizations (4-bit quantization, Flash Attention 2) to maximize performance
- dynamic memory allocation with CUDA cache management

2. **Search System**
- Multi-source search integrating DuckDuckGo (and Tavily if needed)
- deduplication using URL normalization
- Built an LLM-powered ranking system to prioritize results
- Concurrent fetching with rate limiting to prevent overload

3. **Memory & Processing**
- Proactive CUDA cache clearing before LLM calls
- Semaphore-based control to prevent OOM errors
- Chunked processing for large docs (max 4000 char contexts)
- Batch processing with early stopping when quality threshold is met

**The system's strength comes from its hybrid approach:**
- Local model handles planning/ranking (faster + cheaper)
- Cloud model tackles final writing (better quality)
- Smart resource management with proactive GPU optimization
- Multi-source search with AI-driven ranking and deduplication

I built this to be both powerful and practical; it balances speed and accuracy while maintaining stability under load. The modular design means I can easily swap components or scale up as needed.
