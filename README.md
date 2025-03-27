Collecting workspace information# MITRE-ATT&CK-obsidian-markdown

This repository implements a Python tool that parses the MITRE ATT&CK knowledge base into a markdown format optimized for the Obsidian note-taking application. The tool retrieves STIX 2.1 JSON data from MITRE's GitHub repository and transforms it into a network of interlinked markdown notes, making the entire ATT&CK framework browsable and searchable within Obsidian.

## Overview

MITRE ATT&CK is a globally-accessible knowledge base of adversary tactics and techniques based on real-world observations. This tool makes this valuable security framework easily accessible in Obsidian, allowing you to:

- Browse the entire ATT&CK framework within your Obsidian vault
- View relationships between techniques, tactics, mitigations, and threat groups
- Integrate ATT&CK data with your threat intelligence notes
- Leverage Obsidian's powerful features like graph view, backlinks, and search

## Installation

1. Clone this repository
```bash
git clone https://github.com/JafarShamzsi/MITRE-ATTACK-obsidian-markdown.git
```

2. Create a Python virtual environment
```bash
cd MITRE-ATTACK-obsidian-markdown
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

3. Install Python dependencies
```bash
pip install -r requirements.txt
```

## Configuration

The config.yml file contains essential settings:

```yaml
repository_url: https://raw.githubusercontent.com/mitre-attack/attack-stix-data/master
domain: enterprise-attack
version: 15.1
output_dir: output
```

- **repository_url**: URL to MITRE's STIX data repository
- **domain**: ATT&CK domain ("enterprise-attack", "mobile-attack", or "ics-attack")
- **version**: ATT&CK version (remove to use latest)
- **output_dir**: Default output directory (override with -o flag)

## Usage

### Generate the Complete ATT&CK Framework

```bash
python . -o /path/to/your/obsidian/vault
```

This will:
1. Download the MITRE ATT&CK STIX data
2. Parse and convert it to markdown notes
3. Create directories for tactics, techniques, mitigations, and groups
4. Set up Obsidian graph visualization settings

### Add Hyperlinks to Technique IDs in a Note

```bash
python . --generate-hyperlinks --path /path/to/your/note.md
```

This scans your note for technique IDs (e.g., T1078) and converts them to Obsidian internal links pointing to the corresponding technique notes.

### Create an ATT&CK Matrix Canvas

```bash
python . --generate-matrix --path /path/to/your/note.md
```

This creates an Obsidian canvas visualization of all techniques mentioned in your note, organized by tactics.

## Command-Line Options

```
usage: . [-h] [-d DOMAIN] [-o OUTPUT] [--generate-hyperlinks] [--generate-matrix] [--path PATH]

Download MITRE ATT&CK STIX data and parse it to Obsidian markdown notes

options:
  -h, --help            show this help message and exit
  -d DOMAIN, --domain DOMAIN
                        Domain should be 'enterprise-attack', 'mobile-attack' or 'ics-attack'
  -o OUTPUT, --output OUTPUT
                        Output directory in which the notes will be saved. It should be placed inside an Obsidian vault.
  --generate-hyperlinks
                        Generate techniques hyperlinks in a markdown note file
  --generate-matrix     Create ATT&CK matrix starting from a markdown note file
  --path PATH           Filepath to the markdown note file
```

## Project Structure

- **`__main__.py`**: Entry point, handles command-line arguments and workflow
- **`src/stix_parser.py`**: Fetches and parses STIX data from MITRE's repository
- **`src/models.py`**: Defines data models for ATT&CK entities (tactics, techniques, etc.)
- **`src/markdown_generator.py`**: Converts parsed data to markdown files
- **`src/markdown_reader.py`**: Processes existing markdown files for hyperlinks/visualization
- **`src/view.py`**: Creates Obsidian graph visualization configuration
- **`config.yml`**: Configuration settings for the tool
- **`res/graph.json`**: Obsidian graph settings for optimal ATT&CK visualization

## Examples of Generated Content

### Tactic Notes
Each tactic note includes:
- ID and description 
- External references
- Links to associated techniques

### Technique Notes
Each technique note includes:
- ID and description
- Associated tactics with links
- Supported platforms
- Required permissions
- Related mitigations
- Sub-techniques (if applicable)
- External references

### Mitigation Notes
Each mitigation note includes:
- ID and description
- Techniques addressed by the mitigation

### Group Notes
Each group note includes:
- ID and description
- Aliases
- Techniques used by the group
