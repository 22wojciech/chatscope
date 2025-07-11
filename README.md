# 🔍 ChatScope

[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![License: CNCL](https://img.shields.io/badge/License-CNCL-red.svg)](https://github.com/22wojciech/chatscope/blob/main/LICENSE)
[![PyPI version](https://badge.fury.io/py/chatscope.svg)](https://badge.fury.io/py/chatscope)

**Discover the scope of your conversations**

A powerful Python library for analyzing and categorizing ChatGPT conversation exports using OpenAI's API. This tool helps you understand your ChatGPT usage patterns by automatically categorizing your conversations into topics like Programming, AI, Psychology, Philosophy, and more.

## Features

- 📊 **Automatic Categorization**: Uses GPT-4 to intelligently categorize your conversations
- 📈 **Visual Analytics**: Generates beautiful bar charts showing conversation distribution
- 🔒 **Secure**: API keys loaded from environment variables
- ⚡ **Rate Limiting**: Built-in rate limiting to respect OpenAI API limits
- 🔄 **Batch Processing**: Efficiently processes large numbers of conversations
- 💾 **Export Results**: Saves detailed results in JSON format
- 🎨 **Customizable**: Custom categories and visualization options
- 🖥️ **CLI Support**: Command-line interface for easy automation

## Installation

### From PyPI (Recommended)

```bash
pip install chatscope
```

### With plotting support

```bash
pip install chatscope[plotting]
```

### Development Installation

```bash
git clone https://github.com/22wojciech/chatscope.git
cd chatscope
pip install -e .[dev]
```

## Quick Start

### 1. Get Your ChatGPT Data

1. Go to [ChatGPT Settings](https://chat.openai.com/)
2. Navigate to "Data controls" → "Export data"
3. Download your data and extract `conversations.json`

### 2. Set Up OpenAI API Key

Create a `.env` file in your project directory:

```env
OPENAI_API_KEY=your_openai_api_key_here
```

Or set it as an environment variable:

```bash
export OPENAI_API_KEY="your_openai_api_key_here"
```

### 3. Run Analysis

#### Using Python API

```python
from chatscope import ChatGPTAnalyzer

# Initialize analyzer
analyzer = ChatGPTAnalyzer()

# Run analysis
results = analyzer.analyze('conversations.json')

# Print results
print(f"Total conversations: {results['total_conversations']}")
for category, count in results['counts'].items():
    if count > 0:
        print(f"{category}: {count}")
```

#### Using Command Line

```bash
# Basic usage
chatscope conversations.json

# Custom output paths
chatscope conversations.json -o my_chart.png -r my_results.json

# Don't show the plot
chatscope conversations.json --no-show
```

## Advanced Usage

### Custom Categories

```python
from chatscope import ChatGPTAnalyzer

custom_categories = [
    "Work",
    "Personal",
    "Learning",
    "Creative",
    "Technical",
    "Other"
]

analyzer = ChatGPTAnalyzer(categories=custom_categories)
results = analyzer.analyze('conversations.json')
```

### Rate Limiting Configuration

```python
analyzer = ChatGPTAnalyzer(
    batch_size=10,  # Process 10 titles per request
    delay_between_requests=2.0,  # Wait 2 seconds between requests
    max_tokens_per_request=3000  # Limit tokens per request
)
```

### Programmatic Chart Generation

```python
analyzer = ChatGPTAnalyzer()
results = analyzer.analyze(
    'conversations.json',
    output_chart='custom_chart.png',
    show_plot=False  # Don't display, just save
)
```

## Command Line Interface

The package includes a comprehensive CLI:

```bash
chatscope --help
```

### CLI Examples

```bash
# Basic analysis
chatscope conversations.json

# Custom API key and batch size
chatscope --api-key sk-... --batch-size 15 conversations.json

# Custom categories
chatscope --categories "Work" "Personal" "Learning" conversations.json

# Verbose output
chatscope -v conversations.json

# Quiet mode (only errors)
chatscope -q conversations.json

# Custom figure size
chatscope --figsize 16 10 conversations.json
```

## Data Format

Your `conversations.json` should follow this structure:

```json
[
  {
    "title": "Python Data Analysis Tutorial",
    "create_time": 1699123456.789,
    "update_time": 1699123456.789
  },
  {
    "title": "Machine Learning Basics",
    "create_time": 1699123456.789,
    "update_time": 1699123456.789
  }
]
```

## Default Categories

The analyzer uses these categories by default:

- **Programming** - Code, software development, debugging
- **Artificial Intelligence** - AI, ML, data science topics
- **Psychology / Personal Development** - Mental health, self-improvement
- **Philosophy** - Philosophical discussions and questions
- **Astrology / Esoteric** - Spiritual and mystical topics
- **Work / Career** - Professional and career-related conversations
- **Health** - Medical, fitness, and wellness topics
- **Education** - Learning, academic subjects
- **Other** - Everything else

## Output Files

The analyzer generates several output files:

### 1. Chart (`conversation_categories.png`)
A bar chart showing the distribution of conversations across categories.

### 2. Detailed Results (`categorization_results.json`)
```json
{
  "timestamp": "2024-01-15T10:30:00",
  "total_conversations": 150,
  "categories": {
    "Programming": ["Python Tutorial", "Debug Help"],
    "AI": ["ChatGPT Tips", "ML Basics"]
  },
  "counts": {
    "Programming": 45,
    "AI": 32,
    "Other": 73
  }
}
```

## Error Handling

The library includes comprehensive error handling:

```python
from chatscope import ChatGPTAnalyzer
from chatscope.exceptions import ChatGPTAnalyzerError

try:
    analyzer = ChatGPTAnalyzer()
    results = analyzer.analyze('conversations.json')
except APIError as e:
    print(f"OpenAI API error: {e}")
except DataError as e:
    print(f"Data processing error: {e}")
except Exception as e:
    print(f"Unexpected error: {e}")
```

## Requirements

- Python 3.8+
- OpenAI API key
- Required packages (automatically installed):
  - `openai>=0.28.0`
  - `python-dotenv>=0.19.0`
  - `requests>=2.25.0`
- Optional packages:
  - `matplotlib>=3.5.0` (for plotting)

## API Costs

The tool uses OpenAI's GPT-4 API. Costs depend on:
- Number of conversation titles
- Batch size (larger batches = fewer API calls)
- Title length and complexity

Typical costs:
- ~100 conversations: $0.10-0.50
- ~1000 conversations: $1.00-5.00

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

### Development Setup

```bash
git clone https://github.com/22wojciech/chatscope.git
cd chatscope
pip install -e .[dev]
```

### Running Tests

```bash
pytest
```

### Code Formatting

```bash
black chatscope/
flake8 chatscope/
```

## License

This project is licensed under the Custom Non-Commercial License (CNCL) v1.0 - see the [LICENSE](https://github.com/22wojciech/chatscope/blob/main/LICENSE) file for details.

**⚠️ Important:** This software is free for personal, academic, and research use only. Commercial use requires a separate license. Contact plus4822@icloud.com for commercial licensing.

## Changelog

### v1.0.0
- Initial release
- Basic conversation categorization
- CLI interface
- Plotting support
- Rate limiting
- Comprehensive error handling

## Support

If you encounter any issues or have questions:

1. Check the [Issues](https://github.com/22wojciech/chatscope/issues) page
2. Create a new issue with detailed information
3. Include your Python version, OS, and error messages

## Acknowledgments

- OpenAI for providing the GPT-4 API
- The Python community for excellent libraries
- Contributors and users who provide feedback

---

**Made with ❤️ for the ChatGPT community**