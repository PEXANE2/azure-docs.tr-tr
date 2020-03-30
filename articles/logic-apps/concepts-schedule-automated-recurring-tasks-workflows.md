---
title: Azure Mantık Uygulamaları'nda yinelenen görevleri ve iş akışlarını zamanlama
description: Azure Logic Apps ile yinelenen otomatik görevleri, işlemleri ve iş akışlarını zamanlama hakkında genel bakış
services: logic-apps
ms.suite: integration
ms.reviewer: deli, klam, logicappspm
ms.topic: conceptual
ms.date: 05/25/2019
ms.openlocfilehash: 0f6ec158cf6ab855191e6796be3abec7d37439a0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79270569"
---
# <a name="schedule-and-run-recurring-automated-tasks-processes-and-workflows-with-azure-logic-apps"></a>Azure Logic Apps ile yinelenen otomatik görevleri, işlemleri ve iş akışlarını zamanlama ve çalıştırma

Logic Apps, bir zamanlamada otomatik yinelenen görevler ve işlemler oluşturmanıza ve çalıştırmanıza yardımcı olur. Zamançizelgesi türü tetikleyiciler olan yerleşik Yineleme tetikleyicisi veya Kayan Pencere tetikleyicisiyle başlayan bir mantık uygulaması iş akışı oluşturarak, görevleri hemen, daha sonra veya yinelenen bir aralıkta çalıştırabilirsiniz. HTTP veya HTTPS bitiş noktaları gibi Azure'un içindeki ve dışındaki hizmetleri arayabilir, Azure Depolama ve Azure Hizmet Veri Servisi gibi Azure hizmetlerine ileti gönderebilir veya dosya paylaşımına yüklenen dosyaları alabilirsiniz. Yineleme tetikleyicisi ile, görevleri çalıştırmak için karmaşık zamanlamalar ve gelişmiş yinelemeler de ayarlayabilirsiniz. Yerleşik Zamanlama tetikleyicileri ve eylemleri hakkında daha fazla bilgi edinmek için Zamanlama [tetikleyicileri](#schedule-triggers) ve [Zamanlama eylemlerine](#schedule-actions)bakın. 

