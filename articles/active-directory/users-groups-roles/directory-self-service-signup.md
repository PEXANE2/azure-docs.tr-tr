---
title: E-posta yla doğrulanmış kullanıcılar için self servis kayıt - Azure AD | Microsoft Dokümanlar
description: Azure Etkin Dizin (Azure AD) kiracısında self servis kaydolma kullanma
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: users-groups-roles
ms.topic: article
ms.workload: identity
ms.date: 11/08/2019
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 953837e22cdd3ba8a54d702eac61461739db82d2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74027629"
---
# <a name="what-is-self-service-sign-up-for-azure-active-directory"></a>Azure Active Directory için self servis kayıt nedir?

Bu makalede, bir kuruluşu Azure Etkin Dizini'nde (Azure AD) doldurmak için self servis kaydolma nasıl kullanılacağı açıklanmaktadır. Bir etki alanı adını yönetilmeyen bir Azure REKLAM kuruluşundan devralmak istiyorsanız, [yönetilmeyen bir dizinin yönetici olarak devralmasını](domains-admin-takeover.md)'na bakın.

## <a name="why-use-self-service-sign-up"></a>Neden self servis kayıt kullanıyorsun?
* Müşterileri istedikleri hizmetlere daha hızlı gettola
* Hizmet için e-posta tabanlı teklifler oluşturma
* Kullanıcıların hatırlanması kolay iş e-posta takma adlarını kullanarak kimlik oluşturmalarına olanak tanıyan e-posta tabanlı kayıt akışları oluşturun
* Self servis tarafından oluşturulan Azure REKLAM dizini, diğer hizmetler için kullanılabilecek yönetilen bir dizine dönüştürülebilir

## <a name="terms-and-definitions"></a>Terimler ve tanımlar
* **Self servis kayıt**: Bu yöntem, bir kullanıcının bir bulut hizmetine kaydolması ve e-posta etki alanına bağlı olarak Azure AD'de kendileri için otomatik olarak oluşturulan bir kimliğe sahip olduğu yöntemidir.
* **Yönetilmeyen Azure REKLAM dizini**: Bu, bu kimliğin oluşturulduğu dizinidir. Yönetilmeyen dizin, genel yöneticisi olmayan bir dizindir.
* **E-posta yla doğrulanmış kullanıcı**: Azure AD'deki bir kullanıcı hesabı türüdür. Self servis teklifine kaydolduktan sonra otomatik olarak oluşturulan bir kimliğe sahip olan bir kullanıcı, e-posta yla doğrulanmış kullanıcı olarak bilinir. E-posta yla doğrulanmış bir kullanıcı, creationmethod=EmailVerified ile etiketlenmiş bir dizinin düzenli üyesidir.

## <a name="how-do-i-control-self-service-settings"></a>Self servis ayarlarını nasıl kontrol edebilirim?
Yöneticilerin bugün iki self servis denetimi vardır. Şunları olup olmadığını kontrol edebilirler:

* Kullanıcılar e-posta yoluyla dizine katılabilir
* Kullanıcılar uygulamalar ve hizmetler için kendilerini lisanslayabilir

### <a name="how-can-i-control-these-capabilities"></a>Bu yetenekleri nasıl kontrol edebilirim?
Yönetici, aşağıdaki Azure AD cmdlet Set-MsolCompanySettings parametrelerini kullanarak bu özellikleri yapılandırabilir:

* **AllowEmailVerifiedUsers,** bir kullanıcının bir dizin oluşturup oluşturamayacağını veya dizine katılıp katılamadığını denetler. Bu parametreyi $false olarak ayarlarsanız, e-posta yla doğrulanmış hiçbir kullanıcı dizine katılamaz.
* **AllowAdHocSubscriptions,** kullanıcıların self servis kayıt yapma yeteneğini denetler. Bu parametreyi $false olarak ayarlarsanız, hiçbir kullanıcı self servis kayıt yapamaz.
  
AllowEmailVerifiedUsers ve AllowAdHocSubscriptions yönetilen veya yönetilmeyen bir dizine uygulanabilen dizin genelinde ayarlardır. Aşağıda bir örnek verilmiştir:

* Bir dizin, contoso.com gibi doğrulanmış bir etki alanıyla yönetirsiniz
* Contoso.com ev dizininde zaten var olmayan bir kullanıcıyı davet etmekuserdoesnotexist@contoso.comiçin farklı bir dizinden B2B işbirliği kullanıyorsunuzcontoso.com
* Ev dizini AllowEmailVerifiedUsers açık

Önceki koşullar doğruysa, ev dizininde bir üye kullanıcı oluşturulur ve davet eden dizinde bir B2B konuk kullanıcı oluşturulur.

Akış ve PowerApps deneme kayıtları **AllowAdHocSubscriptions** ayarı tarafından denetlenir. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Mevcut kullanıcılarımın Power BI'ı kullanmaya başlamasını nasıl önleyebilirim?](https://support.office.com/article/Power-BI-in-your-Organization-d7941332-8aec-4e5e-87e8-92073ce73dc5#bkmk_preventjoining)
* [Kuruluşunuzda Flow ile ilgili soru-cevap](https://docs.microsoft.com/flow/organization-q-and-a)

### <a name="how-do-the-controls-work-together"></a>Denetimler birlikte nasıl çalışır?
Bu iki parametre self-servis kayıt üzerinde daha kesin kontrol tanımlamak için birlikte kullanılabilir. Örneğin, aşağıdaki komut kullanıcıların self servis kaydolma işlemleri yapmasına izin verir, ancak yalnızca bu kullanıcıların Azure AD'de zaten bir hesabı varsa (diğer bir deyişle, ilk oluşturulacak e-posta yla doğrulanmış bir hesaba ihtiyaç duyan kullanıcılar self servis kayıt yapamaz):

```powershell
    Set-MsolCompanySettings -AllowEmailVerifiedUsers $false -AllowAdHocSubscriptions $true
```

Aşağıdaki akış şeması, bu parametreler için farklı kombinasyonları ve dizin ve self servis kayıt için ortaya çıkan koşulları açıklar.

![self servis kayıt kontrollerinin akış şeması](./media/directory-self-service-signup/SelfServiceSignUpControls.png)

Daha fazla bilgi ve bu parametrelerin nasıl kullanılacağına ilgili örnekler için [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0)bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure AD'ye özel bir etki alanı adı ekleme](../fundamentals/add-custom-domain.md)
* [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview)
* [Azure PowerShell](/powershell/azure/overview)
* [Azure Cmdlet Başvurusu](/powershell/azure/get-started-azureps)
* [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0)
* [İş inizi veya okul hesabınızı yönetilmeyen bir dizinde kapatın](users-close-account.md)
