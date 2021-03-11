# README #

This idea was born because of the need for a simple tool in order to automate execution of simple log analysis obtained from Nokia SROS routers. The tool reads the content of a log in txt format, parses only the necessary information and compares it, thus verifying if there are modifications in the specific values. The logs are generated by the use of [taskAutom](https://github.com/laimaretto/taskAutom).

## Setup ##

#### System Libraries
These libraries have been tested under Ubuntu 20.04 and Python3.8.

```bash
sudo pip3 install -r requirements.txt
```

#### Compile
You can run `logAnalyzer` directly from the CLI using Python. However, compiling improves performance.

```bash
python3 -m nuitka logAnalizer.py
```
Compiling has been tested succesfully under Ubuntu. Don't know if this is directly supported under Windows. If it fails, let me know. Nevertheless, as mentioned, you can run `logAnalyzer_win` directly from the CLI using Python

## Usage ##

The program needs three inputs: a) CSV file with the list of parsing templates to be considered for the analysis; b) a folder, named `Templates`, which contains the parsing templates; and c) a folder, which contains the logs from `taskAutom`.

#### CSV

The CSV file must have in its first column, the name of templates created for the specific commands

```csv
nokia_sros_show_router_bgp_summary.template
nokia_sros_show_router_interface.template
nokia_sros_show_service_sdp.template
```

#### Templates

The templates are stored in the `Templates` folder. The script reads the CSV and uses the indicated templates to perform the function of parsing.

#### Results

If `logAnalyzer` is invoked with folder `pre` only, reads the specific content in the log folder for a given command and then saves the results in an Excel report.

```bash
$ python3 logAnalyzer.py -csv templateExample.csv -pre folderLogs/
<_io.TextIOWrapper name='Templates/nokia_sros_show_service_sdp-using.template' mode='r' encoding='UTF-8'>
#####Plantillas Cargadas Exitosamente#####
#########Logs Cargados Exitosamente#########
ROUTER_EXAMPLE_rx.txt nokia_sros_show_service_sdp-using.template
#
#
Guardando
```

Otherwise, if `logAnalyzer` is invoked with folder `pre` and `post`, it compares the content of pre and post log folders, such as if we run checks to see the status of the routers before and after a task, and then saves the results in an Excel report.

```bash
$ python3 logAnalyzer.py -csv templateExample.csv -pre folderLogsBefore/ -post folderLogsAfter/
<_io.TextIOWrapper name='Templates/nokia_sros_show_service_sdp-using.template' mode='r' encoding='UTF-8'>
#####Plantillas Cargadas Exitosamente#####
#########Logs Cargados Exitosamente#########
#########Logs Cargados Exitosamente#########
ROUTER_EXAMPLE_rx.txt nokia_sros_show_service_sdp-using.template
ROUTER_EXAMPLE_rx.txt nokia_sros_show_service_sdp-using.template
#
#
Guardando
```

#### Configuration Options

`logAnalyzer` can be configured through CLI as shown below.

```bash
$ python3 logAnalyzer.py -h
usage: PROG [options]

Log Analysis

optional arguments:
-h, --help            show this help message and exit
-pre PREFOLDER, --preFolder PREFOLDER
                      Folder with PRE Logs. Must end in "/"
-post POSTFOLDER, --postFolder POSTFOLDER
                      Folder with POST Logs. Must end in "/"
-csv CSVTEMPLATE, --csvTemplate CSVTEMPLATE
                      CSV con with templates to use in parsing.
```
