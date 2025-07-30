# Claude Generated Code

**Task**: Description:
Perform a complete audit of the documentation files to catch broken links, improper markdown usage, inconsistent heading levels, and formatting issues. This will improve the reader experience and SEO, especially if this is planned for external visibility.

Tasks:

- Scan for broken internal and external links.
- Standardize heading levels (H1, H2, etc.) across files.
- Ensure proper code block formatting and inline code consistency.
- Identify duplicate or outdated sections and suggest removals or updates.



**Generated on**: Wed Jul 30 08:43:04 UTC 2025

## Generated Content

```
To perform a complete audit of the documentation files and address broken links, improper markdown usage, inconsistent heading levels, and formatting issues, we will need to use some command-line tools and create a script or script runner. Here are the steps and code changes:

**FILES_TO_CREATE**:

1. `scripts/audit-docs.js`

**FILES_TO_MODIFY**:

1. `package.json`

**CODE_CHANGES**:

In `package.json`, add the following dependencies and scripts:

```json
{
  "dependencies": {
    "glob": "^8.1.0",
    "markdown-link-check": "^3.10.3",
    "markdownlint-cli": "^0.33.0"
  },
  "scripts": {
    "audit:links": "markdown-link-check docs/**/*.md blog/**/*.md",
    "audit:markdown": "markdownlint --config .markdownlint.json docs/**/*.md blog/**/*.md",
    "audit:headings": "node scripts/audit-docs.js headings",
    "audit:docs": "npm run audit:links && npm run audit:markdown && npm run audit:headings"
  }
}
```

Create a new file `scripts/audit-docs.js` with the following content:

```javascript
const fs = require('fs');
const path = require('path');
const glob = require('glob');

const HEADING_LEVELS = ['#', '##', '###', '####', '#####', '######'];

function auditHeadings(files) {
  files.forEach((file) => {
    const fileContent = fs.readFileSync(file, 'utf-8');
    const lines = fileContent.split('\n');
    let currentLevel = 0;

    lines.forEach((line, index) => {
      const trimmedLine = line.trim();
      if (trimmedLine.startsWith('#')) {
        const headingLevel = HEADING_LEVELS.indexOf(trimmedLine.split(' ')[0]);
        if (headingLevel === -1) {
          console.warn(`${file}:${index + 1}: Invalid heading level`);
        } else if (headingLevel > currentLevel + 1) {
          console.warn(`${file}:${index + 1}: Heading level skipped (${HEADING_LEVELS[headingLevel]} after ${HEADING_LEVELS[currentLevel]})`);
        } else {
          currentLevel = headingLevel;
        }
      }
    });
  });
}

function main() {
  const command = process.argv[2];

  if (command === 'headings') {
    const files = glob.sync('**/*.md', { ignore: ['**/node_modules/**', '**/build/**'] });
    auditHeadings(files);
  } else {
    console.error('Invalid command. Usage: node scripts/audit-docs.js <command>');
    console.error('Available commands: headings');
  }
}

main();
```

**INSTRUCTIONS**:

1. Install the new dependencies by running `npm install`.
2. Run the audit scripts with `npm run audit:docs`.

   - `npm run audit:links` will check for broken internal and external links in Markdown files.
   - `npm run audit:markdown` will lint Markdown files according to a set of rules defined in `.markdownlint.json` (create this file if it doesn't exist, or use the default rules).
   - `npm run audit:headings` will scan all Markdown files and check for inconsistent heading levels or skipped levels.
   - `npm run audit:docs` will run all three audit scripts.

3. Review the output from the scripts and address any issues reported.
4. For duplicate or outdated sections, manually review the files and make necessary updates or removals.

The `audit:headings` script uses the `glob` module to find all Markdown files in the project, excluding `node_modules` and `build` directories. It then reads each file line by line and checks the heading levels for inconsistencies or skipped levels.

The `markdown-link-check` tool will scan Markdown files and report any broken internal or external links. The `markdownlint-cli` tool will lint Markdown files based on the rules defined in `.markdownlint.json`.

Note: You may need to customize the glob patterns or paths in the scripts to match your project structure.
```
