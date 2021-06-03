🔧  Work in progress 🔨 

For all operating systems (OS), the location for you Chia directories (folders) that contain the database, configuration, logs, etc. is in directories inside your home folder.

# Home Directory
Most if not all of the Chia software's folders and data will be in your home directory. While the concept is similar across operating systems, the details differ.:

|OS|Home Directory|
|---|---|
|Linux/Ubuntu|`~/` or `/home/yourusername`|
|macOS|`~/` or `/Users/yourusername`|
|Windows|`C:\Users\yourusername`|

## Home Directory General Structure

For Linux, Windows, and macOS, the configuration files are in a hidden `.chia` folder. The application file location varies depending on your operating system.

### Unix-based systems (Linux, Ubuntu, etc.)
```
/home/user
├─  chia-blockchain/
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