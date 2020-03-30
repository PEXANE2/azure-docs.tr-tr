---
title: V1'i otomatik olarak ölçekleme
description: Otomatikleme ve Uygulama Servis Ortamı v1. Bu doküman yalnızca eski v1 ASE'yi kullanan müşteriler için sağlanır.
author: btardif
ms.assetid: c23af2d8-d370-4b1f-9b3e-8782321ddccb
ms.topic: article
ms.date: 07/11/2017
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 4f071c0d09fc2fa97eeea45bd82228b7eb8434a2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74687291"
---
# <a name="autoscaling-and-app-service-environment-v1"></a>Otomatikleme ve Uygulama Servis Ortamı v1

> [!NOTE]
> Bu makale, App Service Environment v1 hakkındadır.  Uygulama Hizmet Ortamı'nın kullanımı daha kolay olan ve daha güçlü altyapıda çalışan daha yeni bir sürümü vardır. Yeni sürüm hakkında daha fazla bilgi edinmek [için Uygulama Hizmet Ortamına Giriş](intro.md)ile başlayın.
> 

Azure Uygulama Hizmeti ortamları *otomatik küçültmeyi*destekler. Ölçümlere veya zamanlamaya göre tek tek çalışan havuzları otomatik ölçeklendirebilirsiniz.

![İşçi havuzu için otomatik ölçeklendirme seçenekleri.][intro]

Otomatik ölçeklendirme, bütçenize ve veya yük profilinize uyacak şekilde bir Uygulama Hizmeti ortamını otomatik olarak büyüterek ve küçülterek kaynak kullanımınızı optimize eder.

## <a name="configure-worker-pool-autoscale"></a>Yapılandırılan işçi havuzu otomatik ölçeklendirme
Otomatik ölçeklendirme işlevine çalışan havuzunun **Ayarlar** sekmesinden erişebilirsiniz.

![Alt havuzun ayarlar sekmesi.][settings-scale]

Buradan, bir Uygulama Hizmeti planını ölçeklendirdiğinizde gördüğünüz deneyimle aynı olduğundan, arabirim oldukça tanıdık olmalıdır. 

![El ile ölçek ayarları.][scale-manual]

Otomatik ölçek profilini de yapılandırabilirsiniz.

![Otomatik ölçeklendirme ayarları.][scale-profile]

Otomatik ölçek profilleri, ölçeğinizde sınırlar belirlemek için yararlıdır. Bu şekilde, daha düşük bir sınır ölçeği değeri (1) ve öngörülebilir bir harcama kapağı nı üst sınır (2) ayarlayarak tutarlı bir performans deneyimi yaşayabilirsiniz.

![Profildeki ayarları ölçeklendirin.][scale-profile2]

Bir profil tanımladıktan sonra, profil tarafından tanımlanan sınırlar içinde alt havuzdaki örnek sayısını ölçeklendirmek veya küçültmek için otomatik ölçeklendirme kuralları ekleyebilirsiniz. Otomatik ölçeklendirme kuralları ölçümlere dayanır.

![Ölçek kuralı.][scale-rule]

 Otomatik ölçeklendirme kurallarını tanımlamak için herhangi bir alt havuz veya ön uç ölçümleri kullanılabilir. Bu ölçümler, kaynak bıçak grafiklerinde izleyebileceğiniz veya uyarılar ayarlayabildiğiniz ölçümlerle aynıdır.

## <a name="autoscale-example"></a>Otomatik ölçekörneği
Bir Uygulama Hizmeti ortamının otomatik ölçeklendirmesi en iyi senaryoda yürüyerek gösterilebilir.

Bu makalede, otomatik ölçek ayarladiğinizde gerekli tüm hususlar açıklanmaktadır. Makale, App Service Environment'da barındırılan Uygulama Hizmeti ortamlarını otomatik olarak ölçeklediğinizde devreye giren etkileşimler arasında size yol sunar.

### <a name="scenario-introduction"></a>Senaryo girişi
Frank, yöneticilik yüklerinin bir kısmını Bir Uygulama Hizmeti ortamına geçiren bir kuruluş için bir sysadmin'dir.

Uygulama Hizmeti ortamı aşağıdaki gibi manuel ölçeklendirilir:

* **Ön uçlar:** 3
* **İşçi havuzu 1**: 10
* **İşçi havuzu 2**: 5
* **İşçi havuzu 3**: 5

İşçi havuzu 1 üretim iş yükleri için kullanılırken, işçi havuzu 2 ve işçi havuzu 3 kalite güvencesi (QA) ve geliştirme iş yükleri için kullanılır.

QA ve dev için Uygulama Hizmeti planları manuel ölçekle yapılandırılır. Üretim App Service planı yük ve trafik varyasyonları ile başa çıkmak için otomatik ölçeklendirme ayarlanır.

