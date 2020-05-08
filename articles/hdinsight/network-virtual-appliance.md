---
title: Azure HDInsight 'ta ağ sanal gereç yapılandırma
description: Azure HDInsight 'ta ağ sanal gereç için birkaç ek özellik yapılandırmayı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/06/2020
ms.openlocfilehash: cbc2104ae3c55ae3670867b7a253d812f3a4be0e
ms.sourcegitcommit: 602e6db62069d568a91981a1117244ffd757f1c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82864714"
---
# <a name="configure-network-virtual-appliance-in-azure-hdinsight"></a>Azure HDInsight 'ta ağ sanal gereç yapılandırma

> [!Important]
> Aşağıdaki bilgiler **yalnızca** Azure Güvenlik Duvarı dışında bir ağ sanal gereci (NVA) yapılandırmak istiyorsanız gereklidir.

Azure Güvenlik Duvarı, yaygın olarak karşılaşılan birçok önemli senaryo için trafiğe izin verecek şekilde otomatik olarak yapılandırılır. Başka bir ağ sanal gerecinin kullanılması, birkaç ek özellik yapılandırmanızı gerektirir. Ağ sanal gerecinizi yapılandırırken aşağıdaki faktörleri göz önünde bulundurun:

* Hizmet uç noktası özellikli Hizmetleri, hizmet uç noktaları ile yapılandırılmalıdır.
* IP adresi bağımlılıkları HTTP/sn olmayan trafiğe yöneliktir (TCP ve UDP trafiği).
* FQDN HTTP/HTTPS uç noktaları, NVA cihazınıza yerleştirilebilir.
* Joker karakter HTTP/HTTPS uç noktaları, bir dizi niteleyicilere göre değişebilen bağımlılıklardır.
* Oluşturduğunuz yol tablosunu HDInsight alt ağına atayın.

## <a name="service-endpoint-capable-dependencies"></a>Hizmet uç noktası özellikli bağımlılıklar

| **Uç Nokta** |
|---|
| Azure SQL |
| Azure Storage |
| Azure Active Directory |

### <a name="ip-address-dependencies"></a>IP adresi bağımlılıkları

| **Uç Nokta** | **Bilgileri** |
|---|---|
| \*: 123 | NTP saat denetimi. Trafik, 123 numaralı bağlantı noktasında birden çok uç noktaya denetlenir |
| [Burada](hdinsight-management-ip-addresses.md) Yayınlanan IP 'ler | Bu IP 'Ler HDInsight hizmetidir |
| ESP kümeleri için AAD-DS özel IP 'Leri |
| \*: KMS Windows etkinleştirmesi 16800 |
| \*Log Analytics için 12000 |

### <a name="fqdn-httphttps-dependencies"></a>FQDN HTTP/HTTPS bağımlılıkları

> [!Important]
> Aşağıdaki liste, en önemli FQDN 'lerin çoğunu sağlar. [Bu dosyada](https://github.com/Azure-Samples/hdinsight-fqdn-lists/blob/master/HDInsightFQDNTags.json)NVA 'nizi yapılandırmak Için ek FQDN 'ler (çoğunlukla Azure depolama ve Azure Service Bus) alabilirsiniz.

| **Uç Nokta**                                                          |
|---|
| azure.archive.ubuntu.com:80                                           |
| security.ubuntu.com:80                                                |
| ocsp.msocsp.com:80                                                    |
| ocsp.digicert.com:80                                                  |
| wawsinfraprodbay063.blob.core.windows.net:443                         |
| registry-1.docker.io:443                                              |
| auth.docker.io:443                                                    |
| production.cloudflare.docker.com:443                                  |
| download.docker.com:443                                               |
| us.archive.ubuntu.com:80                                              |
| download.mono-project.com:80                                          |
| packages.treasuredata.com:80                                          |
| security.ubuntu.com:80                                                |
| azure.archive.ubuntu.com:80                                           |
| ocsp.msocsp.com:80                                                    |
| ocsp.digicert.com:80                                                  |

## <a name="next-steps"></a>Sonraki adımlar

* [Giden trafiği kısıtlamak için güvenlik duvarı kullanma](./hdinsight-restrict-outbound-traffic.md)
* [Azure HDInsight sanal ağ mimarisi](hdinsight-virtual-network-architecture.md)
