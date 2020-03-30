---
title: Azure HDInsight'ta Apache Ambari Hive View'dan sorgu çalıştırırken özel durum
description: Azure HDInsight'ta Apache Ambari Hive Görünümü üzerinden Apache Hive sorgularını çalıştırırken sorun giderme adımları.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 12/23/2019
ms.openlocfilehash: 809b2e383eb57b730fd76ec2194764178aa810c0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75895042"
---
# <a name="exception-when-running-queries-from-apache-ambari-hive-view-in-azure-hdinsight"></a>Azure HDInsight'ta Apache Ambari Hive View'dan sorgu çalıştırırken özel durum

Bu makalede, Azure HDInsight kümeleriyle etkileşimde olurken sorun giderme adımları ve sorunlarla ilgili olası çözümler açıklanmaktadır.

## <a name="issue"></a>Sorun

Apache Ambari Hive Görünümü'nden bir Apache Hive sorgusu çalıştırırken, aralıklı olarak aşağıdaki hata iletisini alırsınız:

```error
Cannot create property 'errors' on string '<!DOCTYPE html PUBLIC '-//W3C//DTD XHTML 1.0 Strict//EN' 'http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd'>
<html xmlns='http://www.w3.org/1999/xhtml'>
<head>
<title>IIS 8.5 Detailed Error - 502.3 - Bad Gateway</title>
```

## <a name="cause"></a>Nedeni

Bir Ağ Geçidi zaman.

Ağ Geçidi zaman ödeme değeri 2 dakikadır. Ambari Hive View'daki sorgular `/hive2` ağ geçidi aracılığıyla bitiş noktasına gönderilir. Sorgu başarıyla derlendikten ve kabul edildikten sonra `queryid`HiveServer bir . İstemciler daha sonra sorgunun durumu için yoklama tutun. Bu işlem sırasında, HiveServer 2 dakika içinde bir HTTP yanıtı döndürmezse, HDI Ağ Geçidi arayana 502,3 Ağ Geçidi zaman alakart hatası atar. Hatalar, sorgu işleme için gönderildiğinde (daha olasıdır) ve ayrıca durum çağrısı nı (daha az olası) alabilir. Kullanıcılar her ikisini de görebiliyordu.

Http işleyici iş parçacığı hızlı olması gerekiyordu: iş `queryid`hazırlamak ve bir . Ancak, çeşitli nedenlerden dolayı, tüm işleyici iş parçacıkları yeni sorgular ve get status aramaları için zaman adabı neden meşgul olabilir.

### <a name="responsibilities-of-the-http-handler-thread"></a>HTTP işleyicisi iş parçacığının sorumlulukları

İstemci HiveServer'a bir sorgu gönderdiğinde, ön planda aşağıdakileri yapar:

* İsteği ayrıştın, anlamsal doğrulama yapın
* Kilit edinme
* Gerekirse Metastore arama
* Sorguyu derleme (DDL veya DML)
* Sorgu planı hazırlama
* Yetkilendirme gerçekleştirin (Geçerli tüm ranger ilkelerini güvenli kümelerde çalıştırın)

## <a name="resolution"></a>Çözüm

Durumu iyileştirmek için size bazı genel öneriler:

* Harici bir kovan metastore kullanıyorsanız, DB ölçümlerini denetleyin ve veritabanının aşırı yüklenmediğinden emin olun. Metastore veritabanı katmanını ölçeklemeyi düşünün.

* Paralel operasyonların açık olduğundan emin olun (bu, HTTP işleyicisi iş parçacıklarının paralel çalışmasını sağlar). Değeri doğrulamak [için, Apache Ambari'yi](../hdinsight-hadoop-manage-ambari.md) başlatın ve **Hive** > **Configs** > **Advanced** > Custom**hive-site'ye**gidin. Değeri `hive.server2.parallel.ops.in.session` olmalıdır. `true`

* Kümenin VM SKU'su yük için çok küçük olmadığından emin olun. Çalışmayı birden çok kümeye bölmeyi düşünün. Daha fazla bilgi için [bkz.](../hdinsight-capacity-planning.md#choose-a-cluster-type)

* Ranger kümeye yüklüyse, lütfen her sorgu için değerlendirilmesi gereken çok fazla Ranger politikası olup olmadığını kontrol edin. Yinelenen veya gereksiz ilkeleri arayın.

* Ambari'den **HiveServer2 Yığın Boyutu** değerini kontrol edin. **Hive** > **Configs** > **Ayarları** > **Optimizasyonu**gidin. Değerin 10 GB'dan büyük olduğundan emin olun. Performansı en iyi duruma getirmek için gerektiği gibi ayarlayın.

* Hive sorgusunun iyi ayarlandığından emin olun. Daha fazla bilgi için Azure [HDInsight'ta Apache Hive sorgularını optimize edin.](../hdinsight-hadoop-optimize-hive-query.md)

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın. Azure topluluğunu doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)yı gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
