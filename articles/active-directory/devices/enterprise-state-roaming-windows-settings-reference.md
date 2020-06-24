---
title: Windows 10 dolaşım ayarları başvurusu-Azure Active Directory
description: ESR ile Windows 10 ' da dolaşılabilir veya yedeklenecek ayarlar
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: reference
ms.date: 02/12/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: na
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6048ee9237640799b7bec37083e607fc74ffb8e4
ms.sourcegitcommit: bf99428d2562a70f42b5a04021dde6ef26c3ec3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85252976"
---
# <a name="windows-10-roaming-settings-reference"></a>Windows 10 dolaşım ayarları başvurusu

Windows 10 ' da dolaşılabilir veya yedeklenecek ayarların bir listesi aşağıda verilmiştir. 

## <a name="devices-and-endpoints"></a>Cihazlar ve uç noktalar

Windows 10 ' da eşitleme, yedekleme ve geri yükleme çerçevesi tarafından desteklenen cihazların ve hesap türlerinin Özeti için aşağıdaki tabloya bakın.

| Hesap türü ve işlem | Masaüstü | Mobil |
| --- | --- | --- |
| Azure Active Directory: eşitleme |Evet |No |
| Azure Active Directory: yedekleme/geri yükleme |Hayır |Hayır |
| Microsoft hesabı: eşitleme |Yes |Yes |
| Microsoft hesabı: yedekleme/geri yükleme |No |Evet |

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
> Bu makale, 2015 Temmuz sürümünde Windows 10 ile başlatılan Microsoft Edge eski HTML tabanlı tarayıcı için geçerlidir. Makale, 15 Ocak 2020 ' de yayınlanan yeni Microsoft Edge Bermıum tabanlı tarayıcı için de geçerlidir. Yeni Microsoft Edge için eşitleme davranışı hakkında daha fazla bilgi için [Microsoft Edge Sync](/deployedge/microsoft-edge-enterprise-sync)makalesine bakın.

Microsoft Edge tarayıcı ayarı grubu (Sık Kullanılanlar, okuma listesi) eşitleme, son kullanıcılar tarafından Microsoft Edge tarayıcı ayarları menü seçeneği aracılığıyla etkinleştirilebilir veya devre dışı bırakılabilir.

![Hesap](./media/enterprise-state-roaming-windows-settings-reference/active-directory-enterprise-state-roaming-edge.png)

