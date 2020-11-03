---
title: Invalidnetworksecuritygroupsecurityrules hatası-Azure HDInsight
description: Küme oluşturma, ErrorCode ınvalidnetworksecuritygroupsecurityrules ile başarısız oluyor
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/31/2019
ms.openlocfilehash: b1423dc965a3169a5f615ccc371849cc177be244
ms.sourcegitcommit: 7863fcea618b0342b7c91ae345aa099114205b03
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2020
ms.locfileid: "93289095"
---
# <a name="scenario-invalidnetworksecuritygroupsecurityrules---cluster-creation-fails-in-azure-hdinsight"></a>Senaryo: ınvalidnetworksecuritygroupsecurityrules-Azure HDInsight 'ta küme oluşturma başarısız oluyor

Bu makalede, Azure HDInsight kümeleriyle etkileşim kurarken sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

`InvalidNetworkSecurityGroupSecurityRules`Aşağıdakine benzer bir açıklamayla hata kodu alıyorsunuz "alt ağ ile yapılandırılan ağ güvenlik grubundaki güvenlik kuralları, gerekli gelen ve/veya giden bağlantılara izin vermiyor."

## <a name="cause"></a>Nedeni

Kümeniz için yapılandırılmış gelen [ağ güvenlik grubu](../../virtual-network/virtual-network-vnet-plan-design-arm.md) kuralları ile ilgili bir sorun olabilir.

## <a name="resolution"></a>Çözüm

Azure portal gidin ve kümenin dağıtıldığı alt ağla ilişkili NSG 'yi tanımla. **Gelen güvenlik kuralları** bölümünde, kuralların [burada](../control-network-traffic.md)bahsedilen IP adresleri için 443 numaralı bağlantı noktasına gelen erişime izin verecek şekilde emin olun.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [troubleshooting next steps](../../../includes/hdinsight-troubleshooting-next-steps.md)]