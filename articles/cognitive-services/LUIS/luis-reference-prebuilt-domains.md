---
title: Önceden oluşturulmuş etki alanı başvurusu-LUSıS
titleSuffix: Azure Cognitive Services
description: Önceden oluşturulmuş koleksiyonları hedefleri ve varlıkların gelen Language Understanding Intelligent Services (LUIS) önceden oluşturulmuş etki alanları için başvuru.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: diberry
ms.openlocfilehash: f5810a813b6c54f190d95061e79914457f51d19c
ms.sourcegitcommit: f209d0dd13f533aadab8e15ac66389de802c581b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71067607"
---
# <a name="prebuilt-domain-reference-for-your-luis-app"></a>LUSıS uygulamanız için önceden oluşturulmuş etki alanı başvurusu
Bu başvuru, hakkında bilgi sağlar. [önceden oluşturulmuş etki alanları](luis-how-to-use-prebuilt-domains.md), önceden oluşturulmuş koleksiyon hedefleri ve LUIS sunan varlıkların olduğu.

[Özel etki alanları](luis-how-to-start-new-app.md), aksine, hiçbir hedefleri ve modelleri başlayın. Herhangi bir önceden oluşturulmuş etki alanı hedefleri ve varlıklar için özel bir model ekleyebilirsiniz.

# <a name="supported-domains-across-cultures"></a>Kültürler genelinde desteklenen etki alanları

Aşağıdaki tabloda, şu anda desteklenen etki alanları özetlenmektedir. Ingilizce desteği genellikle diğerlerinden daha tamamdır. 

