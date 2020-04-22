---
title: Azure Key Vault izleme ve uyarı | Microsoft Dokümanlar
description: Anahtar kasanızın durumunu izlemek ve uyarıları yapılandırmak için bir pano oluşturun.
services: key-vault
author: ShaneBala-keyvault
manager: ravijan
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: conceptual
ms.date: 04/06/2020
ms.author: sudbalas
Customer intent: As a key vault administrator, I want to learn the options available to monitor the health of my vaults
ms.openlocfilehash: cc0d969ff6eb76732768dfed2826762920ae9e62
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81726949"
---
# <a name="monitoring-and-alerting-for-azure-key-vault"></a>Azure Anahtar Kasası için izleme ve uyarı

## <a name="overview"></a>Genel Bakış

Üretim sırlarınızı saklamak için anahtar kasasını kullanmaya başladıktan sonra, hizmetinizin beklendiği gibi çalıştığından emin olmak için anahtar kasanızın durumunu izlemeniz önemlidir. Hizmetinizi ölçeklendirmeye başladığınızda anahtar kasanıza gönderilen istek sayısı artacaktır. Bu, isteklerinizin gecikmesini artırma ve aşırı durumlarda isteklerinizin daraltılmasına neden olma potansiyeline sahiptir ve bu da hizmetinizin performansını etkiler. Ayrıca, anahtar kasanız alışılmadık sayıda hata kodu gönderiyorsa, herhangi bir erişim ilkesi veya güvenlik duvarı yapılandırma sorunları hakkında hızlı bir şekilde bilgilendirilebilmeniz için uyarı almanız gerekir. Bu belge aşağıdaki konuları kapsayacaktır:

+ İzlenecek Temel Anahtar Kasası ölçümleri
+ Ölçümleri yapılandırma ve pano oluşturma 
+ Belirtilen eşiklerde uyarı oluşturma 

## <a name="basic-key-vault-metrics-to-monitor"></a>İzlenecek Temel Anahtar Kasası ölçümleri

+ Kasa Durumu  
+ Tonoz Doygunluğu 
+ Servis API GecikmeSi 
+ Toplam Hizmet API Isabetleri (Etkinlik Türüne Göre Filtre) 
+ Hata Kodları (Durum Koduna Göre Filtre) 

**Vault Kullanılabilirliği** - Bu metrik her zaman % 100 olmalıdır, bu izlemek için önemli bir metriktir, çünkü anahtar kasanızda bir kesinti yaşanMışsa size hızlı bir şekilde gösterebilir. 

**Vault Doygunluğu** – Anahtar kasanın hizmet edebileceği saniyedeki istek sayısı, gerçekleştirilen işlem türüne bağlıdır. Bazı kasa işlemlerinde saniye başına daha düşük istek eşiği vardır. Bu metrik, geçerli anahtar kasası kullanımınızı gösteren bir yüzde değeri bulmak için tüm işlem türlerinde anahtar kasanızın toplam kullanımını toplar. Önemli kasa hizmet sınırlarının tam listesi için aşağıdaki belgeye bakın. [Azure Key Vault Hizmet Sınırları](service-limits.md)

**Servis API Gecikmesi** - Bu metrik, anahtar kasasına yapılan bir çağrının ortalama gecikmesini gösterir. Anahtar kasanız hizmet sınırları içinde olsa da, anahtar kasasının yüksek kullanımı, akış aşağı uygulamaların başarısız olmasına neden olan gecikme süresine neden olabilir. 

**Toplam API Isabetleri** - Bu metrik, anahtar kasanıza yapılan tüm aramaları gösterir. Bu, anahtar kasanızı hangi uygulamaların aradığını belirlemenize yardımcı olur. 

**Hata Kodları** – Bu metrik, anahtar kasanızda olağandışı miktarda hata olup olmadığını gösterir. Hata kodlarının ve sorun giderme kılavuzunun tam listesi için aşağıdaki belgeye bakın. [Azure Key Vault REST API Hata Kodları](rest-error-codes.md)

## <a name="how-to-configure-metrics-and-create-a-dashboard"></a>Ölçümleri yapılandırma ve pano oluşturma

1. Azure portalında oturum açın
2. Anahtar Kasanıza Gidin
3. **İzleme** Altında **Ölçümleri** Seçin 

> [!div class="mx-imgBorder"]
> ![Azure portalının ekran görüntüsü](../media/alert-1.png)

4. Grafiğin başlığını panonuzda görmek istediklerinize güncelleştirin. 
5. Kapsamı seçin. Bu örnekte tek bir anahtar kasası seçeceğiz. 
6. Metrik **Genel Kasa Kullanılabilirliğini** ve Toplama **Avg'sini** seçin 
7. Zaman aralığını Son 24 Saat'e güncelleştirin ve zaman parçalı nı 1 dakikaya güncelleştirin. 

> [!div class="mx-imgBorder"]
> ![Azure portalının ekran görüntüsü](../media/alert-2.png)

