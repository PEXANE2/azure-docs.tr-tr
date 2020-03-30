---
title: Kullanılabilir özel yönetici rol izinleri - Azure AD | Microsoft Dokümanlar
description: Kimlik yönetimini atamak için özel yönetici rol izinleri.
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 11/08/2019
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: d6156857202c1cca94df6d70ec2059daf55178f1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74025157"
---
# <a name="application-registration-subtypes-and-permissions-in-azure-active-directory"></a>Azure Active Directory'de uygulama kaydı alt türleri ve izinleri

Bu makale, Azure Etkin Dizini'ndeki (Azure AD) özel rol tanımları için şu anda kullanılabilen uygulama kaydı izinlerini içerir.

## <a name="permissions-for-managing-single-directory-applications"></a>Tek dizin uygulamalarını yönetme izinleri

Özel rolünüz için izinleri seçerken, yalnızca tek dizin uygulamalarını yönetmek için erişim izni verme seçeneğiniz vardır. Tek dizin uygulamaları yalnızca uygulamanın kayıtlı olduğu Azure AD kuruluşundaki kullanıcılar tarafından kullanılabilir. Tek dizin uygulamaları, Desteklenen **hesap türlerinin** "Yalnızca bu kuruluş dizinindeki hesaplar" olarak ayarlandığı olarak tanımlanır. Grafik API'sinde, tek dizin li uygulamalarda signInAudience özelliği "AzureADMyOrg" olarak ayarlanır.

Yalnızca tek dizin uygulamalarını yönetmeye erişim sağlamak için, alt tür **uygulamaları.myOrganization**ile aşağıdaki izinleri kullanın. Örneğin, microsoft.directory/applications.myOrganization/basic/update.

Genel [custom roles overview](roles-custom-overview.md) terimler alt türü, izin ve özellik kümesinin ne anlama geldiğini niçin açıklayabiliyor. Aşağıdaki bilgiler uygulama kayıtlarıiçin özeldir.

### <a name="create-and-delete"></a>Oluşturma ve silme

Her biri farklı davranışta bulunan uygulama kayıtları oluşturma olanağı vermek için kullanılabilen iki izin vardır:

#### <a name="microsoftdirectoryapplicationscreateasowner"></a>microsoft.directory/applications/createAsOwner

Bu izin atanması, oluşturulan uygulama kaydının ilk sahibi olarak eklenen oluşturucuya neden olur ve oluşturulan uygulama kaydı, oluşturucunun oluşturulan 250 nesne kotasına dahil edilir.

#### <a name="microsoftdirectoryapplicationscreate"></a>microsoft.directory/applications/create

Bu izin atanması, oluşturulan uygulama kaydının ilk sahibi olarak eklenmemesi ile sonuçlanır ve oluşturulan uygulama kaydı, oluşturucunun oluşturulan 250 nesne kotasına dahil edilmez. Dizin düzeyindekota vuruluncaya kadar devralan firmanın uygulama kayıtları oluşturmasını engelleyen hiçbir şey olmadığından, bu izni dikkatle kullanın. Her iki izin de atanmışsa, bu izin önceliklidir.

Her iki izin de atanırsa, /oluşturma izni önceliklidir. /createAsOwner izni otomatik olarak ilk sahibi olarak oluşturucu eklemese de, sahipleri Grafik API'leri veya PowerShell cmdlets kullanırken uygulama kaydının oluşturulması sırasında belirtilebilir.

İzinler **oluşturun, Yeni kayıt** komutuna erişim verir.

[Bu izinler Yeni Kayıt portalı komutuna erişim verir](./media/roles-create-custom/new-custom-role.png)

Uygulama kayıtlarını silme olanağı sağlamak için iki izin vardır:

#### <a name="microsoftdirectoryapplicationsdelete"></a>microsoft.directory/applications/delete

Alt türe bakılmaksızın uygulama kayıtlarını silme olanağı verir; diğer bir şey, hem tek kiracılı hem de çok kiracılı uygulamalar.

#### <a name="microsoftdirectoryapplicationsmyorganizationdelete"></a>microsoft.directory/applications.myOrganization/delete

Yalnızca kuruluşunuzdaki veya tek kiracılı uygulamalardaki (myOrganization alt türü) yalnızca erişilebilen uygulamalarla sınırlı uygulama kayıtlarını silme olanağı sağlar.

