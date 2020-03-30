---
title: Varsayılan kullanıcı izinleri - Azure Etkin Dizin | Microsoft Dokümanlar
description: Azure Active Directory'de kullanılabilen farklı kullanıcı izinleri hakkında bilgi edinin.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 02/16/2019
ms.author: ajburnle
ms.reviewer: vincesm
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 227230f2d6f46fae27e2cec69d99390f5054c7db
ms.sourcegitcommit: 07d62796de0d1f9c0fa14bfcc425f852fdb08fb1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80366257"
---
# <a name="what-are-the-default-user-permissions-in-azure-active-directory"></a>Azure Etkin Dizin'de varsayılan kullanıcı izinleri nelerdir?
Azure Active Directory'de (Azure AD) tüm kullanıcılara varsayılan olarak belirli izinler verilir. Bir kullanıcının erişimi, kullanıcının türünden, [rol atamalarından](active-directory-users-assign-role-azure-portal.md)ve tek tek nesnelerin sahipliğinden oluşur. Bu makalede bu varsayılan izinler ve açıklanmakta ve üye ile konuk varsayılan değerleri karşılaştırılmaktadır. Varsayılan kullanıcı izinleri yalnızca Azure AD'deki kullanıcı ayarlarında değiştirilebilir.

## <a name="member-and-guest-users"></a>Üyeler ve konuk kullanıcılar
Alınan varsayılan izinler kümesi, kullanıcının kiracının (üye kullanıcının) yerel üyesi olup olmadığına veya kullanıcının b2B işbirliği konuğu (konuk kullanıcı) olarak başka bir dizinden getirilip getirilmediğine bağlıdır. Azure [AD B2B işbirliği nedir?](../b2b/what-is-b2b.md)
* Üye kullanıcılar uygulama kaydedebilir, kendi profil fotoğraflarını ve cep telefonu numaralarını yönetebilir, kendi parolalarını değiştirebilir ve B2B konuklarını davet edebilirler. Ayrıca kullanıcılar dizindeki tüm bilgileri okuyabilir (birkaç özel durum haricinde). 
* Konuk kullanıcıların dizin izinleri kısıtlandı. Örneğin konuk kullanıcılar kendi profil bilgileri dışındaki kiracı bilgilerine ulaşamaz. Ancak konuk kullanıcılar Kullanıcı Asıl Adı veya nesne kimliği kullanarak diğer kullanıcılar hakkında bilgi alabilir. Konuk kullanıcı, **konuk kullanıcı izinleri sınırlı** ayarne bakılmaksızın grup üyeliği de dahil olmak üzere ait oldukları grupların özelliklerini okuyabilir. Konuk, diğer kiracı nesneleri hakkındaki bilgileri görüntüleyemez.

Konuklar için varsayılan izinler varsayılan olarak kısıtlanmıştır. Konuklar yönetici rollerine eklenerek rol kapsamındaki tüm yazma ve okuma izinlerine erişmeleri sağlanabilir. Konuklara uygulanabilen diğer bir kısıtlama da başka konukları davet etme özelliğidir. **Konuklar davet edebilir** ayarını **Hayır** olarak belirlerseniz konuklar başka konuk davet edemez. Nasıl yapılacağını öğrenmek için bkz. [B2B işbirliği için davetiye temsilcisi seçme](../b2b/delegate-invitations.md). Konuk kullanıcılara varsayılan olarak üye kullanıcılarla aynı izinleri vermek için **Konuk kullanıcıların izinleri sınırlıdır** ayarını **Hayır** olarak belirleyin. Bu ayar varsayılan olarak konuk kullanıcılara tüm üye kullanıcı izinlerini vermenin yanı sıra konuklara yönetici rollerinin verilmesini sağlar.

## <a name="compare-member-and-guest-default-permissions"></a>Üye ve konuk varsayılan izinlerini karşılaştırma

