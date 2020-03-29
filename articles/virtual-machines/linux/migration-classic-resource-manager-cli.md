---
title: Azure CLI kullanarak VM'leri Kaynak Yöneticisine geçirin
description: Bu makale, Azure CLI'yi kullanarak kaynakların klasikten Azure Kaynak Yöneticisi'ne platform destekli geçişinden geçer
author: tanmaygore
manager: vashan
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.topic: article
ms.date: 02/06/2020
ms.author: tagore
ms.openlocfilehash: c41292a05e5c857cd0b1c120784a400f2f5410ab
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78945350"
---
# <a name="migrate-iaas-resources-from-classic-to-azure-resource-manager-by-using-azure-cli"></a>Azure CLI kullanarak IaaS kaynaklarını klasik modelden Azure Resource Manager’a geçirme

> [!IMPORTANT]
> Bugün, IaaS VM'lerinin yaklaşık %90'ı [Azure Kaynak Yöneticisi](https://azure.microsoft.com/features/resource-manager/)kullanıyor. 28 Şubat 2020 itibariyle, klasik VM'ler amortismana kaldırılmıştır ve 1 Mart 2023'te tamamen emekliye ayırılacaktır. Bu amortisman ve sizi [nasıl etkilediği](https://docs.microsoft.com/azure/virtual-machines/classic-vm-deprecation#how-does-this-affect-me)hakkında [daha fazla bilgi edinin.]( https://aka.ms/classicvmretirement)

Bu adımlar, altyapıyı hizmet (IaaS) kaynaklarını klasik dağıtım modelinden Azure Kaynak Yöneticisi dağıtım modeline geçirmek için Azure komut satırı arabirimi (CLI) komutlarını nasıl kullanacağınızı gösterir. Makale, [Azure klasik CLI](../../cli-install-nodejs.md)gerektirir. Azure CLI yalnızca Azure Kaynak Yöneticisi kaynakları için geçerli olduğundan, bu geçiş için kullanılamaz.

> [!NOTE]
> Burada açıklanan tüm işlemler idempotent vardır. Desteklenmeyen bir özellik veya yapılandırma hatası dışında bir sorununuz varsa, işlemi hazırlamayı, iptal etmeyi veya işleme yi yeniden denemenizi öneririz. Platform daha sonra eylemi yeniden dener.
> 
> 

<br>
Burada, bir geçiş işlemi sırasında adımların yürütülmesi gereken adımları belirlemek için bir akış şeması

![Geçiş adımlarını gösteren ekran görüntüsü](../windows/media/migration-classic-resource-manager/migration-flow.png)

## <a name="step-1-prepare-for-migration"></a>Adım 1: Geçiş için hazırlanın
IaaS kaynaklarını klasikten Kaynak Yöneticisi'ne değerlendirirken önerdiğimiz en iyi birkaç uygulama şunlardır:

* [Desteklenmeyen yapılandırmalar veya özellikler listesini](../windows/migration-classic-resource-manager-overview.md)okuyun. Desteklenmeyen yapılandırmalar veya özellikler kullanan sanal makineleriniz varsa, özellik/yapılandırma desteğinin duyurulmasını beklemenizi öneririz. Alternatif olarak, gereksinimlerinize uygunsa geçişi etkinleştirmek için bu özelliği kaldırabilir veya bu yapılandırmanın dışına taşıyabilirsiniz.
* Bugün altyapınızı ve uygulamalarınızı dağıtan otomatik komut dosyalarınız varsa, geçiş için bu komut dosyalarını kullanarak benzer bir test kurulumu oluşturmaya çalışın. Alternatif olarak, Azure portalını kullanarak örnek ortamlar ayarlayabilirsiniz.

> [!IMPORTANT]
> Uygulama Ağ Geçitleri şu anda klasikten Kaynak Yöneticisi'ne geçiş için desteklenmez. Uygulama ağ geçidi olan klasik bir sanal ağı geçirmek için, ağı taşımak için bir hazırlama işlemi çalıştırmadan önce ağ geçidini kaldırın. Geçişi tamamladıktan sonra, Azure Kaynak Yöneticisi'ndeki ağ geçidini yeniden bağlayın. 
>
>Başka bir abonelikteki ExpressRoute devrelerine bağlanan ExpressRoute ağ geçitleri otomatik olarak geçirilemez. Bu gibi durumlarda, ExpressRoute ağ geçidini kaldırın, sanal ağı geçirin ve ağ geçidini yeniden oluşturun. Daha fazla bilgi için lütfen [klasikten Kaynak Yöneticisi dağıtım modeline geçiş ExpressRoute devrelerine ve ilişkili sanal ağlara](../../expressroute/expressroute-migration-classic-resource-manager.md) bakın.
> 
> 

## <a name="step-2-set-your-subscription-and-register-the-provider"></a>Adım 2: Aboneliğinizi ayarlayın ve sağlayıcıyı kaydettirin
Geçiş senaryoları için, ortamınızı hem klasik hem de Kaynak Yöneticisi için ayarlamanız gerekir. [Azure CLI'yi yükleyin](../../cli-install-nodejs.md) ve [aboneliğinizi seçin.](/cli/azure/authenticate-azure-cli)

Hesabınızda oturum açın.

    azure login

Aşağıdaki komutu kullanarak Azure aboneliğini seçin.

    azure account set "<azure-subscription-name>"

> [!NOTE]
> Kayıt tek seferlik bir adımdır, ancak geçiş girişiminde bulunmadan önce bir kez yapılması gerekir. Kaydolmadan aşağıdaki hata iletisini görürsünüz 
> 
> *BadRequest : Abonelik geçiş için kayıtlı değildir.* 
> 
> 

Aşağıdaki komutu kullanarak geçiş kaynak sağlayıcısına kaydolun. Bazı durumlarda, bu komutun zaman ları dolduğunu unutmayın. Ancak, kayıt başarılı olacaktır.

    azure provider register Microsoft.ClassicInfrastructureMigrate

Lütfen kaydın bitmesi için beş dakika bekleyin. Aşağıdaki komutu kullanarak onay durumunu kontrol edebilirsiniz. Devam etmeden önce `Registered` RegistrationState'in olduğundan emin olun.

    azure provider show Microsoft.ClassicInfrastructureMigrate

Şimdi CLI'yi `asm` moduna geçin.

    azure config mode asm

## <a name="step-3-make-sure-you-have-enough-azure-resource-manager-virtual-machine-vcpus-in-the-azure-region-of-your-current-deployment-or-vnet"></a>Adım 3: Geçerli dağıtımınızın veya VNET'inizin Azure bölgesinde yeterli Azure Kaynak Yöneticisi Sanal Makine vCPU'nuz olduğundan emin olun
Bu adım için `arm` moda geçmeniz gerekir. Bunu aşağıdaki komutla yapın.

```
azure config mode arm
```

Azure Kaynak Yöneticisi'nde mevcut vCPUs sayısını denetlemek için aşağıdaki CLI komutunu kullanabilirsiniz. vCPU kotaları hakkında daha fazla bilgi edinmek için [Sınırlar ve Azure Kaynak Yöneticisi'ne](../../azure-resource-manager/management/azure-subscription-service-limits.md#managing-limits)bakın.

```
azure vm list-usage -l "<Your VNET or Deployment's Azure region"
```

Bu adımı doğrulamayı bitirdikten `asm` sonra, moda geri dönebilirsiniz.

    azure config mode asm


## <a name="step-4-option-1---migrate-virtual-machines-in-a-cloud-service"></a>Adım 4: Seçenek 1 - Sanal makineleri bulut hizmetinde geçirin
Aşağıdaki komutu kullanarak bulut hizmetlerinin listesini alın ve sonra geçirmek istediğiniz bulut hizmetini seçin. Bulut hizmetindeki VM'ler sanal ağdaysa veya web/işçi rolleri varsa bir hata iletisi alırsınız.

    azure service list

Bulut hizmetinin dağıtım adını ayrıntılı çıktıdan almak için aşağıdaki komutu çalıştırın. Çoğu durumda, dağıtım adı bulut hizmeti adı ile aynıdır.

    azure service show <serviceName> -vv

İlk olarak, bulut hizmetini aşağıdaki komutları kullanarak geçirip geçirebileceğinizi doğrulayın:

```shell
azure service deployment validate-migration <serviceName> <deploymentName> new "" "" ""
```

Bulut hizmetindeki sanal makineleri geçiş için hazırlayın. Aralarından seçim yapabileceğiniz iki seçeneğiniz var.

VM'leri platform tarafından oluşturulan bir sanal ağa geçirmek istiyorsanız, aşağıdaki komutu kullanın.

    azure service deployment prepare-migration <serviceName> <deploymentName> new "" "" ""

Kaynak Yöneticisi dağıtım modelinde varolan bir sanal ağa geçiş yapmak istiyorsanız, aşağıdaki komutu kullanın.

    azure service deployment prepare-migration <serviceName> <deploymentName> existing <destinationVNETResourceGroupName> <subnetName> <vnetName>

Hazırlama işlemi başarılı olduktan sonra, VM'lerin geçiş durumunu elde etmek ve `Prepared` bunların durumda olduğundan emin olmak için ayrıntılı çıktıya bakabilirsiniz.

    azure vm show <vmName> -vv

CLI veya Azure portalını kullanarak hazırlanan kaynakların yapılandırmasını denetleyin. Geçiş için hazır değilseniz ve eski duruma dönmek istiyorsanız, aşağıdaki komutu kullanın.

    azure service deployment abort-migration <serviceName> <deploymentName>

Hazırlanan yapılandırma iyi görünüyorsa, aşağıdaki komutu kullanarak ileriye taşıyabilir ve kaynakları alabilirsiniz.

    azure service deployment commit-migration <serviceName> <deploymentName>



## <a name="step-4-option-2----migrate-virtual-machines-in-a-virtual-network"></a>Adım 4: Seçenek 2 - Sanal ağda sanal makineleri geçirin
Geçirmek istediğiniz sanal ağı seçin. Sanal ağ desteklenmeyen yapılandırmaları olan web/işçi rolleri veya VM'ler içeriyorsa, bir doğrulama hatası iletisi alırsınız.

Aşağıdaki komutu kullanarak abonelikteki tüm sanal ağları elde edin.

    azure network vnet list

Çıkış şuna benzer olacaktır:

![Tüm sanal ağ adı vurgulanan komut satırının ekran görüntüsü.](../media/virtual-machines-linux-cli-migration-classic-resource-manager/vnet.png)

Yukarıdaki örnekte, **virtualNetworkName** tüm adı **"Grup classicubuntu16 classicubuntu16"** dır.

İlk olarak, aşağıdaki komutu kullanarak sanal ağı geçirebiliyorsanız doğrulayın:

```shell
azure network vnet validate-migration <virtualNetworkName>
```

Aşağıdaki komutu kullanarak geçiş için seçtiğiniz sanal ağı hazırlayın.

    azure network vnet prepare-migration <virtualNetworkName>

CLI veya Azure portalını kullanarak hazırlanan sanal makinelerin yapılandırmasını kontrol edin. Geçiş için hazır değilseniz ve eski duruma dönmek istiyorsanız, aşağıdaki komutu kullanın.

    azure network vnet abort-migration <virtualNetworkName>

Hazırlanan yapılandırma iyi görünüyorsa, aşağıdaki komutu kullanarak ileriye taşıyabilir ve kaynakları alabilirsiniz.

    azure network vnet commit-migration <virtualNetworkName>

## <a name="step-5-migrate-a-storage-account"></a>Adım 5: Depolama hesabını geçirme
Sanal makineleri geçirmeyi bitirdikten sonra, depolama hesabını geçirmenizi öneririz.

Aşağıdaki komutu kullanarak depolama hesabını geçiş için hazırlama

    azure storage account prepare-migration <storageAccountName>

CLI veya Azure portalını kullanarak hazırlanan depolama hesabı yapılandırmasını denetleyin. Geçiş için hazır değilseniz ve eski duruma dönmek istiyorsanız, aşağıdaki komutu kullanın.

    azure storage account abort-migration <storageAccountName>

Hazırlanan yapılandırma iyi görünüyorsa, aşağıdaki komutu kullanarak ileriye taşıyabilir ve kaynakları alabilirsiniz.

    azure storage account commit-migration <storageAccountName>

## <a name="next-steps"></a>Sonraki adımlar

* [IaaS kaynaklarının klasikten Azure Kaynak Yöneticisi'ne platform destekli geçişine genel bakış](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Klasik modelden Azure Resource Manager’a platform destekli geçişe ayrıntılı teknik bakış](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [IaaS kaynaklarının Klasik’ten Azure Resource Manager’a geçişini planlama](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [IaaS kaynaklarını klasikten Azure Kaynak Yöneticisi'ne geçirmek için PowerShell'i kullanın](../windows/migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [IaaS kaynaklarının klasikten Azure Kaynak Yöneticisi'ne geçişine yardımcı olmak için topluluk araçları](../windows/migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [En sık karşılaşılan geçiş hatalarını gözden geçirme](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [IaaS kaynaklarını klasikten Azure Kaynak Yöneticisi'ne geçirme yle ilgili en sık sorulan soruları gözden geçirin](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
