# NIA CARD Long Read Cramino QC Parser and Dashboard Generator
We often collect read mapping statistics for both small and large cohorts using the quality assessment tool cramino (https://github.com/wdecoster/cramino) in the NanoPack suite (https://github.com/wdecoster/nanopack). However, NanoPack lacks a tool that is useful for examining read mapping statistics of large numbers of samples, or between groups of samples. NanoPlot, for example, is best intended for comparing individual samples against each other. We thus developed a method to parse large numbers of cramino outputs at once, calculate descriptive statistics for each cramino measure (e.g., yield over 25 kb, N50), and generate swarm/violinplots of each property based on all samples. The two scripts below are used in sequence to parse cramino QC output in bulk and generate an analytics dashboard from the bulk summary.
## Dependencies
The Python scripts were tested and developed with the following dependency modules (and respective versions where applicable):

Python 3.10.8  
numpy 1.26.4  
pandas 2.0.3  
seaborn 0.12.2  
matplotlib 3.7.2  
json 2.0.9  
argparse 1.1  
dateutil 2.8.2  
openpyxl 3.1.2  
xlsxwriter 3.1.2  
datetime  
dateutil  
statistics  
dataclasses  
glob  
io  
## Usage
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
Output fields of the parser-generated summary table include the following:

Filename, Number of alignments, Percent of total reads, Yield (Gb), Mean Coverage, Yield (Gb) [>25kb], N50, N75, Median length, Mean length, Median identity, Mean identity, Median mapping Q score, Mean mapping Q score

```
usage: CARDlongread_cramino_dashboard.py [-h] [-input INPUT_FILE [INPUT_FILE ...]] [-names [NAMES ...]] [-output OUTPUT_FILE] [-plot_title PLOT_TITLE] [--plot_cutoff | --no-plot_cutoff] [-run_cutoff RUN_CUTOFF]
                                         [--strip_plot | --no-strip_plot] [-colors [COLORS ...]] [-legend_colors [LEGEND_COLORS ...]] [-legend_labels [LEGEND_LABELS ...]] [--group_count | --no-group_count]

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
  -colors [COLORS ...]  Color palette corresponding to sequential groups displayed (e.g., 'blue', 'red', 'blue'); optional and used only if more than one tsv provided.
  -legend_colors [LEGEND_COLORS ...]
                        Colors shown in the legend (e.g., 'blue', 'red'); optional and used only if more color palette included above. Must be palette subset.
  -legend_labels [LEGEND_LABELS ...]
                        Labels for each color in legend in order specified in -legend_colors.
  --group_count, --no-group_count
                        Show group count in x-axis labels (optional; default false) (default: False)
```
## Tutorial
The commands provided below offer a quick start with a set of 15 cramino outputs from a small cohort (paths given are for outputs on NIH Biowulf HPC cluster). To clone from GitHub and do a test run with example data, run the following commands:
```bash
# Download this repo
git clone https://github.com/molleraj/CARDlongread-cramino-dashboard.git
cd CARDlongread-cramino-dashboard

# create summary table from list of cramino reports
python CARDlongread_cramino_parser.py --bam_type mapped_bam --filelist example_cramino_list.txt --output example_cramino_summary.tsv

# create dashboard from cramino output
python CARDlongread_cramino_dashboard.py -input example_cramino_summary.tsv -output example_cramino_dashboard.xlsx -plot_title "Chile tutorial example" 
```
Example alignment QC visualizations from tutorial summary spreadsheet:

Mean reference (GRCh38) coverage violinplot with embedded boxplot and overlayed swarmplot (30x cutoff marked in red):  
<img width="720" alt="image" src="https://github.com/user-attachments/assets/8101294d-28b3-4393-92e7-c5ce5aca7a4b" />  

Alignment N50 violinplot with embedded boxplot and overlayed swarmplot:  
<img width="720" alt="image" src="https://github.com/user-attachments/assets/4c8a85ee-8fdf-4a17-a43f-980786f14bba" />  

Median mapping Q score (quality score; -10 log (error rate)) violinplot with embedded boxplot and overlayed swarmplot:  
<img width="720" alt="image" src="https://github.com/user-attachments/assets/af8c88ac-e4fb-4edb-99c1-743cd852de5f" />

## Comparing QC metrics across groups
As we described in the [raw QC report parser and dashboard repository](https://github.com/molleraj/CARDlongread-report-parser), it is often advantageous to compare cramino QC metrics across different groups of mapped and unmapped BAMs. We thus implemented group comparison functionality available through the ```-input [INPUT_FILE ...]```, ```-names [NAMES ...]```, and/or ```-colors [COLORS ...]``` command line options. These options take a list of files along with corresponding names and colors to be applied to each input file, in the order given for the ```-input``` option. We have provided an additional tutorial below demonstrating group comparison with custom coloring and labeling for 20 sequencing runs randomly selected from each of five different cohorts. Cohorts are colored and labeled based on sample type (blood in red, brain in blue, colors from tableau palette). Cohorts are set in order to corresponding brain/blood colors with ```-colors```, while the legend is set to blood/brain and red/blue with ```-legend_colors``` and ```-legend_labels```, respectively. We also provide a command to generate a companion dashboard based on the same cohorts with default coloring. Paths provided in cramino output list files are paths to corresponding cramino outputs on the NIH Biowulf HPC cluster. Input and output files for the group comparison tutorial are provided in the provided ```group_comparison``` folder.

```bash
# run in CARDlongread-report-parser directory
cd CARDlongread-report-parser

# prepare input tables for each cohort
# cohort json lists include 20 randomly selected JSONs per cohort
# cohort 1
python CARDlongread_cramino_parser.py --bam_type mapped_bam --filelist group_comparison/cohort_1_cramino_list.txt --output group_comparison/cohort_1_output.tsv
# cohort 2
python CARDlongread_cramino_parser.py --bam_type mapped_bam --filelist group_comparison/cohort_2_cramino_list.txt --output group_comparison/cohort_2_output.tsv
# cohort 3
python CARDlongread_cramino_parser.py --bam_type mapped_bam --filelist group_comparison/cohort_3_cramino_list.txt --output group_comparison/cohort_3_output.tsv
# cohort 4
python CARDlongread_cramino_parser.py --bam_type mapped_bam --filelist group_comparison/cohort_4_cramino_list.txt --output group_comparison/cohort_4_output.tsv
# cohort 5
python CARDlongread_cramino_parser.py --bam_type mapped_bam --filelist group_comparison/cohort_5_cramino_list.txt --output group_comparison/cohort_5_output.tsv

# make dashboard for all five cohorts, coloring cohorts by sample type (blood or brain)
# overlay violinplots with strip plots instead of beeswarm plots
python CARDlongread_cramino_dashboard.py \
  -input group_comparison/cohort_1_output.tsv group_comparison/cohort_2_output.tsv group_comparison/cohort_3_output.tsv group_comparison/cohort_4_output.tsv group_comparison/cohort_5_output.tsv \
  -names "Cohort 1" "Cohort 2" "Cohort 3" "Cohort 4" "Cohort 5" \
  -colors "tab:blue" "tab:red" "tab:blue" "tab:blue" "tab:blue" \
  -legend_colors "tab:red" "tab:blue" \
  -legend_labels "Blood" "Brain" \
  -plot_title "Group comparison tutorial with custom colors and legend" \
  -output group_comparison/five_cohort_sample_comparison_dashboard_custom_colors.xlsx \
  --strip_plot

# make dashboard as above, but don't use custom color/labeling options
# instead use 'top up' colors for runs and seaborn defaults for output per flow cell/per experiment
python CARDlongread_cramino_dashboard.py \
  -input group_comparison/cohort_1_output.tsv group_comparison/cohort_2_output.tsv group_comparison/cohort_3_output.tsv group_comparison/cohort_4_output.tsv group_comparison/cohort_5_output.tsv \
  -names "Cohort 1" "Cohort 2" "Cohort 3" "Cohort 4" "Cohort 5" \
  -plot_title "Group comparison tutorial with default output" \
  -output group_comparison/five_cohort_sample_comparison_dashboard_default_colors.xlsx \
  --strip_plot
```
Below are sample grouped violinplots with overlayed strip plots from the custom colored and default colored dashboards above, respectively.  

<img width="720" alt="image" src="https://github.com/user-attachments/assets/30eb1ec2-bf6f-4d65-add7-4367cba17e6f" />
<img width="720" alt="image" src="https://github.com/user-attachments/assets/7b5fea32-242f-4534-86e2-0d5d02c46874" />

