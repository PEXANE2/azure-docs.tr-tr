---
title: Azure İçe Alma/Dışa Aktarma hizmeti gereksinimleri | Microsoft Dokümanlar
description: Azure İçe Alma/Dışa Aktarma hizmeti için yazılım ve donanım gereksinimlerini anlayın.
author: alkohli
services: storage
ms.service: storage
ms.topic: article
ms.date: 08/12/2019
ms.author: alkohli
ms.subservice: common
ms.openlocfilehash: 58997b20c01f33037a5e5e149caa59e1630373ff
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79255320"
---
# <a name="azure-importexport-system-requirements"></a>Azure İçeri/Dışarı Aktarma için sistem gereksinimleri

Bu makalede, Azure İçe Alma/Dışa Aktarma hizmetinizin önemli gereksinimleri açıklanmaktadır. İçe Alma/Dışa Aktarma hizmetini kullanmadan önce bilgileri dikkatle gözden geçirmenizi ve ardından işlem sırasında gerekli olduğu şekilde geri başvurmanızı öneririz.

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

WAImportExport aracını kullanarak sabit diskleri hazırlamak için **BitLocker Sürücü Şifrelemesini destekleyen** aşağıdaki 64 bit işletim sistemi desteklenir.


|Platform |Sürüm |
|---------|---------|
|Windows     | Windows 7 Kurumsal, Windows 7 Ultimate <br> Windows 8 Pro, Windows 8 Enterprise, Windows 8.1 Pro, Windows 8.1 Kurumsal <br> Windows 10        |
|Windows Server     |Windows Server 2008 R2 <br> Windows Server 2012, Windows Server 2012 R2         |

## <a name="other-required-software-for-windows-client"></a>Windows istemcisi için diğer gerekli yazılımlar

|Platform |Sürüm |
|---------|---------|
|.NET Framework    | 4.5.1       |
| BitLocker        |  _          |


## <a name="supported-storage-accounts"></a>Desteklenen depolama hesapları

Azure İçe Alma/Dışa Aktarma hizmeti aşağıdaki depolama hesabı türlerini destekler:

- Standart Genel Amaçlı v2 depolama hesapları (çoğu senaryo için önerilir)
- Blob Depolama Hesapları
- Genel Amaçlı v1 depolama hesapları (hem Klasik hem de Azure Kaynak Yöneticisi dağıtımları),

Depolama hesapları hakkında daha fazla bilgi için [Azure depolama hesaplarına genel bakış'a](storage-account-overview.md)bakın.

Her iş, yalnızca bir depolama hesabına veya yalnızca bir depolama hesabından veri aktarmak için kullanılabilir. Başka bir deyişle, tek bir alma/dışa aktarma işi birden çok depolama hesabına yayılamaz. Yeni bir depolama hesabı oluşturma hakkında daha fazla bilgi için [bkz.](storage-account-create.md)

> [!IMPORTANT]
> Azure İçe Aktarma hizmeti, Sanal [Ağ Hizmeti Bitiş Noktaları](../../virtual-network/virtual-network-service-endpoints-overview.md) özelliğinin etkin olduğu depolama hesaplarını desteklemez. 

## <a name="supported-storage-types"></a>Desteklenen depolama türleri

Aşağıdaki depolama türleri listesi Azure İçe Alma/Dışa Aktarma hizmetiyle desteklenir.


|İş  |Depolama Hizmeti |Destekleniyor  |Desteklenmiyor  |
|---------|---------|---------|---------|
|İçeri Aktarma     |  Azure Blob depolama <br><br> Azure Dosya Depolama       | Blobs ve Sayfa blobs desteklenen blok <br><br> Desteklenen dosyalar          |
|Dışarı Aktarma     |   Azure Blob depolama       | Blok blobs, Sayfa lekeleri ve Append blobs desteklenen         | Azure Dosyaları desteklenmiyor


## <a name="supported-hardware"></a>Desteklenen donanım

Azure İçe Alma/Dışa Aktarma hizmeti için verileri kopyalamak için desteklenen disklere ihtiyacınız vardır.

### <a name="supported-disks"></a>Desteklenen diskler

Aşağıdaki disk listesi, İçe/Dışa Aktar hizmetinde kullanılmak üzere desteklenir.


|Disk türü  |Boyut  |Destekleniyor |
|---------|---------|---------|
|SSD    |   2.5"      |SATA III          |
|HDD     |  2.5"<br>3.5"       |SATA II, SATA III         |

Aşağıdaki disk türleri desteklenmez:
- USB'ler.
- Dahili USB adaptörü ile harici HDD.
- Harici bir HDD'nin kasasının içindeki diskler.

Tek bir alma/dışa aktarma işi olabilir:
- En fazla 10 HDD/SSD.
- Herhangi bir boyutta HDD / SSD karışımı.

Çok sayıda sürücü birden çok işe yayılabilir ve oluşturulabilecek iş sayısında sınır yoktur. Alma işleri için yalnızca sürücüdeki ilk veri hacmi işlenir. Veri hacmi NTFS ile biçimlendirilmelidir.

Sabit diskleri hazırlarken ve WAImportExport aracını kullanarak verileri kopyalarken harici USB adaptörlerini kullanabilirsiniz. Çoğu off-the-raf USB 3.0 veya daha sonraki adaptörleri çalışması gerekir.


## <a name="next-steps"></a>Sonraki adımlar

* [WAImportExport aracını ayarlama](storage-import-export-tool-how-to.md)
* [AzCopy komut satırı yardımcı programı ile veri aktarımı](storage-use-azcopy.md)
* [Azure İçe Aktarma REST API örneği](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/)
