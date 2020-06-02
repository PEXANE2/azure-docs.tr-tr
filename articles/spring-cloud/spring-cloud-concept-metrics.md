---
title: Azure yay bulutu için ölçümleri anlama
description: Azure Spring Cloud 'da ölçümleri incelemeyi öğrenin
author: bmitchell287
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 12/06/2019
ms.author: brendm
ms.openlocfilehash: f3283e70ec3e0b559b417285c4b154494a88980e
ms.sourcegitcommit: 8017209cc9d8a825cc404df852c8dc02f74d584b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2020
ms.locfileid: "84248459"
---
# <a name="understand-metrics-for-azure-spring-cloud"></a>Azure yay bulutu için ölçümleri anlama

Azure Ölçüm Gezgini, grafiklerin çizdirme, eğilimleri görsel olarak ilişkilendirme ve ölçümler ve DIB 'leri araştırma olanağı sağlayan Microsoft Azure portal bir bileşenidir. Kaynaklarınızın sistem durumunu ve kullanımını araştırmak için ölçüm gezginini kullanın. 

Azure Spring bulutta, ölçümler için iki görünüm noktası bulunur.
* Her uygulamaya Genel Bakış sayfasındaki grafikler
* Ortak ölçümler sayfası

 ![Ölçüm grafikleri](media/metrics/metrics-1.png)

Uygulamaya **genel bakış** bölümünde bulunan grafikler, her uygulama için hızlı durum denetimleri sağlar. Ortak **ölçümler** sayfası, başvuru için kullanılabilen tüm ölçümleri içerir. Ortak ölçümler sayfasında kendi grafiklerinizi oluşturabilir ve bunları panoda sabitleyebilirsiniz.

## <a name="application-overview-page"></a>Uygulamaya genel bakış sayfası
Genel Bakış sayfasında grafikleri bulmak için **uygulamalarda** bir uygulama seçin.  

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

