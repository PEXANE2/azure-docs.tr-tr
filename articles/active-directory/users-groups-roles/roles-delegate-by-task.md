---
title: Yönetici görevine göre rolleri temsilci ye - Azure Etkin Dizini | Microsoft Dokümanlar
description: Azure Etkin Dizini'nde kimlik görevleri için temsilci olarak devralacak roller
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 03/03/2020
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1ac44661dd5a52ba19a3b2dd461aabec1ec250bf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80284883"
---
# <a name="administrator-roles-by-admin-task-in-azure-active-directory"></a>Azure Etkin Dizini'nde yönetici görevine göre yönetici rolleri

Bu makalede, Azure Etkin Dizini'nde (Azure AD) en az ayrıcalıklı roller atayarak kullanıcının yönetici izinlerini kısıtlamak için gereken bilgileri bulabilirsiniz. Özellik alanına göre düzenlenen yönetici görevlerini ve her görevi gerçekleştirmek için gereken en az ayrıcalıklı rolün yanı sıra görevi gerçekleştirebilecek ek Genel Yönetici olmayan rolleri bulacaksınız.

## <a name="application-proxy"></a>Uygulama proxy’si

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Uygulama proxy uygulamasını yapılandırma | Uygulama yöneticisi | 
Konektör grubu özelliklerini yapılandırma | Uygulama yöneticisi | 
Tüm kullanıcılar için yetenek devre dışı bırakıldığında uygulama kaydı oluşturma | Uygulama geliştiricisi | Bulut Uygulama yöneticisi, Uygulama Yöneticisi
Bağlayıcı grubu oluşturma | Uygulama yöneticisi | 
Bağlayıcı grubunu silme | Uygulama yöneticisi | 
Uygulama ara sunucusunu devre dışı bırakma | Uygulama yöneticisi | 
Konektör hizmetini indirin | Uygulama yöneticisi | 
Tüm yapılandırmayı okuyun | Uygulama yöneticisi | 

