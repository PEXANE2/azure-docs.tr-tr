---
title: Iletişim hizmetlerinde Yönetilen kimlikler kullanma
titleSuffix: An Azure Communication Services quickstart
description: Yönetilen kimlikler, Azure Iletişim Hizmetleri 'ne Azure VM 'Leri, işlev uygulamaları ve diğer kaynaklarda çalışan uygulamalardan erişim yetkisi verir.
services: azure-communication-services
author: peiliu
ms.service: azure-communication-services
ms.topic: how-to
ms.date: 2/24/2021
ms.author: peiliu
ms.reviewer: mikben
ms.openlocfilehash: 0d25e5dc97c700daf6655ecd270bfda469a9d353
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101657663"
---
# <a name="use-managed-identities"></a>Yönetilen kimlikleri kullanma
Yönetilen kimlikleri kullanarak Azure Iletişim Hizmetleri ile çalışmaya başlayın. Iletişim Hizmetleri kimliği ve SMS istemci kitaplıkları, [Azure kaynakları için yönetilen kimliklerle](../../active-directory/managed-identities-azure-resources/overview.md)Azure Active Directory (Azure AD) kimlik doğrulamasını destekler.

Bu hızlı başlangıçta, yönetilen kimlikleri destekleyen bir Azure ortamından kimlik ve SMS istemci kitaplıklarına erişim yetkisi verme işlemleri gösterilir. Ayrıca, kodunuzun bir geliştirme ortamında nasıl test edileceğini açıklar.

## <a name="prerequisites"></a>Önkoşullar

 - Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free)
 - Etkin bir Iletişim Hizmetleri kaynağı ve bağlantı dizesi. [Iletişim Hizmetleri kaynağı oluşturun](./create-communication-resource.md?pivots=platform-azp&tabs=windows).

## <a name="setting-up"></a>Ayarlanıyor

### <a name="enable-managed-identities-on-a-virtual-machine-or-app-service"></a>Bir sanal makine veya App Service üzerinde yönetilen kimlikleri etkinleştirme

Yönetilen kimlikler, yetkilendirmekte olduğunuz Azure kaynaklarında etkinleştirilmelidir. Azure kaynakları için yönetilen kimliklerin nasıl etkinleştireceğinizi öğrenmek için şu makalelerden birine bakın:

- [Azure portalındaki](../../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md)
- [Azure PowerShell](../../active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm.md)
- [Azure CLI](../../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md)
- [Azure Resource Manager şablonu](../../active-directory/managed-identities-azure-resources/qs-configure-template-windows-vm.md)
- [Azure Resource Manager istemci kitaplıkları](../../active-directory/managed-identities-azure-resources/qs-configure-sdk-windows-vm.md)
- [Uygulama Hizmetleri](../../app-service/overview-managed-identity.md)

#### <a name="assign-azure-roles-with-the-azure-portal"></a>Azure rollerini Azure portal atama

1. Azure portalına gidin.
1. Azure Iletişim hizmeti kaynağına gidin.
1. Access Control (ıAM) menüsünde > + Add-> rol ataması Ekle ' ye gidin.
1. "Katkıda bulunan" rolünü seçin (Bu, desteklenen tek roldür).
1. "Kullanıcı tarafından atanan yönetilen kimlik" (veya "sistem tarafından atanmış bir kimlik") seçeneğini belirleyin ve istediğiniz kimliği seçin. Seçiminizi kaydedin.

![Yönetilen kimlik rolü](media/managed-identity-assign-role.png)

#### <a name="assign-azure-roles-with-powershell"></a>PowerShell ile Azure rolleri atama

PowerShell kullanarak rol ve izinler atamak için bkz. [Azure PowerShell kullanarak Azure rol atamaları ekleme veya kaldırma](../../../articles/role-based-access-control/role-assignments-powershell.md)

::: zone pivot="programming-language-csharp"
[!INCLUDE [.NET](./includes/managed-identity-net.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [Java](./includes/managed-identity-java.md)]
::: zone-end

::: zone pivot="programming-language-javascript"
[!INCLUDE [JavaScript](./includes/managed-identity-js.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [Python](./includes/managed-identity-python.md)]
::: zone-end