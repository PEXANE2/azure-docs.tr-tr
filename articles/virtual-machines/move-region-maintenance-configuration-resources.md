---
title: Bakım yapılandırmasıyla ilişkili kaynakları başka bir bölgeye taşıma
description: VM bakım yapılandırmasıyla ilişkili kaynakları başka bir Azure bölgesine nasıl taşıyacağımı öğrenin
services: virtual-machines
author: shants123
ms.service: virtual-machines
ms.topic: article
ms.tgt_pltfrm: vm
ms.date: 03/04/2020
ms.author: shants
ms.openlocfilehash: 3e271e2467b495e79a93ce5eab5edee36e65e619
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78304452"
---
# <a name="move-resources-in-a-maintenance-control-configuration-to-another-region"></a>Bakım Denetimi yapılandırmasındaki kaynakları başka bir bölgeye taşıma

Bakım Denetimi yapılandırmasıyla ilişkili kaynakları farklı bir Azure bölgesine taşımak için bu makaleyi izleyin. Yapılandırmayı birkaç nedenden dolayı taşımak isteyebilirsiniz. Örneğin, yeni bir bölgeden yararlanmak, belirli bir bölgede bulunan özellikleri veya hizmetleri dağıtmak, iç politika ve yönetim gereksinimlerini karşılamak veya kapasite planlamasına yanıt olarak.

