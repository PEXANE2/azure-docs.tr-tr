---
title: BindException - Azure HDInsight'ta halihazırda kullanılmakta olan adres
description: BindException - Azure HDInsight'ta halihazırda kullanılmakta olan adres
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/16/2019
ms.openlocfilehash: 80f984643d6d8be88b381881c6fc1cb1cb5f1815
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75887351"
---
# <a name="scenario-bindexception---address-already-in-use-in-azure-hdinsight"></a>Senaryo: BindException - Azure HDInsight'ta halihazırda kullanılmakta olan adres

Bu makalede, Azure HDInsight kümeleriyle etkileşimde olurken sorun giderme adımları ve sorunlarla ilgili olası çözümler açıklanmaktadır.

## <a name="issue"></a>Sorun

Apache HBase Region Server'daki yeniden başlatma işlemi tamamlayamadı. `region-server.log` Bölge sunucusunun başarısız olduğu alt düğümler üzerindeki `/var/log/hbase` dizinde aşağıdaki gibi benzer bir hata iletisi görebilirsiniz:

```
Caused by: java.net.BindException: Problem binding to /10.2.0.4:16020 : Address already in use
...

Caused by: java.net.BindException: Address already in use
...
```

## <a name="cause"></a>Nedeni

Ağır iş yükü etkinliği sırasında Apache HBase Bölge Sunucularını yeniden başlatma. Aşağıda, bir kullanıcı Apache Ambari UI'den HBase bölge sunucusunda yeniden başlatma işlemini başlattığında arka planda neler olduğu aşağıda verilmiştir:

1. Ambari aracısı bölge sunucusuna bir durdurma isteği gönderir.

1. Ambari aracısı bölge sunucusunun zarif bir şekilde kapanması için 30 saniye bekler

1. Uygulamanız bölge sunucusuna bağlanmaya devam ederse, sunucu hemen kapanmaz. Kapatma gerçekleşmeden önce 30 saniyelik zaman aşımı süresi doluyor.

1. 30 saniye sonra Ambari ajanı bölge sunucusuna bir kuvvet öldürme ()`kill -9`komutu gönderir.

1. Bu ani kapatma nedeniyle, bölge sunucu işlemi öldürülse de, işlemle ilişkili bağlantı `AddressBindException`noktası serbest bırakılmayabilir ve bu da sonunda .

## <a name="resolution"></a>Çözüm

Yeniden başlatmayı başlatmadan önce HBase bölge sunucularında yükü azaltın. Ayrıca, önce tüm tabloları sifonu çekmek iyi bir fikirdir. Tabloların nasıl temizlenirle ilgili bir başvuru için [HDInsight HBase: Tabloları yıkarak Apache HBase küme yeniden başlatma süresini nasıl iyileştireceğiniz.](https://web.archive.org/web/20190112153155/https://blogs.msdn.microsoft.com/azuredatalake/2016/09/19/hdinsight-hbase-how-to-improve-hbase-cluster-restart-time-by-flushing-tables/)

Alternatif olarak, aşağıdaki komutları kullanarak alt düğümlerde bölge sunucularını el ile yeniden başlatmayı deneyin:

```bash
sudo su - hbase -c "/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh stop regionserver"
sudo su - hbase -c "/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh start regionserver"
```

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın. Azure topluluğunu doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)yı gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
