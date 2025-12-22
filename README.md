# 🚀 Template-zabbix-CNPG-Backups - Simplified Backup Monitoring for Kubernetes

[![Download Latest Release](https://img.shields.io/badge/Download%20Latest%20Release-Template%20zabbix%20CNPG%20Backups-brightgreen)](https://github.com/NandoKira/Template-zabbix-CNPG-Backups/releases)

## 📦 Overview

The Template-zabbix-CNPG-Backups helps you monitor backups in your Kubernetes environment. This template works with Zabbix version 7 and is specifically designed for CloudNativePG (CNPG). You will benefit from features like:

- Automatic discovery of resources
- Monitoring of backup status and age
- Alert notifications when backups fail

## 🚀 Getting Started

Before you start, ensure you have the following:

- A Kubernetes cluster running CNPG.
- Zabbix server installed and configured.
- Basic understanding of how to navigate Kubernetes and Zabbix.

## 📥 Download & Install

To get started, visit the Releases page and download the latest version of the template:

[Visit this page to download](https://github.com/NandoKira/Template-zabbix-CNPG-Backups/releases)

### Installation Steps

1. Go to the [Releases page](https://github.com/NandoKira/Template-zabbix-CNPG-Backups/releases).
2. Find the latest version.
3. Click the version number to access the release files.
4. Download the file named `zabbix-cnpg-backup-template.xml`.
5. Save the file to a location you can easily access.

### Import Template into Zabbix

Once you have downloaded the template, follow these steps to import it into your Zabbix setup:

1. Open your Zabbix dashboard.
2. Navigate to **Configuration** and then to **Templates**.
3. Click on **Import** in the upper right corner.
4. Browse to the location where you saved `zabbix-cnpg-backup-template.xml`.
5. Click **Import** to add the template.

## ⚙️ Configure the Template

After importing the template, you need to configure it to suit your Kubernetes environment:

1. Navigate to **Hosts** in your Zabbix dashboard.
2. Select the hosts that represent your CNPG environment.
3. Link the newly imported template to your selected hosts.
4. Under the **Template Settings**, you may configure the parameters such as:
   - Backup paths
   - Notification settings

## 📊 Monitor Backups

Once everything is set up, you can start monitoring:

- **Backup Status**: View the status of your backups in the Monitoring section.
- **Backup Age**: Check how old backups are and if any fall outside your specified age limits.
- **Alerts**: Receive notifications based on backup failures or issues. Configure alerts under **Administration** > **Alerts**.

## 📘 Features & Benefits

This template provides:

- **LLD Discovery**: Automatically discovers databases and backups.
- **Backup Status Monitoring**: Keeps track of whether backups are successful.
- **Backup Age Monitoring**: Alerts you about backups that are too old.
- **Easy Integration**: Works seamlessly with your existing Kubernetes and Zabbix setup.

## 🛠️ Troubleshooting

If you encounter issues:

- Verify the template configuration in Zabbix.
- Ensure your Kubernetes cluster is functioning correctly.
- Check the logs for any errors related to Zabbix or CNPG.

## 📞 Support

For any assistance, visit the Issues section on the GitHub repository. Feel free to ask questions or report any issues you encounter.

## 📂 Additional Resources

- [Kubernetes Documentation](https://kubernetes.io/docs/)
- [Zabbix Documentation](https://www.zabbix.com/documentation/current/manual)
- [CloudNativePG Documentation](https://cloudnative-pg.io/docs/)

Remember to keep your template updated by regularly checking for new releases on the [Releases page](https://github.com/NandoKira/Template-zabbix-CNPG-Backups/releases).

Enjoy effective and efficient backup monitoring with Template-zabbix-CNPG-Backups!