Özel bakım yapılandırmalarıyla bakım denetimi, platform güncelleştirmelerinin [Windows](https://docs.microsoft.com/azure/virtual-machines/maintenance-control-cli?toc=/azure/virtual-machines/windows/toc.json&bc=/azure/virtual-machines/windows/breadcrumb/toc.json) ve [Linux](https://docs.microsoft.com/azure/virtual-machines/maintenance-control-cli?toc=%2Fazure%2Fvirtual-machines%2Flinux%2Ftoc.json&bc=%2Fazure%2Fvirtual-machines%2Flinux%2Fbreadcrumb%2Ftoc.json&view=azure-java-stable) VM'lere ve Azure Özel Ana Bilgisayarlara nasıl uygulandığını denetlemenize olanak tanır. Bakım denetimini bölgeler arasında taşımak için birkaç senaryo vardır:

- Yapılandırmanın kendisi değil, bir bakım yapılandırmasıyla ilişkili kaynakları taşımak için bu makaleyi izleyin.
- Bakım denetimi yapılandırmanızı taşımak için, ancak yapılandırmayla ilişkili kaynakları değil, [aşağıdaki yönergeleri](move-region-maintenance-configuration.md)izleyin.
- Hem bakım yapılandırmasını hem de onunla ilişkili kaynakları taşımak için önce [bu yönergeleri](move-region-maintenance-configuration.md)izleyin. Ardından, bu makaledeki yönergeleri izleyin.

## <a name="prerequisites"></a>Ön koşullar

Bakım Denetimi yapılandırmasıyla ilişkili kaynakları taşımaya başlamadan önce:

- Taşımayaptığınız kaynakların başlamadan önce yeni bölgede bulunduğundan emin olun.
- Taşımak istediğiniz Azure VM'leri ve Azure Özel Ana Bilgisayarlarıyla ilişkili Bakım Denetimi yapılandırmalarını doğrulayın. Her kaynağı tek tek denetleyin. Şu anda birden çok kaynak için yapılandırmaları almanın bir yolu yok.
- Bir kaynak için yapılandırmaları alırken:
    - Azure Özel Ana Bilgisayar Kimliği'ni değil, hesap için abonelik kimliğini kullandığınızdan emin olun.
    - CLI: --output tablosu parametresi yalnızca okunabilirlik için kullanılır ve silinebilir veya değiştirilebilir.
    - PowerShell: Biçim-Tablo Adı parametresi yalnızca okunabilirlik için kullanılır ve silinebilir veya değiştirilebilir.
    - PowerShell kullanıyorsanız, ilişkili yapılandırmaları olmayan bir kaynağın yapılandırmalarını listelemeye çalışırsanız hata alırsınız. Hata benzer olacaktır: "İşlem durum ile başarısız oldu: 'Bulunamadı'. Ayrıntılar: 404 İstemci Hatası: url için Bulunamadı".

    
## <a name="prepare-to-move"></a>Hareket etmeye hazırlanın

1. Başlamadan önce, bu değişkenleri tanımlayın. Her biri için bir örnek verdik.

    **Değişken** | **Şey** | **Örnek**
    --- | ---
    $subId | Bakım yapılandırmalarını içeren abonelik için kimlik | "bizim abonelik-ID"
    $rsrcGroupName | Kaynak grubu adı (Azure VM) | "VMResourceGroup"
    $vmName | VM kaynak adı |  "myVM"
    $adhRsrcGroupName |  Kaynak grubu (Özel ana bilgisayarlar) | "HostResourceGroup"
    $adh | Özel ana bilgisayar adı | "myHost"
    $adhParentName | Üst kaynak adı | "HostGroup"
    
2. PowerShell [Get-AZConfigurationAssignment](https://docs.microsoft.com/powershell/module/az.maintenance/Get-AzConfigurationAssignment?view=azps-3.5.0) komutunu kullanarak bakım yapılandırmalarını almak için:

    - Azure Özel Ana Bilgisayarlar için çalıştırın:
        ```
        Get-AzConfigurationAssignment -ResourceGroupName $adhRsrcGroupName -ResourceName $adh -ResourceType hosts -ProviderName Microsoft.Compute -ResourceParentName $adhParentName -ResourceParentType hostGroups | Format-Table Name
        ```

    - Azure VM'ler için çalıştırın:

        ```
        Get-AzConfigurationAssignment -ResourceGroupName $rgName -ResourceName $vmName -ProviderName Microsoft.Compute -ResourceType virtualMachines | Format-Table Name
        ```
3. CLI az bakım atama komutunu kullanarak bakım yapılandırmalarını almak [için:](https://docs.microsoft.com/cli/azure/ext/maintenance/maintenance/assignment?view=azure-cli-latest)

    - Azure'a Özel Ana Bilgisayarlar için:

        ```
        az maintenance assignment list --subscription $subId --resource-group $adhRsrcGroupName --resource-name $adh --resource-type hosts --provider-name Microsoft.Compute --resource-parent-name $adhParentName --resource-parent-type hostGroups --query "[].{HostResourceGroup:resourceGroup,ConfigName:name}" --output table
        ```

    - Azure VM'ler için:

        ```
        az maintenance assignment list --subscription $subId --provider-name Microsoft.Compute --resource-group $rsrcGroupName --resource-name $vmName --resource-type virtualMachines --query "[].{HostResourceGroup:resourceGroup, ConfigName:name}" --output table
        ```


## <a name="move"></a>Taşı 

1. Azure VM'lerini yeni bölgeye taşımak için [aşağıdaki yönergeleri izleyin.](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-migrate?toc=/azure/virtual-machines/windows/toc.json&bc=/azure/virtual-machines/windows/breadcrumb/toc.json)
2. Kaynaklar taşındıktan sonra, bakım yapılandırmalarını taşıyıp taşımadığınıza bağlı olarak bakım yapılandırmalarını uygun şekilde yeni bölgedeki kaynaklara yeniden uygulayın. [PowerShell](../virtual-machines/maintenance-control-powershell.md) veya [CLI'yi](../virtual-machines/maintenance-control-cli.md)kullanarak kaynağa bakım yapılandırması uygulayabilirsiniz.


## <a name="verify-the-move"></a>Hareketi doğrula

Yeni bölgedeki kaynakları doğrulayın ve yeni bölgedeki kaynaklar için ilişkili yapılandırmaları doğrulayın. 

## <a name="clean-up-source-resources"></a>Kaynak kaynaklarını temizleme

Taşımadan sonra, kaynak bölgede taşınan kaynakları silmeyi düşünün.


## <a name="next-steps"></a>Sonraki adımlar

Bakım yapılandırmalarını taşımanız gerekiyorsa [bu yönergeleri](move-region-maintenance-configuration.md) izleyin. 
