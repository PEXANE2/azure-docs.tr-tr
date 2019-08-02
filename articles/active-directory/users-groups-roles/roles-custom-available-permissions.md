---
title: Uygulama kayıt yönetimi için özel yönetici rolü izinleri-Azure Active Directory | Microsoft Docs
description: Kimlik yönetimi temsilcisi için özel yönetici rolü izinleri.
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 07/31/2019
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3e5ff6755f1391ff19e65df669fb51967a904f4f
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68707571"
---
# <a name="application-registration-subtypes-and-permissions-in-azure-active-directory"></a>Azure Active Directory 'de uygulama kaydı alt türleri ve izinleri

Bu makale, Azure Active Directory (Azure AD) içindeki özel rol tanımları için şu anda kullanılabilir olan uygulama kayıt izinlerini içerir.

## <a name="single-tenant-v-multi-tenant-permissions"></a>Tek kiracılı v. çok kiracılı izinler

Özel rol izinleri, tek kiracılı ve çok kiracılı uygulamalar için farklılık gösterir. Tek kiracılı uygulamalar yalnızca uygulamanın kaydedildiği Azure AD kuruluşundaki kullanıcılar tarafından kullanılabilir. Çok kiracılı uygulamalar tüm Azure AD kuruluşları tarafından kullanılabilir. Tek kiracılı uygulamalar **Desteklenen hesap türleri** "yalnızca bu kuruluş dizininde hesaplar" olarak ayarlanmış şekilde tanımlanır. Graph API, tek kiracılı uygulamalarda Signınaudience özelliği "AzureADMyOrg" olarak ayarlanmıştır.

## <a name="application-registration-subtypes-and-permissions"></a>Uygulama kaydı alt türleri ve izinleri