**Alan** | **Üye kullanıcı izinleri** | **Konuk kullanıcı izinleri**
------------ | --------- | ----------
Kullanıcılar ve kişiler | Kullanıcıların ve kişilerin tüm genel özelliklerini okuma<br>Konuk davet etme<br>Kendi parolasını değiştirme<br>Kendi cep telefonu numarasını yönetme<br>Kendi fotoğrafını yönetme<br>Kendi yenileme belirteçlerini geçersiz kılma | Kendi özelliklerini okuma<br>Görüntü adı, e-posta, oturum açma adı, fotoğraf, kullanıcı ana adı ve diğer kullanıcıların ve kişilerin kullanıcı türü özelliklerini okuyun<br>Kendi parolasını değiştirme
Gruplar | Güvenlik grubu oluşturma<br>Office 365 grubu oluşturma<br>Grupların tüm özelliklerini okuma<br>Gizli olmayan grup üyeliklerini okuma<br>Birleştirilen grup için gizli Office 365 grup üyeliklerini okuma<br>Kullanıcının sahip olduğu grupların özelliklerini, sahipliğini ve üyeliğini yönetme<br>Sahip olunan gruplara konuk ekleme<br>Dinamik üyelik ayarlarını yönetme<br>Sahip olunan grupları silme<br>Sahip olunan Office 365 gruplarını geri yükleme | Grupların tüm özelliklerini okuma<br>Gizli olmayan grup üyeliklerini okuma<br>Birleştirilen gruplar için gizli Office 365 grup üyeliklerini okuma<br>Sahip olunan grupları yönetme<br>Sahip olunan gruplara konuk ekleme (izin veriliyorsa)<br>Sahip olunan grupları silme<br>Sahip olunan Office 365 gruplarını geri yükleme<br>Üyelik de dahil olmak üzere ait oldukları grupların özelliklerini okuyun.
Uygulamalar | Yeni uygulama kaydetme (oluşturma)<br>Kayıtlı ve kurumsal uygulamaların özelliklerini okuma<br>Sahip olunan uygulamaların uygulama özelliklerini, atamalarını ve kimlik bilgilerini yönetme<br>Kullanıcı için uygulama parolasını oluşturma veya silme<br>Sahip olunan uygulamaları silme<br>Sahip olunan uygulamaları geri yükleme | Kayıtlı ve kurumsal uygulamaların özelliklerini okuma<br>Sahip olunan uygulamaların uygulama özelliklerini, atamalarını ve kimlik bilgilerini yönetme<br>Sahip olunan uygulamaları silme<br>Sahip olunan uygulamaları geri yükleme
Cihazlar | Cihazların tüm özelliklerini okuma<br>Sahip olunan cihazların tüm özelliklerini yönetme<br> | İzin yok<br>Sahip olunan cihazları silme<br>
Dizin | Tüm şirket bilgilerini okuma<br>Tüm etki alanlarını okuma<br>Tüm iş ortağı sözleşmelerini okuma | Görünen ad ve onaylı etki alanlarını okuma
Roller ve Kapsamlar | Tüm yönetim rollerini ve üyelikleri okuma<br>Tüm yönetim birimlerinin özelliklerini ve üyeliklerini okuma | İzin yok 
Abonelikler | Tüm abonelikleri okuma<br>Hizmet Planı Üyesini etkinleştirme | İzin yok
İlkeler | İlkelerin tüm özelliklerini okuma<br>Sahip olunan ilkelerin tüm özelliklerini yönetme | İzin yok

## <a name="to-restrict-the-default-permissions-for-member-users"></a>Üye kullanıcıların varsayılan izinlerini kısıtlama

Üye kullanıcıların varsayılan izinleri aşağıdaki yöntemlerle kısıtlanabilir.