Tüm ölçümler seçeneğinin ayrıntıları aşağıdaki [bölümde](#user-metrics-options) bulunabilir.

Ardından, her ölçüm için toplama türünü seçin:

![Ölçüm toplama](media/metrics/metrics-5.png)

Toplama türü, grafikteki ölçüm noktalarının zamana göre nasıl toplanacağını gösterir. Her dakikada bir ham ölçüm noktası vardır ve bir dakika içinde ön toplama türü, ölçüm türü tarafından önceden tanımlanmıştır.
* Sum: tüm değerleri hedef çıktı olarak toplayın.
* Average: dönemdeki ortalama değeri hedef çıktı olarak kullanın.
* Max/Min: dönemde hedef çıktı olarak en fazla/en fazla değeri kullanın.

Gösterilecek zaman aralığı da değiştirilebilir. Zaman aralığı son 30 dakikadan son 30 güne veya özel bir zaman aralığına seçilebilir.

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

## <a name="user-metrics-options"></a>Kullanıcı ölçümleri seçenekleri

Aşağıdaki tablolarda kullanılabilir ölçümler ve Ayrıntılar gösterilmektedir.

### <a name="error"></a>Hata
>[!div class="mx-tdCol2BreakAll"]
>| Name | Spring çalıştırıcı ölçüm adı | Birim | Ayrıntılar |
>|----|----|----|------------|
>| TomcatErrorCount<br><br>Tomcat genel hatası (kullanım dışı) (1 Temmuz 2020 tarihinde kaldırıldı) | Tomcat. Global. Error | Sayı | İşlenen isteklerden oluşan hata sayısı |
>| Tomcat. Global. Error | Tomcat. Global. Error | Sayı | İşlenen isteklerden oluşan hata sayısı |

### <a name="performance"></a>Performans
>[!div class="mx-tdCol2BreakAll"]
>| Name | Spring çalıştırıcı ölçüm adı | Birim | Ayrıntılar |
>|----|----|----|------------|
>| SystemCpuUsagePercentage<br><br>Sistem CPU kullanım yüzdesi (kullanım dışı) (1 Temmuz 2020 tarihinde kaldırıldı) | System. CPU. Usage | Yüzde | Tüm sistem için en son CPU kullanımı. Bu değer [0.0, 1.0] aralığındaki bir Double değeridir. 0,0 değeri, izlenen son süre boyunca tüm CPU 'ların boşta kaldığı, 1,0 değeri, tüm CPU 'ların, en son dönemde geçen sürenin %100 ' i etkin bir şekilde çalıştırdığı anlamına gelir.|
>| System. CPU. Usage | System. CPU. Usage | Yüzde | Tüm sistem için en son CPU kullanımı. Bu değer [0.0, 1.0] aralığındaki bir Double değeridir. 0,0 değeri, izlenen son süre boyunca tüm CPU 'ların boşta kaldığı, 1,0 değeri, tüm CPU 'ların, en son dönemde geçen sürenin %100 ' i etkin bir şekilde çalıştırdığı anlamına gelir.|
>| AppCpuUsagePercentage<br><br>Uygulama CPU kullanım yüzdesi (kullanım dışı) (1 Temmuz 2020 tarihinde kaldırıldı) | Uygulama CPU kullanım yüzdesi | Yüzde | Java Sanal Makinesi işlemi için en son CPU kullanımı. Bu değer [0.0, 1.0] aralığındaki bir Double değeridir. 0,0 değeri, en son geçen süre boyunca JVM işlemindeki iş parçacıklarını hiçbir CPU 'nun çalıştırmadığı anlamına gelir, 1,0 değeri, tüm CPU 'ların, en son dönemde geçen sürenin% JVM 100% ' den etkin bir şekilde çalıştığı anlamına gelir. JVM 'deki iş parçacıkları, uygulama iş parçacıklarını ve JVM iç iş parçacıklarını içerir.|
>| Process. CPU. Usage | Uygulama CPU kullanım yüzdesi | Yüzde | Java Sanal Makinesi işlemi için en son CPU kullanımı. Bu değer [0.0, 1.0] aralığındaki bir Double değeridir. 0,0 değeri, en son geçen süre boyunca JVM işlemindeki iş parçacıklarını hiçbir CPU 'nun çalıştırmadığı anlamına gelir, 1,0 değeri, tüm CPU 'ların, en son dönemde geçen sürenin% JVM 100% ' den etkin bir şekilde çalıştığı anlamına gelir. JVM 'deki iş parçacıkları, uygulama iş parçacıklarını ve JVM iç iş parçacıklarını içerir.|
>| AppMemoryCommitted<br><br>Uygulama belleği atandı (kullanım dışı)) (1 Temmuz 2020 tarihinde kaldırılır) | JVM. Memory. taahhüt | Bayt | JVM tarafından kullanılabilecek garanti edilen bellek miktarını temsil eder. JVM, belleği sisteme bırakabilir ve yürütme init 'ten az olabilir. taahhüt edilen her zaman en fazla veya daha büyük olur. |
>| JVM. Memory. taahhüt | JVM. Memory. taahhüt | Bayt | JVM tarafından kullanılabilecek garanti edilen bellek miktarını temsil eder. JVM, belleği sisteme bırakabilir ve yürütme init 'ten az olabilir. taahhüt edilen her zaman en fazla veya daha büyük olur. |
>| Appmemorykullanıldı <br><br>Kullanılan uygulama belleği (kullanım dışı) (1 Temmuz 2020 tarihinde kaldırıldı) | JVM. Memory. kullanıldı | Bayt | Bayt cinsinden şu anda kullanılan bellek miktarını temsil eder. |
>| JVM. Memory. kullanıldı | JVM. Memory. kullanıldı | Bayt | Bayt cinsinden şu anda kullanılan bellek miktarını temsil eder. |
>| AppMemoryMax<br><br>En fazla uygulama belleği (kullanım dışı) (1 Temmuz 2020 tarihinde kaldırılır) | JVM. Memory. Max | Bayt | Bellek yönetimi için kullanılabilecek maksimum bellek miktarını temsil eder. En büyük değer tanımlanırsa, kullanılan ve kaydedilmiş bellek miktarı her zaman en fazla eşit veya daha düşük olacaktır. Kullanılan > kullanılan belleği artırmayı denerse bir bellek ayırma başarısız olabilir. <= Max kullanılması hala true (örneğin, sistem sanal belleği düşük olduğunda). |
>| JVM. Memory. Max | JVM. Memory. Max | Bayt | Bellek yönetimi için kullanılabilecek maksimum bellek miktarını temsil eder. En büyük değer tanımlanırsa, kullanılan ve kaydedilmiş bellek miktarı her zaman en fazla eşit veya daha düşük olacaktır. Kullanılan > kullanılan belleği artırmayı denerse bir bellek ayırma başarısız olabilir. <= Max kullanılması hala true (örneğin, sistem sanal belleği düşük olduğunda). |
>| MaxOldGenMemoryPoolBytes<br><br>En fazla kullanılabilir eski nesil veri boyutu (kullanım dışı) (1 Temmuz 2020 tarihinde kaldırıldı) | JVM. GC. Max. Data. size | Bayt | Java sanal makinesi başlatıldığından bu yana eski nesil bellek havuzunun en yoğun bellek kullanımı. |
>| JVM. GC. Max. Data. size | JVM. GC. Max. Data. size | Bayt | Java sanal makinesi başlatıldığından bu yana eski nesil bellek havuzunun en yoğun bellek kullanımı. |
>| OldGenMemoryPoolBytes<br><br>Eski nesil veri boyutu (kullanım dışı) (1 Temmuz 2020 tarihinde kaldırıldı) | JVM. GC. Live. Data. size | Bayt | Tam GC sonrasında eski nesil bellek havuzunun boyutu. |
>| JVM. GC. Live. Data. size | JVM. GC. Live. Data. size | Bayt | Tam GC sonrasında eski nesil bellek havuzunun boyutu. |
>| OldGenPromotedBytes<br><br>Eski nesil veri boyutuna (kullanım dışı) yükselt (1 Temmuz 2020 tarihinde kaldırılır) | JVM. GC. Memory. yükseltilen | Bayt | GC 'den sonra GC öncesinde, eski nesil bellek havuzunun boyutundaki pozitif artış sayısı. |
>| JVM. GC. Memory. yükseltilen | JVM. GC. Memory. yükseltilen | Bayt | GC 'den sonra GC öncesinde, eski nesil bellek havuzunun boyutundaki pozitif artış sayısı. |
>| YoungGenPromotedBytes<br><br>Küçük ölçekli oluşturma veri boyutuna (kullanım dışı) yükselt (1 Temmuz 2020 tarihinde kaldırılır) | JVM. GC. Memory. ayrılmış | Bayt | Bir GC 'nin bir sonraki öncesinde bir GC sonrasında küçük kuşak bellek havuzunun boyutunun artması için artırılır. |
>| JVM. GC. Memory. ayrılmış | JVM. GC. Memory. ayrılmış | Bayt | Bir GC 'nin bir sonraki öncesinde bir GC sonrasında küçük kuşak bellek havuzunun boyutunun artması için artırılır. |
>| GCPauseTotalCount<br><br>GC duraklatma sayısı (kullanım dışı) (1 Temmuz 2020 tarihinde kaldırıldı) | JVM. GC. Pause (Toplam-sayı) | Sayı | Bu JMV başlatıldıktan sonra, Başak ve eski GC dahil olmak üzere toplam GC sayısı. |
>| JVM. GC. Pause. Total. Count | JVM. GC. Pause (Toplam-sayı) | Sayı | Bu JMV başlatıldıktan sonra, Başak ve eski GC dahil olmak üzere toplam GC sayısı. |
>| GCPauseTotalTime<br><br>GC duraklatma toplam süresi (kullanım dışı) (1 Temmuz 2020 tarihinde kaldırıldı) | JVM. GC. Pause (Toplam süre) | Mayacak | Bu JMV başlatıldıktan sonra, Başak ve eski GC dahil olmak üzere tüketilen toplam GC süresi. |
>| JVM. GC. Pause. Total. Time | JVM. GC. Pause (Toplam süre) | Mayacak | Bu JMV başlatıldıktan sonra, Başak ve eski GC dahil olmak üzere tüketilen toplam GC süresi. |
>| Tomcat. Threads. config. Max | Tomcat. Threads. config. Max | Sayı | Tomcat yapılandırması en fazla Iş parçacığı sayısı |
>| Tomcat. Threads. Current | Tomcat. Threads. Current | Sayı | Tomcat geçerli Iş parçacığı sayısı |
>| Tomcat. Global. Request. ort. saat | Tomcat. Global. Request. ort. saat | Mayacak | Tomcat Isteği ortalama zamanı |