> [!TIP]
> Her zamanlanmış iş için ayrı bir mantık uygulaması oluşturmadan ve bölge ve abonelik başına iş akışları sınırına girmeden yinelenen iş yüklerini zamanlayabilir [ve çalıştırabilirsiniz.](../logic-apps/logic-apps-limits-and-config.md#definition-limits) Bunun yerine, Azure QuickStart şablonu tarafından oluşturulan mantık uygulaması deseni [kullanabilirsiniz: Logic Apps iş zamanlayıcısı.](https://github.com/Azure/azure-quickstart-templates/tree/master/301-logicapps-jobscheduler/)
>
> Logic Apps iş zamanlayıcısı şablonu, TimerJob mantık uygulaması çağıran bir CreateTimerJob mantık uygulaması oluşturur. Daha sonra, http isteği nde bulunarak ve istek için giriş olarak bir zamanlama geçirerek CreateTimerJob mantık uygulamasını API olarak arayabilirsiniz. CreateTimerJob mantık uygulamasına yapılan her arama, belirtilen zamanlamaya göre veya belirtilen sınırı karşılayana kadar sürekli olarak çalışan yeni bir TimerJob örneği oluşturan TimerJob mantık uygulamasını da çağırır. Bu şekilde, örnekler tek tek mantık uygulaması iş akışı tanımları veya kaynakları olmadığından, iş akışı sınırları hakkında endişelenmeden istediğiniz kadar TimerJob örneği çalıştırabilirsiniz.

Bu liste, Zamanlama yerleşik tetikleyicileri ile çalıştırabileceğiniz bazı örnek görevleri gösterir:

* Her gün bir SQL depolanmış yordamı çalıştırmak gibi dahili verileri alın.

* NOAA'dan her 15 dakikada bir hava durumu raporları çekme gibi harici veriler alın.

* Son bir hafta içinde belirli bir miktardan daha büyük tüm siparişler için bir özet e-posta gibi rapor verileri gönderin.

* Günümüzün yüklenen görüntülerini yoğun olmayan saatlerde hafta içi her gün sıkıştırmak gibi verileri işleme.

* Üç aydan eski tüm tweetleri silme gibi verileri temizleyin.

* Sonraki dokuz ay boyunca her gün saat 01:00'de faturaları yedekleme hizmetine itme gibi arşiv verileri.

İş akışınızı bir sonraki eylem çalışmadan önce duraklatmak için Zamanlama yerleşik eylemlerini de kullanabilirsiniz( örneğin:

* E-posta üzerinden durum güncelleştirmesi göndermek için hafta içi bir güne kadar bekleyin.

* Bir HTTP çağrısının sonucu devam edip almadan önce bitirmesi gereken zamanı olana kadar iş akışını geciktirin.

Bu makalede, Zamanlama yerleşik tetikleyiciler ve eylemler için yetenekleri açıklanmaktadır.

<a name="schedule-triggers"></a>

## <a name="schedule-triggers"></a>Zamanlama tetikleyicileri

Office 365 Outlook veya SQL Server gibi belirli bir hizmet veya sistemle ilişkili olmayan Yinelenen Tetikleme veya Kayan Pencere tetikleyicisini kullanarak mantık uygulaması iş akışınızı başlatabilirsiniz. Bu tetikleyiciler, her iki tetikleyicinin saniye, dakika ve saat sayısı veya Yineleme tetikleyicisi için gün, hafta veya ay sayısı gibi aralığı ve sıklığı seçtiğiniz belirtilen yinelemeye göre iş akışınızı başlatır ve çalıştırın. Saat diliminin yanı sıra başlangıç tarihini ve saatini de ayarlayabilirsiniz. Logic Apps, tetikleyici her tetikleher çalıştığında, mantık uygulamanız için yeni bir iş akışı örneği oluşturur ve çalıştırAr.

Bu tetikleyiciler arasındaki farklar şunlardır:

* **Yineleme**: İş akışınızı belirtilen zamanlamanıza göre düzenli zaman aralıklarında çalıştırın. Yinelemeler kaçırılırsa, Yineleme tetikleyicisi kaçırılan yinelemeleri işlemez, ancak bir sonraki zamanlanan aralıkla yinelemeleri yeniden başlatır. Saat diliminin yanı sıra bir başlangıç tarihi ve saati de belirtebilirsiniz. "Gün"ü seçerseniz, günün saatlerini ve saatin dakikalarını belirtebilirsiniz, örneğin, her gün saat 2:30'da. "Hafta"yı seçerseniz, Çarşamba ve Cumartesi gibi haftanın günlerini de seçebilirsiniz. Daha fazla bilgi için bkz: [Yinelenen görevleri ve iş akışlarını Yineleme tetikleyicisiyle oluştur, zamanlama ve çalıştır.](../connectors/connectors-native-recurrence.md)

* **Kayan Pencere**: İş akışınızı, verileri sürekli parçalar halinde işleyen düzenli zaman aralıklarıyla çalıştırın. Yinelemeler kaçırılırsa, Kayan Pencere tetikleyicisi geri döner ve kaçırılan yinelemeleri işler. İş akışınızdaki her yinelemeyi geciktirecek bir başlangıç tarihi ve saati, saat dilimi ve bir süre belirtebilirsiniz. Bu tetikleyicinin gün, hafta ve ay, günün saatleri, saat dakikalarını ve haftanın günlerini belirtme seçenekleri yoktur. Daha fazla bilgi için bkz: [Kayar Pencere tetikleyicisi ile yinelenen görevleri ve iş akışlarını oluştur, zamanlama ve çalıştır.](../connectors/connectors-native-sliding-window.md)

<a name="schedule-actions"></a>

## <a name="schedule-actions"></a>Eylemleri zamanlama

Mantık uygulaması iş akışınızdaki herhangi bir eylemden sonra, iş akışınızı bir sonraki eylem çalışmadan önce bekletmek için Gecikme ve Gecikme eylemlerini kullanabilirsiniz.

* **Gecikme**: Saniye, dakika, saat, gün, hafta veya ay gibi belirtilen zaman birimi sayısı için bir sonraki eylemi çalıştırmayı bekleyin. Daha fazla bilgi için [bkz.](../connectors/connectors-native-delay.md)

* **Gecikme :** Bir sonraki eylemi belirtilen tarih ve saate kadar çalıştırmayı bekleyin. Daha fazla bilgi için [bkz.](../connectors/connectors-native-delay.md)

## <a name="patterns-for-start-date-and-time"></a>Başlangıç tarihi ve saati için desenler

<a name="start-time"></a>

Başlangıç tarihi ve saatle yinelemeyi nasıl denetebileceğinizi ve Logic Apps hizmetinin bu yinelemeleri nasıl çalıştırdığını gösteren bazı desenler aşağıda verilmiştir:

| Başlangıç saati | Zamanlama olmadan yinelenme | Zamanlama ile yineleme (Yalnızca yineleme tetikleyicisi) |
|------------|-----------------------------|----------------------------------------------------|
| {yok} | İlk iş yükünü anında çalıştırın. <p>Gelecekteki iş yüklerini son çalışma süresine göre çalıştırın. | İlk iş yükünü anında çalıştırın. <p>Belirtilen zamanlamaya göre gelecekteki iş yüklerini çalıştırın. |
| Geçmişte başlangıç zamanı | **Yineleme tetikleyicisi:** Belirtilen başlangıç saatini temel alınatır ve geçmiş çalışma sürelerini atar. Bir sonraki çalışma zamanında ilk iş yükünü çalıştırın. <p>Gelecekteki iş yüklerini son çalıştırma saatindeki hesaplamalara göre çalıştırın. <p><p>**Kayan Pencere** tetikleyicisi: Belirtilen başlangıç saatine göre çalışma sürelerini hesaplar ve geçmiş çalışma sürelerini onurlandırz. <p>Belirtilen başlangıç saatindeki hesaplamaları temel alan gelecekteki iş yüklerini çalıştırın. <p><p>Daha fazla açıklama için, aşağıdaki tabloya bakın. | Başlangıç saatinden hesaplanan zamanlamaya göre ilk iş yükünü başlangıç saatinden *daha kısa sürede* çalıştırın. <p>Belirtilen zamanlamaya göre gelecekteki iş yüklerini çalıştırın. <p>**Not:** Zamanlamayla bir yineleme belirtirseniz, ancak zamanlama için saat veya dakika belirtmezseniz, sonraki çalışma süreleri sırasıyla ilk çalışma saatinden itibaren saat veya dakika lar kullanılarak hesaplanır. |
| Şu anda veya gelecekte başlangıç saati | Belirtilen başlangıç saatinde ilk iş yükünü çalıştırın. <p>Gelecekteki iş yüklerini son çalıştırma saatindeki hesaplamalara göre çalıştırın. | Başlangıç saatinden hesaplanan zamanlamaya göre ilk iş yükünü başlangıç saatinden *daha kısa sürede* çalıştırın. <p>Belirtilen zamanlamaya göre gelecekteki iş yüklerini çalıştırın. <p>**Not:** Zamanlamayla bir yineleme belirtirseniz, ancak zamanlama için saat veya dakika belirtmezseniz, sonraki çalışma süreleri sırasıyla ilk çalışma saatinden itibaren saat veya dakika lar kullanılarak hesaplanır. |
||||

> [!IMPORTANT]
> Yinelemeler gelişmiş zamanlama seçenekleri belirtmediğinde, gelecekteki yinelemeler son çalışma süresini temel alar.
> Bu yinelemelerin başlangıç süreleri, depolama aramaları sırasında gecikme süresi gibi etkenler nedeniyle sürüklenebilir. Mantık uygulamanızın bir yinelemeyi kaçırmadığından emin olmak için, özellikle sıklık birkaç gün veya daha uzun olduğunda aşağıdaki seçeneklerden birini kullanın:
> 
> * Yineleme için bir başlangıç zamanı sağlayın.
> 
> * **Bu saatlerde** at ve **bu dakika** özelliklerini kullanarak yinelemenin ne zaman çalıştırılması için saat ve dakika belirtin.
> 
> * Yinelenen [tetikleme yerine Sürgülü Pencere tetikleyicisini](../connectors/connectors-native-sliding-window.md)kullanın.

*Geçmiş başlangıç saati ve yineleme için örnek, ancak zamanlama yok*

Geçerli tarih ve saatin 8 Eylül 2017 saat 13:00 olduğunu varsayalım. Başlangıç tarihini ve saatini 7 Eylül 2017 olarak, geçmişte olan 14:00 olarak ve iki günde bir çalışan bir yineleme olarak belirtirsiniz.

| Başlangıç saati | Geçerli saat | Yineleme | Zamanlama |
|------------|--------------|------------|----------|
| 2017-09-**07**T14:00:00Z <br>(2017-09-**07** saat 14:00) | 2017-09-**08**T13:00:00Z <br>(2017-09-**08** saat 13:00) | Her iki günde bir | {yok} |
|||||

Yineleme tetikleyicisi için, Logic Apps altyapısı çalışma sürelerini başlangıç saatine göre hesaplar, geçmiş çalışma sürelerini atar, ilk çalıştırma için gelecekteki bir sonraki başlangıç saatini kullanır ve son çalıştırma süresine göre gelecekteki çalıştırmaları hesaplar.

Bu yineleme şu şekilde görünür:

| Başlangıç saati | İlk çalıştırma süresi | Gelecekteki çalışma süreleri |
|------------|----------------|------------------|
| 2017-09-**07,** saat 14:00 | 2017-09-**09,** saat 14:00 | 2017-09-**11:** 14:00 </br>2017-09-**13,** saat 14:00 </br>2017-09-**15:** 14:00 </br>ve benzeri ... |
||||

Yani, geçmişte ne kadar uzakta olursa olsun başlangıç saatini belirtirseniz belirtin, örneğin, 2017-09-**05** saat 14:00 veya 2017-09-**01** saat 14:00'te, ilk çalışmanız her zaman bir sonraki başlangıç saatini kullanır.

Sürgülü Pencere tetikleyicisi için, Logic Apps altyapısı çalışma sürelerini başlangıç saatine göre hesaplar, geçmiş çalışma sürelerini onurlandırz, ilk çalıştırmanın başlangıç saatini kullanır ve başlangıç saatine göre gelecekteki çalıştırmaları hesaplar.

Bu yineleme şu şekilde görünür:

| Başlangıç saati | İlk çalıştırma süresi | Gelecekteki çalışma süreleri |
|------------|----------------|------------------|
| 2017-09-**07,** saat 14:00 | 2017-09-**07,** saat 14:00 | 2017-09-**09,** saat 14:00 </br>2017-09-**11:** 14:00 </br>2017-09-**13,** saat 14:00 </br>2017-09-**15:** 14:00 </br>ve benzeri ... |
||||

Yani, geçmişte ne kadar uzağa doğru başlangıç saatini belirtirseniz belirtin, örneğin, 2017-09-**05** saat 14:00 veya 2017-09-**01** saat 14:00'te, ilk çalıştırmanız her zaman belirtilen başlangıç saatini kullanır.

<a name="example-recurrences"></a>

## <a name="example-recurrences"></a>Örnek yinelemeler

Seçenekleri destekleyen tetikleyiciler için ayarlayabildiğiniz çeşitli örnek yinelemeler şunlardır:

| Tetikleyici | Yineleme | Interval | Frequency | Başlangıç saati | Şu günlerde | Şu saatlerde | Şu dakikalarda | Not |
|---------|------------|----------|-----------|------------|---------------|----------------|------------------|------|
| Yineleme <br>Kayan Pencere | Her 15 dakikada bir çalıştırın (başlangıç tarihi ve saati yok) | 15 | Dakika | {yok} | {kullanılamıyor} | {yok} | {yok} | Bu zamanlama hemen başlar, sonra son çalışma süresine göre gelecekteki yinelemeleri hesaplar. |
| Yineleme <br>Kayan Pencere | Her 15 dakikada bir çalıştırın (başlangıç tarihi ve saati ile) | 15 | Dakika | *başlangıç Tarihi* T*başlangıçTime*Z | {kullanılamıyor} | {yok} | {yok} | Bu zamanlama belirtilen başlangıç tarihi ve saatinden *daha erken* başlamaz, ardından son çalıştırma saatini temel alan gelecekteki yinelemeleri hesaplar. |
| Yineleme <br>Kayan Pencere | Her saat çalıştırın, saat (başlangıç tarihi ve saati ile) | 1 | Saat | *başlangıç Tarihi* Perşembe:00:00Z | {kullanılamıyor} | {yok} | {yok} | Bu zamanlama belirtilen başlangıç tarihi ve saatinden *önce* başlamaz. Gelecekteki yinelemeler, başlangıç saatinden hesaplanan "00" dakika işaretiyle her saat çalışır. <p>Sıklık "Hafta" veya "Ay" ise, bu zamanlama sırasıyla haftada yalnızca bir gün veya ayda bir gün çalışır. |
| Yineleme <br>Kayan Pencere | Her saat, her gün çalıştırın (başlangıç tarihi ve saati yok) | 1 | Saat | {yok} | {kullanılamıyor} | {yok} | {yok} | Bu zamanlama hemen başlar ve son çalışma süresine göre gelecekteki yinelemeleri hesaplar. <p>Sıklık "Hafta" veya "Ay" ise, bu zamanlama sırasıyla haftada yalnızca bir gün veya ayda bir gün çalışır. |
| Yineleme <br>Kayan Pencere | Her saat, her gün çalıştırın (başlangıç tarihi ve saati ile) | 1 | Saat | *başlangıç Tarihi* T*başlangıçTime*Z | {kullanılamıyor} | {yok} | {yok} | Bu zamanlama belirtilen başlangıç tarihi ve saatinden *daha erken* başlamaz, ardından son çalıştırma saatini temel alan gelecekteki yinelemeleri hesaplar. <p>Sıklık "Hafta" veya "Ay" ise, bu zamanlama sırasıyla haftada yalnızca bir gün veya ayda bir gün çalışır. |
| Yineleme <br>Kayan Pencere | Her 15 dakikada bir saat, her saat (başlangıç tarihi ve saati ile) çalıştırın | 1 | Saat | *başlangıç Tarihi* T00:15:00Z | {kullanılamıyor} | {yok} | {yok} | Bu zamanlama belirtilen başlangıç tarihi ve saatinden *önce* başlamaz. Gelecekteki yinelemeler, başlangıç saatinden hesaplanan "15" dakika işaretinde çalışır, bu nedenle 00:15, 01:15, 02:15, 02:15 ve benzeri. |
| Yineleme | Her 15 dakikada bir saat, her saat (başlangıç tarihi ve saati yok) çalıştırın | 1 | Gün | {yok} | {kullanılamıyor} | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | 15 | Bu program 00:15, 01:15, 02:15 ve benzeri çalışır. Ayrıca, bu zamanlama "Saat" sıklığına ve "15" dakika içeren bir başlangıç saatine eşdeğerdir. |
| Yineleme | Belirtilen dakika işaretleri (hiçbir başlangıç tarihi ve saati) her 15 dakikada bir çalıştırın. | 1 | Gün | {yok} | {kullanılamıyor} | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | 0, 15, 30, 45 | Bu zamanlama, bir sonraki belirtilen 15 dakikalık işareti ne kadar başlamaz. |
| Yineleme | Her gün 08:00'de *ve* mantık uygulamanızı kaydettiğinizde dakika işaretiyle çalıştırın | 1 | Gün | {yok} | {kullanılamıyor} | 8 | {yok} | Başlangıç tarihi ve saati olmadan, bu zamanlama mantık uygulamasını (PUT işlemi) kaydettiğiniz zamana bağlı olarak çalışır. |
| Yineleme | Her gün 08:00'de çalıştırın (başlangıç tarihi ve saati ile) | 1 | Gün | *başlangıç Tarihi* T08:00:00Z | {kullanılamıyor} | {yok} | {yok} | Bu zamanlama belirtilen başlangıç tarihi ve saatinden *önce* başlamaz. Gelecekteki olaylar her gün SABAH 8:00'de çalışır. | 
| Yineleme | Her gün 08:30'da çalıştırın (başlangıç tarihi ve saati yok) | 1 | Gün | {yok} | {kullanılamıyor} | 8 | 30 | Bu program her gün 8:30'da çalışır. |
| Yineleme | Her gün 08:30 ve 16:30'da çalıştırın | 1 | Gün | {yok} | {kullanılamıyor} | 8, 16 | 30 | |
| Yineleme | Her gün 08:30, 08:45, 16:30 ve 16:45'te çalıştırın | 1 | Gün | {yok} | {kullanılamıyor} | 8, 16 | 30, 45 | |
| Yineleme | Her Cumartesi saat 17:00'de çalıştırın (başlangıç tarihi ve saati yoktur) | 1 | Hafta | {yok} | "Cumartesi" | 17 | 00 | Bu program her Cumartesi saat 17:00'de çalışır. |
| Yineleme | Her Cumartesi saat 17:00'de çalıştırın (başlangıç tarihi ve saati ile) | 1 | Hafta | *başlangıç Tarihi* T17:00:00Z | "Cumartesi" | {yok} | {yok} | Bu zamanlama belirtilen başlangıç tarihi ve saatinden *önce* başlamaz, bu durumda 9 Eylül 2017 saat 17:00'de. Gelecekteki nüksler her Cumartesi saat 17:00'de başlar. |
| Yineleme | Her Salı, Perşembe saat 17:00'de *ve* mantık uygulamanızı kaydettiğinizde dakika işaretiyle çalıştırın| 1 | Hafta | {yok} | "Salı", "Perşembe" | 17 | {yok} | |
| Yineleme | Çalışma saatleri içinde her saat çalıştırın | 1 | Hafta | {yok} | Cumartesi ve Pazar hariç tüm günleri seçin. | Günün saatlerini istediğiniz i seçin. | İstediğiniz saatin herhangi bir dakikasını seçin. | Örneğin, çalışma saatleriniz 08:00-17:00 arasındaysa, günün saati olarak "8, 9, 10, 11, 12, 13, 14, 15, 16, 17" seçeneğini belirleyin. <p>Çalışma saatleriniz 08:30-17:30 arasındaysa, günün önceki saatlerini ve saatdakikası olarak "30"u seçin. |
| Yineleme | Hafta sonları her gün bir kez çalıştırın | 1 | Hafta | {yok} | "Cumartesi", "Pazar" | Günün saatlerini istediğiniz i seçin. | Uygun olarak saatin herhangi bir dakikasını seçin. | Bu program her Cumartesi ve Pazar günü belirtilen zamanlamada çalışır. |
| Yineleme | Sadece Pazartesi günleri haftada bir 15 dakikada bir çalıştırın | 2 | Hafta | {yok} | "Pazartesi" | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | 0, 15, 30, 45 | Bu program her pazartesi her 15 dakikada bir çalışır. |
| Yineleme | Her ay çalıştırın | 1 | Ay | *başlangıç Tarihi* T*başlangıçTime*Z | {kullanılamıyor} | {kullanılamıyor} | {kullanılamıyor} | Bu zamanlama belirtilen başlangıç tarihi ve saatinden *daha erken* başlamaz ve başlangıç tarihi ve saatinde gelecekteki yinelemeleri hesaplar. Bir başlangıç tarihi ve saati belirtmezseniz, bu zamanlama oluşturma tarihi ve saatini kullanır. |
| Yineleme | Ayda bir gün boyunca her saat çalıştırın | 1 | Ay | {bkz. not} | {kullanılamıyor} | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | {bkz. not} | Bir başlangıç tarihi ve saati belirtmezseniz, bu zamanlama oluşturma tarihi ve saatini kullanır. Yineleme zamanlaması için dakikaları denetlemek için saatin dakikalarını, başlangıç saatini belirtin veya oluşturma süresini kullanın. Örneğin, başlangıç saati veya oluşturma saati 8:25 ise, bu zamanlama 08:25, 09:25, 10:25 ve benzeri çalışır. |
|||||||||

<a name="run-once"></a>

## <a name="run-one-time-only"></a>Yalnızca bir kez çalıştırın

Mantık uygulamanızı gelecekte yalnızca bir defada çalıştırmak istiyorsanız, **Zamanlayıcı'yı kullanabilirsiniz: İş şablonunu bir kez çalıştırın.** Yeni bir mantık uygulaması oluşturduktan sonra ancak **Şablonlar** bölümünün altında, Kategori **listesinden** Mantık Uygulamaları Tasarımcısı'nı açmadan önce **Zamanlama'yı**seçin ve ardından şu şablonu seçin:

!["Zamanlayıcı: İş bir kere çalıştır" şablonu](./media/concepts-schedule-automated-recurring-tasks-workflows/choose-run-once-template.png)

Veya, bir **HTTP isteği aldığında** mantık uygulamanızı başlatabilirseniz - İstek tetikleyicisi ve başlangıç saatini tetikleyici için bir parametre olarak geçirin. İlk eylem için **Gecikme'yi** - Zamanlama eylemine kadar kullanın ve bir sonraki eylemin çalışmaya başladığı zamanı sağlayın.

## <a name="next-steps"></a>Sonraki adımlar

* [Yinelenen görevler ve iş akışları yineleme tetikleyicisi ile oluşturma, zamanlama ve çalıştırma](../connectors/connectors-native-recurrence.md)
* [Sürgülü Pencere tetikleyicisi ile yinelenen görevleri ve iş akışlarını oluşturma, zamanlama ve çalıştırma](../connectors/connectors-native-sliding-window.md)
* [Gecikme eylemleriyle iş akışlarını duraklatma](../connectors/connectors-native-delay.md)
