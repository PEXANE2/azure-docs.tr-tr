---
title: Azure PowerShell'i kullanarak Azure Genel IP'sini başka bir Azure bölgesine taşıyın
description: Azure PowerShell'i kullanarak Azure Genel IP'sini bir Azure bölgesinden diğerine taşımak için Azure Kaynak Yöneticisi şablonunu kullanın.
author: asudbring
ms.service: virtual-network
ms.topic: article
ms.date: 08/29/2019
ms.author: allensu
ms.openlocfilehash: c55b6011381d385fed7c7b8175ff02ec9be66fdb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75641596"
---
# <a name="move-azure-public-ip-to-another-region-using-azure-powershell"></a>Azure PowerShell'i kullanarak Azure Genel IP'sini başka bir bölgeye taşıyın

Varolan Azure Genel IP'lerinizi bir bölgeden diğerine taşımak istediğiniz çeşitli senaryolar vardır. Örneğin, aynı yapılandırmaya ve sku sınamak için ortak bir IP oluşturmak isteyebilirsiniz. Ayrıca, olağanüstü durum kurtarma planlamasının bir parçası olarak genel bir IP'yi başka bir bölgeye taşımak isteyebilirsiniz.

Azure Genel Ip'leri bölgeye özgür ve bir bölgeden diğerine taşınamamaktadır. Ancak, genel bir IP'nin varolan yapılandırmasını dışa aktarmak için bir Azure Kaynak Yöneticisi şablonu kullanabilirsiniz.  Daha sonra, ortak IP'yi şablona dışa aktararak, parametreleri hedef bölgeyle eşleşecek şekilde değiştirerek ve ardından şablonu yeni bölgeye dağıtarak kaynağı başka bir bölgeye ayarlayabilirsiniz.  Kaynak Yöneticisi ve şablonlar hakkında daha fazla bilgi için kaynak [gruplarını şablonlara dışa aktarma](https://docs.microsoft.com/azure/azure-resource-manager/manage-resource-groups-powershell#export-resource-groups-to-templates)


## <a name="prerequisites"></a>Ön koşullar

- Azure Genel IP'sinin taşımak istediğiniz Azure bölgesinde olduğundan emin olun.

- Azure Genel IP'leri bölgeler arasında taşınamaz.  Yeni kamu ip'ini hedef bölgedeki kaynaklarla ilişkilendirmeniz gerekir.

- Ortak bir IP yapılandırması dışa aktarmak ve başka bir bölgede genel BIR IP oluşturmak için bir şablon dağıtmak için Ağ Katılımcısı rolüne veya daha yükseğe ihtiyacınız olur.
   
