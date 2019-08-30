---
title: Azure Active Directory (Azure AD) denetim etkinliği başvurusu | Microsoft Docs
description: Azure Active Directory’de (Azure AD) denetim günlüklerinize kaydedilebilecek denetim etkinliklerine genel bakış elde edin.
services: active-directory
documentationcenter: ''
author: cawrites
manager: daveba
editor: ''
ms.assetid: a1f93126-77d1-4345-ab7d-561066041161
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 01/24/2019
ms.author: chadam
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 56a94ef3f6ea5737b629efe44bd3e1cc1f36a693
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70127138"
---
# <a name="azure-ad-audit-activity-reference"></a>Azure AD denetim etkinliği başvurusu

Azure Active Directory (Azure AD) raporlarında, ortamınızın nasıl çalıştığını öğrenmek için gereken bilgileri alabilirsiniz.

Azure AD'nin raporlama mimarisi aşağıdaki bileşenlerden oluşur:

- **Etkinlik raporları** 
    - [Oturum açma](concept-sign-ins.md) işlemleri – yönetilen uygulamaların kullanımı ve Kullanıcı oturum açma etkinlikleri hakkında bilgi sağlar
    - [Denetim günlükleri](concept-audit-logs.md) - Azure AD içindeki çeşitli özellikler tarafından yapılan tüm değişiklikler için günlükler aracılığıyla izlenebilirlik sağlar. 
    
- **Güvenlik raporları** 
    - [Riskli oturum açma işlemleri](concept-risky-sign-ins.md) - Riskli oturum açma işlemi bir kullanıcı hesabının meşru sahibi olmayan bir kişi tarafından gerçekleştirilmiş olabilecek oturum açma girişiminin göstergesidir. 
    - [Riskli oldukları belirlenen kullanıcılar](concept-user-at-risk.md) - Riskli kullanıcı, güvenliği tehlikeye girmiş olabilecek bir kullanıcı hesabının göstergesidir. 

Bu makalede, denetim günlüklerinize kaydedilebilecek denetim etkinlikleri listelenir.

## <a name="access-reviews"></a>Erişim gözden geçirmeleri

|Denetim Kategorisi|Etkinlik|
|---|---|
|Erişim Gözden Geçirmeleri|Erişim gözden geçirmesi sona erdi|
|Erişim Gözden Geçirmeleri|İstek onayına onaylayan ekleme|
|Erişim Gözden Geçirmeleri|Erişim gözden geçirmesine gözden geçiren ekleme|
|Erişim Gözden Geçirmeleri|Erişim incelemesi uygula|
|Erişim Gözden Geçirmeleri|Erişim incelemesi oluştur|
|Erişim Gözden Geçirmeleri|Program oluşturma|
|Erişim Gözden Geçirmeleri|İstek onayı oluşturma|
|Erişim Gözden Geçirmeleri|Erişim incelemesini sil|
|Erişim Gözden Geçirmeleri|Program silme|
|Erişim Gözden Geçirmeleri|Program denetimini bağlama|
|Erişim Gözden Geçirmeleri|Azure AD Erişim Gözden Geçirmelerine Ekleme|
|Erişim Gözden Geçirmeleri|Erişim gözden geçirmesinden gözden geçireni kaldırma|
|Erişim Gözden Geçirmeleri|Gözden Geçirmeyi Durdurmayı İsteme|
|Erişim Gözden Geçirmeleri|Gözden geçirme sonucunu uygulama işlemi iste|
|Erişim Gözden Geçirmeleri|Rbac Rolü üyeliğini gözden geçirme|
|Erişim Gözden Geçirmeleri|Uygulama atamasını gözden geçirme|
|Erişim Gözden Geçirmeleri|Grup üyeliğini gözden geçirme|
|Erişim Gözden Geçirmeleri|İstek onayı isteğini gözden geçirme|
|Erişim Gözden Geçirmeleri|Program denetiminin bağlantısını kaldırma|
|Erişim Gözden Geçirmeleri|Erişim Gözden Geçirmesini Güncelleştirme|
|Erişim Gözden Geçirmeleri|Azure AD erişim Incelemelerini güncelleştirme ekleme durumu|
|Erişim Gözden Geçirmeleri|Erişim gözden geçirmesi posta bildirimi ayarlarını güncelleştirme|
|Erişim Gözden Geçirmeleri|Erişim gözden geçirme yinelenme sayısı ayarını Güncelleştir|
|Erişim Gözden Geçirmeleri|Erişim gözden geçirme gün cinsinden yineleme süresini Güncelleştir ayarı|
|Erişim Gözden Geçirmeleri|Erişim gözden geçirme yineleme bitiş türü ayarını Güncelleştir|
|Erişim Gözden Geçirmeleri|Erişim gözden geçirme yineleme türü ayarını Güncelleştir|
|Erişim Gözden Geçirmeleri|Erişim gözden geçirmesi anımsatıcı için ayarları güncelleştir|
|Erişim Gözden Geçirmeleri|Program güncelleştirme|
|Erişim Gözden Geçirmeleri|İstek onayını güncelleştirme|
|Erişim Gözden Geçirmeleri|Kullanıcı devre dışı|

