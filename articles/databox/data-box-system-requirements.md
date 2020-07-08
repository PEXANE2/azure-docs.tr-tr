---
title: Data Box sistem gereksinimlerini Microsoft Azure | Microsoft Docs
description: Azure Data Box'ınızın yazılım ve ağ gereksinimleri hakkında bilgi edinin
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: article
ms.date: 07/11/2019
ms.author: alkohli
ms.openlocfilehash: e232ad131b1c0930afcf5e7e78b386aba2c9490b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "79259961"
---
# <a name="azure-data-box-system-requirements"></a>Azure Data Box sistem gereksinimleri

Bu makalede, Microsoft Azure Data Box ve Data Box bağlanan istemciler için önemli sistem gereksinimleri açıklanmaktadır. Data Box dağıtmadan önce bilgileri dikkatlice incelemenizi ve ardından dağıtım ve sonraki işlemler sırasında gerektiği şekilde geri başvurmalarını öneririz.

Sistem gereksinimleri şunları içerir:

* **Data Box bağlanan konaklar Için yazılım gereksinimleri** -desteklenen platformları, yerel Web Kullanıcı arabirimine yönelik TARAYıCıLARı, SMB istemcilerini ve Data Box bağlanabilecek konaklara yönelik ek gereksinimleri açıklar.
* **Data Box Için ağ gereksinimleri** -Data Box en iyi işlem için ağ gereksinimleri hakkında bilgi sağlar.


## <a name="software-requirements"></a>Yazılım gereksinimleri

Yazılım gereksinimleri, desteklenen işletim sistemleri, yerel Web Kullanıcı arabirimi için desteklenen tarayıcılar ve SMB istemcileri hakkındaki bilgileri içerir.

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

Veri merkezinizin yüksek hızlı ağı olmalıdır. En az bir adet 10 GbE bağlantınızın olması önemle tavsiye edilir. 10-GbE bağlantı kullanılamıyorsa, verileri kopyalamak için 1-GbE veri bağlantısı kullanılabilir ancak kopyalama hızları bundan etkilenir.

### <a name="port-requirements"></a>Bağlantı noktası gereksinimleri

Aşağıdaki tabloda SMB veya NFS trafiğine izin vermek için güvenlik duvarınızda açılması gereken bağlantı noktaları listelenmektedir. Bu tabloda, veya *gelen* *içinde* , gelen istemci, cihazınıza erişim talep ettiği yöne başvurur. *Out* veya *Outbound* , dağıtım ötesinde Data Box cihazınızın verileri dışarıdan gönderdiği yönü ifade eder. Örneğin, Internet 'e giden.

[!INCLUDE [data-box-port-requirements](../../includes/data-box-port-requirements.md)]


## <a name="next-steps"></a>Sonraki adımlar

* [Azure Data Box dağıtın](data-box-deploy-ordered.md)
