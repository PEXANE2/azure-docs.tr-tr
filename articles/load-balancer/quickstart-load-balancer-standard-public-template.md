---
title: 'Hızlı başlangıç: Yük Dengeleyicisi Oluşturma - Azure şablonu'
titleSuffix: Azure Load Balancer
description: Bu hızlı başlangıç, Azure Kaynak Yöneticisi şablonu kullanılarak nasıl bir yük dengeleyicisi oluşturulacak gösteriş gösterir.
services: load-balancer
documentationcenter: na
author: asudbring
manager: twooley
Customer intent: I want to create a load balancer by using an Azure Resource Manager template so that I can load balance internet traffic to VMs.
ms.service: load-balancer
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/26/2020
ms.author: allensu
ms.custom: mvc,subject-armqs
ms.openlocfilehash: 175c5a36c873d16d50d5192a489133a01018e335
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80474606"
---
# <a name="quickstart-create-a-load-balancer-to-load-balance-vms-by-using-azure-resource-manager-template"></a>Hızlı başlangıç: Azure Kaynak Yöneticisi şablonu kullanarak bakiye VM'lerini yüklemek için Yük Dengeleyicisi oluşturma

Yük dengeleme, gelen istekleri birden fazla sanal makineye (VM) yayarak daha yüksek bir kullanılabilirlik ve ölçek düzeyi sağlar. Bu hızlı başlangıç, bakiye VM'leri yüklemek için Standart yük dengeleyicisi oluşturan bir Azure Kaynak Yöneticisi şablonuna nasıl dağıtılacağınızda size gösterir. Kaynak Yöneticisi şablonu kullanmak, diğer dağıtım yöntemleriyle karşılaştırarak daha az adım alır.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="create-a-load-balancer"></a>Yük Dengeleyici oluşturma

Yük Dengeleyici ve Genel IP SUs'lar eşleşmelidir. Standart Yük Dengeleyicisi oluşturduğunuzda, Standart yük dengeleyicisinin ön uç olarak yapılandırılan yeni bir Standart Genel IP adresi de oluşturmanız gerekir. Temel Yük Dengeleyicisi oluşturmak istiyorsanız, [bu şablonu](https://azure.microsoft.com/resources/templates/201-2-vms-loadbalancer-natrules/)kullanın. Microsoft, üretim iş yükleri için Standart SKU kullanılmasını önerir.

### <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlatmada kullanılan şablon [Azure Quickstart Şablonları'ndan](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-load-balancer-standard-create/azuredeploy.json)dır.

:::code language="json" source="~/quickstart-templates/101-load-balancer-standard-create/azuredeploy.json" range="1-324" highlight="58-122":::

Şablonda birden çok Azure kaynağı tanımlanmıştır:

- [**Microsoft.Network/loadBalancers**](/azure/templates/microsoft.network/loadbalancers)
- [**Microsoft.Network/publicIPAddresses**](/azure/templates/microsoft.network/publicipaddresses): yük dengeleyicisi ve üç sanal makinenin her biri için.
- [**Microsoft.Network/networkSecurityGroups**](/azure/templates/microsoft.network/networksecuritygroups)
- [**Microsoft.Network/virtualNetworks**](/azure/templates/microsoft.network/virtualnetworks)
- [**Microsoft.Compute/virutalMachines**](/azure/templates/microsoft.compute/virtualmachines) (3 tanesi)
- [**Microsoft.Network/networkInterfaces**](/azure/templates/microsoft.network/networkinterfaces) (3 tanesi)
- [**Microsoft.Compute/virtualMachine/extensions**](/azure/templates/microsoft.compute/virtualmachines/extensions) (3 tanesi): IIS'yi ve web sayfalarını yapılandırmak için kullanın

