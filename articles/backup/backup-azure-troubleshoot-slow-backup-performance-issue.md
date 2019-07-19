---
title: Azure Backup’ta dosya ve klasörlerin yavaş yedekleme sorunlarını giderme
description: Azure Backup performans sorunlarının nedenini tanılamanıza yardımcı olması için sorun giderme kılavuzu sağlar
services: backup
author: saurabhsensharma
manager: saurabhsensharma
ms.service: backup
ms.topic: troubleshooting
ms.date: 07/05/2019
ms.author: saurse
ms.openlocfilehash: 592a46077bb9e3469f3a42a95173af1b6db93510
ms.sourcegitcommit: f5075cffb60128360a9e2e0a538a29652b409af9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "67704929"
---
# <a name="troubleshoot-slow-backup-of-files-and-folders-in-azure-backup"></a>Azure Backup’ta dosya ve klasörlerin yavaş yedekleme sorunlarını giderme
Bu makalede, Azure Backup kullanırken dosyalar ve klasörler için yavaş yedekleme performansının Nedenini tanılamanıza yardımcı olacak sorun giderme kılavuzu sağlanmıştır. Dosyaları yedeklemek için Azure Backup Aracısı 'nı kullandığınızda, yedekleme işlemi beklenenden uzun sürebilir. Bu gecikmeye aşağıdakilerden biri veya birkaçı neden olmuş olabilir:

