---
title: Bakım yapılandırmasını başka bir Azure bölgesine taşıma
description: VM bakım yapılandırmasını başka bir Azure bölgesine nasıl taşıyacağım
services: virtual-machines
author: shants123
ms.service: virtual-machines
ms.topic: article
ms.tgt_pltfrm: vm
ms.date: 03/04/2020
ms.author: shants
ms.openlocfilehash: fe03bead238d3fb7bda3ee685bd5587c3e0dbc58
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78304465"
---
# <a name="move-a-maintenance-control-configuration-to-another-region"></a>Bakım Denetimi yapılandırmasını başka bir bölgeye taşıma

Bakım Denetimi yapılandırmasını farklı bir Azure bölgesine taşımak için bu makaleyi izleyin. Yapılandırmayı birkaç nedenden dolayı taşımak isteyebilirsiniz. Örneğin, yeni bir bölgeden yararlanmak, belirli bir bölgede bulunan özellikleri veya hizmetleri dağıtmak, iç politika ve yönetim gereksinimlerini karşılamak veya kapasite planlamasına yanıt olarak.

Özel bakım yapılandırmalarıyla bakım denetimi, platform güncelleştirmelerinin [Windows](https://docs.microsoft.com/azure/virtual-machines/maintenance-control-cli?toc=/azure/virtual-machines/windows/toc.json&bc=/azure/virtual-machines/windows/breadcrumb/toc.json) ve [Linux](https://docs.microsoft.com/azure/virtual-machines/maintenance-control-cli?toc=%2Fazure%2Fvirtual-machines%2Flinux%2Ftoc.json&bc=%2Fazure%2Fvirtual-machines%2Flinux%2Fbreadcrumb%2Ftoc.json&view=azure-java-stable) VM'lere ve Azure Özel Ana Bilgisayarlara nasıl uygulandığını denetlemenize olanak tanır. Bakım denetimini bölgeler arasında taşımak için birkaç senaryo vardır:

- Bakım denetimi yapılandırmanızı taşımak için, ancak yapılandırmayla ilişkili kaynakları değil, bu makaledeki yönergeleri izleyin.
- Bir bakım yapılandırmasıyla ilişkili, ancak yapılandırmanın kendisini değil, kaynakları taşımak için [bu yönergeleri](move-region-maintenance-configuration-resources.md)izleyin.
- Hem bakım yapılandırmasını hem de bununla ilişkili kaynakları taşımak için önce bu makaledeki yönergeleri izleyin. Daha sonra, [bu yönergeleri](move-region-maintenance-configuration-resources.md)izleyin.

## <a name="prerequisites"></a>Ön koşullar

Bir bakım denetimi yapılandırması taşımaya başlamadan önce:

- Bakım yapılandırmaları Azure VM'leri veya Azure Özel Ana Bilgisayarları ile ilişkilidir. Başlamadan önce VM/ana bilgisayar kaynaklarının yeni bölgede bulunduğundan emin olun.
- Tanımlamak: 
    - Varolan bakım kontrol yapılandırmaları.
    - Varolan yapılandırmaların şu anda bulunduğu kaynak grupları. 
    - Yapılandırmaların yeni bölgeye taşındıktan sonra eklenecek kaynak grupları. 
    - Taşımak istediğiniz bakım yapılandırmasıyla ilişkili kaynaklar.
    - Yeni bölgedeki kaynakların geçerli bakım yapılandırmalarıyla ilişkili kaynaklarla aynı olup olmadığını denetleyin. Yapılandırmalar, yeni bölgede eskiyle aynı adlara sahip olabilir, ancak bu gerekli değildir.

## <a name="prepare-and-move"></a>Hazırlanın ve hareket edin 

1. Her abonelikteki tüm bakım yapılandırmalarını alın. Bunu yapmak için CLI [az bakım yapılandırma listesi](https://docs.microsoft.com/cli/azure/ext/maintenance/maintenance/configuration?view=azure-cli-latest#ext-maintenance-az-maintenance-configuration-list) komutunu çalıştırın ve $subId abonelik kimliğinizle değiştirin.

    ```
    az maintenance configuration list --subscription $subId --query "[*].{Name:name, Location:location, ResGroup:resourceGroup}" --output table
    ```
2. Abonelik içindeki yapılandırma kayıtlarının döndürülen tablo listesini gözden geçirin. Bir örneği aşağıda verilmiştir. Listeniz, özel ortamınız için değerler içerir.

    **Adı** | **Konum** | **Kaynak grubu**
    --- | --- | ---
    Bakımı Atla | eastus2 | yapılandırma-kaynak grubu
    IgniteDemoConfig | eastus2 | yapılandırma-kaynak grubu
    defaultMaintenanceConfiguration-eastus | eastus | test yapılandırması
    

3. Listenizi başvuru için kaydedin. Yapılandırmaları taşırken, her şeyin taşındığını doğrulamanıza yardımcı olur.
4. Başvuru olarak, her yapılandırma/kaynak grubunu yeni bölgedeki yeni kaynak grubuna eşle.
5. [PowerShell](../virtual-machines/maintenance-control-powershell.md#create-a-maintenance-configuration)veya [CLI](../virtual-machines/maintenance-control-cli.md#create-a-maintenance-configuration)kullanarak yeni bölgede yeni bakım yapılandırmaları oluşturun.
6. [PowerShell](../virtual-machines/maintenance-control-powershell.md#assign-the-configuration)veya [CLI](../virtual-machines/maintenance-control-cli.md#assign-the-configuration)kullanarak yapılandırmaları yeni bölgedeki kaynaklarla ilişkilendirin.


## <a name="verify-the-move"></a>Hareketi doğrula

Yapılandırmaları hareket ettirdikten sonra, yeni bölgedeki yapılandırmaları ve kaynakları hazırladığınız tablo listesiyle karşılaştırın.


## <a name="clean-up-source-resources"></a>Kaynak kaynaklarını temizleme

Taşımadan sonra, kaynak bölge, [PowerShell](../virtual-machines/maintenance-control-powershell.md#remove-a-maintenance-configuration)veya [CLI'deki](../virtual-machines/maintenance-control-cli.md#delete-a-maintenance-configuration)taşınan bakım yapılandırmalarını silmeyi düşünün.


## <a name="next-steps"></a>Sonraki adımlar

Bakım yapılandırmalarıyla ilişkili kaynakları taşımanız [gerekiyorsa, bu yönergeleri](move-region-maintenance-configuration-resources.md) izleyin. 