Genel terimler alt türünün, izninin ve özellik kümesinin ne anlama geldiğini öğrenmek için [özel rollere genel bakış ' a](roles-custom-overview.md) bakın. Aşağıdaki bilgiler uygulama kayıtlarına özgüdür.

### <a name="subtypes"></a>Alt türleri

Yalnızca bir uygulama kaydı alt türü vardır-uygulamalar. Myorganleştirme. Örneğin, Microsoft. Directory/Applications. Myorganleştirme/temel/güncelleştirme. Bu alt tür, belirli bir uygulama kaydının **kimlik doğrulama** sayfasında ayarlanır ve Graph API veya PowerShell kullanılarak Signınaudience özelliğinin "AzureADMyOrg" olarak ayarlanmasına karşılık gelir. Alt tür, yalnızca kuruluşunuzdaki hesaplarla (tek kiracılı uygulamalar) erişilebilir olarak işaretlenen uygulama kayıtlarıyla izinleri kısıtlar.

Yalnızca diğer kuruluşlardaki hesaplar tarafından erişilebilen uygulamalara okuma veya yönetme izinleri vermeden, iç uygulamalara yönelik okuma veya yönetme izinleri vermek için sınırlı izni kullanabilirsiniz.

Tüm okuma ve Güncelleştirme izinlerinin yanı sıra Silme iznini de olan uygulamalar. Şu anda oluşturulacak bir uygulama yok. oluşturma sürümü. Standart izinler (örneğin, Microsoft. Directory/Applications/Basic/Update) tüm uygulama kayıt türleri için okuma veya yönetim izinleri verir.

![Tek kiracılı bir uygulama veya çok kiracılı uygulama bildirme](./media/roles-custom-available-permissions/supported-account-types.png)

Özel roller önizlemesi için aşağıdaki izinlerin ayrıntıları, [Azure Active Directory ' de kullanılabilir özel rol izinlerinde](roles-custom-available-permissions.md)listelenmiştir.

### <a name="create-and-delete"></a>Oluştur ve Sil

Uygulama kayıtları oluşturma özelliği için iki izin mevcuttur:

- **Microsoft. Directory/Applications/createAsOwner**
- **Microsoft. Directory/uygulamalar/oluşturma**

Her iki izin atanırsa, oluşturma izni öncelikli olur. CreateAsOwner izni oluşturucuyu ilk sahip olarak otomatik olarak eklememese de, Graph API 'Leri veya PowerShell cmdlet 'leri kullanılırken uygulama kaydı oluşturma sırasında sahipler belirtilebilir.

İzin oluştur **Yeni kayıt** komutuna erişim izni verir.

[Bu izinler yeni kayıt portalı komutuna erişim izni verir](./media/roles-create-custom/new-custom-role.png)

Uygulama kayıtlarını silebilme olanağı vermek için iki izin mevcuttur:

#### <a name="microsoftdirectoryapplicationsdelete"></a>Microsoft. Directory/uygulamalar/Sil

Subtype 'dan bağımsız olarak uygulama kayıtlarını silme yeteneği verir; diğer bir deyişle, tek kiracılı ve çok kiracılı uygulamalar.

#### <a name="microsoftdirectoryapplicationsmyorganizationdelete"></a>Microsoft. Directory/Applications. Myorganleştirme/silme

Yalnızca kuruluşunuzdaki hesaplara veya tek kiracılı uygulamalardaki (Myorganın alt türü) erişilebilen uygulama kayıtlarını silme yeteneği verir.

![Bu izinler, uygulama kaydını Sil komutuna erişim verir](./media/roles-custom-available-permissions/delete-app-registration.png)

> [!NOTE]
> Oluşturma izinleri içeren bir rol atarken, rol atamasının dizin kapsamında yapılması gerekir. Kaynak kapsamında atanan bir oluşturma izni, uygulama kayıtları oluşturma olanağı sağlamaz.

### <a name="read"></a>Okuma

Kuruluştaki tüm üye kullanıcıları, varsayılan olarak uygulama kayıt bilgilerini okuyabilir. Ancak Konuk kullanıcılar ve uygulama hizmeti sorumluları olamaz. Bir Konuk Kullanıcı veya uygulamaya bir rol atamayı planlıyorsanız, uygun okuma izinlerini dahil etmeniz gerekir.

#### <a name="microsoftdirectoryapplicationsallpropertiesread"></a>Microsoft. Directory/uygulamalar/allProperties/Read

Kimlik bilgileri gibi hassas özelliklerin dışındaki tek kiracılı ve çok kiracılı uygulamaların tüm özelliklerini okuma özelliği.

#### <a name="microsoftdirectoryapplicationsmyorganizationallpropertiesread"></a>Microsoft. Directory/Applications. Myorganleştirme/allProperties/Read

Yalnızca tek kiracılı uygulamalar için Microsoft. Directory/Applications/allProperties/Read ile aynı izinleri verir.

#### <a name="microsoftdirectoryapplicationsstandardread-grants-access-to-all-fields-on-the-application-registration-branding-page"></a>Microsoft. Directory/uygulamalar/standart/okuma: Uygulama kaydı markalama sayfasındaki tüm alanlara erişim izni verir

![Bu izin, uygulama kaydı markalama sayfasına erişim verir](./media/roles-custom-available-permissions/app-registration-branding.png)

#### <a name="microsoftdirectoryapplicationsmyorganizationstandardread"></a>Microsoft. Directory/Applications. Myorganleştirme/standart/okuma

Yalnızca tek kiracılı uygulamalar için Microsoft. Directory/Applications/standart/Read gibi aynı izinleri verir.

#### <a name="microsoftdirectoryapplicationsownersread"></a>Microsoft. Directory/uygulamalar/sahipler/okuma

Tek kiracılı ve çok kiracılı uygulamalardaki sahipler özelliğini okuma yeteneği verir. Uygulama kayıt sahipleri sayfasındaki tüm alanlara erişim izni verir:

![Bu izinler, uygulama kayıt sahipleri sayfasına erişim verir](./media/roles-custom-available-permissions/app-registration-owners.png)

Uygulama varlığında aşağıdaki özelliklere erişim verir:

- AllowActAsForAllClients
- Allowpassthroughkullanıcıları
- AppAddress
- AppBrandingElements
- AppCategory
- AppCreatedDateTime
- AppData
- Uygulama Kimliği
- Appınformationalurl 'Si
- ApplicationTag
- AppLogoUrl 'Si
- AppMetadata
- AppOptions
- BinaryExtensionAttribute
- BooleanExtensionAttribute
- CountriesBlockedForMinors
- Createdonbeyarıın
- DateTimeExtensionAttribute
- DisplayName
- ExtensionAttributeDefinition
- Integerextensionattribute
- KnownClientApplications
- Largeıntegerextensionattribute
- Ligalagegrouprule
- LocalizedAppBrandingElements
- Ana logo
- Msaappıd
- ResourceApplicationSet
- ServiceDiscoveryEndpoint
- StringExtensionAttribute
- TrustedCertificateSubject
- WebApi
- Web
- WwwHomepage

### <a name="update"></a>Güncelleştirme

#### <a name="microsoftdirectoryapplicationsallpropertiesupdate"></a>Microsoft. Directory/uygulamalar/allProperties/Update

#### <a name="microsoftdirectoryapplicationsmyorganizationallpropertiesupdate"></a>Microsoft. Directory/Applications. Myorganleştirme/allProperties/Update

Yalnızca tek kiracılı uygulamalar için Microsoft. Directory/Applications/allProperties/Update ile aynı izinleri verir.

#### <a name="microsoftdirectoryapplicationsaudienceupdate"></a>Microsoft. Directory/uygulamalar/hedef kitle/güncelleştirme

Uygulama kayıt kimlik doğrulama sayfasındaki tüm alanlara erişim izni verir:

![Bu izin, uygulama kayıt kimlik doğrulama sayfasına erişim verir](./media/roles-custom-available-permissions/supported-account-types.png)

Uygulama kaynağında aşağıdaki özelliklere erişim verir:

- Availabletootherkiracılar
- Signınkitci

#### <a name="microsoftdirectoryapplicationsmyorganizationaudienceupdate"></a>Microsoft. Directory/Applications. Myorganleştirme/izleyici/güncelleştirme

Yalnızca tek kiracılı uygulamalar için Microsoft. Directory/uygulamalar/hedef kitle/güncelleştirme ile aynı izinleri verir.

#### <a name="microsoftdirectoryapplicationsauthenticationupdate"></a>Microsoft. Directory/uygulamalar/kimlik doğrulaması/güncelleştirme

Tek kiracılı ve çok kiracılı uygulamalarda yanıt URL 'SI, oturum kapatma URL 'SI, örtük akış ve yayımcı etki alanı özelliklerini güncelleştirme özelliği. Desteklenen hesap türleri dışındaki uygulama kaydı kimlik doğrulama sayfasındaki tüm alanlara erişim verir:

![Uygulama kaydı kimlik doğrulamasına erişim izni verir ancak desteklenmez.](./media/roles-custom-available-permissions/supported-account-types.png)

 Uygulama kaynağında aşağıdaki özelliklere erişim verir:

- Acceptmappedtalepler
- AccessTokenAcceptedVersion
- Eklentileri
- Groupmembershipclaim
- Ideviceonlyauthsupported
- OAuth2LegacyUrlPathMatching
- Oauthoıdcresponsepolicybitmap
- Optionalclaim
- OrgRestrictions
- PublicClient
- UseCustomTokenSigningKey

#### <a name="microsoftdirectoryapplicationsmyorganizationauthenticationupdate"></a>Microsoft. Directory/Applications. Myorganleştirme/kimlik doğrulama/güncelleştirme

Yalnızca tek kiracılı uygulamalar için Microsoft. Directory/uygulamalar/kimlik doğrulama/güncelleştirme ile aynı izinleri verir.

#### <a name="microsoftdirectoryapplicationsbasicupdate"></a>Microsoft. Directory/uygulamalar/temel/güncelleştirme

Tek kiracılı ve çok kiracılı uygulamalardaki adı, logoyu, ana hat URL 'sini, hizmet URL 'sini ve gizlilik bildirimi URL özelliklerini güncelleştirme özelliği. Uygulama kaydı markalama sayfasındaki tüm alanlara erişim izni verir:

![Bu izin, uygulama kaydı markalama sayfasına erişim verir](./media/roles-custom-available-permissions/app-registration-branding.png)

Uygulama kaynağında aşağıdaki özelliklere erişim verir:

- AllowActAsForAllClients
- Allowpassthroughkullanıcıları
- AppAddress
- AppBrandingElements
- AppCategory
- AppData
- Uygulama Kimliği
- Appınformationalurl 'Si
- ApplicationTag
- AppLogoUrl 'Si
- AppMetadata
- AppOptions
- BinaryExtensionAttribute
- BooleanExtensionAttribute
- CountriesBlockedForMinors
- Createdonbeyarıın
- DateTimeExtensionAttribute
- DisplayName
- ExtensionAttributeDefinition
- Integerextensionattribute
- KnownClientApplications
- Largeıntegerextensionattribute
- Ligalagegrouprule
- LocalizedAppBrandingElements
- Ana logo
- Msaappıd
- ResourceApplicationSet
- ServiceDiscoveryEndpoint
- StringExtensionAttribute
- TrustedCertificateSubject
- WebApi
- Web
- WwwHomepage

#### <a name="microsoftdirectoryapplicationsmyorganizationbasicupdate"></a>Microsoft. Directory/Applications. Myorganleştirme/temel/güncelleştirme

Yalnızca tek kiracılı uygulamalar için Microsoft. Directory/Applications/Basic/Update ile aynı izinleri verir.

#### <a name="microsoftdirectoryapplicationscredentialsupdate"></a>Microsoft. Directory/uygulamalar/kimlik bilgileri/güncelleştirme

Tek kiracılı ve çok kiracılı uygulamalardaki sertifikaları ve istemci gizli dizilerini güncelleştirme özelliği. Uygulama kayıt sertifikaları & gizlilikler sayfasındaki tüm alanlara erişim verir:

![Bu izin, uygulama kayıt sertifikalarına & gizli dizi sayfasına erişim verir](./media/roles-custom-available-permissions/app-registration-secrets.png)

Uygulama kaynağında aşağıdaki özelliklere erişim verir:
- AsymmetricKey
- EncryptedSecretKey
- KeyDescription
- SharedKeyReference
- Tokenencryptionkeyıd

#### <a name="microsoftdirectoryapplicationsmyorganizationcredentialsupdate"></a>Microsoft. Directory/Applications. Myorganleştirme/kimlik bilgileri/güncelleştirme

Yalnızca tek dizin uygulamaları için Microsoft. Directory/uygulamalar/kimlik bilgileri/güncelleştirme ile aynı izinleri verir.

#### <a name="microsoftdirectoryapplicationsownersupdate"></a>Microsoft. Directory/uygulamalar/sahipler/güncelleştirme

Tek kiracılı ve çok kiracılı üzerinde Owner özelliğini güncelleştirme özelliği. Uygulama kayıt sahipleri sayfasındaki tüm alanlara erişim izni verir:

![Bu izinler, uygulama kayıt sahipleri sayfasına erişim verir](./media/roles-custom-available-permissions/app-registration-owners.png)

Uygulama kaynağında aşağıdaki özelliklere erişim verir:
- Sahipler

#### <a name="microsoftdirectoryapplicationsmyorganizationownersupdate"></a>Microsoft. Directory/Applications. Myorganleştirme/sahipler/güncelleştirme

Yalnızca tek kiracılı uygulamalar için Microsoft. Directory/Applications/Owners/Update ile aynı izinleri verir.

#### <a name="microsoftdirectoryapplicationspermissionsupdate"></a>Microsoft. Directory/uygulamalar/izinler/güncelleştirme

Temsilci izinleri, uygulama izinleri, yetkilendirilmiş istemci uygulamaları, gerekli izinler ve tek kiracılı ve çok kiracılı uygulamalarda izin özellikleri verme özelliği. İzin verme olanağı sağlamaz. Uygulama kaydı API 'SI izinlerinde tüm alanlara erişim verir ve bir API sayfası sunar:

![Bu izinler, uygulama kaydı API 'SI izinleri sayfasına erişim verir](./media/roles-custom-available-permissions/app-registration-api-permissions.png)

![Bu izinler, bir API sayfasını açığa çıkarmak için uygulama kaydına erişim verir](./media/roles-custom-available-permissions/app-registration-expose-api.png)

Uygulama kaynağında aşağıdaki özelliklere erişim verir:

- Appıdentifieruri
- Yetkilendirme
- Ön kimlik doğrulama
- RecordConsentConditions
- RequiredResourceAccess

#### <a name="microsoftdirectoryapplicationsmyorganizationpermissionsupdate"></a>Microsoft. Directory/Applications. Myorganleştirme/izinler/güncelleştirme

Yalnızca tek kiracılı uygulamalar için Microsoft. Directory/Applications/Permissions/Update ile aynı izinleri verir.

## <a name="required-license-plan"></a>Gerekli lisans planı

[!INCLUDE [License requirement for using custom roles in Azure AD](../../../includes/active-directory-p1-license.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Portal, Azure AD PowerShell ve Graph API](roles-create-custom.md) kullanarak özel roller oluşturun
- [Özel bir rol için atamaları görüntüleme](roles-view-assignments.md)
