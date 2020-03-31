---
title: Azure Monitör ile Active Directory ortamınızı optimize edin | Microsoft Dokümanlar
description: Ortamlarınızın risk ve sağlığını düzenli aralıklarla değerlendirmek için Active Directory Health Check çözümünü kullanabilirsiniz.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 09/10/2019
ms.openlocfilehash: 57c474c8391168702154b71e0c454253ab921dc1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77667236"
---
# <a name="optimize-your-active-directory-environment-with-the-active-directory-health-check-solution-in-azure-monitor"></a>Azure İzleyici'de Active Directory Sistem Durumu Denetimi çözümüyle Active Directory ortamınızı iyileştirme

![AD Sağlık Kontrolü simgesi](./media/ad-assessment/ad-assessment-symbol.png)

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

Sunucu ortamlarınızın risk ve sistem durumunu düzenli aralıklarla değerlendirmek için Active Directory Health Check çözümünü kullanabilirsiniz. Bu makale, olası sorunlar için düzeltici eylemlerde bulunmak için çözümü yüklemenize ve kullanmanıza yardımcı olur.

Bu çözüm, dağıtılmış sunucu altyapınıza özgü öncelikle öneriler listesi sağlar. Öneriler, riski hızlı bir şekilde anlamanıza ve harekete geçmenize yardımcı olan dört odak alanına göre sınıflandırılır.

Öneriler, Microsoft mühendislerinin binlerce müşteri ziyaretinden edindikleri bilgi ve deneyime dayanmaktadır. Her öneri, bir sorunun sizin için neden önemli olabileceği ve önerilen değişikliklerin nasıl uygulanacağı hakkında rehberlik sağlar.

Kuruluşunuz için en önemli olan odak alanlarını seçebilir ve risksiz ve sağlıklı bir ortam çalıştırma yolundaki ilerlemenizi izleyebilirsiniz.

Çözümü ekledikten ve bir denetim tamamlandıktan sonra, odak alanlarına ait özet bilgiler ortamınızdaki altyapı için **AD Sağlık Kontrolü** panosunda gösterilir. Aşağıdaki bölümlerde, Etkin Dizin sunucu altyapınız için önerilen eylemleri görüntüleyip sonra da yapabileceğiniz **AD Sağlık Durumu Denetimi** panosundaki bilgilerin nasıl kullanılacağı açıklanmıştır.  

![AD Sağlık Kontrolü karosu görüntüsü](./media/ad-assessment/ad-healthcheck-summary-tile.png)

![AD Sağlık Kontrolü panosu görüntüsü](./media/ad-assessment/ad-healthcheck-dashboard-01.png)

## <a name="prerequisites"></a>Ön koşullar

* Active Directory Health Check çözümü, Windows için Log Analytics aracısı (Microsoft İzleme Aracısı (MMA) olarak da adlandırılır) yüklü olan her bilgisayarda yüklü olan .NET Framework 4.6.2 veya üzeri desteklenen bir sürümünü gerektirir.  Aracı, System Center 2016 - Operations Manager, Operations Manager 2012 R2 ve Azure Monitor tarafından kullanılır.
* Çözüm, Windows Server 2008 ve 2008 R2, Windows Server 2012 ve 2012 R2 ve Windows Server 2016 çalıştıran etki alanı denetleyicilerini destekler.
* Azure portalındaki Azure pazar yerinden Active Directory Health Check çözümlerini eklemek için Bir Günlük Analizi çalışma alanı. Ek yapılandırma gerekmez.

  > [!NOTE]
  > Çözümü ekledikten sonra AdvisorAssessment.exe dosyası aracılı sunuculara eklenir. Yapılandırma verileri okunur ve işlenmek üzere buluttaki Azure Monitörü'ne gönderilir. Alınan verilere mantık uygulanır ve bulut hizmeti verileri kaydeder.
  >
  >

Değerlendirilecek etki alanının üyeleri olan etki alanı denetleyicilerinize karşı sistem durumu denetimini gerçekleştirmek için, bu etki alanındaki her etki alanı denetleyicisi, aşağıdaki desteklenen yöntemlerden birini kullanarak bir aracı ve Azure Monitor bağlantısı gerektirir:

