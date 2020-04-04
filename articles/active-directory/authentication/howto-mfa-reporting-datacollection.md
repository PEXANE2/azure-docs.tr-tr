---
title: Azure MFA kullanıcı veri toplama - Azure Active Directory
description: Kullanıcıların Azure Çok Faktörlü Kimlik Doğrulaması tarafından kimlik doğrulamasına yardımcı olmak için hangi bilgiler kullanılır?
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6f3b5af972ad6dd15b7c992d5e264ede97bd1dde
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80653624"
---
# <a name="azure-multi-factor-authentication-user-data-collection"></a>Azure Çok Faktörlü Kimlik Doğrulama kullanıcı veri toplama

Bu belge, kaldırmak istediğiniz durumlarda Azure Çok Faktörlü Kimlik Doğrulama Sunucusu (MFA Server) ve Azure MFA (Bulut Tabanlı) tarafından toplanan kullanıcı bilgilerinin nasıl bulunacağını açıklar.

[!INCLUDE [gdpr-hybrid-note](../../../includes/gdpr-hybrid-note.md)]

## <a name="information-collected"></a>Toplanan bilgiler

MFA Server, NPS Uzantı ve Windows Server 2016 Azure MFA AD FS Bağdaştırıcısı aşağıdaki bilgileri 90 gün boyunca toplar ve saklar.

Kimlik Doğrulama Denemeleri (raporlama ve sorun giderme için kullanılır):

- Zaman damgası
- Kullanıcı adı
- Ad
- Soyadı
- E-posta Adresi
- Kullanıcı Grubu
- Kimlik Doğrulama Yöntemi (Telefon Görüşmesi, Kısa Mesaj, Mobil Uygulama, YEMIN Belirteci)
- Telefon Arama Modu (Standart, PIN)
- Metin İleti Yönü (Tek Yönlü, İki Yönlü)
- Metin Mesaj Modu (OTP, OTP + PIN)
- Mobil Uygulama Modu (Standart, PIN)
- OATH Belirteç Modu (Standart, PIN)
- Kimlik Doğrulama Türü
- Uygulama Adı
- Birincil Arama Ülke Kodu
- Birincil Arama Telefon Numarası
- Birincil Arama Uzantısı
- Birincil Çağrı Kimlik Doğrulaması
- Birincil Çağrı Sonucu
- Yedek Arama Ülke Kodu
- Yedek Arama Telefon Numarası
- Yedek Arama Uzantısı
- Yedek Arama Kimlik Doğrulaması
- Yedek Arama Sonucu
- Genel Kimlik Doğrulaması
- Genel Sonuç
- Sonuçlar
- Kimlik doğrulaması
- Sonuç
- IP Adresi Başlatma
- Cihazlar
- Aygıt Belirteci
- Cihaz Türü
- Mobil Uygulama Sürümü
- İşletim Sistemi Sürümü
- Sonuç
- Bildirim Için Kullanılan Denetim

Etkinleştirmeler (Microsoft Authenticator mobil uygulamasında bir hesabı etkinleştirme girişimleri):
- Kullanıcı adı
- Hesap Adı
- Zaman damgası
- Etkinleştirme Kodu Sonucu Alın
- Başarıyı Etkinleştir
- Hatayı Etkinleştir
- Etkinleştirme Durumu Sonucu
- Cihaz Adı
- Cihaz Türü
- Uygulama Sürümü
- YEMIN Belirteci Etkin

Bloklar (engellenen durumu belirlemek ve raporlama kiçin kullanılır):

- Blok Zaman Damgası
- Kullanıcı Adına Göre Blok
- Kullanıcı adı
- Ülke Kodu
- Telefon Numarası
- Telefon Numarası Biçimlendirilmiş
- Dahili numara
- Temizleme Uzantısı
- Engellendi
- Blok Nedeni
- Tamamlama Zaman Damgası
- Tamamlama Nedeni
- Hesap Kilitli
- Dolandırıcılık Uyarısı
- Dolandırıcılık Uyarısı Engellenmiyor
- Dil

Baypas (raporlama için kullanılır):

- Zaman Damgasını Bayındır
- Saniye atlama
- Kullanıcı Adına Göre Bypass
- Kullanıcı adı
- Ülke Kodu
- Telefon Numarası
- Telefon Numarası Biçimlendirilmiş
- Dahili numara
- Temizleme Uzantısı
- Baypas Nedeni
- Tamamlama Zaman Damgası
- Tamamlama Nedeni
- Bypass Kullanılan

Değişiklikler (kullanıcı değişikliklerini MFA Server veya Azure AD ile eşitlemek için kullanılır):

- Zaman Damgasını Değiştir
- Kullanıcı adı
- Yeni Ülke Kodu
- Yeni Telefon Numarası
- Yeni Uzantı
- Yeni Yedekleme Ülke Kodu
- Yeni Yedek Telefon Numarası
- Yeni Yedekleme Uzantısı
- Yeni PIN
- PIN Değişimi Gerekli
- Eski Cihaz Belirteci
- Yeni Cihaz Belirteci

## <a name="gather-data-from-mfa-server"></a>MFA Server'dan veri toplama

MFA Server sürüm 8.0 veya üzeri için aşağıdaki işlem yöneticilerin kullanıcılar için tüm verileri dışa aktarmasına olanak tanır:

