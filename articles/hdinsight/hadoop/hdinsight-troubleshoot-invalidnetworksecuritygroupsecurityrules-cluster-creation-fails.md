---
title: Invalidnetworksecuritygroupsecurityrules hatası-Azure HDInsight
description: Küme oluşturma, ErrorCode ınvalidnetworksecuritygroupsecurityrules ile başarısız oluyor
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/31/2019
ms.openlocfilehash: 1662be3bedb9ec0f2dc21e98ffbd57d8f8ed5777
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2020
ms.locfileid: "92540627"
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

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* [@AzureSupport](https://twitter.com/azuresupport)Azure Community 'yi doğru kaynaklara bağlayarak müşteri deneyimini iyileştirmeye yönelik resmi Microsoft Azure hesabı ile bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği oluşturma](../../azure-portal/supportability/how-to-create-azure-support-request.md)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.