8. Vault Doygunluk ve Hizmet API Gecikme sonu ölçümleri için yukarıdaki adımları tekrarlayın. Metriklerinizi bir panoya kaydetmek için **Pano'ya Pin'i** seçin. 

> [!IMPORTANT]
> "Panoya Sabitle" seçeneğini belirleyin ve yapılandırdığınız her metrik kaydedin. Sayfayı terk edip kaydetmeden sayfaya geri dönerseniz, yapılandırma değişiklikleriniz kaybolur. 

9. Anahtar kasasındaki tüm işlem türlerini izlemek için **Toplam Hizmet API Hits** Metrik'i kullanın ve Etkinlik **Türüne Göre Splitting Uygula'yı** seçin

> [!div class="mx-imgBorder"]
> ![Azure portalının ekran görüntüsü](../media/alert-3.png)

10. Anahtar kasasındaki hata kodlarını izlemek için **Toplam Hizmet API Sonuç** Metrik'ini kullanın ve Etkinlik **Türüne Göre Bölme Uygula'yı** seçin

> [!div class="mx-imgBorder"]
> ![Azure portalının ekran görüntüsü](../media/alert-4.png)

Şimdi bu gibi görünen bir pano olacak. Her kutucuğun sağ üst kısmındaki 3 noktayı tıklatabilir ve kutucukları istediğiniz gibi yeniden düzenleyip yeniden boyutlandırabilirsiniz. 

Panonu kaydedip yayımladığınızda, Azure aboneliğinizde yeni bir kaynak oluşturur. "Paylaşılan pano" için arama yaparak istediğiniz zaman görebilirsiniz. 

> [!div class="mx-imgBorder"]
> ![Azure portalının ekran görüntüsü](../media/alert-5.png)

## <a name="how-to-configure-alerts-on-your-key-vault"></a>Key Vault'unuzdaki uyarıları yapılandırma 

Bu bölümde, anahtar kasanızda uyarıları nasıl yapılandıracağınızı gösterilecek, böylece anahtar kasanız sağlıksız durumdaysa ekibinizi derhal harekete geçmeleri konusunda uyarabilirsiniz. Tercihen bir dl ekibine e-posta gönderen uyarıları yapılandırabilir, olay ızgara bildirimi ni verebilir veya bir telefon numarasını arayabilir veya kısa mesaj atabilirsiniz. Ayrıca, sabit bir değere dayalı statik uyarıları veya izlenen bir metrik, belirli bir zaman aralığı içinde anahtar kasanızın ortalama sınırını aşarsa sizi uyaracak dinamik bir uyarı da seçebilirsiniz. 

> [!IMPORTANT]
> Bildirimleri göndermeye başlamak için yeni yapılandırılan uyarıların 10 dakika kadar sürebileceğini lütfen unutmayın. 

### <a name="configure-an-action-group"></a>Eylem grubunu yapılandırma 

Eylem grubu, bildirimlerin ve özelliklerin yapılandırılabilir bir listesidir.

1. Azure portalında oturum açın
2. Arama kutusunda **Uyarıları** Ara
3. **Eylemleri Yönet'i** seçin

> [!div class="mx-imgBorder"]
> ![Azure portalının ekran görüntüsü](../media/alert-6.png)

4. Select **+ Add Action Group**

> [!div class="mx-imgBorder"]
> ![Azure portalının ekran görüntüsü](../media/alert-7.png)

5. Eylem Grubunuz için **Eylem Türünü** seçin. Bu örnekte, bir e-posta uyarısı oluştururuz.

> [!div class="mx-imgBorder"]
> ![Azure portalının ekran görüntüsü](../media/alert-8.png)

> [!div class="mx-imgBorder"]
> ![Azure portalının ekran görüntüsü](../media/alert-9.png)

6. Sayfanın altından **Tamam**’a tıklayın. Başarılı bir eylem grubu oluşturdunuz. 

Artık bir eylem grubu yapılandırdığınıza göre, anahtar tonoz uyarı eşlemlerini yapılandıracağız. 

### <a name="configure-alert-thresholds"></a>Uyarı eşeklerini yapılandırma 

1. Azure portalında anahtar kasa kaynağınızı seçin ve **İzleme** altında **Uyarılar'ı** seçin

> [!div class="mx-imgBorder"]
> ![Azure portalının ekran görüntüsü](../media/alert-10.png)

2. **Yeni Uyarı Kuralı'nı** seçin

> [!div class="mx-imgBorder"]
> ![Azure portalının ekran görüntüsü](../media/alert-11.png)

3. Uyarı kuralınızın kapsamını seçin. Tek bir kasa veya birden çok seçebilirsiniz. 

> [!IMPORTANT]
> Uyarılarınızın kapsamı için birden çok kasa seçerken, seçilen tüm kasaların aynı bölgede olması gerektiğini lütfen unutmayın. Farklı bölgelerdeki kasalar için ayrı uyarı kuralları yapılandırmanız gerekir. 