### <a name="request"></a>İstek
>[!div class="mx-tdCol2BreakAll"]
>| Name | Spring çalıştırıcı ölçüm adı | Birim | Ayrıntılar |
>|----|----|----|------------|
>| TomcatSentBytes<br><br>Tomcat gönderilen toplam bayt (kullanım dışı) (1 Temmuz 2020 tarihinde kaldırıldı) | Tomcat. Global. gönderildi | Bayt | Gönderilen veri miktarı Tomcat Web sunucusu |
>| Tomcat. Global. gönderildi | Tomcat. Global. gönderildi | Bayt | Gönderilen veri miktarı Tomcat Web sunucusu |
>| Tomcatreceived baytları<br><br>Tomcat Toplam alınan bayt (kullanım dışı) (1 Temmuz 2020 tarihinde kaldırıldı) | Tomcat. Global. alındı | Bayt | Alınan veri miktarı Tomcat Web sunucusu |
>| Tomcat. Global. alındı | Tomcat. Global. alındı | Bayt | Alınan veri miktarı Tomcat Web sunucusu |
>| TomcatRequestTotalTime<br><br>Tomcat Isteği toplam süre (kullanım dışı) (1 Temmuz 2020 tarihinde kaldırıldı) | Tomcat. Global. Request (Toplam süre) | Mayacak | İstekleri işlemek için Tomcat Web sunucusunun toplam süresi |
>| TomcatRequestTotalCount<br><br>Tomcat Isteği toplam sayısı (kullanım dışı) (1 Temmuz 2020 tarihinde kaldırıldı) | Tomcat. Global. Request (Toplam sayı) | Sayı | Toplam Tomcat Web sunucusu işlenen istek sayısı |
>| Tomcat. Global. Request. Total. Count | Tomcat. Global. Request (Toplam sayı) | Sayı | Toplam Tomcat Web sunucusu işlenen istek sayısı |
>| TomcatRequestMaxTime<br><br>Tomcat Isteği en uzun süresi (kullanım dışı) (1 Temmuz 2020 tarihinde kaldırıldı) | Tomcat. Global. Request. Max | Mayacak | Bir isteği işlemek için en fazla Tomcat Web sunucusu süresi |
>| Tomcat. Global. Request. Max | Tomcat. Global. Request. Max | Mayacak | Bir isteği işlemek için en fazla Tomcat Web sunucusu süresi |