## <a name="account-provisioning"></a>Hesap sağlama

|Denetim Kategorisi|Etkinlik|
|---|---|
|Uygulama Yönetimi|V2 uygulama izinleri alma|
|Uygulama Yönetimi|Geçerli kiracıdaki V2 uygulama hizmet sorumlularını alma|
|Uygulama Yönetimi|V1 uygulamasını güncelleştirme|
|Uygulama Yönetimi|V2 uygulamasını güncelleştirme|
|Uygulama Yönetimi|V2 uygulama iznini güncelleştirme|
|Uygulama Yönetimi|OAuth2PermissionGrant ekleme|
|Uygulama Yönetimi|Hizmet sorumlusuna uygulama rol ataması ekleme|

## <a name="application-proxy"></a>Uygulama proxy'si

|Denetim Kategorisi|Etkinlik|
|---|---|
|Uygulama Yönetimi|Uygulama ekleme|
|Uygulama Yönetimi|Uygulamaya sahip ekleme|
|Uygulama Yönetimi|Hizmet sorumlusuna sahip ekleme|
|Uygulama Yönetimi|Hizmet sorumlusuna ilke ekleme|
|Dizin Yönetimi|Hizmet sorumlusu ekleme|
|Dizin Yönetimi|Hizmet sorumlusu kimlik bilgileri ekleme|
|Dizin Yönetimi|Uygulama onayı|
|Dizin Yönetimi|Uygulamayı sil|
|Dizin Yönetimi|Uygulamayı Kalıcı Olarak Silme|
|Dizin Yönetimi|OAuth2PermissionGrant’ı kaldırma|
|Dizin Yönetimi|Hizmet sorumlusundan uygulama rolü atamasını kaldırma|
|Dizin Yönetimi|Uygulamadan sahibi kaldırma|
|Resource|Hizmet sorumlusundan sahibi kaldırma|
|Resource|Hizmet sorumlusundan ilkeyi kaldırma|
|Resource|Hizmet sorumlusunu kaldırma|


## <a name="automated-password-rollover"></a>Otomatik parola geçişi

|Denetim Kategorisi|Etkinlik|
|---|---|
|Uygulama Yönetimi|Hizmet sorumlusu kimlik bilgilerini kaldırma|


## <a name="b2c"></a>B2C

