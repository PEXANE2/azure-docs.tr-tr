---
title: 'Hızlı başlangıç: Load Balancer oluşturma-Azure şablonu'
titleSuffix: Azure Load Balancer
description: Bu hızlı başlangıçta, Azure Resource Manager şablonu kullanılarak yük dengeleyici oluşturma gösterilmektedir.
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
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80474606"
---
# <a name="quickstart-create-a-load-balancer-to-load-balance-vms-by-using-azure-resource-manager-template"></a>Hızlı başlangıç: Azure Resource Manager şablonu kullanarak VM 'Lerin yükünü dengelemek için Load Balancer oluşturma

Yük dengeleme, gelen istekleri birden fazla sanal makineye (VM) yayarak daha yüksek bir kullanılabilirlik ve ölçek düzeyi sağlar. Bu hızlı başlangıçta, VM 'Lerin yük dengelemesi için standart yük dengeleyici oluşturan bir Azure Resource Manager şablonunun nasıl dağıtılacağı gösterilir. Kaynak Yöneticisi şablonu kullanmak diğer dağıtım yöntemleriyle karşılaştırıldığında daha az adım sürer.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-a-load-balancer"></a>Yük Dengeleyici oluşturma

Load Balancer ve genel IP SKU 'Larının eşleşmesi gerekir. Bir Standart Load Balancer oluşturduğunuzda, standart yük dengeleyici için ön uç olarak yapılandırılmış yeni bir standart genel IP adresi de oluşturmanız gerekir. Temel bir Load Balancer oluşturmak istiyorsanız [Bu şablonu](https://azure.microsoft.com/resources/templates/201-2-vms-loadbalancer-natrules/)kullanın. Microsoft, üretim iş yükleri için standart SKU kullanmayı önerir.

### <a name="review-the-template"></a>Şablonu gözden geçirin

Bu hızlı başlangıçta kullanılan şablon [Azure hızlı başlangıç şablonlarından](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-load-balancer-standard-create/azuredeploy.json).

:::code language="json" source="~/quickstart-templates/101-load-balancer-standard-create/azuredeploy.json" range="1-324" highlight="58-122":::

Şablonda birden çok Azure kaynağı tanımlanmış:

- [**Microsoft. Network/loadBalancers**](/azure/templates/microsoft.network/loadbalancers)
- [**Microsoft. Network/Publicıpaddresses**](/azure/templates/microsoft.network/publicipaddresses): yük dengeleyici için ve üç sanal makinenin her biri için.
- [**Microsoft. Network/networkSecurityGroups**](/azure/templates/microsoft.network/networksecuritygroups)
- [**Microsoft. Network/virtualNetworks**](/azure/templates/microsoft.network/virtualnetworks)
- [**Microsoft. COMPUTE/virutalMachines**](/azure/templates/microsoft.compute/virtualmachines) (3)
- [**Microsoft. Network/NetworkInterfaces**](/azure/templates/microsoft.network/networkinterfaces) (3)
- [**Microsoft. COMPUTE/virtualMachine/Extensions**](/azure/templates/microsoft.compute/virtualmachines/extensions) (3 BT): IIS 'i ve Web sayfalarını yapılandırmak için kullanın

Azure Load Balancer ilgili daha fazla şablon bulmak için bkz. [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Network&pageNumber=1&sort=Popular).

### <a name="deploy-the-template"></a>Şablonu dağıtma

1. Azure Cloud Shell açmak için aşağıdaki kod bloğundan **deneyin** ' i seçin ve ardından Azure 'da oturum açmak için yönergeleri izleyin.

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

   İstemi konsolundan görene kadar bekleyin.

1. PowerShell betiğini kopyalamak için önceki kod bloğundan **Kopyala** ' yı seçin.

1. Kabuk konsol bölmesine sağ tıklayın ve ardından **Yapıştır**' ı seçin.

1. Değerleri girin.

   Şablon dağıtımı üç kullanılabilirlik bölgesi oluşturur. Kullanılabilirlik alanları yalnızca [belirli bölgelerde](../availability-zones/az-overview.md)desteklenir. Desteklenen bölgelerden birini kullanın. Emin değilseniz, **merkezileştirme**girin.

   Kaynak grubu adı, **RG** eklenmiş proje adıdır. Sonraki bölümde kaynak grubu adına ihtiyacınız vardır.

Şablonun dağıtılması yaklaşık 10 dakika sürer. Tamamlandığında, çıkış şuna benzerdir:

![Azure Standart Load Balancer Kaynak Yöneticisi şablonu PowerShell dağıtım çıkışı](./media/quickstart-load-balancer-standard-public-template/azure-standard-load-balancer-resource-manager-template-powershell-output.png)

Azure PowerShell, şablonu dağıtmak için kullanılır. Azure PowerShell ek olarak, Azure portal, Azure CLı ve REST API de kullanabilirsiniz. Diğer dağıtım yöntemlerini öğrenmek için bkz. [şablonları dağıtma](../azure-resource-manager/templates/deploy-portal.md).

## <a name="test-the-load-balancer"></a>Yük dengeleyiciyi test etme

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Sol bölmeden **kaynak grupları** ' nı seçin.

1. Önceki bölümde oluşturduğunuz kaynak grubunu seçin. Varsayılan kaynak grubu adı, **RG** eklenmiş proje adıdır.

1. Yük dengeleyiciyi seçin. Varsayılan adı **-lb** eklenen proje adıdır.

1. Genel IP adresinin yalnızca IP adresi kısmını kopyalayın ve ardından tarayıcınızın adres çubuğuna yapıştırın.

   ![Azure Standart yük dengeleyici Kaynak Yöneticisi şablonu genel IP 'si](./media/quickstart-load-balancer-standard-public-template/azure-standard-load-balancer-resource-manager-template-deployment-public-ip.png)

    Tarayıcı Internet Information Services (IIS) Web sunucusunun varsayılan sayfasını görüntüler.

   ![IIS web sunucusu](./media/quickstart-load-balancer-standard-public-template/load-balancer-test-web-page.png)

Yük dengeleyicinin trafiği üç VM 'ye dağıtmasını görmek için, istemci makinesinden Web tarayıcınızı yenilemeye zorlayabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyacınız kalmadığında, kaynak grubunu, yük dengeleyiciyi ve tüm ilgili kaynakları silin. Bunu yapmak için Azure portal gidin, yük dengeleyiciyi içeren kaynak grubunu seçin ve **kaynak grubunu sil**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir standart yük dengeleyici oluşturdunuz, bu sanal makineye bağlı VM 'Ler, yük dengeleyici trafik kuralını yapılandırdınız, bir sistem durumu araştırması ve sonra yük dengeleyiciyi test ediyor.

Daha fazla bilgi edinmek için Load Balancer öğreticilerine geçin.

> [!div class="nextstepaction"]
> [Azure Load Balancer öğreticileri](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
