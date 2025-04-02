# NIA CARD Long Read Cramino QC Parser and Dashboard Generator
We often collect read mapping statistics for both small and large cohorts using the quality assessment tool cramino (https://github.com/wdecoster/cramino) in the NanoPack suite (https://github.com/wdecoster/nanopack). However, NanoPack lacks a tool that is useful for examining read mapping statistics of large numbers of samples, or between groups of samples. NanoPlot, for example, is best intended for comparing individual samples against each other. We thus developed a method to parse large numbers of cramino outputs at once, calculate descriptive statistics for each cramino measure (e.g., yield over 25 kb, N50), and generate swarm/violinplots of each property based on all samples. The two scripts below are used in sequence to parse cramino QC output in bulk and generate an analytics dashboard from the bulk summary.
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
usage: CARDlongread_cramino_dashboard.py [-h] [-input INPUT_FILE [INPUT_FILE ...]] [-names [NAMES ...]] [-output OUTPUT_FILE] [-plot_title PLOT_TITLE] [--plot_cutoff | --no-plot_cutoff] [-run_cutoff RUN_CUTOFF]
                                         [--strip_plot | --no-strip_plot]

This program gets summary statistics from long read sequencing report data.

optional arguments:
  -h, --help            show this help message and exit
  -input INPUT_FILE [INPUT_FILE ...]
                        Input tab-delimited tsv file containing features extracted from long read sequencing reports.
  -names [NAMES ...]    Names corresponding to input tsv file(s); required if more than one tsv provided.
  -output OUTPUT_FILE   Output long read sequencing summary statistics XLSX
  -plot_title PLOT_TITLE
                        Title for each plot in output XLSX (optional)
  --plot_cutoff, --no-plot_cutoff
                        Include cutoff lines in violin plots (optional; default true; --no-plot_cutoff to override) (default: True)
  -run_cutoff RUN_CUTOFF
                        Minimum data output per flow cell run to include (optional, 1 Gb default)
  --strip_plot, --no-strip_plot
                        Show strip plots instead of swarm plots inside violin plots (optional; default false) (default: False)
```
