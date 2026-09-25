|                    | **MARS**                                    | **MABS**                                                |
| ------------------ | ------------------------------------------- | ------------------------------------------------------- |
| Stands for         | Microsoft **Azure Recovery Services Agent** | Microsoft **Azure Backup Server**                       |
| Use when           | **Directly** backing up a server to Azure   | Need a **central backup server** for multiple workloads |
| Installed on       | Protected Windows machine                   | Dedicated backup server                                 |
| Backup destination | Azure                                       | MABS → Azure                                            |
| Best for           | Files, folders, system state                | VMs, applications, multiple servers                     |
| Infrastructure     | **Less** infrastructure                     | **More** infrastructure                                 |

[[MARS]] is the simpler choice. But MABS provides capabilities that MARS doesn't, particularly around centralised backup management and broader workload protection.