Frank başvuruyu çok iyi biliyor. Bu, çalışanların ofisteyken kullandıkları bir iş hattı (LOB) uygulaması olduğu için, yükleme için en yoğun saatlerin 09:00 ile 18:00 arasında olduğunu bilirler. Kullanıcılar o gün için yapıldığında kullanım daha sonra düşer. Yoğun saatler dışında, kullanıcılar mobil cihazlarını veya ev bilgisayarlarını kullanarak uygulamaya uzaktan erişebildikleri için hala bazı yükler vardır. Üretim App Service planı zaten aşağıdaki kurallarla CPU kullanımına göre otomatik ölçeklendirme için yapılandırılmıştır:

![LOB uygulaması için özel ayarlar.][asp-scale]

| **Otomatik ölçekprofili – Hafta içi – Uygulama Hizmeti planı** | **Otomatik ölçekprofili – Hafta sonları – Uygulama Hizmeti planı** |
| --- | --- |
| **Adı:** Hafta içi profili |**Adı:** Hafta sonu profili |
| **Şuna göre ölçeklendirin:** Zamanlama ve performans kuralları |**Şuna göre ölçeklendirin:** Zamanlama ve performans kuralları |
| **Profil:** Hafta içi |**Profil:** Hafta sonu |
| **Türü:** Yineleme |**Türü:** Yineleme |
| **Hedef aralığı:** 5 ila 20 örnek |**Hedef aralığı:** 3 ila 10 örnek |
| **Gün Sayısı:** Pazartesi, Salı, Çarşamba, Perşembe, Cuma |**Gün Sayısı:** Cumartesi, Pazar |
| **Başlangıç saati:** 9:00 |**Başlangıç saati:** 9:00 |
| **Saat dilimi:** UTC-08 |**Saat dilimi:** UTC-08 |
|  | |
| **Otomatik ölçeklendirme kuralı (Yukarı Ölçekle)** |**Otomatik ölçeklendirme kuralı (Yukarı Ölçekle)** |
| **Kaynak:** Üretim (Uygulama Servis Ortamı) |**Kaynak:** Üretim (Uygulama Servis Ortamı) |
| **Metrik:** CPU % |**Metrik:** CPU % |
| **İşlem:** %60'tan büyük |**İşlem:** %80'den büyük |
| **Süre:** 5 Dakika |**Süre:** 10 Dakika |
| **Zaman toplama:** Ortalama |**Zaman toplama:** Ortalama |
| **Eylem:** Sayıyı 2 artırın |**Eylem:** Sayıyı 1 artırın |
| **Soğuma (dakika):** 15 |**Soğuma (dakika):** 20 |
|  | |
| **Otomatik ölçeklendirme kuralı (Aşağı Yıkı)** |**Otomatik ölçeklendirme kuralı (Aşağı Yıkı)** |
| **Kaynak:** Üretim (Uygulama Servis Ortamı) |**Kaynak:** Üretim (Uygulama Servis Ortamı) |
| **Metrik:** CPU % |**Metrik:** CPU % |
| **İşlem:** %30'dan az |**İşlem:** %20'den az |
| **Süre:** 10 dakika |**Süre:** 15 dakika |
| **Zaman toplama:** Ortalama |**Zaman toplama:** Ortalama |
| **Eylem:** Sayıyı 1 azaltın |**Eylem:** Sayıyı 1 azaltın |
| **Soğuma (dakika):** 20 |**Soğuma (dakika):** 10 |

### <a name="app-service-plan-inflation-rate"></a>App Service planı enflasyon oranı
Otomatik ölçeklendirme için yapılandırılan Uygulama Hizmeti planları bunu saat başına maksimum hızda yapar. Bu oran, otomatik ölçek kuralında sağlanan değerlere göre hesaplanabilir.

*Uygulama Hizmeti planı enflasyon oranını* anlamak ve hesaplamak, çalışma havuzundaki ölçek değişiklikleri anlık olmadığından, App Service ortamı için oto ölçeklendirme için önemlidir.

App Service planı enflasyon oranı aşağıdaki gibi hesaplanır:

![App Service planı enflasyon oranı hesaplaması.][ASP-Inflation]

Üretim App Service planının Hafta içi profili için Otomatik Ölçek - Ölçeklendirme kuralına göre:

![Otomatik Ölçek - Ölçeklendirme kuralına göre hafta içi için Uygulama Hizmeti planı enflasyon oranı.][Equation1]

Otomatik Ölçekleme durumunda – Üretim App Service planının Hafta sonu profili için Yükseltme kuralını ölçeklendirin, formül aşağıdakileri çözecektir:

![Otomatik Ölçeklendirme - Ölçeklendirme kuralına göre hafta sonları için Uygulama Hizmeti planı enflasyon oranı.][Equation2]

