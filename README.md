# NIA CARD Long Read Cramino QC Parser and Dashboard Generator
Tools to parse cramino QC output in bulk and generate an analytics dashboard from the bulk summary.
```
usage: CARDlongread_cramino_parser.py [-h] [--cramino_dir CRAMINO_DIR] [--filelist FILELIST] [--output OUTPUT_FILE]

Extract data from long read cramino mapping QC reports into summary table

optional arguments:
  -h, --help            show this help message and exit
  --cramino_dir CRAMINO_DIR
                        path to directory containing cramino files, if converting whole directory
  --filelist FILELIST   text file containing list of all cramino reports to parse
  --output OUTPUT_FILE  Output long read cramino report summary table in tab-delimited format
```
```
usage: CARDlongread_cramino_dashboard.py [-h] [-input INPUT_FILE] [-output OUTPUT_FILE] [-plot_title PLOT_TITLE] [--plot_cutoff | --no-plot_cutoff] [-run_cutoff RUN_CUTOFF]

This program gets summary statistics from long read sequencing report data.

optional arguments:
  -h, --help            show this help message and exit
  -input INPUT_FILE     Input tab-delimited tsv file containing features extracted from long read sequencing reports.
  -output OUTPUT_FILE   Output long read sequencing summary statistics XLSX
  -plot_title PLOT_TITLE
                        Title for each plot in output XLSX (optional)
  --plot_cutoff, --no-plot_cutoff
                        Include cutoff lines in violin plots (optional; default true; --no-plot_cutoff to override) (default: True)
  -run_cutoff RUN_CUTOFF
                        Minimum data output per flow cell run to include (optional, 1 Gb default)
```
