---
title: Windows 10 dolaşım ayarları başvurusu - Azure Active Directory
description: ESR ile Windows 10'da gezinilecek veya yedeklenecek ayarlar
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
ms.openlocfilehash: 3a7abc402f1fc2e449e7aac5effdb01b6b941100
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78672620"
---
# <a name="windows-10-roaming-settings-reference"></a>Windows 10 dolaşım ayarları başvurusu

Aşağıda, Windows 10'da gezinilecek veya yedeklenecek ayarların bir listesi verilmiştir. 

## <a name="devices-and-endpoints"></a>Cihazlar ve uç noktalar

Windows 10'da eşitleme, yedekleme ve geri yükleme çerçevesi tarafından desteklenen aygıtların ve hesap türlerinin özeti için aşağıdaki tabloya bakın.

| Hesap türü ve işlemi | Masaüstü | Mobil |
| --- | --- | --- |
| Azure Etkin Dizini: eşitleme |Evet |Hayır |
| Azure Etkin Dizini: yedekleme/geri yükleme |Hayır |Hayır |
| Microsoft hesabı: eşitleme |Evet |Evet |
| Microsoft hesabı: yedekleme/geri yükleme |Hayır |Evet |

## <a name="what-is-backup"></a>Yedekleme nedir?

Windows ayarları genellikle varsayılan olarak eşitlenir, ancak aygıttaki yüklü uygulamaların listesi gibi bazı ayarlar yalnızca yedeklenir. Yedekleme yalnızca mobil aygıtlar içindir ve Şu anda Enterprise State Roaming kullanıcıları için kullanılamaz. Yedekleme bir Microsoft hesabı kullanır ve ayarları ve uygulama verilerini OneDrive'da depolar. Bir kullanıcı Ayarlar uygulamasını kullanarak cihazdaki eşitlemeyi devre dışı ederse, normalde eşitlenen uygulama verileri yalnızca yedek olur. Yedekleme verilerine yalnızca yeni bir aygıtın ilk çalıştırma deneyimi sırasında geri yükleme işlemi yoluyla erişilebilir. Yedeklemeler aygıt ayarları üzerinden devre dışı tutulabilir ve kullanıcının OneDrive hesabı üzerinden yönetilebilir ve silinebilir.

## <a name="windows-settings-overview"></a>Windows Ayarlarına genel bakış

Windows 10 aygıtlarında ayarları eşitlemek/devre dışı kalmak için son kullanıcılar için aşağıdaki ayarlar grupları kullanılabilir.

* Tema: masaüstü arka plan, kullanıcı döşemesi, görev çubuğu konumu, vb. 
* Internet Explorer Ayarları: tarama geçmişi, yazılan URL'ler, sık kullanılanlar, vb. 
* Parolalar: Wi-Fi profilleri de dahil olmak üzere Windows kimlik bilgisi yöneticisi 
* Dil Tercihleri: yazım sözlüğü, sistem dili ayarları 
* Erişim Kolaylığı: anlatıcı, ekran klavyesi, büyüteç 
* Diğer Windows Ayarları: Windows Ayarları ayrıntılarına bakın
* Microsoft Edge tarayıcı ayarı: Microsoft Edge sık kullanılanlar, okuma listesi ve diğer ayarlar

![Ayarlarınızı eşitleyin](./media/enterprise-state-roaming-windows-settings-reference/active-directory-enterprise-state-roaming-syncyoursettings.png)

> [!NOTE]
> Bu makale, Temmuz 2015'te Windows 10 ile başlatılan Microsoft Edge Legacy HTML tabanlı tarayıcı için geçerlidir. Makale, 15 Ocak 2020'de yayımlanan yeni Microsoft Edge Krom tabanlı tarayıcı için geçerli değildir. Yeni Microsoft Edge için Eşitleme davranışı hakkında daha fazla bilgi için [Microsoft Edge Eşitleme](/deployedge/microsoft-edge-enterprise-sync)makalesine bakın.

