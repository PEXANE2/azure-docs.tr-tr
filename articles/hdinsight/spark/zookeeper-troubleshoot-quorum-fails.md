---
title: Apache ZooKeeper sunucusu Azure HDInsight 'ta çekirdek oluşturmuyor
description: Apache ZooKeeper sunucusu Azure HDInsight 'ta çekirdek oluşturmuyor
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/20/2019
ms.openlocfilehash: a0874826529b5c9ca5d6d4107fe820cd522d81d0
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2020
ms.locfileid: "75894041"
---
# <a name="apache-zookeeper-server-fails-to-form-a-quorum-in-azure-hdinsight"></a>Apache ZooKeeper sunucusu Azure HDInsight 'ta çekirdek oluşturmuyor

Bu makalede, Azure HDInsight kümeleriyle etkileşim kurarken sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Apache ZooKeeper sunucusu sağlıksız, belirtiler şunları içerebilir: hem kaynak yöneticileri/ad düğümleri bekleme modunda, basit Ise işlemleri çalışmaz, `zkFailoverController` durdurulur ve başlatılamaz, Yarn/Spark/Livy işleri Zookeeper hataları nedeniyle başarısız olur. Şuna benzer bir hata iletisi görebilirsiniz:

```
19/06/19 08:27:08 ERROR ZooKeeperStateStore: Fatal Zookeeper error. Shutting down Livy server.
19/06/19 08:27:08 INFO LivyServer: Shutting down Livy server.
```

## <a name="cause"></a>Nedeni

Anlık görüntü dosyaları hacmi büyük veya anlık görüntü dosyaları bozuksa, ZooKeeper sunucusu bir çekirdek oluşturmayacak ve bu da ZooKeeper ilgili hizmetlerin sağlıksız olmasına neden olur. ZooKeeper sunucusu eski anlık görüntü dosyalarını veri dizininden kaldırmayacak, bunun yerine, ZooKeeper 'in sistem durumunu korumak için Kullanıcı tarafından gerçekleştirilecek düzenli bir görevdir. Daha fazla bilgi için bkz. [ZooKeeper güçleri ve sınırlamaları](https://zookeeper.apache.org/doc/r3.3.5/zookeeperAdmin.html#sc_strengthsAndLimitations).

## <a name="resolution"></a>Çözünürlük

Anlık görüntü dosyasının boyutunun büyük olup olmadığını öğrenmek için ZooKeeper veri dizinini `/hadoop/zookeeper/version-2` ve `/hadoop/hdinsight-zookeepe/version-2` denetleyin. Büyük anlık görüntüler varsa aşağıdaki adımları uygulayın:

1. `/hadoop/zookeeper/version-2` ve `/hadoop/hdinsight-zookeepe/version-2`anlık görüntülerini yedekleyin.

1. `/hadoop/zookeeper/version-2` ve `/hadoop/hdinsight-zookeepe/version-2`anlık görüntüleri temizleyin.

1. Tüm ZooKeeper sunucularını Apache ambarı kullanıcı arabiriminden yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

- Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

- [@AzureSupport](https://twitter.com/azuresupport) ile bağlanma-müşteri deneyimini iyileştirmek için resmi Microsoft Azure hesabı. Azure Community 'yi doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

- Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
