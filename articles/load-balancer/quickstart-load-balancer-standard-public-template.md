---
title: 'Hızlı Başlangıç: Standart yük dengeleyici-Azure Resource Manager şablonu oluşturma'
titlesuffix: Azure Load Balancer
description: Bu hızlı başlangıçta, Azure Resource Manager şablonu kullanılarak standart yük dengeleyicinin nasıl oluşturulacağı gösterilmektedir.
services: load-balancer
documentationcenter: na
author: asudbring
manager: twooley
Customer intent: I want to create a Standard load balancer by using an Azure Resource Manager template so that I can load balance internet traffic to VMs.
ms.service: load-balancer
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2019
ms.author: allensu
ms.custom: mvc
ms.openlocfilehash: e1a04cad7fe5c9c95397ffad2faa80c7d657d0f8
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68273805"
---
# <a name="quickstart-create-a-standard-load-balancer-to-load-balance-vms-by-using-an-azure-resource-manager-template"></a>Hızlı Başlangıç: Azure Resource Manager şablonu kullanarak VM 'Lerin yükünü dengelemek için standart yük dengeleyici oluşturma

Yük dengeleme, gelen istekleri birden fazla sanal makineye (VM) yayarak daha yüksek bir kullanılabilirlik ve ölçek düzeyi sağlar. Bu hızlı başlangıçta, VM 'Lerin yük dengelemesi için standart yük dengeleyici oluşturan bir Azure Resource Manager şablonunun nasıl dağıtılacağı gösterilir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-a-standard-load-balancer"></a>Standart yük dengeleyici oluşturma

Standart Load Balancer yalnızca standart bir genel IP adresini destekler. Standart yük dengeleyici oluşturduğunuzda, standart yük dengeleyici için ön uç olarak yapılandırılmış yeni bir standart genel IP adresi de oluşturmanız gerekir.

Standart yük dengeleyici oluşturmak için birçok yöntemi kullanabilirsiniz. Bu hızlı başlangıçta, bir [Kaynak Yöneticisi şablonu](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-load-balancer-standard-create/azuredeploy.json)dağıtmak için Azure PowerShell kullanırsınız. Resource Manager şablonları, çözümünüz için dağıtmanız gereken kaynakları tanımlayan JSON dosyalarıdır.

Azure çözümlerinizi dağıtma ve yönetme ile ilgili kavramları anlamak için bkz. [Azure Resource Manager belgeleri](/azure/azure-resource-manager/). Azure Load Balancer ilgili daha fazla şablon bulmak için bkz. [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Network&pageNumber=1&sort=Popular).

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

   Şablon dağıtımı üç kullanılabilirlik bölgesi oluşturur. Kullanılabilirlik alanları yalnızca [belirli bölgelerde](../availability-zones/az-overview.md)desteklenir. Desteklenen bölgelerden birini kullanın. Emin değilseniz, merkezileştirme girin .

   Kaynak grubu adı, **RG** eklenmiş proje adıdır. Sonraki bölümde kaynak grubu adına ihtiyacınız vardır.

Şablonun dağıtılması yaklaşık 10 dakika sürer.

## <a name="test-the-load-balancer"></a>Yük dengeleyiciyi test etme

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Sol bölmeden **kaynak grupları** ' nı seçin.

1. Önceki bölümde oluşturduğunuz kaynak grubunu seçin. Varsayılan kaynak grubu adı, **RG** eklenmiş proje adıdır.

1. Yük dengeleyiciyi seçin. Varsayılan adı **-lb** eklenen proje adıdır.

1. Genel IP adresinin yalnızca IP adresi kısmını kopyalayın ve ardından tarayıcınızın adres çubuğuna yapıştırın. Tarayıcı Internet Information Services (IIS) Web sunucusunun varsayılan sayfasını görüntüler.

   ![IIS web sunucusu](./media/tutorial-load-balancer-standard-zonal-portal/load-balancer-test.png)

Yük dengeleyicinin trafiği üç VM 'ye dağıtmasını görmek için, istemci makinesinden Web tarayıcınızı yenilemeye zorlayabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyacınız kalmadığında, kaynak grubunu, yük dengeleyiciyi ve tüm ilgili kaynakları silin. Bunu yapmak için Azure portal gidin, yük dengeleyiciyi içeren kaynak grubunu seçin ve **kaynak grubunu sil**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir standart yük dengeleyici oluşturdunuz, bu sanal makineye bağlı VM 'Ler, yük dengeleyici trafik kuralını yapılandırdınız, bir sistem durumu araştırması ve sonra yük dengeleyiciyi test ediyor.

Daha fazla bilgi edinmek için Load Balancer öğreticilerine geçin.

> [!div class="nextstepaction"]
> [Azure Load Balancer öğreticileri](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
 