Export a vault note to a clean PDF using md-to-pdf.

If $ARGUMENTS is provided, use it as the file path relative to the vault root.
If no argument is provided, ask the user which file to export.

Run this bash command:
```
cd "${VAULT_ROOT:-$HOME/Library/CloudStorage/OneDrive-3Cloud/3Cloud}" && md-to-pdf "$ARGUMENTS"
```

The PDF will be saved in the same folder as the source .md file with the same filename.

Notes:
- md-to-pdf must be installed: npm install -g md-to-pdf
- Works on any .md file in the vault
- Output goes to same directory as the source file
