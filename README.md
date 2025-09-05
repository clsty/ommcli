# ommcli
Ollama Model Manager (CLI) which imports/exports/updates Ollama models.

## Features
- Designed for Ollama both as docker compose (default) or directly on the system (`ommcli --mode host`).
- import/export(dump/backup)/update/... Multiple functions in a single script.
- Automatically skips existing models in Ollama during import and export by default.
- During export, the full model name is saved into the `ModelName` file, and it is automatically recognized during import to retrieve the correct name, allowing existing model upgrades using `ollama pull`.

## Limitations
- Though limited codes for Windows system have been applied, it has not been tested. Feedback and PR welcomed.

## Credits
- The original code of this project is modified from [AaronFeng753/Ollama-Model-Dumper](https://github.com/AaronFeng753/Ollama-Model-Dumper).

## Usage
`ommcli` is a python script.
- Run `python ./ommcli` to show help.
- You can also run `./ommcli` directly if shebang is supported.
- Or copy `ommcli` to somewhere suitable, e.g. `/usr/local/bin/`, and then you can run `ommcli` everywhere.

By default, `ommcli` assume your working directory has the target `docker-compose.yml` and use it to interact with `ollama`. If you want to use ollama on the system, use `ommcli --mode host` instead.

A copy of help from the script (maybe outdated):

```plain
ommcli: Ollama Model Manager (CLI) which imports/exports/updates Ollama models

Syntax:
          ommcli [global option] <subcommand> [subcommand option]

Global option:
  --mode <mode>      Running mode: compose (default; for Ollama as docker
                     compose project) or host (for Ollama in system)

Subcommand:
  help     Show this help
  export   Export model(s) from Ollama to backup directory
  import   Import model(s) from backup directory to Ollama
  update   Update all models used by Ollama
  list     List all models used by Ollama
  exec     Execute any command inside Ollama docker container


Options for `export`:

  --overwrite           Overwrite existing model inside the backup directory
  --model <name>        name of one model to export (by default all models)

  --c-model-dir <path>  Models directory used by Ollama in the container (default: /root/.ollama/models)
  --h-model-dir <path>  Models directory used by Ollama on the host system (default: ./data/models)
  --h-backup-dir <path> Models backup directory on the host system (default: ./ollama-model-backup)

  --model-dir <path>    Alias of `--c-model-dir <path>`
  --backup-dir <path>   Alias of `--h-backup-dir <path>`

  Attention: The last character of the path should not be `/` or `\` .
  Attention: Under `host` mode, `--h-model-dir` is ignored and `--c-model-dir` will be used instead.
        It's recommended to directly use `--backup-dir` and `--model-dir` to avoid confusion.
  Attention: Under `compose` mode, subcommand `export` requires that both the model directories on the host
        and in the container, have been mapped as same ones.


Options for `import`:

  --overwrite           Overwrite existing model used by Ollama
  --model <name>        name of one model to import (by default all models)

  --h-backup-dir <path> Models backup directory on the host system (default: ./ollama-model-backup)
  --c-backup-dir <path> Models backup directory in the container (default: /ollama-model-backup)

  --backup-dir <path>   Alias of `--h-backup-dir <path>`

  Attention: The last character of the path should not be `/` or `\` .
  Attention: Under `host` mode, `--c-backup-dir` is ignored and `--h-backup-dir` will be used instead.
        It's recommended to directly use `--backup-dir` to avoid confusion.
  Attention: Under `compose` mode, subcommand `import` requires that both the backup directories on the host
        and in the container, have been mapped as same ones.


Options for `exec`:
  -d <path>       Specify the working directory for the command to be executed.


Examples:
  Under `compose` mode, export all models:
    ommcli --mode compose export

  Under `host` mode, update all models:
    ommcli --mode host update

  Under `host` mode, import a model named `llama3`:
    ommcli --mode host import --model llama3


Attention for Windows:
  1. The script is developed under Linux.
     It's also for Windows as a wish, and some lines are written for Windows,
     but this has not been actually tested.
     Feedback welcomed on https://github.com/clsty/ommcli
  2. In the path please use `/` or `\\`, and avoid `\`.
  3. For path inside the container, use `/`
```
