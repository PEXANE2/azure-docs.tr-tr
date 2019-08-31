---
title: Data Box Disk sistem gereksinimlerini Microsoft Azure | Microsoft Docs
description: Azure Data Box Disk için yazılım ve ağ gereksinimleri hakkında bilgi edinin
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: article
ms.date: 08/29/2019
ms.author: alkohli
ms.localizationpriority: high
ms.openlocfilehash: 20c27a3e4e9a96a19b347e5ef57ab9fb3c047140
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70194438"
---
::: zone target="docs"

# <a name="azure-data-box-disk-system-requirements"></a>Azure Data Box Disk sistem gereksinimleri

Bu makalede, Microsoft Azure Data Box Disk çözümünüz ve Data Box Disk bağlanan istemciler için önemli sistem gereksinimleri açıklanmaktadır. Data Box Disk dağıtmadan önce bilgileri dikkatlice incelemenizi ve ardından dağıtım ve sonraki işlemler sırasında gerektiği şekilde geri başvurmalarını öneririz.

Sistem gereksinimleri, disklere, desteklenen depolama hesaplarına ve depolama türlerine bağlanan istemciler için desteklenen platformları içerir.

::: zone-end

::: zone target="chromeless"

# <a name="review-prerequisites"></a>Önkoşulları gözden geçirin

1. Öğreticiyi kullanarak Data Box Disk sipariş etmeniz gerekir: Azure Data Box Disk sıralayın. Disklerinizi ve disk başına bir bağlantı kablosunu aldınız.
2. Verileri kopyalayabilmeniz için kullanabileceğiniz bir istemci bilgisayarınız var. İstemci bilgisayarınızda:

    - Desteklenen bir işletim sistemi çalıştırın.
    - Diğer gerekli yazılımların yüklü olmasını gerektirir.

::: zone-end

## <a name="supported-operating-systems-for-clients"></a>İstemciler için desteklenen işletim sistemleri

Data Box Disk bağlı istemciler aracılığıyla disk kilidi açma ve veri kopyalama işlemi için desteklenen işletim sistemlerinin bir listesi aşağıda verilmiştir.

| **İşletim sistemi** | **Sınanan sürümler** |
| --- | --- |
| Windows Server |2008 R2 SP1 <br> 2012 <br> 2012 R2 <br> 2016 |
| Windows (64-bit) |7, 8, 10 |
|Linux <br> <li> Ubuntu </li><li> Debian </li><li> Red Hat Enterprise Linux (RHEL) </li><li> CentOS| <br>14.04, 16.04 18.04 <br> 8,11, 9 <br> 7.0 <br> 6,5, 6,9, 7,0, 7,5 |  

## <a name="other-required-software-for-windows-clients"></a>Windows istemcileri için gereken diğer yazılımlar

Windows istemcisi için, aşağıdakilerin de yüklenmesi gerekir.

| **Yazılım**| **Sürüm** |
| --- | --- |
| Windows PowerShell |5.0 |
| .NET Framework |4.5.1 |
| Windows Management Framework |5.0|
| BitLocker| - |

## <a name="other-required-software-for-linux-clients"></a>Linux istemcileri için gereken diğer yazılımlar

Linux istemcisi için Data Box Disk araç takımı, aşağıdaki gerekli yazılımları yüklerse:

- disdolabı
- OpenSSL

::: zone target="docs"

## <a name="supported-connection"></a>Desteklenen bağlantı

Verileri içeren istemci bilgisayar, USB 3,0 veya üzeri bir bağlantı noktasına sahip olmalıdır. Diskler, bu istemciye, belirtilen kablo aracılığıyla bağlanır.

## <a name="supported-storage-accounts"></a>Desteklenen depolama hesapları

Data Box Disk için desteklenen depolama türlerinin bir listesi aşağıda verilmiştir.

| **Depolama hesabı** | **Notlar** |
| --- | --- |
| Klasik | Standart |
| Genel Amaçlı  |Stand V1 ve v2 desteklenir. Hem sık hem de Cool katmanları desteklenir. |
| BLOB depolama hesabı | |

>[!NOTE]
> Azure Data Lake Storage Gen 2 hesapları desteklenmez.


## <a name="supported-storage-types-for-upload"></a>Karşıya yükleme için desteklenen depolama türleri

Data Box Disk kullanılarak Azure 'a yüklenmek üzere desteklenen depolama türlerinin bir listesi aşağıda verilmiştir.

| **Dosya biçimi** | **Notlar** |
| --- | --- |
| Azure Blok Blobu | |
| Azure Sayfa Blobu  | |
| Azure Dosyaları  | |
| Yönetilen Diskler | |


## <a name="next-step"></a>Sonraki adım

* [Azure Data Box Disk dağıtın](data-box-disk-deploy-ordered.md)

::: zone-end