|Denetim Kategorisi|Etkinlik|
|---|---|
|Uygulama Yönetimi|Uygulamayı geri yükleme|
|Uygulama Yönetimi|İzni iptal etme|
|Uygulama Yönetimi|Uygulamayı güncelleştirme|
|Uygulama Yönetimi|Dış gizli anahtarları güncelleştirme|
|Uygulama Yönetimi|Hizmet sorumlusunu güncelleştirme|
|Uygulama Yönetimi|Uygulamaya bir erişim belirteci verme|
|Uygulama Yönetimi|Uygulamaya bir yetkilendirme kodu verme|
|Uygulama Yönetimi|Uygulamaya bir id_token verme|
|Uygulama Yönetimi|Yerel hesap kimlik bilgilerini doğrulama|
|Uygulama Yönetimi|Kullanıcı kimlik doğrulamasını doğrulama|
|Uygulama Yönetimi|V2 uygulama izinleri ekleme|
|Uygulama Yönetimi|CPIM anahtar kapsayıcısına ASCII gizli anahtarına dayalı bir anahtar ekleme|
|Uygulama Yönetimi|CPIM anahtar kapsayıcısına bir anahtar ekleme|
|Uygulama Yönetimi|AdminPolicyDatas-SetResources|
|Uygulama Yönetimi|AdminUserJourneys-GetResources|
|Uygulama Yönetimi|AdminUserJourneys-RemoveResources|
|Authentication|AdminUserJourneys-SetResources|
|Authentication|IdentityProvider oluşturma|
|Authentication|V1 uygulaması oluşturma|
|Authentication|V2 uygulaması oluşturma|
|Authentication|Kiracıda özel etki alanları oluşturma|
|Authorization|Yeni bir AdminUserJourney oluşturma|
|Authorization|Yerelleştirilmiş kaynak json oluşturma|
|Authorization|Yeni Özel IDP oluşturma|
|Authorization|Yeni IDP oluşturma|
|Authorization|B2C dizin kaynağı oluşturma veya güncelleştirme|
|Authorization|İlke oluştur|
|Authorization|trustFramework ilkesi oluşturma|
|Authorization|Yapılandırılabilir önek ile trustFramework ilkesi oluşturma|
|Authorization|Kullanıcı özniteliği oluşturma|
|Authorization|CreateTrustFrameworkPolicy|
|Authorization|Yeni bir AdminUserJourney oluşturur veya güncelleştirir|
|Authorization|IDP’yi silme|
|Authorization|IdentityProvider’ı silme|
|Authorization|V1 uygulamasını silme|
|Authorization|V2 uygulamasını silme|
|Authorization|V2 uygulama iznini silme|
|Authorization|B2C dizini kaynağını silme|
|Authorization|CPIM anahtar kapsayıcısını silme|
|Authorization|trustFramework ilkesini silme|
|Authorization|Kullanıcı özniteliğini silme|
|Authorization|B2C özelliğini etkinleştirme|
|Authorization|Bir abonelikteki B2C dizin kaynaklarını alma|
|Authorization|Özel IDP alma|
|Authorization|IDP alma|
|Authorization|V1 ve V2 uygulamalarını alma|
|Authorization|V1 uygulaması alma|
|Authorization|V1 uygulamaları alma|
|Authorization|V2 uygulaması alma|
|Authorization|V2 uygulamaları alma|
|Authorization|B2C dizin kaynağı al|
|Authorization|Kiracıdaki özel etki alanlarının listesini alma|
|Authorization|Kullanıcı yolculuğu alma|
|Authorization|Kullanıcı yolculuğu için izin verilen uygulama taleplerini alma|
|Authorization|Kullanıcı yolculuğu için izin verilen otomatik olarak onaylanan talepler alma|
|Authorization|İlkenin izin verilen otomatik olarak onaylanan taleplerini alma|
|Authorization|Kullanılabilir çıktı talepleri listesini alma|
|Authorization|Kullanıcı yolculuğu için içerik tanımlarını alma|
|Authorization|Belirli bir yönetim akışı için idp’leri alma|
|Authorization|JWK içindeki anahtar kapsayıcısı etkin anahtar meta verilerini alma|
|Authorization|Tüm yönetim akışlarının listesini alma|
|Authorization|Tüm kullanıcılar için tüm yönetim akışlarının etiketlerinin listesini alma|
|Authorization|Bir kullanıcı için kiracıların listesini alma|
|Authorization|Yerel hesapların otomatik olarak onaylanan taleplerini alma|
|Authorization|Yerelleştirilmiş kaynak json alma|
|Authorization|Microsoft.AzureActiveDirectory kaynak sağlayıcısının işlemlerini alma|
|Authorization|İlkeleri alma|
|Authorization|İlke alma|
|Authorization|Bir kiracının kaynak özelliklerini alma|
|Authorization|Desteklenen IDP listesini alma|
|Authorization|Kullanıcı yolculuğunun desteklenen IDP listesini alma|
|Authorization|Kiracı Bilgilerini alma|
|Authorization|Kiracının izin verilen özelliklerini alma|
|Authorization|Kiracı tanımlı Özel IDP listesini alma|
|Authorization|Kiracı tanımlı IDP listesini alma|
|Authorization|Kiracı tanımlı yerel IDP listesini alma|
|Authorization|Kaynak oluşturma için bir kullanıcının kiracı ayrıntılarını alma|
|Authorization|Kiracı listesini alma|
|Authorization|tenantDomains alma|
|Authorization|CPIM için varsayılan desteklenen kültürü alma|
|Authorization|Bir yönetim akışının ayrıntılarını alma|
|Authorization|Bu kiracı için UserJourneys listesini alma|
|Authorization|CPIM için kullanılabilir desteklenen kültürler kümesini alma|
|Authorization|trustFramework ilkesini alma|
|Authorization|trustFramework ilkesini xml olarak alma|
|Authorization|Kullanıcı özniteliğini alma|
|Authorization|Kullanıcı özniteliklerini alma|
|Authorization|Kullanıcı yolculuğu listesini alma|
|Authorization|GetIEFPolicies|
|Authorization|GetIdentityProviders|
|Authorization|GetTrustFrameworkPolicy|
|Authorization|Bir CPIM anahtar kapsayıcısını jwk biçiminde alır|
|Authorization|Kiracıdaki anahtar kapsayıcılarının listesini alır|
|Authorization|Kiracı türünü alır|
|Authorization|MigrateTenantMetadata|
|Authorization|IdentityProvider için düzeltme eki uygulama|
|Authorization|PutTrustFrameworkPolicy|
|Authorization|PutTrustFrameworkpolicy|
|Authorization|Kullanıcı yolculuğunu kaldırma|
|Authorization|CPIM anahtar kapsayıcısı yedeklemesini geri yükleme|
|Authorization|V2 uygulama izinleri alma|
|Authorization|Geçerli kiracıdaki V2 uygulama hizmet sorumlularını alma|
|Authorization|Özel IDP’yi güncelleştirme|
|Authorization|IDP’yi güncelleştirme|
|Authorization|Yerel IDP'yi güncelleştir|
|Authorization|V1 uygulamasını güncelleştirme|
|Authorization|V2 uygulamasını güncelleştirme|
|Authorization|V2 uygulama iznini güncelleştirme|
|Authorization|İlkeyi güncelleştir|
|Authorization|Kullanıcı özniteliğini güncelleştirme|
|Authorization|CPIM şifrelenmiş anahtarını karşıya yükleme|
|Authorization|Kullanıcı yetkilendirmesi: API, kiracı için devre dışı bırakıldı|
|Authorization|Kullanıcı yetkilendirmesi: ' Kiracı Admin' ' olarak Kullanıcı erişimi verildi|
|Authorization|Kullanıcı yetkilendirmesi: Kullanıcıya ' kimliği doğrulanmış kullanıcılar ' erişim hakları verildi|
|Authorization|B2C özelliğinin etkinleştirildiğini doğrulama|
|Authorization|Özelliğin etkin olduğunu doğrulayın|
|Authorization|Program oluşturma|
|Authorization|Program silme|
|Authorization|Program denetimini bağlama|
|Authorization|Azure AD Erişim Gözden Geçirmelerine Ekleme|
|Authorization|Program denetiminin bağlantısını kaldırma|
|Authorization|Program güncelleştirme|
|Authorization|Masaüstü Sso’yu devre dışı bırakma|
|Authorization|Belirli bir etki alanı için Masaüstü Sso’yu devre dışı bırakma|
|Authorization|Uygulama proxy'sini devre dışı bırak|
|Authorization|Geçişli kimlik doğrulamasını devre dışı bırakma|
|Authorization|Masaüstü Sso’yu etkinleştirme|
|Dizin Yönetimi|Belirli bir etki alanı için Masaüstü Sso’yu etkinleştirme|
|Dizin Yönetimi|Uygulama proxy'sini etkinleştir|
|Dizin Yönetimi|Geçişli kimlik doğrulamasını etkinleştirme|
|Dizin Yönetimi|Kiracıda özel etki alanları oluşturma|
|Dizin Yönetimi|B2C özelliğini etkinleştirme|
|Dizin Yönetimi|Kiracıdaki özel etki alanlarının listesini alma|
|Dizin Yönetimi|Bir kiracının kaynak özelliklerini alma|
|Dizin Yönetimi|Kiracı Bilgilerini alma|
|Dizin Yönetimi|Kiracının izin verilen özelliklerini alma|
|Dizin Yönetimi|tenantDomains alma|
|Anahtar|Kiracı türünü alır|
|Anahtar|B2C özelliğinin etkinleştirildiğini doğrulama|
|Anahtar|Özelliğin etkin olduğunu doğrulayın|
|Anahtar|Şirkete iş ortağı ekleme|
|Anahtar|Doğrulanmamış etki alanı ekleme|
|Anahtar|Doğrulanmış etki alanı ekleme|
|Anahtar|Şirket oluşturma|
|Anahtar|Şirket ayarları oluşturma|
|Anahtar|Şirket ayarlarını silme|
|Anahtar|İş ortağını indirgeme|
|Anahtar|Dizin silindi|
|Diğer|Dizin kalıcı olarak silindi|
|Diğer|Dizin silinmek üzere zamanlandı|
|Resource|Şirketi iş ortağına yükseltme|
|Resource|Hak yönetimi özelliklerini temizleme|
|Resource|Şirketten iş ortağını kaldırma|
|Resource|Doğrulanmamış etki alanını kaldırma|
|Resource|Doğrulanmış etki alanını kaldırma|
|Resource|Şirket Bilgilerini Ayarlama|
|Resource|DirSync özelliğini ayarlama|
|Resource|DirSyncEnabled bayrağını ayarlama|
|Resource|İş Ortaklığı Ayarlama|
|Resource|Yanlışlıkla silme eşiğini ayarlama|
|Resource|Şirket tarafından izin verilen veri konumunu ayarlama|
|Resource|Şirket çok uluslu özelliğini etkin olarak ayarlama|
|Resource|Kiracıda dizin özelliğini ayarlama|
|Resource|Etki alanı kimlik doğrulaması ayarlama|
|Resource|Etki alanında federasyon ayarlarını belirleme|
|Resource|Parola ilkesi ayarlama|
|Resource|Rights management özelliklerini ayarlama|
|Resource|Şirketi güncelleştirme|
|Resource|Şirket ayarlarını güncelleştirme|
|Resource|Etki alanını güncelleştir|
|Resource|Etki alanını doğrula|
|Resource|E-posta ile doğrulanmış etki alanını doğrulama|
|Resource|Ekleme|
|Resource|Uyarı ayarlarını güncelleştir|
|Resource|Haftalık özet ayarlarını güncelleştirme|
|Resource|Dizin için parola geri yazmayı devre dışı bırakma|
|Resource|Dizin için parola geri yazmayı etkinleştirme|
|Resource|Gruba uygulama rolü ataması ekleme|
|Resource|Grup ekle|
|Resource|Gruba üye ekle|
|Resource|Gruba sahip ekleme|
|Resource|Grup ayarları oluşturma|
|Resource|Grubu sil|
|Resource|Grup ayarlarını silme|
|Resource|Kullanıcılara grup tabanlı lisans uygulamayı sonlandırma|
|Resource|Grubu Kalıcı Olarak Silme|
|Resource|Gruptan uygulama rolü atamasını kaldırma|
|Resource|Üyeyi gruptan kaldır|
|Resource|Gruptan sahibi kaldırma|
|Resource|Grubu geri yükleme|
|Resource|Grup lisansı ayarlama|
|Resource|Grubu kullanıcı tarafından yönetilecek şekilde ayarlama|
|Resource|Kullanıcılara grup tabanlı lisans uygulamaya başlama|
|Resource|Tetikleyici grubu lisans yeniden hesaplaması|
|Resource|Grubu güncelleştirme|
|Resource|Grup ayarlarını güncelleştirme|
|Resource|Üye Ekle|
|Resource|Grup Oluşturma|
|Resource|Grubu Sil|
|Resource|Üye Kaldır|
|Resource|Grubu Güncelleştir|
|Resource|Onay bekleyen bir gruba katılma isteğini onaylama|
|Resource|Onay bekleyen bir gruba katılma isteğini iptal etme|
|Resource|Yaşam döngüsü yönetim ilkesi oluşturma|
|Resource|Onay bekleyen bir gruba katılma isteğini silme|
|Resource|Onay bekleyen bir gruba katılma isteğini reddetme|
|Resource|Grubu yenile|
|Resource|Bir gruba katılma isteğinde bulunma|
|Resource|Dinamik grup özelliklerini ayarlama|
|Resource|Yaşam döngüsü yönetim ilkesini güncelleştirme|
|Resource|CPIM anahtar kapsayıcısına ASCII gizli anahtarına dayalı bir anahtar ekleme|
|Resource|CPIM anahtar kapsayıcısına bir anahtar ekleme|
|Resource|CPIM anahtar kapsayıcısını silme|
|Resource|Anahtar kapsayıcısını silme|
|Resource|JWK içindeki anahtar kapsayıcısı etkin anahtar meta verilerini alma|
|Resource|Anahtar kapsayıcısı meta verilerini alma|
|Resource|Bir CPIM anahtar kapsayıcısını jwk biçiminde alır|
|Resource|Kiracıdaki anahtar kapsayıcılarının listesini alır|
|Resource|CPIM anahtar kapsayıcısı yedeklemesini geri yükleme|
|Resource|Anahtar kapsayıcısını kaydetme|
|Resource|CPIM şifrelenmiş anahtarını karşıya yükleme|
|Resource|Uygulamaya bir yetkilendirme kodu verme|
|Resource|Uygulamaya bir id_token verme|


