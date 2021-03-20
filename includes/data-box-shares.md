---
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: include
ms.date: 06/05/2020
ms.author: alkohli
ms.openlocfilehash: 5aaf0ce747b14b2fa9f2fcd9a65b774aa7d2db3b
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "87103077"
---
Seçilen depolama hesabına bağlı olarak, Data Box şunların tamamını veya bir bölümünü oluşturabilir:

* İlişkili her depolama hesabına GPv1 ve GPv2 için üç paylaşım.
* Premium depolama için bir paylaşım.
* Blob depolama hesabı için bir paylaşım.

Blok blobu ve sayfa blobu paylaşımlarının altında birinci düzeydeki varlıklar kapsayıcılar, ikinci düzeydeki varlıklar ise bloblardır. Azure Dosyaları paylaşımlarında birinci düzeydeki varlıklar paylaşımlar, ikinci düzeydeki varlıklar ise dosyalardır.

Aşağıdaki tabloda, Data Box üzerindeki paylaşımların UNC yolu ve verilerin karşıya yüklendiği Azure Depolama yolu URL'si gösterilmektedir. Son Azure depolama yolu URL'si, UNC paylaşım yolundan türetilebilir.
 
| Bloblar ve Dosyalar | Yollar ve URL'ler |
| --------------- | -------------- |
| Azure Blok blobları | <li>Paylaşımların UNC yolu: `\\<DeviceIPAddress>\<StorageAccountName_BlockBlob>\<ContainerName>\files\a.txt`</li><li>Azure Depolama URL’si: `https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/files/a.txt`</li> |  
| Azure Sayfa blobları  | <li>Paylaşımların UNC yolu: `\\<DeviceIPAddres>\<StorageAccountName_PageBlob>\<ContainerName>\files\a.txt`</li><li>Azure Depolama URL’si: `https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/files/a.txt`</li>   |  
| Azure Dosyaları       |<li>Paylaşımların UNC yolu: `\\<DeviceIPAddres>\<StorageAccountName_AzFile>\<ShareName>\files\a.txt`</li><li>Azure Depolama URL’si: `https://<StorageAccountName>.file.core.windows.net/<ShareName>/files/a.txt`</li>        |      