İzin | Ayar açıklaması
---------- | ------------
Kullanıcılar uygulamayı kaydedebilir | Bu seçeneği Hayır olarak ayarlamak, kullanıcıların uygulama kayıtları oluşturmasını engeller. Bu yetenek daha sonra uygulama geliştiricisi rolüne ekleyerek belirli kişilere geri verilebilir.
Kullanıcıların iş veya okul hesabını LinkedIn ile bağlamasına izin verme | Bu seçeneği Hayır olarak ayarlamak, kullanıcıların iş veya okul hesaplarını LinkedIn hesaplarıyla bağlamalarını engeller. Daha fazla bilgi için [LinkedIn hesap bağlantıları veri paylaşımı ve onayı'na](https://docs.microsoft.com/azure/active-directory/users-groups-roles/linkedin-user-consent)bakın.
Güvenlik grubu oluşturma olanağı | Bu seçenek Hayır olarak ayarlanırsa kullanıcılar güvenlik grubu oluşturamaz. Genel yöneticiler ve Kullanıcı yöneticileri yine de güvenlik grupları oluşturabilir. Nasıl yapılacağını öğrenmek için bkz. [Grup ayarlarını yapılandırmak için Azure Active Directory cmdlet'leri](../users-groups-roles/groups-settings-cmdlets.md).
Office 365 grubu oluşturma olanağı | Bu seçenek Hayır olarak ayarlanırsa kullanıcılar Office 365 grubu oluşturamaz. Bu seçeneğin Bazı olarak ayarlanması belirli bir kullanıcı kümesinin Office 365 grubu oluşturmasını sağlar. Genel yöneticiler ve Kullanıcı yöneticileri Office 365 grupları oluşturmaya devam edecektir. Nasıl yapılacağını öğrenmek için bkz. [Grup ayarlarını yapılandırmak için Azure Active Directory cmdlet'leri](../users-groups-roles/groups-settings-cmdlets.md).
Azure AD yönetim portalına erişimi sınırlayın | Bu seçeneği Hayır olarak ayarlamak, yönetici olmayanların Azure AD kaynaklarını okumak ve yönetmek için Azure AD yönetim portalını kullanmasına izin verir. Evet, yönetici olmayan tüm yöneticilerin yönetim portalındaki azure REKLAM verilerine erişmelerini kısıtlar. Unutulmaması gereken önemli: Bu ayar, PowerShell veya Visual Studio gibi diğer istemcileri kullanarak Azure REKLAM verilerine erişimi kısıtlamaz. Belirli bir yönetici olmayan kullanıcıya Azure AD yönetim portalını kullanma olanağı vermek için Evet olarak ayarlandığında, Dizin Okuyucular rolü gibi herhangi bir yönetim rolü atayın. Bu rol, üye kullanıcıların varsayılan olarak sahip olduğu temel dizin bilgilerinin okunmasını sağlar (misafirler ve hizmet ilkeleri bunu yapmaz).
Diğer kullanıcıları okuma olanağı | Bu ayar yalnızca PowerShell ile kullanılabilir. Bu bayrağı $false olarak ayarlamak, yönetici olmayan tüm yöneticilerin dizindeki kullanıcı bilgilerini okumasını engeller. Bu bayrak, Exchange Online gibi diğer Microsoft hizmetlerinde kullanıcı bilgilerinin okunmasını engellemez. Bu ayar özel durumlar içindir ve bu bayrağı $false ayarlamak önerilmez.

## <a name="object-ownership"></a>Nesne sahipliği

### <a name="application-registration-owner-permissions"></a>Uygulama kaydı sahibi izinleri
Uygulamayı kaydeden kullanıcılar otomatik olarak uygulamanın sahibi olarak eklenir. Bu uygulama sahipleri ad ve uygulama isteği izinleri gibi uygulama meta verilerini yönetebilir. Bu kullanıcılar ayrıca SSO yapılandırması ve kullanıcı atamaları gibi kiracıya özgü uygulama yapılandırmalarını da yönetebilir. Sahipler başka kullanıcıları sahip olarak ekleyebilir veya kaldırabilir. Genel Yöneticilerden farklı olarak sahipler yalnızca kendilerine ait uygulamaları yönetebilir.

### <a name="enterprise-application-owner-permissions"></a>Kurumsal uygulama sahibi izinleri
Bir kullanıcı yeni bir kurumsal uygulama eklediğinde, otomatik olarak sahibi olarak eklenir. Bir sahip olarak, SSO yapılandırması, sağlama ve kullanıcı atamaları gibi uygulamanın kiracıya özgü yapılandırmasını yönetebilirler. Sahipler başka kullanıcıları sahip olarak ekleyebilir veya kaldırabilir. Genel Yöneticilerin aksine, sahipleri yalnızca sahip oldukları uygulamaları yönetebilir.

### <a name="group-owner-permissions"></a>Grup sahibi izinleri
Grup oluşturan kullanıcılar otomatik olarak o grubun sahibi olur. Bir sahip olarak, grubun ad gibi özelliklerini yönetebilir ve grup üyeliğini yönetebilirler. Sahipler başka kullanıcıları sahip olarak ekleyebilir veya kaldırabilir. Genel Yöneticiler ve Kullanıcı yöneticilerinin aksine, sahipler yalnızca sahip oldukları grupları yönetebilir. Grup sahibi atamak için bkz. [Grup sahiplerini yönetme](active-directory-accessmanagement-managing-group-owners.md).

### <a name="ownership-permissions"></a>Sahiplik İzinleri
Aşağıdaki tablolar, Azure Etkin Dizin idenine üye kullanıcıların sahip olduğu nesnelerden daha fazla sahip olduğu belirli izinleri açıklar. Kullanıcı, bu izinleri yalnızca sahip oldukları nesnelerüzerinde kullanır.

#### <a name="owned-application-registrations"></a>Sahip olunan başvuru kayıtları
Kullanıcılar, sahip olunan uygulama kayıtlarında aşağıdaki eylemleri gerçekleştirebilir.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.directory/applications/audience/update | Azure Etkin Dizini'nde uygulamaları.hedef kitle özelliğini güncelleştirin. |
| microsoft.directory/applications/authentication/update | Azure Active Directory'deki uygulamalar.kimlik doğrulama özelliğini güncelleştirin. |
| microsoft.directory/applications/basic/update | Azure Etkin Dizini'ndeki uygulamalardaki temel özellikleri güncelleştirin. |
| microsoft.directory/applications/credentials/update | Azure Active Directory'deki applications.credentials özelliğini güncelleştirin. |
| microsoft.directory/applications/delete | Azure Etkin Dizini'ndeki uygulamaları silin. |
| microsoft.directory/applications/owners/update | Azure Active Directory'deki applications.owners özelliğini güncelleştirin. |
| microsoft.directory/applications/permissions/update | Azure Active Directory'deki uygulamalar.izinözelliğini güncelleştirin. |
| microsoft.directory/applications/policies/update | Azure Active Directory'deki applications.policies özelliğini güncelleştirin. |
| microsoft.directory/applications/restore | Azure Etkin Dizini'ndeki uygulamaları geri yükleyin. |

#### <a name="owned-enterprise-applications"></a>Sahip olunan kurumsal uygulamalar
Kullanıcılar, sahip olunan kurumsal uygulamalarda aşağıdaki eylemleri gerçekleştirebilir. Kurumsal uygulama, hizmet ilkesi, bir veya daha fazla uygulama ilkesi ve bazen hizmet ilkesiyle aynı kiracıdaki bir uygulama nesnesi oluşur.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.directory/auditLogs/allProperties/read | Azure Etkin Dizini'ndeki auditLogs'daki tüm özellikleri (ayrıcalıklı özellikler dahil) okuyun. |
| microsoft.directory/policies/basic/update | Azure Etkin Dizini'ndeki ilkelerdeki temel özellikleri güncelleştirin. |
| microsoft.directory/policies/delete | Azure Etkin Dizini'ndeki ilkeleri silin. |
| microsoft.directory/policies/owners/update | İlkeleri güncelleştirin.sahipleri Azure Etkin Dizini'nde. |
| microsoft.directory/servicePrincipals/appRoleAssignedTo/update | HizmetPrincipals.appRoleAssignedTo özelliğini Azure Etkin Dizini'nde güncelleştirin. |
| microsoft.directory/servicePrincipals/appRoleAssignments/update | Azure Active Directory'deki kullanıcıları.appRoleAssignments özelliğini güncelleştirin. |
| microsoft.directory/servicePrincipals/audience/update | HizmetPrincipals.audience özelliğini Azure Etkin Dizini'nde güncelleştirin. |
| microsoft.directory/servicePrincipals/authentication/update | Azure Etkin Dizini'nde hizmetPrincipals.authentication özelliğini güncelleştirin. |
| microsoft.directory/servicePrincipals/basic/update | Azure Etkin Dizini'ndeki hizmet Müdürleri'ndeki temel özellikleri güncelleştirin. |
| microsoft.directory/servicePrincipals/credentials/update | Azure Etkin Dizini'nde servicePrincipals.credentials özelliğini güncelleştirin. |
| microsoft.directory/servicePrincipals/delete | Azure Etkin Dizini'ndeki hizmet Müdürlerini silin. |
| microsoft.directory/servicePrincipals/owners/update | HizmetPrincipals.owners özelliğini Azure Etkin Dizini'nde güncelleştirin. |
| microsoft.directory/servicePrincipals/permissions/update | Azure Etkin Dizini'nde hizmetPrincipals.permissions özelliğini güncelleştirin. |
| microsoft.directory/servicePrincipals/policies/update | Azure Etkin Dizini'nde hizmetPrincipals.policies özelliğini güncelleştirin. |
| microsoft.directory/signInReports/allProperties/read | Azure Active Directory'de signInReports'daki tüm özellikleri (ayrıcalıklı özellikler dahil) okuyun. |

#### <a name="owned-devices"></a>Sahip olunan aygıtlar
Kullanıcılar, sahip olunan aygıtlarda aşağıdaki eylemleri gerçekleştirebilir.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.directory/devices/bitLockerRecoveryKeys/read | Azure Active Directory'de devices.bitLockerRecoveryKeys özelliğini okuyun. |
| microsoft.directory/devices/devre dışı | Azure Etkin Dizini'ndeki aygıtları devre dışı katın. |

#### <a name="owned-groups"></a>Sahip olunan gruplar
Kullanıcılar sahip olunan gruplar da aşağıdaki eylemleri gerçekleştirebilir.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.directory/groups/appRoleAssignments/update | Azure Etkin Dizini'nde grupları.appRoleAssignments özelliğini güncelleştirin. |
| microsoft.directory/groups/basic/update | Azure Etkin Dizini'ndeki gruplardaki temel özellikleri güncelleştirin. |
| microsoft.directory/groups/delete | Azure Etkin Dizini'ndeki grupları silin. |
| microsoft.directory/groups/dynamicMembershipRule/update | Grupları güncelleştirin.dinamikAzure Active Directory'deki dinamikÜyelik Kuralı özelliği. |
| microsoft.directory/groups/members/update | Grup.üyeler özelliğini Azure Etkin Dizini'nde güncelleştirin. |
| microsoft.directory/groups/owners/update | Grup.sahipleri azure Etkin Dizini'nde güncelleştirin. |
| microsoft.directory/groups/geri yükleme | Azure Etkin Dizini'ndeki grupları geri yükleyin. |
| microsoft.directory/groups/settings/update | Azure Etkin Dizini'nde groups.settings özelliğini güncelleştirin. |

## <a name="next-steps"></a>Sonraki adımlar

* Azure AD yöneticisi rollerini nasıl ataysüreceğiniz hakkında daha fazla bilgi edinmek için Azure [Etkin Dizin'de yönetici rollerine kullanıcı atama](active-directory-users-assign-role-azure-portal.md)
* Microsoft Azure'da kaynak erişiminin nasıl denetlendiği konusunda daha fazla bilgi için bkz. [Azure'da kaynak erişimini anlama](../../role-based-access-control/rbac-and-directory-admin-roles.md)
* Azure Active Directory ile Azure aboneliğinizin arasındaki ilişki hakkında bilgi için bkz. [Azure aboneliklerinin Azure Active Directory ile ilişkisi](active-directory-how-subscriptions-associated-directory.md)
* [Kullanıcıları yönetme](add-users-azure-active-directory.md)