## <a name="core-directory"></a>Çekirdek dizin

|Denetim Kategorisi|Etkinlik|
|---|---|
|Yönetim Birimi Yönetimi|Tek bir risk algılama türünü indirin|
|Yönetim Birimi Yönetimi|Yöneticileri ve haftalık özet katılımı durumunu indirme|
|Yönetim Birimi Yönetimi|Tüm risk algılama türlerini indir|
|Yönetim Birimi Yönetimi|Ücretsiz Kullanıcı risk algılamalarını indirin|
|Yönetim Birimi Yönetimi|Riskli olduğu belirlenen kullanıcıları indirme|
|Uygulama Yönetimi|İşlenen toplu davetler|
|Uygulama Yönetimi|Karşıya yüklenen toplu davetler|
|Uygulama Yönetimi|İlkeye sahip ekleme|
|Uygulama Yönetimi|İlke ekleme|
|Uygulama Yönetimi|İlkeyi sil|
|Uygulama Yönetimi|İlke kimlik bilgilerini kaldırma|
|Uygulama Yönetimi|İlkeyi güncelleştir|
|Uygulama Yönetimi|MFA kayıt ilkesini ayarlama|
|Uygulama Yönetimi|Oturum açma riski ilkesini ayarlama|
|Uygulama Yönetimi|Kullanıcı riski ilkesini ayarlama|
|Uygulama Yönetimi|Kullanım Koşullarını Kabul Etme|
|Uygulama Yönetimi|Kullanım Koşulları Oluşturma|
|Uygulama Yönetimi|Kullanım Koşullarını Reddetme|
|Uygulama Yönetimi|Kullanım Koşullarını Silme|
|Uygulama Yönetimi|Kullanım Koşullarını Düzenleme|
|Uygulama Yönetimi|Kullanım Koşullarını Yayımlama|
|Uygulama Yönetimi|Kullanım Koşullarını Yayımdan Kaldırma|
|Uygulama Yönetimi|Uygulama SSL sertifikası ekleme|
|Uygulama Yönetimi|SSL bağlamasını silme|
|Uygulama Yönetimi|Bağlayıcıyı kaydetme|
|Uygulama Yönetimi|AdminPolicyDatas-RemoveResources|
|Uygulama Yönetimi|AdminPolicyDatas-SetResources|
|Uygulama Yönetimi|AdminUserJourneys-GetResources|
|Dizin Yönetimi|AdminUserJourneys-RemoveResources|
|Dizin Yönetimi|AdminUserJourneys-SetResources|
|Dizin Yönetimi|IdentityProvider oluşturma|
|Dizin Yönetimi|Yeni bir AdminUserJourney oluşturma|
|Dizin Yönetimi|Yerelleştirilmiş kaynak json oluşturma|
|Dizin Yönetimi|Yeni Özel IDP oluşturma|
|Dizin Yönetimi|Yeni IDP oluşturma|
|Dizin Yönetimi|B2C dizin kaynağı oluşturma veya güncelleştirme|
|Dizin Yönetimi|İlke oluştur|
|Dizin Yönetimi|trustFramework ilkesi oluşturma|
|Dizin Yönetimi|Yapılandırılabilir önek ile trustFramework ilkesi oluşturma|
|Dizin Yönetimi|Kullanıcı özniteliği oluşturma|
|Dizin Yönetimi|CreateTrustFrameworkPolicy|
|Dizin Yönetimi|IDP’yi silme|
|Dizin Yönetimi|IdentityProvider’ı silme|
|Dizin Yönetimi|B2C dizini kaynağını silme|
|Dizin Yönetimi|trustFramework ilkesini silme|
|Dizin Yönetimi|Kullanıcı özniteliğini silme|
|Dizin Yönetimi|Bir kaynak grubundaki B2C dizin kaynaklarını alma|
|Dizin Yönetimi|Bir abonelikteki B2C dizin kaynaklarını alma|
|Dizin Yönetimi|Özel IDP alma|
|Dizin Yönetimi|IDP alma|
|Dizin Yönetimi|B2C dizin kaynağı al|
|Dizin Yönetimi|Kullanıcı yolculuğu alma|
|Dizin Yönetimi|Kullanıcı yolculuğu için izin verilen uygulama taleplerini alma|
|Dizin Yönetimi|Kullanıcı yolculuğu için izin verilen otomatik olarak onaylanan talepler alma|
|Dizin Yönetimi|İlkenin izin verilen otomatik olarak onaylanan taleplerini alma|
|Dizin Yönetimi|Kullanılabilir çıktı talepleri listesini alma|
|Dizin Yönetimi|Kullanıcı yolculuğu için içerik tanımlarını alma|
|Dizin Yönetimi|Belirli bir yönetim akışı için idp’leri alma|
|Dizin Yönetimi|Tüm yönetim akışlarının listesini alma|
|Dizin Yönetimi|Tüm kullanıcılar için tüm yönetim akışlarının etiketlerinin listesini alma|
|Grup Yönetimi|Bir kullanıcı için kiracıların listesini alma|
|Grup Yönetimi|Yerel hesapların otomatik olarak onaylanan taleplerini alma|
|Grup Yönetimi|Yerelleştirilmiş kaynak json alma|
|Grup Yönetimi|Microsoft.AzureActiveDirectory kaynak sağlayıcısının işlemlerini alma|
|Grup Yönetimi|İlkeleri alma|
|Grup Yönetimi|İlke alma|
|Grup Yönetimi|Desteklenen IDP listesini alma|
|Grup Yönetimi|Kullanıcı yolculuğunun desteklenen IDP listesini alma|
|Grup Yönetimi|Kiracı tanımlı Özel IDP listesini alma|
|Grup Yönetimi|Kiracı tanımlı IDP listesini alma|
|Grup Yönetimi|Kiracı tanımlı yerel IDP listesini alma|
|Grup Yönetimi|Kaynak oluşturma için bir kullanıcının kiracı ayrıntılarını alma|
|Grup Yönetimi|CPIM için varsayılan desteklenen kültürü alma|
|Grup Yönetimi|Bir yönetim akışının ayrıntılarını alma|
|Grup Yönetimi|Bu kiracı için UserJourneys listesini alma|
|Grup Yönetimi|CPIM için kullanılabilir desteklenen kültürler kümesini alma|
|Grup Yönetimi|trustFramework ilkesini alma|
|Grup Yönetimi|trustFramework ilkesini xml olarak alma|
|Grup Yönetimi|Kullanıcı özniteliğini alma|
|İlke Yönetimi|Kullanıcı özniteliklerini alma|
|İlke Yönetimi|Kullanıcı yolculuğu listesini alma|
|İlke Yönetimi|GetIEFPolicies|
|İlke Yönetimi|GetIdentityProviders|
|İlke Yönetimi|GetTrustFrameworkPolicy|
|Resource|MigrateTenantMetadata|
|Resource|Kaynakları taşı|
|Resource|IdentityProvider için düzeltme eki uygulama|
|Resource|PutTrustFrameworkPolicy|
|Resource|PutTrustFrameworkpolicy|
|Resource|Kullanıcı yolculuğunu kaldırma|
|Resource|Özel IDP’yi güncelleştirme|
|Resource|IDP’yi güncelleştirme|
|Resource|Yerel IDP'yi güncelleştir|
|Resource|B2C dizini kaynağını güncelleştirme|
|Resource|İlkeyi güncelleştir|
|Resource|Abonelik durumunu güncelleştirme|
|Rol Yönetimi|Kullanıcı özniteliğini güncelleştirme|
|Rol Yönetimi|Kaynakları taşımayı doğrulama|
|Rol Yönetimi|Cihaz ekleme|
|Rol Yönetimi|Cihaz yapılandırması ekleme|
|Rol Yönetimi|Cihaza kayıtlı sahip ekleme|
|Rol Yönetimi|Cihaza kayıtlı kullanıcılar ekleme|
|Rol Yönetimi|Cihazı sil|
|Rol Yönetimi|Cihaz yapılandırmasını silme|
|Rol Yönetimi|Cihaz artık uyumlu değil|
|Rol Yönetimi|Cihaz artık yönetilen cihaz değil|
|Kullanıcı Yönetimi|Cihazdan kayıtlı sahibi kaldırma|
|Kullanıcı Yönetimi|Cihazdan kayıtlı kullanıcıları kaldırma|
|Kullanıcı Yönetimi|Cihazı güncelleştir|
|Kullanıcı Yönetimi|Cihaz yapılandırmasını güncelleştirme|
|Kullanıcı Yönetimi|Role uygun üye ekleme|
|Kullanıcı Yönetimi|Role üye ekleme|
|Kullanıcı Yönetimi|Rol tanımına rol ataması ekleme|
|Kullanıcı Yönetimi|Şablondan rol ekleme|
|Kullanıcı Yönetimi|Role kapsamlı üye ekleme|
|Kullanıcı Yönetimi|Rolden uygun üyeyi kaldırma|
|Kullanıcı Yönetimi|Üyeyi rolden kaldır|
|Kullanıcı Yönetimi|Rol tanımından rol atamasını kaldırma|
|Kullanıcı Yönetimi|Rolden kapsamlı üyeyi kaldırma|
|Kullanıcı Yönetimi|Rolü güncelleştirme|
|Kullanıcı Yönetimi|AccessReview_Review|
|Kullanıcı Yönetimi|AccessReview_Update|
|Kullanıcı Yönetimi|ActivationAborted|
|Kullanıcı Yönetimi|ActivationApproved|
|Kullanıcı Yönetimi|ActivationCanceled|
|Kullanıcı Yönetimi|ActivationRequested|
|Kullanıcı Yönetimi|Eklendi|
|Kullanıcı Yönetimi|Ata|


