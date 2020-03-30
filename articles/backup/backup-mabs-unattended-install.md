---
title: Azure Backup Server V2'nin sessiz kurulumu
description: Azure Backup Server V2'yi sessizce yüklemek için powershell komut dosyası kullanın. Bu tür yükleme, katılımsız yükleme olarak da adlandırılır.
ms.topic: conceptual
ms.date: 11/13/2018
ms.openlocfilehash: 1539089e713bcf8e959707c6ff4a608f062a7c00
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74172238"
---
# <a name="run-an-unattended-installation-of-azure-backup-server"></a>Azure Yedekleme Sunucusu'nun gözetimsiz yüklemesini çalıştırma

Azure Yedekleme Sunucusu'nun gözetimsiz yüklemesini nasıl çalıştırılacınız öğrenin.

Azure Backup Server V1'i yüklüyorsanız, bu adımlar geçerli değildir.

## <a name="install-backup-server"></a>Yedekleme Sunucusu Yükle

1. Azure Backup Server V2 veya daha sonra barındıran sunucuda bir metin dosyası oluşturun. (Dosyayı Not Defteri'nde veya başka bir metin düzenleyicisinde oluşturabilirsiniz.) Dosyayı MABSSetup.ini olarak kaydedin.

2. MABSSetup.ini dosyasına aşağıdaki kodu yapıştırın. Parantez içindeki metni (\< \>) ortamınızdaki değerlerle değiştirin. Aşağıdaki metin bir örnektir:

   ```text
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

3. Dosyayı kaydedin. Ardından, yükleme sunucusundaki yüksek komut isteminde şu komutu girin:

   ```cmd
   start /wait <cdlayout path>/Setup.exe /i  /f <.ini file path>/setup.ini /L <log path>/setup.log
   ```

Yükleme için bu bayrakları kullanabilirsiniz:</br>
**/f**: .ini dosya yolu</br>
**/l**: Giriş yolu</br>
**/i**: Kurulum yolu</br>
**/x**: Yolu kaldır</br>

## <a name="next-steps"></a>Sonraki adımlar

Backup Server'ı yükledikten sonra sunucunuzu nasıl hazırlayacağınızı öğrenin veya iş yükünü korumaya başlayın.

- [Yedekleme Sunucusu iş yüklerini hazırlama](backup-azure-microsoft-azure-backup.md)
- [VMware sunucusuyedeklemek için Yedek Sunucu'yı kullanma](backup-azure-backup-server-vmware.md)
- [SQL Server'ı yedeklemek için Yedekleme Sunucusu'nı kullanın](backup-azure-sql-mabs.md)
- [Yedekleme Sunucusuna Modern Yedekleme Depolaması Ekleme](backup-mabs-add-storage.md)