> [!div class="mx-imgBorder"]
> ![Azure portalının ekran görüntüsü](../media/alert-12.png)

4. Uyarılarınız için koşulları seçin. Aşağıdaki sinyallerden herhangi birini seçebilir ve uyarı için mantığınızı tanımlayabilirsiniz. Key Vault ekibi aşağıdaki uyarı eşiklerini yapılandırmanızı önerir. 

    + Anahtar Kasa Sıyrık %100'ünün altına düşer (Statik Eşik)
    + Anahtar Kasa Gecikmesi 500m'den büyüktür (Statik Eşik) 
    + Genel Kasa Doygunluğu %75'ten büyüktür (Statik Eşik) 
    + Genel Kasa Doygunluğu ortalamayı aşıyor (Dinamik Eşik)
    + Ortalamanın üzerinde toplam Hata Kodları (Dinamik Eşik) 

> [!div class="mx-imgBorder"]
> ![Azure portalının ekran görüntüsü](../media/alert-13.png)

### <a name="example-1-configuring-a-static-alert-threshold-for-latency"></a>Örnek 1: Gecikme için statik uyarı eşiğini yapılandırma

Sinyal adı olarak **Genel Hizmet API Gecikmesi'ni** seçin
> [!div class="mx-imgBorder"]
> ![Azure portalının ekran görüntüsü](../media/alert-14.png)

Lütfen aşağıdaki yapılandırma parametrelerine bakın.

+ Eşiği **Statik** Olarak Ayarlama 
+ İşleciyi **Büyük**
+ Toplama Türünü **Ortalamaya** Ayarlama
+ Eşik Değerini **500** olarak ayarlama
+ Toplama Süresini 5 **dakikaya** ayarla
+ Değerlendirme Sıklığını **1 dakikaya** ayarlama
+ **Bitti'yi** seçin  

> [!div class="mx-imgBorder"]
> ![Azure portalının ekran görüntüsü](../media/alert-15.png)

### <a name="example-2-configuring-a-dynamic-alert-threshold-for-vault-saturation"></a>Örnek 2: Kasa doygunluğu için dinamik bir uyarı eşiğini yapılandırma 

Dinamik bir uyarı kullandığınızda, seçtiğiniz anahtar kasasının geçmiş verilerini görebilirsiniz. Mavi alan, anahtar kasanızın ortalama kullanımını temsil eder. Kırmızı alan, uyarı yapılandırmasındaki diğer ölçütlerin karşılanması koşuluyla bir uyarıtetikleyen ani artışlar gösterir. Kırmızı nokta, uyarı ölçütlerinin toplu zaman penceresi sırasında karşılandığı ihlal örneklerini gösterir. Belirli bir süre içinde belirli sayıda ihlalden sonra yangın alarmı ayarlayabilirsiniz. Geçmiş verileri eklemek istemiyorsanız, gelişmiş ayarlarda aşağıda eski verileri hariç tutma seçeneği vardır. 

> [!div class="mx-imgBorder"]
> ![Azure portalının ekran görüntüsü](../media/alert-16.png)

Lütfen aşağıdaki yapılandırma parametrelerine bakın.

+ Eşiği **Dinamik** Olarak Ayarlayın 
+ İşleciyi **Büyük**
+ Toplama Türünü **Ortalamaya** Ayarlama
+ Eşik Hassasiyetini **Orta** Ortama Ayarlama
+ Toplama Süresini 5 **dakikaya** ayarla
+ Değerlendirme Sıklığını **1 dakikaya** ayarlama
+ **İsteğe bağlı** Gelişmiş Ayarları Yapılandırma 
+ **Bitti'yi** seçin

> [!div class="mx-imgBorder"]
> ![Azure portalının ekran görüntüsü](../media/alert-17.png)

5. Yapılandırma yaptığınız eylem grubunu ekleme

> [!div class="mx-imgBorder"]
> ![Azure portalının ekran görüntüsü](../media/alert-18.png)

6. Uyarıyı etkinleştirin ve önem derecesi atama

> [!div class="mx-imgBorder"]
> ![Azure portalının ekran görüntüsü](../media/alert-19.png)

7. Uyarıyı oluşturma 


## <a name="next-steps"></a>Sonraki adımlar

Tebrikler, şimdi başarıyla bir izleme panosu ve anahtar kasa için uyarılar yapılandırılmış oluşturduk! Yukarıdaki tüm adımları izledikten sonra, anahtar kasanız yapılandırdığınız uyarı ölçütlerini karşıladığında e-posta uyarıları almanız gerekir. Aşağıda bir örnek gösterilmiştir. Anahtar kasanızın durumunu etkin bir şekilde izlemek için bu makalede ayarlamış olduğunuz araçları kullanın. 

### <a name="example-email-alert"></a>Örnek e-posta uyarısı 

> [!div class="mx-imgBorder"]
> ![Azure portalının ekran görüntüsü](../media/alert-20.png)