## <a name="identity-protection"></a>Kimlik koruması

|Denetim Kategorisi|Etkinlik|
|---|---|
|Dizin Yönetimi|Yükselt|
|Dizin Yönetimi|Kaldırıldı|
|Dizin Yönetimi|Rol Ayarı değişiklikleri|
|Diğer|ScanAlertsNow|
|Diğer|Kaydol|
|Diğer|Yetkiyi Kaldır|
|Diğer|UpdateAlertSettings|
|Diğer|UpdateCurrentState|
|İlke Yönetimi|Erişim gözden geçirmesi sona erdi|
|İlke Yönetimi|İstek onayına onaylayan ekleme|
|İlke Yönetimi|Erişim gözden geçirmesine gözden geçiren ekleme|
|Kullanıcı Yönetimi|Erişim incelemesi uygula|
|Kullanıcı Yönetimi|Erişim incelemesi oluştur|


## <a name="invited-users"></a>Davetli kullanıcılar

|Denetim Kategorisi|Etkinlik|
|---|---|
|Diğer|İstek onayı oluşturma|
|Diğer|Erişim incelemesini sil|
|Kullanıcı Yönetimi|Erişim gözden geçirmesinden gözden geçireni kaldırma|
|Kullanıcı Yönetimi|Gözden geçirme sonucunu uygulama işlemi iste|
|Kullanıcı Yönetimi|Gözden Geçirmeyi Durdurmayı İsteme|
|Kullanıcı Yönetimi|Uygulama atamasını gözden geçirme|
|Kullanıcı Yönetimi|Grup üyeliğini gözden geçirme|
|Kullanıcı Yönetimi|Rbac Rolü üyeliğini gözden geçirme|


