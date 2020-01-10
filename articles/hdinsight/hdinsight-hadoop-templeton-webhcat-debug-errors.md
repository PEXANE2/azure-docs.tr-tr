---
title: HDInsight 'ta WebHCat hatalarını anlama ve çözme-Azure
description: HDInsight 'ta WebHCat tarafından döndürülen yaygın hataları ve bunların nasıl çözümleneceğini öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 01/01/2020
ms.openlocfilehash: 011ef4f192bbae12be7d2464d5b0526f584821a6
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75638859"
---
# <a name="understand-and-resolve-errors-received-from-webhcat-on-hdinsight"></a>HDInsight üzerinde WebHCat 'den alınan hataları anlama ve çözme

HDInsight ile WebHCat kullanırken alınan hatalar ve bunları çözme hakkında bilgi edinin. WebHCat, Azure PowerShell gibi istemci tarafı araçları ve Visual Studio için Data Lake araçları tarafından dahili olarak kullanılır.

## <a name="what-is-webhcat"></a>WebHCat nedir?

[Webhcat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) , Apache Hadoop Için bir [hcatalog](https://cwiki.apache.org/confluence/display/Hive/HCatalog), tablo ve depolama yönetimi katmanı REST API. WebHCat, HDInsight kümelerinde varsayılan olarak etkindir ve kümede oturum açmadan işleri göndermek, iş durumunu almak vb. gibi çeşitli araçlar tarafından kullanılır.

## <a name="modifying-configuration"></a>Yapılandırmayı değiştirme

Yapılandırılan en büyük değer aşıldığından bu belgede listelenen hataların birkaçı meydana gelir. Çözüm adımı bir değeri değiştiremeyeceğiniz konusunda bahsetme yaparken, değeri değiştirmek için Apache ambarı 'nı (Web veya REST API) kullanın. Daha fazla bilgi için bkz. [Apache ambarı kullanarak HDInsight 'ı yönetme](hdinsight-hadoop-manage-ambari.md)

### <a name="default-configuration"></a>Varsayılan yapılandırma

Aşağıdaki varsayılan değerler aşılırsa, WebHCat performansını düşürebilir veya hatalara neden olabilir:

| Ayar | Neler yapar? | Varsayılan değer |
| --- | --- | --- |
| [Yarn. Scheduler. kapasite. maksimum-uygulamalar][maximum-applications] |Aynı anda etkin olabilen en fazla iş sayısı (bekliyor veya çalışıyor) |10,000 |
| [temptaton. Exec. Max-procs][max-procs] |Eşzamanlı olarak sunulabilecek en fazla istek sayısı |20 |
| [MapReduce. jobhistory. max-age-MS][max-age-ms] |İş geçmişinin tutulacağı gün sayısı |7 gün |

## <a name="too-many-requests"></a>Çok fazla istek

**Http durum kodu**: 429

| Nedeni | Çözünürlük |
| --- | --- |
| Web Hcat tarafından dakikada sunulan en fazla eşzamanlı istek sayısını aştık (varsayılan 20) |En fazla eşzamanlı istek sayısından daha fazla bilgi göndermemenizi veya `templeton.exec.max-procs`değiştirerek eşzamanlı istek sınırını arttırtığınızdan emin olmak için iş yükünüzü azaltın. Daha fazla bilgi için bkz. [yapılandırmayı değiştirme](#modifying-configuration) |

## <a name="server-unavailable"></a>Sunucu kullanılamıyor

**Http durum kodu**: 503

| Nedeni | Çözünürlük |
| --- | --- |
| Bu durum kodu genellikle küme için birincil ve ikincil HeadNode arasında yük devretme sırasında oluşur |İki dakika bekleyip işlemi yeniden deneyin |

## <a name="bad-request-content-could-not-find-job"></a>Hatalı istek Içeriği: iş bulunamadı

**Http durum kodu**: 400

| Nedeni | Çözünürlük |
| --- | --- |
| İş ayrıntıları, iş geçmişi temizleyici tarafından temizlendi |İş geçmişi için varsayılan saklama süresi 7 gündür. Varsayılan saklama süresi `mapreduce.jobhistory.max-age-ms`değiştirilerek değiştirilebilir. Daha fazla bilgi için bkz. [yapılandırmayı değiştirme](#modifying-configuration) |
| İş, yük devretme nedeniyle sonlandırıldı |İş gönderimini iki dakikaya kadar yeniden deneyin |
| Geçersiz bir iş KIMLIĞI kullanıldı |İş KIMLIĞININ doğru olup olmadığını denetle |

## <a name="bad-gateway"></a>Hatalı ağ geçidi

**Http durum kodu**: 502

| Nedeni | Çözünürlük |
| --- | --- |
| Web Hcat işleminde iç çöp toplama işlemi yapılıyor |Çöp toplamanın bitmesini bekleyin veya WebHCat hizmetini yeniden başlatın |
| ResourceManager hizmetinden yanıt beklerken zaman aşımı. Etkin uygulama sayısı yapılandırılmış en yüksek değeri geçtiğinde bu hata oluşabilir (varsayılan 10.000) |Çalışmakta olan işlerin tamamlanmasını bekleyin veya `yarn.scheduler.capacity.maximum-applications`değiştirerek eşzamanlı iş sınırını artırın. Daha fazla bilgi için [yapılandırma değiştirme](#modifying-configuration) bölümüne bakın. |
| `Fields` `*` olarak ayarlandığında [Get/Jobs](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference+Jobs) çağrısı aracılığıyla tüm işler alınmaya çalışılıyor |*Tüm* iş ayrıntılarını alma. Bunun yerine, yalnızca belirli iş KIMLIĞINDEN daha büyük işlerin ayrıntılarını almak için `jobid` kullanın. Veya `Fields` kullanmayın |
| WebHCat hizmeti, yayın düğümü yük devretmesi sırasında çalışmıyor |İki dakika bekleyip işlemi yeniden deneyin |
| WebHCat aracılığıyla en çok 500 bekleyen iş gönderildi |Daha fazla iş göndermeden önce Şu anda bekleyen işlerin tamamlanmasını bekleyin |

[maximum-applications]: https://docs.cloudera.com/HDPDocuments/HDP2/HDP-2.1.3/bk_system-admin-guide/content/setting_application_limits.html
[max-procs]: https://cwiki.apache.org/confluence/display/Hive/WebHCat+Configure#WebHCatConfigure-WebHCatConfiguration
[max-age-ms]: https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.6.0/ds_Hadoop/hadoop-mapreduce-client/hadoop-mapreduce-client-core/mapred-default.xml
