# Variant Structure Analyzer

A browser-based tool for analyzing genetic variants in their structural context. Built for clinical geneticists evaluating variants of uncertain significance (VUS).

🔗 **Live Demo:** [https://dpique.github.io/variant-structure-analyzer](https://dpique.github.io/variant-structure-analyzer)

## Features

- **Smart Structure Discovery**: Enter any gene symbol → see all available PDB structures with descriptions
- **Structure Selection**: Choose from experimental structures (X-ray, Cryo-EM) or AlphaFold predictions
- **Clinically Relevant Tags**: Structures tagged as Apo, Substrate-bound, Mutant, or Complex
- **Functional Annotations**: Active sites and binding sites automatically highlighted from UniProt
- **ClinVar Integration**: Pathogenic variants in the region shown with distance to your variant
- **Neighborhood Analysis**: All residues within adjustable radius, sorted by distance
- **Chemistry-Aware**: Highlights histidines (acid-base catalysis), positive charges (substrate binding), etc.
- **Key Finding Generation**: Automatic detection of significant patterns (e.g., "only histidine in region")

## Use Cases

1. **VUS Evaluation**: Determine if a variant is near functional sites
2. **Active Site Analysis**: Check if a residue could participate in catalysis
3. **Pathogenic Clustering**: See if other pathogenic variants cluster in the same region
4. **Domain Context**: Understand structural environment of the variant

## How to Use

1. Enter a **gene symbol** (e.g., `BRCA1`, `TP53`) or **UniProt protein ID** (e.g., `P38398`)
   - Gene symbols are automatically converted to the corresponding canonical protein
   - Prefers reviewed Swiss-Prot entries over unreviewed TrEMBL entries
2. **Select a structure** from the list:
   - **AlphaFold**: AI-predicted full-length structure (good for overall context)
   - **PDB experimental**: X-ray, Cryo-EM structures (more reliable but may be partial or contain mutations/ligands)
   - Click any structure to select it (look for the green highlight)
3. Enter the **residue number** of your variant (using UniProt canonical numbering)
4. Click **Analyze Variant** to load the 3D structure and analysis

### What You'll See

- **3D Structure** with variant highlighted in bright red
- **Neighbor Table**: All residues within adjustable radius, sorted by distance
- **Annotations Tab**: UniProt functional annotations
- **ClinVar Tab**: Known pathogenic variants nearby (shown in magenta)
- **Key Finding**: Automated analysis of significant features

### View Controls

The tool includes powerful visualization controls:

**Quick Views:**
- 🔍 **Overview** - Full protein view
- 🎯 **Variant Site** - Zoom to your variant of interest
- ⚡ **Active Sites** - Zoom to annotated catalytic residues
- **H All His** - Highlight all histidines in the structure

**Toggle Features:**
- ✓/✗ **Pathogenic Variants** - Show/hide ClinVar pathogenic variants (magenta)
- ✓/✗ **Ligands/RNA** - Show/hide bound molecules and nucleic acids

**Protein Style:**
- **Cartoon** - Traditional ribbon representation (default)
- **Surface** - Molecular surface (70% transparent)
- **Ribbon** - Smooth ribbon backbone

**Color Scheme:**
- **Bright Red** (#ff3b3b) - Your variant of interest
- **Magenta** (#e066ff) - Pathogenic ClinVar variants
- **Blue** (#58a6ff) - Active site residues
- **Cyan** (#7dd3fc) - Binding site residues
- **Gold** (#ffd700) - Positively charged residues
- **Orange** (#ff9f43) - Flanking residues

## Example Queries

| Gene  | Residue | Why It's Interesting |
| ----- | ------- | -------------------- |
| BRCA1 | 1699    | In BRCT domain       |
| TP53  | 248     | Mutational hotspot   |
| CFTR  | 508     | ΔF508 region         |

## Deployment to GitHub Pages

1. Fork this repository
2. Go to Settings → Pages
3. Set source to "main" branch, root folder
4. Your site will be live at `https://YOUR_USERNAME.github.io/variant-structure-analyzer`

## Technical Details

**All computation runs in the browser.** No server required.

### APIs Used

- [UniProt REST API](https://www.uniprot.org/help/api) - Protein annotations
- [AlphaFold API](https://alphafold.ebi.ac.uk/api-docs) - Predicted structures
- [RCSB PDB](https://www.rcsb.org/) - Experimental structures
- [NCBI E-utilities](https://www.ncbi.nlm.nih.gov/books/NBK25500/) - ClinVar variants

### Libraries

- [3Dmol.js](https://3dmol.csb.pitt.edu/) - Molecular visualization

## Limitations

**This tool provides computational evidence only.**

- Structural proximity does not establish pathogenicity
- AlphaFold predictions are not experimental structures
- ClinVar data may be incomplete for some genes
- Always combine with other ACMG/AMP evidence types

## Troubleshooting

### Common Issues

**Wrong protein returned for gene symbol**
- The tool now prioritizes reviewed Swiss-Prot entries over unreviewed TrEMBL entries
- For genes with multiple isoforms, the canonical isoform is selected
- Check the displayed UniProt ID to verify it's the correct protein
- If the wrong protein is returned, try searching by the specific UniProt ID instead (e.g., `P38398` for BRCA1)
- Example: `BRCA1` → should resolve to `P38398`, not `A0A2R8Y7V5` (unreviewed fragment)

**"AlphaFold structure not available"**
- Not all proteins have AlphaFold predictions available
- The tool now automatically checks availability and falls back to experimental structures when possible
- If no structures are available, try searching for a related protein or isoform

**"PDB search failed" or "Warning: PDB search unavailable"**
- The RCSB PDB API may experience temporary outages
- The tool will continue with AlphaFold predictions only
- Try refreshing the page and searching again in a few minutes
- Check the browser console (F12) for detailed error messages

**"No structures available for [gene]"**
- The gene may not have any structural data in PDB or AlphaFold
- Verify the gene symbol is correct (use official HGNC symbols)
- Try searching by UniProt ID instead of gene symbol
- Some proteins lack structural coverage

**"Residue not found in structure"**
- The residue number may be outside the structured region
- AlphaFold predictions may not cover the full protein sequence
- Try an experimental structure if available
- Verify the residue numbering matches the UniProt canonical sequence

**Slow loading or timeouts**
- Large structures (>1000 residues) may take longer to render
- The tool includes automatic retry logic with exponential backoff
- Check your internet connection
- Try clearing the browser cache and reloading

### Performance Optimizations

The tool now includes several optimizations:
- **Response caching**: API responses are cached to reduce redundant requests
- **Retry logic**: Failed API calls are automatically retried (up to 3 attempts)
- **Load protection**: Button debouncing prevents accidental double-loading
- **Graceful degradation**: The tool continues working even if some APIs fail
- **Automatic fallback**: If AlphaFold fails, experimental structures are tried automatically

## For Clinical Use

This tool is intended to **support**, not replace, clinical variant interpretation. Key findings should be:

1. Validated against primary literature
2. Combined with population data (gnomAD)
3. Considered alongside segregation and functional data
4. Reviewed by qualified clinical geneticists

## Citation

If you use this tool in research or clinical reports, please cite:

```
Variant Structure Analyzer
https://github.com/dpique/variant-structure-analyzer
```

## License

MIT License - free for academic and clinical use.

## Recent Improvements (v1.2)

### New Visualization Features ✨
- **Toggle Pathogenic Variants** - Show/hide ClinVar pathogenic variants independently
- **Toggle Ligands/RNA** - Show/hide bound molecules and nucleic acids as sticks
- **Protein Style Options** - Switch between Cartoon, Surface, and Ribbon representations
- **Improved Color Scheme** - Pathogenic variants now in magenta (#e066ff) to clearly distinguish from variant of interest (bright red #ff3b3b)
- **Interactive View Controls** - Quick buttons for Overview, Variant Site, Active Sites, and All Histidines

### Gene/Protein Handling
- **Fixed**: Gene symbols now correctly map to canonical proteins (e.g., `BRCA1` → `P38398` instead of wrong TrEMBL entries)
- Prioritizes reviewed Swiss-Prot entries over unreviewed TrEMBL entries
- Direct UniProt ID input now supported (e.g., `P38398`)
- Auto-detection of whether input is gene symbol or UniProt ID
- Clearer display of gene name vs. protein ID in results

### Structure Selection
- **Improved**: Users can now clearly see and select between AlphaFold and PDB experimental structures
- Visual distinction between structure types (purple for AlphaFold, blue for PDB)
- Status messages show exactly what was found (e.g., "Found 6 structures: AlphaFold + 5 PDB")
- PDB search now uses more robust API with fallback methods

### Reliability Enhancements
- AlphaFold availability is now checked before display to prevent 404 errors
- Automatic fallback from AlphaFold to experimental structures when loading fails
- Better error messages show specific issues instead of generic failures
- Retry logic with exponential backoff for all API calls (up to 3 attempts)
- Response caching reduces redundant API requests

### User Experience
- Progress indicators show each step (4 steps total): UniProt lookup, PDB search, structure details, AlphaFold check
- Button debouncing prevents accidental double-clicks
- Graceful degradation: tool continues working even if some APIs fail
- More informative status messages throughout the workflow
- Better validation of all API responses before processing
- Structure list clearly labeled "(click to select)"

### Bug Fixes
- **Fixed**: Wrong proteins being returned for common gene symbols
- **Fixed**: Multiple simultaneous structure loads causing errors
- **Fixed**: Silent failures in annotation and ClinVar fetches
- **Fixed**: PDB search 400 errors with improved API query format
- Added validation for empty or corrupted structure files
- Better handling of proteins without AlphaFold predictions

## Contributing

Issues and pull requests welcome! Particularly interested in:

- Additional annotation sources
- Disease-specific variant databases
- Improved key finding algorithms
- Testing with edge cases and rare proteins
