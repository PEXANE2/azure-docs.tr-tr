---
title: Oturum açma için Azure VM'de yönetilen kimlikleri kullanma - Azure AD
description: Komut dosyası istemci oturum açma ve kaynak erişimi için Azure kaynakları hizmeti ilkesi için Azure VM yönetilen kimlikler kullanmak için adım adım talimatlar ve örnekler.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/01/2017
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 34f4dc749c0254b5aa4e9ff018d2a869832de3f0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74547380"
---
# <a name="how-to-use-managed-identities-for-azure-resources-on-an-azure-vm-for-sign-in"></a>Oturum açma için Azure VM'de Azure kaynakları için yönetilen kimlikler nasıl kullanılır? 

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]  
Bu makalede, Azure kaynakları hizmeti ilkesi için yönetilen kimlikleri kullanmak için PowerShell ve CLI komut dosyası örnekleri ve hata işleme gibi önemli konularda kılavuz lar sağlanmaktadır.

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

Bu makalede Azure PowerShell veya Azure CLI örneklerini kullanmayı planlıyorsanız, [Azure PowerShell](/powershell/azure/install-az-ps) veya [Azure CLI'nin](https://docs.microsoft.com/cli/azure/install-azure-cli)en son sürümünü yüklediğinizden emin olun. 

> [!IMPORTANT]
> - Bu makaledeki tüm örnek komut dosyası, komut satırı istemcisinin Azure kaynakları için yönetilen kimliklerle yönetilen bir VM üzerinde çalıştığını varsayar. VM'inize uzaktan bağlanmak için Azure portalındaki VM "Bağlan" özelliğini kullanın. VM'de Azure kaynakları için yönetilen kimlikleri etkinleştirme yle ilgili ayrıntılar için, Azure portalını veya varyant makalelerinden birini (PowerShell, CLI, şablon veya Azure SDK kullanarak) [kullanarak VM'deki Azure kaynakları için yönetilen kimlikleri yapılandır'](qs-configure-portal-windows-vm.md)a bakın. 
> - Kaynak erişimi sırasında hataları önlemek için, VM'nin yönetilen kimliğine, VM'de Azure Kaynak Yöneticisi işlemlerine izin vermek için uygun kapsamda (VM veya daha yüksek) en az "Reader" erişimi verilmelidir. Ayrıntılar [için Azure portalını kullanarak bir kaynağa Azure kaynakları erişimi için yönetilen kimlikler atayın.](howto-assign-access-portal.md)

## <a name="overview"></a>Genel Bakış

Azure kaynakları için yönetilen kimlikler, VM'deki [Azure kaynakları için yönetilen kimlikleri etkinleştirerek oluşturulan](overview.md#how-does-the-managed-identities-for-azure-resources-work) bir hizmet temel [nesnesi](../develop/developer-glossary.md#service-principal-object) sağlar. Hizmet ilkesine Azure kaynaklarına erişim verilebilir ve oturum açma ve kaynak erişimi için komut dosyası/komut satırı istemcileri tarafından kimlik olarak kullanılabilir. Geleneksel olarak, güvenli kaynaklara kendi kimliği altında erişmek için bir komut dosyası istemcisinin şunları içmesi gerekir:  

   - gizli/web istemcisi uygulaması olarak Azure AD'ye kaydolabilir ve onaylanabilir
   - uygulamanın kimlik bilgilerini kullanarak hizmet sorumlusunun altında oturum açın (büyük olasılıkla komut dosyasına gömülü)

Azure kaynakları için yönetilen kimliklerle, Azure kaynakları hizmeti ilkesinin yönetilen kimlikleri altında oturum açabileceğinden komut dosya istemcinizin artık ikisini de yapması gerekmez. 

## <a name="azure-cli"></a>Azure CLI

Aşağıdaki komut dosyası nasıl gösterin:

1. Azure kaynakları hizmeti ilkesi için VM'nin yönetilen kimliği altında Azure AD'de oturum açın  
2. Azure Kaynak Yöneticisi'ni arayın ve VM'nin hizmet sorumlusu kimliğini alın. CLI, sizin için jeton edinimi/kullanımını otomatik olarak yönetmeyi halledin. Sanal makine adınızı `<VM-NAME>`yerine'  

   ```azurecli
   az login --identity
   
   spID=$(az resource list -n <VM-NAME> --query [*].identity.principalId --out tsv)
   echo The managed identity for Azure resources service principal ID is $spID
   ```

## <a name="azure-powershell"></a>Azure PowerShell

Aşağıdaki komut dosyası nasıl gösterin:

1. Azure kaynakları hizmeti ilkesi için VM'nin yönetilen kimliği altında Azure AD'de oturum açın  
2. VM hakkında bilgi almak için Azure Kaynak Yöneticisi cmdlet'i arayın. PowerShell, jeton kullanımını sizin için otomatik olarak yönetmeyi halledin.  

   ```azurepowershell
   Add-AzAccount -identity

   # Call Azure Resource Manager to get the service principal ID for the VM's managed identity for Azure resources. 
   $vmInfoPs = Get-AzVM -ResourceGroupName <RESOURCE-GROUP> -Name <VM-NAME>
   $spID = $vmInfoPs.Identity.PrincipalId
   echo "The managed identity for Azure resources service principal ID is $spID"
   ```

## <a name="resource-ids-for-azure-services"></a>Azure hizmetleri için kaynak it'leri

Azure AD'yi destekleyen ve Azure kaynakları ve ilgili kaynak kimlikleri için yönetilen kimliklerle sınanmış kaynakların listesi için [Azure AD kimlik doğrulamasını destekleyen Azure hizmetlerine](services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication) bakın.

## <a name="error-handling-guidance"></a>Hata işleme kılavuzu 

Aşağıdaki gibi yanıtlar, Azure kaynakları için VM'nin yönetilen kimliğinin doğru şekilde yapılandırılmadığını gösterebilir:

- PowerShell: *Invoke-WebRequest : Uzak sunucuya bağlanamıyor*
- CLI: *MSI: 'HTTPConnectionPool(host='localhost', port=50342) hatası `http://localhost:50342/oauth2/token` ile bir belirteç almak için başarısız oldu* 

Bu hatalardan birini alırsanız, [Azure portalındaki](https://portal.azure.com) Azure VM'ye dönün ve:

- **Kimlik** sayfasına gidin ve **atanan Sistemin** "Evet" olarak ayarlı olduğundan emin olun.
- **Uzantılar** sayfasına gidin ve Başarıyla dağıtılan Azure kaynakları uzantısı **(Ocak 2019'da amortisman için planlanan)** için yönetilen kimliklerin olduğundan emin olun.

Ya yanlışsa, kaynağınızdaki Azure kaynakları için yönetilen kimlikleri yeniden dağıtmanız veya dağıtım hatasını gidermeniz gerekebilir. VM yapılandırması ile ilgili yardıma ihtiyacınız [varsa, Azure portalını kullanarak Bir VM'de Azure kaynakları için Yönetilen kimlikleri Yapılandır'](qs-configure-portal-windows-vm.md) a bakın.

## <a name="next-steps"></a>Sonraki adımlar

- Azure VM'de Azure kaynakları için yönetilen kimlikleri etkinleştirmek [için PowerShell'i kullanarak Azure VM'de Azure kaynakları için yönetilen kimlikleri yapılandırma](qs-configure-powershell-windows-vm.md)veya Azure [CLI kullanarak Azure kaynakları için yönetilen kimlikleri yapılandırma](qs-configure-cli-windows-vm.md)






