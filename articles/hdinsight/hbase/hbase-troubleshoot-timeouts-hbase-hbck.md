---
title: Azure HDInsight 'ta ' HBase hbck ' komutuyla zaman aşımları
description: Bölge atamalarını çözmede ' HBase hbck ' komutuyla zaman aşımı sorunu
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 08/01/2019
ms.openlocfilehash: 4f3f1c22fa1dc05a66a8b6bf0179903a44cef9b6
ms.sourcegitcommit: c662440cf854139b72c998f854a0b9adcd7158bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2019
ms.locfileid: "68737933"
---
# <a name="scenario-timeouts-with-hbase-hbck-command-in-azure-hdinsight"></a>Senaryo: Azure HDInsight 'ta ' HBase hbck ' komutuyla zaman aşımları

Bu makalede, Azure HDInsight kümeleriyle etkileşim kurarken sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Bölge atamalarını düzelterek `hbase hbck` komutla birlikte zaman aşımları ile karşılaşırsınız.

## <a name="cause"></a>Nedeni

Buradaki olası neden, "geçiş sırasında" durumunda uzun bir süre içinde birkaç bölge olabilir. Bu bölgeler, Apache HBase Master kullanıcı arabiriminden çevrimdışı olarak görülebilir. Geçişe çalışan yüksek sayıda bölge nedeniyle HBase Master zaman aşımına uğrar ve bu bölgeleri çevrimiçi duruma geri getiremeyecektir.

## <a name="resolution"></a>Çözüm

1. SSH kullanarak HDInsight HBase kümesinde oturum açın.

1. Zookeeper `hbase zkcli` Shell ile bağlanmak için komutunu çalıştırın.

1. `rmr /hbase/regions-in-transition` Veya`rmr /hbase-unsecure/regions-in-transition` komutunu çalıştırın.

1. Komutunu kullanarak `hbase zkcli` `exit` kabuktan çıkın.

1. Ambarı Kullanıcı arabirimini açın ve Active HBase Master hizmeti 'ni ambarı 'ndan yeniden başlatın.

1. Hiçbir bölgenin takılı olmadığından emin olmak için, bu bölümdeki HBase Master Kullanıcı arabirimi "geçiş sırasında bölgesi" ni izleyin.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* Azure Community [@AzureSupport](https://twitter.com/azuresupport) 'yi doğru kaynaklara bağlayarak müşteri deneyimini iyileştirmeye yönelik resmi Microsoft Azure hesabı ile bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
