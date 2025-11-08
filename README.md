# SMS Spam Classification with LLMs

A comprehensive comparison of spam detection methods using traditional ML and modern LLMs.

## Overview

This notebook compares three approaches to SMS spam classification:
1. **TF-IDF + Logistic Regression** - Traditional bag-of-words approach
2. **SentenceTransformer Embeddings + Logistic Regression** - Semantic embeddings
3. **HuggingFace LLM** - Zero-shot classification with open-source models

## Quick Start

### Option 1: Google Colab (Recommended)
1. Click the "Open in Colab" badge in the notebook
2. Get a free HuggingFace API token from https://huggingface.co/settings/tokens
3. Store your token in Colab: `Tools → Secrets` → Add `HF_API_KEY`
4. Run all cells

### Option 2: Local Setup
```bash
pip install pandas numpy scikit-learn seaborn matplotlib sentence-transformers scipy liac-arff huggingface_hub
export HF_API_KEY='your_token_here'
jupyter notebook SMS_LLM_Colab.ipynb
```

## Features

- 📊 **Complete ML Pipeline**: Data loading, preprocessing, training, evaluation
- 🤖 **Multiple LLM Options**: Phi-3, Llama 3.2, Mistral, Zephyr
- 📈 **Comprehensive Metrics**: Accuracy, Precision, Recall, F1-Score
- 🎯 **Confusion Matrices**: Visual comparison of all approaches
- 💰 **Free Tier Friendly**: Optimized for HuggingFace's free API limits
- ⚡ **Rate Limit Handling**: Automatic retries and backoff strategies

## HuggingFace Free Tier Limits

- **Rate Limit**: ~1,000 requests per day
- **Sample Size**: Default 100 messages (adjustable)
- **Request Delay**: 0.5 seconds between calls
- **Estimated Time**: ~50 seconds for 100 messages

See [HUGGINGFACE_SETUP.md](HUGGINGFACE_SETUP.md) for detailed usage guidelines.

## Supported Models

| Model | Size | Speed | Best For |
|-------|------|-------|----------|
| microsoft/Phi-3-mini-4k-instruct | 3.8B | Fast | **Default** - Balanced performance |
| meta-llama/Llama-3.2-3B-Instruct | 3B | Very Fast | High-speed processing |
| mistralai/Mistral-7B-Instruct-v0.3 | 7B | Moderate | Better accuracy |
| HuggingFaceH4/zephyr-7b-beta | 7B | Moderate | Instruction following |

## Dataset

Uses the SMS Spam Collection dataset in ARFF format (`TextCollection_sms.arff`):
- **Total Messages**: ~5,574
- **Ham (legitimate)**: ~4,827 (87%)
- **Spam**: ~747 (13%)
- **Train/Test Split**: 80/20 stratified

## Results

Typical performance metrics:
- **TF-IDF + Logistic Regression**: ~96-97% accuracy, high recall on spam
- **MiniLM Embeddings**: ~96-98% accuracy, reduced false positives
- **HuggingFace LLM**: ~93-96% accuracy, no training required, understands context

## Files

- `SMS_LLM_Colab.ipynb` - Main notebook with all experiments
- `HUGGINGFACE_SETUP.md` - Detailed setup and usage guide
- `TextCollection_sms.arff` - Dataset (not included, upload separately)

## Documentation

- [HuggingFace Setup Guide](HUGGINGFACE_SETUP.md) - Complete setup instructions
- [HuggingFace API Docs](https://huggingface.co/docs/api-inference/)
- [Model Hub](https://huggingface.co/models)

## Troubleshooting

Common issues and solutions:

**Rate limit exceeded**: Wait 24 hours or reduce `LLM_SAMPLE_SIZE`
**Invalid API key**: Verify at https://huggingface.co/settings/tokens
**Slow processing**: Use smaller model (Phi-3 or Llama-3.2-3B)

See [HUGGINGFACE_SETUP.md](HUGGINGFACE_SETUP.md) for more details.

## Upgrading

Need more requests? Consider **HuggingFace Pro** ($9/month):
- 10,000+ requests/day
- Priority access
- Faster model loading

## License

This project is for educational purposes. The SMS Spam Collection dataset has its own license terms.

## Acknowledgments

- SMS Spam Collection dataset
- HuggingFace for free API access
- SentenceTransformers library
- Scikit-learn team