# translate-filename-llm

A Python script that intelligently translates filenames using Large Language Models (LLMs), with support for preserving unique identifiers like YouTube IDs, UUIDs, and custom patterns.

## Features

- **Smart ID Detection**: Automatically detects and preserves various types of unique identifiers:
  - YouTube-style IDs (11 characters)
  - UUIDs (standard format)
  - Numeric IDs (customizable minimum length)
  - Mixed alphanumeric IDs
  - Bracketed IDs `[ID]` or `(ID)`
  - Hashtag-style IDs `#ID`
  - Custom regex patterns

- **Flexible Translation**: Uses the `llm` CLI tool with support for:
  - Custom models
  - Custom templates
  - Configurable delays between API calls

- **Cross-platform Safety**: 
  - Sanitizes filenames for compatibility
  - Preserves file extensions intelligently
  - Groups files by base name for batch processing

- **User-friendly Interface**:
  - Preview changes before applying
  - Batch confirmation or auto-accept mode
  - Comprehensive logging and debug output

## Installation

### Prerequisites

1. Python 3.7+
2. The `llm` CLI tool installed and configured:
   ```bash
   pip install llm
   # Configure your preferred LLM provider
   llm keys set openai
   # or configure other providers as needed
   ```

### Download

Download the script directly:
```bash
curl -O https://raw.githubusercontent.com/yourusername/translate-filenames/main/translate-filenames.py
chmod +x translate-filenames.py
```

Or clone the repository:
```bash
git clone https://github.com/yourusername/translate-filenames.git
cd translate-filenames
```

## Usage

### Basic Usage

```bash
# Translate simple filenames
./translate-filenames.py file1.txt file2.txt

# Auto-accept all changes
./translate-filenames.py -y *.pdf

# Use specific model and add delay between API calls
./translate-filenames.py -m gpt-4 -d 2 *.mp4
```

### With ID Detection

```bash
# Preserve YouTube IDs and bracketed content
./translate-filenames.py --youtube-id --bracketed-id *.mp4

# Detect numeric IDs with minimum 8 digits
./translate-filenames.py --numeric-id --numeric-min-length 8 *.jpg

# Use custom regex pattern
./translate-filenames.py --custom-pattern "IMG_\d{4}" *.jpg
```

### Advanced Options

```bash
# Use custom template with specific model
./translate-filenames.py -t translation-template -m claude-3-sonnet *.txt

# Limit filename length and preserve up to 3 extensions
./translate-filenames.py -l 50 --max-extensions 3 *.tar.gz.backup

# Enable debug output
./translate-filenames.py --debug *.pdf
```

## Command Line Options

### Basic Options
- `files`: Files to rename (required)
- `-l, --max-length`: Maximum filename length (default: 100)
- `-y, --yes`: Auto-accept all renames
- `--max-extensions`: Maximum number of extensions to preserve (default: 2)
- `-d, --delay`: Seconds to wait between translations (default: 0.0)
- `--debug`: Enable debug output

### LLM Options
- `-m, --model`: LLM model to use
- `-t, --template`: LLM template to use

### ID Detection Options
- `--youtube-id`: Detect YouTube-style IDs (11 chars)
- `--uuid`: Detect UUIDs
- `--numeric-id`: Detect numeric IDs
- `--numeric-min-length`: Minimum length for numeric IDs (default: 6)
- `--mixed-id`: Detect mixed alphanumeric IDs
- `--mixed-min-length`: Minimum length for mixed IDs (default: 8)
- `--bracketed-id`: Detect IDs in brackets/parentheses
- `--bracketed-min-length`: Minimum length for bracketed IDs (default: 3)
- `--hash-id`: Detect hashtag-style IDs (#id)
- `--hash-min-length`: Minimum length for hash IDs (default: 6)
- `--custom-pattern`: Custom regex pattern for ID detection

## Examples

### Example 1: YouTube Videos
```bash
# Original: "Como hacer pasta italiana [dQw4w9WgXcQ].mp4"
# Command: ./translate-filenames.py --youtube-id --bracketed-id *.mp4
# Result: "How to make Italian pasta_dQw4w9WgXcQ.mp4"
```

### Example 2: Academic Papers
```bash
# Original: "论文_12345678.pdf"
# Command: ./translate-filenames.py --numeric-id *.pdf
# Result: "Paper_12345678.pdf"
```

### Example 3: Image Files with UUIDs
```bash
# Original: "照片_550e8400-e29b-41d4-a716-446655440000.jpg"
# Command: ./translate-filenames.py --uuid *.jpg
# Result: "Photo_550e8400-e29b-41d4-a716-446655440000.jpg"
```

## How It Works

1. **File Grouping**: Files are grouped by their base name (without extensions)
2. **ID Extraction**: Configured ID patterns are detected and extracted
3. **Text Preparation**: Remaining text is cleaned and prepared for translation
4. **Translation**: Text is sent to the LLM for translation
5. **Reconstruction**: Translated text is combined with preserved IDs and extensions
6. **Sanitization**: Final filename is sanitized for cross-platform compatibility
7. **Preview & Confirmation**: User reviews changes before applying

## Configuration

The script can be configured through command-line arguments or by modifying the constants at the top of the file:

```python
DEFAULT_MAX_LENGTH = 100
DEFAULT_MAX_EXTENSIONS = 2
DEFAULT_DELAY = 0.0
EXTENSION_MAX_CHARS = 7
LLM_TIMEOUT = 30
```

## Error Handling

- Files that don't exist are skipped with warnings
- LLM API failures fall back to original filename
- Cross-platform filename conflicts are detected
- Comprehensive logging for debugging

## License

This project is licensed under the MIT License - see below for details:

```
MIT License

Copyright (c) 2025

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request. For major changes, please open an issue first to discuss what you would like to change.

## Support

If you encounter any issues or have questions, please open an issue on the GitHub repository.

Sources
[1] translate-filenames.py https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/709294/b604451a-3083-44de-910a-bca2037e25b6/translate-filenames.py
