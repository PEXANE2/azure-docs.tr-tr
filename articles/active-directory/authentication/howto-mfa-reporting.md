---
title: Azure MFA için erişim ve kullanım raporları - Azure Etkin Dizin
description: Bu, Azure Çok Faktörlü Kimlik Doğrulama özelliğinin nasıl kullanılacağını açıklar - raporlar.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: b47e6bac031a5f2cffc8734fee976fbf8dadc666
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80129078"
---
# <a name="reports-in-azure-multi-factor-authentication"></a>Azure Çok Faktörlü Kimlik Doğrulama'daki Raporlar

Azure Çok Faktörlü Kimlik Doğrulama, azure portalı üzerinden erişilebilen sizin ve kuruluşunuz tarafından kullanılabilecek çeşitli raporlar sağlar. Aşağıdaki tabloda kullanılabilir raporlar listelenmektedir:

| Rapor | Konum | Açıklama |
|:--- |:--- |:--- |
| Engellenen Kullanıcı Geçmişi | Azure AD > Security > MFA > Kullanıcıları Engelleme/Engellemeyi Kaldırma | Kullanıcıları engelleme veya engellemeyi kaldırma isteklerinin geçmişini gösterir. |
| Kullanım ve dolandırıcılık uyarıları | Azure REKLAM > Oturum Açma | Genel kullanım, kullanıcı özeti ve kullanıcı ayrıntıları hakkında bilgi sağlar; belirtilen tarih aralığında gönderilen dolandırıcılık uyarılarının geçmişi. |
| Şirket içi bileşenler için kullanım | Azure AD > Güvenlik > MFA > Faaliyet Raporu | NPS uzantısı, ADFS ve MFA sunucusu aracılığıyla MFA'nın genel kullanımı hakkında bilgi sağlar. |
| Atladı Kullanıcı Geçmişi | Azure AD > Security > MFA > Tek seferlik baypas | Bir kullanıcı için Çok Faktörlü Kimlik Doğrulaması atlamak için isteklerin geçmişini sağlar. |
| Sunucu durumu | Azure AD > Security > MFA > Sunucu durumu | Hesabınızla ilişkili Çok Faktörlü Kimlik Doğrulama Sunucularının durumunu görüntüler. |

## <a name="view-mfa-reports"></a>MFA raporlarını görüntüleme

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Solda, Azure **Etkin Dizin** > **Güvenlik** > **MFA'sını**seçin.
3. Görüntülemek istediğiniz raporu seçin.

   ![Azure portalında MFA Server sunucu durum raporu](./media/howto-mfa-reporting/report.png)

## <a name="azure-ad-sign-ins-report"></a>Azure AD oturum açma raporu