Windows 10 sürüm 1803 veya üzeri için, Internet Explorer ayar grubu (Sık Kullanılanlar, yazılı URL 'Ler) eşitleme, son kullanıcılar tarafından Internet Explorer ayarları menü seçeneği aracılığıyla etkinleştirilebilir veya devre dışı bırakılabilir. 

![Ayarlar](./media/enterprise-state-roaming-windows-settings-reference/active-directory-enterprise-state-roaming-ie.png)

## <a name="windows-settings-details"></a>Windows ayarları ayrıntıları

Aşağıdaki tabloda, ayarlar grubu sütunundaki diğer girişler ayarlar > hesaplar ' a giderek devre dışı bırakılabilecek ayarlar ' a başvurur > diğer Windows ayarları > ayarlarınızı eşitler. 

Ayarlar Grup sütunundaki iç girişler yalnızca uygulamanın kendisi içinde eşitlenmesi devre dışı bırakılabilecek veya mobil cihaz yönetimi (MDM) veya grup ilkesi ayarları kullanılarak tüm cihazın eşitlemesini devre dışı bırakarak, ayarlar ve uygulamalar bölümüne başvurur.
Dolaşımda olmayan veya eşitlenmeyecek ayarlar bir gruba ait değil.

| Ayarlar | Masaüstü | Mobil | Grup |
| --- | --- | --- | --- |
| **Hesaplar**: hesap resmi |eşitleme |X |Tema |
| **Hesaplar**: diğer hesap ayarları |X |X | |
| **Gelişmiş Mobil geniş bant**: Internet bağlantısı paylaşımı ağ adı (Bluetooth Ile mobil Wi-Fi etkin noktalarına otomatik bulma imkanı sunar) |X |X |Parolalar |
| **Uygulama verileri**: ayrı uygulamalar, verileri eşitleyebilir |Eşitleme yedeklemesi |Eşitleme yedeklemesi |internal |
| **Uygulama listesi**: yüklü uygulamaların listesi |X |yedekleme |Diğer |
| **Bluetooth**: tüm Bluetooth ayarları |X |X | |
| **Komut istemi**: komut Istemi "Varsayılanlar" ayarları |eşitleme |X |internal |
| **Kimlik bilgileri**: kimlik bilgisi dolap |eşitleme |eşitleme |password |
| **Tarih, saat ve bölge**: otomatik saat (Internet saat eşitleme) |eşitleme |eşitleme |language |
| **Tarih, saat ve bölge**: 24 saat saat |eşitleme |X |language |
| **Tarih, saat ve bölge**: Tarih ve saat |eşitleme |X |language |
| **Tarih, saat ve bölge**: Saat dilimi | |X |language |
| **Tarih, saat ve bölge**: yaz tasarrufu süresi |eşitleme |X |language |
| **Tarih, saat ve bölge**: ülke/bölge |eşitleme |X |language |
| **Tarih, saat ve bölge**: haftanın ilk günü |eşitleme |X |language |
| **Tarih, saat ve bölge**: bölge biçimi (yerel ayar) |eşitleme |X |language |
| **Tarih, saat ve bölge**: kısa tarih |eşitleme |X |language |
| **Tarih, saat ve bölge**: uzun tarih |eşitleme |X |language |
| **Tarih, saat ve bölge**: kısa saat |eşitleme |X |language |
| **Tarih, saat ve bölge**: uzun saat |eşitleme |X |language |
| **Masaüstü kişiselleştirme**: masaüstü teması (arka plan, sistem rengi, varsayılan sistem sesleri, ekran koruyucusu) |eşitleme |X |Tema |
| **Masaüstü kişiselleştirme**: slayt gösterisi duvar kağıdı |eşitleme |X |Tema |
| **Masaüstü kişiselleştirme**: görev çubuğu ayarları (konum, otomatik gizleme vb.) |eşitleme |X |Tema |
| **Masaüstü kişiselleştirme**: başlangıç ekranı düzeni |X |yedekleme | |
| **Cihazlar**: bağladığınız paylaşılan yazıcılar |X |X |diğer |
| **Microsoft Edge tarayıcısı**: okuma listesi |eşitleme |eşitleme |internal |
| **Microsoft Edge tarayıcısı**: Sık Kullanılanlar |eşitleme |eşitleme |internal |
| **Microsoft Edge tarayıcısı**: en iyi siteler <sup> [[1]](#footnote-1)</sup> |eşitleme |eşitleme |internal |
| **Microsoft Edge tarayıcısı**: yazılan URL 'ler <sup> [[1]](#footnote-1)</sup> |eşitleme |eşitleme |internal |
| **Microsoft Edge tarayıcısı**: Sık Kullanılanlar çubuk ayarları <sup> [[1]](#footnote-1)</sup> |eşitleme |eşitleme |internal |
| **Microsoft Edge tarayıcısı**: giriş düğmesini göster <sup> [[1]](#footnote-1)</sup> |eşitleme |eşitleme |internal |
| **Microsoft Edge tarayıcısı**: açılan pencereleri engelle <sup> [[1]](#footnote-1)</sup> |eşitleme |eşitleme |internal |
| **Microsoft Edge tarayıcısı**: her indirmenin ne yapacağımı sor <sup> [[1]](#footnote-1)</sup> |eşitleme |eşitleme |internal |
| **Microsoft Edge tarayıcısı**: parolaları kaydetme teklifi <sup> [[1]](#footnote-1)</sup> |eşitleme |eşitleme |internal |
| **Microsoft Edge tarayıcısı**: izleme istekleri gönderme <sup> [[1]](#footnote-1)</sup> |eşitleme |eşitleme |internal |
| **Microsoft Edge tarayıcısı**: form girdilerini kaydetme <sup> [[1]](#footnote-1)</sup> |eşitleme |eşitleme |internal |
| **Microsoft Edge tarayıcısı**: yazarken arama ve site önerilerini göster <sup> [[1]](#footnote-1)</sup> |eşitleme |eşitleme |internal |
| **Microsoft Edge tarayıcısı**: tanımlama bilgileri tercihi <sup> [[1]](#footnote-1)</sup> |eşitleme |eşitleme |internal |
| **Microsoft Edge tarayıcısı**: sitelerle korunan medya lisanslarını Cihazlarıma kaydetmesine izin ver <sup> [[1]](#footnote-1)</sup> |eşitleme |eşitleme |internal |
| **Microsoft Edge tarayıcısı**: ekran okuyucusu ayarı <sup> [[1]](#footnote-1)</sup> |eşitleme |eşitleme |internal |
| **Yüksek karşıtlık**: açık veya kapalı |eşitleme |X |erişim kolaylığı |
| **Yüksek karşıtlık**: tema ayarları |eşitleme |X |erişim kolaylığı |
| **Internet Explorer**: açık sekmeler (URL ve başlık) |eşitleme |eşitleme |Internet Explorer |
| **Internet Explorer**: okuma listesi |eşitleme |eşitleme |Internet Explorer |
| **Internet Explorer**: yazılan URL 'ler |eşitleme |eşitleme |Internet Explorer |
| **Internet Explorer**: tarama geçmişi |eşitleme |eşitleme |Internet Explorer |
| **Internet Explorer**: Sık Kullanılanlar |eşitleme |eşitleme |Internet Explorer |
| **Internet Explorer**: dışlanan URL 'ler |eşitleme |eşitleme |Internet Explorer |
| **Internet Explorer**: giriş sayfaları |eşitleme |eşitleme |Internet Explorer |
| **Internet Explorer**: etki alanı önerileri |eşitleme |eşitleme |Internet Explorer |
| **Klavye**: kullanıcılar ekran klavyesi açabilir/kapatabilir |eşitleme |X |erişim kolaylığı |
| **Klavye**: yapışkan Evet 'i aç (varsayılan olarak kapalı) |eşitleme |X |erişim kolaylığı |
| **Klavye**: filtre anahtarlarını aç (varsayılan olarak kapalı) |eşitleme |X |erişim kolaylığı |
| **Klavye**: geçiş tuşlarını aç (varsayılan olarak kapalı) |eşitleme |X |erişim kolaylığı |
| **Internet Explorer**: etki alanı dili: ÇINCE (CHS) QWERTY-kendi kendine öğrenmeyi etkinleştirin |eşitleme |X |Dil |
| **Dil**: CHS QWERTY-dinamik aday sıralamasını etkinleştir |eşitleme |X |Dil |
| **Dil**: CHS QWERTY-char-Basitleştirilmiş Çince ayarla |eşitleme |X |Dil |
| **Dil**: CHS QWERTY-char-geleneksel Çince ayarla |eşitleme |X |Dil |
| **Dil**: CHS QWERTY-benzer Pinyin |eşitleme |yedekleme |Dil |
| **Dil**: CHS QWERTY-belirsiz çiftler |eşitleme |yedekleme |Dil |
| **Dil**: CHS QWERTY-tam Pinyin |eşitleme |X |Dil |
| **Dil**: CHS QWERTY-çift Pinyin |eşitleme |X |Dil |
| **Dil**: CHS QWERTY-okuma otomatik düzeltme |eşitleme |X |Dil |
| **Dil**: CHS QWERTY-C/E anahtar anahtarı, Shift |eşitleme |X |Dil |
| **Dil**: CHS QWERTY-C/E anahtar tuşu, CTRL |eşitleme |X |Dil |
| **Dil**: CHS wubı-tek karakter giriş modu |eşitleme |X |Dil |
| **Dil**: CHS wubı-aday 'nın kalan kodlamasını göster |eşitleme |X |Dil |
| **Dil**: CHS Wubi-4 kodlama geçersiz olduğunda bip |eşitleme |X |Dil |
| **Dil**: CHT Bopomofo-Include CJK ext-A |eşitleme |X |Dil |
| **Dil**: Japonca IME-tahmine dayalı yazı ve özel sözcükler |eşitleme |eşitleme |Dil |
| **Dil**: Korece (kor) IME |X |X |Dil |
| **Dil**: el yazısı tanıma |X |X |Dil |
| **Dil**: Dil profili |eşitleme |yedekleme |Dil |
| **Dil**: yazım denetimi-otomatik düzelt ve yazım hatalarını Vurgula |eşitleme |yedekleme |Dil |
| **Dil**: klavyeler listesi |eşitleme |yedekleme |Dil |
| **Kilit ekranı**: tüm kilit ekranı ayarları |X |X | |
| **Büyüteç**: açık veya kapalı (ana geçiş) |X |X |Erişim kolaylığı |
| **Büyüteç**: Inversion rengini aç veya kapat (varsayılan olarak kapalı) |eşitleme |X |Erişim kolaylığı |
| **Büyüteç**: izleme-klavye odağını izleyin |eşitleme |X |Erişim kolaylığı |
| **Büyüteç**: izleme-fare imlecini izleyin |eşitleme |X |Erişim kolaylığı |
| **Büyüteç**: kullanıcılar oturum açtığında Başlat (varsayılan olarak kapalıdır) |eşitleme |X |Erişim kolaylığı |
| **Fare**: fare imlecinin boyutunu değiştirme |eşitleme |X |diğer |
| **Fare**: fare imlecinin rengini değiştirme |eşitleme |X |diğer |
| **Fare**: diğer tüm ayarlar |X |X | |
| **Ekran okuyucusu**: hızlı başlatma |eşitleme |X |Erişim kolaylığı |
| **Ekran okuyucusu**: kullanıcılar ekran okuyucusu konuşma aralığını değiştirebilir |eşitleme |X |Erişim kolaylığı |
| **Ekran okuyucusu**: kullanıcılar, sık kullanılan öğeler için okuyucu okuma ipuçlarını açabilir veya kapatabilir (varsayılan olarak açık) |eşitleme |X |Erişim kolaylığı |
| **Ekran okuyucusu**: kullanıcılar, yazılan karakterleri (varsayılan olarak açık) duyabilecekleri veya kapatırım |eşitleme |X |Erişim kolaylığı |
| **Ekran okuyucusu**: kullanıcılar yazılan sözcükleri (varsayılan olarak açık) duyabilecekleri veya kapatırım |eşitleme |X |Erişim kolaylığı |
| **Ekran okuyucusu**: ekran okuyucuyu izleyen imleç ekleme (varsayılan olarak açık) |eşitleme |X |Erişim kolaylığı |
| **Ekran okuyucusu**: Ekran Okuyucusu imlecinin görsel vurgulanmasını etkinleştir (varsayılan olarak açık) |eşitleme |X |Erişim kolaylığı |
| **Ekran okuyucusu**: ses ipuçlarını Oynat (varsayılan olarak açık) |eşitleme |X |Erişim kolaylığı |
| **Ekran okuyucusu**: parmağınızı kaldırdığınızda dokunmatik klavyede tuşları etkinleştirin (varsayılan olarak kapalı) |eşitleme |X |Erişim kolaylığı |
| **Erişim kolaylığı**: yanıp sönen imlecin kalınlığını ayarla |eşitleme |X |Erişim kolaylığı |
| **Erişim kolaylığı**: arka plan görüntülerini Kaldır (varsayılan olarak kapalı) |eşitleme |X |Erişim kolaylığı |
| **Güç ve uyku**: tüm ayarlar |X |X | |
| **Başlangıç ekranı kişiselleştirmesi**: vurgu rengi (yalnızca telefon) |X |eşitleme |Tema |
| **Yazma**: yazım denetimi sözlüğü |eşitleme |yedekleme |Dil |
| **Yazma**: otomatik olarak yanlış yazılan sözcük |eşitleme |yedekleme |Dil |
| **Yazma**: yanlış yazılan sözcükleri Vurgula |eşitleme |yedekleme |Dil |
| **Yazma**: metin önerilerini yazarken göster |eşitleme |yedekleme |Dil |
| **Yazma**: metin önerisi seçtiğimde bir boşluk ekleyin |eşitleme |yedekleme |Dil |
| **Yazma**: boşluk çubuğuna çift dokunduktan sonra bir dönem ekleyin |eşitleme |yedekleme |Dil |
| **Yazma**: her tümcenin ilk harfini büyük harfe çevir |eşitleme |yedekleme |Dil |
| **Yazma**: SHIFT tuşuna çift dokunduktan sonra tüm büyük harfleri kullan |eşitleme |yedekleme |Dil |
| **Yazma**: yazarken anahtar seslerini oynat |eşitleme |yedekleme |Dil |
| **Yazma**: dokunmatik klavye için kişiselleştirme verileri |eşitleme |yedekleme |Dil |
| **Wi-Fi**: Wi-Fi profilleri (yalnızca WPA) |eşitleme |eşitleme |Parolalar |

###### <a name="footnote-1"></a>Dipnot 1

Windows Creators Update 'in desteklenen en düşük işletim sistemi sürümü (derleme 15063). 

## <a name="next-steps"></a>Sonraki adımlar

Genel bakış için bkz. [kuruluş durumu dolaşımına genel bakış](enterprise-state-roaming-overview.md).
