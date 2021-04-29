# How to Check If Everything is Working (or Not)
:hammer_and_wrench: **A work-in-progress** :hammer_and_wrench:

This doc assumes you know how to use the CLI. Using the CLI is the best way to troubleshoot (and to do everything Chia too). The [Quick Start Guide](https://github.com/Chia-Network/chia-blockchain/wiki/Quick-Start-Guide) and [CLI Commands Reference](https://github.com/Chia-Network/chia-blockchain/wiki/CLI-Commands-Reference) have useful info to get you familiar with the CLI.

## Where to Find Things: The File Paths
The file paths for Linux, macOS, and Windows versions of Chia are similar. 
```
/home/user
├─ .chia/
│   └── mainnet/
│      ├─ config/
│      │      ├─ config.yaml
│      │      └─ ssl/
│      │            └─ (and more...)
│      ├─ db/
│      ├─ log/
│      │      └─ debug.log
│      ├─ run/
│      │      └─ (and more...)
│      └─ wallet/
│             └─ (and more...)
└── /chia-blockchain
       └─ (and more...)
```

### Linux & macOS
* Chia config: `~/.chia/mainnet/config/config.yaml`
* Chia log: `~/.chia.mainnet/log/debug.log`

### Windows
* Chia config:  `C:\Users\USERNAME\.chia\mainnet\config\config.yaml`
* Chia log:  `C:\Users\USERNAME\.chia\mainnet\log\debug.log`

# Logs
In `config.yaml` you can set the level of detail for your logs. It’s useful to change the logger settings from `WARNING` to `INFO` to get the detail needed to troubleshoot.

You can run `grep`  (Linux, macOS) or `Select-String` (Windows) to search through your logs for relevant information. 

# Is It Working?
## Harvester
You want your the time it takes to check your plot for a proof to be under 5 seconds. If you see higher times, something is wrong with your setup.

You can use these commands to narrow down your search for problems in `debug.log`

* Linux/macOS: `tail ~/.chia/mainnet/log/debug* | grep eligible`
* Windows:
	* `Select-String -Path “~\.chia\mainnet\log\debug*” -Pattern “eligible”`
	* `Select-String -Path “~\.chia\mainnet\log\debug*” -Pattern “Found [^0] proof”`
	* `Select-String -Path “~\.chia\mainnet\log\debug*” -Pattern “Farmed unfinished_block`

## Plotting
* With the CLI, run the command  `chia plots check` 