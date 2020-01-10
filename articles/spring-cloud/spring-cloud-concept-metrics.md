---
title: Azure yay bulutu için ölçümleri anlama
description: Azure Spring Cloud 'da ölçümleri incelemeyi öğrenin
author: jpconnock
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 12/06/2019
ms.author: jeconnoc
ms.openlocfilehash: 682cc8383db5704b28b9f4d6b8c22c4bbfc3f1ab
ms.sourcegitcommit: ce4a99b493f8cf2d2fd4e29d9ba92f5f942a754c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/28/2019
ms.locfileid: "75528207"
---
# <a name="understand-metrics-for-azure-spring-cloud"></a>Azure yay bulutu için ölçümleri anlama

Azure Ölçüm Gezgini, grafiklerin çizdirme, eğilimleri görsel olarak ilişkilendirme ve ölçümler ve DIB 'leri araştırma olanağı sağlayan Microsoft Azure portal bir bileşenidir. Kaynaklarınızın sistem durumunu ve kullanımını araştırmak için ölçüm gezginini kullanın. 

Azure Spring bulutta, ölçümler için iki görünüm noktası bulunur.
* Her uygulamaya Genel Bakış sayfasındaki grafikler
* Ortak ölçümler sayfası

 ![Ölçüm grafikleri](media/metrics/metrics-1.png)

Uygulamaya **genel bakış** bölümünde bulunan grafikler, her uygulama için hızlı durum denetimleri sağlar. Ortak **ölçümler** sayfası, başvuru için kullanılabilen tüm ölçümleri içerir. Ortak ölçümler sayfasında kendi grafiklerinizi oluşturabilir ve bunları panoda sabitleyebilirsiniz.

## <a name="application-overview-page"></a>Uygulamaya genel bakış sayfası
Genel Bakış sayfasında grafikler bulmak için **uygulama yönetimi** 'nde bir uygulama seçin.  

 ![Uygulama ölçümleri yönetimi](media/metrics/metrics-2.png)

Her uygulamanın **genel bakış** sayfasında, uygulamanızda hızlı bir durum denetimi gerçekleştirmenize olanak tanıyan bir ölçüm grafiği sunulmaktadır.  

 ![Uygulama ölçümlerine genel bakış](media/metrics/metrics-3.png)

Azure Spring Cloud, her dakikada güncelleştirilmiş ölçümlerle bu beş grafik sağlar:

* **Http sunucusu hataları**: UYGULAMANıZA yönelik http istekleri için hata sayısı
* **Içindeki veriler**: uygulamanız tarafından alınan bayt
* **Giden veri**: uygulamanız tarafından gönderilen baytlar
* **İstekler**: uygulamanız tarafından alınan istekler
* **Ortalama yanıt süresi**: uygulamanızdan alınan ortalama yanıt süresi

Grafik için bir saatten ve yedi güne kadar bir zaman aralığı seçebilirsiniz.

## <a name="common-metrics-page"></a>Ortak ölçümler sayfası

Sol gezinti bölmesindeki **ölçümler** ortak ölçümler sayfasına bağlanır.

İlk olarak, görüntülenecek ölçümleri seçin:

![Ölçüm görünümü seçin](media/metrics/metrics-4.png)

Ardından, her ölçüm için toplama türünü seçin:

![Ölçüm toplama](media/metrics/metrics-5.png)

Toplama türü nasıl toplanmış olduğunu gösterir. Her dakikada bir ölçüm noktası vardır.
* Toplam: tüm ölçümleri hedef çıktı olarak toplayın.
* Average: dönemdeki ortalama değeri hedef çıktı olarak kullanın.
* Max/Min: dönemde hedef çıktı olarak en fazla/en fazla değeri kullanın.

Gösterilecek zaman aralığı da değiştirilebilir.  Zaman aralığı son 30 dakikadan son 30 güne veya özel bir zaman aralığına seçilebilir.

![Ölçüm değişikliği](media/metrics/metrics-6.png)

Varsayılan görünüm, bir Azure yay bulut hizmeti 'nin applicationın ölçümlerini birlikte içerir. Bir uygulama veya örneğin ölçümleri, ekranda filtrelenebilir.  **Filtre Ekle**' ye tıklayın, özelliği **uygulama**olarak ayarlayın ve **değerler** metin kutusunda izlemek istediğiniz hedef uygulamayı seçin. 