| Varlık türü       | EN-US      | ZH-CN   | DE    | GS     | ES    | BT      | PT-BR |  JP  |      DILI |        NL |    TR |
|:-----------------:|:-------:|:-------:|:-----:|:------:|:-----:|:-------:| :-------:| :-------:| :-------:| :-------:|  :-------:| 
| [Takvim](#calendar)    | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| [Kurulan](#communication)   | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| [E-posta](#email)           | ✓    | ✓       | ✓   | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| [HomeAutomation](#homeautomation)           | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| [Notlar](#notes)      | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| [Yer](#places)    | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| [Restoran Antreservatıon](#restaurantreservation)   | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| [ToDo](#todo)     | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| [Programların](#utilities)          | ✓    | ✓        | ✓    | ✓      | ✓     | ✓       | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| [Gü](#weather)        | ✓    | ✓        | ✓    | ✓      | ✓     | ✓       | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| [Web](#web)    | ✓    | ✓        | ✓    | ✓      | ✓     | ✓       | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |

Önceden oluşturulmuş etki alanları ' de **desteklenmez** :

* Kanada Fransızcası
* Hintçe
* İspanyolca Meksika

# <a name="description-for-luis-prebuilt-domains"></a>LUIS önceden oluşturulmuş etki alanları için açıklama
## <a name="calendar"></a>**Takvim**
Takvim, kişisel toplantılar ve randevular (örneğin, dünya Kupa zamanlamaları, Seattle olay takvimi) veya genel takvim (örneğin, bugün ne güne denk gelir, Işçilik günü ne zaman başlar) hakkında herhangi bir şeydir.
### <a name="intents"></a>**Hedefleri**
Amaç adı | Açıklama | Örnekler
---------|----------|---------------
 AcceptEventEntry | Takvimdeki (n) bir randevuyu/toplantıyı/olayı kabul edin. | Bir randevuyu kabul edin. <br> Olayı kabul et <br> Bugünün toplantısını kabul edin.
 İptal | Bir toplantı oluşturma işlemini iptal etme gibi, sanal yardım ile devam eden işlemi iptal edin. <br> _**Uyarı**: Bu amaç genellikle takvim senaryosunda "Iptal" eylemini içerir. "Iptal" üzerinde genel ifadeye ihtiyacınız varsa, lütfen **yardımcı programlar** etki alanındaki "iptal" hedefini kullanın._ | Tamam, yalnızca olayı iptal et. <br> Hayır, yalnızca randevuyu iptal ediyorum.
 ChangeCalendarEntry | Takvim girişini değiştirin veya yeniden zamanlayın. | 6 ' yı yeniden zamanla Yarın-2 p.m. arası randevu <br> Doktor randevusu 5 PM için Yeniden Çizelgele <br> Jenny Olson ile Cuma günleri yeniden zamanlayın. <br> Olay saatini değiştir.
 CheckAvailability | Kullanılabilirlik randevu veya kullanıcının takvim veya başka bir kişinin Takvim toplantı bulun. | Jim karşılamak kullanılabilir olduğunda? <br> Carol 'ın yarın ne zaman kullanılabilir olduğunu gösterir. <br> Chris Cumartesi günleri ücretsiz mi?
 Onayla | Önceki amaca göre bir işlem/eylem yapılıp yapılmayacağını onaylayın. <br> _**Uyarı**: Bu amaç genellikle takvim senaryosu için "Onayla" eylemini içerir. "Onayla" üzerinde daha fazla genel ifadeye ihtiyacınız varsa, lütfen **yardımcı programlar** etki alanında "Onayla" hedefini kullanın._| Bu doğru, lütfen toplantıyı oluşturun <br> Evet, teşekkürler, toplantıya Bağlan.
 ConnectToMeeting | Bir toplantıya bağlanın. | Andy ile 11:00 konferans çağrısına beni bağlayın. <br> Bütçe toplantısı çağrısını kabul edin.
 Contactmeeting ınkatılımcılar | Toplantı katılımcıları ile iletişim kurun. | Toplantıya geç 3:00 toplantısını çalıştırdığımı söyleyin. <br> İş arkadaşlarınıza, 8:30 ' de başlaması gereken 8 toplantı toplantısı için bildirim gönderin.
 CreateCalendarEntry | Yeni bir kerelik öğe takviminize ekleyin. | Sorunları ele alarak bir toplantı oluşturun. <br> ile bir toplantı oluşturunabc@microsoft.com
 DeleteCalendarEntry | Bir takvime girişi silme isteği.| Carol ile My randevu iptal edin. <br> 9\. sabah Sil toplantısını. <br> Olayımı Sil.
  FindCalendarEntry | Takvim uygulamasını açın veya takvim girişinde arama yapın. | Bul diş randevu gözden geçirin. <br> Takvimimi göster. <br> Takvimlerimin Perşembe günü ne?
 Findcalendarne zaman | Zamanlamanın gerçekleştiği saati denetleyin. | Bu ve Çiğdem ile ne zaman buluşmalıyım? <br> Bir brunme ne zaman zamanlandı? 
 FindCalendarWhere | Zamanlamanın gerçekleştiği yeri kontrol edin. | Randevularım nerede yarın? <br>Akşam yemeği için yarın ve Michael yarın ile nerede karşılaşmalıyım?
  Findcalendarkim | Hedef zamanlamaya katılacak katılımcı (ler) i kontrol edin. | 2 ' de yarın 'nin toplantısının risk altında olduğunu onaylıyorum. <br> Emre, sonraki yeni bir toplantıda mi olacak?
 FindCalendarDetail | Zamanlamanın ayrıntılarını denetleyin ve görüntüleyin. | İş arkadaşımın bulunduğu toplantının ayrıntılarını bana sağlamanız gerekiyor.
 FindDuration | Süreyi denetleyin. | Market 'i ne kadar zaman çekmem gerekir? <br> Öğle yemeği ne kadar süreyle var?
 Findmeeting Inodi | Kullanılabilir toplantı odalarını bulun. | Odalara uydum? <br> Yeni bir toplantı konumu bulun.
 GoBack | Son adıma veya öğeye geri dönün.  <br> _**Uyarı**: Daha fazla GoBack genel atıfta bulunmak için lütfen **yardımcı programlar** etki alanına bakın._ | Önceki bir <br> Son e-postaya geri dönün.
 Reddet | Kullanıcı sanal yardımcıyı önerdiğini reddeder. <br> _**Uyarı**: Daha fazla reddetme genel kullanım için lütfen toC **yardımcı programları** etki alanına bakın._ | Olayı ayarlama gereksinimi yoktur. <br> Bu sırada yapacağım başka şeyler var.
ShowNext | Sonraki olayı denetleyin. <br> _**Uyarı**: Daha fazla ShowNext genel kullanım için lütfen **yardımcı programlar** etki alanına bakın._ | Bana bir sonraki olayımı ver. <br> Takvimde ileri git?
 ShowPrevious | Önceki olayı denetleyin. <br> _**Uyarı**: Daha fazla ShowPrevious genel kullanım için lütfen **yardımcı programlar** etki alanına bakın._ | Zamanlama ne kadar önce?
 TimeRemaining | Sonraki olaya kadar kalan süreyi kontrol edin. | Toplantılarıma ne kadar süre sonra ihtiyacım olduğunu görüntüleyin. <br> Sonraki toplantım başlamadan önce sahip olduğumu gösteren süreyi görüntüleyin.
 
### <a name="entities"></a>**Varlıklar**
Varlık Adı | Varlık türü | Açıklama | Örnekler | Yuvalar
-------|-----------------------|-------------|---------|--------
ContactName | kişi adı | Bir kişinin veya toplantı katılımcısının adı. | **Betsy**ile tanışın. <br>  3 Temmuz 'da 7 p.m. üzerinde **Aubrey** ile tanışın | Betsy <br> Aubrey <br> Amy 
Hedef takvim | MPLE | Hedef takvim adı. | MOM ile öğle yemeği 12 **Kişisel** Salı <br> **Google** takvimimi varsayılan takvimim olarak kullan. <br> Yoga sınıfını 3 p.m. ' de Mon 'a Güncelleştir **Kişisel** takvimdeki liste. | Google <br> C <br> başlar <br> Ana
Duration | datetime | Toplantý, randevunun veya kalan sürenin süresi. | **20 dakika**boyunca 00 ' da yarın Scholarship ayrıntılarını tartışmak için Gary ile birlikte iş tavder toplantısına ekleyin. <br> 9 p.m. ' de **bir saat** boyunca Thomas ile bir etkinlik olacak şekilde takvime kısa bir olay ekleyin | bir saat <br> 2 gün <br> 20 dakika 
EndDate | datetime | Bir toplantının veya randevunun bitiş tarihi. | Takvim bas salonu, **Mary 5** ' te konser ekleyin | Mary 5  
EndTime | datetime | Toplantı veya randevunun bitiş saati. | Bunu **üç** ile 2 30 yapabilirsiniz | üç 
Location | MPLE | Takvim öğesi, toplantı veya randevu konumu.  Adres, şehir ve bölgeler konumları, iyi örneklerdir. | bir toplantıyı Burma 'ya yerleştirmek için **Fremont** 'ye bir toplantı koy <br> **Edina** 'de Pro Bono toplantısı | 209 Nashville uygulamaları <br> 897 Krep Merkezi <br> İş dışındaki vakitlerimi 
Toplantı odası | MPLE | Toplantý veya randevunun odasıdır. | 2 p.m. ' de Jake ile iş takvimi toplantısına ekleyin **Office** bu Cuma günü | BT ofisi <br> Konferans odası <br> Oda 2
MoveEarlierTimeSpan | datetime | Kullanıcı bir toplantıyı veya randevuyu daha önce taşımak istiyor. | Öğle yemeği tarihini **30 dakika**sonrasına taşıyın. | 30 dakika <br> iki saat 
Moveyan zaman aralığı |  datetime | Kullanıcı bir toplantıyı veya randevuyu daha sonra taşımak istiyor. | ORCHID kutusuyla **4 saat**geri göndererek toplantımı gönderin. | 4 saat <br> 15 dakika 
OrderReference | MPLE | Listesini almak için bir öğeyi tanımlamak, sıralı ya da göreli konumu. | Yarın için sonraki randevum ne var? | sonraki
OriginalEndDate | datetime | Bir toplantının veya randevunun orijinal bitiş tarihi. | Tatillerimin, **Cuma** günü Pazartesi gününe kadar olacak şekilde değiştirin | Cuma 
OriginalEndTime | datetime | Toplantının veya randevunun orijinal bitiş saati. | 4 ' e kadar olan **3** ' e kadar bir bitiş yapın | 3
OriginalStartDate | datetime | Bir toplantının veya randevunun başlangıçtaki başlangıç tarihi. | **Yarının**randevusunu 10 ' dan değiştirin 9 ' da saat 10:00'da  | yarın 
OriginalStartTime | datetime | Toplantının veya randevunun orijinal başlangıç saati. | Yarının randevusunu **10** ' dan değiştirin 9 ' da saat 10:00'da | 10 saat
PositionReference | ordinal | Bir listedeki mutlak konum, alınacak bir öğeyi tanımlar. | **İkinci** bir | second <br> Hayır. 3 <br> 5\. sayı
RelationshipName | list | Bir kişinin ilişki adı. | 1:00 P.M. ' ye öğle yemeği ekleyin **kadın** ile | Eşim <br> koca <br> Sister 
SlotAttribute | MPLE | Özniteliği Kullanıcı sorgulamak veya düzenlemek istiyor. | olay **konumunu** Değiştir <br> **saati** yedi saat olarak değiştirin | location <br> time 
Başlangıç | datetime | Bir toplantının veya randevunun başlangıç tarihi. | **Çarşamba** günü 4 p.m. ' de bir toplantı oluşturun | Çarşamba 
StartTime | datetime | Toplantının veya randevunun başlangıç saati. | Çarşamba günü **4 p.m.** ' de bir toplantı oluşturun | 4 p.m.
Subject | basit, model. Kaydedilmemiş | Bir toplantının veya randevunun başlığı gibi. | **Parti hazırlığı** toplantısı ne zaman? | Dentist 'ın <br> Öğle yemeği Julia ile 
`Message` | basit, model. Kaydedilmemiş | Katılanlara göndereceğiniz ileti. | Akşam yemeği toplantısı 'nın **geç**olacağı uyarı katılımcıları. | Geç kalacağım

## <a name="communication"></a>**Kurulan**
Çağrı yapma, metin/anlık mesaj gönderme, kişileri bulma/ekleme ve diğer iletişimle ilgili istekler (genellikle giden) istekleri. _Kişi adı yalnızca_ , iletişim etki alanına ait değildir.

### <a name="intents"></a>**Hedefleri**
Amaç adı | Açıklama | Örnekler
---------|----------|---------
AddContact | Yeni bir kişi, kullanıcının kişiler listesine ekleyin. | Yeni kişi ekle.  <br>   Bu numarayı kaydedin ve adı Gamze olarak koyun.
AddFlag | E-postaya bayrak Ekle | Bu e-postayı işaretle <br> Bu e-postaya bayrak ekleyin.
AddMore | Daha fazla e-posta veya metin tarafınızdaki e-posta veya metin birleşimin parçası ekleyin. | Metne daha fazla metin ekleyin.  <br>   E-posta gövdesine daha fazla bilgi ekleyin.
Yanıt | Gelen telefon aramasını yanıtlayın. | Çağrıyı yanıtlayın.  <br>   Bunu seçin.
AssignContactNickname | Bir takma ad, bir kişi için atayın. | Isaac 'yi babacığım olarak değiştirin.  <br>   Jim 'in takma adı 'nı düzenleyin. <br>   PATTI Owleştirir 'a takma ad ekleyin.
CallBack | Telefon araması döndürün. | Geri arama yapın.
CallVoiceMail | İçin kullanıcının sesli posta bağlanın. | Beni sesli bana bağla kutusu. <br>Sesli posta. <br>   Sesli mesaj arayın.
İptal | Bir işlemi iptal edin. | İptal. <br>   Sonlandırın.
CheckIMStatus | Im 'deki bir kişinin durumunu denetleyin. | Can'ın çevrimiçi durumu hemen ayarlanır? 
CheckMessages | Yeni iletileri veya e-postaları denetleyin. | Gelen kutumu denetle <br>  Yeni bir e-posta mı var?
Onayla | Bir eylemi onaylayın. | Evet, gönder. <br> Sağ, bu e-postayı göndermek istediğim olduğunu onaylayın.
EndCall | Telefon aramasını sonlandır. | Çağrıyı kapatın. <br> Sonlandırın.
FindContact | Ada göre iletişim bilgilerini bulabilir. | Mum 'in numarasını bulun. <br>   Carol 'nun numarasını göster.
FinishTask | Geçerli görevi sona erdirin. | İşim bitti <br> Hepsi bu.
TurnForwardingOff | Çağrı iletme devre dışı bırakın. | Çağrılarımı iletmeyi durdur. <br> Çağrı iletmeyi kapatma.
TurnForwardingOn | Arama iletmeyi açın. | Çağrılarım 3333 için iletme <br>  Çağrı iletme 3333 ' ye geçin.
GetForwardingsStatus | Çağrı iletme geçerli durumunu alın. | My çağrı iletme açık mı? <br>   Çağrı durumumun açık veya kapalı olduğunu bana bildirin.
GetNotifications | Bildirimleri alın. | bildirimlerimi aç <br>   Telefonumu bildirimleri kontrol edebilir
GoBack | Önceki adıma geri dönün. | Twitter dönün <br>   Bir adıma geri dönün <br>   Geri git
Yoksayma | Gelen bir arama yoksayın. | Yanıt yok <br>   Çağrı yoksay
IgnoreWithMessage | Gelen bir arama yoksay ve bunun yerine metin ile yanıtla. | Bu aramayı yanıtlamalı yoktur ancak bir ileti gönderin. <br>   Yoksay ve bir metin geri gönderin.
Arama | Telefon araması yapın. | Jim çağırın <br>   Lütfen 311 çevirin.
FindSpeedDial | Bir telefon numarası için ve tersi ayarlamak hızlı arama numarası bulun. | Arama numaramı 5 nedir? <br>   Kümesi çevirmek hızı var mı? <br>   941 5555 333 arama numaralı nedir?
İlet | E-postayı ilet | Bu e-postayı Greg adresine iletin.
ReadAloud | Bir mesaj veya e-posta kullanıcıya okuyun. | Metni okuyun. <br>   Hangi Filiz iletisinde konuları?
PressKey | Bir düğme veya sayı tuş takımında tuşuna basın. | Arama yıldız. <br>   1 2 3'e basın.
QueryLastText | Son metni veya iletiyi sorgulayın. | Beni kim verdi? <br>   Son zamanlarda bana kim gönderdim?
Arama | Arayın veya bir sayı yeniden çağırın. | Arayın. <br>   Benim son çağrı arayın.
Reddet | Bir gelen çağrıyı reddeder. | Çağrı Reddet <br>   Artık yanıt bulamadığınız <br>   Şu anda kullanılamıyor ve daha sonra geri çağırır.
Yanıtla | İleti yanıtlayın. | Lore Hound 'e yanıt verme <br>   My Me yazarken yanıtla
SearchMessages | İletileri bazı koşullara göre arayın. | Gamze tarafından gönderilen e-postalarımı arayabilir miyim?
SendEmail | E-posta gönderin. Bu amaç için e-posta ancak metin iletileri için geçerlidir. | Emre Waters e-posta: Bu akşam yemeği geçen hafta SPLENDID idi. <br>   Emre 'ye e-posta gönderin.
SendMessage | SMS mesajı ya da anlık ileti gönderin. | Chris ve Carol metin Gönder <br>   Facebook iletisi Greg <br>   
SetSpeedDial | Kişinin telefon numarası için hızlı arama kısayol ayarlayın. | Carol için hızlı arama birini ayarlayın. <br>   Mom için hızlı arama ayarlayın.
ShowCurrentNotification | Geçerli bildirimleri göster. | Yeni bildirim var mı? <br> Bir bildirim göster.
ShowNext | Sonraki öğeyi, örneğin, bir kısa mesaj veya e-postaları listesinde bakın. | Sonrakini Göster. <br>   Bir sonraki sayfasına gidin.
ShowPrevious | Önceki öğeyi, örneğin, bir kısa mesaj veya e-postaları listesinde görürsünüz. | Öncekine gösterir. <br>   Öncekini. <br>   Önceki gidin.
TurnSpeakerOff | Konuşmacı telefonu kapatın. | Bana hoparlör kapalı yararlanın. <br>   Hoparlör devre dışı bırakın.
TurnSpeakerOn | Konuşmacı telefonda açın. | Hoparlör modu. <br>   Hoparlör koyabilirsiniz.

### <a name="entities"></a>**Varlıklar**
Varlık Adı | Varlık türü | Açıklama | Örnekler | Yuvalar
------|-------|----------|--------------|---------------
Attachment | MPLE | Kullanıcının metin veya e-posta ile göndermek istediği ek. | OneNote 'tan bir **dosyayı** e-posta ile gönderin. <br> Evlerimin evmi olduğunu bir kaya gönderin. | dosyası <br> Belg
AudioDeviceType | MPLE | Ses cihazının türü (konuşmacı, kulaklık, mikrofon vb.). | **Eller ücretsiz**kullanarak yanıtlayın. <br> **Konuşmacı telefonunda**yeniden arayın. | sesini <br> Eller ücretsiz <br> Bluetooth
Category | MPLE | Bir ileti veya e-postanın kategorisi, Kategori "okunmamış", "bayrak" gibi e-posta sisteminde açık bir tanıma sahip olmalıdır. Açıklama w/o Clear Definition, örneğin, "New" ve "son" Kategoriler değildir. | Tüm e-postaları **okundu** olarak işaretle  <br> Paul için yeni **yüksek öncelikli** e-posta | önemli <br> yüksek öncelikli <br> oku
ContactAttribute | MPLE | Kullanıcı ile ilgili olarak iletişim kurmak için bir özniteliği.| Sonraki ayın **Doğum günlerini** öğrenmem gerekir mi? | günleri <br> address <br> Telefon numarası
ContactName | kişi adı  | Bir kişi veya ileti alıcı adı. | E-postayı **Stevens** 'e gönder | Stevenler
Date/Time | datetime | Alınan e-postanın tarih ve saati. | **Bugünün**postasını okuyun <br> **Bugün**bana kim e-posta gönder? <br> 7:00 **'** da kimler kurdu? | bugün <br> yarın
Hedef telefon | MPLE | Hedef Kullanıcı ' a bir metin çağırmak veya göndermek istiyor. | **evin** bir çağrısını yapın <br> **ana sayfaya** kısa mesaj gönder | diği <br> giriş sayfası
EmailAddress | email | Kullanıcı e-posta adresi göndermek veya sorgulamak istiyor. | e-posta gönderMegan.Flynn@MKF.com<br> abc@outlook.com 
EmailSubject | basit, model. Kaydedilmemiş | E-posta konusu kullanılan metin. | Konu **Hey** ile David 'e e-posta oluştur  | RE: ilgi çekici hikayesi
Anahtar | MPLE | Anahtar Kullanıcı tuşuna basmanız istiyor. | **Boşluk** tuşuna basın. <br> **9** ' a basın | libre <br> yıldız <br> 8
Çizgi | MPLE | Satır kullanıcısı, öğesinden bir e-posta veya metin göndermek için kullanmak istiyor. | Son **hotmail** e-postamı okuyun. <br> **Mobil**olarak Peter 'yi çağırın. <br> **Çalışma** satırımı kullanarak babacığım 'ı arayın.| hotmail <br> Skype <br> İngiliz hücresi
Gönderenin adı | kişi adı | Gönderen adı. | **David** 'ten e-postayı okuyun <br> Chanda ile e-postalar | David <br> Chanda dili
FromRelationshipName | MPLE | Gönderenin ilişki adı. | **Babacığım**iletisini okuyun. <br> **Anne**'ten gelen tüm metin iletilerini okuyabilir miyim? | M <br> An 
`Message` | basit, model. Kaydedilmemiş |  Bir e-posta veya metin olarak gönderilecek ileti.  | "**Meşgul olduğum**" e-posta gönderin. | Meşgul mıyım
OrderReference | MPLE | Listesini almak için bir öğeyi tanımlamak, sıralı ya da göreli konumu. | Gönderdiğim **son** ileti neydi? <br> **En son** Nokia e-postasını okuyun. <br> **Yeni** metin iletilerini okuyun. | soyadına <br> latest <br> yapılan <br> alanına
PositionReference | basit, sıralı | Listesini almak için bir öğeyi tanımlamak, sıralı ya da göreli konumu.| Gönderdiğim **ilk** ileti neydi? <br> **3** . bir.| İlk <br> 3 taraf
PhoneNumber | PhoneNumber | Kullanıcının, bir metin aramasını veya uygulamasına bir metin göndermesini isteyen telefon numarası. | **4 1 5 6 8 4 5 2 8 4** 'ye metin gönderin | 3525214446
RelationshipName | MPLE | Bir kişinin veya ileti alıcısının ilişki adı. | **Wife** 'e e-posta gönder | Eşim
Searchmetinmetinler | basit, model. any | E-postaları veya iletileri filtrelemek için kullanılan metinler | "**Surface Pro**" içeren tüm e-postaları ara | Surface Pro
Yerleştirildi | MPLE | Hızlı arama. | Çağrı **3 4 5**. <br> Hız çevir **tek**ayarla. | 345 <br> 5

## <a name="email"></a>**E-posta**
E-posta, *iletişim* etki alanının bir alt etki alanıdır. Genellikle e-posta yoluyla ileti gönderme ve alma isteklerini içerir.
### <a name="intents"></a>**Hedefleri**
Amaç adı | Açıklama | Örnekler
---------|----------|---------
AddMore | Daha fazla e-posta veya metin tarafınızdaki e-posta veya metin birleşimin parçası ekleyin. | E-posta gövdesine daha fazla bilgi ekleyin.
İptal | Bir işlemi iptal edin. <br> ***Uyarı**: Daha fazla Iptal için lütfen **yardımcı programlar** etki alanına bakın. * | İptal. Gönderme. <br> Sonlandırın.
CheckMessages | Koşullu sorgu olmadan yeni iletileri/e-postaları denetleyin. Herhangi bir koşul varsa, bu, *arama ileti* amacına aittir. | Gelen kutumu denetle. <br> Yeni bir postam var mı? 
Onayla | E-postaların işlenmesiyle ilgili devam eden eylemi onaylayın. <br> ***Uyarı**: Lütfen daha fazla bilgi için bkz. **yardımcı programlar** etki alanı. * | Evet, gönder. <br> Bu e-posta göndermek istediğiniz onaylıyorum.
Sil | Bir e-postayı veya bazı koşullara göre filtrelenmiş postaları silin. | E-postayı geri dönüşüm kutusu 'na koy <br> Gelen kutumu boşaltın. <br> Mary 'nin e-postasını kaldırın.
ReadAloud | Bir mesaj veya e-posta kullanıcıya okuyun. <br> ***Uyarı**: Lütfen daha fazla bilgi için bkz. genel kullanım için **yardımcı programlar** etki alanı. *  | Mary tarafından gönderilen e-postayı okuyun.
Yanıtla | Geçerli e-postaya bir ileti yanıtlayın. | E-postaya bir yanıt oluşturun. <br> "Teşekkürler çok fazla, en iyi şekilde, John" yazarak yanıtlayın.
SearchMessages | İletileri gönderen adı, zamanı ve konusu dahil bazı koşullara göre arayın. | Nisheen 'dan iletileri göster. <br> "ABC" adlı e-postalarıma arama yapabilir miyim?
SendEmail | E-posta gönderin. | Emre 'ye e-posta: Bu akşam yemeği geçen hafta SPLENDID idi. <br> Emre 'ye e-posta gönderin.
ShowNext | Kısa mesaj veya e-posta listesindeki bir sonraki öğeye bakın. <br> ***Uyarı**: Daha fazla ShowNext genel kullanım için lütfen **yardımcı programlar** etki alanına bakın. * | Sonrakini Göster. <br> Bir sonraki sayfasına gidin.
ShowPrevious | Bir metin iletisi veya e-posta listesinde önceki öğeyi (öğeleri) göster. <br> ***Uyarı**: Daha fazla ShowPrevious genel kullanım için lütfen **yardımcı programlar** etki alanına bakın. * | Öncekine gösterir. <br> Öncekini. <br> Önceki gidin.
İlet | E-posta ilet. | Bu e-postayı Greg adresine iletin.
AddFlag | E-postaya bayrak ekleyin. | Bu e-postayı işaretle <br> Bu e-postaya bayrak ekleyin.
QueryLastText | Son e-postayı sorgulayın. | Bana e-posta gönderen kim? <br> Son zamanlarda bana kim gönderdiniz?


### <a name="entities"></a>**Varlıklar**
Varlık Adı | Varlık türü | Açıklama | Örnekler | Yuvalar
------|-------|----------|--------------|---------------
Attachment | MPLE | Kullanıcının metin veya e-posta ile göndermek istediği ek. | OneNote 'tan bir **dosyayı** e-posta ile gönderin. <br> Evlerimin evmi olduğunu bir kaya gönderin. | dosyası <br> Belg
ContactName | kişi adı  | Bir kişi veya ileti alıcı adı. | E-postayı **Stevens** 'e gönder | Stevenler
Date | datetime | Alınan e-postanın tarihi. | **Bugünün**postasını okuyun <br> **Bugün**bana kim e-posta gönder? | bugün
EmailAddress | email | Kullanıcı e-posta adresi göndermek veya sorgulamak istiyor. | e-posta gönderMegan.Flynn@MKF.com<br> abc@outlook.com 
EmailSubject | basit, model. Kaydedilmemiş | E-posta konusu kullanılan metin. | Konu **Hey** ile David 'e e-posta oluştur  | RE: ilgi çekici hikayesi
Gönderenin adı | kişi adı | Gönderen adı. | **David** 'ten e-postayı okuyun <br> Chanda ile e-postalar | David <br> Chanda dili
FromRelationshipName | MPLE | Gönderenin ilişki adı. | **Babacığım**iletisini okuyun. <br> **Anne**'ten gelen tüm metin iletilerini okuyabilir miyim? | M <br> An 
`Message` | basit, model. Kaydedilmemiş |  Bir e-posta veya metin olarak gönderilecek ileti.  | "**Meşgul olduğum**" e-posta gönderin. | Meşgul mıyım
Category | MPLE | Bir ileti veya e-postanın kategorisi, Kategori "okunmamış", "bayrak" gibi e-posta sisteminde açık bir tanıma sahip olmalıdır. Açıklama w/o Clear Definition, örneğin, "New" ve "son" Kategoriler değildir. | Tüm e-postaları **okundu** olarak işaretle  <br> Paul için yeni **yüksek öncelikli** e-posta | önemli <br> yüksek öncelikli <br> oku
OrderReference | MPLE | Listesini almak için bir öğeyi tanımlamak, sıralı ya da göreli konumu. | Gönderdiğim **son** ileti neydi? <br> **En son** Nokia e-postasını okuyun. <br> **Yeni** metin iletilerini okuyun. | soyadına <br> latest <br> yapılan <br> alanına
PositionReference | basit, sıralı | Listesini almak için bir öğeyi tanımlamak, sıralı ya da göreli konumu.| Gönderdiğim **ilk** ileti neydi? <br> **3** . bir.| İlk <br> 3 taraf
RelationshipName | MPLE | Bir kişinin veya ileti alıcısının ilişki adı. | **Wife** 'e e-posta gönder | Eşim
Time | datetime | Time | e-posta **Tonight**gönder. | Tonight dili
Searchmetinmetinler | basit, model. any | E-postaları veya iletileri filtrelemek için kullanılan metinler | "**Surface Pro**" içeren tüm e-postaları ara | Surface Pro 
Çizgi | MPLE | Kullanıcı bir e-posta göndermek için kullanmak istiyor. | Son **hotmail** e-postamı okuyun. <br> **İş hesabınızdan**bir e-posta gönderin.| hotmail <br> iş hesabı 

## <a name="homeautomation"></a>**HomeAutomation**
HomeAutomation etki alanı, akıllı giriş cihazlarını denetleme ile ilgili amaçlar ve varlıklar sağlar. Genellikle ışıklar ve hava koşullayıcı ile ilgili denetim komutunu destekliyoruz. Ancak diğer elektrik gereçlerinde bazı Genelleştirme becerileri vardır.
### <a name="supported-devices-and-properties"></a>**Desteklenen cihazlar ve Özellikler**
Cihaz | Özellikler
-------|---------
Sıcaklık algılayıcısı | Sıcaklık
Hafif, lamba | Açık, parlaklık, renk
TV, radyo | Açık, hacim
AIR conditioner (A/C), termostat | Açık, sıcaklık, güç
Ran | Açık, güç
Dış izin, DVD oynatıcı, buz, vb. | Açık

### <a name="intents"></a>**Hedefleri**
Amaç adı | Açıklama | Örnekler
---------|----------|---------
 Kapatma | Kullanıcı, cihazı veya ayarları kapatmak istiyor.  | Işıkları devre dışı bırakın. <br> Bir şeyi devre dışı bırakın. <br> Bir şey.
 Aç | Bir cihazı açın veya açın ve cihazı belirli bir ayar veya moda ayarlayın. | Işığı% 50 ' e açın. <br> Kahve Oluşturucu 'yu aç <br> Kahve Oluşturucu 'yu açabilir miyim?
 SetDevice | Kullanıcı, cihazı belirli bir ayar veya türe ayarlamak istiyor.  | Hava koşullayıcı ' i 26 derece olarak ayarlayın. <br> Işıkları mavi olarak açın. <br> Bu ışığı Niak Light olarak çağırın.
 Sorgu durumu | Kullanıcı bir cihazın veya ayarın durumunu ister.  | Termostat ne olarak ayarlanır? <br> A/C açık mı? <br> Yatak odası sıcaklığının anlamı nedir?
 Kullanıma hazır | Cihazların ayarlarını ve parlaklığını açın. | Işıkları yüzde 75 oranında parlaklaştırın. <br> Parlaklığı yüzde 10 oranında artırma.  <br> Oturma odasını çarpıtma yapın.
 Kapatma | Bir cihaz kapatılıyor, ancak ışıkları, sıcaklığın veya parlaklığın parlaklığını veya parlaklığını DEĞIL. | Kitaplığı% 44 oranında kapatın. <br> Işıkları karartma. <br> Odayı daha soğuk yapın.
### <a name="entities"></a>**Varlıklar**
Varlık Adı | Varlık türü | Açıklama | Örnekler
-------|----------|--------------|---------------
DeviceName | List | Cihazları için Kullanıcı tanımlı metin. | Mavi<br> Yan <br> İrtibat
DeviceType | List | Desteklenen cihazlar. | Işıklar <br> Hava koşullayıcı <br> Niak ışığı
Location | MPLE | Cihazın bulunduğu konum veya oda. | Ğa<br> DOWNSTAIRS <br> Yatak odası
NumericalChange | MPLE | Bir ayarın artırılması veya azaltılması miktarı. <br> <br> _Yuva yalnızca turn_up ve turn_down hedefleri ile birlikte görüntülenir._ | 3<br> 50%<br>
OrderReference | ordinal | Bu yuvanın amacı öğelerin seçimini yakalamadır. Öğenin bir listedeki konumunu gösterir. | İlk<br> İkinci
Miktar Niteleyici | List | Nicelik sayısı, belirli bir varlığa ait kaç örneğe başvurulduğunu gösterir. Örneğin, "tümü", "her", vb. | Tümü<br> Her<br> Tümü
Ayar | Basit | Kullanıcı, cihazın sahne, düzey, yoğunluk, renk, mod, sıcaklık ve durum bilgilerini içeren cihazlarını olarak ayarlamak için dilekleri. | Mavi<br> 72 <br> Yı 
SettingType | List | Kullanıcının ilgilendiğiniz cihaz ayarı. | Sıcaklık<br> 
Saat/Tarih |  datetime | Bir cihazın çalışma süresi ve süresi| 5 dakika <br> 3 p.m.
Birim | List | Sözcükleri ' derece ', ' yüzde ', "Fahrenhayt", "santigrat" olarak etiketlemek için her birim terimi ayrı olarak etiketlenmesi gerekir. | Dir<br> Percent


## <a name="notes"></a>**Notlar**
Not etki alanı, kullanıcılar için Not oluşturmayı ve öğeleri yazmayı kolaylaştırır.
### <a name="intents"></a>**Hedefleri**
Amaç adı | Açıklama | Örnekler
---------|----------|---------
AddToNote | Açılan nota bilgi ekleyin. | Proje hakkında pauygulamam ile iletişim kurmak için planlama notuma ekleyin.
Temizle | Tüm öğeleri önceden varolan bir notun temizleyin. | Tatil Notumdaki tüm öğeleri kaldırın. <br>Okumadan tümünü temizle notum.
Onayla | Not ile ilgili bir eylemi onaylayın. <br> ***Uyarı**: Bu amaç genellikle "Onayla" eylemini Note senaryosu için içerir. "Onayla" üzerinde daha fazla genel ifadeye ihtiyacınız varsa, lütfen **Utilities** etki alanında "Onayla" hedefini kullanın. * | Bu notta bana sorun. <br>Listelerdeki tüm öğelerin tutulmasını onaylıyorum.
Create | Yeni bir not oluşturun. | Bir Note oluşturun. <br>Jason 'in ilk haftası olan "ın şehir içinde olduğunu hatırlatmak için unutmayın. 
Sil | Tüm bir not silin. | Tatil notumu Sil. <br>Market notumu silin.
ReadAloud | Sesli bir çıkış okuyun. | İlk notun beni okuyun. <br>Ayrıntıları okuyun.
Kapat | Geçerli notnu kapatın. | Nottan çıkın. <br>Geçerli notnu kapatın.
Açık | Önceden var olan bir notun açın. | Vocation notumu açın. <br>"Planlama" notuna göz açın.
Uzaktan Boyutlandır | Notun cümlesini kaldırın. | Son tümceyi kaldırın. <br>Market Notumdaki yongaları silin. <br>Okul alışveriş Notumdaki kalemleri kaldır
Değişiklik başlığı | Notun başlığını değiştirin. | Bu Notda "planlama" olarak adlandırılmaktadır.

### <a name="entities"></a>**Varlıklar**
Varlık Adı | Varlık türü | Açıklama | Örnekler 
------- | ------- | ------- | -------
Text | basit, model. Kaydedilmemiş | Not veya anımsatıcı metni. | Yürüyen önce Uzat <br> uzun süre çalışan yarın
Başlık | basit, model. Kaydedilmemiş | Not ın başlığı. | Market <br> çağrılacak kişiler <br> Yapılacaklar
CreationDate | datetimeV2 | Bu yuva, kullanıcının belirli bir tarih/saat penceresinde oluşturulmuş notları istediğinde kullanılır. | 
Miktar Niteleyici | List | Bir Kullanıcı ' All ', ' her ' veya ' any ' öğeleri veya bir nottaki tüm metinler üzerinde bir eylem gerçekleştirmeyi istediğinde. | tümü <br> Kaydedilmemiş <br> Her
OrderReference | ordinal | Kullanıcı, "First", "Last", "Next" vs. Items ile eylem yapmak istiyor. | adı <br> soyadına


## <a name="places"></a>**Yer**
Yerler arasında işletmeler, kurumlar, restoranlar, kamu alanları ve adresler bulunur. Etki alanı, konum, çalışma saatleri ve mesafe gibi genel bir yerde bilgileri bulma ve inquiring için yer bulmayı destekler. 
### <a name="intents"></a>**Hedefleri**
Amaç adı | Açıklama | Örnekler
---------|----------|---------
MakeCall | Telefon görüşmesi için bir yer. | applebees on 1000/200 Rojas St ve Call.
FindPlace | Bir yer (iş, kurum, Restoran, ortak alan, adresi) arayın. Değil: <li> Yalnızca iş adı: Başlangıçları <li> Yalnızca konum adı: Seattle/Norveç <li> Cuisine, yiyecek veya yalnızca ürün: Pizza/guacamole/Italyanca | En yakın kitaplık nerede? <br> Seattle 'daki başlangıçları. <br> En yakın kitaplık nerede? <br> 
GetAddress | Bir yer adresi isteyin. | Robson cadde üzerinde Guu adresini göster. <br> En yakın kahve adresini nedir?
GetDistance | Geçerli konumdan belirli bir yere kadar olan mesafeyi sorun. | Tatil Inn nasıl uzakta mi?<br> Bellevue kare ne kadar sürer? <br> Tahoe uzaklığı nedir?
GetPhoneNumber | Bir yer için telefon numarası isteyin. | En yakın kahve telefon numarasını nedir?<br> Giriş deposu için numarayı verin. <br> Disneyıland için telefon numarası.
GetPriceRange | Bir yer için GDP tüketim başına aralığı ister. | Seattle 'da J. Sushi 'nin ortalama fiyatı. <br> Cineplex yarı fiyatına Çarşamba günleri mi? <br> Ne kadar Sizzler lobster tüm Akşam Yemeği maliyetle?
GetStarRating | Bir yer için yıldız derecelendirmesi isteyin. | Zucca nasıl derecelendirilir?<br> Kaç yıldızı Fransızca Çamaşırlar var mı?<br> Aquarium Monterrey içinde iyi mi?
GetHours | Bir alan için çalışma saatleri hakkında isteyin. | Ne zaman Safeway kapatmak?<br> Giriş Depot saatliğine nelerdir?<br> Kahve hala açık mı?
GetReviews | Bir yerde gözden geçirmeler için isteyin. | Cheesecake Factory için İncelemeleri göster. <br> Cineplex incelemelerini okuyun. <br> Kumperdinks için en son incelemeler var mı?
GetMenu | Bir Restoran için menü öğeleri için isteyin. | Herhangi bir şey Zucca sunmuyor vegan? <br> Sizzler adresindeki menüdeki yenilikler nelerdir? <br> Applebee 'in menüsünü göster.
RatePlace | Bir yer değerlendirin. | Sıfırdan büyük bir maki 'nin pizza için 4 yıldız derecelendirmesi. <br> Üçlü Advisor 'a 4 yıldız verin. <br> La Hacienda için 4 yıldızlı bir inceleme oluşturun.
AddFavoritePlace | Kullanıcı, sık kullanılanları veya MVP listesine bir konum eklemek istemektedir. | Bu konumu sık kullanılanlarıma Kaydet. <br> Sık Kullanılanlarıma En Iyi satın alma ekleyin.

### <a name="entities"></a>**Varlıklar**
LUSıS varlıkları | Varlık türü | Açıklama | Örnekler | Utterance örnekleri
--------------|-------------|-------------|----------|-------------------
AbsoluteLocation | MPLE | Konumu veya bir yerde adresidir. | Palo Alto <br> 300 112th Ave SE <br> Seattle | **1020 Middlefield Rd.** **Palo Alto** 'da <br> **Seattle** 'daki kedi tohum mağazaları <br> Buradan **300 112**' ye kadar olan mesafeyi alın.
Kullanılmıyordu | MPLE | Genel bir yerde amaç özellikleri ve avantajları. | rıhtımının <br> Park ücretsiz | Kirkland **su ön** mevsimler restoranlar. <br> Yakınımdaki bir **Ücretsiz Park** var mı?
Cuisine | MPLE | Gıda, cuisine veya cuisine Uyruğu türü. | Çince <br> İtalyanca <br> Suşi <br> Noon <br> | Bir **Çince** Restoran bulmama yardım edin. <br> **Susme** restormasının açılış saatleri nelerdir? <br> **En yakın** BT evi nerede?
Date | datetime | hedeflenen konumun tarih/saati veya süresi. | yarın <br> bugün <br> 6 p.m. | Yarın ne zaman **yarın**kapalı? <br> **6 p.m.** sonra açık olan en yakın Bisiklet Mağazası
Uzaklık | boyut | Kullanıcının currenct konumundan ortak bir konuma uzaklık. | 15 mil <br> 10 mil | **15 mil** içinde bir giyme deposu <br> Yalnızca **10 mil** uzakta olan bir çocuk Restoran
MealType | List | Kahvaltı veya öğle yemeği türü. | Kahvaltı <br> Akşam Yemeği | Ara **kahhızlı** , Seattle İstanbul <br> **Öğle yemeği**için bir yer bulun.
Inızda | List | Mutlak konum veya adres olmadan yakındaki bir yeri tanıtın. | ýn <br> Bu alanda <br> benim istediklerim | **En yakın** Hindistan 'ı bulun. <br> **Yerel** wethermi nerede? <br> **Benimle**ilgili tüm lotlar
OpenStatus | List | Bir yer açık veya kapalı olup olmadığını belirtir. | açık <br> Kapalı | Bugün hangi süreyi **kapatsın** ? <br> Costco için **açılış** saatleri nelerdir?
PlaceName | MPLE | Bir iş, Restoran, ortak bir çekim veya kurum bir hedef adı. Ortak kullanılıyorsa, yer adı bir PlaceType içeriyor olabilir. | Orta parka <br> Safeway <br> Walmart| **Safeway** ilaç ne zaman açık? <br> **Walmart** açık mı?
PlaceType | MPLE | Yerel işletme, Restoran, ortak bir çekim veya kurum bir hedef türü. | Restoran <br> Opera <br> sinema | Cambridge 'de **sinemalar** <br> Yakınımdaki bir **Restoran** var mı?
PriceRange | MPLE | Ürünlerin veya hizmetin Fiyat aralığı. | ucuz <br> uygun maliyetli <br> maliyeti | **Uygun maliyetli** gereç onarımı bulma <br> Şu anda açık olan bir **ucuz** pizza yeri nedir?
Ürün | MPLE | Bir yer tarafından sunulan ürün. | Giyim <br> Lara | **Yiyecek**almanın en iyi yeri nerede? <br> **Televizyonlara**bakmaya yönelik daha fazla bilgi edinin.
Derecelendirme | MPLE | Yer derecelendirmesi. | 5 yıldız <br> Sayfanın Üstü <br> iyi | Yarın gitmem ve yemek için **iyi** bir yer var <br> **en iyi** Amsterdam Restoranlar <br> **En üstteki** üç pizza mağazayı listeleyin.


## <a name="restaurantreservation"></a>**Restoran Antreservatıon**
Restoran ayırma etki alanı, restoranlar için rezervasyon işleme amaçlarını destekler.
### <a name="intents"></a>**Hedefleri**
Amaç adı | Açıklama | Örnekler
---------|----------|---------
ayırma | Bir Restoran için bir ayırma isteği. | Tonight için iki adet Zucca 'da ayır. <br> Yarın için bir tablo kitabı. <br> Palo Alto 'da 3 ' te 3 tablosu. <br> Kırmızı Lobster ile 3 için bir ayırma yapın.
DeleteReservation | Restoran için bir rezervasyonu iptal edin veya silin. | Zucca yarın gece ile rezervasyonu iptal edin. <br> 7:00 p.m. tarihinde Red Lobster için rezervasyonumu unutma sonraki Cuma. <br> Zucca için ayırmaya ihtiyacım yok, iptal et.
ChangeReservation | Mevcut bir restoran ayırması için saati, yeri veya kişi sayısını değiştirin. | Rezervasyonumu 9 p.m. olarak değiştir <br> Zucca 'dan Red Lobster 'a olan rezervasyonumu değiştirin. <br> Zucca 'da ayırma için 6 yerine 7 kişi.
Onayla | Rezervasyonla ilgili bir eylemi onaylayın. <br> ***Uyarı**: Bu amaç, genellikle Restoran rezervasyon senaryosu için "Onayla" eylemini içerir. "Onayla" üzerinde daha fazla genel ifadeye ihtiyacınız varsa, lütfen **Utilities** etki alanında "Onayla" hedefini kullanın. * | Evet, pasta Oluşturucu 'da en saat 8 ' de ton için ayırmaları yaptı. <br> Evet, yalnızca kitap olarak adlandırılırlar. <br> Sustahı çubuğundan ayırmayı onaylayın.
Findrezervationdetail | Mevcut rezervasyonun ayrıntılı bilgilerini görüntüleyin. | Renaissance San Diego 'de rezervasyonumu bul <br> Yarın rezervasyonu görüntüleyin. <br> Zucca için Rezervasyonumun ayrıntılarını görüntüleme.
Findrezervationwhere | Ayırmanın mutlak konumunu denetleyin. | Ayırma içindeki Zucca 'nın konumu nerede? <br> Yarın Rezervasyonumun yerel ayarını göster.
Findrezervationne zaman | Rezervasyonun tam süresini denetleyin | Yarın için Zucca 'nın ayırması ne zaman? <br> Red Lobster ' de rezervasyon süresi.
Reddet | Kullanıcı, bir ayırmayı yönetmek için hangi sanal yardımcıyı önerdiğini reddeder. <br> *Daha fazla bilgi almak için bkz **.:P kirası**, **yardımcı programlar** etki alanı. * | Olayı ayarlama gereksinimi yoktur. | Hayır, ayırmayı değiştirmek istemiyorum. <br> Hayır, kitabı, bir hata yaptık.

### <a name="entities"></a>**Varlıklar**
LUSıS varlığı | Varlık türü | Açıklama | Örnekler
-------|------|---------|-------------------
Adres | MPLE | Olay konumu veya bir ayırma adresi. | Palo Alto<br>300 112th Ave SE<br>Seattle
Atmosfer | List | Restorana ait atmokürenin açıklaması. | Romantik<br>sıradan<br>gruplar için iyi<br>Adres
Cuisine | MPLE | Gıda, cuisine veya cuisine Uyruğu türü. | Çince<br>İtalyanca<br>Meksika<br>Suşi<br>Noon<br>Steak
MealType | List | Ayırma ile ilişkili bir paket türü. | Kahvaltı<br>Akşam Yemeği<br>Öğle yemeği<br>Supper
PlaceName | MPLE | Restoran adı | Zucca<br>Cheesecake Fabrika<br>Red Lobster
Derecelendirme | MPLE | Çalındığında ya da Restoran derecesi. | 5 yıldız<br>3 yıldız<br>4 yıldız
Numberinsanlar | MPLE | Ayırma için kişi sayısı | 3<br>altı
Time | datetime| Restoran rezervasyon saati | yarın<br>Tonight dili<br>7:00 p.m.<br>Noel gecesi


## <a name="todo"></a>**ToDo**
_Todo_ etki alanı, kullanıcıların Todo öğelerini eklemesi, işaretlemesi ve silmesi için görev listesi türleri sağlar.
### <a name="intents"></a>**Hedefleri**
Amaç adı | Açıklama | Örnekler
---------|----------|---------
AddToDo | Kullanıcı, liste türlerinden herhangi biri için görev öğeleri eklemek istemektedir. |  Mılnu satın almayı hatırlatın. <br> "Mılmu al" adlı bir öğeyi yapılacaklar listeme ekle
Onayla | Kullanıcı devam eden bir eylemi onaylamak istiyor. Söylenişi, bir işlemi onaylamak için "Yes" gibi açık bir sinyal içerir. <br> <br> ***Uyarı**: Bu amaç genellikle ToDo senaryosu için "Onayla" eylemini içerir. "Onayla" üzerinde daha fazla genel ifadeye ihtiyacınız varsa, lütfen **Utilities** etki alanında "Onayla" hedefini kullanın. * | Lütfen görevi silin. <br> Bu görevi eklemek istediğsem eminim. <br> Evet, bunu yapmak istiyorum.
İptal | Kullanıcı devam eden eylemi iptal etmek istiyor.  <br> <br> ***Uyarı**: Bu amaç, genellikle Restoran rezervasyon senaryosu için "Onayla" eylemini içerir. "Onayla" üzerinde daha fazla genel ifadeye ihtiyacınız varsa, lütfen **Utilities** etki alanında "Onayla" hedefini kullanın. * | Hayır, unutur. <br> yalnızca görevi iptal edin. <br> Hayır, eklemeyin.
DeleteToDo | Görev listesindeki bir öğeyi, sırasına başvurarak veya tüm yapılacaklar öğelerini silerek silin. | Tüm görevleri kaldırın. <br> İkinci bir tane silmeme yardımcı olun.
MarkToDo | Bir görevi, sırası veya görev içeriğine başvurarak tamamlandı veya bitti olarak işaretleyin. | "Milk satın alma" görevini tamamlandı olarak işaretleyin. <br> Tamamlanan görevi okuyun. <br> Tümünü doldurun.
ShowNextPage | Bir liste gösterilen birçok sayfaya bölünecektir. Kullanıcı için bir sonraki sayfada liste öğelerini göster. | Alışveriş listesindeki bir sonraki sayfayı gösterebilir misiniz? <br> Bir sonraki adımda neler var? <br> Sırada ne var?
ShowPreviousPage | Kullanıcı için önceki sayfada liste öğelerini göster. | öncekini göster. <br> Önceki görevleri denetmem gerekiyor.
ShowToDo | Yapılacaklar listesindeki tüm öğeleri görüntüleyin. | Alışveriş listemi göster. <br> Market listemi gösterin.

### <a name="entities"></a>**Varlıklar**
LUSıS varlığı | Varlık türü | Açıklama | Örnekler
-------|------|---------|-------------------
ContainsAll | MPLE | Görev listesindeki tüm öğeleri veya öğeleri temsil etme | **Tüm** görevleri kaldırmaya yardımcı olabilir. <br> **Her şeyi**tamamlayın.
ordinal | ordinal | Bir öğeye yönelik sıra veya sayısal başvuru. | **Üçüncü** bir tane tamamlandı olarak işaretleyin. <br> **İlk** görevi silin.
ListType | MPLE | Görev listesi türü.  | **Alışveriş** listeme göre ayakkabılar Ekle.
Kidofmarket | List | Yiyecek öğelerinin bir listesini Algıla | **Mılnu**satın almayı hatırlatın. <br> Market listeme bir **Beef** ekleyin.
TaskContent | basit, model. any | Görevin içeriğini tespit edin. | Lütfen bana **anne 'i çağırmayı** hatırlatın. <br> **John 'un Doğum gününü** yapılacaklar listenize Ekle


## <a name="utilities"></a>**Programların**
_Yardımcı programlar_ etki alanı, tüm _lu,_ önceden oluşturulmuş modellerdeki, fark senaryolarında ortak amaçlar ve söyler içeren genel bir etki alanıdır.
### <a name="intents"></a>**Hedefleri**
Amaç adı | Açıklama | Örnekler
---------|----------|---------
 İptal | Bir eylemi/isteği/görevi/hizmeti iptal edin. | İptal edin. <br> Hiçbir şekilde aklınızda bulundurun.
 Onayla | Bir eylemi onaylayın. | Elbette <br> Evet, lütfen <br> Göründüğünü.
 Reddet | Kullanıcı sanal yardımcıyı önerdiğini reddeder. | Hayır
 FinishTask | Kullanıcı başlatılan bir görevi tamamlayın. | Bitti. <br> Hepsi bu kadar. <br> Sona.
 GoBack | Bir adıma geri dönün veya bir önceki adıma geri dönün. | Bir adım geri dönün. <br> Geri Git.
 Help | Yardım isteği. | Lütfen yardım. <br> Yardım 'ı açın.
 Yinele | Bir eylemi yineleyin. | Yeniden söyleyin.
 ShowNext | Sonraki öğeyi (öğeleri) göster veya bildir. | Sonrakini Göster.
 ShowPrevious | Önceki öğeyle bir dizide gösterir. | Öncekini göster.
 StartOver | Uygulamayı yeniden başlatın veya yeni bir oturum başlatın. | Uygulamasını.
 Durdur | Sanal Yardımcınıza konuşmayı durdurmasını söyleyin.  | Bunu durdur.
 SelectAny | Kullanıcı ekranda gösterilen herhangi bir öğeyi veya sonucu seçmenizi ister. | Bunlardan herhangi biri. <br> Herhangi birini seçin.
 SelectNone | Kullanıcı var olan öğelerin hiçbirini seçiyor ve daha fazla seçenek istiyor. | Daha fazla öneri sunun. <br> Bunlardan hiçbiri yok.
  SelectItem | Kullanıcı ekranda gösterilen bir öğe veya sonucu seçmenizi ister. | Üçüncü birini seçin. <br> Sağ üstteki birini seçin.
 İlerlet | Bir müşteri hizmetinin sorunları işlemesini isteyin. | Bir kişiyle iletişim kurabilir miyim?
 ReadAloud | Kullanıcıya sesli bir şeyi okuyun. | Bu sayfayı okuyun. <br> Sesli okuyun.

### <a name="entities"></a>**Varlıklar**
LUSıS varlığı | Varlık türü | Açıklama | Örnekler
------------|-------------|-------------|---------
ordinal | ordinal | Bir öğeye yönelik sıra veya sayısal başvuru. | **İkinci** bir. <br> **Sonraki** bir.
numarası | numarası | Kullanıcının istediği öğe miktarı | Sonraki **3** öğe
DirectionalReference | MPLE | Ekranda bir öğenin bulunduğu bir başvuru noktası. | Bir doğru <br> Üst

## <a name="weather"></a>**Gü**
Hava durumu etki alanı, hava durumu koşullarına ve BT danışmanlarını, hava durumu koşullarına göre konum ve saat ya da denetim saatine
### <a name="intents"></a>**Hedefleri**
Amaç adı | Açıklama | Örnekler
---------|----------|---------
 Checkhava değeri | Tahmine veya geçmiş bilgilere ilişkin bir konum için hava durumu veya hava durumu ile ilgili bir faktör isteyin. <br> Hava durumu ile ilgili faktörlere şunlar dahildir: <li> sıcaklık </li> <li> yağmur/kar/yağış </li> <li> kurutma/Wet/nem </li> <li> sis </li> <li> UV dizini </li> <li> inç/bir yağmur/kar </li> | Pekin hava kalitesi dizini nedir? <br> Mart 'ta Seattle 'da ne kadar yağmur bekleniyor? <br> Hawaii 'nin en yüksek sıcaklığını kaydedin.
 Çek saati | Bir konum için tahmin edilen veya geçmiş hava durumu ile ilgili faktörlerin saatini sorun. | Ne zaman yağmur bekleniyor? <br> Kanada 'ya gitmek iyi bir zaman <br> Minnesota 'daki en yoğun ayı ne zaman?
 Queryhava durumu | "Evet, Hayır veya belki" yanıtı beklenildiği belirli bir konum için hava durumu koşulu veya hava durumu ile ilgili faktörleri sorun veya hava durumu koşuluna bağlı olan etkinlik önerisi hakkında bilgi ister. | Yarın ister misiniz? <br> Bugün bugün mi var? <br> Alaska 'e gitmek çok erken olabilir mi?
 ChangeTemperatureUnit | Santigrat, Kelvin ve Fahrenhayt gibi hava durumu birimini değiştirin. | Santi$ cinsinden Dönüştür. <br> Bunu Kelvin olarak alabilir miyim?
 Getdalgalı Therdanın | Hava durumu belgelerini veya belirli bir konumdan uyarı alın. | Mempa 'da Hava durumu Danışma belgeleri var mı? <br> Alanım için hava durumu uyarıları var mı?

### <a name="entities"></a>**Varlıklar**
LUSıS varlığı | Varlık türü | Açıklama | Örnekler
------------|-------------|-------------|---------
Location | Coğrafya | Hava durumu isteğinin mutlak veya örtük konumu. | Palo Alto<br>Shanghai<br>Seattle<br>Delvina<br>
Date/Time   | datetime | Hava durumunu sorgulamak için tarih/saat veya süre. | Kasım<br>hourly<br>BAH<br>Bu hafta sonu<br>10 gün<br>
Additionalhava durumu | list | Hava durumu için, Rüzgar hızı veya yönü gibi ek açıklama sözcüğü. | direction<br>Hızlı<br>Inız
Yer | MPLE | Geçmiş hava durumu koşulunun açıklama kelimeleri, son zaman döneminde ortalama Joomla boralimli durumlar da dahil olmak üzere. | başlangıcından<br>geçmiş/geçmiş<br>Dönemsel<br>en iyi zaman<br>Şimdiye kadar kaydedildi
PrecipitationUnit | boyut | Kar veya yağmur için yağış. | 5 inç<br>6 cm
SuitableFor | MPLE | Bir hava durumu koşulunda bir insan etkinliğinin açıklaması; kullanıcılar, hava durumu koşuluna bağlı olan etkinlik önerisi ' ne geldiğinde yaygın olarak kullanılır. | ceket<br>şemsiye<br>Yüzme
TemperatureUnit |sıcaklık | sıcaklık | 18 Santi<br>7 Kelvin derece
Hava aralığı | MPLE | Belirli bir süre içinde sıcaklık, Rüzgar ve diğer hava durumu koşullarının belirli bir koşulu | en fazla<br>Geniş<br>düşük<br>Ortalama yüksek<br>En Yüksek
Hava durumu | MPLE | Hava durumu koşulu açıklaması | güneşli<br>maktan<br>yağan<br>sıcaklık<br>kar<br>Sık erişimli
Wınte Directionunit | list | Rüzgar yönü kelimeleri | Kuzeydoğu<br>Eli<br>üzerinden<br>Batı<br>Kuzeydoğu


## <a name="web"></a>**Web**
Web etki alanı, bir Web sitesi aramak için amaç ve varlıklar sağlar.
### <a name="intents"></a>**Hedefleri**
Amaç adı | Açıklama | Örnekler
---------|----------|---------
 Web araması | Belirtilen bir Web sitesine gitme veya arama altyapısında arama yapma isteği. | Google.com içinde arama yüzeyi. <br> Web 'de mutlu Doğum günü şarkısını bulun <br> Www.twitter.com adresine gidin.

### <a name="entities"></a>**Varlıklar**
LUSıS varlığı | Varlık türü | Açıklama | Örnekler
------------|-------------|-------------|---------
SearchEngine | List | Arama motoru kullanıcısı kullanmak istiyor. | Bing <br> Google
Aramametni | basit, model. Kaydedilmemiş | Kullanıcı aramak isteyen metin. <br> _"İn" bir arama altyapısı değilse, Web sitesi "Facebook 'ta arkadaşlar" ifadesini SearchText olarak etiketleyin. URL ayrıca SearchText olarak etiketlemelidir._ | Film <br> Derin öğrenme <br> Tom CRUISE
Bağlantı | url | Web sitesi bağlantısı. | www.twitter.com