- MFA Sunucunuzda oturum açın, **Kullanıcılar** sekmesine gidin, söz konusu kullanıcıyı seçin ve **Düzenleme** düğmesini tıklatın. Kullanıcıya geçerli MFA ayarlarını sağlamak için her sekmeye ait ekran görüntülerini (Alt-PrtScn) alın.
- MFA Server'ın komut satırından, JSON biçimlendirilmiş bir `C:\Program Files\Multi-Factor Authentication Server\MultiFactorAuthGdpr.exe export <username>` dosya oluşturmak için yüklemenize göre yolu değiştirerek aşağıdaki komutu çalıştırın.
- Yöneticiler ayrıca, Belirli bir kullanıcı için toplanan tüm MFA bulut hizmeti bilgilerini dışa aktarmak veya daha büyük bir raporlama çözümüne dahil etmek için Web Hizmeti SDK GetUserGdpr işlemini bir seçenek olarak kullanabilir.
- Kullanıcı `C:\Program Files\Multi-Factor Authentication Server\Logs\MultiFactorAuthSvc.log` kaydının eklenen\<veya değiştirilen tüm örneklerini bulmak için " kullanıcı adı>" (aramadaki teklifleri içerir) için yapılan yedeklemeler.
   - Bu kayıtlar, MFA Server UX, Günlük bölümü, Günlük Dosyaları sekmesindeki **"Kullanıcı değişikliklerini günlüğe kaydetme"** işaretlerini kaldırarak sınırlandırılabilir (ancak ortadan kaldırılmaz).
   - Syslog yapılandırılırsa ve MFA Server UX, Günlük bölümü, Syslog sekmesinde **"Kullanıcı değişikliklerini kaydet"** işaretlenirse, günlük girişleri bunun yerine syslog'dan toplanabilir.
- MultiFactorAuthSvc.log ve kimlik doğrulama girişimleri ile ilgili diğer MFA Server günlük dosyaları kullanıcı adının diğer oluşumları operasyonel ve MultiFactorAuthGdpr.exe dışa aktarma veya Web Hizmeti SDK GetUserGdpr kullanılarak sağlanan bilgilere çoğaltılabilir olarak kabul edilir.

## <a name="delete-data-from-mfa-server"></a>MFA Server'dan veri silme

MFA Server'ın komut satırından, bu kullanıcı için toplanan `C:\Program Files\Multi-Factor Authentication Server\MultiFactorAuthGdpr.exe delete <username>` tüm MFA bulut hizmeti bilgilerini silmek için yüklemenize göre yolu değiştirerek aşağıdaki komutu çalıştırın.

- Dışa aktarmada yer alan veriler gerçek zamanlı olarak silinir, ancak operasyonel veya yinelemeli verilerin tamamen kaldırılması 30 gün kadar sürebilir.
- Yöneticiler ayrıca Web Hizmeti SDK DeleteUserGdpr işlemini, belirli bir kullanıcı için toplanan tüm MFA bulut hizmeti bilgilerini silmek veya daha büyük bir raporlama çözümüne dahil etmek için bir seçenek olarak kullanabilirler.

## <a name="gather-data-from-nps-extension"></a>NPS Uzantı'dan veri toplama

Dışa Aktarma için istekte bulunmak için [Microsoft Gizlilik Portalı'nı](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) kullanın.

- MFA bilgileri, tamamlanması saatler veya günler sürebilir dışa aktarmada yer alır.
- AzureMfa/AuthN/AuthNOptCh, AzureMfa/AuthZ/AuthZAdminCh ve AzureMfa/AuthZ/AuthZ/AuthZOptCh olay günlüklerinde kullanıcı adının oluşumları, dışa aktarmada sağlanan bilgilere işlevsel ve yinelemeli olarak kabul edilir.

## <a name="delete-data-from-nps-extension"></a>NPS Uzantısından verileri silme

Bu kullanıcı için toplanan tüm MFA bulut hizmeti bilgilerini silmek için Hesap Kapat için bir istekte bulunmak için [Microsoft Gizlilik Portalı'nı](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) kullanın.

- Verilerin tamamen kaldırılması 30 gün kadar sürebilir.

## <a name="gather-data-from-windows-server-2016-azure-mfa-ad-fs-adapter"></a>Windows Server 2016 Azure MFA AD FS Adaptöründen veri toplama

Dışa Aktarma için istekte bulunmak için [Microsoft Gizlilik Portalı'nı](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) kullanın. 

- MFA bilgileri, tamamlanması saatler veya günler sürebilir dışa aktarmada yer alır.
- AD FS İzleme/Hata Ayıklama olay günlüklerinde (etkinse) kullanıcı adının oluşumları, dışa aktarmada sağlanan bilgilere operasyonel ve yinelemeli olarak kabul edilir.

## <a name="delete-data-from-windows-server-2016-azure-mfa-ad-fs-adapter"></a>Windows Server 2016 Azure MFA AD FS Adaptörü'nden veri silme

Bu kullanıcı için toplanan tüm MFA bulut hizmeti bilgilerini silmek için Hesap Kapat için bir istekte bulunmak için [Microsoft Gizlilik Portalı'nı](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) kullanın.

- Verilerin tamamen kaldırılması 30 gün kadar sürebilir.

## <a name="gather-data-for-azure-mfa"></a>Azure MFA için veri toplama

Dışa Aktarma için istekte bulunmak için [Microsoft Gizlilik Portalı'nı](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) kullanın.

- MFA bilgileri, tamamlanması saatler veya günler sürebilir dışa aktarmada yer alır.

## <a name="delete-data-for-azure-mfa"></a>Azure MFA için Verileri Silme

Bu kullanıcı için toplanan tüm MFA bulut hizmeti bilgilerini silmek için Hesap Kapat için bir istekte bulunmak için [Microsoft Gizlilik Portalı'nı](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) kullanın.

- Verilerin tamamen kaldırılması 30 gün kadar sürebilir.

## <a name="next-steps"></a>Sonraki adımlar

[MFA Server raporlama](howto-mfa-reporting.md)
