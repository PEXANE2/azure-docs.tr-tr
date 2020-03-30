---
title: Bir uygulamayı Azure sanal makine ölçeği kümesine dağıtma
description: Uygulamaları bir ölçek kümesinde Linux ve Windows sanal makine örneklerine nasıl dağıtılayın öğrenin
author: cynthn
tags: azure-resource-manager
ms.assetid: f8892199-f2e2-4b82-988a-28ca8a7fd1eb
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: cynthn
ms.openlocfilehash: 6bc319ea50da4ff6a654b2c9ab09bbe218695533
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76278113"
---
# <a name="deploy-your-application-on-virtual-machine-scale-sets"></a>Uygulamanızı sanal makine ölçek kümelerine dağıtma

Bir ölçek kümesindeki sanal makine (VM) örneklerinde uygulamaları çalıştırmak için önce uygulama bileşenlerini ve gerekli dosyaları yüklemeniz gerekir. Bu makalede, bir ölçek kümesindeki örneğin özel bir VM görüntüsü oluşturmanın veya varolan VM örneklerinde yükleme komut dosyalarını otomatik olarak çalıştırmanın yolları tanıtış. Ayrıca, bir ölçek kümesinde uygulama veya işletim sistemi güncelleştirmelerini nasıl yöneteceklerini de öğrenirsiniz.


## <a name="build-a-custom-vm-image"></a>Özel bir VM görüntüsü oluşturma
Ölçek setinizdeki örnekleri oluşturmak için Azure platform görüntülerinden birini kullandığınızda, ek yazılım yüklenmez veya yapılandırılamaz. Ancak, bu bileşenlerin yüklenmesini otomatikleştirebilirsiniz, ancak bu, VM örneklerini ölçek kümelerinize sağlama süresini de ekler. VM örneklerine çok sayıda yapılandırma değişikliği uygularsanız, bu yapılandırma komut dosyaları ve görevleri ile yönetim yükü vardır.

Yapılandırma yönetimini ve VM sağlama süresini azaltmak için, ölçek kümesinde bir örnek sağlanır sağlanır sağlanır sağlamaz uygulamanızı çalıştırmaya hazır özel bir VM görüntüsü oluşturabilirsiniz. Ölçek kümesine sahip özel bir VM görüntüsünün nasıl oluşturulup kullanılacağı hakkında daha fazla bilgi için aşağıdaki öğreticilere bakın:

- [Azure CLI](tutorial-use-custom-image-cli.md)
- [Azure PowerShell](tutorial-use-custom-image-powershell.md)


## <a name="install-an-app-with-the-custom-script-extension"></a><a name="already-provisioned"></a>Özel Komut Dosyası Uzantısı ile bir uygulama yükleme
Özel Betik Uzantısı, Azure VM’lerinde betik indirir ve yürütür. Bu uzantı dağıtım sonrası yapılandırma, yazılım yükleme veya diğer yapılandırma/yönetim görevleri için kullanışlıdır. Betikler Azure depolama veya GitHub konumlarından indirilebilir ya da Azure portalına uzantı çalışma zamanında iletilebilir. Özel Komut Dosyası Uzantısı içeren bir uygulamanın nasıl yüklenirhakkında daha fazla bilgi için aşağıdaki öğreticilere bakın:

- [Azure CLI](tutorial-install-apps-cli.md)
- [Azure PowerShell](tutorial-install-apps-powershell.md)
- [Azure Resource Manager şablonu](tutorial-install-apps-template.md)


## <a name="install-an-app-to-a-windows-vm-with-powershell-dsc"></a>PowerShell DSC ile windows vm'ye uygulama yükleme
[PowerShell İstenen Durum Yapılandırması (DSC),](/powershell/scripting/dsc/overview/overview) hedef makinelerin yapılandırmasını tanımlayan bir yönetim platformudur. DSC yapılandırmaları, makineye ne yüklenir ve ana bilgisayar nasıl yapılandırılabildiğini tanımlar. Yerel Yapılandırma Yöneticisi (LCM) altyapısı, istenen eylemleri itilen yapılandırmalara göre işleyen her hedef düğümde çalışır.

PowerShell DSC uzantısı, VM örneklerini PowerShell ile ayarlanmış bir ölçekte özelleştirmenize olanak tanır. Aşağıdaki örnek:

- VM örneklerine GitHub'dan bir DSC paketi indirmetalimatı verir -*https://github.com/Azure-Samples/compute-automation-configurations/raw/master/dsc.zip*
- Bir yükleme komut dosyası çalıştırmak için uzantısı ayarlar -`configure-http.ps1`
- [Get-AzVmss](/powershell/module/az.compute/get-azvmss) ile bir ölçek kümesi hakkında bilgi alır
- [Uzantıyı Update-AzVmss](/powershell/module/az.compute/update-azvmss) ile VM örneklerine uygular

DSC uzantısı *myResourceGroup*adlı kaynak grubunda *myScaleSet* VM örnekleri uygulanır. Kendi adlarınızı aşağıdaki gibi girin:

