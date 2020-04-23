---
title: Azure AD Connect tarafından senkronize edilen özellikler | Microsoft Dokümanlar
description: Azure Etkin Dizini'ne eşitlenen öznitelikleri listeler.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: c2bb36e0-5205-454c-b9b6-f4990bcedf51
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 04/15/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: ab51030ad39e1360cabc7d63390af7c1654d2891
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82082127"
---
# <a name="azure-ad-connect-sync-attributes-synchronized-to-azure-active-directory"></a>Azure AD Connect eşitleme: Azure Active Directory ile eşitlenen öznitelikler
Bu konu, Azure AD Connect eşitlemi tarafından eşitlenen öznitelikleri listeler.  
Öznitelikler, ilgili Azure AD uygulamasına göre gruplandırılır.

## <a name="attributes-to-synchronize"></a>Eşitlemek için öznitelikleri
Sık sorulan soru, *eşitlemek için en az özniteliklerin listesinin ne olduğudur.* Varsayılan ve önerilen yaklaşım, bulutta tam bir GAL (Global Adres Listesi) oluşturulabilmesi ve Office 365 iş yüklerinde tüm özellikleri niçin alabilmesi için varsayılan öznitelikleri tutmaktır. Bazı durumlarda, bu öznitelikler bu örnekte olduğu gibi hassas veya kişisel olarak tanımlanabilir bilgiler içerdiğinden kuruluşunuzun bulutla eşitlemesini istemediği bazı öznitelikler vardır:  
![kötü öznitelikleri](./media/reference-connect-sync-attributes-synchronized/badextensionattribute.png)