Bu değer, küçültme işlemleri için de hesaplanabilir.

Üretim App Service planının Hafta içi profili için Otomatik Ölçek - Küçült kuralına göre, bu aşağıdaki gibi görünür:

![Otomatik Ölçek - Küçültme kuralına göre hafta içi için Uygulama Hizmeti planı enflasyon oranı.][Equation3]

Üretim App Service planının Hafta sonu profili için Otomatik Ölçeklendirme - Küçült kuralı söz konusu olduğunda, formül aşağıdakileri çözecektir:  

![Otomatik Ölçek - Küçült kuralına göre hafta sonları için Uygulama Hizmeti planı enflasyon oranı.][Equation4]

Üretim App Service planı hafta boyunca en fazla sekiz örnek/saat, hafta sonu ise dört örnek/saat hızında büyüyebilir. Hafta boyunca en fazla dört örnek/saat, hafta sonları ise altı örnek/saat gibi durumlarda örnekleri yayınlayabilir.

Bir işçi havuzunda birden çok Uygulama Hizmeti planı barındırılıyorsa, *toplam enflasyon oranını,* o çalışan havuzunda barındırmakta olan tüm Uygulama Hizmeti planlarının enflasyon oranının toplamı olarak hesaplamanız gerekir.

![İşçi havuzunda barındırılan birden çok Uygulama Hizmeti planı için toplam enflasyon oranı hesaplaması.][ASP-Total-Inflation]

### <a name="use-the-app-service-plan-inflation-rate-to-define-worker-pool-autoscale-rules"></a>İşçi havuzu otomatik ölçeklendirme kurallarını tanımlamak için App Service planı enflasyon oranını kullanma
Otomatik ölçeklendirme için yapılandırılan Uygulama Hizmeti planlarını barındıran alt havuzlarının bir kapasite arabelleği ayrılması gerekir. Arabellek, otomatik ölçeklendirme işlemlerinin gerektiğinde App Service planını büyütmesine ve küçültmesine olanak tanır. Minimum arabellek hesaplanan Toplam Uygulama Hizmeti Planı Enflasyon Oranı olacaktır.

Uygulama Hizmeti ortamı ölçeği işlemlerinin uygulanması biraz zaman aldığından, herhangi bir değişikliğin bir ölçek işlemi devam ederken meydana gelebilecek diğer talep değişikliklerini hesaba katması gerekir. Bu gecikmeyi karşılamak için, hesaplanan Toplam Uygulama Hizmeti Planı Enflasyon Oranını, her otomatik ölçeklendirme işlemi için eklenen en az örnek sayısı olarak kullanmanızı öneririz.

Bu bilgilerle, Frank aşağıdaki otomatik ölçek profilini ve kurallarını tanımlayabilir:

![LOB örneği için otomatik ölçeklendirme profil kuralları.][Worker-Pool-Scale]

| **Otomatik ölçeklendirme profili – Hafta içi** | **Otomatik ölçeklendirme profili – Hafta sonları** |
| --- | --- |
| **Adı:** Hafta içi profili |**Adı:** Hafta sonu profili |
| **Şuna göre ölçeklendirin:** Zamanlama ve performans kuralları |**Şuna göre ölçeklendirin:** Zamanlama ve performans kuralları |
| **Profil:** Hafta içi |**Profil:** Hafta sonu |
| **Türü:** Yineleme |**Türü:** Yineleme |
| **Hedef aralığı:** 13 ila 25 örnek |**Hedef aralığı:** 6 ila 15 örnek |
| **Gün Sayısı:** Pazartesi, Salı, Çarşamba, Perşembe, Cuma |**Gün Sayısı:** Cumartesi, Pazar |
| **Başlangıç saati:** 7:00 |**Başlangıç saati:** 9:00 |
| **Saat dilimi:** UTC-08 |**Saat dilimi:** UTC-08 |
|  | |
| **Otomatik ölçeklendirme kuralı (Yukarı Ölçekle)** |**Otomatik ölçeklendirme kuralı (Yukarı Ölçekle)** |
| **Kaynak:** İşçi havuzu 1 |**Kaynak:** İşçi havuzu 1 |
| **Metrik:** İşçiler Kullanılabilir |**Metrik:** İşçiler Kullanılabilir |
| **İşlem:** 8'den az |**İşlem:** 3'ten az |
| **Süre:** 20 dakika |**Süre:** 30 dakika |
| **Zaman toplama:** Ortalama |**Zaman toplama:** Ortalama |
| **Eylem:** Sayıyı 8 artırın |**Eylem:** Sayıyı 3 artırın |
| **Soğuma (dakika):** 180 |**Soğuma (dakika):** 180 |
|  | |
| **Otomatik ölçeklendirme kuralı (Aşağı Yıkı)** |**Otomatik ölçeklendirme kuralı (Aşağı Yıkı)** |
| **Kaynak:** İşçi havuzu 1 |**Kaynak:** İşçi havuzu 1 |
| **Metrik:** İşçiler Kullanılabilir |**Metrik:** İşçiler Kullanılabilir |
| **İşlem:** 8'den büyük |**İşlem:** 3'ten büyük |
| **Süre:** 20 dakika |**Süre:** 15 dakika |
| **Zaman toplama:** Ortalama |**Zaman toplama:** Ortalama |
| **Eylem:** Sayıyı 2'ye düşürün |**Eylem:** Sayıyı 3'e düşürün |
| **Soğuma (dakika):** 120 |**Soğuma (dakika):** 120 |