## <a name="microsoft-identity-manager-mim"></a>Microsoft Identity Manager (MIM)

|Denetim Kategorisi|Etkinlik|
|---|---|
|Grup Yönetimi|İstek onayı isteğini gözden geçirme|
|Grup Yönetimi|Erişim Gözden Geçirmesini Güncelleştirme|
|Grup Yönetimi|Erişim gözden geçirmesi posta bildirimi ayarlarını güncelleştirme|
|Grup Yönetimi|Erişim gözden geçirme yinelenme sayısı ayarını Güncelleştir|
|Grup Yönetimi|Erişim gözden geçirme gün cinsinden yineleme süresini Güncelleştir ayarı|
|Kullanıcı Yönetimi|Erişim gözden geçirme yineleme bitiş türü ayarını Güncelleştir|
|Kullanıcı Yönetimi|Erişim gözden geçirme yineleme türü ayarını Güncelleştir|



## <a name="privileged-identity-management"></a>Privileged Identity Management

|Denetim Kategorisi|Etkinlik|
|---|---|
|PIM|ActivationAborted|
|PIM|ActivationApproved|
|PIM|ActivationCanceled|
|PIM|Activationreddedildi|
|PIM|ActivationRequested|
|PIM|Eklendi|
|PIM|AddedOutsidePIM|
|PIM|Ata|
|PIM|Dissalert|
|PIM|Yükselt|
|PIM|ReactivateAlert|
|PIM|Kaldırıldı|
|PIM|RemovedOutsidePIM|
|PIM|Gözden Geçirmeyi Durdurmayı İsteme|
|PIM|Rol Ayarı değişiklikleri|
|PIM|ScanAlertsNow|
|PIM|Kaydol|
|PIM|Kaldırmanız|
|PIM|Yetkiyi Kaldır|
|PIM|UpdateAlertSettings|
|PIM|UpdateCurrentState|