Bu durumda, bu konudaki özniteliklerin listesiyle başlayın ve hassas veya kişisel bilgi işlem verileri içeren ve eşitlenemeyen öznitelikleri tanımlayın. Ardından Azure AD uygulamasını kullanarak yükleme sırasında bu öznitelikleri seçin ve filtreleme özelliğini [belirleyin.](how-to-connect-install-custom.md#azure-ad-app-and-attribute-filtering)

> [!WARNING]
> Öznitelikleri seçerken dikkatli olmalısınız ve yalnızca eşitlemek kesinlikle mümkün olmayan bu öznitelikleri seçin. Diğer öznitelikleri seçilme, özellikler üzerinde olumsuz bir etki yaratabilir.
>
>

## <a name="office-365-proplus"></a>Office 365 ProPlus
| Öznitelik Adı | Kullanıcı | Açıklama |
| --- |:---:| --- |
| hesapEtkin |X |Bir hesap etkinleştirilip etkinleştirilen ini tanımlar. |
| Cn |X | |
| displayName |X | |
| objectSID |X |mekanik özellik. Azure AD ve AD arasında eşitleme sağlamak için kullanılan AD kullanıcı tanımlayıcısı. |
| pwdLastSet |X |mekanik özellik. Zaten verilmiş jetonları geçersiz kılınacağını bilmek için kullanılır. Her iki parola karma eşitleme, geçiş kimlik doğrulaması ve federasyon tarafından kullanılır. |
|samAccountName|X| |
| kaynakÇapa |X |mekanik özellik. ADDS ve Azure AD arasındaki ilişkiyi sürdürmek için değişmez tanımlayıcı. |
| kullanımKonum |X |mekanik özellik. Kullanıcının ülkesi/bölgesi. Lisans ataması için kullanılır. |
| userPrincipalName |X |UPN, kullanıcının giriş kimliğidir. Çoğu zaman [posta] değeriyle aynıdır. |

## <a name="exchange-online"></a>Exchange Online
| Öznitelik Adı | Kullanıcı | İletişim | Grup | Açıklama |
| --- |:---:|:---:|:---:| --- |
| hesapEtkin |X | | |Bir hesap etkinleştirilip etkinleştirilen ini tanımlar. |
| Yardımcısı |X |X | | |
| altAlıcı |X | | |Azure AD Connect oluşturma 1.1.552.0 veya sonrası gerektirir. |
| authOrig |X |X |X | |
| c |X |X | | |
| Cn |X | |X | |
| Co |X |X | | |
| şirket |X |X | | |
| ülkeKodu |X |X | | |
| bölüm |X |X | | |
| açıklama | | |X | |
| displayName |X |X |X | |
| dLMemRejectPerms |X |X |X | |
| dLMemSubmitPerms |X |X |X | |
| extensionAttribute1 |X |X |X | |
| uzatmaÖznitelik10 |X |X |X | |
| uzatmaÖznitelik11 |X |X |X | |
| extensionAttribute12 |X |X |X | |
| uzatmaÖznitelik13 |X |X |X | |
| uzatmaÖznitelik14 |X |X |X | |
| uzatmaÖznitelik15 |X |X |X | |
| extensionAttribute2 |X |X |X | |
| extensionAttribute3 |X |X |X | |
| extensionAttribute4 |X |X |X | |
| extensionAttribute5 |X |X |X | |
| extensionAttribute6 |X |X |X | |
| extensionAttribute7 |X |X |X | |
| extensionAttribute8 |X |X |X | |
| extensionAttribute9 |X |X |X | |
| fakstelefon numarası |X |X | | |
| givenName |X |X | | |
| anasayfaTelefon |X |X | | |
| bilgiler |X |X |X |Bu öznitelik şu anda gruplar için tüketilmez. |
| Baş harfler |X |X | | |
| l |X |X | | |
| legacyExchangeDN |X |X |X | |
| Mailnickname |X |X |X | |
| managedBy | | |X | |
| manager |X |X | | |
| üye | | |X | |
| mobil |X |X | | |
| msDS-HABSeniorityIndex |X |X |X | |
| msDS-FonetikEkran Adı |X |X |X | |
| msExchArşivGUID |X | | | |
| msExchArchiveName |X | | | |
| msExchAssistantName |X |X | | |
| msExchAuditAdmin |X | | | |
| msExchAuditDelegate |X | | | |
| msExchAuditDelegeAdmin |X | | | |
| msExchAuditSahibi |X | | | |
| msExchBlockedSendersHash |X |X | | |
| msExchBypassAudit |X | | | |
| msExchBypassModerationLink | | |X |Azure AD Connect sürümü 1.1.524.0 sürümünde kullanılabilir |
| msExchCoManagedByLink | | |X | |
| msExchDelegeListLink |X | | | |
| msExchELCExpirySuspensionEnd |X | | | |
| msExchELCExpirySuspensionStart |X | | | |
| msExchELCMailboxFlags |X | | | |
| msExchEnableModeration |X | |X | |
| msExchExtensionCustomAttribute1 |X |X |X |Bu özellik şu anda Exchange Online tarafından tüketilmesin. |
| msExchExtensionCustomAttribute2 |X |X |X |Bu özellik şu anda Exchange Online tarafından tüketilmesin. |
| msExchExtensionCustomAttribute3 |X |X |X |Bu özellik şu anda Exchange Online tarafından tüketilmesin. |
| msExchExtensionCustomAttribute4 |X |X |X |Bu özellik şu anda Exchange Online tarafından tüketilmesin. |
| msExchExtensionCustomAttribute5 |X |X |X |Bu özellik şu anda Exchange Online tarafından tüketilmesin. |
| msExchHideFromAdresListeleri |X |X |X | |
| msExchImmutableID |X | | | |
| msExchLitigationHoldDate |X |X |X | |
| msExchLitigationHoldSahibi |X |X |X | |
| msExchMailboxAuditEnable |X | | | |
| msExchMailboxAuditLogAgeLimit |X | | | |
| msExchMailboxGuid |X | | | |
| msExchOrtadByLink |X |X |X | |
| msExchModerationBayraklar |X |X |X | |
| msExchRecipientDisplayType |X |X |X | |
| msExchRecipientTypeDetails |X |X |X | |
| msExchRemoteRecipientType |X | | | |
| msExchRequireAuthToSendTo |X |X |X | |
| msExchResourceCapacity |X | | | |
| msExchResourceDisplay |X | | | |
| msExchResourceMetaData |X | | | |
| msExchResourceSearchProperties |X | | | |
| msExchRetentionYorum |X |X |X | |
| msExchRetentionURL |X |X |X | |
| msExchSafeAlıcılarHash |X |X | | |
| msExchSafeSendersHash |X |X | | |
| msExchSenderHintÇeviriler |X |X |X | |
| msExchTeamMailboxExpiration |X | | | |
| msExchTeamMailboxSahipleri |X | | | |
| msExchTeamMailboxSharePointUrl |X | | | |
| msExchUserHoldPolitikalar |X | | | |
| msOrg-IsOrganizational | | |X | |
| objectSID |X | |X |mekanik özellik. Azure AD ve AD arasında eşitleme sağlamak için kullanılan AD kullanıcı tanımlayıcısı. |
| oOFReplyToOriginator | | |X | |
| diğerFacsimileTelefon |X |X | | |
| diğerHomePhone |X |X | | |
| diğerTelefon |X |X | | |
| Çağrı cihazı |X |X | | |
| physicalDeliveryOfficeName |X |X | | |
| Postakodu |X |X | | |
| proxyAddresses |X |X |X | |
| publicDelegeler |X |X |X | |
| pwdLastSet |X | | |mekanik özellik. Zaten verilmiş jetonları geçersiz kılınacağını bilmek için kullanılır. Hem parola eşitleme hem de federasyon tarafından kullanılır. |
| raporToOriginator | | |X | |
| reportToOwner | | |X | |
| sn |X |X | | |
| kaynakÇapa |X |X |X |mekanik özellik. ADDS ve Azure AD arasındaki ilişkiyi sürdürmek için değişmez tanımlayıcı. |
| st |X |X | | |
| Streetaddress |X |X | | |
| Targetaddress |X |X | | |
| telephoneAssistant |X |X | | |
| Telephonenumber |X |X | | |
| küçük resim fotoğrafı |X |X | | |
| başlık |X |X | | |
| unauthOrig |X |X |X | |
| kullanımKonum |X | | |mekanik özellik. Kullanıcının ülkesi/bölgesi. Lisans ataması için kullanılır. |
| userCertificate |X |X | | |
| userPrincipalName |X | | |UPN, kullanıcının giriş kimliğidir. Çoğu zaman [posta] değeriyle aynıdır. |
| kullanıcıSMIMECertificates |X |X | | |
| wWWHomePage |X |X | | |

## <a name="sharepoint-online"></a>SharePoint Online
| Öznitelik Adı | Kullanıcı | İletişim | Grup | Açıklama |
| --- |:---:|:---:|:---:| --- |
| hesapEtkin |X | | |Bir hesap etkinleştirilip etkinleştirilen ini tanımlar. |
| authOrig |X |X |X | |
| c |X |X | | |
| Cn |X | |X | |
| Co |X |X | | |
| şirket |X |X | | |
| ülkeKodu |X |X | | |
| bölüm |X |X | | |
| açıklama |X |X |X | |
| displayName |X |X |X | |
| dLMemRejectPerms |X |X |X | |
| dLMemSubmitPerms |X |X |X | |
| extensionAttribute1 |X |X |X | |
| uzatmaÖznitelik10 |X |X |X | |
| uzatmaÖznitelik11 |X |X |X | |
| extensionAttribute12 |X |X |X | |
| uzatmaÖznitelik13 |X |X |X | |
| uzatmaÖznitelik14 |X |X |X | |
| uzatmaÖznitelik15 |X |X |X | |
| extensionAttribute2 |X |X |X | |
| extensionAttribute3 |X |X |X | |
| extensionAttribute4 |X |X |X | |
| extensionAttribute5 |X |X |X | |
| extensionAttribute6 |X |X |X | |
| extensionAttribute7 |X |X |X | |
| extensionAttribute8 |X |X |X | |
| extensionAttribute9 |X |X |X | |
| fakstelefon numarası |X |X | | |
| givenName |X |X | | |
| hideDLÜyelik | | |X | |
| ev telefonu |X |X | | |
| bilgiler |X |X |X | |
| Baş harf -leri |X |X | | |
| ipPhone |X |X | | |
| l |X |X | | |
| posta |X |X |X | |
| Mailnickname |X |X |X | |
| managedBy | | |X | |
| manager |X |X | | |
| üye | | |X | |
| Middlename |X |X | | |
| mobil |X |X | | |
| msExchTeamMailboxExpiration |X | | | |
| msExchTeamMailboxSahipleri |X | | | |
| msExchTeamMailboxSharePointLinkedBy |X | | | |
| msExchTeamMailboxSharePointUrl |X | | | |
| objectSID |X | |X |mekanik özellik. Azure AD ve AD arasında eşitleme sağlamak için kullanılan AD kullanıcı tanımlayıcısı. |
| oOFReplyToOriginator | | |X | |
| diğerFacsimileTelefon |X |X | | |
| diğerHomePhone |X |X | | |
| Otherıpphone |X |X | | |
| diğerMobil |X |X | | |
| otherPager |X |X | | |
| diğerTelefon |X |X | | |
| Çağrı cihazı |X |X | | |
| physicalDeliveryOfficeName |X |X | | |
| Postakodu |X |X | | |
| postOfficeBox |X |X | |Bu öznitelik şu anda SharePoint Online tarafından tüketilmesin. |
| tercihDil |X | | | |
| proxyAddresses |X |X |X | |
| pwdLastSet |X | | |mekanik özellik. Zaten verilmiş jetonları geçersiz kılınacağını bilmek için kullanılır. Her iki parola karma eşitleme, geçiş kimlik doğrulaması ve federasyon tarafından kullanılır. |
| raporToOriginator | | |X | |
| reportToOwner | | |X | |
| sn |X |X | | |
| kaynakÇapa |X |X |X |mekanik özellik. ADDS ve Azure AD arasındaki ilişkiyi sürdürmek için değişmez tanımlayıcı. |
| st |X |X | | |
| Streetaddress |X |X | | |
| Targetaddress |X |X | | |
| telephoneAssistant |X |X | | |
| Telephonenumber |X |X | | |
| küçük resim fotoğrafı |X |X | | |
| başlık |X |X | | |
| unauthOrig |X |X |X | |
| url |X |X | | |
| kullanımKonum |X | | |mekanik özellik. Kullanıcının ülkesi/bölgesi
. Lisans ataması için kullanılır. |
| userPrincipalName |X | | |UPN, kullanıcının giriş kimliğidir. Çoğu zaman [posta] değeriyle aynıdır. |
| wWWHomePage |X |X | | |

## <a name="teams-and-skype-for-business-online"></a>Ekipler ve Kurumsal Çevrimiçi Skype
| Öznitelik Adı | Kullanıcı | İletişim | Grup | Açıklama |
| --- |:---:|:---:|:---:| --- |
| hesapEtkin |X | | |Bir hesap etkinleştirilip etkinleştirilen ini tanımlar. |
| c |X |X | | |
| Cn |X | |X | |
| Co |X |X | | |
| şirket |X |X | | |
| bölüm |X |X | | |
| açıklama |X |X |X | |
| displayName |X |X |X | |
| fakstelefon numarası |X |X |X | |
| givenName |X |X | | |
| ev telefonu |X |X | | |
| ipPhone |X |X | | |
| l |X |X | | |
| posta |X |X |X | |
| Mailnickname |X |X |X | |
| managedBy | | |X | |
| manager |X |X | | |
| üye | | |X | |
| mobil |X |X | | |
| msExchHideFromAdresListeleri |X |X |X | |
| msRTCSIP-Uygulama Seçenekleri |X | | | |
| msRTCSIP-DeploymentLocator |X |X | | |
| msRTCSIP-Line |X |X | | |
| msRTCSIP-OptionFlags |X |X | | |
| msRTCSIP-OwnerUrn |X | | | |
| msRTCSIP-PrimaryUserAddress |X |X | | |
| msRTCSIP-UserEnabled |X |X | | |
| objectSID |X | |X |mekanik özellik. Azure AD ve AD arasında eşitleme sağlamak için kullanılan AD kullanıcı tanımlayıcısı. |
| diğerTelefon |X |X | | |
| physicalDeliveryOfficeName |X |X | | |
| Postakodu |X |X | | |
| tercihDil |X | | | |
| proxyAddresses |X |X |X | |
| pwdLastSet |X | | |mekanik özellik. Zaten verilmiş jetonları geçersiz kılınacağını bilmek için kullanılır. Her iki parola karma eşitleme, geçiş kimlik doğrulaması ve federasyon tarafından kullanılır. |
| sn |X |X | | |
| kaynakÇapa |X |X |X |mekanik özellik. ADDS ve Azure AD arasındaki ilişkiyi sürdürmek için değişmez tanımlayıcı. |
| st |X |X | | |
| Streetaddress |X |X | | |
| Telephonenumber |X |X | | |
| küçük resim fotoğrafı |X |X | | |
| başlık |X |X | | |
| kullanımKonum |X | | |mekanik özellik. Kullanıcının ülkesi/bölgesi. Lisans ataması için kullanılır. |
| userPrincipalName |X | | |UPN, kullanıcının giriş kimliğidir. Çoğu zaman [posta] değeriyle aynıdır. |
| wWWHomePage |X |X | | |

## <a name="azure-rms"></a>Azure RMS
| Öznitelik Adı | Kullanıcı | İletişim | Grup | Açıklama |
| --- |:---:|:---:|:---:| --- |
| hesapEtkin |X | | |Bir hesap etkinleştirilip etkinleştirilen ini tanımlar. |
| Cn |X | |X |Ortak ad veya takma ad. Çoğu zaman [posta] değerinin öneki. |
| displayName |X |X |X |Genellikle dost ad (ad soyadı) olarak gösterilen adı temsil eden bir dize. |
| posta |X |X |X |tam e-posta adresi. |
| üye | | |X | |
| objectSID |X | |X |mekanik özellik. Azure AD ve AD arasında eşitleme sağlamak için kullanılan AD kullanıcı tanımlayıcısı. |
| proxyAddresses |X |X |X |mekanik özellik. Azure AD tarafından kullanılır. Kullanıcı için tüm ikincil e-posta adreslerini içerir. |
| pwdLastSet |X | | |mekanik özellik. Zaten verilmiş jetonları geçersiz kılınacağını bilmek için kullanılır. |
| kaynakÇapa |X |X |X |mekanik özellik. ADDS ve Azure AD arasındaki ilişkiyi sürdürmek için değişmez tanımlayıcı. |
| kullanımKonum |X | | |mekanik özellik. Kullanıcının ülkesi/bölgesi. Lisans ataması için kullanılır. |
| userPrincipalName |X | | |Bu UPN, kullanıcının giriş kimliğidir. Çoğu zaman [posta] değeriyle aynıdır. |

## <a name="intune"></a>Intune
| Öznitelik Adı | Kullanıcı | İletişim | Grup | Açıklama |
| --- |:---:|:---:|:---:| --- |
| hesapEtkin |X | | |Bir hesap etkinleştirilip etkinleştirilen ini tanımlar. |
| c |X |X | | |
| Cn |X | |X | |
| açıklama |X |X |X | |
| displayName |X |X |X | |
| posta |X |X |X | |
| Mailnickname |X |X |X | |
| üye | | |X | |
| objectSID |X | |X |mekanik özellik. Azure AD ve AD arasında eşitleme sağlamak için kullanılan AD kullanıcı tanımlayıcısı. |
| proxyAddresses |X |X |X | |
| pwdLastSet |X | | |mekanik özellik. Zaten verilmiş jetonları geçersiz kılınacağını bilmek için kullanılır. Her iki parola karma eşitleme, geçiş kimlik doğrulaması ve federasyon tarafından kullanılır. |
| kaynakÇapa |X |X |X |mekanik özellik. ADDS ve Azure AD arasındaki ilişkiyi sürdürmek için değişmez tanımlayıcı. |
| kullanımKonum |X | | |mekanik özellik. Kullanıcının ülkesi/bölgesi. Lisans ataması için kullanılır. |
| userPrincipalName |X | | |UPN, kullanıcının giriş kimliğidir. Çoğu zaman [posta] değeriyle aynıdır. |

## <a name="dynamics-crm"></a>Dynamics CRM
| Öznitelik Adı | Kullanıcı | İletişim | Grup | Açıklama |
| --- |:---:|:---:|:---:| --- |
| hesapEtkin |X | | |Bir hesap etkinleştirilip etkinleştirilen ini tanımlar. |
| c |X |X | | |
| Cn |X | |X | |
| Co |X |X | | |
| şirket |X |X | | |
| ülkeKodu |X |X | | |
| açıklama |X |X |X | |
| displayName |X |X |X | |
| fakstelefon numarası |X |X | | |
| givenName |X |X | | |
| l |X |X | | |
| managedBy | | |X | |
| manager |X |X | | |
| üye | | |X | |
| mobil |X |X | | |
| objectSID |X | |X |mekanik özellik. Azure AD ve AD arasında eşitleme sağlamak için kullanılan AD kullanıcı tanımlayıcısı. |
| physicalDeliveryOfficeName |X |X | | |
| Postakodu |X |X | | |
| tercihDil |X | | | |
| pwdLastSet |X | | |mekanik özellik. Zaten verilmiş jetonları geçersiz kılınacağını bilmek için kullanılır. Her iki parola karma eşitleme, geçiş kimlik doğrulaması ve federasyon tarafından kullanılır. |
| sn |X |X | | |
| kaynakÇapa |X |X |X |mekanik özellik. ADDS ve Azure AD arasındaki ilişkiyi sürdürmek için değişmez tanımlayıcı. |
| st |X |X | | |
| Streetaddress |X |X | | |
| Telephonenumber |X |X | | |
| başlık |X |X | | |
| kullanımKonum |X | | |mekanik özellik. Kullanıcının ülkesi/bölgesi. Lisans ataması için kullanılır. |
| userPrincipalName |X | | |UPN, kullanıcının giriş kimliğidir. Çoğu zaman [posta] değeriyle aynıdır. |

## <a name="3rd-party-applications"></a>3. taraf uygulamaları
Bu grup, genel bir iş yükü veya uygulama için gereken en az öznitelikleri olarak kullanılan öznitelikler kümesidir. Başka bir bölümde listelenmemiş bir iş yükü veya Microsoft olmayan bir uygulama için kullanılabilir. Açıkça aşağıdaki ler için kullanılır:

* Yammer (yalnızca Kullanıcı tüketilir)
* [SharePoint gibi kaynaklar tarafından sunulan Karma İşletmelerarası İş (B2B) çapraz org işbirliği senaryoları](https://go.microsoft.com/fwlink/?LinkId=747036)

Bu grup, Azure AD dizininin Office 365, Dynamics veya Intune'u desteklemek için kullanılmaması durumunda kullanılabilecek öznitelikler kümesidir. Küçük bir çekirdek öznitelikleri kümesi vardır. Bazı üçüncü taraf uygulamalarda tek oturum açma veya sağlama nın, burada açıklanan özniteliklere ek olarak özniteliklerin eşitleştirilmesini gerektirdiğini unutmayın. Uygulama gereksinimleri, her uygulama için [SaaS uygulama öğreticisinde](../saas-apps/tutorial-list.md) açıklanmıştır.

| Öznitelik Adı | Kullanıcı | İletişim | Grup | Açıklama |
| --- |:---:|:---:|:---:| --- |
| hesapEtkin |X | | |Bir hesap etkinleştirilip etkinleştirilen ini tanımlar. |
| Cn |X | |X | |
| displayName |X |X |X | |
| employeeID |X |  |  | |
| givenName |X |X | | |
| posta |X | |X | |
| managedBy | | |X | |
| mailNickName |X |X |X | |
| üye | | |X | |
| objectSID |X | | |mekanik özellik. Azure AD ve AD arasında eşitleme sağlamak için kullanılan AD kullanıcı tanımlayıcısı. |
| proxyAddresses |X |X |X | |
| pwdLastSet |X | | |mekanik özellik. Zaten verilmiş jetonları geçersiz kılınacağını bilmek için kullanılır. Her iki parola karma eşitleme, geçiş kimlik doğrulaması ve federasyon tarafından kullanılır. |
| sn |X |X | | |
| kaynakÇapa |X |X |X |mekanik özellik. ADDS ve Azure AD arasındaki ilişkiyi sürdürmek için değişmez tanımlayıcı. |
| kullanımKonum |X | | |mekanik özellik. Kullanıcının ülkesi/bölgesi. Lisans ataması için kullanılır. |
| userPrincipalName |X | | |UPN, kullanıcının giriş kimliğidir. Çoğu zaman [posta] değeriyle aynıdır. |

## <a name="windows-10"></a>Windows 10
Windows 10 etki alanı birleştirilmiş bilgisayar(aygıt) Azure AD bazı öznitelikleri ni senkronize eder. Senaryolar hakkında daha fazla bilgi için, [etki alanına katılan aygıtları Windows 10 deneyimleri için Azure AD'ye bağlayın'a](../active-directory-azureadjoin-devices-group-policy.md)bakın. Bu öznitelikler her zaman eşitlenir ve Windows 10 seçebileceğiniz bir uygulama olarak görünmez. Windows 10 etki alanı birleştirilmiş bilgisayar, kullanıcı Sertifikası'nın doldurulan özniteliğiyle tanımlanır.

| Öznitelik Adı | Cihaz | Açıklama |
| --- |:---:| --- |
| hesapEtkin |X | |
| deviceTrustType |X |Etki alanı birleştirilmiş bilgisayarlar için kodlanmış değer. |
| displayName |X | |
| ms-DS-CreatorSID |X |RegisteredOwnerReference olarak da adlandırılır. |
| Objectguıd |X |DeviceID olarak da adlandırılır. |
| objectSID |X |Ayrıca onPremisesSecurityIdentifier denir. |
| Operatingsystem |X |DeviceOSType olarak da adlandırılır. |
| işletimSystemVersion |X |DeviceOSVersion olarak da adlandırılır. |
| userCertificate |X | |

**Kullanıcı** için bu öznitelikler, seçtiğiniz diğer uygulamalara ek olarak gelir.  

| Öznitelik Adı | Kullanıcı | Açıklama |
| --- |:---:| --- |
| etki alanıFQDN |X |Ayrıca dnsDomainName denir. Örneğin, contoso.com. |
| etkiNetBios |X |Ayrıca netBiosName denir. Örneğin, CONTOSO. |
| msDS-KeyCredentialLink |X |Kullanıcı Windows Hello for Business'a kaydolduktan sonra. | 

## <a name="exchange-hybrid-writeback"></a>Exchange hibrid writeback
Bu öznitelikler, **Exchange karmasını**etkinleştirmek için seçtiğinizde Azure AD'den şirket içi Active Directory'ye geri yazılır. Exchange sürümünüze bağlı olarak, daha az öznitelik eşitlenebilir.

| Öznitelik Adı (Şirket içi AD) | Öznitelik Adı (Connect UI) | Kullanıcı | İletişim | Grup | Açıklama |
| --- |:---:|:---:|:---:| --- |---|
| msDS-ExternalDirectoryObjectID| ms-DS-Dış-Dizin-Nesne-Id |X | | |Azure AD'de cloudAnchor'dan türetilmiştir. Bu özellik Exchange 2016 ve Windows Server 2016 AD'de yenidir. |
| msExchArşivDurumu| ms-Exch-ArchiveStatus |X | | |Çevrimiçi Arşiv: Müşterilerin postaları arşivlemesini sağlar. |
| msExchBlockedSendersHash| ms-Exch-BlockedSendersHash |X | | |Filtreleme: Şirket içi filtreleme ve çevrimiçi güvenli ve engellenmiş gönderen verilerini istemcilerden geri yazar. |
| msExchSafeAlıcılarHash| ms-Exch-SafeRecipientsHash  |X | | |Filtreleme: Şirket içi filtreleme ve çevrimiçi güvenli ve engellenmiş gönderen verilerini istemcilerden geri yazar. |
| msExchSafeSendersHash| ms-Exch-SafeSendersHash  |X | | |Filtreleme: Şirket içi filtreleme ve çevrimiçi güvenli ve engellenmiş gönderen verilerini istemcilerden geri yazar. |
| msExchUCVoiceMailSettings| ms-Exch-UCVoiceMailAyarları |X | | |Tümlatif İletiyi Etkinleştir (UM) - Çevrimiçi sesli posta: Microsoft Lync Server tümleştirmesi tarafından, kullanıcının çevrimiçi hizmetlerde sesli posta olduğunu şirket içinde Lync Server'a belirtmek için kullanılır. |
| msExchUserHoldPolitikalar| ms-Exch-UserHoldPolicies |X | | |Dava Tutma: Bulut hizmetlerinin hangi kullanıcıların Dava Tutma altında olduğunu belirlemesini sağlar. |
| proxyAddresses| proxyAddresses |X |X |X |Exchange Online'dan yalnızca x500 adresi eklenir. |
| publicDelegeler| ms-Exch-Kamu Delegeleri  |X | | |Exchange Online posta kutusunun SendOnBehalfTo'nun şirket içi Exchange posta kutusu olan kullanıcılara haklarını vermesini sağlar. Azure AD Connect oluşturma 1.1.552.0 veya sonrası gerektirir. |

## <a name="exchange-mail-public-folder"></a>Exchange Mail Ortak Klasörü
Exchange **Mail Ortak Klasörünü**etkinleştirmek için seçtiğinizde, bu öznitelikler şirket içi Active Directory'den Azure AD'ye eşitlenir.

| Öznitelik Adı | Herkese Açık Klasör | Açıklama |
| --- | :---:| --- |
| displayName | X |  |
| posta | X |  |
| msExchRecipientTypeDetails | X |  |
| Objectguıd | X |  |
| proxyAddresses | X |  |
| Targetaddress | X |  |

## <a name="device-writeback"></a>Cihaz geri yazma
Aygıt nesneleri Active Directory'de oluşturulur. Bu nesneler, Azure AD'ye veya etki alanına birleştirilmiş Windows 10 bilgisayarlarına katılan aygıtlar olabilir.

| Öznitelik Adı | Cihaz | Açıklama |
| --- |:---:| --- |
| altSecurityIdentities |X | |
| displayName |X | |
| dn |X | |
| msDS-CloudAnchor |X | |
| msDS-DeviceID |X | |
| msDS-DeviceObjectVersion |X | |
| msDS-DeviceOSType |X | |
| msDS-DeviceOSVersion |X | |
| msDS-DevicePhysicalIDs |X | |
| msDS-KeyCredentialLink |X |Sadece Windows Server 2016 AD şeması ile |
| msDS-Uyumlu |X | |
| msDS-IsEnabled |X | |
| msDS-Yönetildi |X | |
| msDS-RegisteredOwner |X | |

## <a name="notes"></a>Notlar
* Alternatif Kimlik kullanırken, şirket içi öznitelik userPrincipalName, Azure AD özniteliği onPremisesUserPrincipalName ile senkronize edilir. Örneğin posta gibi Alternatif Kimlik özniteliği, Azure AD özniteliği kullanıcısıPrincipalName ile senkronize edilir.
* Yukarıdaki listelerde, nesne türü **Kullanıcı** da nesne türü **iNetOrgPerson**için geçerlidir.

## <a name="next-steps"></a>Sonraki adımlar
Azure AD [Connect eşitleme](how-to-connect-sync-whatis.md) yapılandırması hakkında daha fazla bilgi edinin.

[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
