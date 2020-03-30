---
title: Microsoft Azure Veri Kutusu Ağır sistem gereksinimleri| Microsoft Dokümanlar
description: Azure Veri Kutusu Heavy'niz için yazılım ve ağ gereksinimleri hakkında bilgi edinin
services: databox
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: article
ms.date: 07/03/2019
ms.author: alkohli
ms.openlocfilehash: 6097dba9193fce4c8158137b488080107881994a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79260078"
---
# <a name="azure-data-box-heavy-system-requirements"></a>Azure Veri Kutusu Ağır sistem gereksinimleri

Bu makalede, Azure Veri Kutusu Ağır aygıtınız ve aygıta bağlanan istemciler için önemli sistem gereksinimleri açıklanmaktadır. Veri Kutusu Ağır'ınızı dağıtmadan önce bilgileri dikkatle gözden geçirmenizi ve dağıtım ve sonraki işlem sırasında gerekli olduğu şekilde geri başvurmanızı öneririz.

Sistem gereksinimleri şunlardır:

* **Data Box Heavy'ye bağlanan ana bilgisayarlar için yazılım gereksinimleri** , desteklenen platformları, yerel web arabirimi tarayıcılarını, Kobİ istemcilerini ve Veri Kutusu'na bağlanabilen ana bilgisayarlar için ek gereksinimleri açıklar.
* **Data Box Heavy için ağ gereksinimleri** - Data Box Heavy cihazının optimum çalışması için ağ gereksinimleri hakkında bilgi sağlar.

## <a name="software-requirements"></a>Yazılım gereksinimleri

Yazılım gereksinimleri desteklenen işletim sistemleri, yerel web kullanıcı arabirimi için desteklenen tarayıcılar ve Kobİ istemcileri hakkındaki bilgileri içerir.

### <a name="supported-operating-systems-for-clients"></a>İstemciler için desteklenen işletim sistemleri

[!INCLUDE [data-box-supported-os-clients](../../includes/data-box-supported-os-clients.md)]

### <a name="supported-file-systems-for-linux-clients"></a>Linux istemcileri için desteklenen dosya sistemleri

[!INCLUDE [data-box-supported-file-systems-clients](../../includes/data-box-supported-file-systems-clients.md)]

### <a name="supported-storage-accounts"></a>Desteklenen depolama hesapları

[!INCLUDE [data-box-supported-storage-accounts](../../includes/data-box-supported-storage-accounts.md)]

### <a name="supported-storage-types"></a>Desteklenen depolama türleri

[!INCLUDE [data-box-supported-storage-types](../../includes/data-box-supported-storage-types.md)]

### <a name="supported-web-browsers"></a>Desteklenen web tarayıcıları

[!INCLUDE [data-box-supported-web-browsers](../../includes/data-box-supported-web-browsers.md)]

## <a name="networking-requirements"></a>Ağ gereksinimleri

Veri merkezinizin yüksek hızlı ağı olmalıdır. En hızlı kopyalama hızları için, iki adet 40 GbE bağlantı paralel olarak kullanılabilir (düğüm başına bir bağlantı). 40 GbE'niz yoksa, en az iki adet 10 GbE bağlantınız (düğüm başına bir) sahip olduğunuzu öneririz.

### <a name="port-requirements"></a>Bağlantı noktası gereksinimleri

Aşağıdaki tabloda, SMB veya NFS trafiğine izin vermek için güvenlik duvarınızda açılması gereken bağlantı noktaları listelenildi. Bu tabloda, *gelen* *istemcinin* cihazınıza erişmelerini talep ettiği yönü ifade eder. *Giden* veya *giden,* Veri Kutusu Ağır aygıtınızın verileri dağıtımın ötesinde harici olarak gönderdiği yönü ifade eder: örneğin, Internet'e giden.

[!INCLUDE [data-box-port-requirements](../../includes/data-box-port-requirements.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Kutunuzu Dağıtma](data-box-deploy-ordered.md)