1. Etki alanı denetleyicisi System Center 2016 - Operations Manager veya Operations Manager 2012 R2 tarafından zaten izlenmiyorsa [Windows için Log Analytics aracısını](../../azure-monitor/platform/agent-windows.md) yükleyin.
2. System Center 2016 - Operations Manager veya Operations Manager 2012 R2 ve yönetim grubu Azure Monitor ile entegre edilmezse, etki alanı denetleyicisi veri toplamak ve hizmete iletmek için Azure Monitor ile birden fazla yuvaya sahip olabilir ve yine de Operasyon Müdürü tarafından izlenmelidir.  
3. Aksi takdirde, Operations Manager yönetim grubunuz hizmetle bütünleşmişse, çözüme olanak verdikten sonra iş yeriniz tarafından [yönetilen bilgisayarlar ekle](../../azure-monitor/platform/om-agents.md#connecting-operations-manager-to-azure-monitor) adımları altında nağmeler altında veri toplama için etki alanı denetleyicilerini eklemeniz gerekir.  

Etki alanı denetleyicinizde bir Operasyon Yöneticisi yönetim grubuna rapor veren, veri toplayan, atanan yönetim sunucusuna ileten ve doğrudan bir yönetim sunucusundan Azure Monitörü'ne gönderilen aracı.  Veriler Operasyon Yöneticisi veritabanlarına yazılmadı.  

## <a name="active-directory-health-check-data-collection-details"></a>Active Directory Sistem Durumu Denetimi veri toplama ayrıntıları

Etkin Dizin Durumu Denetimi, etkinleştirdiğiniz aracıyı kullanarak aşağıdaki kaynaklardan veri toplar:

- Kayıt Defteri
- LDAP
- .NET Framework
- Olay günlüğü
- Aktif Dizin Hizmeti arayüzleri (ADSI)
- Windows PowerShell
- Dosya verileri
- Windows Yönetim Araçları (WMI)
- DCDIAG aracı API
- Dosya Çoğaltma Hizmeti (NTFRS) API
- Özel C# kodu

Veriler etki alanı denetleyicisinde toplanır ve her yedi günde bir Azure Monitor'a iletilir.  

## <a name="understanding-how-recommendations-are-prioritized"></a>Önerilerin önceliklerini belirlemeyi anlama

Yapılan her öneriye, önerinin göreli önemini tanımlayan bir ağırlık değeri verilir. Yalnızca en önemli 10 öneri gösterilir.

### <a name="how-weights-are-calculated"></a>Ağırlıklar nasıl hesaplanır?

Ağırlıklandırmalar üç temel faktöre dayalı toplam değerlerdir:

* Bir sorunun tanımlanması *olasılığı* sorunlara neden olur. Daha yüksek bir olasılık, öneri için daha büyük bir genel puana eşittir.
* Sorunun kuruluşunuz üzerindeki *etkisi,* bir soruna neden oluyorsa. Daha yüksek bir etki, tavsiye için daha büyük bir genel puana eşittir.
* Tavsiyeyi uygulamak için gereken *çaba.* Daha yüksek bir çaba, tavsiye için daha küçük bir genel puana eşittir.

Her öneriiçin ağırlıklandırma, her odak alanı için kullanılabilir toplam puanın yüzdesi olarak ifade edilir. Örneğin, Güvenlik ve Uyumluluk odak alanındaki bir öneri %5'e sahipse, bu öneriyi uygulamak genel Güvenlik ve Uyumluluk puanınızı %5 artırır.

### <a name="focus-areas"></a>Odak alanları

**Güvenlik ve Uyumluluk** - Bu odak alanı, potansiyel güvenlik tehditleri ve ihlalleri, şirket politikaları ve teknik, yasal ve mevzuata uygunluk gereklilikleri için öneriler gösterir.

**Kullanılabilirlik ve İş Sürekliliği** - Bu odak alanı, hizmet kullanılabilirliği, altyapınızın esnekliği ve iş koruması için öneriler gösterir.

**Performans ve Ölçeklenebilirlik** - Bu odak alanı, kuruluşunuzun BT altyapısının büyümesine yardımcı olacak, BT ortamınızın geçerli performans gereksinimlerini karşıladığından ve değişen altyapı gereksinimlerine yanıt verebilmesini sağlayacak öneriler gösterir.

**Yükseltme, Geçiş ve Dağıtım** - Bu odak alanı, Active Directory'yi yükseltmenize, geçiş inizi ve dağıtmanıza yardımcı olacak öneriler gösterir.

### <a name="should-you-aim-to-score-100-in-every-focus-area"></a>Her odak alanında %100 puanı mı hedeflemeniz gerekiyor?

Mutlaka. Öneriler, Microsoft mühendislerinin binlerce müşteri ziyaretinde edindikleri bilgi ve deneyimlere dayanmaktadır. Ancak, iki sunucu altyapısı aynı değildir ve belirli öneriler sizin için az çok alakalı olabilir. Örneğin, sanal makineleriniz Internet'e açık değilse, bazı güvenlik önerileri daha az alakalı olabilir. Bazı kullanılabilirlik önerileri, düşük öncelikli geçici veri toplama ve raporlama sağlayan hizmetler için daha az alakalı olabilir. Olgun bir işletme için önemli olan sorunlar, yeni kurulan bir işletme için daha az önemli olabilir. Önceliklerinizin hangi odak alanları olduğunu belirlemek ve puanlarınızın zaman içinde nasıl değiştiğine bakmak isteyebilirsiniz.

Her öneri, neden önemli olduğu hakkında rehberlik içerir. BT hizmetlerinizin doğası ve kuruluşunuzun iş gereksinimleri göz önüne alındığında, önerinin uygulanmasının sizin için uygun olup olmadığını değerlendirmek için bu kılavuzu kullanmalısınız.

## <a name="use-health-check-focus-area-recommendations"></a>Sistem durumu denetimi odak alanı önerilerini kullanma

Yüklendikten sonra, Azure portalındaki çözüm sayfasındaKi Sistem Durumu Denetimi döşemesini kullanarak önerilerin özetini görüntüleyebilirsiniz.

Altyapınız için özet uyumluluk değerlendirmelerini görüntüleyin ve ardından ayrıntılı önerilere sahip.

### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a>Odak alanı için önerileri görüntülemek ve düzeltici eylemde bulunmak için

[!INCLUDE [azure-monitor-solutions-overview-page](../../../includes/azure-monitor-solutions-overview-page.md)]

1. Genel **Bakış** sayfasında, **Active Directory Health Check** kutusunu tıklatın.

2. Sistem **Durumu Denetimi** sayfasında, odak alanı bıçaklarından birinde özet bilgileri gözden geçirin ve ardından bu odak alanı yla ilgili önerileri görüntülemek için bir tanesini tıklatın.

3. Odak alanı sayfalarından herhangi birinde, ortamınız için yapılan öncelikli önerileri görüntüleyebilirsiniz. Önerinin neden yapıldığıyla ilgili ayrıntıları görüntülemek için **Etkilenen Nesneler** altında bir öneriyi tıklatın.

    ![Sağlık Kontrolü önerileri görüntü](./media/ad-assessment/ad-healthcheck-dashboard-02.png)

4. **Önerilen Eylemler'de**önerilen düzeltici eylemleri alabilirsiniz. Öğe ele alındığında, daha sonra yapılan değerlendirmeler, önerilen eylemlerin alındığını ve uyumluluk puanınızın artacağını kaydeder. Düzeltilen öğeler **Geçirilen Nesneler**olarak görünür.

## <a name="ignore-recommendations"></a>Önerileri yoksayma

Göz ardı etmek istediğiniz önerileriniz varsa, Azure Monitor'un değerlendirme sonuçlarınızda önerilerin görünmesini önlemek için kullanacağı bir metin dosyası oluşturabilirsiniz.

### <a name="to-identify-recommendations-that-you-will-ignore"></a>Yoksayacağınız önerileri tanımlamak için

[!INCLUDE [azure-monitor-log-queries](../../../includes/azure-monitor-log-queries.md)]

Ortamınızdaki bilgisayarlarda başarısız olan önerileri listelemek için aşağıdaki sorguyu kullanın.

```
ADAssessmentRecommendation | where RecommendationResult == "Failed" | sort by Computer asc | project Computer, RecommendationId, Recommendation
```

Burada günlük sorgusu gösteren bir ekran görüntüsü:<

![başarısız öneriler](media/ad-assessment/ad-failed-recommendations.png)

Yoksaymak istediğiniz önerileri seçin. Sonraki yordamda RecommendationId için değerleri kullanırsınız.

### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a>IgnoreRecommendations.txt metin dosyası oluşturmak ve kullanmak için

1. IgnoreRecommendations.txt adlı bir dosya oluşturun.

2. Azure Monitor'un ayrı bir satırda yoksaymasını istediğiniz her öneri için her Öneri'yi yapıştırın veya yazın ve ardından dosyayı kaydedip kapatın.

3. Azure Monitor'un önerileri yok saymasını istediğiniz her bilgisayarda dosyayı aşağıdaki klasöre koyun.

   * Microsoft Monitoring Agent (doğrudan veya Operations Manager üzerinden bağlı) ile bilgisayarlarda - *SystemDrive*:\Program Files\Microsoft Monitoring Agent\Agent
   * Operations Manager 2012 R2 yönetim sunucusu - *SystemDrive*:\Program Files\Microsoft System Center 2012 R2\Operations Manager\Server
   * Operations Manager 2016 yönetim sunucusunda - *SystemDrive*:\Program Files\Microsoft System Center 2016\Operations Manager\Server

### <a name="to-verify-that-recommendations-are-ignored"></a>Önerilerin yoksayDığını doğrulamak için

Bir sonraki zamanlanmış sistem durumu denetimi çalıştırıldıktan sonra, varsayılan olarak her yedi günde bir, belirtilen öneriler *Yoksayıldı* olarak işaretlenir ve panoda görünmez.

1. Tüm gözardı edilen önerileri listelemek için aşağıdaki günlük sorgularını kullanabilirsiniz.

    ```
    ADAssessmentRecommendation | where RecommendationResult == "Ignored" | sort by Computer asc | project Computer, RecommendationId, Recommendation
    ```

2. Daha sonra yoksayan önerileri görmek istediğinize karar verirseniz, YokSayöneriler.txt dosyalarını kaldırın veya RecommendationsID'leri kaldırabilirsiniz.

## <a name="ad-health-check-solutions-faq"></a>AD Sistem Durumu Denetimi çözümleri SSS

*AD Değerlendirmesi çözümü tarafından hangi kontroller yapılır?*

* Aşağıdaki sorgu, şu anda gerçekleştirilen tüm denetimlerin açıklamasını gösterir:

```Kusto
ADAssessmentRecommendation
| distinct RecommendationId, FocusArea, ActionArea, Recommendation, Description
| sort by FocusArea,ActionArea, Recommendation
```
Sonuçlar daha sonra daha fazla gözden geçirilmek üzere Excel'e aktarılabilir.

*Sağlık kontrolü ne sıklıkta çalışır?*

* Çek her yedi günde bir çalışır.

*Sağlık denetiminin ne sıklıkta çalıştığını yapılandırmanın bir yolu var mı?*

* Şu anda değil.

*Ben bir sağlık denetimi çözümü ekledikten sonra başka bir sunucu keşfedilirse, kontrol edilecektir*

* Evet, bir kez o andan itibaren, her yedi günde bir kontrol edilir keşfedilir.

*Bir sunucu kullanımdan kaldırılırsa, sistem durumu denetiminden ne zaman kaldırılır?*

* Bir sunucu 3 hafta boyunca veri göndermezse, kaldırılır.

*Veri toplama işleminin adı nedir?*

* AdvisorAssessment.exe

*Verilerin toplanması ne kadar sürer?*

* Sunucudaki gerçek veri toplama yaklaşık 1 saat sürer. Çok sayıda Active Directory sunucusu olan sunucularda daha uzun sürebilir.

*Veriler toplandığında yapılandırmanın bir yolu var mı?*

* Şu anda değil.

*Neden sadece en iyi 10 önerileri görüntüleniyor?*

* Size kapsamlı ezici bir görev listesi vermek yerine, önce öncelik verilen önerileri ele almaya odaklanmanızı öneririz. Bunları ele aldıktan sonra, ek öneriler kullanılabilir olacaktır. Ayrıntılı listeyi görmek isterseniz, günlük sorgusunu kullanarak tüm önerileri görüntüleyebilirsiniz.

*Bir öneriyi yok saymanın bir yolu var mı?*

* Evet, yukarıdaki [önerileri yoksay](#ignore-recommendations) bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

Ayrıntılı AD Sistem Durumu Denetimi verilerini ve önerilerini nasıl analiz edeceğiz öğrenmek için [Azure Monitor günlük sorgularını](../log-query/log-query-overview.md) kullanın.
