---
title: Data Box sistem gereksinimlerini Microsoft Azure | Microsoft Docs
description: Azure Data Box ve Data Box bağlanan istemciler için önemli sistem gereksinimleri hakkında bilgi edinin.
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: article
ms.date: 10/02/2020
ms.author: alkohli
ms.openlocfilehash: 5f1623ef4dde59e816e3afe5a5f5894c49469580
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91767883"
---
# <a name="azure-data-box-system-requirements"></a>Azure Data Box sistem gereksinimleri

Bu makalede, Microsoft Azure Data Box ve Data Box bağlanan istemciler için önemli sistem gereksinimleri açıklanmaktadır. Data Box dağıtmadan önce bilgileri dikkatle incelemenizi ve dağıtım ve işlem sırasında gerektiği şekilde bu bilgilere başvurmalarını öneririz.

Sistem gereksinimleri şunları içerir:

* **Yazılım gereksinimleri:** Data Box bağlanan konaklarda, yerel Web Kullanıcı arabirimi için desteklenen işletim sistemlerini, dosya aktarım protokollerini, depolama hesaplarını, depolama türlerini ve tarayıcıları açıklar.
* **Ağ gereksinimleri:** Data Box için, Data Box en iyi işlem için ağ bağlantısı ve bağlantı noktası gereksinimleri açıklanmaktadır.


## <a name="software-requirements"></a>Yazılım gereksinimleri

Yazılım gereksinimleri, yerel Web Kullanıcı arabirimi için desteklenen işletim sistemlerini, dosya aktarım protokollerini, depolama hesaplarını, depolama türlerini ve tarayıcıları içerir.

### <a name="supported-operating-systems-for-clients"></a>İstemciler için desteklenen işletim sistemleri

[!INCLUDE [data-box-supported-os-clients](../../includes/data-box-supported-os-clients.md)]


### <a name="supported-file-transfer-protocols-for-clients"></a>İstemciler için desteklenen dosya aktarımı protokolleri

[!INCLUDE [data-box-supported-file-systems-clients](../../includes/data-box-supported-file-systems-clients.md)]

> [!IMPORTANT] 
> Data Box paylaşımlarına bağlantı, dışarı aktarma siparişleri için REST aracılığıyla desteklenmez. 

### <a name="supported-storage-accounts"></a>Desteklenen depolama hesapları

[!INCLUDE [data-box-supported-storage-accounts](../../includes/data-box-supported-storage-accounts.md)]

### <a name="supported-storage-types"></a>Desteklenen depolama türleri

[!INCLUDE [data-box-supported-storage-types](../../includes/data-box-supported-storage-types.md)]

### <a name="supported-web-browsers"></a>Desteklenen web tarayıcıları

[!INCLUDE [data-box-supported-web-browsers](../../includes/data-box-supported-web-browsers.md)]

## <a name="networking-requirements"></a>Ağ gereksinimleri

Veri merkezinizin yüksek hızlı ağı olmalıdır. En az bir adet 10 GbE bağlantınızın olması önemle tavsiye edilir. 10-GbE bağlantı kullanılamıyorsa, verileri kopyalamak için 1-GbE veri bağlantısı kullanılabilir ancak kopyalama hızları bundan etkilenir.

### <a name="port-requirements"></a>Bağlantı noktası gereksinimleri

Aşağıdaki tabloda SMB veya NFS trafiğine izin vermek için güvenlik duvarınızda açılması gereken bağlantı noktaları listelenmektedir. Bu tabloda, (*gelen* *), gelen* istemcinin cihazınıza erişim talep ettiği yöne başvurur. *Out* (veya *Outbound*), dağıtım ötesinde Data Box cihazınızın verileri dışarıdan gönderdiği yönü ifade eder. Örneğin, Internet 'e giden.

[!INCLUDE [data-box-port-requirements](../../includes/data-box-port-requirements.md)]


## <a name="next-steps"></a>Sonraki adımlar

* [Azure Data Box dağıtın](data-box-deploy-ordered.md)