## <a name="self-service-group-management"></a>Self servis grup yönetimi

|Denetim Kategorisi|Etkinlik|
|---|---|
|Grup Yönetimi|Kullanıcı parolasını sıfırlama|
|Grup Yönetimi|Kullanıcıyı geri yükle|
|Grup Yönetimi|Kullanıcı parolasını değiştirmeye zorlamayı ayarlama|
|Grup Yönetimi|Kullanıcı yöneticisini ayarlama|
|Grup Yönetimi|Kullanıcılar için kimlik doğrulaması belirteci meta verilerini etkin olarak ayarlama|
|Grup Yönetimi|StsRefreshTokenValidFrom Zaman Damgasını Güncelleştirme|
|Grup Yönetimi|Dış gizli anahtarları güncelleştirme|
|Grup Yönetimi|Kullanıcıyı güncelleştir|
|Grup Yönetimi|Yönetici geçici bir parola oluşturur|


## <a name="self-service-password-management"></a>Self servis parola yönetimi

|Denetim Kategorisi|Etkinlik|
|---|---|
|Dizin Yönetimi|Yöneticiler, kullanıcının parolasını sıfırlamasını zorunlu tutar|
|Dizin Yönetimi|Uygulamaya dış kullanıcı atama|
|Kullanıcı Yönetimi|E-posta gönderilmedi, kullanıcının aboneliği kaldırıldı|
|Kullanıcı Yönetimi|Dış kullanıcıyı davet etme|
|Kullanıcı Yönetimi|Dış kullanıcı davetini kullanma|
|Kullanıcı Yönetimi|Virüslü kiracı oluşturma|
|Kullanıcı Yönetimi|Virüslü kullanıcı oluşturma|
|Kullanıcı Yönetimi|Kullanıcı Parolası Kaydı|
|Kullanıcı Yönetimi|Kullanıcı Parolası Sıfırlama|
|Kullanıcı Yönetimi|Self servis parola sıfırlaması engellendi|


## <a name="terms-of-use"></a>Kullanım koşulları

|Denetim Kategorisi|Etkinlik|
|---|---|
|Kullanım Koşulları|Kullanım Koşullarını Kabul Etme|
|Kullanım Koşulları|Kullanım Koşulları Oluşturma|
|Kullanım Koşulları|Kullanım Koşullarını Reddetme|
|Kullanım Koşulları|Onayı Sil|
|Kullanım Koşulları|Kullanım Koşullarını Silme|
|Kullanım Koşulları|Kullanım Koşullarını Düzenleme|
|Kullanım Koşulları|Süre sonu kullanım koşulları|
|Kullanım Koşulları|Kullanım koşullarını kalıcı olarak sil|
|Kullanım Koşulları|Kullanım Koşullarını Yayımlama|
|Kullanım Koşulları|Kullanım Koşullarını Yayımdan Kaldırma|


## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD raporlarına genel bakış](overview-reports.md).
- [Denetim günlükleri raporu](concept-audit-logs.md). 
- [Azure AD raporlarına programlı erişim](concept-reporting-api.md)
