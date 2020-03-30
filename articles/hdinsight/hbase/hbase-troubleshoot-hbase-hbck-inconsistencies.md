---
title: hbase hbck Azure HDInsight tutarsızlıkları döndürür
description: hbase hbck Azure HDInsight tutarsızlıkları döndürür
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/08/2019
ms.openlocfilehash: fa02ac0dfe229f3e82d1c1c62d83ca06a81efca6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75887334"
---
# <a name="scenario-hbase-hbck-command-returns-inconsistencies-in-azure-hdinsight"></a>Senaryo: `hbase hbck` komut Azure HDInsight'ta tutarsızlıkları döndürür

Bu makalede, Azure HDInsight kümeleriyle etkileşimde olurken sorun giderme adımları ve sorunlarla ilgili olası çözümler açıklanmaktadır.

## <a name="issue-region-is-not-in-hbasemeta"></a>Sorun: Bölge içinde değil`hbase:meta`

HDFS'de bölge xxx, `hbase:meta` ancak herhangi bir bölge sunucusunda listelenmez veya dağıtılmez.

### <a name="cause"></a>Nedeni

Değişir.

### <a name="resolution"></a>Çözüm

1. Çalıştırarak meta tabloyu düzeltin:

    ```
    hbase hbck -ignorePreCheckPermission –fixMeta
    ```

1. Çalıştırarak regionServers'a bölge atama:

    ```
    hbase hbck -ignorePreCheckPermission –fixAssignment
    ```
---

## <a name="issue-region-is-offline"></a>Sorun: Bölge çevrimdışı

Bölge xxx herhangi bir RegionServer'da dağıtılmamada. Bu, bölgenin çevrimdışı `hbase:meta`olduğu, ancak çevrimdışı olduğu anlamına gelir.

### <a name="cause"></a>Nedeni

Değişir.

### <a name="resolution"></a>Çözüm

Bölgeleri çalıştırarak çevrimiçi duruma getirin:

```
hbase hbck -ignorePreCheckPermission –fixAssignment
```

---

## <a name="issue-regions-have-the-same-startend-keys"></a>Sorun: Bölgeler aynı başlangıç/bitiş tuşlarına sahiptir

### <a name="cause"></a>Nedeni

Değişir.

### <a name="resolution"></a>Çözüm

Bu üst üste binen bölgeleri el ile birleştirin. HBase HMaster Web UI tablosu bölümüne gidin, sorun olan tablo bağlantısını seçin. Bu tabloya ait her bölgenin başlangıç anahtarı/bitiş anahtarını görürsünüz. Ardından, üst üste tokatlanan bölgeleri birleştirin. HBase kabuğunda, `merge_region 'xxxxxxxx','yyyyyyy', true`yap. Örnek:

```
RegionA, startkey:001, endkey:010,

RegionB, startkey:001, endkey:080,

RegionC, startkey:010, endkey:080.
```

Bu senaryoda, RegionA ve RegionC'yi birleştirmeniz ve RegionB ile aynı anahtar aralığıyla RegionD'yi almanız, ardından RegionB ve RegionD'yi birleştirmeniz gerekir. xxxxxxx ve yyyy her bölge adının sonunda hash dize vardır. Burada iki kesintili bölgeleri birleştirmemeye dikkat edin. A ve C birleştirme gibi her birleştirmeden sonra, HBase RegionD'de bir sıkıştırma başlatır. Bölge D ile başka bir birleştirme yapmadan önce sıkıştırmanın tamamlanmasını bekleyin. Sıkıştırma durumunu HBase HMaster UI'de o bölge sunucusu sayfasında bulabilirsiniz.

---

## <a name="issue-cant-load-regioninfo"></a>Sorun: Yükleyemiyorum`.regioninfo`

Bölge `/hbase/data/default/tablex/regiony`için `.regioninfo` yük olamaz.

### <a name="cause"></a>Nedeni

Bunun nedeni büyük olasılıkla RegionServer çöktüğünde veya VM yeniden başlatıldığında kısmi silme işleminden kaynaklanmaktadır. Şu anda Azure Depolama düz bir blob dosya sistemidir ve bazı dosya işlemleri atomik değildir.

### <a name="resolution"></a>Çözüm

Kalan dosya ve klasörleri el ile temizleyin:

1. Hangi `hdfs dfs -ls /hbase/data/default/tablex/regiony` klasörlerin/dosyaların hala altında olduğunu denetlemek için yürütün.

1. Tüm `hdfs dfs -rmr /hbase/data/default/tablex/regiony/filez` alt dosyaları/klasörleri silmek için yürüt

1. Bölge `hdfs dfs -rmr /hbase/data/default/tablex/regiony` klasörünü silmek için yürütün.

---

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın. Azure topluluğunu doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)yı gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
