---
title: Apache ambarı Kullanıcı arabirimi, Azure HDInsight 'ta konakları ve Hizmetleri gösterir
description: Azure HDInsight 'ta konakları ve Hizmetleri gösterdiği bir Apache ambarı Kullanıcı arabirimi sorununu giderme
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 08/02/2019
ms.openlocfilehash: bc65387252fd425256684474f38ec5d783bb45e7
ms.sourcegitcommit: 97605f3e7ff9b6f74e81f327edd19aefe79135d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70734846"
---
# <a name="scenario-apache-ambari-ui-shows-down-hosts-and-services-in-azure-hdinsight"></a>Senaryo: Apache ambarı Kullanıcı arabirimi, Azure HDInsight 'ta konakları ve Hizmetleri gösterir

Bu makalede, Azure HDInsight kümeleriyle etkileşim kurarken sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Apache ambarı Kullanıcı arabirimine erişilebilir, ancak kullanıcı arabirimi neredeyse tüm hizmetleri gösteriyor, sinyal kaybı gösteren tüm konaklar.

## <a name="cause"></a>Nedeni

Çoğu senaryoda, bu, ambarı sunucusu etkin yayın düğümünde çalıştırmayan bir sorundur. Hangi baş düğümüne 'un etkin bir yayın düğümü olduğunu kontrol edin ve tek başına ambarı-sunucunuzun çalıştığından emin olun. Ambarı-sunucu ' ı el ile başlatmayın, yük devretme denetleyicisi hizmeti 'nin, doğru baş düğümde ambarı-sunucu başlatmasından sorumlu olmasına izin verin. Yük devretmeyi zorlamak için etkin yayın düğümünü yeniden başlatın.

Ağ sorunları da bu soruna neden olabilir. Her küme düğümünden, ping işlemi `headnodehost`yapabiliyor musunuz? bölümüne bakın. Küme düğümünün bağlanamabileceği `headnodehost`nadir bir durum vardır:

```
$>telnet headnodehost 8440
... No route to host
```

## <a name="resolution"></a>Çözüm

Genellikle etkin baş düğümüne 'un yeniden başlatılması bu sorunu azaltır. Değilse lütfen HDInsight destek ekibine başvurun.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* Azure Community [@AzureSupport](https://twitter.com/azuresupport) 'yi doğru kaynaklara bağlayarak müşteri deneyimini iyileştirmeye yönelik resmi Microsoft Azure hesabı ile bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