![Bu izinler, Delete uygulaması kayıt komutuna erişim verir](./media/roles-custom-available-permissions/delete-app-registration.png)

> [!NOTE]
> Oluşturma izinleri içeren bir rol atarken, rol atamasının dizin kapsamında yapılması gerekir. Kaynak kapsamında atanan oluşturma izni, uygulama kayıtları oluşturma olanağı sağlamaz.

### <a name="read"></a>Okuma

Kuruluştaki tüm üye kullanıcılar varsayılan olarak uygulama kayıt bilgilerini okuyabilir. Ancak, konuk kullanıcılar ve uygulama hizmeti ilkeleri bunu yapamaz. Bir rol konuk kullanıcıya veya uygulamaya atamayı planlıyorsanız, uygun okuma izinlerini eklemeniz gerekir.

#### <a name="microsoftdirectoryapplicationsallpropertiesread"></a>microsoft.directory/applications/allProperties/read

Kimlik bilgileri gibi herhangi bir durumda okunamayan özellikleridışında tek kiracı ve çok kiracılı uygulamaların tüm özelliklerini okuma yeteneği.

#### <a name="microsoftdirectoryapplicationsmyorganizationallpropertiesread"></a>microsoft.directory/applications.myOrganization/allProperties/read

Microsoft.directory/applications/allProperties/read ile aynı izinleri verir, ancak yalnızca tek kiracılı uygulamalar için.

#### <a name="microsoftdirectoryapplicationsownersread"></a>microsoft.directory/applications/owners/read

Tek kiracılı ve çok kiracılı uygulamalarda sahiplerinin mülklerini okuma olanağı verir. Başvuru kayıt sahipleri sayfasındaki tüm alanlara erişim izni verir:

![Bu izinler, uygulama kayıt sahipleri sayfasına erişim izni verir](./media/roles-custom-available-permissions/app-registration-owners.png)

#### <a name="microsoftdirectoryapplicationsstandardread"></a>microsoft.directory/applications/standard/read

Okuma standart uygulama kayıt özelliklerine erişim sağlar. Bu, uygulama kayıt sayfalarındaki özellikleri içerir.

#### <a name="microsoftdirectoryapplicationsmyorganizationstandardread"></a>microsoft.directory/applications.myOrganization/standard/read

Microsoft.directory/applications/standard/read ile aynı izinleri verir, ancak yalnızca tek kiracılı uygulamalar için.

### <a name="update"></a>Güncelleştirme

#### <a name="microsoftdirectoryapplicationsallpropertiesupdate"></a>microsoft.directory/applications/allProperties/update

Tek dizin ve çok dizin uygulamalarındaki tüm özellikleri güncelleştirme yeteneği.

#### <a name="microsoftdirectoryapplicationsmyorganizationallpropertiesupdate"></a>microsoft.directory/applications.myOrganization/allProperties/update

Microsoft.directory/applications/allProperties/update ile aynı izinleri verir, ancak yalnızca tek kiracılı uygulamalar için.

#### <a name="microsoftdirectoryapplicationsaudienceupdate"></a>microsoft.directory/applications/audience/update

Desteklenen hesap türünü (signInAudience) özelliğini tek dizin ve çok dizinli uygulamalarda güncelleştirme yeteneği.

![Bu izin, kimlik doğrulama sayfasında uygulama kaydı destekli hesap türü özelliğine erişim verir](./media/roles-custom-available-permissions/supported-account-types.png)

#### <a name="microsoftdirectoryapplicationsmyorganizationaudienceupdate"></a>microsoft.directory/applications.myOrganization/audience/update

Microsoft.directory/applications/audience/update ile aynı izinleri verir, ancak yalnızca tek kiracılı uygulamalar için.

#### <a name="microsoftdirectoryapplicationsauthenticationupdate"></a>microsoft.directory/applications/authentication/update

Tek kiracılı ve çok kiracılı uygulamalarda yanıt URL'sini, oturum açma URL'sini, örtülü akışı ve yayımcı etki alanı özelliklerini güncelleştirme yeteneği. Desteklenen hesap türleri dışında uygulama kaydı kimlik doğrulama sayfasındaki tüm alanlara erişim izni verir:

