---
name: think-in-code
description: Use scripts (Python, Node, shell) to analyze data instead of reading into context. Saves 90-98% tokens on large files, logs, and data analysis. Use when analyzing CSV, JSON, logs, counting files, extracting patterns, or any task requiring data processing.
---

# Think in Code

Use scripts to analyze data instead of reading entire files into context.

## When to Use

**Use scripts when:**
- Analyzing CSV/JSON/XML files > 100 lines
- Processing log files
- Counting files or patterns
- Extracting specific data
- Statistical analysis
- Any task requiring data transformation

**Don't use scripts when:**
- File is small (< 50 lines)
- Need to see full content for understanding
- Editing file content directly

## Examples

### 1. Analyze CSV File

**❌ Bad (reads 10,000 lines into context):**
```
read(file="data.csv")
# → 10,000 tokens in context
```

**✅ Good (script returns summary):**
```bash
python3 -c "
import pandas as pd
df = pd.read_csv('data.csv')
print('Rows:', len(df))
print('Columns:', df.columns.tolist())
print('Missing:', df.isnull().sum().sum())
print('Sample:')
print(df.head(5).to_string())
"
# → 200 tokens in context
```

### 2. Find Large Functions

**❌ Bad:**
```
read(file="src/auth.ts")  # 500 tokens
read(file="src/login.ts")  # 400 tokens
# ... read 50 files = 20,000 tokens
```

**✅ Good:**
```bash
find src -name "*.ts" -exec wc -l {} + | awk '$1 > 100 {print}'
# → 50 tokens
```

### 3. Extract JSON Data

**❌ Bad:**
```
read(file="package.json")
# → Read full file, manually parse
```

**✅ Good:**
```bash
cat package.json | jq '.dependencies | keys[]'
# → Only dependency names
```

### 4. Analyze Logs

**❌ Bad:**
```
read(file="app.log")
# → 10,000 lines in context
```

**✅ Good:**
```bash
grep -i "error\|warning\|fail" app.log | tail -50
# → Only relevant lines
```

### 5. Count Files by Type

**❌ Bad:**
```
glob(pattern="**/*.ts")
# → Returns all file paths
```

**✅ Good:**
```bash
find . -name "*.ts" | wc -l
# → Just the count
```

### 6. Extract Specific Data

**❌ Bad:**
```
read(file="users.json")
# → Full JSON in context
```

**✅ Good:**
```bash
cat users.json | jq '.[] | select(.age > 18) | .name'
# → Only names of adults
```

## Common Patterns

### Python (pandas)
```bash
python3 -c "
import pandas as pd
df = pd.read_csv('file.csv')
print(df.describe())
print(df.groupby('category').size())
"
```

### Node.js
```bash
node -e "
const fs = require('fs');
const data = JSON.parse(fs.readFileSync('data.json'));
console.log(data.length);
console.log(data.filter(x => x.active).length);
"
```

### Shell (jq for JSON)
```bash
cat data.json | jq '.items[] | {id, name}'
```

### Shell (awk for text)
```bash
awk '/pattern/ {print $1, $3}' file.txt
```

### Shell (grep + wc)
```bash
grep -c "pattern" file.txt
```

## Token Savings

| Task | Without Script | With Script | Savings |
|------|----------------|-------------|---------|
| CSV 10K lines | 10,000 tokens | 200 tokens | 98% |
| JSON analysis | 5,000 tokens | 100 tokens | 98% |
| Count files | 3,000 tokens | 50 tokens | 98% |
| Log analysis | 8,000 tokens | 300 tokens | 96% |

## Tips

1. **Use one-liners** when possible
2. **Pipe commands** to filter output
3. **Use head/tail** to limit output
4. **Use jq** for JSON processing
5. **Use pandas** for CSV/data analysis
6. **Use awk/sed** for text transformation

## When NOT to Use

- File is small (< 50 lines)
- Need to understand full context
- Editing file directly
- Debugging specific line numbers
