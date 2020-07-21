---
title: Bakım yapılandırmasını başka bir Azure bölgesine taşıma
description: Bir VM bakım yapılandırmasını başka bir Azure bölgesine taşımayı öğrenin
author: shants123
ms.service: virtual-machines
ms.topic: how-to
ms.tgt_pltfrm: vm
ms.date: 03/04/2020
ms.author: shants
ms.openlocfilehash: 4cff7eb4a69005f2e74747b6e58447f100c69b60
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86501611"
---
# <a name="move-a-maintenance-control-configuration-to-another-region"></a>Bakım denetim yapılandırmasını başka bir bölgeye taşıma

Bakım denetim yapılandırmasını farklı bir Azure bölgesine taşımak için bu makaleyi izleyin. Bir yapılandırmayı bir kaç nedenden dolayı taşımak isteyebilirsiniz. Örneğin, yeni bir bölgeden yararlanmak için, belirli bir bölgedeki özellikleri veya hizmetleri dağıtmak, iç ilke ve idare gereksinimlerini karşılamak veya kapasite planlamasına yanıt vermek için.

Bakım denetimi, özelleştirilmiş bakım yapılandırmalarında, platform güncelleştirmelerinin [Windows](./maintenance-control-cli.md?toc=/azure/virtual-machines/windows/toc.json&bc=/azure/virtual-machines/windows/breadcrumb/toc.json) ve [Linux](./maintenance-control-cli.md?toc=%2Fazure%2Fvirtual-machines%2Flinux%2Ftoc.json&bc=%2Fazure%2Fvirtual-machines%2Flinux%2Fbreadcrumb%2Ftoc.json&view=azure-java-stable) VM 'Lerine ve Azure adanmış konaklarına nasıl uygulandığını denetlemenize olanak tanır. Bakım denetimini bölgeler arasında taşımak için birkaç senaryo vardır:

- Bakım denetimi yapılandırmanızı taşımak, ancak yapılandırmayla ilişkili kaynakları değil, bu makaledeki yönergeleri izleyin.
- Bir bakım yapılandırmasıyla ilişkili kaynakları taşımak, ancak yapılandırmanın kendisi için [Bu yönergeleri](move-region-maintenance-configuration-resources.md)izleyin.
- Hem bakım yapılandırmasını hem de onunla ilişkili kaynakları taşımak için, önce bu makaledeki yönergeleri izleyin. Ardından, [Bu yönergeleri](move-region-maintenance-configuration-resources.md)izleyin.

## <a name="prerequisites"></a>Önkoşullar

Bir bakım denetim yapılandırmasını taşımaya başlamadan önce:

- Bakım yapılandırması, Azure VM 'Leri veya Azure adanmış Konakları ile ilişkilendirilir. Başlamadan önce VM/konak kaynaklarının yeni bölgede mevcut olduğundan emin olun.
- Tanıtan 
    - Mevcut bakım denetimi yapılandırması.
    - Mevcut yapılandırmaların Şu anda bulunduğu kaynak grupları. 
    - Yeni bölgeye taşıdıktan sonra yapılandırmaların ekleneceği kaynak grupları. 
    - Taşımak istediğiniz bakım yapılandırmasıyla ilişkili kaynaklar.
    - Yeni bölgedeki kaynakların, geçerli bakım yapılandırmasıyla ilişkili olanlarla aynı olup olmadığını kontrol edin. Konfigürasyonlar, yeni bölgede eskilerle aynı ada sahip olabilir, ancak bu gerekli değildir.

## <a name="prepare-and-move"></a>Hazırlama ve taşıma 

1. Her abonelikteki tüm bakım yapılandırmasını alın. Bunu yapmak için CLı [az Maintenance Configuration List](/cli/azure/ext/maintenance/maintenance/configuration?view=azure-cli-latest#ext-maintenance-az-maintenance-configuration-list) komutunu çalıştırın ve $SUBID abonelik Kimliğinizle değiştirin.

    ```
    az maintenance configuration list --subscription $subId --query "[*].{Name:name, Location:location, ResGroup:resourceGroup}" --output table
    ```
2. Abonelik içindeki yapılandırma kayıtlarının döndürülen tablo listesini gözden geçirin. Bir örneği aşağıda verilmiştir. Listeniz, belirli ortamınızın değerlerini içerir.

    **Ad** | **Konum** | **Kaynak grubu**
    --- | --- | ---
    Bakımı atla | eastus2 | yapılandırma-kaynak grubu
    IgniteDemoConfig | eastus2 | yapılandırma-kaynak grubu
    defaultMaintenanceConfiguration-eastus | eastus | test-yapılandırma
    

3. Listenizi başvuru için kaydedin. Konfigürasyonları taşırken, her şeyin taşındığını doğrulamanıza yardımcı olur.
4. Başvuru olarak, her yapılandırma/kaynak grubunu yeni bölgedeki yeni kaynak grubuyla eşleyin.
5. Yeni bölgede [PowerShell](../virtual-machines/maintenance-control-powershell.md#create-a-maintenance-configuration)veya [CLI](../virtual-machines/maintenance-control-cli.md#create-a-maintenance-configuration)kullanarak yeni bakım yapılandırması oluşturun.
6. Konfigürasyonları, [PowerShell](../virtual-machines/maintenance-control-powershell.md#assign-the-configuration)veya [CLI](../virtual-machines/maintenance-control-cli.md#assign-the-configuration)kullanarak yeni bölgedeki kaynaklarla ilişkilendirin.


## <a name="verify-the-move"></a>Taşımayı doğrulama

Yapılandırma taşıdıktan sonra, yeni bölgedeki yapılandırma ve kaynakları, hazırladığınız Tablo listesiyle karşılaştırın.


## <a name="clean-up-source-resources"></a>Kaynak kaynaklarını Temizleme

Taşıma işleminden sonra, kaynak bölgedeki, [PowerShell](../virtual-machines/maintenance-control-powershell.md#remove-a-maintenance-configuration)veya [CLI](../virtual-machines/maintenance-control-cli.md#delete-a-maintenance-configuration)'de taşınan bakım yapılandırmalarının silinmesini göz önünde bulundurun.


## <a name="next-steps"></a>Sonraki adımlar

Bakım yapılandırmasıyla ilişkili kaynakları taşımanız gerekiyorsa [Bu yönergeleri](move-region-maintenance-configuration-resources.md) izleyin. 
