---
title: HDInsight'ta WebHCat hatalarını anlama ve çözme - Azure
description: WEBHCat tarafından HDInsight'ta döndürülen yaygın hatalar hakkında bilgi edinin ve bunları nasıl çözeceğinizi öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 01/01/2020
ms.openlocfilehash: 011ef4f192bbae12be7d2464d5b0526f584821a6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75638859"
---
# <a name="understand-and-resolve-errors-received-from-webhcat-on-hdinsight"></a>HDInsight’ta WebHCat’ten alınan hataları anlama ve düzeltme

HDInsight ile WebHCat kullanırken alınan hatalar hakkında bilgi edinin ve bunları nasıl çözeceğiniz hakkında bilgi edinin. WebHCat, Azure PowerShell ve Visual Studio için Veri Gölü Araçları gibi istemci tarafındaki araçlar tarafından dahili olarak kullanılır.

## <a name="what-is-webhcat"></a>WebHCat Nedir

[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) [HCatalog](https://cwiki.apache.org/confluence/display/Hive/HCatalog)için bir REST API, bir tablo ve Apache Hadoop için depolama yönetimi katmanıdır. WebHCat varsayılan olarak HDInsight kümelerinde etkinleştirilir ve kümeye giriş yapmadan iş göndermek, iş durumu almak ve benzeri çeşitli araçlar tarafından kullanılır.

## <a name="modifying-configuration"></a>Yapılandırmayı değiştirme

Yapılandırılan bir maksimum aşıldığından, bu belgede listelenen hatalardan bazıları oluşur. Çözünürlük adımında bir değeri değiştirebileceğinizden bahsettiğinde, değeri değiştirmek için Apache Ambari'yi (web veya REST API) kullanın. Daha fazla bilgi için [Bkz. Apache Ambari kullanarak HDInsight'ı Yönet](hdinsight-hadoop-manage-ambari.md)

### <a name="default-configuration"></a>Varsayılan yapılandırma

Aşağıdaki varsayılan değerler aşılırsa, WebHCat performansını düşürebilir veya hatalara neden olabilir:

| Ayar | Ne yapar? | Varsayılan değer |
| --- | --- | --- |
| [iplik.scheduler.capacity.maksimum uygulamalar][maximum-applications] |Aynı anda etkin olabilecek maksimum iş sayısı (beklemede veya çalışıyor) |10,000 |
| [templeton.exec.max-procs][max-procs] |Aynı anda sunulabilecek en fazla istek sayısı |20 |
| [mapreduce.jobhistory.max-age-ms][max-age-ms] |İş geçmişinin tutulduğu gün sayısı |7 gün |

## <a name="too-many-requests"></a>Çok fazla istek

**HTTP Durum kodu**: 429

| Nedeni | Çözüm |
| --- | --- |
| WebHCat tarafından dakikada sunulan maksimum eşzamanlı istekleri aştınız (varsayılan 20) |En fazla eşzamanlı istek sayısından fazla göndermediğinizden emin olmak için iş yükünüzü azaltın `templeton.exec.max-procs`veya değiştirerek eşzamanlı istek sınırını artırın. Daha fazla bilgi için [bkz.](#modifying-configuration) |

## <a name="server-unavailable"></a>Sunucu kullanılamıyor

**HTTP Durum kodu**: 503

| Nedeni | Çözüm |
| --- | --- |
| Bu durum kodu genellikle küme için birincil ve ikincil HeadNode arasında failover sırasında oluşur |İki dakika bekleyin ve işlemi yeniden deneyin |

## <a name="bad-request-content-could-not-find-job"></a>Kötü istek İçerik: İş bulamadım

**HTTP Durum kodu**: 400

| Nedeni | Çözüm |
| --- | --- |
| İş detayları iş geçmişi temizleyicisi tarafından temizlendi |İş geçmişi için varsayılan bekletme süresi 7 gündür. Varsayılan bekletme süresi değiştirilerek `mapreduce.jobhistory.max-age-ms`değiştirilebilir. Daha fazla bilgi için [bkz.](#modifying-configuration) |
| Eyüp bir başarısızlık yüzünden öldürüldü. |İki dakikaya kadar iş göndermeyi yeniden deneyin |
| Geçersiz iş kimliği kullanıldı |İş kimliğinin doğru olup olmadığını denetleme |

## <a name="bad-gateway"></a>Kötü ağ geçidi

**HTTP Durum kodu**: 502

| Nedeni | Çözüm |
| --- | --- |
| İç çöp toplama WebHCat işlemi içinde meydana geliyor |WebHCat hizmetini bitirmek veya yeniden başlatmak için çöp toplamayı bekleyin |
| Zaman dolan kaynak yöneticisi hizmetinden bir yanıt bekliyor. Etkin uygulama sayısı yapılandırılan maksimum (varsayılan 10.000) gittiğinde bu hata oluşabilir |Şu anda çalışan işlerin, değiştirerek eşzamanlı iş `yarn.scheduler.capacity.maximum-applications`sınırını tamamlamasını veya artırmasını bekleyin. Daha fazla bilgi için [Yapılandırmayı Değiştirme](#modifying-configuration) bölümüne bakın. |
| Get [/jobs](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference+Jobs) çağrısı `Fields` ile tüm işleri almaya çalışmak`*` |*Tüm* iş bilgilerini almayın. Bunun `jobid` yerine yalnızca belirli iş kimliğinden daha büyük işler için ayrıntıları almak için kullanın. Veya, kullanmayın`Fields` |
| WebHCat hizmeti HeadNode failover sırasında aşağı |İki dakika bekleyin ve işlemi yeniden deneyin |
| WebHCat üzerinden gönderilen 500'den fazla bekleyen iş vardır |Daha fazla iş göndermeden önce bekleyen işler tamamlanana kadar bekleyin |

[maximum-applications]: https://docs.cloudera.com/HDPDocuments/HDP2/HDP-2.1.3/bk_system-admin-guide/content/setting_application_limits.html
[max-procs]: https://cwiki.apache.org/confluence/display/Hive/WebHCat+Configure#WebHCatConfigure-WebHCatConfiguration
[max-age-ms]: https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.6.0/ds_Hadoop/hadoop-mapreduce-client/hadoop-mapreduce-client-core/mapred-default.xml
