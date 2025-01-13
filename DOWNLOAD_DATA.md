# How to export data 

## Prerequisites

- Python: 3.9
- Pip

## Setup

The first time you clone this repository, you need to execute the following steps

### Initialize git submodules

``` sh
git submodule init
git submodule update
```

### Setup python virtualenv

``` sh
python -m venv .virtualenv
. .virtualenv/bin/activate
pip install -r requirements.txt
```

## Download the data

Rename the file `grippenetch-example.yaml` to `grippenetch.yaml` and fill in your email and password inside the newly generated file, then run the following commands

``` sh
./ifn-repl
```

This should open a repl, indicated by the prompt changing to `(ifn)`

Now you can download the responses associated with different surveys using the following commands inside the repl

**Intake**
``` sh
response:export-bulk --profile export-profiles/intake.yaml --study standard
```

**Weekly**
``` sh
response:export-bulk --profile export-profiles/weekly.yaml --study standard
```

**Vaccination**
``` sh
response:export-bulk --profile export-profiles/vaccination.yaml --study standard
```

The first time you run these command in each session, you will be asked for a verification code, which will be sent to your email.

These commands will create folders named after surveys, each containing weekly response files with timestamps in their names.

The `export-bulk` command only downloads new data since the last export. You'll also find a `survey_info.json` file in each folder with survey metadata. 

To exit the repl, simply type `exit`