---
title: Microsoft Azure Veri Kutusu sistem gereksinimleri| Microsoft Dokümanlar
description: Azure Data Box'ınızın yazılım ve ağ gereksinimleri hakkında bilgi edinin
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: article
ms.date: 07/11/2019
ms.author: alkohli
ms.openlocfilehash: e232ad131b1c0930afcf5e7e78b386aba2c9490b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79259961"
---
# <a name="azure-data-box-system-requirements"></a>Azure Veri Kutusu sistem gereksinimleri

Bu makalede, Microsoft Azure Veri Kutunuz ve Veri Kutusu'na bağlanan istemciler için önemli sistem gereksinimleri açıklanmaktadır. Veri Kutunuzu dağıtmadan önce bilgileri dikkatle gözden geçirmenizi ve dağıtım ve sonraki işlem sırasında gerekli olduğu şekilde geri başvurmanızı öneririz.

Sistem gereksinimleri şunlardır:

* **Veri Kutusu'na bağlanan ana bilgisayarlar için yazılım gereksinimleri** - desteklenen platformları, yerel web arabirimi tarayıcılarını, Kobİ istemcilerini ve Veri Kutusu'na bağlanabilen ana bilgisayarlar için ek gereksinimleri açıklar.
* **Veri Kutusu için ağ gereksinimleri** - Veri Kutusu'nun en iyi çalışması için ağ gereksinimleri hakkında bilgi sağlar.


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

Veri merkezinizin yüksek hızlı ağı olmalıdır. En az bir adet 10 GbE bağlantınızın olması önemle tavsiye edilir. 10 GbE bağlantı yoksa, verileri kopyalamak için 1 GbE veri bağlantısı kullanılabilir, ancak kopyalama hızları etkilenir.

### <a name="port-requirements"></a>Bağlantı noktası gereksinimleri

Aşağıdaki tabloda, SMB veya NFS trafiğine izin vermek için güvenlik duvarınızda açılması gereken bağlantı noktaları listelenildi. Bu tabloda, *gelen* *istemcinin* cihazınıza erişmelerini talep ettiği yönü ifade eder. *Giden* veya *giden,* Veri Kutusu aygıtınızın verileri dağıtımın ötesinde harici olarak gönderdiği yönü ifade eder: örneğin, Internet'e giden.

[!INCLUDE [data-box-port-requirements](../../includes/data-box-port-requirements.md)]


## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Kutunuzu Dağıtma](data-box-deploy-ordered.md)
