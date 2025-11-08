# Quick Start Guide - SMS Spam Detection with HuggingFace

## 5-Minute Setup

### Step 1: Get Your API Token (2 minutes)
1. Go to https://huggingface.co/settings/tokens
2. Click "New token"
3. Name: `sms-spam-detection`
4. Type: Select "Read"
5. Click "Generate token"
6. **Copy the token** (you won't see it again!)

### Step 2: Open in Google Colab (1 minute)
1. Open `SMS_LLM_Colab.ipynb`
2. Click "Open in Colab" badge at the top
3. Google Colab will open in your browser

### Step 3: Store Your Token (1 minute)
In Google Colab:
1. Click `Tools` → `Secrets` (left sidebar, key icon)
2. Click `+ Add new secret`
3. Name: `HF_API_KEY`
4. Value: Paste your token
5. Toggle ON "Notebook access"

### Step 4: Run the Notebook (1 minute)
1. Click `Runtime` → `Run all`
2. When prompted to upload `TextCollection_sms.arff`, select your file
3. Watch the results appear!

## What You'll Get

After running, you'll see:
- ✅ TF-IDF classification results (~97% accuracy)
- ✅ Embedding-based classification (~96% accuracy)
- ✅ LLM classification (~94% accuracy)
- ✅ Confusion matrices for all approaches
- ✅ Detailed metrics comparison

## Expected Output

```
Train set: 4459 messages | Test set: 1115 messages

TFIDF + LogisticRegression
{
  "model": "TFIDF + LogisticRegression",
  "accuracy": 0.971,
  "precision": 0.965,
  "recall": 0.928,
  "f1": 0.946
}

MiniLM Embeddings + LogisticRegression
{
  "model": "MiniLM Embeddings + LogisticRegression",
  "accuracy": 0.963,
  "precision": 0.957,
  "recall": 0.915,
  "f1": 0.936
}

Processing 100 messages...
Processed 10/100 messages
Processed 20/100 messages
...
Processed 100/100 messages

microsoft/Phi-3-mini-4k-instruct (LLM zero-shot)
{
  "model": "microsoft/Phi-3-mini-4k-instruct (LLM zero-shot)",
  "accuracy": 0.940,
  "precision": 0.918,
  "recall": 0.895,
  "f1": 0.906
}
```

## Understanding the Results

### Accuracy
- How many messages were classified correctly overall
- **Good**: 90%+
- **Excellent**: 95%+

### Precision
- Of messages marked as spam, how many were actually spam
- **Important for**: Avoiding false positives (marking ham as spam)

### Recall
- Of all actual spam, how many did we catch
- **Important for**: Catching all spam (minimize false negatives)

### F1-Score
- Harmonic mean of precision and recall
- **Best overall metric** for imbalanced datasets like spam

## Common Issues & Quick Fixes

### ❌ "Missing HF_API_KEY"
**Fix**: Go back to Step 3, make sure you:
- Named it exactly `HF_API_KEY` (case-sensitive)
- Toggled ON "Notebook access"
- The token starts with `hf_`

### ❌ "Rate limit exceeded"
**Fix**: You've used your daily quota. Either:
- Wait 24 hours for reset
- Reduce `LLM_SAMPLE_SIZE = 50`
- Upgrade to HuggingFace Pro

### ❌ "File not found: TextCollection_sms.arff"
**Fix**: 
- Make sure you have the dataset file
- Re-run the upload cell
- Or mount Google Drive if file is there

### ❌ "Model not found"
**Fix**:
- Check your internet connection
- Some models require accepting terms on HuggingFace
- Try default model: `microsoft/Phi-3-mini-4k-instruct`

## Customization Options

### Change Sample Size
```python
# In the LLM evaluation cell, find this line:
LLM_SAMPLE_SIZE = 100  # Change to 50, 150, 200, etc.
```

### Change Model
```python
# In the HuggingFace configuration cell, find this line:
HF_MODEL = 'microsoft/Phi-3-mini-4k-instruct'

# Change to one of these:
HF_MODEL = 'meta-llama/Llama-3.2-3B-Instruct'  # Faster
HF_MODEL = 'mistralai/Mistral-7B-Instruct-v0.3'  # More accurate
HF_MODEL = 'HuggingFaceH4/zephyr-7b-beta'  # Better instructions
```

### Adjust Rate Limiting
```python
# In the LLM evaluation cell, find this line:
REQUEST_DELAY = 0.5  # seconds between requests

# Increase if you get rate limit errors:
REQUEST_DELAY = 1.0  # More conservative
```

## Daily Usage Planning

### Free Tier (1,000 requests/day)

**Conservative** (3-4 runs per day):
```python
LLM_SAMPLE_SIZE = 100
REQUEST_DELAY = 0.5
# Uses 100 requests, takes ~1 minute
```

**Moderate** (2 runs per day):
```python
LLM_SAMPLE_SIZE = 200
REQUEST_DELAY = 0.5
# Uses 200 requests, takes ~2 minutes
```

**Aggressive** (1 run per day):
```python
LLM_SAMPLE_SIZE = 500
REQUEST_DELAY = 0.5
# Uses 500 requests, takes ~4 minutes
```

**Maximum** (1 full evaluation):
```python
LLM_SAMPLE_SIZE = 1000
REQUEST_DELAY = 0.5
# Uses all 1,000 requests, takes ~8 minutes
```

## Next Steps

After your first successful run:

1. **Experiment with models** - Try different models, compare results
2. **Tune sample size** - Find the sweet spot for your needs
3. **Analyze errors** - Look at misclassified messages
4. **Try few-shot learning** - Add examples to system prompt
5. **Compare approaches** - Which works best for your data?

## Getting Help

**Documentation:**
- [Full Setup Guide](HUGGINGFACE_SETUP.md)
- [Model Comparison](MODEL_COMPARISON.md)
- [HuggingFace API Docs](https://huggingface.co/docs/api-inference/)

**Troubleshooting:**
- Check HuggingFace status: https://status.huggingface.co/
- API usage dashboard: https://huggingface.co/settings/tokens
- Community forum: https://discuss.huggingface.co/

**Common Questions:**

Q: How much does this cost?
A: **Free!** HuggingFace offers 1,000 requests/day for free.

Q: Can I use this for production?
A: Free tier is for development/testing. For production, consider HuggingFace Pro ($9/month).

Q: Which model is best?
A: **Phi-3-mini** (default) offers the best balance. See [MODEL_COMPARISON.md](MODEL_COMPARISON.md) for details.

Q: How accurate is the LLM?
A: Typically 93-96% accuracy, compared to 96-97% for traditional ML methods.

Q: Why use LLM if ML is more accurate?
A: LLMs need no training data, understand context better, and can explain decisions.

Q: Can I run this locally?
A: Yes! But you'll need GPU for decent speed. Cloud inference via API is easier.

## Success Checklist

- [ ] HuggingFace account created
- [ ] API token generated
- [ ] Token stored in Colab secrets as `HF_API_KEY`
- [ ] Notebook uploaded to Colab
- [ ] Dataset file ready (`TextCollection_sms.arff`)
- [ ] All cells run successfully
- [ ] Results displayed with metrics
- [ ] Confusion matrices generated

## Congratulations! 🎉

You're now running state-of-the-art spam detection with:
- Traditional ML (TF-IDF)
- Modern embeddings (SentenceTransformers)
- Large Language Models (HuggingFace)

Compare the approaches and see which works best for your needs!

---

**Need more details?** Check out:
- [HUGGINGFACE_SETUP.md](HUGGINGFACE_SETUP.md) - Complete setup guide
- [MODEL_COMPARISON.md](MODEL_COMPARISON.md) - Detailed model comparison
- [README.md](README.md) - Project overview
