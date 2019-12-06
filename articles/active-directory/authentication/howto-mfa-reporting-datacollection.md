---
title: Azure MFA Kullanıcı verileri toplama-Azure Active Directory
description: Azure Multi-Factor Authentication kullanıcıların kimliğini doğrulamaya yardımcı olmak için hangi bilgiler kullanılır?
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3bc0f0dbcd08df887b2484be6ca8c92a85962c1c
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74848298"
---
# <a name="azure-multi-factor-authentication-user-data-collection"></a>Azure Multi-Factor Authentication Kullanıcı verisi koleksiyonu

Bu belgede, kaldırmak istediğiniz olayda Azure Multi-Factor Authentication Sunucusu (MFA sunucusu) ve Azure MFA (bulut tabanlı) tarafından toplanan kullanıcı bilgilerinin nasıl bulunacağı açıklanmaktadır.

[!INCLUDE [gdpr-hybrid-note](../../../includes/gdpr-hybrid-note.md)]

## <a name="information-collected"></a>Toplanan bilgiler

MFA sunucusu, NPS uzantısı ve Windows Server 2016 Azure MFA AD FS bağdaştırıcısı, aşağıdaki bilgileri toplar ve 90 gün boyunca depolar.

Kimlik doğrulama denemeleri (Raporlama ve sorun giderme için kullanılır):

- Zaman damgası
- Kullanıcı adı
- Ad
- Soyadı
- E-posta Adresi
- User Group
- Kimlik doğrulama yöntemi (telefon araması, SMS mesajı, mobil uygulama, OATH belirteci)
- Telefon araması modu (Standart, PIN)
- Kısa mesaj yönü (tek yönlü, çift yönlü)
- Kısa mesaj modu (OTP, OTP + PIN)
- Mobil uygulama modu (Standart, PIN)
- OATH belirteci modu (Standart, PIN)
- Kimlik Doğrulama Türü
- Uygulama Adı
- Birincil çağrı ülke kodu
- Birincil arama telefon numarası
- Birincil çağrı uzantısı
- Birincil çağrının kimliği doğrulandı
- Birincil çağrı sonucu
- Yedekleme çağrısı ülke kodu
- Yedekleme araması telefon numarası
- Yedekleme çağrısı uzantısı
- Yedekleme çağrısının kimliği doğrulandı
- Yedekleme çağrısı sonucu
- Genel kimlik doğrulamalı
- Genel sonuç
- Sonuçlar
- Denetiminden
- Sonuç
- IP adresi başlatılıyor
- Cihazlar
- Cihaz belirteci
- Cihaz Türü
- Mobil uygulama sürümü
- İşletim Sistemi Sürümü
- Sonuç
- Bildirim için kullanılan denetim

Etkinleştirmeler (Microsoft Authenticator mobil uygulamada bir hesabı etkinleştirmeye çalışır):
- Kullanıcı adı
- Hesap Adı
- Zaman damgası
- Etkinleştirme kodu sonucunu al
- Başarıyı etkinleştir
- Etkinleştirme hatası
- Etkinleştirme durumu sonucu
- Cihaz adı
- Cihaz Türü
- Uygulama Sürümü
- OATH belirteci etkin

Bloklar (engellenen durumu ve raporlamayı tespit etmek için kullanılır):

- Engelleme zaman damgası
- Kullanıcı adına göre engelle
- Kullanıcı adı
- Ülke Kodu
- Telefon Numarası
- Telefon numarası biçimli
- Uzantı
- Uzantıyı temizle
- Engellendi
- Engelleme Nedeni
- Tamamlama zaman damgası
- Tamamlanma nedeni
- Hesap Kilitli
- Sahtekarlık Uyarısı
- Sahtekarlık uyarısı engellenmedi
- Dil

Atlanır (Raporlama için kullanılır):

- Zaman damgasını atla
- Saniye Cinsinden Geçiş Süresi
- Kullanıcı adına göre atla
- Kullanıcı adı
- Ülke Kodu
- Telefon Numarası
- Telefon numarası biçimli
- Uzantı
- Uzantıyı temizle
- Atlama nedeni
- Tamamlama zaman damgası
- Tamamlanma nedeni
- Atlama kullanıldı

Değişiklikler (Kullanıcı değişikliklerini MFA sunucusu veya Azure AD ile eşitlemek için kullanılır):

- Zaman damgasını Değiştir
- Kullanıcı adı
- Yeni ülke kodu
- Yeni telefon numarası
- Yeni uzantı
- Yeni yedek ülke kodu
- Yeni yedek telefon numarası
- Yeni yedekleme uzantısı
- Yeni PIN
- PIN Değişimi Gerekiyor
- Eski cihaz belirteci
- Yeni cihaz belirteci

## <a name="gather-data-from-mfa-server"></a>MFA sunucusundan veri toplama

MFA sunucusu sürüm 8,0 veya üzeri için aşağıdaki işlem, yöneticilerin kullanıcılar için tüm verileri dışarı aktaralmasına izin verir:

