---
title: GeçersizAğGüvenliğiGroupSecurityRules hatası - Azure HDInsight
description: Küme Oluşturma Hatası Kodu InvalidNetworkSecurityGroupSecurityRules ile Başarısız Olur
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/31/2019
ms.openlocfilehash: a73e1e9f7a9c017dd29b627a24c25ae2e064c0a9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75894138"
---
# <a name="scenario-invalidnetworksecuritygroupsecurityrules---cluster-creation-fails-in-azure-hdinsight"></a>Senaryo: GeçersizAğGüvenlikGroupSecurityRules - küme oluşturma Azure HDInsight başarısız

Bu makalede, Azure HDInsight kümeleriyle etkileşimde olurken sorun giderme adımları ve sorunlarla ilgili olası çözümler açıklanmaktadır.

## <a name="issue"></a>Sorun

"Alt ağla yapılandırılan Ağ Güvenlik Grubu'ndaki güvenlik kuralları, gerekli gelen ve/veya giden bağlantıya izin vermez" açıklamasına benzer bir açıklama içeren hata kodu `InvalidNetworkSecurityGroupSecurityRules` alırsınız.

## <a name="cause"></a>Nedeni

Büyük olasılıkla kümeniz için yapılandırılan gelen [ağ güvenlik grubu](../../virtual-network/virtual-network-vnet-plan-design-arm.md) kurallarıyla ilgili bir sorun.

## <a name="resolution"></a>Çözüm

Azure portalına gidin ve kümenin dağıtıldığı alt ağla ilişkili NSG'yi tanımlayın. Gelen **güvenlik kuralları** bölümünde, kurallar [burada](../hdinsight-plan-virtual-network-deployment.md#hdinsight-ip)belirtilen IP adresleri için bağlantı noktası 443'e gelen erişime izin verdi.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Azure topluluğunu doğru kaynaklara bağlayarak müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği nin nasıl oluşturulabildiğini](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
