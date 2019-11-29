---
title: HDInsight 'ta WebHCat hatalarını anlama ve çözme-Azure
description: HDInsight 'ta WebHCat tarafından döndürülen yaygın hataları ve bunların nasıl çözümleneceğini öğrenin.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: hrasheed
ms.openlocfilehash: 5c103482771b829730d009d65283a54ec1d8eb8a
ms.sourcegitcommit: 428fded8754fa58f20908487a81e2f278f75b5d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2019
ms.locfileid: "74555007"
---
# <a name="understand-and-resolve-errors-received-from-webhcat-on-hdinsight"></a>HDInsight üzerinde WebHCat 'den alınan hataları anlama ve çözme

HDInsight ile WebHCat kullanırken alınan hatalar ve bunları çözme hakkında bilgi edinin. WebHCat, Azure PowerShell gibi istemci tarafı araçları ve Visual Studio için Data Lake araçları tarafından dahili olarak kullanılır.

## <a name="what-is-webhcat"></a>WebHCat nedir?

[Webhcat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) , Apache Hadoop Için bir [hcatalog](https://cwiki.apache.org/confluence/display/Hive/HCatalog), tablo ve depolama yönetimi katmanı REST API. WebHCat, HDInsight kümelerinde varsayılan olarak etkindir ve kümede oturum açmadan işleri göndermek, iş durumunu almak vb. için çeşitli araçlar tarafından kullanılır.

## <a name="modifying-configuration"></a>Yapılandırmayı değiştirme

> [!IMPORTANT]  
> Yapılandırılan en büyük değer aşıldığından bu belgede listelenen hataların birkaçı meydana gelir. Çözüm adımı bir değeri değiştirebilmeniz durumunda, değişikliği gerçekleştirmek için aşağıdakilerden birini kullanmanız gerekir:

* **Windows** kümeleri için: küme oluşturma sırasında değeri yapılandırmak için bir betik eylemi kullanın. Daha fazla bilgi için bkz. [betik eylemleri geliştirme](hdinsight-hadoop-script-actions-linux.md).

* **Linux** kümeleri için: değeri değiştirmek Için Apache ambarı (web veya REST API) kullanın. Daha fazla bilgi için bkz. [Apache ambarı kullanarak HDInsight 'ı yönetme](hdinsight-hadoop-manage-ambari.md)


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
| Web Hcat tarafından dakikada sunulan en fazla eşzamanlı istek sayısını aştınız (varsayılan 20) |En fazla eşzamanlı istek sayısından daha fazla bilgi göndermemesini sağlamak için iş yükünüzü azaltın veya `templeton.exec.max-procs`değiştirerek eşzamanlı istek sınırını artırın. Daha fazla bilgi için bkz. [yapılandırmayı değiştirme](#modifying-configuration) |

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
