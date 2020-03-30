---
title: Azure Bulutu için ölçümleri anlama
description: Azure Bulutu'nda ölçümleri nasıl inceleyiniz öğrenin
author: bmitchell287
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 12/06/2019
ms.author: brendm
ms.openlocfilehash: bb23afff2b4b449897d8e420934d038938d20205
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79256763"
---
# <a name="understand-metrics-for-azure-spring-cloud"></a>Azure Bulutu için ölçümleri anlama

Azure Ölçümleri gezgini, Microsoft Azure portalının grafikleri çizmeye, eğilimleri görsel olarak ilişkilendirmesine ve ölçümlerdeki ani artışları ve düşüşleri araştırmasına olanak tanıyan bir bileşenidir. Kaynaklarınızın durumunu ve kullanımını araştırmak için metrikler gezginini kullanın. 

Azure İlkbahar Bulutu'nda ölçümler için iki bakış noktası vardır.
* Her uygulamaya genel bakış sayfasındaki grafikler
* Ortak ölçümler sayfası

 ![Ölçüm Grafikleri](media/metrics/metrics-1.png)

Uygulamadaki **Grafiklergenel bakış** her uygulama için hızlı durum denetimleri sağlar. Ortak **Ölçümler** sayfası, başvuru için kullanılabilen tüm ölçümleri içerir. Ortak ölçümler sayfasında kendi grafiklerinizi oluşturabilir ve Pano'da sabitleyebilirsiniz.

## <a name="application-overview-page"></a>Uygulamaya genel bakış sayfası
Genel bakış sayfasında grafikleri bulmak için **Uygulama Yönetimi'nde** bir uygulama seçin.  

 ![Uygulama Ölçümleri Yönetimi](media/metrics/metrics-2.png)

Her uygulamanın **Uygulamaya Genel Bakış** sayfası, uygulamanızın hızlı durum denetimini gerçekleştirmenize olanak tanıyan bir ölçüm grafiği sunar.  

 ![Uygulama Ölçümlerine Genel Bakış](media/metrics/metrics-3.png)

Azure İlkbahar Bulutu, bu beş grafiği her dakika güncelleştirilen ölçümlerle sağlar:

* **Http Sunucu Hataları**: Uygulamanızdaki HTTP istekleri için hata sayısı
* **Data In**: Uygulamanız tarafından alınan baytlar
* **Veri Çıkış**: Uygulamanız tarafından gönderilen baytlar
* **İstekler**: Uygulamanız tarafından alınan istekler
* **Ortalama Yanıt Süresi**: Uygulamanızdan ortalama yanıt süresi

Grafik için, bir saat ile yedi gün arasında bir zaman aralığı seçebilirsiniz.

## <a name="common-metrics-page"></a>Ortak ölçümler sayfası

Sol gezinti bölmesindeki **Ölçümler,** ortak ölçümler sayfasına bağlanır.

İlk olarak, görüntülemek için ölçümleri seçin:

![Metrik Görünümü Seçin](media/metrics/metrics-4.png)