Microsoft Edge tarayıcı ayar grubu (sık kullanılanlar, okuma listesi) eşitleme, Microsoft Edge tarayıcı Ayarları menüsü seçeneği aracılığıyla son kullanıcılar tarafından etkinleştirilebilir veya devre dışı bırakılır.

![Hesap](./media/enterprise-state-roaming-windows-settings-reference/active-directory-enterprise-state-roaming-edge.png)

Windows 10 sürüm 1803 veya sonraki sürümiçin, Internet Explorer ayar grubu (sık kullanılanlar, yazılan URL'ler) eşitleme, Internet Explorer Ayarları menüsü seçeneği aracılığıyla son kullanıcılar tarafından etkinleştirilebilir veya devre dışı bırakılır. 

![Ayarlar](./media/enterprise-state-roaming-windows-settings-reference/active-directory-enterprise-state-roaming-ie.png)

## <a name="windows-settings-details"></a>Windows Ayarları ayrıntıları

Aşağıdaki tabloda, Ayarlar Grubu sütunundaki diğer girişler, Ayarlar > Hesaplar'a giderek devre dışı kalınabilen ayarlara > ayarlarınızı diğer Windows ayarları> eşitleme dir. 

Ayarlar Grubu sütunundaki dahili girişler, yalnızca uygulamanın kendi içinde eşitleme den devre dışı bırakılabilir ayarları ve uygulamalar veya mobil cihaz yönetimi (MDM) veya Grup İlkesi ayarlarını kullanarak tüm cihaz için eşitleme devre dışı bırakarak bakın.
Dolaşımda olmayan veya eşitlenmemiş ayarlar bir gruba ait olmaz.

| Ayarlar | Masaüstü | Mobil | Grup |
| --- | --- | --- | --- |
| **Hesaplar**: hesap resmi |eşitleme |X |Tema |
| **Hesaplar**: diğer hesap ayarları |X |X | |
| **Gelişmiş mobil geniş bant**: Internet bağlantısı paylaşım ağı adı (Bluetooth üzerinden mobil Wi-Fi etkin noktalarının otomatik olarak keşfedilmesini sağlar) |X |X |Parolalar |
| **Uygulama verileri**: tek tek uygulamalar verileri senkronize edebilir |eşitleme yedekleme |eşitleme yedekleme |internal |
| **Uygulama listesi**: yüklü uygulamaların listesi |X |yedekleme |Diğer |
| **Bluetooth**: tüm Bluetooth ayarları |X |X | |
| **Komut istemi**: Komut istemi "Varsayılanlar" ayarları |eşitleme |X |internal |
| **Kimlik Bilgileri**: Kimlik Bilgileri Dolabı |eşitleme |eşitleme |password |
| **Tarih, Saat ve Bölge**: otomatik saat (Internet zaman eşitleme) |eşitleme |eşitleme |language |
| **Tarih, Saat ve Bölge**: 24 saatlik saat |eşitleme |X |language |
| **Tarih, Saat ve Bölge**: tarih ve saat |eşitleme |X |language |
| **Tarih, Saat ve Bölge**: saat dilimi | |X |language |
| **Tarih, Saat ve Bölge**: gün ışığından yararlanma saati |eşitleme |X |language |
| **Tarih, Saat ve Bölge**: ülke/bölge |eşitleme |X |language |
| **Tarih, Saat ve Bölge**: haftanın ilk günü |eşitleme |X |language |
| **Tarih, Saat ve Bölge**: bölge biçimi (yerel) |eşitleme |X |language |
| **Tarih, Saat ve Bölge**: kısa tarih |eşitleme |X |language |
| **Tarih, Saat ve Bölge**: uzun tarih |eşitleme |X |language |
| **Tarih, Saat ve Bölge**: kısa saat |eşitleme |X |language |
| **Tarih, Saat ve Bölge**: uzun zaman |eşitleme |X |language |
| **Masaüstü kişiselleştirme**: masaüstü Teması (arka plan, sistem rengi, varsayılan sistem sesleri, ekran koruyucu) |eşitleme |X |Tema |
| **Masaüstü kişiselleştirme**: slideshow duvar kağıdı |eşitleme |X |Tema |
| **Masaüstü kişiselleştirme:** görev çubuğu ayarları (konum, otomatik gizleme, vb.) |eşitleme |X |Tema |
| **Masaüstü kişiselleştirme:** ekran düzenini başlat |X |yedekleme | |
| **Cihazlar**: bağladiğiniz paylaşılan yazıcılar |X |X |diğer |
| **Microsoft Edge tarayıcı:** okuma listesi |eşitleme |eşitleme |internal |
| **Microsoft Edge tarayıcı:** sık kullanılanlar |eşitleme |eşitleme |internal |
| **Microsoft Edge tarayıcı**: en iyi siteler <sup> [[1]](#footnote-1)</sup> |eşitleme |eşitleme |internal |
| **Microsoft Edge tarayıcısı**: URL'ler yazdı <sup> [[1]](#footnote-1)</sup> |eşitleme |eşitleme |internal |
| **Microsoft Edge tarayıcı**: sık kullanılanlar çubuğu ayarları <sup> [[1]](#footnote-1)</sup> |eşitleme |eşitleme |internal |
| **Microsoft Edge tarayıcısı**: ana ekran düğmesini göster <sup> [[1]](#footnote-1)</sup> |eşitleme |eşitleme |internal |
| **Microsoft Edge tarayıcı:** blok açılır pencereler <sup> [[1]](#footnote-1)</sup> |eşitleme |eşitleme |internal |
| **Microsoft Edge tarayıcı**: her indirme ile ne yapacağımı bana sorun <sup> [[1]](#footnote-1)</sup> |eşitleme |eşitleme |internal |
| **Microsoft Edge tarayıcı**: parolaları kaydetmek için teklif <sup> [[1]](#footnote-1)</sup> |eşitleme |eşitleme |internal |
| **Microsoft Edge tarayıcı**: gönder isteklerini izlemeyin <sup> [[1]](#footnote-1)</sup> |eşitleme |eşitleme |internal |
| **Microsoft Edge tarayıcısı**: form girişlerini kaydet <sup> [[1]](#footnote-1)</sup> |eşitleme |eşitleme |internal |
| **Microsoft Edge tarayıcı**: ben yazarken arama ve site önerilerini göster <sup> [[1]](#footnote-1)</sup> |eşitleme |eşitleme |internal |
| **Microsoft Edge tarayıcı**: çerezleri tercih <sup> [[1]](#footnote-1)</sup> |eşitleme |eşitleme |internal |
| **Microsoft Edge tarayıcısı**: sitelerin aygıtımda korumalı medya lisanslarını kaydedin <sup> [[1]](#footnote-1)</sup> |eşitleme |eşitleme |internal |
| **Microsoft Edge tarayıcı**: ekran okuyucu ayarı <sup> [[1]](#footnote-1)</sup> |eşitleme |eşitleme |internal |
| **Yüksek Karşıtlık**: A veya Kapalı |eşitleme |X |erişim kolaylığı |
| **Yüksek kontrast**: Tema ayarları |eşitleme |X |erişim kolaylığı |
| **Internet Explorer:** açık sekmeler (URL ve başlık) |eşitleme |eşitleme |Internet Explorer |
| **Internet Explorer**: okuma listesi |eşitleme |eşitleme |Internet Explorer |
| **Internet Explorer**: typed URL'leri |eşitleme |eşitleme |Internet Explorer |
| **Internet Explorer**: tarama geçmişi |eşitleme |eşitleme |Internet Explorer |
| **Internet Explorer**: sık kullanılanlar |eşitleme |eşitleme |Internet Explorer |
| **Internet Explorer**: url'ler hariç |eşitleme |eşitleme |Internet Explorer |
| **Internet Explorer**: ana sayfalar |eşitleme |eşitleme |Internet Explorer |
| **Internet Explorer**: etki alanı önerileri |eşitleme |eşitleme |Internet Explorer |
| **Klavye**: kullanıcılar ekran klavyesi açabilir/kapatabilir |eşitleme |X |erişim kolaylığı |
| **Klavye**: yapışkan evet'i açın (varsayılan olarak kapatın) |eşitleme |X |erişim kolaylığı |
| **Klavye**: filtre tuşlarını açın (varsayılan olarak kapat) |eşitleme |X |erişim kolaylığı |
| **Klavye**: geçiş tuşlarını açın (varsayılan olarak kapatın) |eşitleme |X |erişim kolaylığı |
| **Internet Explorer**: etki alanı Dili: Çince (CHS) QWERTY - kendi kendine öğrenme sağlamak |eşitleme |X |Dil |
| **Dil**: CHS QWERTY - dinamik aday sıralaması etkinleştirin |eşitleme |X |Dil |
| **Dil**: CHS QWERTY - char-set Basitleştirilmiş Çince |eşitleme |X |Dil |
| **Dil**: CHS QWERTY - char-set Geleneksel Çince |eşitleme |X |Dil |
| **Dil**: CHS QWERTY - bulanık pinyin |eşitleme |yedekleme |Dil |
| **Dil**: CHS QWERTY - bulanık çiftleri |eşitleme |yedekleme |Dil |
| **Dil**: CHS QWERTY - tam pinyin |eşitleme |X |Dil |
| **Dil**: CHS QWERTY - çift pinyin |eşitleme |X |Dil |
| **Dil**: CHS QWERTY - okuma otomatik düzeltme |eşitleme |X |Dil |
| **Dil**: CHS QWERTY - C/E anahtar anahtarı, shift |eşitleme |X |Dil |
| **Dil**: CHS QWERTY - C/E anahtar tuşu, Ctrl |eşitleme |X |Dil |
| **Dil**: CHS WUBI - tek karakter giriş modu |eşitleme |X |Dil |
| **Dil**: CHS WUBI - adayın kalan kodlama göstermek |eşitleme |X |Dil |
| **Dil**: CHS WUBI - 4-kodlama geçersiz olduğunda bip |eşitleme |X |Dil |
| **Dil**: CHT Bopomofo - CJK Ext-A dahil |eşitleme |X |Dil |
| **Dil**: Japonca IME - tahmine dayalı yazma ve özel kelimeler |eşitleme |eşitleme |Dil |
| **Dil**: Korece (KOR) IME |X |X |Dil |
| **Dil**: el yazısı tanıma |X |X |Dil |
| **Dil**: dil profili |eşitleme |yedekleme |Dil |
| **Dil**: yazım denetimi - otomatik düzeltme ve yazım hatalarını vurgulamak |eşitleme |yedekleme |Dil |
| **Dil**: klavye listesi |eşitleme |yedekleme |Dil |
| **Kilit Ekranı**: tüm kilit ekranı ayarları |X |X | |
| **Büyüteç**: a veya kapalı (ana geçiş) |X |X |Erişim kolaylığı |
| **Büyüteç**: Ters çevirme rengini açma veya kapatma (varsayılan olarak kapalı) |eşitleme |X |Erişim kolaylığı |
| **Büyüteç**: izleme - klavye odağı izleyin |eşitleme |X |Erişim kolaylığı |
| **Büyüteç**: izleme - fare imlecini izle |eşitleme |X |Erişim kolaylığı |
| **Büyüteç**: Kullanıcılar oturum açtığında başlatın (varsayılan olarak kapalı) |eşitleme |X |Erişim kolaylığı |
| **Fare**: fare imleci boyutunu değiştirmek |eşitleme |X |diğer |
| **Fare**: fare imlecinin rengini değiştirmek |eşitleme |X |diğer |
| **Fare**: diğer tüm ayarlar |X |X | |
| **Anlatıcı**: hızlı fırlatma |eşitleme |X |Erişim kolaylığı |
| **Anlatıcı**: kullanıcılar Ekran Okuyucukonuşma perdesi değiştirebilirsiniz |eşitleme |X |Erişim kolaylığı |
| **Ekran Okuyucusu**: kullanıcılar Ortak öğeler için Ekran Okuyucusu okuma ipuçlarını açabilir veya kapatabilir (varsayılan olarak) |eşitleme |X |Erişim kolaylığı |
| **Ekran Okuyucusu**: kullanıcılar yazılan karakterleri duyup duyamadıklarını açabilir veya kapatabilir (varsayılan olarak) |eşitleme |X |Erişim kolaylığı |
| **Ekran Okuyucusu**: kullanıcılar yazılan sözcükleri duyup duyamadıklarını açabilir veya kapatabilir (varsayılan olarak) |eşitleme |X |Erişim kolaylığı |
| **Ekran Okuyucusu**: Ekran Okuyucusu'ndan sonra kesici ekleme (varsayılan olarak) |eşitleme |X |Erişim kolaylığı |
| **Ekran Okuyucusu**: Ekran Okuyucusunu görsel vurgulamayı etkinleştirin (varsayılan olarak) |eşitleme |X |Erişim kolaylığı |
| **Ekran Okuyucusu**: ses ipuçlarını oynatma (varsayılan olarak açık) |eşitleme |X |Erişim kolaylığı |
| **Ekran Okuyucusu**: parmağınızı kaldırdığınızda dokunmatik klavyedeki tuşları etkinleştirin (varsayılan olarak kapalı) |eşitleme |X |Erişim kolaylığı |
| **Erişim kolaylığı**: yanıp sönen imlecin kalınlığını ayarlayın |eşitleme |X |Erişim kolaylığı |
| **Erişim kolaylığı:** arka plan görüntülerini kaldırma (varsayılan olarak kapalı) |eşitleme |X |Erişim kolaylığı |
| **Güç ve Uyku**: tüm ayarlar |X |X | |
| **Başlangıç ekranı kişiselleştirme:** vurgu rengi (sadece telefon) |X |eşitleme |Tema |
| **Yazma**: yazım sözlüğü |eşitleme |yedekleme |Dil |
| **Yazma**: yanlış yazılmış sözcüğü otomatik olarak düzeltme |eşitleme |yedekleme |Dil |
| **Yazma**: yanlış yazılmış sözcükleri vurgulama |eşitleme |yedekleme |Dil |
| **Yazma**: yazarken metin önerilerini göster |eşitleme |yedekleme |Dil |
| **Yazma**: bir metin önerisi seçtikten sonra boşluk ekleme |eşitleme |yedekleme |Dil |
| **Yazma**: boşluk çubuğuna çift dokunduktan sonra bir nokta ekleyin |eşitleme |yedekleme |Dil |
| **Yazma**: her cümlenin ilk harfini büyük harfe |eşitleme |yedekleme |Dil |
| **Yazma**: shift tuşuna çift dokunduğumda tüm büyük harfleri kullanın |eşitleme |yedekleme |Dil |
| **Yazma**: ben yazarken anahtar sesleri çalma |eşitleme |yedekleme |Dil |
| **Yazma**: dokunmatik klavye için kişiselleştirme verileri |eşitleme |yedekleme |Dil |
| **Wi-Fi**: Wi-Fi profilleri (sadece WPA) |eşitleme |eşitleme |Parolalar |

###### <a name="footnote-1"></a>Dipnot 1

Windows Creators Update(Derleme 15063) en az desteklenen işletim sistemi sürümü. 

## <a name="next-steps"></a>Sonraki adımlar

Genel bakış [için, kurumsal durum dolaşımına genel bakışa](enterprise-state-roaming-overview.md)bakın.
