---
title: Azure Logic Apps yinelenen görevleri ve iş akışlarını zamanlama
description: Azure Logic Apps ile yinelenen otomatikleştirilmiş görevler, süreçler ve iş akışlarını zamanlamaya yönelik genel bakış
services: logic-apps
ms.suite: integration
ms.reviewer: deli, jonfan, logicappspm
ms.topic: conceptual
ms.date: 03/25/2020
ms.openlocfilehash: a5f01e81564561fe43ef6e55e6e9b3b67d6e1d77
ms.sourcegitcommit: 51977b63624dfd3b4f22fb9fe68761d26eed6824
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84945622"
---
# <a name="schedule-and-run-recurring-automated-tasks-processes-and-workflows-with-azure-logic-apps"></a>Azure Logic Apps ile yinelenen otomatik görevleri, işlemleri ve iş akışlarını zamanlama ve çalıştırma

Logic Apps, otomatikleştirilmiş yinelenen görevleri ve işlemleri bir zamanlamaya göre oluşturmanıza ve çalıştırmanıza yardımcı olur. Zamanlama türü Tetikleyicileri olan yerleşik bir yinelenme tetikleyicisi veya kayan pencere tetikleyicisi ile başlayan bir mantıksal uygulama iş akışı oluşturarak, görevleri daha sonra veya yinelenen bir zaman aralığında çalıştırabilirsiniz. HTTP veya HTTPS uç noktaları gibi Azure içindeki ve dışındaki hizmetleri çağırabilirsiniz, Azure depolama ve Azure Service Bus gibi Azure hizmetlerine iletiler gönderebilir veya bir dosya paylaşımında karşıya yüklenen dosyaları alabilirsiniz. Yineleme tetikleyicisiyle, görevleri çalıştırmak için karmaşık zamanlamalar ve gelişmiş tekrarlamalar da ayarlayabilirsiniz. Yerleşik zamanlama Tetikleyicileri ve eylemleri hakkında daha fazla bilgi edinmek için bkz. tetikleyici ve [zamanlama eylemlerini](#schedule-actions) [zamanlama](#schedule-triggers) . 

> [!TIP]
> Her zamanlanan iş için ayrı bir mantıksal uygulama oluşturmadan ve her bir [bölge ve abonelik için iş akışı sınırına](../logic-apps/logic-apps-limits-and-config.md#definition-limits)kadar çalışan yinelenen iş yüklerini zamanlayabilir ve çalıştırabilirsiniz. Bunun yerine, Azure hızlı başlangıç şablonu tarafından oluşturulan mantıksal uygulama modelini kullanabilirsiniz [: Logic Apps İş Zamanlayıcısı](https://github.com/Azure/azure-quickstart-templates/tree/master/301-logicapps-jobscheduler/).
>
> Logic Apps işi Zamanlayıcı şablonu, bir TimerJob mantıksal uygulaması çağıran bir CreateTimerJob mantıksal uygulaması oluşturur. Daha sonra, bir HTTP isteği yaparak ve istek için bir zamanlamayı giriş olarak geçirerek, CreateTimerJob mantıksal uygulamasını bir API olarak çağırabilirsiniz. CreateTimerJob mantıksal uygulamasına yapılan her çağrı, belirtilen zamanlamaya göre veya belirli bir sınıra kadar sürekli olarak çalışan yeni bir TimerJob örneği oluşturan TimerJob mantıksal uygulamasını da çağırır. Bu şekilde, örnekler ayrı ayrı mantıksal uygulama iş akışı tanımları veya kaynakları olmadığından, iş akışı sınırları hakkında endişelenmeden istediğiniz sayıda TimerJob örneği çalıştırabilirsiniz.

Bu liste, zamanlama yerleşik tetikleyicilerle çalıştırabileceğiniz bazı örnek görevleri gösterir:

* Her gün SQL saklı yordamını çalıştırma gibi iç verileri alın.

* NOAA 'den 15 dakikada bir çekme için hava durumu raporları gibi dış verileri alın.

* Son haftadaki belirli bir tutardan daha büyük bir Özet e-posta gibi rapor verileri gönderin.

* Bugünün karşıya yüklenen görüntülerini, yoğun olmayan saatlerde her gün içinde sıkıştırmak gibi işleme verileri.

* Üç aydan daha eski olan tüm verileri silme gibi verileri temizleyin.

* Sonraki dokuz ay boyunca her gün 1:00 ' de bir yedekleme hizmetine fatura gönderme gibi verileri arşivle.

Bir sonraki eylem çalıştırılmadan önce iş akışınızı duraklatmak için yerleşik eylemleri zamanlama ' yı da kullanabilirsiniz, örneğin:

* Bir iş gününü e-posta üzerinden bir durum güncelleştirmesi göndermek için bekleyin.

* Bir HTTP çağrısının, sonuca devam etmeden önce bitirime süresi olana kadar iş akışını geciktirin.

Bu makalede, zamanlama yerleşik Tetikleyicileri ve eylemleri için yetenekler açıklanır.

<a name="schedule-triggers"></a>

## <a name="schedule-triggers"></a>Zamanlama Tetikleyicileri

Mantıksal uygulama iş akışınızı, belirli bir hizmet veya sistemle ilişkili olmayan yineleme tetikleyicisi veya kayan pencere tetikleyicisi kullanarak başlatabilirsiniz. Bu Tetikleyiciler, gün, dakika, saat, gün, hafta veya ay sayısı gibi Aralık ve sıklığı seçtiğiniz zaman, belirttiğiniz yineleme temelinde iş akışınızı başlatır ve çalıştırır. Ayrıca, başlangıç tarihi ve saati ile saat dilimini de ayarlayabilirsiniz. Her tetikleyici tetiklendiğinde, Logic Apps mantıksal uygulamanız için yeni bir iş akışı örneği oluşturup çalıştırır.

Bu Tetikleyiciler arasındaki farklılıklar aşağıda verilmiştir:

* **Yinelenme**: iş akışınızı, belirtilen zamanlamanıza göre düzenli zaman aralıklarıyla çalıştırır. Örneğin, kesintiler veya devre dışı iş akışları nedeniyle Yinelenmeler kaçırıldığında, yinelenme tetikleyicisi atlanan tekrarları işlemez ancak sonraki zamanlanan aralıkla yinelenmeleri yeniden başlatır. Saat dilimini ve bir başlangıç tarihi ve saati de belirtebilirsiniz. "Gün" seçeneğini belirlerseniz saatin gün ve dakikalarının saatini (örneğin, her gün 2:30 ' de) belirtebilirsiniz. "Hafta" seçeneğini belirlerseniz, hafta içinde Çarşamba ve Cumartesi gibi günleri de seçebilirsiniz. Daha fazla bilgi için bkz. [yineleme tetikleyicisi ile yinelenen görevler ve iş akışları oluşturma, zamanlama ve çalıştırma](../connectors/connectors-native-recurrence.md).

* **Kayan pencere**: iş akışınızı sürekli öbeklerdeki verileri işleyen düzenli zaman aralıklarıyla çalıştırır. Örneğin, kesintiler veya devre dışı iş akışları nedeniyle Yinelenmeler kaçırıldığında, kayan pencere tetikleyicisi geri döner ve kaçırılan tekrarları işler. İş akışınızda her yinelemeyi geciktirmek için bir başlangıç tarihi ve saati, saat dilimi ve bir süre belirtebilirsiniz. Bu tetikleyici, örneğin günün belirli saatleri, saatin dakikası ve haftanın günleri gibi gelişmiş zamanlamaları desteklemez. Daha fazla bilgi için bkz. [kayan pencere tetikleyicisiyle yinelenen görevler ve iş akışları oluşturma, zamanlama ve çalıştırma](../connectors/connectors-native-sliding-window.md).

<a name="schedule-actions"></a>

## <a name="schedule-actions"></a>Zamanlama eylemleri

Mantıksal uygulama iş akışınızın herhangi bir eyleminden sonra, bir sonraki eylem çalıştırılmadan önce iş akışınızı beklemek için gecikme ve gecikme işlemlerini kullanabilirsiniz.

* **Gecikme**: saniye, dakika, saat, gün, hafta veya ay gibi belirtilen sayıda zaman birimi için sonraki eylemi çalıştırmayı bekleyin. Daha fazla bilgi için bkz. [iş akışlarında sonraki eylemi geciktirme](../connectors/connectors-native-delay.md).

* Şu **kadar geciktir**: belirtilen tarih ve saate kadar sonraki eylemi çalıştırmaya bekle. Daha fazla bilgi için bkz. [iş akışlarında sonraki eylemi geciktirme](../connectors/connectors-native-delay.md).

## <a name="patterns-for-start-date-and-time"></a>Başlangıç tarihi ve saati desenleri

<a name="start-time"></a>

İşte, başlangıç tarihi ve saatine sahip yinelemeyi nasıl denetleyebildiğiniz ve Logic Apps hizmetinin bu tekrarları nasıl çalıştığı gösterilmektedir:

| Başlangıç saati | Zamanlama olmadan yinelenme | Zamanlama ile yinelenme (yalnızca yineleme tetikleyicisi) |
|------------|-----------------------------|----------------------------------------------------|
| seçim | İlk iş yükünü anında çalıştırır. <p>Son çalışma zamanına göre gelecekteki iş yüklerini çalıştırır. | İlk iş yükünü anında çalıştırır. <p>Gelecekteki iş yüklerini belirtilen zamanlamaya göre çalıştırır. |
| Geçmişteki başlangıç zamanı | **Yinelenme** tetikleyicisi: belirtilen başlangıç zamanına göre çalışma zamanlarını hesaplar ve geçen çalışma zamanlarını atar. Sonraki gelecek çalışma zamanında ilk iş yükünü çalıştırır. <p>Son çalışma zamanından hesaplamalar temelinde gelecekteki iş yüklerini çalıştırır. <p><p>**Kayan pencere** tetikleyicisi: belirtilen başlangıç zamanına göre çalışma sürelerini hesaplar ve çalışma sürelerini geçmiş olarak hesaplar. <p>Gelecekteki iş yüklerini, belirtilen başlangıç zamanından hesaplamalar temelinde çalıştırır. <p><p>Daha fazla açıklama için bu tabloyu izleyen örneğe bakın. | Başlangıç zamanından hesaplanan zamanlamaya göre başlangıç zamanından daha önce *olmayan* ilk iş yükünü çalıştırır. <p>Gelecekteki iş yüklerini belirtilen zamanlamaya göre çalıştırır. <p>**Note:** Zamanlama ile bir yinelenme belirtirseniz, ancak zamanlama için saat veya dakika belirtmezseniz, gelecekteki çalışma süreleri sırasıyla, ilk çalışma zamanından itibaren saatler veya dakikalar kullanılarak hesaplanır. |
| Bugünkü veya gelecekteki başlangıç zamanı | Belirtilen başlangıç zamanında ilk iş yükünü çalıştırır. <p>Son çalışma zamanından hesaplamalar temelinde gelecekteki iş yüklerini çalıştırır. | Başlangıç zamanından hesaplanan zamanlamaya göre başlangıç zamanından daha önce *olmayan* ilk iş yükünü çalıştırır. <p>Gelecekteki iş yüklerini belirtilen zamanlamaya göre çalıştırır. <p>**Note:** Zamanlama ile bir yinelenme belirtirseniz, ancak zamanlama için saat veya dakika belirtmezseniz, gelecekteki çalışma süreleri sırasıyla, ilk çalışma zamanından itibaren saatler veya dakikalar kullanılarak hesaplanır. |
||||

> [!IMPORTANT]
> Yinelenmeler Gelişmiş zamanlama seçenekleri belirtmezseniz, gelecekteki Yinelenmeler son çalışma zamanına göre yapılır.
> Bu Yinelenmeler için başlangıç zamanları, depolama aramaları sırasında gecikme süresi gibi faktörlerden dolayı yetersiz kalabilir. Mantıksal uygulamanızın bir yinelemeyi kaçırmayın, özellikle sıklık gün veya daha uzun bir süre içinde olduğundan emin olmak için şu seçeneklerden birini kullanın:
> 
> * Yinelenme için bir başlangıç saati belirtin.
> 
> * **Bu saatlerde** ve **Bu dakika** özelliklerinde kullanarak yinelenme ne zaman çalıştırılacağı saat ve dakika sayısını belirtin.
> 
> * Yineleme tetikleyicisi yerine [kayan pencere tetikleyicisini](../connectors/connectors-native-sliding-window.md)kullanın.

*Son başlangıç zamanı ve yinelenme örneği, ancak zamanlama yok*

Geçerli tarih ve saatin 8 Eylül 2017 ile 1:00 PM arasında olduğunu varsayalım. Başlangıç tarihini ve saatini 7 Eylül 2017 itibariyle, geçmişte olan 2:00 PM ve her iki günde bir çalışan bir yinelenme olarak belirtirsiniz.

| Başlangıç saati | Geçerli saat | Yineleme | Zamanla |
|------------|--------------|------------|----------|
| 2017-09-**07**T14:00:00Z <br>(2017-09-**07** saat 2:00 PM) | 2017-09-**08**T13:00:00Z <br>(2017-09-**08** , 1:00 PM) | İki günde bir | seçim |
|||||

Yineleme tetikleyicisi için, Logic Apps Altyapısı başlangıç zamanına göre çalışma sürelerini hesaplar, son çalıştırma sürelerini atar, ilk çalıştırmanın sonraki gelecek başlangıç zamanını kullanır ve son çalışma zamanına göre gelecekteki çalıştırmaları hesaplar.

Bu yinelenme şöyle görünür:

| Başlangıç saati | İlk çalıştırma zamanı | Gelecekteki çalışma süreleri |
|------------|----------------|------------------|
| 2017-09-**07** saat 2:00 PM | 2017-09-**09** 2:00 PM | 2017-09-**11** 2:00 PM </br>2017-09-**13** 2:00 PM </br>2017-09-**15** 2:00 PM </br>vb... |
||||

Bu nedenle, geçmişteki bir süre içinde başlangıç saatini ne kadar belirttiğinize bakılmaksızın 2:00 2017-09 2:00 2017-09, ilk**05** çalıştırana bilgisayarınız her zaman sonraki gelecek**01** başlangıç saatini kullanır.

Kayan pencere tetikleyicisi için, Logic Apps Altyapısı başlangıç zamanına göre çalışma zamanlarını hesaplar, geçen çalışma süreleri, ilk çalıştırmanın başlangıç saatini kullanır ve gelecekteki çalıştırmaları başlangıç saatine göre hesaplar.

Bu yinelenme şöyle görünür:

| Başlangıç saati | İlk çalıştırma zamanı | Gelecekteki çalışma süreleri |
|------------|----------------|------------------|
| 2017-09-**07** saat 2:00 PM | 2017-09-**07** saat 2:00 PM | 2017-09-**09** 2:00 PM </br>2017-09-**11** 2:00 PM </br>2017-09-**13** 2:00 PM </br>2017-09-**15** 2:00 PM </br>vb... |
||||

Bu nedenle, başlangıç saatini ne kadar belirttiğinize bakılmaksızın, örneğin, 2017-09-**05** , 2:00 pm veya 2017-09-**01 2:00 tarihleri** arasında her zaman belirtilen başlangıç saatini kullanır.

<a name="example-recurrences"></a>

## <a name="example-recurrences"></a>Örnek Yinelenmeler

Seçenekleri destekleyen Tetikleyiciler için ayarlayabileceğiniz çeşitli örnek tekrarları aşağıda verilmiştir:

| Tetikleyici | Yineleme | Interval | Sıklık | Başlangıç saati | Şu günlerde | Şu saatlerde | Şu dakikalarda | Not |
|---------|------------|----------|-----------|------------|---------------|----------------|------------------|------|
| Yinelemeyi <br>Kayan Pencere | Her 15 dakikada bir Çalıştır (başlangıç tarihi ve saati olmadan) | 15 | Dakika | seçim | mıyor | seçim | seçim | Bu zamanlama hemen başlatılır, ardından son çalışma zamanına göre gelecekteki tekrarları hesaplar. |
| Yinelemeyi <br>Kayan Pencere | Her 15 dakikada bir Çalıştır (başlangıç tarihi ve saati ile) | 15 | Dakika | *StartDate* T*StartTime*Z | mıyor | seçim | seçim | Bu zamanlama, belirtilen başlangıç tarihi ve saatinden daha *önce* başlamaz, son çalışma zamanına göre gelecekteki tekrarları hesaplar. |
| Yinelemeyi <br>Kayan Pencere | Saatte (başlangıç tarihi ve saati ile) her saatte bir Çalıştır | 1 | Saat | *StartDate* THH: 00:00Z | mıyor | seçim | seçim | Bu zamanlama, belirtilen başlangıç tarihi ve saatinden daha *önce* başlamıyor. Gelecekteki Yinelenmeler, başlangıç zamanından hesaplanan "00" dakika işaretiyle her saat çalışır. <p>Sıklık "Week" veya "month" ise bu zamanlama, her ay yalnızca bir gün veya ayda bir gün çalışır. |
| Yinelemeyi <br>Kayan Pencere | Her gün (başlangıç tarihi ve saati olmadan) her saat Çalıştır | 1 | Saat | seçim | mıyor | seçim | seçim | Bu zamanlama hemen başlar ve son çalışma zamanına göre gelecekteki tekrarları hesaplar. <p>Sıklık "Week" veya "month" ise bu zamanlama, her ay yalnızca bir gün veya ayda bir gün çalışır. |
| Yinelemeyi <br>Kayan Pencere | Her gün (başlangıç tarihi ve saati ile) her saatte bir Çalıştır | 1 | Saat | *StartDate* T*StartTime*Z | mıyor | seçim | seçim | Bu zamanlama, belirtilen başlangıç tarihi ve saatinden daha *önce* başlamaz, son çalışma zamanına göre gelecekteki tekrarları hesaplar. <p>Sıklık "Week" veya "month" ise bu zamanlama, her ay yalnızca bir gün veya ayda bir gün çalışır. |
| Yinelemeyi <br>Kayan Pencere | Saatte bir saat sonra her 15 dakikada bir Çalıştır (başlangıç tarihi ve saati ile) | 1 | Saat | *StartDate* T00:15:00Z | mıyor | seçim | seçim | Bu zamanlama, belirtilen başlangıç tarihi ve saatinden daha *önce* başlamıyor. Gelecekteki Yinelenmeler, başlangıç zamanından hesaplanan "15" dakika işaretiyle çalışır, bu nedenle 00:15, 1:15, 2:15, ve bu şekilde devam eder. |
| Yineleme | Saatte bir saat sonra her 15 dakikada bir Çalıştır (başlangıç tarihi ve saati olmadan) | 1 | Gün | seçim | mıyor | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | 15 | Bu zamanlama 00:15, 1:15, 2:15, vb. olarak çalışır. Ayrıca, bu zamanlama "saat" sıklığıyla "15" dakikalık bir başlangıç zamanına eşittir. |
| Yineleme | Belirtilen dakika işaretlerinde 15 dakikada bir Çalıştır (başlangıç tarihi ve saati yok). | 1 | Gün | seçim | mıyor | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | 0, 15, 30, 45 | Bu zamanlama sonraki belirtilen 15 dakikalık işaretine kadar başlamaz. |
| Yineleme | Mantıksal uygulamanızı kaydettiğinizde her gün 8 *saat ve dakika* olarak çalıştır | 1 | Gün | seçim | mıyor | 8 | seçim | Başlangıç tarihi ve saati olmadan, bu zamanlama mantıksal uygulamayı kaydettiğinizde (PUT işlemi) zamana göre çalışır. |
| Yineleme | Günlük Çalıştır: 8:00 (başlangıç tarihi ve saati ile) | 1 | Gün | *StartDate* T08:00:00Z | mıyor | seçim | seçim | Bu zamanlama, belirtilen başlangıç tarihi ve saatinden daha *önce* başlamıyor. Gelecek oluşumların saati 8:00 ' de günde bir kez çalıştırılır. | 
| Yineleme | Günlük Çalıştır: 8:00 (başlangıç tarihi ve saati yok) | 1 | Gün | seçim | mıyor | 8 | 00 | Bu zamanlama her gün 8:00 ' de çalışır. |
| Yineleme | Günlük Çalıştır: 8:00 ve 4:00 PM | 1 | Gün | seçim | mıyor | 8, 16 | 0 | |
| Yineleme | Günde 8:30, 8:45, 4:30 ve 4:45 saat olarak çalıştır | 1 | Gün | seçim | mıyor | 8, 16 | 30, 45 | |
| Yineleme | Her Cumartesi 5:00 saat (başlangıç tarihi ve saati olmadan) olarak çalıştır | 1 | Hafta | seçim | Günü | 17 | 0 | Bu zamanlama, her Cumartesi 5:00: ' de çalışır. |
| Yineleme | Her Cumartesi 5:00 saat (başlangıç tarihi ve saati) ile Çalıştır | 1 | Hafta | *StartDate* T17:00:00Z | Günü | seçim | seçim | Bu zamanlama, belirtilen başlangıç tarihi ve saatinden daha *önce* başlatılmaz, bu durumda 9 Eylül 2017, 5:00 PM. Gelecekteki Yinelenmeler, her Cumartesi 5:00 PM 'de çalışır. |
| Yineleme | Her Salı günü çalıştırın, mantıksal uygulamanızı kaydettiğinizde 5 ve *sonra da dakika* olarak Perşembe| 1 | Hafta | seçim | "Salı", "Perşembe" | 17 | seçim | |
| Yineleme | Çalışma saatleri boyunca her saat çalıştırın. | 1 | Hafta | seçim | Cumartesi ve Pazar hariç tüm günleri seçin. | İstediğiniz günün saatini seçin. | İstediğiniz saatin dakikasını seçin. | Örneğin, çalışma saatleriniz 8:00 ila 5:00 PM ise *, günün saatleri ile "0* " saat olarak "8, 9, 10, 11, 12, 13, 14, 15, 16, 17" öğesini seçin. |
| Yineleme | Her gün hafta sonları üzerinde Çalıştır | 1 | Hafta | seçim | "Cumartesi", "Pazar" | İstediğiniz günün saatini seçin. | Uygun şekilde saatin dakikasını seçin. | Bu zamanlama, belirtilen zamanlamaya göre her Cumartesi ve Pazar günü çalışır. |
| Yineleme | Her 15 dakikada bir iki haftada bir Çalıştır yalnızca Mondays | 2 | Hafta | seçim | Pazartesi | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | 0, 15, 30, 45 | Bu zamanlama her 15 dakikalık işarette Pazartesi günü çalışır. |
| Yineleme | Her ay Çalıştır | 1 | Ay | *StartDate* T*StartTime*Z | mıyor | mıyor | mıyor | Bu zamanlama, belirtilen başlangıç tarihi ve saatinden daha *önce* başlatılmaz ve başlangıç tarihi ve saatine göre gelecekteki tekrarları hesaplar. Bir başlangıç tarihi ve saati belirtmezseniz, bu zamanlama Oluşturulma tarihi ve saatini kullanır. |
| Yineleme | Ayda bir gün boyunca her saat Çalıştır | 1 | Ay | {bkz. Note} | mıyor | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | {bkz. Note} | Bir başlangıç tarihi ve saati belirtmezseniz, bu zamanlama Oluşturulma tarihi ve saatini kullanır. Yinelenme zamanlamasının dakikalarını denetlemek için saatin dakikalarını, başlangıç zamanını belirtin veya oluşturma süresini kullanın. Örneğin, başlangıç saati veya oluşturma saati 8:25 ise, bu zamanlama 8:25:, 9:25, 10:25, vb. ' de çalışır. |
|||||||||

<a name="run-once"></a>

## <a name="run-one-time-only"></a>Yalnızca bir kez çalıştır

Mantıksal uygulamanızı gelecekte tek seferde çalıştırmak istiyorsanız **Zamanlayıcı: bir kez çalıştır işleri** şablonunu kullanabilirsiniz. Yeni bir mantıksal uygulama oluşturduktan sonra, Logic Apps tasarımcısını açmadan önce, **Şablonlar** bölümünde, **Kategori** listesinden **zamanlama**' yı seçin ve ardından şu şablonu seçin:

!["Zamanlayıcı: bir kez çalıştır işleri" şablonunu seçin](./media/concepts-schedule-automated-recurring-tasks-workflows/choose-run-once-template.png)

Ya da, **BIR http isteği alındığında** mantıksal uygulamanızı ile başlatabilir ve tetikleyici için bir parametre olarak başlangıç saatini geçitirsiniz. İlk eylem için **geciktir-Schedule** eylemini kullanın ve sonraki eylemin çalışmaya başladığı zamanı belirtin.

## <a name="next-steps"></a>Sonraki adımlar

* [Yineleme tetikleyicisiyle yinelenen görevleri ve iş akışlarını oluşturma, zamanlama ve çalıştırma](../connectors/connectors-native-recurrence.md)
* [Kayan pencere tetikleyicisi ile yinelenen görevler ve iş akışları oluşturma, zamanlama ve çalıştırma](../connectors/connectors-native-sliding-window.md)
* [Gecikme eylemleriyle iş akışlarını duraklatma](../connectors/connectors-native-delay.md)