İki tür filtre (Özellikler) kullanabilirsiniz:
* Uygulama: uygulama adına göre filtrele
* Örnek: uygulama örneğine göre filtrele

![Ölçüm filtreleri](media/metrics/metrics-7.png)

Ayrıca, bir uygulama için birden çok satır çizecek **bölme Uygula** seçeneğini de kullanabilirsiniz:

![Ölçüm bölme](media/metrics/metrics-8.png)

>[!TIP]
> Ölçümler sayfasında kendi grafiklerinizi oluşturabilir ve bunları **panonuza**sabitleyebilirsiniz. Grafiğinizi adlandırarak başlayın.  Sonra **sağ üst köşedeki panoya sabitle**' yi seçin. Artık uygulamanızı Portal **panonuzda**denetleyebilirsiniz.

## <a name="user-portal-metrics-options"></a>Kullanıcı Portalı ölçüm seçenekleri

Aşağıdaki tabloda kullanılabilir ölçümler ve Ayrıntılar gösterilmektedir.
>[!div class="mx-tdBreakAll"]
>| Ad | Görünen Ad | Spring çalıştırıcı ölçüm adı | Birim | Ayrıntılar |
>|----|----|----|----|------------|
>| SystemCpuUsagePercentage | Sistem CPU kullanım yüzdesi | System. CPU. Usage | Yüzde | Tüm sistem için en son CPU kullanımı. Bu değer [0.0, 1.0] aralığındaki bir Double değeridir. 0,0 değeri, izlenen son süre boyunca tüm CPU 'ların boşta kaldığı, 1,0 değeri, tüm CPU 'ların, en son dönemde geçen sürenin %100 ' i etkin bir şekilde çalıştırdığı anlamına gelir. 0,0 ve 1,0 arasındaki tüm değerler, sistemde yer alan etkinliklere bağlı olarak mümkündür. Sistemin en son CPU kullanımı kullanılabilir değilse, yöntem negatif bir değer döndürür. |
>| AppCpuUsagePercentage | Uygulama CPU kullanım yüzdesi | Uygulama CPU kullanım yüzdesi | Yüzde | Java Sanal Makinesi işlemi için en son CPU kullanımı. Bu değer [0.0, 1.0] aralığındaki bir Double değeridir. 0,0 değeri, en son geçen süre boyunca JVM işlemindeki iş parçacıklarını hiçbir CPU 'nun çalıştırmadığı anlamına gelir, 1,0 değeri, tüm CPU 'ların, en son dönemde geçen sürenin% JVM 100% ' den etkin bir şekilde çalıştığı anlamına gelir. JVM 'deki iş parçacıkları, uygulama iş parçacıklarını ve JVM iç iş parçacıklarını içerir. 0,0 ve 1,0 arasındaki tüm değerler, JVM işlemindeki etkinliklere ve sistemin tamamına bağlı olarak mümkündür. Java Sanal Makinesi son CPU kullanımı kullanılamıyorsa, yöntem negatif bir değer döndürür. |
>| AppMemoryCommitted | Atanan uygulama belleği | JVM. Memory. taahhüt | Bayt | Java sanal makinesi tarafından kullanılabilecek garanti edilen bellek miktarını (bayt cinsinden) temsil eder. Kaydedilmiş bellek miktarı zaman içinde değişebilir (artış veya azaltma). Java sanal makinesi belleği sisteme bırakabilir ve yürütme init 'den az olabilir. taahhüt edilen her zaman en fazla veya daha büyük olur. |
>| Appmemorykullanıldı | Kullanılan uygulama belleği | JVM. Memory. kullanıldı | Bayt | Bayt cinsinden şu anda kullanılan bellek miktarını temsil eder. |
>| AppMemoryMax | En fazla uygulama belleği | JVM. Memory. Max | Bayt | Bellek yönetimi için kullanılabilen en fazla bellek miktarını (bayt cinsinden) temsil eder. Değeri tanımsız olabilir. Tanımlanmışsa, en yüksek bellek miktarı zaman içinde değişebilir. En büyük değer tanımlanırsa, kullanılan ve kaydedilmiş bellek miktarı her zaman en fazla eşit veya daha düşük olacaktır. Kullanılan > kullanılan belleği artırmayı denerse bir bellek ayırma başarısız olabilir. < = Max kullanılması hala true (örneğin, sistem sanal belleği düşük olduğunda). |
>| MaxOldGenMemoryPoolBytes | En fazla kullanılabilir eski nesil veri boyutu | JVM. GC. Max. Data. size | Bayt | Java sanal makinesi başlatıldığından bu yana eski nesil bellek havuzunun en yoğun bellek kullanımı. |
>| OldGenMemoryPoolBytes | Eski nesil veri boyutu | JVM. GC. Live. Data. size | Bayt | Tam GC sonrasında eski nesil bellek havuzunun boyutu. |
>| OldGenPromotedBytes | Eski nesil veri boyutuna yükselt | JVM. GC. Memory. yükseltilen | Bayt | GC 'den sonra GC öncesinde, eski nesil bellek havuzunun boyutundaki pozitif artış sayısı. |
>| YoungGenPromotedBytes | Küçük ölçekli oluşturma veri boyutuna yükselt | JVM. GC. Memory. ayrılmış | Bayt | Bir GC 'nin bir sonraki öncesinde bir GC sonrasında küçük kuşak bellek havuzunun boyutunun artması için artırılır. |
>| GCPauseTotalCount | GC duraklatma sayısı | JVM. GC. Pause (Toplam-sayı) | Sayı | Bu JMV başlatıldıktan sonra, Başak ve eski GC dahil olmak üzere toplam GC sayısı. |
>| GCPauseTotalTime | GC duraklatma toplam süre | JVM. GC. Pause (Toplam süre) | Milisaniye | Bu JMV başlatıldıktan sonra, Başak ve eski GC dahil olmak üzere tüketilen toplam GC süresi. |
>| TomcatSentBytes | Tomcat gönderilen toplam bayt sayısı | Tomcat. Global. gönderildi | Bayt | Bayt cinsinden gönderilen veri Tomcat Web sunucusu miktarı |
>| Tomcatreceived baytları | Tomcat alınan toplam bayt sayısı | Tomcat. Global. alındı | Bayt | Bayt cinsinden alınan veri Tomcat Web sunucusu miktarı |
>| TomcatRequestTotalTime | Tomcat Isteği toplam süre | Tomcat. Global. Request (Toplam süre) | Milisaniye | İstekleri işlemek için Tomcat Web sunucusunun toplam süresi. |
>| TomcatRequestTotalCount | Tomcat Isteği toplam sayısı | Tomcat. Global. Request (Toplam sayı) | Sayı | Toplam Tomcat Web sunucusu işlenen istek sayısı |
>| TomcatRequestMaxTime | Tomcat Isteği maksimum zamanı | Tomcat. Global. Request. Max | Milisaniye | Bir isteği işlemek için en fazla Tomcat Web sunucusu süresi |
>| TomcatErrorCount | Tomcat genel hatası | Tomcat. Global. Error | Sayı | İşlenen isteklerden oluşan hata sayısı |
>| TomcatSessionActiveMaxCount | Tomcat oturumu en fazla etkin sayısı | Tomcat. Sessions. Active. Max | Sayı | Aynı anda etkin olan en fazla oturum sayısı |
>| TomcatSessionAliveMaxTime | Tomcat oturumu maksimum etkin süresi | Tomcat. Sessions. canlı. Max | Milisaniye | Süresi biten bir oturumun etkin olduğu en uzun süre (saniye cinsinden) |
>| TomcatSessionCreatedCount | Tomcat oturum oluşturma sayısı | Tomcat. Sessions. oluşturuldu | Sayı | Oluşturulan oturum sayısı |
>| TomcatSessionExpiredCount | Tomcat oturumunun süre dolma sayısı | Tomcat. Sessions. süre geçildi | Sayı | Zaman aşımına uğramış oturum sayısı |
>| TomcatSessionRejectedCount | Tomcat oturumu reddedildi sayısı | Tomcat. Sessions. reddedildi | Sayı | En fazla etkin oturum sayısına ulaşıldığından, oluşturulmayan oturum sayısı. |

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Ölçüm Gezgini'ni kullanmaya başlama](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-getting-started)

* [Tanılama ayarlarıyla günlükleri ve ölçümleri çözümleme](https://docs.microsoft.com/azure/spring-cloud/diagnostic-services)

## <a name="next-steps"></a>Sonraki adımlar
* [Öğretici: uyarıları ve eylem gruplarını kullanarak yay bulut kaynaklarını Izleme](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-tutorial-alerts-action-groups)

* [Azure yay bulutu için kotalar ve hizmet planları](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-quotas)

