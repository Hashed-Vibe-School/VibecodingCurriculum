# Chapter 16: Data Processing

**English** | [한국어](./README.ko.md)

## What You Will Learn

- Working with files (CSV, JSON)
- Transforming data
- Building automation scripts

---

## Why Data Processing?

A lot of coding is about handling data:
- Organizing Excel files
- Merging multiple files
- Converting formats (CSV → JSON)
- Automating repetitive tasks

Claude can help you do these quickly.

---

## Understanding File Formats

### CSV (Like Excel)

Comma-separated data:
```csv
name,age,city
John,25,Seoul
Jane,30,Busan
```

### JSON (Structured Data)

Format commonly used in programming:
```json
[
  {"name": "John", "age": 25, "city": "Seoul"},
  {"name": "Jane", "age": 30, "city": "Busan"}
]
```

---

## Working with CSV

### Reading CSV

```
> Read data.csv and show me the contents
```

### CSV → JSON Conversion

```
> Convert data.csv to data.json
```

Code Claude creates:
```javascript
const fs = require('fs')

// Read CSV
const csv = fs.readFileSync('data.csv', 'utf-8')
const lines = csv.split('\n')
const headers = lines[0].split(',')

// Convert to JSON
const data = lines.slice(1).map(line => {
  const values = line.split(',')
  return headers.reduce((obj, header, i) => {
    obj[header] = values[i]
    return obj
  }, {})
})

// Save
fs.writeFileSync('data.json', JSON.stringify(data, null, 2))
```

### Filtering CSV

```
> Extract only people age 30 or older from data.csv
> and save to a new file
```

---

## Working with JSON

### Reading and Modifying JSON

```
> Change the port value in config.json from 3000 to 8080
```

### Merging Multiple JSON Files

```
> Merge data1.json and data2.json into one
```

### Formatting JSON

```
> Format this JSON nicely (apply indentation)
```

---

## Real Project: Data Cleanup Tool

### Situation

Say you have this messy CSV:

```csv
name,phone,email
John Doe ,  010-1234-5678, john@email.com
 Jane Smith,01098765432,JANE@EMAIL.COM
Bob Wilson,  010.5555.6666  ,bob@email
```

### Goal

- Remove whitespace
- Standardize phone format
- Convert email to lowercase
- Flag invalid emails

### Building It

```
> Make a CSV cleanup tool.
> - Remove leading/trailing whitespace
> - Phone format: 010-0000-0000
> - Email to lowercase
> - Validate email format
```

### Result

```csv
name,phone,email,valid
John Doe,010-1234-5678,john@email.com,OK
Jane Smith,010-9876-5432,jane@email.com,OK
Bob Wilson,010-5555-6666,bob@email,Invalid email
```

---

## Batch File Processing

### Process Multiple Files at Once

```
> Convert all CSV files in the data folder to JSON
```

### Bulk Rename Files

```
> Add "2024_" prefix to all files in the images folder
```

### Organize Folders

```
> In the downloads folder:
> - Move images to images folder
> - Move documents to docs folder
> - Move rest to others folder
```

---

## Automation Scripts

### Automate Daily Tasks

```
> Create a script to run every morning:
> 1. Find yesterday's files in data folder
> 2. Clean up the data
> 3. Save results to report folder
> 4. Print summary statistics
```

### Example: Backup Script

```
> Create a project backup script.
> - Important files only (exclude node_modules)
> - Save in date-based folders
> - Delete backups older than 7 days
```

---

## Data Analysis

### Basic Statistics

```
> Analyze sales.csv file:
> - Total sales
> - Average sale
> - Best selling product
> - Monthly sales trend
```

### Visualization

```
> Show the analysis results in a chart
```

Claude will create graphs using Chart.js or other libraries.

---

## Web Scraping Basics

### Cautions

- Check website terms of service
- Don't make too many requests
- Follow robots.txt

### Simple Scraping

```
> Extract product price list from this webpage:
> [URL]
```

### Save Data

```
> Save the extracted data as CSV
```

---

## Practice: Build Automation Tools

### Basic Tasks

```
# Task 1: File Converter
> Make a CSV ↔ JSON converter.
> Drag and drop file to convert.

# Task 2: Data Cleaner
> Make a tool that cleans data pasted from Excel.
```

### Extra Challenges

```
# Image Resizer
> Tool to resize all images in a folder to specific dimensions

# Log Analyzer
> Tool to analyze server log files and show summary

# Duplicate Finder
> Tool to find duplicate files in a folder
```

---

## Useful Libraries

### Node.js

| Library | Purpose |
|---------|---------|
| `csv-parser` | Read CSV |
| `json2csv` | JSON → CSV |
| `glob` | File pattern search |
| `sharp` | Image processing |
| `cheerio` | Web scraping |

### Python

| Library | Purpose |
|---------|---------|
| `pandas` | Data analysis |
| `openpyxl` | Excel processing |
| `beautifulsoup4` | Web scraping |
| `pillow` | Image processing |

```
> Show me an example of analyzing CSV with pandas
```

---

## Error Handling

### When File Doesn't Exist

```javascript
const fs = require('fs')

try {
  const data = fs.readFileSync('data.csv', 'utf-8')
} catch (error) {
  if (error.code === 'ENOENT') {
    console.log('File not found')
  }
}
```

### Invalid Format

```
> If there's an error parsing CSV, skip that line
> and log which line had the error
```

---

## Mini Project: Data Dashboard

Build a dashboard that collects and visualizes data.

### Goals

- Automate data processing
- Visualization

### Build It

```
> Create a sales data dashboard.
> - CSV file upload
> - Data cleanup and analysis
> - Chart visualization (bar, line, pie)
> - Summary statistics display
```

### Advanced Challenges (For Experts)

```
> Create a cron job for periodic data collection

> Add real-time data update feature

> Add automatic PDF report generation

> Add anomaly detection alerts
```

---

## Summary

What you learned in this chapter:
- [x] Working with CSV, JSON files
- [x] Transforming data
- [x] Batch processing
- [x] Building automation scripts
- [x] Data analysis basics

In the next chapter, we'll learn how to integrate external APIs.

[Chapter 17: API Integration](../Chapter17/README.md)