Profilde tanımlanan Hedef aralığı, App Service planı + arabelleği profilinde tanımlanan minimum örneklere göre hesaplanır.

Maksimum aralık, işçi havuzunda barındırılan tüm Uygulama Hizmeti planları için maksimum aralıkların toplamı olacaktır.

Ölçeklendirme kurallarıiçin Artış sayısı, ölçeklendirme için Uygulama Hizmeti Planı Enflasyon Oranı'nın en az 1Kat olarak ayarlanmalıdır.

Azalma sayısı, küçültme için Uygulama Hizmeti Planı Enflasyon Oranı'nın 1/2X veya 1X arasında bir şeye ayarlanabilir.

### <a name="autoscale-for-front-end-pool"></a>Ön uç havuzu için otomatik ölçeklendirme
Ön uç otomatik ölçeklendirme kuralları, çalışan havuzlarına göre daha basittir. Öncelikle,  
ölçüm süresinin ve bekleme sürelerinin, Bir Uygulama Hizmeti planındaki ölçek işlemlerinin anlık olmadığını göz önünde bulundurun.

Bu senaryo için Frank, ön uçlar %80 CPU kullanımına ulaştıktan sonra hata oranının arttığını bilir ve örnekleri aşağıdaki gibi artırmak için otomatik ölçek kuralını ayarlar:

![Ön uç havuzu için otomatik ölçeklendirme ayarları.][Front-End-Scale]

| **Otomatik ölçeklendirme profili – Ön uçlar** |
| --- |
| **Adı:** Otomatik ölçeklendirme – Ön uçlar |
| **Şuna göre ölçeklendirin:** Zamanlama ve performans kuralları |
| **Profil:** Günlük |
| **Türü:** Yineleme |
| **Hedef aralığı:** 3 ila 10 örnek |
| **Gün Sayısı:** Günlük |
| **Başlangıç saati:** 9:00 |
| **Saat dilimi:** UTC-08 |
|  |
| **Otomatik ölçeklendirme kuralı (Yukarı Ölçekle)** |
| **Kaynak:** Ön uç havuzu |
| **Metrik:** CPU % |
| **İşlem:** %60'tan büyük |
| **Süre:** 20 dakika |
| **Zaman toplama:** Ortalama |
| **Eylem:** Sayıyı 3 artırın |
| **Soğuma (dakika):** 120 |
|  |
| **Otomatik ölçeklendirme kuralı (Aşağı Yıkı)** |
| **Kaynak:** İşçi havuzu 1 |
| **Metrik:** CPU % |
| **İşlem:** %30'dan az |
| **Süre:** 20 Dakika |
| **Zaman toplama:** Ortalama |
| **Eylem:** Sayıyı 3'e düşürün |
| **Soğuma (dakika):** 120 |

<!-- IMAGES -->
[intro]: ./media/app-service-environment-auto-scale/introduction.png
[settings-scale]: ./media/app-service-environment-auto-scale/settings-scale.png
[scale-manual]: ./media/app-service-environment-auto-scale/scale-manual.png
[scale-profile]: ./media/app-service-environment-auto-scale/scale-profile.png
[scale-profile2]: ./media/app-service-environment-auto-scale/scale-profile-2.png
[scale-rule]: ./media/app-service-environment-auto-scale/scale-rule.png
[asp-scale]: ./media/app-service-environment-auto-scale/asp-scale.png
[ASP-Inflation]: ./media/app-service-environment-auto-scale/asp-inflation-rate.png
[Equation1]: ./media/app-service-environment-auto-scale/equation1.png
[Equation2]: ./media/app-service-environment-auto-scale/equation2.png
[Equation3]: ./media/app-service-environment-auto-scale/equation3.png
[Equation4]: ./media/app-service-environment-auto-scale/equation4.png
[ASP-Total-Inflation]: ./media/app-service-environment-auto-scale/asp-total-inflation-rate.png
[Worker-Pool-Scale]: ./media/app-service-environment-auto-scale/wp-scale.png
[Front-End-Scale]: ./media/app-service-environment-auto-scale/fe-scale.png