[Azure portalındaki](https://portal.azure.com) **oturum açma etkinlikleri raporuyla,** ortamınızın nasıl olduğunu belirlemek için ihtiyacınız olan bilgileri alabilirsiniz.

Oturum açma raporu, yönetilen uygulamaların kullanımı ve çok faktörlü kimlik doğrulama (MFA) kullanımı hakkında bilgi içeren kullanıcı oturum açma etkinlikleri hakkında size bilgi sağlayabilir. MFA verileri MFA'nın kuruluşunuzdaki kullanımı hakkında öngörüler sunar. Aşağıdaki gibi sorulara yanıt bulmanızı sağlar:

- Oturum açma sırasında MFA kullanıldı mı?
- Kullanıcı MFA'yı nasıl tamamladı?
- Kullanıcının MFA'yı tamamlayamadığı durumlar oldu mu?
- Kaç kullanıcıdan MFA istendi?
- Kaç kullanıcı MFA isteğini tamamlayamadı?
- Son kullanıcıların karşılaştığı ortak MFA sorunları nelerdir?

Bu veriler Azure [portalı](https://portal.azure.com) ve [raporlama API'si](../reports-monitoring/concept-reporting-api.md)aracılığıyla kullanılabilir.

![Azure portalında Azure AD oturum açma raporu](./media/howto-mfa-reporting/sign-in-report.png)

### <a name="sign-ins-report-structure"></a>Oturum açma raporu yapısı

MFA hakkındaki oturum açma etkinliği raporları aşağıdaki bilgilere erişmenizi sağlar:

**MFA gerekli:** Oturum açma işlemi için MFA'nın gerekli olup olmadığı. MFA, kullanıcı başına MFA, Koşullu Erişim veya diğer nedenlerden dolayı gerekli olabilir. Olası değerler **Evet** veya **Hayır'dır.**

**MFA Sonucu:** MFA'nın tamamlanıp tamamlanmadığı hakkında daha fazla bilgi:

- MFA tamamlandıysa bu sütunda MFA'nın tamamlanma şekli hakkında daha fazla bilgi yer alır.
   - Azure Multi-Factor Authentication
      - bulutta tamamlandı
      - kiracıda yapılandırılmış ilkeler nedeniyle süresi doldu
      - kayıt istendi
      - belirteç talebiyle gerçekleştirildi
      - harici sağlayıcı tarafından sağlanan taleple gerçekleştirildi
      - güçlü kimlik doğrulamasıyla gerçekleştirildi
      - gerçekleştirilen akış Windows aracısı oturum açma akışı tarafı olduğundan atlandı
      - uygulama parolası nedeniyle atlandı
      - konum nedeniyle atlandı
      - kayıtlı cihaz nedeniyle atlandı
      - hatırlanan cihaz nedeniyle atlandı
      - başarıyla tamamlandı
   - Çok faktörlü kimlik doğrulaması için harici sağlayıcıya yönlendirildi

- MFA reddedildiyse bu sütunda reddedilme nedeni yer alır.
   - Azure Multi-Factor Authentication tarafından reddedildi;
      - kimlik doğrulaması devam ediyor
      - yinelenen kimlik doğrulaması girişimi
      - çok fazla kez yanlış kod girildi
      - geçersiz kimlik doğrulaması
      - geçersiz mobil uygulama doğrulama kodu
      - yanlış yapılandırma
      - telefon araması sesli mesaja düştü
      - telefon numarası biçimi geçersiz
      - hizmet hatası
      - kullanıcının telefonuna ulaşılamadı
      - cihaza mobil uygulama bildirimi gönderilemedi
      - mobil uygulama bildirimi gönderilemedi
      - kullanıcı kimlik doğrulamasını reddetti
      - kullanıcı mobil uygulama bildirimine yanıt vermedi
      - kullanıcının kayıtlı doğrulama yöntemi yok
      - kullanıcı hatalı kod girdi
      - kullanıcı hatalı PIN girdi
      - kullanıcı kimlik doğrulaması başarılı olmadan telefon aramasını sonlandırdı
      - kullanıcı engellendi
      - kullanıcı doğrulama kodunu hiç girmedi
      - kullanıcı bulunamadı
      - doğrulama kodu zaten bir kez kullanıldı

**MFA kimlik doğrulama yöntemi:** Kullanıcının MFA'yı tamamlaması için kullandığı kimlik doğrulama yöntemi. Olası değerler şunlardır:

- Kısa mesaj
- Mobil uygulama bildirimi
- Telefon araması (Kimlik doğrulama telefonu)
- Mobil uygulama doğrulama kodu
- Telefon araması (Ofis telefonu)
- Telefon araması (Alternatif kimlik doğrulama telefonu)

**MFA kimlik doğrulama ayrıntısı:** Telefon numarasının temizlenmiş sürümü. Örnek: +X XXXXXXXX64.

**Koşullu Erişim** Oturum açma girişimini etkileyen Koşullu Erişim ilkeleri hakkında aşağıdakiler dahil olmak üzere bilgi bulun:

- İlke adı
- Hibe denetimleri
- Oturum denetimleri
- Sonuç

## <a name="powershell-reporting-on-users-registered-for-mfa"></a>PowerShell MFA için kayıtlı kullanıcılar hakkında raporlama

İlk olarak, [MSOnline V1 PowerShell modülünün](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-1.0) takılı olduğundan emin olun.

Aşağıdaki PowerShell'i kullanarak MFA'ya kaydolan kullanıcıları belirleyin. Bu komut kümesi, bu hesapların Azure AD'ye karşı kimliği belirlenemediği için devre dışı bırakılan kullanıcıları hariç tutar.

```Get-MsolUser -All | Where-Object {$.StrongAuthenticationMethods -ne $null -and $.BlockCredential -eq $False} | Select-Object -Property UserPrincipalName```

Aşağıdaki PowerShell'i kullanarak MFA'ya kaydolmayan kullanıcıları belirleyin. Bu komut kümesi, bu hesapların Azure AD'ye karşı kimliği belirlenemediği için devre dışı bırakılan kullanıcıları hariç tutar.

```Get-MsolUser -All | Where-Object {$.StrongAuthenticationMethods.Count -eq 0 -and $.BlockCredential -eq $False} | Select-Object -Property UserPrincipalName```

Kayıtlı kullanıcıları ve çıktı yöntemlerini tanımlayın. 

```PowerShell
Get-MsolUser -All | Select-Object @{N='UserPrincipalName';E={$_.UserPrincipalName}},

@{N='MFA Status';E={if ($_.StrongAuthenticationRequirements.State){$_.StrongAuthenticationRequirements.State} else {"Disabled"}}},

@{N='MFA Methods';E={$_.StrongAuthenticationMethods.methodtype}} | Export-Csv -Path c:\MFA_Report.csv -NoTypeInformation
```

## <a name="possible-results-in-activity-reports"></a>Faaliyet raporlarında olası sonuçlar

Aşağıdaki tablo, çok faktörlü kimlik doğrulama faaliyet raporunun indirilen sürümünü kullanarak çok faktörlü kimlik doğrulamayı gidermek için kullanılabilir. Bunlar doğrudan Azure portalında görünmez.

| Arama Sonucu | Açıklama | Geniş açıklama |
| --- | --- | --- |
| SUCCESS_WITH_PIN | PIN Girilen | Kullanıcı bir PIN girdi. Kimlik doğrulama başarılı olduysa, doğru PIN'i girdiler. Kimlik doğrulaması reddedilirse, yanlış bir PIN girer veya kullanıcı Standart modolarak ayarlanır. |
| SUCCESS_NO_PIN | Yalnızca # Girilen | Kullanıcı PIN moduna ayarlanırsa ve kimlik doğrulaması reddedilirse, bu kullanıcının PIN'ini girmediği ve yalnızca #girdiği anlamına gelir.  Kullanıcı Standart modda ayarlanmışsa ve kimlik doğrulaması başarılı olursa, bu kullanıcının yalnızca Standart modda yapılacak doğru şey olan #'a girdiği anlamına gelir. |
| SUCCESS_WITH_PIN_BUT_TIMEOUT | # Girişten Sonra Basılmayacak | # girilmediğinden kullanıcı herhangi bir DTMF rakamı göndermedi. # girilmedikçe girilen diğer basamaklar, girişin tamamlanındığını gösteren bir girilmediği sürece gönderilmez. |
|SUCCESS_NO_PIN_BUT_TIMEOUT | Telefon Girişi Yok - ZamanLanmış Çıkış | Arama yanıtlandı, ancak yanıt gelmedi. Bu genellikle aramanın sesli mesajla alındığını gösterir. |
| SUCCESS_PIN_EXPIRED | PIN'in Süresi Doldu ve Değiştirilmedi | Kullanıcının PIN süresi doldu ve değiştirmeleri istendi, ancak PIN değişikliği başarıyla tamamlanmadı. |
| SUCCESS_USED_CACHE | İkinci El Önbellek | Kimlik doğrulama, yapılandırılan önbellek zaman dilimi içinde aynı kullanıcı adı için önceki başarılı kimlik doğrulaması oluştuğundan, Çok Faktörlü Kimlik Doğrulama çağrısı olmadan başarılı oldu. |
| SUCCESS_BYPASSED_AUTH | Bypass Auth | Kimlik doğrulama, kullanıcı için başlatılan bir One-Time Bypass kullanılarak başarılı oldu. Bypass hakkında daha fazla bilgi için Atlı Kullanıcı Geçmişi Raporu'na bakın. |
| SUCCESS_USED_IP_BASED_CACHE | İkinci el IP tabanlı Önbellek | Kimlik doğrulama, yapılandırılan önbellek zaman dilimi içinde gerçekleşen aynı kullanıcı adı, kimlik doğrulama türü, uygulama adı ve IP için önceki başarılı kimlik doğrulaması olduğundan, Çok Faktörlü Kimlik Doğrulama çağrısı olmadan başarılı oldu. |
| SUCCESS_USED_APP_BASED_CACHE | İkinci El Uygulama Tabanlı Önbellek | Kimlik doğrulama, yapılandırılan önbellek zaman dilimi içinde aynı kullanıcı adı, kimlik doğrulama türü ve uygulama adı için önceki başarılı kimlik doğrulaması beri çok faktörlü kimlik doğrulama çağrısı olmadan başarılı oldu. |
| SUCCESS_INVALID_INPUT | Geçersiz Telefon Girişi | Telefondan gönderilen yanıt geçerli değildir. Bu bir faks makinesi veya modem den olabilir veya kullanıcı kendi PIN bir parçası olarak * girmiş olabilir. |
| SUCCESS_USER_BLOCKED | Kullanıcı Engellendi | Kullanıcının telefon numarası engellendi. Engellenen bir numara, kullanıcı tarafından kimlik doğrulama çağrısı sırasında veya Azure portalını kullanan bir yönetici tarafından başlatılabilir. <br> NOT: Engellenen numara, dolandırıcılık uyarısının bir yan ürünüdür. |
| SUCCESS_SMS_AUTHENTICATED | Metin İleti Kimlik Doğrulaması | İki yönlü test iletisi için, kullanıcı tek seferlik parola (OTP) veya OTP + PIN ile doğru yanıtverdi. |
| SUCCESS_SMS_SENT | Gönderilen Kısa Mesaj | Kısa Mesaj için, tek seferlik parolayı (OTP) içeren kısa mesaj başarıyla gönderildi. Kullanıcı kimlik doğrulamasını tamamlamak için uygulamaya OTP veya OTP + PIN girer. |
| SUCCESS_PHONE_APP_AUTHENTICATED | Mobil Uygulama Kimlik Doğrulaması | Kullanıcı mobil uygulama üzerinden başarıyla doğrulanmış. |
| SUCCESS_OATH_CODE_PENDING | BEKLEMEDE OATH Kodu | Kullanıcı, OATH kodu için istendi, ancak yanıt vermedi. |
| SUCCESS_OATH_CODE_VERIFIED | YEMIN Kodu Doğrulandı | Kullanıcı istendiğinde geçerli bir OATH kodu girdi. |
| SUCCESS_FALLBACK_OATH_CODE_VERIFIED | Geri Dönüş YEMINi Kodu Doğrulandı | Kullanıcı, birincil Çok Faktörlü Kimlik Doğrulama yöntemini kullanarak kimlik doğrulaması reddedildi ve ardından geri dönüş için geçerli bir OATH kodu sağladı. |
| SUCCESS_FALLBACK_SECURITY_QUESTIONS_ANSWERED | Geri Dönüş Güvenlik Soruları Yanıtlandı | Kullanıcı, birincil Çok Faktörlü Kimlik Doğrulama yöntemini kullanarak kimlik doğrulaması reddedildi ve ardından geri dönüş için güvenlik sorularını doğru bir şekilde yanıtladı. |
| FAILED_PHONE_BUSY | Auth Zaten Devam Ediyor | Çok Faktörlü Kimlik Doğrulama zaten bu kullanıcı için bir kimlik doğrulama işleme. Bunun nedeni genellikle, aynı oturum açma sırasında birden çok kimlik doğrulama isteği gönderen RADIUS istemcileri dir. |
| CONFIG_ISSUE | Telefon Erişilemez | Arama denendi, ancak ya yerleştirilemedi veya yanıtlanmadı. Buna yoğun sinyal, hızlı meşgul sinyali (bağlantısı kesilmiş), üç tonları (artık hizmette olmayan numara), zil sırasında zaman aşımı, vb. dahildir. |
| FAILED_INVALID_PHONENUMBER | Geçersiz Telefon Numarası Biçimi | Telefon numarasıgeçersiz bir biçime sahiptir. Telefon numaraları sayısal olmalıdır ve ülke kodu +1 için 10 basamak olmalıdır (Amerika Birleşik Devletleri & Kanada). |
| FAILED_USER_HUNGUP_ON_US | Kullanıcı Telefonu Kapattı | Kullanıcı telefona cevap verdi, ancak daha sonra herhangi bir düğmeye basmadan kapattı. |
| FAILED_INVALID_EXTENSION | Geçersiz Uzantı | Uzantı geçersiz karakterler içerir. Yalnızca basamaklara, virgüllere ve #'ya izin verilir. @ öneki de kullanılabilir. |
| FAILED_FRAUD_CODE_ENTERED | Dolandırıcılık Kodu Girilen | Arama sırasında sahtekarlığı bildirmeyi seçen kullanıcı, kimlik doğrulamanın reddedilmesi ve engellenen bir telefon numarasıyla sonuçlanır.| 
| FAILED_SERVER_ERROR | Çağrı Yerleştirilemiyor | Çok Faktörlü Kimlik Doğrulama hizmeti aramayı yerleştiremedi. |
| FAILED_SMS_NOT_SENT | Kısa Mesaj Gönderilemedi | Kısa mesaj gönderilemedi. Kimlik doğrulaması reddedildi. |
| FAILED_SMS_OTP_INCORRECT | Metin Mesajı OTP Yanlış | Kullanıcı, aldıkları kısa mesajdan tek seferlik yanlış bir parola (OTP) girdi. Kimlik doğrulaması reddedildi. |
| FAILED_SMS_OTP_PIN_INCORRECT | Metin Mesajı OTP + PIN Yanlış | Kullanıcı yanlış bir tek seferlik parola (OTP) ve/veya yanlış kullanıcı PIN'i girdi. Kimlik doğrulaması reddedildi. |
| FAILED_SMS_MAX_OTP_RETRY_REACHED | Aşıldı Maksimum Metin Mesajı OTP Girişimleri | Kullanıcı, tek seferlik parola (OTP) denemelerinin maksimum sayısını aşmıştır. |
| FAILED_PHONE_APP_DENIED | Mobil Uygulama Reddedildi | Kullanıcı, Reddet düğmesine basarak mobil uygulamadaki kimlik doğrulamasını reddetti. |
| FAILED_PHONE_APP_INVALID_PIN | Mobil Uygulama Geçersiz PIN | Kullanıcı, mobil uygulamada kimlik doğrulaması yaparken geçersiz bir PIN girdi. |
| FAILED_PHONE_APP_PIN_NOT_CHANGED | Mobil Uygulama PIN'i Değiştirilmedi | Kullanıcı mobil uygulamada gerekli PIN değişikliğini başarıyla tamamlayamadı. |
| FAILED_FRAUD_REPORTED | Dolandırıcılık Bildirilen | Kullanıcı mobil uygulamada sahtekarlık olduğunu bildirdi. |
| FAILED_PHONE_APP_NO_RESPONSE | Mobil Uygulama Yanıt Yok | Kullanıcı mobil uygulama kimlik doğrulama isteğine yanıt vermedi. |
| FAILED_PHONE_APP_ALL_DEVICES_BLOCKED | Mobil Uygulama Tüm Cihazlar Engellendi | Bu kullanıcının mobil uygulama aygıtları artık bildirimlere yanıt vermiyor ve engellendi. |
| FAILED_PHONE_APP_NOTIFICATION_FAILED | Mobil Uygulama Bildirimi Başarısız Oldu | Kullanıcının cihazındaki mobil uygulamaya bildirim göndermeye çalışırken bir hata oluştu. |
| FAILED_PHONE_APP_INVALID_RESULT | Mobil Uygulama Geçersiz Sonuç | Mobil uygulama geçersiz bir sonuç döndürür. |
| FAILED_OATH_CODE_INCORRECT | OATH Kodu Yanlış | Kullanıcı yanlış bir OATH kodu girdi.  Kimlik doğrulaması reddedildi. |
| FAILED_OATH_CODE_PIN_INCORRECT | OATH Kodu + PIN Yanlış | Kullanıcı yanlış bir OATH kodu ve/veya yanlış kullanıcı PIN'i girdi.  Kimlik doğrulaması reddedildi. |
| FAILED_OATH_CODE_DUPLICATE | Yinelenen YEMIN Kodu | Kullanıcı daha önce kullanılan bir OATH kodu girdi.  Kimlik doğrulaması reddedildi. |
| FAILED_OATH_CODE_OLD | OATH Kodu Güncel Değil | Kullanıcı, daha önce kullanılan bir OATH kodundan önce gelen bir OATH kodu girdi.  Kimlik doğrulaması reddedildi. |
| FAILED_OATH_TOKEN_TIMEOUT | YEMIN Kodu Sonuç Zaman Dilimi | Kullanıcının OATH kodunu girmesi çok uzun sürdü ve Çok Faktörlü Kimlik Doğrulama denemesi çoktan zamanlanmıştı. |
| FAILED_SECURITY_QUESTIONS_TIMEOUT | Güvenlik Soruları Sonuç Zaman | Kullanıcının güvenlik sorularının yanıtını girmesi çok uzun sürdü ve Çok Faktörlü Kimlik Doğrulama denemesi çoktan zamanlanmıştı. |
| FAILED_AUTH_RESULT_TIMEOUT | Auth Sonuç Zaman Dilimi | Kullanıcının Çok Faktörlü Kimlik Doğrulama denemesini tamamlaması çok uzun sürdü. |
| FAILED_AUTHENTICATION_THROTTLED | Kimlik Doğrulama Daraltılmış | Çok Faktörlü Kimlik Doğrulama denemesi hizmet tarafından azaltıldı. |

## <a name="next-steps"></a>Sonraki adımlar

* [SSPR ve MFA kullanımı ve öngörüraporlama](howto-authentication-methods-usage-insights.md)
* [Kullanıcılar İçin](../user-help/multi-factor-authentication-end-user.md)
* [Nerede dağıtılır](concept-mfa-whichversion.md)
