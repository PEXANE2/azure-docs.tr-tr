---
title: Apache ZooKeeper sunucusu Azure HDInsight 'ta çekirdek oluşturmuyor
description: Apache ZooKeeper sunucusu Azure HDInsight 'ta çekirdek oluşturmuyor
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/20/2019
ms.openlocfilehash: 41ac109e5c5379e6085dd57a3fcd8119915558fb
ms.sourcegitcommit: edccc241bc40b8b08f009baf29a5580bf53e220c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82133284"
---
# <a name="apache-zookeeper-server-fails-to-form-a-quorum-in-azure-hdinsight"></a>Apache ZooKeeper sunucusu Azure HDInsight 'ta çekirdek oluşturmuyor

Bu makalede, Azure HDInsight kümeleriyle etkileşim kurarken sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Apache ZooKeeper sunucusu sağlıksız, belirtiler şunları içerebilir: hem kaynak yöneticileri/ad düğümleri bekleme modunda, basit Ise işlemleri çalışmaz, `zkFailoverController` durdurulur ve başlatılamaz, Yarn/Spark/Livy işleri ZooKeeper hataları nedeniyle başarısız olur. LLAP Daemon 'ları, güvenli Spark veya etkileşimli Hive kümelerinde da başlayamaz. Şuna benzer bir hata iletisi görebilirsiniz:

```
19/06/19 08:27:08 ERROR ZooKeeperStateStore: Fatal Zookeeper error. Shutting down Livy server.
19/06/19 08:27:08 INFO LivyServer: Shutting down Livy server.
```

/Var/log/Zookeeper/Zookeeper-Zookeeper-Server-\*. out konumundaki herhangi bir Zookeeper konağında bulunan Zookeeper Server günlüklerinde aşağıdaki hatayı da görebilirsiniz:

```
2020-02-12 00:31:52,513 - ERROR [CommitProcessor:1:NIOServerCnxn@178] - Unexpected Exception:
java.nio.channels.CancelledKeyException
```

## <a name="cause"></a>Nedeni

Anlık görüntü dosyaları hacmi büyük veya anlık görüntü dosyaları bozuksa, ZooKeeper sunucusu bir çekirdek oluşturmayacak ve bu da ZooKeeper ilgili hizmetlerin sağlıksız olmasına neden olur. ZooKeeper sunucusu eski anlık görüntü dosyalarını veri dizininden kaldırmayacak, bunun yerine, ZooKeeper 'in sistem durumunu korumak için Kullanıcı tarafından gerçekleştirilecek düzenli bir görevdir. Daha fazla bilgi için bkz. [ZooKeeper güçleri ve sınırlamaları](https://zookeeper.apache.org/doc/r3.3.5/zookeeperAdmin.html#sc_strengthsAndLimitations).

## <a name="resolution"></a>Çözüm

ZooKeeper veri dizinini `/hadoop/zookeeper/version-2` denetleyin ve `/hadoop/hdinsight-zookeeper/version-2` anlık görüntülerin dosya boyutunun büyük olup olmadığını öğrenin. Büyük anlık görüntüler varsa aşağıdaki adımları uygulayın:

1. "`echo stat | nc {zk_host_ip} 2181 (or 2182)`" Komutuyla sorunsuz çalıştıdiklerinden emin olmak için aynı çekirdekte diğer ZooKeeper sunucularının durumunu kontrol edin.  

1. Sorunlu ZooKeeper konağını, yedekleme anlık görüntülerini ve işlem günlüklerini ve içinde `/hadoop/zookeeper/version-2` oturum `/hadoop/hdinsight-zookeeper/version-2`açın ve ardından bu dosyaları iki dizinde temizleyin. 

1. ZooKeeper veya ZooKeeper konağındaki sorunlu bir sunucuyu yeniden başlatın. Ardından sorunları olan hizmeti yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

- Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

- [@AzureSupport](https://twitter.com/azuresupport) Müşteri deneyimini iyileştirmek için resmi Microsoft Azure hesabına bağlanın. Azure Community 'yi doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

- Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
