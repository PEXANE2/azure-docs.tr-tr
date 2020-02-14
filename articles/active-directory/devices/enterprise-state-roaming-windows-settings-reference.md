---
title: Windows 10 dolaşım ayarları başvurusu-Azure Active Directory
description: ESR ile Windows 10 ' da dolaşılabilir veya yedeklenecek ayarlar
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: troubleshooting
ms.date: 02/12/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: na
ms.collection: M365-identity-device-management
ms.openlocfilehash: 54a75b662fabcce8ffa64f1b705e86a1c1dde625
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77194339"
---
# <a name="windows-10-roaming-settings-reference"></a>Windows 10 dolaşım ayarları başvurusu

Windows 10 ' da dolaşılabilir veya yedeklenecek ayarların bir listesi aşağıda verilmiştir. 

## <a name="devices-and-endpoints"></a>Cihazlar ve uç noktalar

Windows 10 ' da eşitleme, yedekleme ve geri yükleme çerçevesi tarafından desteklenen cihazların ve hesap türlerinin Özeti için aşağıdaki tabloya bakın.

| Hesap türü ve işlem | Masaüstü | Cep Telefonu |
| --- | --- | --- |
| Azure Active Directory: eşitleme |Evet |Hayır |
| Azure Active Directory: yedekleme/geri yükleme |Hayır |Hayır |
| Microsoft hesabı: eşitleme |Evet |Evet |
| Microsoft hesabı: yedekleme/geri yükleme |Hayır |Evet |

## <a name="what-is-backup"></a>Yedekleme nedir?

Windows ayarları genellikle varsayılan olarak eşitlenir, ancak bazı ayarlar yalnızca bir cihazdaki yüklü uygulamaların listesi gibi yedeklenir. Yedekleme yalnızca mobil cihazlara yöneliktir ve şu anda Enterprise State Roaming kullanıcılar için kullanılamaz. Yedekleme bir Microsoft hesabı kullanır ve ayarları ve uygulama verilerini OneDrive 'a depolar. Bir Kullanıcı, Ayarlar uygulamasını kullanarak cihazda eşitlemeyi devre dışı bırakırsa, normalde eşitlenen uygulama verileri yalnızca yedekleme haline gelir. Yedekleme verilerine yalnızca yeni bir cihazın ilk çalıştırma deneyimi sırasında geri yükleme işlemi üzerinden erişilebilir. Yedeklemeler cihaz ayarları aracılığıyla devre dışı bırakılabilir ve kullanıcının OneDrive hesabıyla yönetilebilecek ve silinebilirler.

## <a name="windows-settings-overview"></a>Windows ayarlarına genel bakış

Son kullanıcıların Windows 10 cihazlarında ayarları eşitlemeyi etkinleştirmesine/devre dışı bırakmasına olanak sağlayan aşağıdaki ayarlar grupları vardır.

* Tema: masaüstü arka planı, Kullanıcı kutucuğu, görev çubuğu konumu vb. 
* Internet Explorer ayarları: gözatma geçmişi, yazılan URL 'Ler, Sık Kullanılanlar vb. 
* Parolalar: Wi-Fi profilleri dahil Windows kimlik bilgileri Yöneticisi 
* Dil tercihleri: yazım denetimi sözlüğü, Sistem dil ayarları 
* Erişim kolaylığı: ekran okuyucusu, ekran klavyesi, Büyüteç 
* Diğer Windows ayarları: bkz. Windows ayarları ayrıntıları
* Microsoft Edge tarayıcı ayarı: Microsoft Edge sık kullanılanları, okuma listesi ve diğer ayarlar

![Ayarlarınızı eşitleyin](./media/enterprise-state-roaming-windows-settings-reference/active-directory-enterprise-state-roaming-syncyoursettings.png)

