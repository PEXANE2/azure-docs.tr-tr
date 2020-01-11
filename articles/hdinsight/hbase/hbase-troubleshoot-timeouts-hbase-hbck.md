---
title: Azure HDInsight 'ta ' HBase hbck ' komutuyla zaman aşımları
description: Bölge atamalarını çözmede ' HBase hbck ' komutuyla zaman aşımı sorunu
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/16/2019
ms.openlocfilehash: 5604b42e1611830f3aaea9ae180cdb8142ab0942
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2020
ms.locfileid: "75887198"
---
# <a name="scenario-timeouts-with-hbase-hbck-command-in-azure-hdinsight"></a>Senaryo: Azure HDInsight 'ta ' HBase hbck ' komutuyla zaman aşımları

Bu makalede, Azure HDInsight kümeleriyle etkileşim kurarken sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Bölge atamalarını düzelterek `hbase hbck` komutuyla zaman aşımları ile karşılaşırsınız.

## <a name="cause"></a>Nedeni

`hbck` komutunu kullandığınızda zaman aşımı sorunları için olası bir neden, uzun bir süredir "geçişte" durumunda birkaç bölgenin olması olabilir. Bu bölgeleri HBase Master Kullanıcı arabiriminde çevrimdışı olarak görebilirsiniz. Çok sayıda bölge geçişe çalıştığından HBase Master zaman aşımına uğrar ve bu bölgeleri yeniden çevrimiçi getiremeyebilirsiniz.

## <a name="resolution"></a>Çözünürlük

1. SSH kullanarak HDInsight HBase kümesinde oturum açın.

1. Apache ZooKeeper Shell ile bağlanmak için `hbase zkcli` komutunu çalıştırın.

1. `rmr /hbase/regions-in-transition` veya `rmr /hbase-unsecure/regions-in-transition` komutunu çalıştırın.

1. `exit` komutu kullanarak `hbase zkcli` kabuğu 'ndan çıkın.

1. Apache ambarı kullanıcı arabiriminden, etkin HBase Master hizmetini yeniden başlatın.

1. `hbase hbck -fixAssignments` komutunu çalıştırın.

1. Hiçbir bölgenin takılı olmadığından emin olmak için, bu bölümdeki HBase Master Kullanıcı arabirimi "geçiş sırasında bölgesi" ni izleyin.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

- Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

- [@AzureSupport](https://twitter.com/azuresupport) ile bağlanma-müşteri deneyimini iyileştirmek için resmi Microsoft Azure hesabı. Azure Community 'yi doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

- Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
