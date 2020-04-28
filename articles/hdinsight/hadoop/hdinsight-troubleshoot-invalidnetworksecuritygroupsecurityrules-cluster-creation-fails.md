---
title: Invalidnetworksecuritygroupsecurityrules hatası-Azure HDInsight
description: Küme oluşturma, ErrorCode ınvalidnetworksecuritygroupsecurityrules ile başarısız oluyor
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/31/2019
ms.openlocfilehash: a73e1e9f7a9c017dd29b627a24c25ae2e064c0a9
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75894138"
---
# <a name="scenario-invalidnetworksecuritygroupsecurityrules---cluster-creation-fails-in-azure-hdinsight"></a>Senaryo: ınvalidnetworksecuritygroupsecurityrules-Azure HDInsight 'ta küme oluşturma başarısız oluyor

Bu makalede, Azure HDInsight kümeleriyle etkileşim kurarken sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Aşağıdakine benzer bir açıklamayla `InvalidNetworkSecurityGroupSecurityRules` hata kodu alıyorsunuz "alt ağ Ile yapılandırılan ağ güvenlik grubundaki güvenlik kuralları, gerekli gelen ve/veya giden bağlantılara izin vermiyor."

## <a name="cause"></a>Nedeni

Kümeniz için yapılandırılmış gelen [ağ güvenlik grubu](../../virtual-network/virtual-network-vnet-plan-design-arm.md) kuralları ile ilgili bir sorun olabilir.

## <a name="resolution"></a>Çözüm

Azure portal gidin ve kümenin dağıtıldığı alt ağla ilişkili NSG 'yi tanımla. **Gelen güvenlik kuralları** bölümünde, kuralların [burada](../hdinsight-plan-virtual-network-deployment.md#hdinsight-ip)bahsedilen IP adresleri için 443 numaralı bağlantı noktasına gelen erişime izin verecek şekilde emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* Azure Community [@AzureSupport](https://twitter.com/azuresupport) 'yi doğru kaynaklara bağlayarak müşteri deneyimini iyileştirmeye yönelik resmi Microsoft Azure hesabı ile bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
