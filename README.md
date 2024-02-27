# grippenet.ch resources

## Initial setup

### Initialize git submodules

``` sh
git submodule init
git submodule update python-ifncli
```

### Setup a python virtualenv

``` sh
python -m venv .virtualenv
. .virtualenv/bin/activate
pip install -r requirements.txt
```

### Setup access variables and credentials

In order to access a deploy environment you will have to create the corresponding `yaml` files containing access variables and secrets, eg: to access the `grippenetch` deploy you should create a file named `grippenetch.yaml`:

``` yaml
management_api_url: "https://grippenetch.org/admin"
participant_api_url: "https://grippenetch.org/api"
user_credentials:
  email: "your_email_here"
  password: "your_password_here"
  instanceId: switzerland
resources_path: ../
vars:
  banner: ""
  domain: "grippenet.ch"
```

## Export responses from a deploy environment

### Select a deploy environment

To select a deploy environment, eg: `grippenetch`, from all the environments defined inside the `contexts.yaml` file:

``` sh
./ifn-repl
(inf) config:switch grippenetch
```

### Run an export profile

``` sh
./ifn-repl
(inf) response:download --study-key standard --profile export-profiles/[profile-name].yaml
```

Where `profile-name` is the name of one of the export profiles included in this repository, eg:

``` sh
./ifn-repl
(inf) response:download --study-key standard --profile export-profiles/intake.yaml
```

or a custom export profile.

After running the `response:download` command an export folder containing the exported data will be created in the root of the repository.

## Upload resources to a deploy environment

### Update the studies submodule

``` sh
git submodule update --init --recursive
```

### Build study and surveys

To build a specific study, eg: `standard`

``` sh
cd studies
yarn install
yarn generate
```

The `yarn generate` command must be run each time the study source files are modified.

### System email templates

``` sh
./ifn-repl
(inf) email:import-templates
```

### Custom email template

``` sh
./ifn-repl
(inf) email:import-template --study standard custom_template
```

### Auto messages

To upload a specific auto message, eg: `weekly_reminder`:

``` sh
./ifn-repl
(inf) email:import-auto weekly_reminder
```

### Studies

To create a specific study, eg: `standard`:

``` sh
./ifn-repl
(inf) study:create --study-key standard [--secret-from secret]
```

To update a specific study's rules, eg: `standard`:

``` sh
./ifn-repl
(inf) study:update-rules --study_key standard --default
```

### Surveys

To upload a specific survey, eg: `intake`, inside a study, eg: `standard`:

``` sh
./ifn-repl
(inf) study:import-survey --from-name standard/intake
```

### Run custom rules

``` sh
study:custom-rules --rules ./studies/output/standard/customRules/[rule_name].json --study standard --all
```

## Additional references:

- python-ifncli [documentation](python-ifncli/readme.md)