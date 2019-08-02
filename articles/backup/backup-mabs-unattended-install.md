---
title: Azure Backup Sunucusu v2 sessiz yüklemesi
description: Azure Backup Sunucusu v2 'yi sessizce yüklemek için bir PowerShell betiği kullanın. Bu tür bir yüklemeye katılımsız yükleme de denir.
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: dacurwin
ms.openlocfilehash: 4bce7aeebee729ed253d39720ef520880c261a22
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68639653"
---
# <a name="run-an-unattended-installation-of-azure-backup-server"></a>Katılımsız Azure Backup Sunucusu yüklemesi çalıştırma

Azure Backup Sunucusu katılımsız bir yüklemesini çalıştırmayı öğrenin.

Azure Backup Sunucusu v1 yüklüyorsanız bu adımlar uygulanmaz.

## <a name="install-backup-server"></a>Yedekleme sunucusunu yükler

1. Azure Backup Sunucusu v2 veya üstünü barındıran sunucuda bir metin dosyası oluşturun. (Dosyayı Not defteri 'nde veya başka bir metin düzenleyicisinde oluşturabilirsiniz.) Dosyayı MABSSetup. ini olarak kaydedin.

2. Aşağıdaki kodu MABSSetup. ini dosyasına yapıştırın. Köşeli ayracın içindeki metni (\< \>), ortamınızdaki değerlerle değiştirin. Aşağıdaki metin bir örnektir:

   ```
   [OPTIONS]
   UserName=administrator
   CompanyName=<Microsoft Corporation>
   SQLMachineName=localhost
   SQLInstanceName=<SQL instance name>
   SQLMachineUserName=administrator
   SQLMachinePassword=<admin password>
   SQLMachineDomainName=<machine domain>
   ReportingMachineName=localhost
   ReportingInstanceName=<reporting instance name>
   SqlAccountPassword=<admin password>
   ReportingMachineUserName=<username>
   ReportingMachinePassword=<reporting admin password>
   ReportingMachineDomainName=<domain>
   VaultCredentialFilePath=<vault credential full path and complete name>
   SecurityPassphrase=<passphrase>
   PassphraseSaveLocation=<passphrase save location>
   UseExistingSQL=<1/0 use or do not use existing SQL>
   ```

3. Dosyayı kaydedin. Sonra, yükleme sunucusundaki yükseltilmiş bir komut isteminde şu komutu girin:

   ```
   start /wait <cdlayout path>/Setup.exe /i  /f <.ini file path>/setup.ini /L <log path>/setup.log
   ```

Yükleme için bu bayrakları kullanabilirsiniz:</br>
**/f**:. ini dosya yolu</br>
**/l**: Günlük yolu</br>
**/i**: Yükleme yolu</br>
**/x**: Kaldırma yolu</br>

## <a name="next-steps"></a>Sonraki adımlar
Yedekleme sunucusunu yükledikten sonra, sunucunuzu nasıl hazırlayacağınızı veya bir iş yükünü korumaya nasıl başlayacağınızı öğrenin.

- [Yedekleme sunucusu iş yüklerini hazırlama](backup-azure-microsoft-azure-backup.md)
- [Bir VMware sunucusunu yedeklemek için yedekleme sunucusu kullanma](backup-azure-backup-server-vmware.md)
- [Yedekleme sunucusunu kullanarak yedekleme SQL Server](backup-azure-sql-mabs.md)
- [Yedekleme sunucusuna Modern Yedekleme Alanı Ekle](backup-mabs-add-storage.md)