- MFA sunucunuzda oturum açın, **Kullanıcılar** sekmesine gidin, söz konusu kullanıcıyı seçin ve **Düzenle** düğmesine tıklayın. Kullanıcıya geçerli MFA ayarlarını sağlamak için her sekmenin ekran görüntülerini (alt-PrtScn) alın.
- MFA sunucusunun komut satırından, JSON biçimli bir dosya oluşturmak için yolu yükleme `C:\Program Files\Multi-Factor Authentication Server\MultiFactorAuthGdpr.exe export <username>` göre değiştirerek aşağıdaki komutu çalıştırın.
- Yöneticiler, belirli bir kullanıcı için toplanan veya daha büyük bir raporlama çözümüne dahil olan tüm MFA bulut hizmeti bilgilerini dışarı aktarma seçeneği olarak Web hizmeti SDK GetUserGdpr işlemini de kullanabilir.
- Eklenen veya değiştirilen Kullanıcı kaydının tüm örneklerini bulmak için, "\<username >" (aramaya tırnak işareti dahil) için `C:\Program Files\Multi-Factor Authentication Server\Logs\MultiFactorAuthSvc.log` ve tüm yedeklemeleri arayın.
   - Bu kayıtlar MFA sunucusu UX, günlük bölümü, günlük dosyaları sekmesindeki **"Kullanıcı değişikliklerini günlüğe kaydet"** seçeneğinin işaretini kaldırarak sınırlı olabilir (ancak ortadan kaldırılamaz).
   - Syslog yapılandırıldıysa ve **"Kullanıcı değişikliklerini günlüğe kaydet"** , MFA sunucusu UX, günlük bölümü, syslog sekmesinde işaretlenirse, bunun yerine günlük girdileri Syslog 'dan toplanabilir.
- MultiFactorAuthSvc. log ve, kimlik doğrulama girişimleriyle ilgili diğer MFA sunucusu günlük dosyalarındaki Kullanıcı adının diğer örnekleri, işletimsel ve MultiFactorAuthGdpr. exe dışarı aktarma veya Web hizmeti SDK 'Sı kullanılarak verilen bilgilere göre ele alınır. GetUserGdpr.

## <a name="delete-data-from-mfa-server"></a>MFA sunucusundan verileri silme

MFA sunucusunun komut satırından, bu kullanıcı için toplanan tüm MFA bulut hizmeti bilgilerini silmek için, yolu yükleme `C:\Program Files\Multi-Factor Authentication Server\MultiFactorAuthGdpr.exe delete <username>` göre değiştirerek aşağıdaki komutu çalıştırın.

- Dışarı aktarmaya dahil edilen veriler gerçek zamanlı olarak silinir, ancak işlemsel veya yinelenen verilerin tamamen kaldırılması 30 güne kadar sürebilir.
- Yöneticiler, belirli bir kullanıcı için toplanan veya daha büyük bir raporlama çözümüne dahil olan tüm MFA bulut hizmeti bilgilerini silme seçeneği olarak Web hizmeti SDK DeleteUserGdpr işlemini de kullanabilir.

## <a name="gather-data-from-nps-extension"></a>NPS uzantısından veri toplama

Dışarı aktarma isteği yapmak için [Microsoft Gizlilik portalı](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) 'nı kullanın.

- MFA bilgileri dışarı aktarmaya dahildir, bu da tamamlanması saat veya gün olabilir.
- AzureMfa/AuthN/AuthNOptCh, AzureMfa/AuthZ/AuthZAdminCh ve AzureMfa/AuthZ/AuthZOptCh olay günlüklerinde Kullanıcı adının oluşumları işletimsel kabul edilir ve dışarı aktarma sırasında verilen bilgilere karşı silinir.

## <a name="delete-data-from-nps-extension"></a>NPS uzantısından verileri silme

Bu Kullanıcı için toplanan tüm MFA bulut hizmeti bilgilerini silmeye yönelik bir hesap isteğinin kapatılmasını sağlamak için [Microsoft Gizlilik portalı](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) 'nı kullanın.

- Verilerin tam olarak kaldırılması 30 güne kadar sürebilir.

## <a name="gather-data-from-windows-server-2016-azure-mfa-ad-fs-adapter"></a>Windows Server 2016 Azure MFA AD FS bağdaştırıcıdan veri toplama

Dışarı aktarma isteği yapmak için [Microsoft Gizlilik portalı](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) 'nı kullanın. 

- MFA bilgileri dışarı aktarmaya dahildir, bu da tamamlanması saat veya gün olabilir.
- AD FS Izleme/hata ayıklama olay günlüklerinde (etkinse) Kullanıcı adının oluşumları işletimsel kabul edilir ve dışarı aktarma sırasında sağlanan bilgilere göre tekrarlanmaktadır.

## <a name="delete-data-from-windows-server-2016-azure-mfa-ad-fs-adapter"></a>Windows Server 2016 Azure MFA AD FS bağdaştırıcısındaki verileri silme

Bu Kullanıcı için toplanan tüm MFA bulut hizmeti bilgilerini silmeye yönelik bir hesap isteğinin kapatılmasını sağlamak için [Microsoft Gizlilik portalı](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) 'nı kullanın.

- Verilerin tam olarak kaldırılması 30 güne kadar sürebilir.

## <a name="gather-data-for-azure-mfa"></a>Azure MFA için veri toplama

Dışarı aktarma isteği yapmak için [Microsoft Gizlilik portalı](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) 'nı kullanın.

- MFA bilgileri dışarı aktarmaya dahildir, bu da tamamlanması saat veya gün olabilir.

## <a name="delete-data-for-azure-mfa"></a>Azure MFA için verileri silme

Bu Kullanıcı için toplanan tüm MFA bulut hizmeti bilgilerini silmeye yönelik bir hesap isteğinin kapatılmasını sağlamak için [Microsoft Gizlilik portalı](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) 'nı kullanın.

- Verilerin tam olarak kaldırılması 30 güne kadar sürebilir.

## <a name="next-steps"></a>Sonraki adımlar

[MFA sunucusu raporlama](howto-mfa-reporting.md)
