---
title: Temel ilke hizmet yönetimi için MFA gerektir (Önizleme)-Azure Active Directory
description: Azure Resource Manager için MFA gerektirme koşullu erişim ilkesi
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 05/16/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 06b3a3f12b2955ae97c43b0caf6a4ac13c9b770a
ms.sourcegitcommit: c79aa93d87d4db04ecc4e3eb68a75b349448cd17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71086743"
---
# <a name="baseline-policy-require-mfa-for-service-management-preview"></a>Temel ilke: Hizmet yönetimi için MFA gerektir (Önizleme)

Kuruluşunuzda çeşitli Azure Hizmetleri kullanıyor olabilirsiniz. Bu hizmetler Azure Resource Manager API aracılığıyla yönetilebilir:

* Azure portal
* Azure PowerShell
* Azure CLI

Hizmetlerinizi yönetmek için Azure Resource Manager kullanmak, yüksek ayrıcalıklı bir işlemdir. Azure Resource Manager, hizmet ayarları ve abonelik faturalandırması gibi kiracı genelinde yapılandırmaların üzerinde değişiklik yapabilir. Tek faktörlü kimlik doğrulaması, kimlik avı ve parola spreyi gibi çeşitli saldırılara açıktır. Bu nedenle, erişime izin vermeden önce çok faktörlü kimlik doğrulaması gerektirerek Azure Resource Manager ve güncelleştirme yapılandırmalarına erişmek isteyen kullanıcıların kimliğini doğrulamak önemlidir.

**Hizmet yönetimi IÇIN MFA gerektir** , Azure portal, Azure PowerShell veya Azure CLI 'ye erişen tüm KULLANıCıLAR için MFA gerektiren bir [temel ilkedir](concept-baseline-protection.md) . Bu ilke, yönetici olduklarından bağımsız olarak Azure Resource Manager erişen tüm kullanıcılara uygulanır.

Bu ilke bir kiracıda etkinleştirildikten sonra, Azure yönetim kaynakları 'nda oturum açan tüm kullanıcılara, çok faktörlü kimlik doğrulamasıyla karşılaşacaktır. Kullanıcı MFA için kayıtlı değilse, kullanıcının devam edebilmesi için Microsoft Authenticator uygulamasını kullanarak kaydetmesi gerekecektir.

[Azure PowerShell](https://docs.microsoft.com/powershell/azure/authenticate-azureps)'i kullanarak etkileşimli oturum açma gerçekleştirmek için [Connect-azaccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount) cmdlet 'ini kullanın.

```PowerShell
Connect-AzAccount
```

Bu cmdlet çalıştırıldığında bir belirteç dizesi sunar. Oturum açmak için, bu dizeyi kopyalayın ve tarayıcıya yapıştırın [https://microsoft.com/devicelogin](https://microsoft.com/devicelogin) . Azure’a bağlanmak için PowerShell oturumunuzun kimliği doğrulanır.

[Azure CLI](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest)kullanarak etkileşimli oturum açma gerçekleştirmek için [az Login](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest#az-login) komutunu çalıştırın.

```azurecli
az login
```

CLI varsayılan tarayıcınızı açabiliyorsa, tarayıcıyı açar ve oturum açma sayfasını yükler. Aksi takdirde, tarayıcınızda gezindikten [https://aka.ms/devicelogin](https://aka.ms/devicelogin) sonra bir yetkilendirme kodu girmek için bir tarayıcı sayfası açmanız ve komut satırındaki yönergeleri izlemeniz gerekir. Daha sonra, tarayıcıda hesap kimlik bilgilerinizle oturum açın.

## <a name="deployment-considerations"></a>Dağıtma konuları

**Hizmet yönetimi IÇIN MFA gerektir** ilkesi tüm Azure Resource Manager kullanıcılara uygulanır.

## <a name="enable-the-baseline-policy"></a>Temel ilkeyi etkinleştirme

İlke **temel ilkesi: Hizmet yönetimi için MFA (Önizleme)** önceden yapılandırılmış olarak gelir ve Azure Portal 'de koşullu erişim dikey penceresine gittiğinizde en üstte görünür.

Bu ilkeyi etkinleştirmek ve yöneticilerinizi korumak için:

1. **Azure Portal** genel yönetici, güvenlik yöneticisi veya koşullu erişim Yöneticisi olarak oturum açın.
1. **Koşullu erişimi** **Azure Active Directory** > için gidin.
1. İlke listesinde temel ilke ' yi seçin **: Hizmet yönetimi için MFA gerektir (Önizleme)** .
1. İlkeyi **ilkeyi hemen kullanacak** **şekilde ayarlayın** .
1. **Kaydet**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz.

* [Koşullu erişim temel koruma ilkeleri](concept-baseline-protection.md)
* [Kimlik altyapınızı güvenli hale getirmenin beş adımı](../../security/fundamentals/steps-secure-identity.md)
* [Azure Active Directory Koşullu erişim nedir?](overview.md)