Azure Yük Bakiyesi ile ilgili daha fazla şablon bulmak için [Azure Hızlı Başlatma Şablonları'na](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Network&pageNumber=1&sort=Popular)bakın.

### <a name="deploy-the-template"></a>Şablonu dağıtma

1. Azure Bulut Kabuğu'nu açmak için aşağıdaki kod bloğundan **deneyin'i** seçin ve ardından Azure'da oturum açmak için yönergeleri izleyin.

   ```azurepowershell-interactive
   $projectName = Read-Host -Prompt "Enter a project name with 12 or less letters or numbers that is used to generate Azure resource names"
   $location = Read-Host -Prompt "Enter the location (i.e. centralus)"
   $adminUserName = Read-Host -Prompt "Enter the virtual machine administrator account name"
   $adminPassword = Read-Host -Prompt "Enter the virtual machine administrator password" -AsSecureString

   $resourceGroupName = "${projectName}rg"
   $templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-load-balancer-standard-create/azuredeploy.json"

   New-AzResourceGroup -Name $resourceGroupName -Location $location
   New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -projectName $projectName -location $location -adminUsername $adminUsername -adminPassword $adminPassword

   Write-Host "Press [ENTER] to continue."
   ```

   Konsoldan gelen istemi görene kadar bekleyin.

1. PowerShell komut dosyasını kopyalamak için önceki kod bloğundan **Kopyala'yı** seçin.

1. Kabuk konsol bölmesine sağ tıklayın ve ardından **Yapıştır'ı**seçin.

1. Değerleri girin.

   Şablon dağıtımı üç kullanılabilirlik bölgesi oluşturur. Kullanılabilirlik bölgeleri yalnızca [belirli bölgelerde](../availability-zones/az-overview.md)desteklenir. Desteklenen bölgelerden birini kullanın. Emin değilseniz, **centralus**girin.

   Kaynak grubu adı **rg** eklenen proje adıdır. Bir sonraki bölümde kaynak grubu adı gerekir.

Şablonun dağıtılması yaklaşık 10 dakika sürer. Tamamlandığında, çıktı aşağıdakilere benzer:

![Azure Standart Yük Dengeleyici Kaynak Yöneticisi şablonu PowerShell dağıtım çıktısı](./media/quickstart-load-balancer-standard-public-template/azure-standard-load-balancer-resource-manager-template-powershell-output.png)

Azure PowerShell şablonu dağıtmak için kullanılır. Azure PowerShell'e ek olarak Azure portalı, Azure CLI ve REST API'yi de kullanabilirsiniz. Diğer dağıtım yöntemlerini öğrenmek için [şablonları dağıt'a](../azure-resource-manager/templates/deploy-portal.md)bakın.

## <a name="test-the-load-balancer"></a>Yük dengeleyiciyi test etme

1. [Azure portalında](https://portal.azure.com)oturum açın.

1. Sol bölmeden **Kaynak gruplarını** seçin.

1. Önceki bölümde oluşturduğunuz kaynak grubunu seçin. Varsayılan kaynak grubu adı **rg** eklenen proje adıdır.

1. Yük dengeleyicisini seçin. Varsayılan adı **-lb** ekli proje adıdır.

1. Herkese açık IP adresinin yalnızca IP adresi kısmını kopyalayın ve tarayıcınızın adres çubuğuna yapıştırın.

   ![Azure standart yük dengeleyici Kaynak Yöneticisi şablonu genel IP](./media/quickstart-load-balancer-standard-public-template/azure-standard-load-balancer-resource-manager-template-deployment-public-ip.png)

    Tarayıcı, Internet Information Services (IIS) web sunucusunun varsayılan sayfasını görüntüler.

   ![IIS web sunucusu](./media/quickstart-load-balancer-standard-public-template/load-balancer-test-web-page.png)

Yük bakiyesi dağıtım trafiğini üç VM'ye de dağıtmak için, istemci makineden web tarayıcınızın yenilenmesini zorlayabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık bunlara ihtiyacınız olmadığında, kaynak grubunu, yük bakiyesini ve ilgili tüm kaynakları silin. Bunu yapmak için Azure portalına gidin, yük bakiyesini içeren kaynak grubunu seçin ve ardından **kaynak grubunu sil'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, standart bir yük dengeleyicisi oluşturdunuz, ona VM'ler iliştirdin, yük dengeleyici trafik kuralını yapılandırdın, bir sağlık sondası yaptınız ve sonra yük dengeleyicisini test ettin.

Daha fazla bilgi edinmek için Yük Dengeleyici öğreticilerine devam edin.

> [!div class="nextstepaction"]
> [Azure Load Balancer öğreticileri](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
