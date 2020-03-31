---
title: Apache ZooKeeper sunucusu Azure HDInsight'ta çoğunluk oluşturamıyor
description: Apache ZooKeeper sunucusu Azure HDInsight'ta çoğunluk oluşturamıyor
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/20/2019
ms.openlocfilehash: 4e46efaf17ae9bad5df6f1f61f401d3e6de58a85
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78250231"
---
# <a name="apache-zookeeper-server-fails-to-form-a-quorum-in-azure-hdinsight"></a>Apache ZooKeeper sunucusu Azure HDInsight'ta çoğunluk oluşturamıyor

Bu makalede, Azure HDInsight kümeleriyle etkileşimde olurken sorun giderme adımları ve sorunlarla ilgili olası çözümler açıklanmaktadır.

## <a name="issue"></a>Sorun

Apache ZooKeeper sunucu sağlıksız, belirtiler içerebilir: hem Kaynak Yöneticileri / Ad Düğümleri bekleme modunda, basit HDFS işlemleri çalışmıyor, `zkFailoverController` durdurulur ve başlatılamaz, İplik / Spark / Livy işleri Zookeeper hataları nedeniyle başarısız. LLAP Daemons da Secure Spark veya Interactive Hive kümeleri başlatmak için başarısız olabilir. Şuna benzer bir hata iletisi görebilirsiniz:

```
19/06/19 08:27:08 ERROR ZooKeeperStateStore: Fatal Zookeeper error. Shutting down Livy server.
19/06/19 08:27:08 INFO LivyServer: Shutting down Livy server.
```

Zookeeper Server/var/log/zookeeper/zookeeper-zookeeper-zookeeper-server-\*adresindeki zookeeper host'ungünlüklerinde aşağıdaki hatayı da görebilirsiniz:

```
2020-02-12 00:31:52,513 - ERROR [CommitProcessor:1:NIOServerCnxn@178] - Unexpected Exception:
java.nio.channels.CancelledKeyException
```

## <a name="cause"></a>Nedeni

Anlık görüntü dosyalarının hacmi büyük olduğunda veya anlık görüntü dosyaları bozulduğunda, ZooKeeper sunucusu ZooKeeper ile ilgili hizmetlerin sağlıksız olması nedeniyle bir çoğunluk oluşturmakta başarısız olur. ZooKeeper sunucusu eski anlık görüntü dosyalarını veri dizininden kaldırmaz, bunun yerine, ZooKeeper'ın sağlıklı lığını korumak için kullanıcılar tarafından gerçekleştirilecek periyodik bir görevdir. Daha fazla bilgi için [ZooKeeper Güçlü ve Sınırlamalar'a](https://zookeeper.apache.org/doc/r3.3.5/zookeeperAdmin.html#sc_strengthsAndLimitations)bakın.

## <a name="resolution"></a>Çözüm

ZooKeeper veri dizinini kontrol edin `/hadoop/zookeeper/version-2` ve `/hadoop/hdinsight-zookeeper/version-2` anlık görüntü dosya boyutunun büyük olup olmadığını öğrenin. Büyük anlık görüntüler varsa aşağıdaki adımları izleyin:

1. Anlık görüntüleri `/hadoop/zookeeper/version-2` yedekle `/hadoop/hdinsight-zookeeper/version-2`ve .

1. Anlık görüntüleri `/hadoop/zookeeper/version-2` temizleyin `/hadoop/hdinsight-zookeeper/version-2`ve .

1. Apache Ambari UI'deki tüm ZooKeeper sunucularını yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

- [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

- [@AzureSupport](https://twitter.com/azuresupport) Müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın. Azure topluluğunu doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

- Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)yı gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