> [!NOTE]
> Bu makale, 2015 Temmuz sürümünde Windows 10 ile başlatılan Microsoft Edge eski HTML tabanlı tarayıcı için geçerlidir. Makale, 15 Ocak 2020 ' de yayınlanan yeni Microsoft Edge Bermıum tabanlı tarayıcı için de geçerlidir. Yeni Microsoft Edge için eşitleme davranışı hakkında daha fazla bilgi için [Microsoft Edge Sync](https://docs.microsoft.com/deployedge/microsoft-edge-enterprise-sync)makalesine bakın.

Microsoft Edge tarayıcı ayarı grubu (Sık Kullanılanlar, okuma listesi) eşitleme, son kullanıcılar tarafından Microsoft Edge tarayıcı ayarları menü seçeneği aracılığıyla etkinleştirilebilir veya devre dışı bırakılabilir.

![Hesap](./media/enterprise-state-roaming-windows-settings-reference/active-directory-enterprise-state-roaming-edge.png)

Windows 10 sürüm 1803 veya üzeri için, Internet Explorer ayar grubu (Sık Kullanılanlar, yazılı URL 'Ler) eşitleme, son kullanıcılar tarafından Internet Explorer ayarları menü seçeneği aracılığıyla etkinleştirilebilir veya devre dışı bırakılabilir. 

![Ayarlar](./media/enterprise-state-roaming-windows-settings-reference/active-directory-enterprise-state-roaming-ie.png)

## <a name="windows-settings-details"></a>Windows ayarları ayrıntıları

Aşağıdaki tabloda, ayarlar grubu sütunundaki diğer girişler Ayarlar > Hesaplar ' a giderek devre dışı bırakılabilecek ayarlar ' a başvurur > diğer Windows ayarları > ayarlarınızı eşitler. 

Ayarlar Grup sütunundaki iç girişler yalnızca uygulamanın kendisi içinde eşitlenmesi devre dışı bırakılabilecek veya mobil cihaz yönetimi (MDM) veya grup ilkesi ayarları kullanılarak tüm cihazın eşitlemesini devre dışı bırakarak, ayarlar ve uygulamalar bölümüne başvurur.
Dolaşımda olmayan veya eşitlenmeyecek ayarlar bir gruba ait değil.

| Ayarlar | Masaüstü | Cep Telefonu | Grup |
| --- | --- | --- | --- |
| **Hesaplar**: hesap resmi |eşitle |X |Tema |
| **Hesaplar**: diğer hesap ayarları |X |X | |
| **Gelişmiş Mobil geniş bant**: Internet bağlantısı paylaşımı ağ adı (Bluetooth Ile mobil Wi-Fi etkin noktalarına otomatik bulma imkanı sunar) |X |X |Parolalar |
| **Uygulama verileri**: ayrı uygulamalar, verileri eşitleyebilir |Eşitleme yedeklemesi |Eşitleme yedeklemesi |iç |
| **Uygulama listesi**: yüklü uygulamaların listesi |X |yedekleme |Diğer |
| **Bluetooth**: tüm Bluetooth ayarları |X |X | |
| **Komut istemi**: komut Istemi "Varsayılanlar" ayarları |eşitle |X |iç |
| **Kimlik bilgileri**: kimlik bilgisi dolap |eşitle |eşitle |parola |
| **Tarih, saat ve bölge**: otomatik saat (Internet saat eşitleme) |eşitle |eşitle |language |
| **Tarih, saat ve bölge**: 24 saat saat |eşitle |X |language |
| **Tarih, saat ve bölge**: Tarih ve saat |eşitle |X |language |
| **Tarih, saat ve bölge**: Saat dilimi | |X |language |
| **Tarih, saat ve bölge**: yaz tasarrufu süresi |eşitle |X |language |
| **Tarih, saat ve bölge**: ülke/bölge |eşitle |X |language |
| **Tarih, saat ve bölge**: haftanın ilk günü |eşitle |X |language |
| **Tarih, saat ve bölge**: bölge biçimi (yerel ayar) |eşitle |X |language |
| **Tarih, saat ve bölge**: kısa tarih |eşitle |X |language |
| **Tarih, saat ve bölge**: uzun tarih |eşitle |X |language |
| **Tarih, saat ve bölge**: kısa saat |eşitle |X |language |
| **Tarih, saat ve bölge**: uzun saat |eşitle |X |language |
| **Masaüstü kişiselleştirme**: masaüstü teması (arka plan, sistem rengi, varsayılan sistem sesleri, ekran koruyucusu) |eşitle |X |Tema |
| **Masaüstü kişiselleştirme**: slayt gösterisi duvar kağıdı |eşitle |X |Tema |
| **Masaüstü kişiselleştirme**: görev çubuğu ayarları (konum, otomatik gizleme vb.) |eşitle |X |Tema |
| **Masaüstü kişiselleştirme**: başlangıç ekranı düzeni |X |yedekleme | |
| **Cihazlar**: bağladığınız paylaşılan yazıcılar |X |X |diğer |
| **Microsoft Edge tarayıcısı**: okuma listesi |eşitle |eşitle |iç |
| **Microsoft Edge tarayıcısı**: Sık Kullanılanlar |eşitle |eşitle |iç |
| **Microsoft Edge tarayıcısı**: en iyi siteler <sup> [[1]](#footnote-1)</sup> |eşitle |eşitle |iç |
| **Microsoft Edge tarayıcısı**: yazılan URL 'ler <sup> [[1]](#footnote-1)</sup> |eşitle |eşitle |iç |
| **Microsoft Edge tarayıcısı**: Sık Kullanılanlar çubuk ayarları <sup> [[1]](#footnote-1)</sup> |eşitle |eşitle |iç |
| **Microsoft Edge tarayıcısı**: giriş düğmesini göster <sup> [[1]](#footnote-1)</sup> |eşitle |eşitle |iç |
| **Microsoft Edge tarayıcısı**: açılan pencereleri engelle <sup> [[1]](#footnote-1)</sup> |eşitle |eşitle |iç |
| **Microsoft Edge tarayıcısı**: her indirmenin ne yapacağımı sor <sup> [[1]](#footnote-1)</sup> |eşitle |eşitle |iç |
| **Microsoft Edge tarayıcısı**: parolaları kaydetme teklifi <sup> [[1]](#footnote-1)</sup> |eşitle |eşitle |iç |
| **Microsoft Edge tarayıcısı**: izleme istekleri gönderme <sup> [[1]](#footnote-1)</sup> |eşitle |eşitle |iç |
| **Microsoft Edge tarayıcısı**: form girdilerini kaydetme <sup> [[1]](#footnote-1)</sup> |eşitle |eşitle |iç |
| **Microsoft Edge tarayıcısı**: yazarken arama ve site önerilerini göster <sup> [[1]](#footnote-1)</sup> |eşitle |eşitle |iç |
| **Microsoft Edge tarayıcısı**: tanımlama bilgileri tercihi <sup> [[1]](#footnote-1)</sup> |eşitle |eşitle |iç |
| **Microsoft Edge tarayıcısı**: sitelerle korunan medya lisanslarını Cihazlarıma kaydetmesine izin ver <sup> [[1]](#footnote-1)</sup> |eşitle |eşitle |iç |
| **Microsoft Edge tarayıcısı**: ekran okuyucusu ayarı <sup> [[1]](#footnote-1)</sup> |eşitle |eşitle |iç |
| **Yüksek karşıtlık**: açık veya kapalı |eşitle |X |Erişim kolaylığı |
| **Yüksek karşıtlık**: tema ayarları |eşitle |X |Erişim kolaylığı |
| **Internet Explorer**: açık sekmeler (URL ve başlık) |eşitle |eşitle |Internet Explorer |
| **Internet Explorer**: okuma listesi |eşitle |eşitle |Internet Explorer |
| **Internet Explorer**: yazılan URL 'ler |eşitle |eşitle |Internet Explorer |
| **Internet Explorer**: tarama geçmişi |eşitle |eşitle |Internet Explorer |
| **Internet Explorer**: Sık Kullanılanlar |eşitle |eşitle |Internet Explorer |
| **Internet Explorer**: dışlanan URL 'ler |eşitle |eşitle |Internet Explorer |
| **Internet Explorer**: giriş sayfaları |eşitle |eşitle |Internet Explorer |
| **Internet Explorer**: etki alanı önerileri |eşitle |eşitle |Internet Explorer |
| **Klavye**: kullanıcılar ekran klavyesi açabilir/kapatabilir |eşitle |X |Erişim kolaylığı |
| **Klavye**: yapışkan Evet 'i aç (varsayılan olarak kapalı) |eşitle |X |Erişim kolaylığı |
| **Klavye**: filtre anahtarlarını aç (varsayılan olarak kapalı) |eşitle |X |Erişim kolaylığı |
| **Klavye**: geçiş tuşlarını aç (varsayılan olarak kapalı) |eşitle |X |Erişim kolaylığı |
| **Internet Explorer**: etki alanı dili: ÇINCE (CHS) QWERTY-kendi kendine öğrenmeyi etkinleştirin |eşitle |X |Dil |
| **Dil**: CHS QWERTY-dinamik aday sıralamasını etkinleştir |eşitle |X |Dil |
| **Dil**: CHS QWERTY-char-Basitleştirilmiş Çince ayarla |eşitle |X |Dil |
| **Dil**: CHS QWERTY-char-geleneksel Çince ayarla |eşitle |X |Dil |
| **Dil**: CHS QWERTY-benzer Pinyin |eşitle |yedekleme |Dil |
| **Dil**: CHS QWERTY-belirsiz çiftler |eşitle |yedekleme |Dil |
| **Dil**: CHS QWERTY-tam Pinyin |eşitle |X |Dil |
| **Dil**: CHS QWERTY-çift Pinyin |eşitle |X |Dil |
| **Dil**: CHS QWERTY-okuma otomatik düzeltme |eşitle |X |Dil |
| **Dil**: CHS QWERTY-C/E anahtar anahtarı, Shift |eşitle |X |Dil |
| **Dil**: CHS QWERTY-C/E anahtar tuşu, CTRL |eşitle |X |Dil |
| **Dil**: CHS wubı-tek karakter giriş modu |eşitle |X |Dil |
| **Dil**: CHS wubı-aday 'nın kalan kodlamasını göster |eşitle |X |Dil |
| **Dil**: CHS Wubi-4 kodlama geçersiz olduğunda bip |eşitle |X |Dil |
| **Dil**: CHT Bopomofo-Include CJK ext-A |eşitle |X |Dil |
| **Dil**: Japonca IME-tahmine dayalı yazı ve özel sözcükler |eşitle |eşitle |Dil |
| **Dil**: Korece (kor) IME |X |X |Dil |
| **Dil**: el yazısı tanıma |X |X |Dil |
| **Dil**: Dil profili |eşitle |yedekleme |Dil |
| **Dil**: yazım denetimi-otomatik düzelt ve yazım hatalarını Vurgula |eşitle |yedekleme |Dil |
| **Dil**: klavyeler listesi |eşitle |yedekleme |Dil |
| **Kilit ekranı**: tüm kilit ekranı ayarları |X |X | |
| **Büyüteç**: açık veya kapalı (ana geçiş) |X |X |Erişim kolaylığı |
| **Büyüteç**: Inversion rengini aç veya kapat (varsayılan olarak kapalı) |eşitle |X |Erişim kolaylığı |
| **Büyüteç**: izleme-klavye odağını izleyin |eşitle |X |Erişim kolaylığı |
| **Büyüteç**: izleme-fare imlecini izleyin |eşitle |X |Erişim kolaylığı |
| **Büyüteç**: kullanıcılar oturum açtığında Başlat (varsayılan olarak kapalıdır) |eşitle |X |Erişim kolaylığı |
| **Fare**: fare imlecinin boyutunu değiştirme |eşitle |X |diğer |
| **Fare**: fare imlecinin rengini değiştirme |eşitle |X |diğer |
| **Fare**: diğer tüm ayarlar |X |X | |
| **Ekran okuyucusu**: hızlı başlatma |eşitle |X |Erişim kolaylığı |
| **Ekran okuyucusu**: kullanıcılar ekran okuyucusu konuşma aralığını değiştirebilir |eşitle |X |Erişim kolaylığı |
| **Ekran okuyucusu**: kullanıcılar, sık kullanılan öğeler için okuyucu okuma ipuçlarını açabilir veya kapatabilir (varsayılan olarak açık) |eşitle |X |Erişim kolaylığı |
| **Ekran okuyucusu**: kullanıcılar, yazılan karakterleri (varsayılan olarak açık) duyabilecekleri veya kapatırım |eşitle |X |Erişim kolaylığı |
| **Ekran okuyucusu**: kullanıcılar yazılan sözcükleri (varsayılan olarak açık) duyabilecekleri veya kapatırım |eşitle |X |Erişim kolaylığı |
| **Ekran okuyucusu**: ekran okuyucuyu izleyen imleç ekleme (varsayılan olarak açık) |eşitle |X |Erişim kolaylığı |
| **Ekran okuyucusu**: Ekran Okuyucusu imlecinin görsel vurgulanmasını etkinleştir (varsayılan olarak açık) |eşitle |X |Erişim kolaylığı |
| **Ekran okuyucusu**: ses ipuçlarını Oynat (varsayılan olarak açık) |eşitle |X |Erişim kolaylığı |
| **Ekran okuyucusu**: parmağınızı kaldırdığınızda dokunmatik klavyede tuşları etkinleştirin (varsayılan olarak kapalı) |eşitle |X |Erişim kolaylığı |
| **Erişim kolaylığı**: yanıp sönen imlecin kalınlığını ayarla |eşitle |X |Erişim kolaylığı |
| **Erişim kolaylığı**: arka plan görüntülerini Kaldır (varsayılan olarak kapalı) |eşitle |X |Erişim kolaylığı |
| **Güç ve uyku**: tüm ayarlar |X |X | |
| **Başlangıç ekranı kişiselleştirmesi**: vurgu rengi (yalnızca telefon) |X |eşitle |Tema |
| **Yazma**: yazım denetimi sözlüğü |eşitle |yedekleme |Dil |
| **Yazma**: otomatik olarak yanlış yazılan sözcük |eşitle |yedekleme |Dil |
| **Yazma**: yanlış yazılan sözcükleri Vurgula |eşitle |yedekleme |Dil |
| **Yazma**: metin önerilerini yazarken göster |eşitle |yedekleme |Dil |
| **Yazma**: metin önerisi seçtiğimde bir boşluk ekleyin |eşitle |yedekleme |Dil |
| **Yazma**: boşluk çubuğuna çift dokunduktan sonra bir dönem ekleyin |eşitle |yedekleme |Dil |
| **Yazma**: her tümcenin ilk harfini büyük harfe çevir |eşitle |yedekleme |Dil |
| **Yazma**: SHIFT tuşuna çift dokunduktan sonra tüm büyük harfleri kullan |eşitle |yedekleme |Dil |
| **Yazma**: yazarken anahtar seslerini oynat |eşitle |yedekleme |Dil |
| **Yazma**: dokunmatik klavye için kişiselleştirme verileri |eşitle |yedekleme |Dil |
| **Wi-Fi**: Wi-Fi profilleri (yalnızca WPA) |eşitle |eşitle |Parolalar |

###### <a name="footnote-1"></a>Dipnot 1

Windows Creators Update 'in desteklenen en düşük işletim sistemi sürümü (derleme 15063). 

## <a name="next-steps"></a>Sonraki adımlar

Genel bakış için bkz. [kuruluş durumu dolaşımına genel bakış](enterprise-state-roaming-overview.md).
