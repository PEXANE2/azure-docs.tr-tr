---
title: Azure HDInsight 'ta ağ sanal gereç yapılandırma
description: Azure HDInsight 'ta ağ sanal gereç için birkaç ek özellik yapılandırmayı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 06/30/2020
ms.openlocfilehash: abedaf6f66bdd7aea36512c90bb5ee799f8e7a99
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85800953"
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

| **Uç Nokta** |
|---|
| Azure SQL |
| Azure Storage |
| Azure Active Directory |

### <a name="ip-address-dependencies"></a>IP adresi bağımlılıkları

| **Uç Nokta** | **Ayrıntılar** |
|---|---|
| [Burada](hdinsight-management-ip-addresses.md) Yayınlanan IP 'ler | Bu IP 'Ler HDInsight denetim yerini alır ve asimetrik yönlendirmeyi önlemek için UDR 'ye eklenmelidir |
| AAD-DS özel IP 'Leri | Yalnızca ESP kümeleri için gereklidir|


### <a name="fqdn-httphttps-dependencies"></a>FQDN HTTP/HTTPS bağımlılıkları

> [!Important]
> Aşağıdaki liste, en önemli FQDN 'lerin çoğunu sağlar. [Bu dosyada](https://github.com/Azure-Samples/hdinsight-fqdn-lists/blob/master/HDInsightFQDNTags.json)NVA 'nizi yapılandırmak için FQDN 'lerin (çoğunlukla Azure depolama ve Azure Service Bus) tüm listesini alabilirsiniz. Bu bağımlılıklar, HDInsight denetim düzlemi işlemleri tarafından bir kümeyi başarıyla oluşturmak için kullanılır.

| **Uç Nokta**                                                          |
|---|
| azure.archive.ubuntu.com:80                                           |
| security.ubuntu.com:80                                                |
| ocsp.msocsp.com:80                                                    |
| ocsp.digicert.com:80                                                  |

## <a name="next-steps"></a>Sonraki adımlar

* [Giden trafiği kısıtlamak için güvenlik duvarı kullanma](./hdinsight-restrict-outbound-traffic.md)
* [Azure HDInsight sanal ağ mimarisi](hdinsight-virtual-network-architecture.md)