Tüm ölçümler seçeneğinin ayrıntılarını aşağıdaki [bölümde](#user-metrics-options) bulabilirsiniz.

Ardından, her metrik için toplama türünü seçin:

![Metrik Toplama](media/metrics/metrics-5.png)

Toplama türü, grafikteki metrik noktaların zamana göre nasıl toplanmayacağını gösterir. Her dakika bir ham metrik nokta vardır ve bir dakika içindeki ön toplama türü metrik türüne göre önceden tanımlanır.
* Toplam: Tüm değerleri hedef çıktı olarak özetle.
* Ortalama: Dönemdeki ortalama değeri hedef çıktı olarak kullanın.
* Max/Min: Periyottaki Max/Min değerini hedef çıktı olarak kullanın.

Gösterilecek zaman aralığı da değiştirilebilir. Zaman aralığı son 30 dakikadan son 30 güne veya özel bir zaman aralığına kadar seçilebilir.

![Metrik Modifikasyon](media/metrics/metrics-6.png)

Varsayılan görünüm, bir Azure İlkbahar Bulutu hizmetinin tüm ölçümlerini birlikte içerir. Bir uygulamanın veya örneğin ölçümleri ekranda filtrelenebilir.  **Filtre Ekle'yi**tıklatın, özelliği **Uygulamaya**ayarlayın ve **Değerler** metin kutusunda izlemek istediğiniz hedef uygulamayı seçin. 

İki tür filtre (özellik) kullanabilirsiniz:
* Uygulama: uygulama adına göre filtre
* Örnek: uygulama örneğine göre filtre

![Metrik Filtreler](media/metrics/metrics-7.png)

Bir uygulama için birden çok çizgi çizecek olan **Uygula bölme** seçeneğini de kullanabilirsiniz:

![Metrik Bölme](media/metrics/metrics-8.png)

>[!TIP]
> Ölçümler sayfasında kendi grafiklerinizi oluşturabilir ve **Panonuza**sabitleyebilirsiniz. Grafiğinizi adlandırarak başlayın.  Ardından, **sağ üst köşedeki panoya Pin'i**seçin. Artık Uygulamanızı Portal **Panonuzdan**kontrol edebilirsiniz.

## <a name="user-metrics-options"></a>Kullanıcı ölçümleri seçenekleri

Aşağıdaki tablolarda kullanılabilir ölçümler ve ayrıntılar gösterilebilir.

### <a name="error"></a>Hata
>[!div class="mx-tdCol2BreakAll"]
>| Adı | Bahar Aktüatör Metrik Adı | Birim | Ayrıntılar |
>|----|----|----|------------|
>| Tomcat Global Hatası | tomcat.global.error | Sayı | İşlenen isteklerin hata sayısı oluşur |

### <a name="performance"></a>Performans
>[!div class="mx-tdCol2BreakAll"]
>| Adı | Bahar Aktüatör Metrik Adı | Birim | Ayrıntılar |
>|----|----|----|------------|
>|Sistem CPU Kullanım Yüzdesi | system.cpu.usage | Yüzde | Tüm sistem için son CPU kullanımı. Bu değer [0.0,1.0] aralığında bir çifttir. 0,0 değeri, tüm CPU'ların gözlemlenen son dönemde boşta olduğu anlamına gelirken, 1,0 değeri tüm CPU'ların gözlemlenen son dönemde etkin olarak %100 oranında çalıştığını ifade ediyor.|
>| Uygulama CPU Kullanım Yüzdesi | Uygulama CPU Kullanım Yüzdesi | Yüzde | Java Sanal Makine işlemi için son CPU kullanımı. Bu değer [0.0,1.0] aralığında bir çifttir. 0,0 değeri, gözlemlenen son dönemde CPU'ların hiçbirinin JVM işleminden iş parçacığı çalışmadığı anlamına gelirken, 1,0 değeri tüm CPU'ların gözlemlenen son dönemde JVM'den %100'lük iş parçacığı çalıştırdığı anlamına gelir. JVM'den gelen iş parçacıkları, jvm dahili iş parçacıklarının yanı sıra uygulama iş parçacığı da içerir.|
>| Atanan Uygulama Belleği | jvm.memory.committed | Bayt | JVM tarafından kullanılabilir olması garanti edilen bellek miktarını temsil eder. JVM sisteme bellek bırakabilir ve taahhüt giriş daha az olabilir. taahhüt her zaman kullanılandan daha büyük veya eşit olacaktır. |
>| Kullanılan Uygulama Belleği | jvm.memory.used | Bayt | Şu anda baytlarda kullanılan bellek miktarını temsil eder. |
>| Uygulama Bellek Max | jvm.memory.max | Bayt | Bellek yönetimi için kullanılabilecek maksimum bellek miktarını temsil eder. Kullanılan ve işlenen bellek miktarı, max tanımlanırsa her zaman maksimumdan daha az veya max'e eşit olacaktır. Kullanılan > kullanılan > kullanılan bellek leri artırmaya çalışırsa bellek ayırma, kullanılan <= max hala doğru olacaktır (örneğin, sistem sanal bellekte düşükolduğunda) başarısız olabilir. |
>| Maksimum Kullanılabilir Eski Nesil Veri Boyutu | jvm.gc.max.data.size | Bayt | Java sanal makine başlatıldığından beri eski nesil bellek havuzunun en yüksek bellek kullanımı. |
>| Eski Nesil Veri Boyutu | jvm.gc.live.data.size | Bayt | Tam bir GC sonra eski nesil bellek havuzu boyutu. |
>| Eski Nesil Veri Boyutuna Tanıtın | jvm.gc.memory.promoted | Bayt | GC'den gc'den sonraeski nesil bellek havuzunun boyutundaki olumlu artışların sayısı. |
>| Genç Nesil Veri Boyutuna Tanıtın | jvm.gc.memory.allocated | Bayt | Bir GC'den diğerine bir GC'den sonra genç nesil bellek havuzunun büyüklüğünde bir artış için artırıldı. |
>| GC Duraklatma Sayısı | jvm.gc.pause (toplam sayı) | Sayı | Bu JMV başladıktan sonra toplam GC sayısı, Genç ve Eski GC dahil. |
>| GC Duraklama Toplam Süresi | jvm.gc.pause (toplam zaman) | Milisaniye | Bu JMV başladıktan sonra tüketilen toplam GC süresi, Young ve Old GC dahil. |

### <a name="request"></a>İstek
>[!div class="mx-tdCol2BreakAll"]
>| Adı | Bahar Aktüatör Metrik Adı | Birim | Ayrıntılar |
>|----|----|----|------------|
>| Tomcat Toplam Gönderilen Bayt | tomcat.global.sent | Bayt | Tomcat web sunucusunun gönderdiği veri miktarı |
>| Tomcat Toplam Alınan Baytlar | tomcat.global.alınan | Bayt | Tomcat web sunucusualınan veri miktarı |
>| Tomcat İstek Toplam Süresi | tomcat.global.request (toplam zaman) | Milisaniye | İstekleri işlemek için Tomcat web sunucusunun toplam süresi |
>| Tomcat İstek Toplam Sayısı | tomcat.global.request (toplam sayı) | Sayı | Tomcat web sunucusu işlenen isteklerin toplam sayısı |
>| Tomcat İstek Max Zaman | tomcat.global.request.max | Milisaniye | Bir isteği işlemek için Tomcat web sunucusunun maksimum süresi |

### <a name="session"></a>Oturum
>[!div class="mx-tdCol2BreakAll"]
>| Adı | Bahar Aktüatör Metrik Adı | Birim | Ayrıntılar |
>|----|----|----|------------|
>| Tomcat Oturum Max Etkin Sayısı | tomcat.sessions.active.max | Sayı | Aynı anda etkin olan maksimum oturum sayısı |
>| Tomcat Oturum Max Alive Time | tomcat.sessions.alive.max | Milisaniye | Süresi dolan bir oturumun canlı olduğu en uzun süre (saniye cinsinden) |
>| Tomcat Oturum Oluşturulan Sayı | tomcat.sessions.created | Sayı | Oluşturulan oturum sayısı |
>| Tomcat Oturumu Süresi Dolmuş Sayı | tomcat.sessions.expired | Sayı | Süresi dolmuş oturum sayısı |
>| Tomcat Oturumu Reddedilen Sayı | tomcat.sessions.reddedildi | Sayı | Maksimum etkin oturum sayısı na ulaşıldığından oluşturulmayan oturum sayısı. |

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Ölçüm Gezgini'ni kullanmaya başlama](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-getting-started)

* [Günlükleri ve ölçümleri tanılama ayarlarıyla analiz edin](https://docs.microsoft.com/azure/spring-cloud/diagnostic-services)

## <a name="next-steps"></a>Sonraki adımlar
* [Öğretici: Uyarıları ve eylem gruplarını kullanarak Bahar Bulutu kaynaklarını izleyin](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-tutorial-alerts-action-groups)

* [Azure Bahar Bulutu için Kotalar ve Hizmet Planları](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-quotas)

