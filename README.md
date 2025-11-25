# CFM-ID Log and MSP Spectra Processing Notebook

This notebook provides a complete workflow for converting CFM-ID log files to MSP format and then visualizing the resulting mass spectra as PNG images.

## 1. Convert CFM-ID Log Files to .msp files

This section contains Python code to parse CFM-ID `.log` files, extract spectral data (m/z, intensity, annotations, metadata), and convert them into the MSP (Mass Spectral Peak) format. It supports combining all spectra into a single `.msp` file or creating separate `.msp` files for each input log file.

**Key Functions:**
- `parse_cfm_log(filepath)`: Reads a CFM-ID log file and extracts metadata, energy-dependent spectra, and fragment mapping.
- `normalize_peaks(peaks)`: Normalizes peak intensities to a maximum of 100.
- `energy_to_collision(energy)`: Maps CFM-ID energy labels (energy0, energy1, energy2) to approximate collision energies.
- `create_msp_entry(spectrum, filename_stem)`: Formats a single spectrum into an MSP entry string.
- `process_log_files(log_files, output_mode)`: Orchestrates the parsing and MSP file generation for a list of log files.

**Usage:**
- The script first checks for uploaded `.log` files in Colab. If none are uploaded, it scans the `/content/` directory.
- It processes the found `.log` files and generates one or more `.msp` files in the `/content/` directory.
- Finally, it downloads the generated `.msp` files if running in Colab.

## 2. Generate Spectra .png files

This section provides code to generate high-quality PNG plots of mass spectra from `.msp`, `.log`, or `.txt` files. It includes functionalities for plotting individual spectra and creating mirror (head-to-tail) comparison plots.

**Key Features:**
- **Flexible Input**: Supports `.msp` files (recommended) and CFM-ID `.log`/`.txt` files.
- **Normalization**: Peak intensities are normalized to 100% relative to the base peak.
- **Customizable Plotting**: Adjustable figure size, DPI, and number of top peaks to annotate.
- **Output Management**: Plots are saved to a specified output directory (`/content/spectra_plots/`).
- **Mirror Plotting**: Ability to generate head-to-tail comparison plots between consecutive spectra.

**Key Functions:**
- `parse_msp_file(filepath)`: Parses an MSP file to extract individual spectra and their metadata.
- `parse_cfm_log(filepath)`: (Re-used/re-defined) Parses CFM-ID log files to extract spectra.
- `normalize_spectrum(peaks)`: Normalizes peak intensities and sorts peaks by m/z.
- `plot_spectrum(spectrum, output_path)`: Generates and saves a single spectrum plot with annotations and metadata.
- `plot_mirror_spectrum(spectrum1, spectrum2, output_path)`: Generates and saves a mirror plot comparing two spectra.
- `process_file(filepath, create_mirror)`: Main function to process a single input file, parse spectra, and generate plots.

**Usage:**
- The script detects spectrum files by checking uploaded files or scanning the `/content/` directory.
- It iterates through each found file, parses its spectra, and generates individual PNG plots.
- If `CREATE_MIRRORS` is set to `True`, it also generates mirror plots for consecutive spectra.
- All generated plots are compressed into a `spectra_plots.zip` file, which is then downloaded in Colab.
- A preview of the first few generated plots is displayed in the notebook output.