## <a name="b2c"></a>B2C

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Azure AD B2C dizinleri oluşturma | Tüm konuk olmayan kullanıcılar ([belgelere bakın](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | 
B2C uygulamaları oluşturma | Genel Yönetici | 
Kurumsal uygulamalar oluşturma | Bulut Uygulaması Yöneticisi | Uygulama Yöneticisi
B2C ilkeleri oluşturma, okuma, güncelleme ve silme | B2C IEF İlke Yöneticisi | 
Kimlik sağlayıcılar oluşturma, okuma, güncelleme ve silme | Dış Kimlik Sağlayıcı Yöneticisi | 
Parola sıfırlama kullanıcı akışlarını oluşturma, okuma, güncelleme ve silme | B2C Kullanıcı Akış Yöneticisi | 
Profil düzenleme kullanıcı akışlarını oluşturma, okuma, güncelleme ve silme | B2C Kullanıcı Akış Yöneticisi | 
Oturum açma kullanıcı akışlarını oluşturma, okuma, güncelleme ve silme | B2C Kullanıcı Akış Yöneticisi | 
Kaydolma kullanıcı akışını oluşturma, okuma, güncelleme ve silme |B2C Kullanıcı Akış Yöneticisi | 
Kullanıcı öznitelikleri oluşturma, okuma, güncelleme ve silme | B2C Kullanıcı Akışı Öznitelik Yöneticisi | 
Kullanıcıları oluşturma, okuma, güncelleme ve silme | Kullanıcı Yöneticisi
Tüm yapılandırmayı okuyun | Küresel okuyucu | 
B2C denetim günlüklerini okuyun | Küresel okuyucu ([bkz. belgeler](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-faqs)) | 

> [!NOTE]
> Azure AD B2C Global okuyucuları, Azure AD genel yöneticileriyle aynı izinlere sahip değildir. Azure AD B2C genel yönetici ayrıcalıklarınız varsa, Azure AD dizininizin içinde değil, Bir Azure AD B2C dizininde olduğunuzu unutmayın.

## <a name="company-branding"></a>Şirket markası

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Şirket markası yapılandırma | Genel Yönetici | 
Tüm yapılandırmayı okuyun | Dizin okuyucular | Varsayılan kullanıcı rolü ([bkz. belgeler](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions))

## <a name="company-properties"></a>Şirket özellikleri

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Şirket özelliklerini yapılandırma | Genel Yönetici | 

## <a name="connect"></a>Bağlan

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Geçiş kimlik doğrulaması | Genel Yönetici | 
Tüm yapılandırmayı okuyun | Küresel okuyucu | 
Sorunsuz tek oturum açma | Genel Yönetici | 

## <a name="connect-health"></a>Connect Health

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Hizmet ekleme veya silme | Sahibi ([bkz. belgeler](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-health-operations)) | 
Eşitleme hatasına düzeltmeler uygulama | Katılımcı ([bkz. belgeler](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Sahip
Bildirimleri yapılandırma | Katılımcı ([bkz. belgeler](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Sahip
Ayarları yapılandırma | Sahibi ([bkz. belgeler](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-health-operations)) | 
Eşitleme bildirimlerini yapılandırma | Katılımcı ([bkz. belgeler](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Sahip
ADFS güvenlik raporlarını okuyun | Güvenlik Okuyucu | Katkıda Bulunan, Sahip
Tüm yapılandırmayı okuyun | Reader ([bkz. belgeler](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Katkıda Bulunan, Sahip
Eşitleme hatalarını okuma | Reader ([bkz. belgeler](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Katkıda Bulunan, Sahip
Eşitleme hizmetlerini okuma | Reader ([bkz. belgeler](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Katkıda Bulunan, Sahip
Ölçümleri ve uyarıları görüntüleme | Reader ([bkz. belgeler](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Katkıda Bulunan, Sahip
Ölçümleri ve uyarıları görüntüleme | Reader ([bkz. belgeler](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Katkıda Bulunan, Sahip
Eşitleme hizmeti ölçümlerini ve uyarılarını görüntüleme | Reader ([bkz. belgeler](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Katkıda Bulunan, Sahip

## <a name="custom-domain-names"></a>Özel etki alanı adları

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Etki alanlarını yönetme | Genel Yönetici | 
Tüm yapılandırmayı okuyun | Dizin okuyucular | Varsayılan kullanıcı rolü ([bkz. belgeler](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions))

## <a name="domain-services"></a>Etki Alanı Hizmetleri

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Azure AD Etki Alanı Hizmetleri örneği oluşturma | Genel Yönetici | 
Tüm Azure AD Etki Alanı Hizmetleri görevlerini gerçekleştirin | Azure AD DC Yöneticileri grubu ([bkz. belgeler](../../active-directory-domain-services/tutorial-create-management-vm.md#administrative-tasks-you-can-perform-on-an-azure-ad-ds-managed-domain)) | 
Tüm yapılandırmayı okuyun | AD DS hizmetini içeren Azure aboneliğinde okuyucu | 

## <a name="devices"></a>Cihazlar

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Aygıtı devre dışı | Bulut aygıt yöneticisi | 
Cihazı etkinleştirme | Bulut aygıt yöneticisi | 
Temel yapılandırmayı okuyun | Varsayılan kullanıcı rolü ([bkz. belgeler](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | 
BitLocker tuşlarını okuyun | Güvenlik Okuyucu | Parola yöneticisi, Güvenlik yöneticisi

## <a name="enterprise-applications"></a>Kurumsal uygulamalar

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Devredilen izinlere izin verme | Bulut uygulama yöneticisi | Uygulama yöneticisi
Microsoft Graph dahil olmayan uygulama izinlerine izin verme | Bulut uygulama yöneticisi | Uygulama yöneticisi
Microsoft Graph'a uygulama izinlerinin onayı | Ayrıcalıklı Rol Yöneticisi | 
Kendi verilerine erişen uygulamalara izin verme | Varsayılan kullanıcı rolü ([bkz. belgeler](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | 
Kurumsal uygulama oluşturma | Bulut uygulama yöneticisi | Uygulama yöneticisi
Uygulama Proxy'yi Yönetme | Uygulama yöneticisi | 
Kullanıcı ayarlarını yönetme | Genel Yönetici | 
Bir grubun veya bir uygulamanın erişim incelemesini okuma | Güvenlik Okuyucu | Güvenlik Yöneticisi, Kullanıcı Yöneticisi
Tüm yapılandırmayı okuyun | Varsayılan kullanıcı rolü ([bkz. belgeler](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | 
Kurumsal uygulama atamalarını güncelleştirme | Kurumsal uygulama sahibi ([bkz. belgeler](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Bulut uygulama yöneticisi, Uygulama yöneticisi
Kurumsal uygulama sahiplerini güncelleştirme | Kurumsal uygulama sahibi ([bkz. belgeler](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Bulut uygulama yöneticisi, Uygulama yöneticisi
Kurumsal uygulama özelliklerini güncelleştirme | Kurumsal uygulama sahibi ([bkz. belgeler](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Bulut uygulama yöneticisi, Uygulama yöneticisi
Kurumsal uygulama sağlamayı güncelleştirme | Kurumsal uygulama sahibi ([bkz. belgeler](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Bulut uygulama yöneticisi, Uygulama yöneticisi
Kurumsal uygulama self servis güncelleme | Kurumsal uygulama sahibi ([bkz. belgeler](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Bulut uygulama yöneticisi, Uygulama yöneticisi
Tek oturum açma özelliklerini güncelleştirme | Kurumsal uygulama sahibi ([bkz. belgeler](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Bulut uygulama yöneticisi, Uygulama yöneticisi

## <a name="entitlement-management"></a>Yetkilendirme yönetimi
Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Kataloğuna kaynak ekleme | Kullanıcı yöneticisi | Yetkilendirme yönetimi yle, bu görevi katalog sahibine devredebilirsiniz ([bkz. belgeler](../governance/entitlement-management-catalog-create.md#add-additional-catalog-owners))
SharePoint Online sitelerini kataloya ekleme | Genel yönetici


## <a name="groups"></a>Gruplar

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Lisans atama | Kullanıcı yöneticisi | 
Grup oluşturma | Kullanıcı yöneticisi | 
Bir grubun veya bir uygulamanın erişim incelemesi oluşturma, güncelleme veya silme | Kullanıcı yöneticisi | 
Grup sona ermesini yönetme | Kullanıcı yöneticisi | 
Grup ayarlarını yönetme | Gruplar Yöneticisi | Kullanıcı Yöneticisi | 
Tüm yapılandırmayı okuyun (gizli üyelik hariç) | Dizin okuyucular | Varsayılan kullanıcı rolü ([bkz. belgeler](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions))
Gizli üyeliği okuyun | Grup üyesi | Grup sahibi, Parola yöneticisi, Exchange yöneticisi, SharePoint yöneticisi, Takımlar yöneticisi, Kullanıcı yöneticisi
Gizli üyeliği olan grupların üyeliğini okuyun | Yardım Masası Yöneticisi | Kullanıcı yöneticisi, Takımlar yöneticisi
Lisansı iptal et | Lisans yöneticisi | Kullanıcı yöneticisi
Grup üyeliğini güncelleştir | Grup sahibi ([bkz. belgeler](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Kullanıcı yöneticisi
Grup sahiplerini güncelleştirme | Grup sahibi ([bkz. belgeler](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Kullanıcı yöneticisi
Grup özelliklerini güncelleştirme | Grup sahibi ([bkz. belgeler](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Kullanıcı yöneticisi

## <a name="identity-protection"></a>Kimlik Koruması

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Uyarı bildirimlerini yapılandırma| Güvenlik Yöneticisi | 
MFA ilkesini yapılandırma ve etkinleştirme veya devre dışı| Güvenlik Yöneticisi | 
Oturum açma risk ilkesini yapılandırma ve etkinleştirme veya devre dışı| Güvenlik Yöneticisi | 
Kullanıcı risk ilkesini yapılandırma ve etkinleştirme veya devre dışı | Güvenlik Yöneticisi | 
Haftalık özetleri yapılandırma | Güvenlik Yöneticisi| 
Tüm risk algılamalarını reddetme | Güvenlik Yöneticisi | 
Güvenlik açığını düzeltme veya reddetme | Güvenlik Yöneticisi | 
Tüm yapılandırmayı okuyun | Güvenlik Okuyucu | 
Tüm risk algılamalarını okuyun | Güvenlik Okuyucu | 
Güvenlik açıklarını okuyun | Güvenlik Okuyucu | 

## <a name="licenses"></a>Lisanslar

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Lisans atama | Lisans yöneticisi | Kullanıcı yöneticisi
Tüm yapılandırmayı okuyun | Dizin okuyucular | Varsayılan kullanıcı rolü ([bkz. belgeler](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions))
Lisansı iptal et | Lisans yöneticisi | Kullanıcı yöneticisi
Aboneliği deneyin veya satın alın | Faturalama yöneticisi | 


## <a name="monitoring---audit-logs"></a>İzleme - Denetim günlükleri

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Denetim günlüklerini okuma | Rapor okuyucu | Güvenlik Okuyucu, Güvenlik yöneticisi

## <a name="monitoring---sign-ins"></a>İzleme - Oturum Açma

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Oturum açma günlüklerini okuma | Rapor okuyucu | Güvenlik Okuyucu, Güvenlik yöneticisi

## <a name="multi-factor-authentication"></a>Multi-factor authentication

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Seçili kullanıcılar tarafından oluşturulan tüm varolan uygulama parolalarını silme | Genel Yönetici | 
MFA'yı devre dışı | Genel Yönetici | 
MFA’yı etkinleştirme | Genel Yönetici | 
MFA hizmet ayarlarını yönetme | Genel Yönetici | 
Seçili kullanıcıların ilgili kişi yöntemlerini yeniden sağlamalarını zorunlu kılmasını | Kimlik Doğrulama Yöneticisi | 
Hatırlanan tüm cihazlarda çok faktörlü kimlik doğrulamayı geri yükleme  | Kimlik Doğrulama Yöneticisi | 

## <a name="mfa-server"></a>MFA Sunucusu

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Kullanıcıları engelleme/engellemeyi kaldırma | Genel Yönetici | 
Hesap kilitlemeyi yapılandırma | Genel Yönetici | 
Önbelleğe alma kurallarını yapılandırma | Genel Yönetici | 
Dolandırıcılık uyarısını yapılandırma | Genel Yönetici
Bildirimleri yapılandırma | Genel Yönetici | 
Tek seferlik baypas yapılandırma | Genel Yönetici | 
Telefon görüşmesi ayarlarını yapılandırma | Genel Yönetici | 
Sağlayıcıları yapılandırma | Genel Yönetici | 
Sunucu ayarlarını yapılandırma | Genel Yönetici | 
Etkinlik raporunu okuma | Küresel okuyucu | 
Tüm yapılandırmayı okuyun | Küresel okuyucu | 
Sunucu durumunu okuma | Küresel okuyucu |  

## <a name="organizational-relationships"></a>Kuruluş ilişkileri

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Kimlik sağlayıcılarını yönetme | Dış Kimlik Sağlayıcı Yöneticisi | 
Ayarları yönetme | Genel Yönetici | 
Kullanım koşullarını yönetme | Genel Yönetici | 
Tüm yapılandırmayı okuyun | Küresel okuyucu | 

## <a name="password-reset"></a>Parola sıfırlama

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Kimlik doğrulama yöntemlerini yapılandırma | Genel Yönetici |
Özelleştirmeyi yapılandırma | Genel Yönetici |
Bildirimi yapılandırma | Genel Yönetici |
Şirket içi tümleştirmeyi yapılandırma | Genel Yönetici |
Parola sıfırlama özelliklerini yapılandırma | Kullanıcı Yöneticisi | Genel Yönetici
Kaydı yapılandırma | Genel Yönetici |
Tüm yapılandırmayı okuyun | Güvenlik Yöneticisi | Kullanıcı Yöneticisi |

## <a name="privileged-identity-management"></a>Privileged Identity Management

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Kullanıcıları rollere atama | Ayrıcalıklı rol yöneticisi | 
Rol ayarlarını yapılandırma | Ayrıcalıklı rol yöneticisi | 
Denetim etkinliğini görüntüleme | Güvenlik okuyucusu | 
Rol üyeliklerini görüntüleme | Güvenlik okuyucusu | 

## <a name="roles-and-administrators"></a>Roller ve yöneticiler

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Rol atamalarını yönetme | Ayrıcalıklı rol yöneticisi | 
Azure AD rolünün erişim incelemesini okuma  | Güvenlik Okuyucu | Güvenlik yöneticisi, Ayrıcalıklı rol yöneticisi
Tüm yapılandırmayı okuyun | Varsayılan kullanıcı rolü ([bkz. belgeler](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | 

## <a name="security---authentication-methods"></a>Güvenlik - Kimlik doğrulama yöntemleri

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Kimlik doğrulama yöntemlerini yapılandırma | Genel Yönetici | 
Tüm yapılandırmayı okuyun | Küresel okuyucu | 

## <a name="security---conditional-access"></a>Güvenlik - Koşullu Erişim

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
MFA güvenilen IP adreslerini yapılandırma | Koşullu Erişim yöneticisi | 
Özel denetimler oluşturma | Koşullu Erişim yöneticisi | Güvenlik yöneticisi
Adlandırılmış konumlar oluşturma | Koşullu Erişim yöneticisi | Güvenlik yöneticisi
İlkeleri oluşturma | Koşullu Erişim yöneticisi | Güvenlik yöneticisi
Kullanım koşulları oluşturma | Koşullu Erişim yöneticisi | Güvenlik yöneticisi
VPN bağlantı sertifikası oluşturma | Koşullu Erişim yöneticisi | Güvenlik yöneticisi
Klasik ilkeyi silme | Koşullu Erişim yöneticisi | Güvenlik yöneticisi
Kullanım koşullarını silme | Koşullu Erişim yöneticisi | Güvenlik yöneticisi
VPN bağlantı sertifikasını silme | Koşullu Erişim yöneticisi | Güvenlik yöneticisi
Klasik politikayı devre dışı | Koşullu Erişim yöneticisi | Güvenlik yöneticisi
Özel denetimleri yönetme | Koşullu Erişim yöneticisi | Güvenlik yöneticisi
Adlandırılmış konumları yönetme | Koşullu Erişim yöneticisi | Güvenlik yöneticisi
Kullanım koşullarını yönetme | Koşullu Erişim yöneticisi | Güvenlik yöneticisi
Tüm yapılandırmayı okuyun | Güvenlik okuyucusu | Güvenlik yöneticisi
Adlandırılmış konumları okuma | Güvenlik okuyucusu | Koşullu Erişim yöneticisi, güvenlik yöneticisi

## <a name="security---identity-security-score"></a>Güvenlik - Kimlik güvenliği puanı

Görev | En az ayrıcalıklı rol | Ek roller | 
---- | --------------------- | ----------------
Tüm yapılandırmayı okuyun | Güvenlik okuyucusu | Güvenlik yöneticisi
Güvenlik puanını okuma | Güvenlik okuyucusu | Güvenlik yöneticisi
Olay durumunu güncelleştir | Güvenlik yöneticisi | 

## <a name="security---risky-sign-ins"></a>Güvenlik - Riskli oturum açma

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Tüm yapılandırmayı okuyun | Güvenlik Okuyucu | 
Riskli oturum açma ları okuyun | Güvenlik Okuyucu | 

## <a name="security---users-flagged-for-risk"></a>Güvenlik - Risk için işaretlenmiş kullanıcılar

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Tüm olayları kapatabilirsiniz. | Güvenlik Yöneticisi | 
Tüm yapılandırmayı okuyun | Güvenlik Okuyucu | 
Risk için işaretlenmiş kullanıcıları okuma | Güvenlik Okuyucu | 

## <a name="users"></a>Kullanıcılar

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Kullanıcıyı dizin rolüne ekleme | Ayrıcalıklı rol yöneticisi | 
Kullanıcıyı gruba ekleme | Kullanıcı yöneticisi | 
Lisans atama | Lisans yöneticisi | Kullanıcı yöneticisi
Konuk kullanıcı oluşturma | Konuk davetçi | Kullanıcı yöneticisi
Kullanıcı oluştur | Kullanıcı yöneticisi | 
Kullanıcıları silme | Kullanıcı yöneticisi | 
Sınırlı yöneticilerin yenileme belirteçlerini geçersiz kılma (belgelere bakın) | Kullanıcı yöneticisi | 
Yönetici olmayanların yenileme belirteçlerini geçersiz kılma (belgelere bakın) | Parola yöneticisi | Kullanıcı yöneticisi
Ayrıcalıklı yöneticilerin yenileme belirteçlerini geçersiz kılma (belgelere bakın) | Ayrıcalıklı Kimlik Doğrulama Yöneticisi | 
Temel yapılandırmayı okuyun | Varsayılan Kullanıcı rolü ([bkz. belgeler](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions) | 
Sınırlı yöneticiler için parolayı sıfırlama (belgelere bakın) | Kullanıcı yöneticisi | 
Yönetici olmayanların parolasını sıfırlama (belgelere bakın) | Parola yöneticisi | Kullanıcı yöneticisi
Ayrıcalıklı yöneticilerin parolasını sıfırlama | Ayrıcalıklı Kimlik Doğrulama Yöneticisi | 
Lisansı iptal et | Lisans yöneticisi | Kullanıcı yöneticisi
Kullanıcı Adı Dışındaki Tüm Özellikleri Güncelleştir | Kullanıcı yöneticisi | 
Sınırlı yöneticiler için Kullanıcı Ana Adını Güncelleştirme (belgelere bakın) | Kullanıcı yöneticisi | 
Ayrıcalıklı yöneticilerde Kullanıcı Adı özelliğini güncelleştirme (belgelere bakın) | Genel Yönetici | 
Kullanıcı ayarlarını güncelleştirme | Genel Yönetici | 


## <a name="support"></a>Destek

Görev | En az ayrıcalıklı rol | Ek roller
---- | --------------------- | ----------------
Destek bileti gönder | Hizmet Yöneticisi | Uygulama Yöneticisi, Azure Bilgi Koruma Yöneticisi, Fatura Yöneticisi, Bulut Uygulama Yöneticisi, Uyumluluk Yöneticisi, Dynamics 365 Yöneticisi, Masaüstü Analiz Yöneticisi, Exchange Administrator, Şifre Yönetici, Intune Administrator, Skype for Business Administrator, Power BI Administrator, Ayrıcalıklı Kimlik Doğrulama Yöneticisi, SharePoint Yöneticisi, Ekipler İletişim Yöneticisi, Ekipler Yöneticisi, Kullanıcı Yöneticisi, İşyeri Analiz Yöneticisi

## <a name="next-steps"></a>Sonraki adımlar

* [Azure AD yöneticisi rollerini atama veya kaldırma](directory-manage-roles-portal.md)
* [Azure AD yöneticisi rolleri başvurusu](directory-assign-admin-roles.md)
