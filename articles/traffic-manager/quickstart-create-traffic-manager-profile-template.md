---
title: 'Hızlı başlangıç: Azure Resource Manager şablonu kullanarak Traffic Manager oluşturma (ARM şablonu)'
description: Bu hızlı başlangıç makalesinde, Azure Resource Manager şablonu (ARM şablonu) kullanılarak bir Azure Traffic Manager profilinin nasıl oluşturulacağı açıklanmaktadır.
services: traffic-manager
author: duongau
ms.service: traffic-manager
ms.topic: quickstart
ms.custom: subject-armqs
ms.author: duau
ms.date: 09/01/2020
ms.openlocfilehash: dbdb6a255fdf0214103a0011f25b0a6d25014e69
ms.sourcegitcommit: 58d3b3314df4ba3cabd4d4a6016b22fa5264f05a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89299159"
---
# <a name="quickstart-create-a-traffic-manager-profile-using-an-arm-template"></a>Hızlı başlangıç: ARM şablonunu kullanarak Traffic Manager profili oluşturma

Bu hızlı başlangıçta, performans yönlendirme yöntemini kullanarak dış uç noktalara sahip bir Traffic Manager profili oluşturmak için bir Azure Resource Manager şablonunun (ARM şablonu) nasıl kullanılacağı açıklanmaktadır.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Ortamınız önkoşulları karşılıyorsa ve ARM şablonlarını kullanma hakkında bilginiz varsa, **Azure’a dağıtma** düğmesini seçin. Şablon Azure portalda açılır.

[![Azure’a dağıtma](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-traffic-manager-external-endpoint%2Fazuredeploy.json)

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlangıçta kullanılan şablon [Azure Hızlı Başlangıç Şablonlarından](https://azure.microsoft.com/resources/templates/101-traffic-manager-external-endpoint) alınmıştır.

:::code language="json" source="~/quickstart-templates/101-traffic-manager-external-endpoint/azuredeploy.json":::

Şablonda bir Azure kaynağı tanımlanmıştır:

* [**Microsoft. Network/trafficManagerProfiles**](/azure/templates/microsoft.network/trafficmanagerprofiles)

Azure Traffic Manager ile ilgili daha fazla şablon bulmak için bkz. [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Network&pageNumber=1&sort=Popular).

## <a name="deploy-the-template"></a>Şablonu dağıtma

1. Azure Cloud Shell açmak için aşağıdaki kod bloğundan **deneyin** ' i seçin ve ardından Azure 'da oturum açmak için yönergeleri izleyin. 

    ```azurepowershell-interactive
    $projectName = Read-Host -Prompt "Enter a project name that is used for generating resource names"
    $location = Read-Host -Prompt "Enter the location (i.e. centralus)"
    $templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-traffic-manager-external-endpoint/azuredeploy.json"

    $resourceGroupName = "${projectName}rg"

    New-AzResourceGroup -Name $resourceGroupName -Location "$location"
    New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri

    Read-Host -Prompt "Press [ENTER] to continue ..."
    ```

    İstemi konsolundan görene kadar bekleyin.

1. PowerShell betiğini kopyalamak için önceki kod bloğundan **Kopyala** ' yı seçin.

1. Kabuk konsol bölmesine sağ tıklayın ve ardından **Yapıştır**' ı seçin.

1. Değerleri girin.

    Şablon dağıtımı iki dış uç nokta içeren bir profil oluşturur. **Endpoint1** , **Kuzey Avrupa**konumu ile *w<span>ww. Microsoft</span>. com* ' un hedef uç noktasını kullanır. **Endpoint2** , **Orta Güney ABD**konumu ile *d<span>OCS. Microsoft</span>. com* ' un hedef uç noktasını kullanır. 

    Kaynak grubu adı, **RG** eklenmiş proje adıdır.

    > [!NOTE]
    > şablonun başarıyla dağıtılması için **uniqueDNSname** 'in genel olarak benzersiz bir ad olması gerekir. Dağıtım başarısız olursa, 1. adımdan baştan başlayın.

    Şablonun dağıtılması birkaç dakika sürer. Tamamlandığında, çıkış şuna benzerdir:

    :::image type="content" source="./media/quickstart-create-traffic-manager-profile/traffic-manager-arm-powershell-output.png" alt-text="Azure Traffic Manager Kaynak Yöneticisi şablonu PowerShell dağıtım çıkışı":::

Azure PowerShell, şablonu dağıtmak için kullanılır. Azure PowerShell ek olarak, Azure portal, Azure CLı ve REST API de kullanabilirsiniz. Diğer dağıtım yöntemlerini öğrenmek için bkz. [şablonları dağıtma](../azure-resource-manager/templates/deploy-portal.md).

## <a name="validate-the-deployment"></a>Dağıtımı doğrulama

1. [Get-AzTrafficManagerProfile](/powershell/module/az.trafficmanager/get-aztrafficmanagerprofile)kullanarak TRAFFIC Manager profilin DNS adını belirleme.

    ```azurepowershell-interactive
    Get-AzTrafficManagerProfile -Name ExternalEndpointExample -ResourceGroupName $resourceGroupName | Select RelativeDnsName
    ```

    **Relativednsname** değerini kopyalayın. Traffic Manager profilinizin DNS adı *<* relativednsname *>. trafficmanager.net*. 

1. Yerel bir PowerShell 'den **{relativednsname}** değişkenini *<* relativednsname *>. trafficmanager.net*ile değiştirerek aşağıdaki komutu çalıştırın.

    ```powershell
    Resolve-DnsName -Name {relativeDNSname} | Select-Object NameHost | Select -First 1
    ```
    Size hangi bölgeye yaklaşdığına bağlı olarak, *w<span>ww. Microsoft</span>. com* veya *d<span>OCS. Microsoft</span>. com* ' un bir namehost 'u almalısınız.

1. Diğer uç noktaya çözümleneceğini denetlemek için, son adımda aldığınız hedefin uç noktasını devre dışı bırakın. Sırasıyla *w<span>ww. Microsoft</span>. com* veya *d<span>OCS. Microsoft</span>. com* ' un hedefini devre dışı bırakmak için, **{EndpointName}** öğesini **endpoint1** veya **endpoint2** ile değiştirin.

    ```azurepowershell-interactive
    Disable-AzTrafficManagerEndpoint -Name {endpointName} -Type ExternalEndpoints -ProfileName ExternalEndpointExample -ResourceGroupName $resourceGroupName -Force
    ```
1. 2. adımdaki komutu bir yerel PowerShell içinde yeniden çalıştırın. Bu kez diğer uç nokta için diğer ad konağını almalısınız. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Traffic Manager profile artık ihtiyacınız kalmadığında, kaynak grubunu silin. Bu, Traffic Manager profilini ve ilgili tüm kaynakları kaldırır.

Kaynak grubunu silmek için `Remove-AzResourceGroup` cmdlet 'ini çağırın:

```azurepowershell-interactive
Remove-AzResourceGroup -Name <your resource group name>
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta şunu oluşturdunuz:
* Traffic Manager profili

Yönlendirme trafiği hakkında daha fazla bilgi edinmek için Traffic Manager öğreticilerine geçin.

> [!div class="nextstepaction"]
> [Traffic Manager öğreticileri](tutorial-traffic-manager-improve-website-response.md)
