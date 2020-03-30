---
title: Azure HDInsight'ta bağlantı sıfırladıktan sonra depolama özel durumu
description: Azure HDInsight'ta bağlantı sıfırladıktan sonra depolama özel durumu
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/08/2019
ms.openlocfilehash: a7af6407191577112f936bfb9048985e85c868ea
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75887232"
---
# <a name="scenario-storage-exception-after-connection-reset-in-azure-hdinsight"></a>Senaryo: Azure HDInsight'ta bağlantı sıfırladıktan sonra depolama özel durumu

Bu makalede, Azure HDInsight kümeleriyle etkileşimde olurken sorun giderme adımları ve sorunlarla ilgili olası çözümler açıklanmaktadır.

## <a name="issue"></a>Sorun

Yeni Apache HBase tablosu oluşturulamıyor.

## <a name="cause"></a>Nedeni

Tablo kesilme işlemi sırasında bir depolama bağlantısı sorunu vardı. Tablo girişi HBase meta veri tablosunda silindi. Bir blob dosyası hariç hepsi silindi.

Depoda oturma denilen `/hbase/data/default/ThatTable` bir klasör blob olmasına rağmen. WASB sürücüsü yukarıdaki blob dosyasının varlığını buldu ve ana klasörlerin `/hbase/data/default/ThatTable` var olduğunu varsaydığı için herhangi bir blob oluşturmaya izin vermez, böylece tablo oluşturma başarısız olur.

## <a name="resolution"></a>Çözüm

1. Apache Ambari UI'den etkin HMaster'ı yeniden başlatın. Bu, iki bekleme HMaster'dan birinin etkin olmasını sağlar ve yeni etkin HMaster meta veri tablosu bilgilerini yeniden yükler. Böylece `already-deleted` HMaster UI tablo görmezsiniz.

1. Bulut Gezgini veya 'yi çalıştıran komut gibi UI `hdfs dfs -ls /xxxxxx/yyyyy`araçlarından yetim blob dosyasını bulabilirsiniz. Bu `hdfs dfs -rmr /xxxxx/yyyy` lekeyi silmek için çalıştırın. Örneğin, `hdfs dfs -rmr /hbase/data/default/ThatTable/ThatFile`.

Artık HBase'de aynı ada sahip yeni tablo oluşturabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın. Azure topluluğunu doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)yı gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