![Uygulama kaydı kimlik doğrulamasına erişim verir, ancak desteklenmeyen hesap türlerine](./media/roles-custom-available-permissions/supported-account-types.png)

#### <a name="microsoftdirectoryapplicationsmyorganizationauthenticationupdate"></a>microsoft.directory/applications.myOrganization/authentication/update

Microsoft.directory/applications/authentication/update ile aynı izinleri verir, ancak yalnızca tek kiracılı uygulamalar için.

#### <a name="microsoftdirectoryapplicationsbasicupdate"></a>microsoft.directory/applications/basic/update

Tek kiracılı ve çok kiracılı uygulamalarda ad, logo, ana sayfa URL'si, hizmet koşulları URL'si ve gizlilik bildirimi URL özelliklerini güncelleştirme yeteneği. Başvuru kayıt markalama sayfasındaki tüm alanlara erişim izni verir:

![Bu izin, uygulama kaydı markalandırma sayfasına erişim izni verir](./media/roles-custom-available-permissions/app-registration-branding.png)

#### <a name="microsoftdirectoryapplicationsmyorganizationbasicupdate"></a>microsoft.directory/applications.myOrganization/basic/update

Microsoft.directory/applications/basic/update ile aynı izinleri verir, ancak yalnızca tek kiracılı uygulamalar için.

#### <a name="microsoftdirectoryapplicationscredentialsupdate"></a>microsoft.directory/applications/credentials/update

Tek kiracılı ve çok kiracılı uygulamalarda sertifikaları ve istemci sırlarını güncelleştirme yeteneği. Başvuru kayıt sertifikaları & sırlar sayfasındaki tüm alanlara erişim verir:

![Bu izin, uygulama kayıt sertifikaları & sırlar sayfasına erişim izni verir](./media/roles-custom-available-permissions/app-registration-secrets.png)

#### <a name="microsoftdirectoryapplicationsmyorganizationcredentialsupdate"></a>microsoft.directory/applications.myOrganization/credentials/update

Microsoft.directory/applications/credentials/update ile aynı izinleri verir, ancak yalnızca tek dizin uygulamaları için.

#### <a name="microsoftdirectoryapplicationsownersupdate"></a>microsoft.directory/applications/owners/update

Tek kiracı ve çok kiracıüzerinde sahibi mülkiyet güncelleştirme yeteneği. Başvuru kayıt sahipleri sayfasındaki tüm alanlara erişim izni verir:

![Bu izinler, uygulama kayıt sahipleri sayfasına erişim izni verir](./media/roles-custom-available-permissions/app-registration-owners.png)

#### <a name="microsoftdirectoryapplicationsmyorganizationownersupdate"></a>microsoft.directory/applications.myOrganization/owners/update

Microsoft.directory/applications/owners/update ile aynı izinleri verir, ancak yalnızca tek kiracılı uygulamalar için.

#### <a name="microsoftdirectoryapplicationspermissionsupdate"></a>microsoft.directory/applications/permissions/update

Devredilen izinleri, uygulama izinlerini, yetkili istemci uygulamalarını, gerekli izinleri güncelleştirme ve tek kiracılı ve çok kiracılı uygulamalarda onay özellikleri verme becerisi. İzin verme olanağı vermez. Uygulama kaydı API izinleri üzerindeki tüm alanlara erişim sağlar ve bir API sayfasını açığa çıkarır:

![Bu izinler, uygulama kaydı API izinleri sayfasına erişim izni verir](./media/roles-custom-available-permissions/app-registration-api-permissions.png)

![Bu izinler, uygulama kaydına erişim izni verir ApI sayfasını göster](./media/roles-custom-available-permissions/app-registration-expose-api.png)

#### <a name="microsoftdirectoryapplicationsmyorganizationpermissionsupdate"></a>microsoft.directory/applications.myOrganization/permissions/update

Microsoft.directory/applications/permissions/update ile aynı izinleri verir, ancak yalnızca tek kiracılı uygulamalar için.

## <a name="required-license-plan"></a>Gerekli lisans planı

[!INCLUDE [License requirement for using custom roles in Azure AD](../../../includes/active-directory-p1-license.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Azure portalı, Azure AD PowerShell ve Grafik API'sini](roles-create-custom.md) kullanarak özel roller oluşturun
- [Özel bir rol için atamaları görüntüleme](roles-view-assignments.md)