* [Yedeklenmekte olan bilgisayarda performans sorunları var.](#cause1)
* [Başka bir işlem veya virüsten koruma yazılımı Azure Backup işlemini engelliyor.](#cause2)
* [Yedekleme aracısı bir Azure sanal makinesinde (VM) çalışıyor.](#cause3)  
* [Çok sayıda (milyonlarca) dosyayı yedekliyorsunuz.](#cause4)

Sorun giderme sorunlarını gidermeye başlamadan önce [en son Azure Backup aracısını](https://aka.ms/azurebackup_agent)indirip yüklemenizi öneririz. Çeşitli sorunları gidermek, özellikler eklemek ve performansı artırmak için yedekleme aracısında sık sık güncelleştirmeler yaptık.

Ayrıca, yaygın yapılandırma sorunlarından biriyle karşılaşmadığınızdan emin olmak için [Azure Backup hizmeti hakkında SSS](backup-azure-backup-faq.md) 'yi incelemenizi öneririz.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

<a id="cause1"></a>

## <a name="cause-performance-bottlenecks-on-the-computer"></a>Neden: Bilgisayardaki performans sorunları
Yedeklenen bilgisayardaki performans sorunları gecikmelere neden olabilir. Örneğin, bilgisayarın disk okuma veya diske yazma veya ağ üzerinden veri göndermek için kullanılabilir bant genişliği gibi performans sorunlarına neden olabilir.

Windows, bu darboğazları algılamak için [Performans İzleyicisi](https://technet.microsoft.com/magazine/2008.08.pulse.aspx) (PerfMon) adlı yerleşik bir araç sağlar.

En iyi yedeklemeler için performans sorunlarını tanılamaya yardımcı olabilecek bazı performans sayaçları ve aralıkları aşağıda verilmiştir.

| Sayaç | Durum |
| --- | --- |
| Mantıksal disk (fiziksel disk)--% boşta |•% 100 boşta =% 50 boşta = sağlıklı</br>•% 49 boşta =% 20 boşta = uyarı ya da Izleyici</br>•% 19 boşta,% 0 boşta = kritik veya spec dışı |
| Mantıksal disk (fiziksel disk)--% ort. Disk sn okuma veya yazma |• 0,001 MS-0,015 MS = sağlıklı</br>• 0,015 MS-0,025 MS = uyarı veya Izleyici</br>• 0,026 MS veya daha uzun = kritik veya spec dışı |
| Mantıksal disk (fiziksel disk)--geçerli disk sırası uzunluğu (tüm örnekler için) |6 dakikadan uzun süre 80 istek |
| Bellek--Sayfalamayan baytlar |• Kullanılan havuzun% 60 ' inden az olması = sağlıklı<br>•% 61, kullanılan havuzun% 80 ' i = uyarı veya Izleyici</br>•% 80 daha fazla kullanılan havuz = kritik veya spec dışı |
| Bellek--havuzda disk belleğine alınan baytlar |• Kullanılan havuzun% 60 ' inden az olması = sağlıklı</br>•% 61, kullanılan havuzun% 80 ' i = uyarı veya Izleyici</br>•% 80 daha fazla kullanılan havuz = kritik veya spec dışı |
| Bellek--kullanılabilir megabayt |•% 50 boş bellek kullanılabilir veya daha fazla = sağlıklı</br>• boş belleğin% 25 ' i kullanılabilir = Izleyici</br>• kullanılabilir boş belleğin% 10 ' ü = uyarı</br>• 100 MB 'tan az veya kullanılabilir boş belleğin% 5 ' inden az olması = kritik veya spec dışı |
| Processor--\%Processor Time (all instances) |• Less than 60% consumed = Healthy</br>• 61% to 90% consumed = Monitor or Caution</br>• 91% to 100% consumed = Critical |

> [!NOTE]
> If you determine that the infrastructure is the culprit, we recommend that you defragment the disks regularly for better performance.
>
>

<a id="cause2"></a>

## Cause: Another process or antivirus software interfering with Azure Backup
We've seen several instances where other processes in the Windows system have negatively affected performance of the Azure Backup agent process. For example, if you use both the Azure Backup agent and another program to back up data, or if antivirus software is running and has a lock on files to be backed up, the multiple locks on files might cause contention. In this situation, the backup might fail, or the job might take longer than expected.

The best recommendation in this scenario is to turn off the other backup program to see whether the backup time for the Azure Backup agent changes. Usually, making sure that multiple backup jobs are not running at the same time is sufficient to prevent them from affecting each other.

For antivirus programs, we recommend that you exclude the following files and locations:

* C:\Program Files\Microsoft Azure Recovery Services Agent\bin\cbengine.exe as a process
* C:\Program Files\Microsoft Azure Recovery Services Agent\ folders
* Scratch location (if you're not using the standard location)

<a id="cause3"></a>

## Cause: Backup agent running on an Azure virtual machine
If you're running the Backup agent on a VM, performance will be slower than when you run it on a physical machine. This is expected due to IOPS limitations.  However, you can optimize the performance by switching the data drives that are being backed up to Azure Premium Storage. We're working on fixing this issue, and the fix will be available in a future release.

<a id="cause4"></a>

## Cause: Backing up a large number (millions) of files
Moving a large volume of data will take longer than moving a smaller volume of data. In some cases, backup time is related to not only the size of the data, but also the number of files or folders. This is especially true when millions of small files (a few bytes to a few kilobytes) are being backed up.

This behavior occurs because while you're backing up the data and moving it to Azure, Azure is simultaneously cataloging your files. In some rare scenarios, the catalog operation might take longer than expected.

The following indicators can help you understand the bottleneck and accordingly work on the next steps:

* **UI is showing progress for the data transfer**. The data is still being transferred. The network bandwidth or the size of data might be causing delays.
* **UI is not showing progress for the data transfer**. Open the logs located at C:\Program Files\Microsoft Azure Recovery Services Agent\Temp, and then check for the FileProvider::EndData entry in the logs. This entry signifies that the data transfer finished and the catalog operation is happening. Don't cancel the backup jobs. Instead, wait a little longer for the catalog operation to finish. If the problem persists, contact [Azure support](https://portal.azure.com/#create/Microsoft.Support).İşlemci--\`işlemci zamanı (tüm örnekler)es and folders in Azure Backup
description: Provides troubleshooting guidance to help you diagnose the cause of Azure Backup performance issues
services: backup
author: saurabhsensharma
manager: saurabhsensharma
ms.service: backup
ms.topic: troubleshooting
ms.date: 07/05/2019
ms.author: saurse
---
# Troubleshoot slow backup of files and folders in Azure Backup
This article provides troubleshooting guidance to help you diagnose the cause of slow backup performance for files and folders when you're using Azure Backup. When you use the Azure Backup agent to back up files, the backup process might take longer than expected. This delay might be caused by one or more of the following:

* [There are performance bottlenecks on the computer that’s being backed up.](#cause1)
* [Another process or antivirus software is interfering with the Azure Backup process.](#cause2)
* [The Backup agent is running on an Azure virtual machine (VM).](#cause3)  
* [You're backing up a large number (millions) of files.](#cause4)

Before you start troubleshooting issues, we recommend that you download and install the [latest Azure Backup agent](https://aka.ms/azurebackup_agent). We make frequent
updates to the Backup agent to fix various issues, add features, and improve performance.

We also strongly recommend that you review the [Azure Backup service FAQ](backup-azure-backup-faq.md) to make sure you're not experiencing any of the common configuration issues.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

<a id="cause1"></a>

## Cause: Performance bottlenecks on the computer
Bottlenecks on the computer that's being backed up can cause delays. For example, the computer's ability to read or write to disk, or available bandwidth to send data over the network, can cause bottlenecks.

Windows provides a built-in tool that's called [Performance Monitor](https://technet.microsoft.com/magazine/2008.08.pulse.aspx) (Perfmon) to detect these bottlenecks.

Here are some performance counters and ranges that can be helpful in diagnosing bottlenecks for optimal backups.

| Counter | Status |
| --- | --- |
| Logical Disk(Physical Disk)--%idle |• 100% idle to 50% idle = Healthy</br>• 49% idle to 20% idle = Warning or Monitor</br>• 19% idle to 0% idle = Critical or Out of Spec |
| Logical Disk(Physical Disk)--%Avg. Disk Sec Read or Write |• 0.001 ms to 0.015 ms  = Healthy</br>• 0.015 ms to 0.025 ms = Warning or Monitor</br>• 0.026 ms or longer = Critical or Out of Spec |
| Logical Disk(Physical Disk)--Current Disk Queue Length (for all instances) |80 requests for more than 6 minutes |
| Memory--Pool Non Paged Bytes |• Less than 60% of pool consumed = Healthy<br>• 61% to 80% of pool consumed = Warning or Monitor</br>• Greater than 80% pool consumed = Critical or Out of Spec |
| Memory--Pool Paged Bytes |• Less than 60% of pool consumed = Healthy</br>• 61% to 80% of pool consumed = Warning or Monitor</br>• Greater than 80% pool consumed = Critical or Out of Spec |
| Memory--Available Megabytes |• 50% of free memory available or more = Healthy</br>• 25% of free memory available = Monitor</br>• 10% of free memory available = Warning</br>• Less than 100 MB or 5% of free memory available = Critical or Out of Spec |
| Processor--\%Processor Time (all instances) |•% 60 daha az tüketilen = sağlıklı</br>•% 61-90% tüketilen = Izleyici veya uyarı</br>•% 91-100% tüketilen = kritik |

> [!NOTE]
> Altyapının külün olduğunu belirlerseniz, daha iyi performans için diskleri düzenli olarak birleştirmeniz önerilir.
>
>

<a id="cause2"></a>

## <a name="cause-another-process-or-antivirus-software-interfering-with-azure-backup"></a>Neden: Azure Backup kesintiye uğramadan başka bir işlem veya virüsten koruma yazılımı
Windows sistemindeki diğer işlemlerin Azure Backup Aracısı işleminin performansını olumsuz yönde etkilediği birkaç örnek gördük. Örneğin, verileri yedeklemek için hem Azure Backup Aracısı hem de başka bir program kullanıyorsanız veya virüsten koruma yazılımı çalışıyorsa ve yedeklenmek üzere dosyalar üzerinde bir kilit varsa, dosyalardaki birden fazla kilit çekişmeye neden olabilir. Bu durumda, yedekleme başarısız olabilir veya iş beklenenden uzun sürebilir.

Bu senaryodaki en iyi öneri, Azure Backup Aracısı için yedekleme saatinin değişmediğini görmek üzere diğer Yedekleme programını kapatmmsudur. Genellikle, birden çok yedekleme işinin aynı anda çalışmadığı ve birbirini etkilemelerini engellemek için yeterli olduğundan emin olun.

Virüsten koruma programları için aşağıdaki dosyaları ve konumları dışlanmasını öneririz:

* C:\Program Files\Microsoft Azure kurtarma hizmetleri bir işlem olarak Agent\bin\cbengine.exe
* C:\Program Files\Microsoft Azure kurtarma hizmetleri Aracısı \ klasörler
* Karalama konumu (Standart konumu kullanmıyorsanız)

<a id="cause3"></a>

## <a name="cause-backup-agent-running-on-an-azure-virtual-machine"></a>Neden: Azure sanal makinesinde çalışan yedekleme Aracısı
Yedekleme aracısını bir VM üzerinde çalıştırıyorsanız, performans fiziksel bir makinede çalıştırılırken daha yavaş olur. Bu, ıOPS sınırlamaları nedeniyle beklenmektedir.  Ancak, Azure Premium depolamaya yedeklenmekte olan veri sürücülerine geçerek performansı iyileştirebilirsiniz. Bu sorunu düzeltmek için çalışıyoruz ve düzeltme gelecekteki bir sürümde kullanıma sunulacaktır.

<a id="cause4"></a>

## <a name="cause-backing-up-a-large-number-millions-of-files"></a>Neden: Büyük sayıda (milyonlarca) dosyayı yedekleme
Büyük hacimli verilerin taşınması, daha küçük bir veri hacmi taşımadan daha uzun sürer. Bazı durumlarda, yedekleme zamanı yalnızca verilerin boyutuyla ve ayrıca dosya veya klasör sayısıyla ilgilidir. Bu özellikle, milyonlarca küçük dosya (birkaç kilobayt kadar bayt) yedeklendiğinde geçerlidir.

Bu davranış, verileri yedeklemekte ve Azure 'a taşırken, Azure 'un aynı anda dosyalarınızı kataloglarken meydana gelir. Nadir bazı senaryolarda, Katalog işlemi beklenenden uzun sürebilir.

Aşağıdaki göstergeler, bir sonraki adımlarda performans sorunlarını ve buna uygun işi anlamanıza yardımcı olabilir:

* **Kullanıcı arabirimi, veri aktarımının ilerlemesini gösteriyor**. Veriler hala aktarılmakta. Ağ bant genişliği veya veri boyutu gecikmelere neden olabilir.
* **UI veri aktarımı için ilerlemeyi gösterilmiyor**. C:\Program Files\Microsoft Azure Recovery Services \Temp dizininde bulunan günlükleri açın ve sonra günlüklerde FileProvider:: EndData girdisini kontrol edin. Bu giriş, veri aktarımının bittiğini ve Katalog işleminin gerçekleştiğini belirtir. Yedekleme işlerini iptal etmeyin. Bunun yerine, Katalog işleminin bitmesini biraz daha uzun süre bekleyin. Sorun devam ederse [Azure desteği](https://portal.azure.com/#create/Microsoft.Support)'ne başvurun.