### <a name="session"></a>Oturum
>[!div class="mx-tdCol2BreakAll"]
>| Name | Spring çalıştırıcı ölçüm adı | Birim | Ayrıntılar |
>|----|----|----|------------|
>| TomcatSessionActiveMaxCount<br><br>Tomcat oturumu en fazla etkin sayısı (kullanım dışı) (1 Temmuz 2020 tarihinde kaldırıldı) | Tomcat. Sessions. Active. Max | Sayı | Aynı anda etkin olan en fazla oturum sayısı |
>| Tomcat. Sessions. Active. Max | Tomcat. Sessions. Active. Max | Sayı | Aynı anda etkin olan en fazla oturum sayısı |
>| TomcatSessionAliveMaxTime<br><br>Tomcat oturumu en fazla etkin kalma süresi (kullanım dışı) (1 Temmuz 2020 tarihinde kaldırılır) | Tomcat. Sessions. canlı. Max | Mayacak | Süresi biten bir oturumun etkin olduğu en uzun süre (saniye cinsinden) |
>| Tomcat. Sessions. canlı. Max | Tomcat. Sessions. canlı. Max | Mayacak | Süresi biten bir oturumun etkin olduğu en uzun süre (saniye cinsinden) |
>| TomcatSessionCreatedCount<br><br>Tomcat oturumunun oluşturulma sayısı (kullanım dışı) (1 Temmuz 2020 tarihinde kaldırıldı) | Tomcat. Sessions. oluşturuldu | Sayı | Oluşturulan oturum sayısı |
>| Tomcat. Sessions. oluşturuldu | Tomcat. Sessions. oluşturuldu | Sayı | Oluşturulan oturum sayısı |
>| TomcatSessionExpiredCount<br><br>Tomcat oturumunun süre dolma sayısı (kullanım dışı) (1 Temmuz 2020 tarihinde kaldırıldı) | Tomcat. Sessions. süre geçildi | Sayı | Zaman aşımına uğramış oturum sayısı |
>| Tomcat. Sessions. süre geçildi | Tomcat. Sessions. süre geçildi | Sayı | Zaman aşımına uğramış oturum sayısı |
>| TomcatSessionRejectedCount<br><br>Tomcat oturumu reddedildi sayısı (kullanım dışı) (1 Temmuz 2020 tarihinde kaldırıldı) | Tomcat. Sessions. reddedildi | Sayı | En fazla etkin oturum sayısına ulaşıldığından, oluşturulmayan oturum sayısı. |
>| Tomcat. Sessions. reddedildi | Tomcat. Sessions. reddedildi | Sayı | En fazla etkin oturum sayısına ulaşıldığından, oluşturulmayan oturum sayısı. |
>| Tomcat. Sessions. ACTIVE. Current | Tomcat. Sessions. ACTIVE. Current | Sayı | Tomcat oturumu etkin sayısı |

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Ölçüm Gezgini'ni kullanmaya başlama](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-getting-started)

* [Tanılama ayarlarıyla günlükleri ve ölçümleri çözümleme](https://docs.microsoft.com/azure/spring-cloud/diagnostic-services)

## <a name="next-steps"></a>Sonraki adımlar
* [Öğretici: uyarıları ve eylem gruplarını kullanarak yay bulut kaynaklarını Izleme](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-tutorial-alerts-action-groups)

* [Azure yay bulutu için kotalar ve hizmet planları](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-quotas)