- Kaynak ağ düzenini ve şu anda kullanmakta olduğunuz tüm kaynakları tanımlayın. Bu düzen, yük dengeleyicileri, ağ güvenlik gruplarını (NSG'ler) ve sanal ağlar içerir, ancak bunlarla sınırlı değildir.

- Azure aboneliğinizin, kullanılan hedef bölgede herkese açık IP'ler oluşturmanıza olanak sağladığını doğrulayın. Gerekli kotayı sağlamak için desteğe başvurun.

- Aboneliğinizin bu işlem için genel IP'lerin eklenmesini destekleyecek yeterli kaynağa sahip olduğundan emin olun.  Bkz. [Azure aboneliği ve hizmet sınırları, kotalar ve kısıtlamalar](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#networking-limits).


## <a name="prepare-and-move"></a>Hazırlanın ve hareket edin
Aşağıdaki adımlar, kaynak yöneticisi şablonu kullanarak ortak IP'yi yapılandırma hareketine nasıl hazırlayacağımı ve azure PowerShell kullanarak ortak IP yapılandırmasını hedef bölgeye nasıl taşıyarak göstereceğimizdir.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

### <a name="export-the-template-and-deploy-from-a-script"></a>Şablonu dışa aktarma ve komut dosyasından dağıtma

1. [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-2.5.0) komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin:
    
    ```azurepowershell-interactive
    Connect-AzAccount
    ```

2. Hedef bölgeye taşımak istediğiniz genel IP'nin kaynak kimliğini alın ve [Get-AzPublicIPAddress](https://docs.microsoft.com/powershell/module/az.network/get-azpublicipaddress?view=azps-2.6.0)kullanarak bir değişkene yerleştirin:

    ```azurepowershell-interactive
    $sourcePubIPID = (Get-AzPublicIPaddress -Name <source-public-ip-name> -ResourceGroupName <source-resource-group-name>).Id

    ```
3. Kaynak sanal ağı [Export-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/export-azresourcegroup?view=azps-2.6.0)komutunu çalıştırdığınız dizine .json dosyasına dışa aktarın:
   
   ```azurepowershell-interactive
   Export-AzResourceGroup -ResourceGroupName <source-resource-group-name> -Resource $sourceVNETID -IncludeParameterDefaultValue
   ```

4. İndirilen dosya, kaynağın dışa aktarılan kaynak grubundan sonra adlandırılacaktır.  ** \<kaynak grubu adı>.json** adlı komuttan dışa aktarılan dosyayı bulun ve seçtiğiniz bir editörde açın:
   
   ```azurepowershell
   notepad <source-resource-group-name>.json
   ```

5. Ortak IP adının parametresini değiştirmek için, kaynak genel IP adının özellik **varsayılan değerini** hedef genel IP'nizin adıyla değiştirin ve adın tırnak içinde olduğundan emin olun:
    
    ```json
        {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "publicIPAddresses_myVM1pubIP_name": {
        "defaultValue": "<target-publicip-name>",
        "type": "String"
        }
    }

    ```

6. Genel IP'nin taşınacağı hedef bölgeyi değiştirmek için, kaynakların altındaki **konum** özelliğini değiştirin:

    ```json
            "resources": [
            {
            "type": "Microsoft.Network/publicIPAddresses",
            "apiVersion": "2019-06-01",
            "name": "[parameters('publicIPAddresses_myPubIP_name')]",
            "location": "<target-region>",
            "sku": {
                "name": "Basic",
                "tier": "Regional"
            },
            "properties": {
                "provisioningState": "Succeeded",
                "resourceGuid": "7549a8f1-80c2-481a-a073-018f5b0b69be",
                "ipAddress": "52.177.6.204",
                "publicIPAddressVersion": "IPv4",
                "publicIPAllocationMethod": "Dynamic",
                "idleTimeoutInMinutes": 4,
                "ipTags": []
               }
               }
             ]             
    ```
  
7. Bölge konum kodlarını elde etmek için aşağıdaki komutu çalıştırarak Azure PowerShell cmdlet [Get-AzLocation'ı](https://docs.microsoft.com/powershell/module/az.resources/get-azlocation?view=azps-1.8.0) kullanabilirsiniz:

    ```azurepowershell-interactive

    Get-AzLocation | format-table
    
    ```
8. İsterseniz şablondaki diğer parametreleri de değiştirebilirsiniz ve gereksinimlerinize bağlı olarak isteğe bağlıdır:

    * **Sku** - ** \<Kaynak grubu adı>.json** dosyasındaki **sku** > **adı** özelliğini değiştirerek yapılandırmadaki genel IP'nin sku'yu standarttan temele veya temelden standarda değiştirebilirsiniz:

         ```json
            "resources": [
                   {
                    "type": "Microsoft.Network/publicIPAddresses",
                    "apiVersion": "2019-06-01",
                    "name": "[parameters('publicIPAddresses_myPubIP_name')]",
                    "location": "<target-region>",
                    "sku": {
                        "name": "Basic",
                        "tier": "Regional"
                    },
         ```

         Temel ve standart sku genel ips arasındaki farklar hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address)

    * **Genel IP ayırma yöntemi** ve **Boşta zaman ayarı** - **PublicIPAllocationMethod** özelliğini **Dinamik'ten** **Statik** veya **Dynamic** **Statik'e** dinamik olarak değiştirerek şablondaki bu seçeneklerin her ikisini de değiştirebilirsiniz. Boşta zaman dilimi, **boşta kalanTimeoutInMinutes** özelliğini istediğiniz miktarda değiştirerek değiştirilebilir.  Varsayılan **4:**

         ```json
         "resources": [
                {
                "type": "Microsoft.Network/publicIPAddresses",
                "apiVersion": "2019-06-01",
                "name": "[parameters('publicIPAddresses_myPubIP_name')]",
                "location": "<target-region>",
                  "sku": {
                  "name": "Basic",
                  "tier": "Regional"
                 },
                "properties": {
                "provisioningState": "Succeeded",
                "resourceGuid": "7549a8f1-80c2-481a-a073-018f5b0b69be",
                "ipAddress": "52.177.6.204",
                "publicIPAddressVersion": "IPv4",
                "publicIPAllocationMethod": "Dynamic",
                "idleTimeoutInMinutes": 4,
                "ipTags": []
                   }
                }            
         ```

        Ayırma yöntemleri ve boşta kalma zaman ayırma değerleri hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address)


