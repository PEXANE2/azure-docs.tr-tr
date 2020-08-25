---
title: Azure HDInsight 'ta ağ sanal gereç yapılandırma
description: Azure HDInsight 'ta ağ sanal gereç için birkaç ek özellik yapılandırmayı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.date: 06/30/2020
ms.openlocfilehash: b9e33ba8d9610067cb9e844477ec273391fbdb0b
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2020
ms.locfileid: "88751730"
---
# <a name="configure-network-virtual-appliance-in-azure-hdinsight"></a>Azure HDInsight 'ta ağ sanal gereç yapılandırma

> [!Important]
> Aşağıdaki bilgiler **yalnızca** Azure Güvenlik Duvarı dışında bir ağ sanal gereci (NVA) yapılandırmak istiyorsanız gereklidir.

Azure Güvenlik Duvarı, yaygın olarak karşılaşılan birçok önemli senaryo için trafiğe izin verecek şekilde otomatik olarak yapılandırılır. Başka bir ağ sanal gerecinin kullanılması, birkaç ek özellik yapılandırmanızı gerektirir. Ağ sanal gerecinizi yapılandırırken aşağıdaki faktörleri göz önünde bulundurun:

* Hizmet uç noktası özellikli Hizmetleri, genellikle maliyet veya performans konuları için NVA 'nın atlanmasına neden olan hizmet uç noktaları ile yapılandırılabilir.
* IP adresi bağımlılıkları HTTP/sn olmayan trafiğe yöneliktir (TCP ve UDP trafiği).
* FQDN HTTP/HTTPS uç noktaları, NVA cihazınızda beyaz listeye eklenebilir.
* Oluşturduğunuz yol tablosunu HDInsight alt ağına atayın.

## <a name="service-endpoint-capable-dependencies"></a>Hizmet uç noktası özellikli bağımlılıklar

İsteğe bağlı olarak, aşağıdaki hizmet uç noktalarından birini veya daha fazlasını etkinleştirerek, NVA 'yi atlayarak elde edebilirsiniz. Bu seçenek, maliyetten tasarruf etmek için büyük miktarlarda veri aktarımı ve ayrıca performans iyileştirmeleri için yararlı olabilir. 

| **Uç Noktası** |
|---|
| Azure SQL |
| Azure Storage |
| Azure Active Directory |

### <a name="ip-address-dependencies"></a>IP adresi bağımlılıkları

| **Uç Noktası** | **Ayrıntılar** |
|---|---|
| [Burada](hdinsight-management-ip-addresses.md) Yayınlanan IP 'ler | Bu IP 'Ler HDInsight denetim yerini alır ve asimetrik yönlendirmeyi önlemek için UDR 'ye eklenmelidir |
| AAD-DS özel IP 'Leri | Yalnızca ESP kümeleri için gereklidir|


### <a name="fqdn-httphttps-dependencies"></a>FQDN HTTP/HTTPS bağımlılıkları

> [!Important]
> Aşağıdaki liste yalnızca, işletim sistemi ve güvenlik düzeltme ekleri veya küme oluşturulduktan sonra ve küme işlemlerinin ömrü boyunca sertifika doğrulamaları için gerekebilecek birkaç FQDN sağlar. [Bu dosyada](https://github.com/Azure-Samples/hdinsight-fqdn-lists/blob/master/HDInsightFQDNTags.json)NVA 'nizi yapılandırmak Için, FQDN bağımlılıklarının (çoğunlukla Azure depolama ve Azure Service Bus) listesini alabilirsiniz. Bu bağımlılıklar, HDInsight kaynak sağlayıcısı (RP) tarafından kümeleri oluşturmak ve izlemek/yönetmek için kullanılır. Bunlar telemetri/tanılama günlüklerini, sağlama meta verilerini, kümeyle ilgili konfigürasyonları, betikleri, ARM şablonlarını vb. içerir. FQDN bağımlılığı listesi, gelecekteki HDIngisht güncelleştirmeleri serbest bırakılması ile değiştirilebilir.

| **Uç Noktası**                                                          |
|---|
| azure.archive.ubuntu.com:80                                           |
| security.ubuntu.com:80                                                |
| ocsp.msocsp.com:80                                                    |
| ocsp.digicert.com:80                                                  |

## <a name="next-steps"></a>Sonraki adımlar

* [Giden trafiği kısıtlamak için güvenlik duvarı kullanma](./hdinsight-restrict-outbound-traffic.md)
* [Azure HDInsight sanal ağ mimarisi](hdinsight-virtual-network-architecture.md)