```powershell
# Define the script for your Desired Configuration to download and run
$dscConfig = @{
  "wmfVersion" = "latest";
  "configuration" = @{
    "url" = "https://github.com/Azure-Samples/compute-automation-configurations/raw/master/dsc.zip";
    "script" = "configure-http.ps1";
    "function" = "WebsiteTest";
  };
}

# Get information about the scale set
$vmss = Get-AzVmss `
                -ResourceGroupName "myResourceGroup" `
                -VMScaleSetName "myScaleSet"

# Add the Desired State Configuration extension to install IIS and configure basic website
$vmss = Add-AzVmssExtension `
    -VirtualMachineScaleSet $vmss `
    -Publisher Microsoft.Powershell `
    -Type DSC `
    -TypeHandlerVersion 2.24 `
    -Name "DSC" `
    -Setting $dscConfig

# Update the scale set and apply the Desired State Configuration extension to the VM instances
Update-AzVmss `
    -ResourceGroupName "myResourceGroup" `
    -Name "myScaleSet"  `
    -VirtualMachineScaleSet $vmss
```

Ölçek kümenizdeki yükseltme ilkesi *el ile*gerçekse, VM örneklerinizi [Update-AzVmssInstance](/powershell/module/az.compute/update-azvmssinstance)ile güncelleyin. Bu cmdlet, güncelleştirilmiş ölçek kümesi yapılandırmasını VM örneklerine uygular ve uygulamanızı yükler.


## <a name="install-an-app-to-a-linux-vm-with-cloud-init"></a>Bulut init ile bir Linux VM'ye uygulama yükleme
[Cloud-init](https://cloudinit.readthedocs.io/en/latest/index.html), Linux VM’sini ilk kez önyüklendiğinde özelleştirmeyi sağlayan, sık kullanılan bir yaklaşımdır. cloud-init’i paket yükleme, dosyalara yazma ve kullanıcılar ile güvenliği yapılandırma işlemleri için kullanabilirsiniz. cloud-init önyükleme işlemi sırasında çalışırken, yapılandırmanıza uygulayabileceğiniz ek adım veya gerekli aracı yoktur.

Cloud-init, dağıtımlar arasında da çalışır. Örneğin, bir paket yüklemek için **apt-get install** veya **yum install** kullanmazsınız. Bunun yerine, yüklenecek paketlerin listesini tanımlayabilirsiniz. Cloud-init, seçtiğiniz dağıtım için yerel paket yönetim aracını otomatik olarak kullanır.

Örnek bir *cloud-init.txt* dosyası da dahil olmak üzere daha fazla bilgi [için Azure VM'leri özelleştirmek için bulut init'i kullan'a](../virtual-machines/linux/using-cloud-init.md)bakın.

Bir ölçek kümesi oluşturmak ve bir bulut-init `--custom-data` dosyası kullanmak için, [az vmss komutu oluşturmak](/cli/azure/vmss) parametre ekleyin ve bir bulut-init dosyasının adını belirtin. Aşağıdaki örnek, *myResourceGroup'ta* *myScaleSet* adında bir ölçek kümesi oluşturur ve VM örneklerini *cloud-init.txt*adlı bir dosyayla yapılandırır. Kendi adlarınızı aşağıdaki gibi girin:

```azurecli
az vmss create \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --custom-data cloud-init.txt \
  --admin-username azureuser \
  --generate-ssh-keys
```


### <a name="install-applications-with-os-updates"></a>İşletim sistemi güncelleştirmeleriyle uygulamaları yükleme
Yeni işletim sistemi sürümleri kullanılabilir olduğunda, yeni bir özel görüntü kullanabilir veya oluşturabilir ve [işletim sistemi yükseltmelerini](virtual-machine-scale-sets-upgrade-scale-set.md) bir ölçek kümesine dağıtabilirsiniz. Her VM örneği, belirttiğiniz en son görüntüye yükseltilir. Yükseltmeyi gerçekleştirirken uygulamanızın otomatik olarak kullanılabilir olması için uygulama önceden yüklenmiş, Özel Komut Dosyası Uzantısı veya PowerShell DSC ile özel bir görüntü kullanabilirsiniz. Sürüm uyumluluğu sorunları olmadığından emin olmak için bu işlemi gerçekleştirirken uygulama bakımı için planlamanız gerekebilir.

Uygulama önceden yüklenmiş özel bir VM görüntüsü kullanıyorsanız, yeni görüntüler oluşturmak ve ölçek kümesi boyunca işletim sistemi yükseltmeleri dağıtmak için uygulama güncelleştirmelerini dağıtım ardışık alanıyla tümleştirebilirsiniz. Bu yaklaşım, ardışık yapının en son uygulama yapılarını toplamasına, vm görüntüsü oluşturmasına ve doğrulamasına ve ardından ölçek kümesindeki VM örneklerini yükseltmesine olanak tanır. Özel VM görüntüleri arasında uygulama güncelleştirmeleri oluşturan ve dağıtan bir dağıtım ardışık hattını çalıştırmak için [bir Packer görüntüsü oluşturabilir ve Azure DevOps Hizmetleri ile dağıtabilir](/azure/devops/pipelines/apps/cd/azure/deploy-azure-scaleset)veya [Spinnaker](https://www.spinnaker.io/) veya [Jenkins](https://jenkins.io/)gibi başka bir platform kullanabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar
Uygulamaları ölçek kümelerinize oluşturup dağıtırken, [Ölçek Tasarımı genel görünümünü](virtual-machine-scale-sets-design-overview.md)gözden geçirebilirsiniz. Ölçek kümenizi nasıl yönetkullanacağınız hakkında daha fazla bilgi [için, ölçek kümenizi yönetmek için PowerShell'i kullanın'](virtual-machine-scale-sets-windows-manage.md)a bakın.
