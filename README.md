# kafka_librarydata
This coding challenge uses Brooklyn library data (not provided).

The purpose of this pipeline is to read a json file into python, do some cleaning, transform it into a specific format, produce it as a kafka message, consume it, then write it to a csv. Kafka is being run locally with one broker and one partition; it is not efficient, just a proof of concept to demonstrate kafka ingestion.

## Requirements
This pipeline requires python3, zookeeper, and kafka

**The following python packages are required for librarydata_preprocess.py**:  
configparser  
json
csv
time
kafka (kafka-python)
uuid

## Config
You will need to create a .ini file as a config with the following sections:

**[config]**    
header=comma-separated list of the variables (column names)  
datapath=path for the json input file  
subset=if the json file has a key embedding all of the data, this is to subset the values of that key out  
outputpath=path for the csv output file  
rowkey=if the individual rows have a key embedding the data, this is to subset the values of that key out  
rename_from=names of variables to be renamed from json to csv, comma-separated  
rename_to=the new variable names in the same order as above, comma-separated  
split_from=variables that need to be split e.g. splitting begin and end times, comma-separated  
split_to=the new variables from the split, comma-separated within split variables and semicolon-separated between split variables e.g. monday, tuesday in split_from might look like monday_start, monday_end; tuesday_start, tuesday_end  
splitter=the separator e.g. blank, - , (an actual comma) , ; , etc.  
kafkahost=localhost:9092 (or your kafka server and port)
kafkatopic=library_data (or preferred kafka topic name)

**configpath.py**  
A python script that only contains a string with the path for the .ini file. My .ini file is in the main directory but it should be flexible.

example:  
'your preferred python shebang'  
configpath = "path/configname.ini"  

## Start Zookeeper and Kafka
An instance of kafka (in this case run locally) must be running in order to produce and consume kafka messages, and kafka requires zookeeper. Instructions for how to properly install and configure zookeeper and kafka are not included in this readme.

zookeeper_start.bat opens a command prompt window running zookeeper. 

kafka_start.bat opens a command prompt window running kafka.


## Preprocess (librarydata_preprocess.py)
This python pipeline loads a configuration file, loads a json input file, creates a writer to a csv output file, cleans and transforms the data, produces kafka messages and consumes them, and then writes to the csv.

Within the library_clean function, variables are renamed, split, removed, a processing date stamp is added, and the transformed data are ingested through kafka and written to a csv.