9. Kaynak ** \<grubu adı>.json** dosyasını kaydedin.

10. Hedef ortak IP'nin [Yeni-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup?view=azps-2.6.0)kullanarak dağıtılması için hedef bölgede bir kaynak grubu oluşturun.
    
    ```azurepowershell-interactive
    New-AzResourceGroup -Name <target-resource-group-name> -location <target-region>
    ```
11. Düzenlenen ** \<kaynak grubu adı>.json** dosyasını [Yeni-AzResourceGroupDeployment](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroupdeployment?view=azps-2.6.0)kullanarak önceki adımda oluşturulan kaynak grubuna dağıtın:

    ```azurepowershell-interactive

    New-AzResourceGroupDeployment -ResourceGroupName <target-resource-group-name> -TemplateFile <source-resource-group-name>.json
    
    ```

12. Hedef bölgede oluşturulan kaynakları doğrulamak için [Get-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/get-azresourcegroup?view=azps-2.6.0) ve [Get-AzPublicIPAddress'i](https://docs.microsoft.com/powershell/module/az.network/get-azpublicipaddress?view=azps-2.6.0)kullanın:
    
    ```azurepowershell-interactive

    Get-AzResourceGroup -Name <target-resource-group-name>

    ```

    ```azurepowershell-interactive

    Get-AzPublicIPAddress -Name <target-publicip-name> -ResourceGroupName <target-resource-group-name>

    ```
## <a name="discard"></a>Vazgeç 

Dağıtımdan sonra, yeniden başlamak veya hedefteki genel ip'yi atmak isterseniz, hedefte oluşturulan kaynak grubunu silin ve taşınan genel IP silinir.  Kaynak grubunu kaldırmak için [Remove-AzResourceGroup'u](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup?view=azps-2.6.0)kullanın:

```azurepowershell-interactive

Remove-AzResourceGroup -Name <target-resource-group-name>

```

## <a name="clean-up"></a>Temizleme

Değişiklikleri işlemek ve sanal ağın hareketini tamamlamak için, kaynak sanal ağ veya kaynak grubunu silmek için [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup?view=azps-2.6.0) veya [Remove-AzPublicIPAddress](https://docs.microsoft.com/powershell/module/az.network/remove-azpublicipaddress?view=azps-2.6.0)kullanın:

```azurepowershell-interactive

Remove-AzResourceGroup -Name <source-resource-group-name>

```

``` azurepowershell-interactive

Remove-AzPublicIpAddress -Name <source-publicip-name> -ResourceGroupName <resource-group-name>

```

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, bir Azure Genel IP'sini bir bölgeden diğerine taşıdınız ve kaynak kaynaklarını temizlediniz.  Azure'da kaynakları bölgeler arasında taşıma ve olağanüstü durum kurtarma hakkında daha fazla bilgi edinmek için şu na bakın:


- [Kaynakları yeni kaynak grubuna veya aboneliğe taşıma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)
- [Azure VM’lerini başka bir bölgeye taşıma](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-migrate)
