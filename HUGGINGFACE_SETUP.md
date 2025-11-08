# HuggingFace LLM Setup Guide for SMS Spam Detection

## Overview
This notebook has been updated to use HuggingFace's free Inference API instead of Google's Gemini. This provides free access to various open-source LLMs for spam message detection.

## Getting Started

### 1. Get Your Free HuggingFace API Token
1. Go to https://huggingface.co/settings/tokens
2. Create a new token (select "Read" access)
3. Copy the token - you'll need it in the notebook

### 2. Store Your Token Securely
In Google Colab:
- Go to `Tools → Secrets`
- Add a new secret with name: `HF_API_KEY`
- Paste your token as the value

Alternatively, set it as an environment variable:
```python
os.environ['HF_API_KEY'] = 'your_token_here'  # Not recommended for shared notebooks
```

## Free Tier Limits

### Rate Limits
- **Daily Requests**: ~1,000 requests per day
- **Concurrent Requests**: 1-2 at a time
- **Token Limits**: 1,024-4,096 tokens per request (varies by model)

### Best Practices to Stay Within Limits
1. **Use smaller sample sizes**: Default `LLM_SAMPLE_SIZE = 100` (was 200 with Gemini)
2. **Add delays between requests**: `REQUEST_DELAY = 0.5` seconds
3. **Implement exponential backoff**: Automatically handled in `classify_with_hf()`
4. **Monitor your usage**: Check https://huggingface.co/settings/tokens

## Recommended Models

### 1. microsoft/Phi-3-mini-4k-instruct (Default)
- **Size**: 3.8B parameters
- **Speed**: Fast (~1-2s per request)
- **Best for**: Quick classification, good balance
- **Context**: 4,096 tokens
- **Why we chose this**: Fast inference + good accuracy for spam detection

### 2. meta-llama/Llama-3.2-3B-Instruct
- **Size**: 3B parameters
- **Speed**: Very fast (~1s per request)
- **Best for**: High-speed processing
- **Context**: 8,192 tokens

### 3. mistralai/Mistral-7B-Instruct-v0.3
- **Size**: 7B parameters
- **Speed**: Moderate (~2-3s per request)
- **Best for**: Better accuracy on nuanced messages
- **Context**: 8,192 tokens
- **Note**: Slightly slower but more accurate

### 4. HuggingFaceH4/zephyr-7b-beta
- **Size**: 7B parameters
- **Speed**: Moderate (~2-3s per request)
- **Best for**: Instruction following
- **Context**: 8,192 tokens

## How to Switch Models

Simply change the `HF_MODEL` variable in the configuration cell:

```python
# Option 1: Phi-3-mini (Default - Fast and efficient)
HF_MODEL = 'microsoft/Phi-3-mini-4k-instruct'

# Option 2: Llama 3.2 (Fastest)
HF_MODEL = 'meta-llama/Llama-3.2-3B-Instruct'

# Option 3: Mistral (More accurate)
HF_MODEL = 'mistralai/Mistral-7B-Instruct-v0.3'

# Option 4: Zephyr (Good instruction following)
HF_MODEL = 'HuggingFaceH4/zephyr-7b-beta'
```

## Usage Estimates

### With Default Settings (100 messages, 0.5s delay)
- **API Calls**: 100 requests
- **Time**: ~0.8 minutes (50 seconds)
- **Daily Quota Used**: 10% (900 requests remaining)

### If You Process 200 Messages
- **API Calls**: 200 requests
- **Time**: ~1.7 minutes (100 seconds)
- **Daily Quota Used**: 20% (800 requests remaining)

### Maximum Daily Processing (1000 messages)
- **API Calls**: 1,000 requests
- **Time**: ~8.3 minutes (500 seconds)
- **Daily Quota Used**: 100% (0 requests remaining)

## Error Handling

The notebook includes robust error handling:

1. **Rate Limit Errors (429)**:
   - Automatic exponential backoff
   - Waits progressively longer between retries
   - Falls back to 'ham' after max retries

2. **API Connection Errors**:
   - 3 retry attempts with backoff
   - Detailed error logging
   - Graceful fallback to default classification

3. **Model Loading Errors**:
   - Clear error messages
   - Suggestions for troubleshooting

## Testing Your Setup

Use the test function in the notebook:

```python
test_hf_connection()
```

This will:
- Verify your API key works
- Test a sample classification
- Show estimated quota usage
- Display expected processing time

## Comparison: Gemini vs HuggingFace

| Feature | Gemini (Previous) | HuggingFace (New) |
|---------|------------------|-------------------|
| **Cost** | Paid tier required | Free tier available |
| **Rate Limit** | Higher (paid) | ~1,000 req/day (free) |
| **Sample Size** | 200 messages | 100 messages (adjusted) |
| **Speed** | Fast | Fast (similar) |
| **Models** | Gemini 2.0 Flash | Multiple open-source options |
| **Setup** | Google account + payment | Free HuggingFace account |

## Troubleshooting

### Issue: "Rate limit exceeded"
**Solution**: 
- Wait 24 hours for quota reset
- Reduce `LLM_SAMPLE_SIZE`
- Increase `REQUEST_DELAY`

### Issue: "Invalid API key"
**Solution**:
- Verify token at https://huggingface.co/settings/tokens
- Ensure token has "Read" permission
- Check if token is properly stored in Colab secrets

### Issue: "Model not found"
**Solution**:
- Verify model name is correct
- Some models may require acceptance of terms on HuggingFace
- Try alternative model from recommended list

### Issue: Slow processing
**Solution**:
- Use smaller model (Phi-3-mini or Llama-3.2-3B)
- Reduce `LLM_SAMPLE_SIZE`
- Check network connection

## Upgrading to Pro

If you need more requests, consider HuggingFace Pro:
- **Cost**: $9/month
- **Rate Limit**: 10,000+ requests/day
- **Priority Access**: Faster model loading
- **More Models**: Access to more models
- **Sign up**: https://huggingface.co/pricing

## Additional Resources

- **HuggingFace Inference API Docs**: https://huggingface.co/docs/api-inference/
- **Model Hub**: https://huggingface.co/models
- **Usage Dashboard**: https://huggingface.co/settings/tokens
- **Community Forum**: https://discuss.huggingface.co/

## Summary of Changes

1. ✅ Replaced `google-generativeai` with `huggingface_hub`
2. ✅ Changed from Gemini models to HuggingFace models
3. ✅ Reduced sample size from 200 to 100 to respect rate limits
4. ✅ Added request delays (0.5s) to avoid rate limiting
5. ✅ Improved error handling for rate limits
6. ✅ Added model alternatives documentation
7. ✅ Added usage tracking and estimation
8. ✅ Updated all documentation and instructions

## Next Steps

1. Get your HuggingFace API token
2. Store it in Colab secrets as `HF_API_KEY`
3. Run the test function to verify setup
4. Start with `LLM_SAMPLE_SIZE = 100`
5. Experiment with different models
6. Monitor your usage and adjust as needed
