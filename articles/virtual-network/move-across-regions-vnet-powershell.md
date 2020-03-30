---
title: Azure PowerShell'i kullanarak Azure sanal ağını başka bir Azure bölgesine taşıma
description: Kaynak Yöneticisi şablonu ve Azure PowerShell kullanarak bir Azure sanal ağını bir Azure bölgesinden diğerine taşıyın.
author: asudbring
ms.service: virtual-network
ms.topic: article
ms.date: 08/26/2019
ms.author: allensu
ms.openlocfilehash: dc316e5bbb88359ff8b1e8a4fc35a56541a577f6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75646719"
---
# <a name="move-an-azure-virtual-network-to-another-region-by-using-azure-powershell"></a>Azure PowerShell'i kullanarak Azure sanal ağını başka bir bölgeye taşıma

Varolan bir Azure sanal ağını bir bölgeden diğerine taşımak için çeşitli senaryolar vardır. Örneğin, varolan sanal ağınızla test etmek ve kullanılabilirlik için aynı yapılandırmaya sahip bir sanal ağ oluşturmak isteyebilirsiniz. Veya olağanüstü durum kurtarma planlamanızın bir parçası olarak bir üretim sanal ağını başka bir bölgeye taşımak isteyebilirsiniz.

Sanal ağın başka bir bölgeye taşınmasını tamamlamak için bir Azure Kaynak Yöneticisi şablonu kullanabilirsiniz. Bunu, sanal ağı şablona dışa aktararak, parametreleri hedef bölgeyle eşleşecek şekilde değiştirerek ve ardından şablonu yeni bölgeye dağıtarak yaparsınız. Kaynak Yöneticisi şablonları hakkında daha fazla bilgi için [kaynak gruplarını şablonlara dışa aktar'a](https://docs.microsoft.com/azure/azure-resource-manager/manage-resource-groups-powershell#export-resource-groups-to-templates)bakın.


## <a name="prerequisites"></a>Ön koşullar

- Sanal ağınızın taşınmak istediğiniz Azure bölgesinde olduğundan emin olun.

- Sanal ağ dışa aktarmak ve başka bir bölgede sanal ağ oluşturmak için şablon dağıtmak için Ağ Katılımcısı rolüne veya daha yüksek olması gerekir.

- Sanal ağ eşlemeleri yeniden oluşturulmaz ve şablonda hala bulunurlarsa başarısız olurlar. Şablonu dışa aktarmadan önce, sanal ağ eşlerini kaldırmanız gerekir. Daha sonra sanal ağ hareket ettikten sonra bunları yeniden oluşturabilirsiniz.
    
- Kaynak ağ düzenini ve şu anda kullanmakta olduğunuz tüm kaynakları tanımlayın. Bu düzen, yük dengeleyicileri, ağ güvenlik gruplarını (NSG'ler) ve genel IP'leri içerir, ancak bunlarla sınırlı değildir.

- Azure aboneliğinizin hedef bölgede sanal ağlar oluşturmanıza olanak sağladığını doğrulayın. Gerekli kotayı etkinleştirmek için desteğe başvurun.

- Aboneliğinizin bu işlem için sanal ağların eklenmesini destekleyecek yeterli kaynağa sahip olduğundan emin olun. Daha fazla bilgi için bkz. [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#networking-limits).


## <a name="prepare-for-the-move"></a>Harekete hazırlanın
Bu bölümde, kaynak yöneticisi şablonu kullanarak sanal ağı taşımaya hazırlarsınız. Ardından Azure PowerShell komutlarını kullanarak sanal ağı hedef bölgeye taşırsınız.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Sanal ağı dışa aktarmak ve PowerShell'i kullanarak hedef sanal ağı dağıtmak için aşağıdakileri yapın:

1. [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-2.5.0) komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin:
    
    ```azurepowershell-interactive
    Connect-AzAccount
    ```

1. Hedef bölgeye taşımak istediğiniz sanal ağın kaynak kimliğini edinin ve [get-AzVirtualNetwork'ü](https://docs.microsoft.com/powershell/module/az.network/get-azvirtualnetwork?view=azps-2.6.0)kullanarak bir değişkene yerleştirin:

    ```azurepowershell-interactive
    $sourceVNETID = (Get-AzVirtualNetwork -Name <source-virtual-network-name> -ResourceGroupName <source-resource-group-name>).Id
    ```

1. Kaynak sanal ağı [Dışa Aktarma-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/export-azresourcegroup?view=azps-2.6.0)komutunu çalıştırdığınız dizindeki bir .json dosyasına dışa aktarma:
   
   ```azurepowershell-interactive
   Export-AzResourceGroup -ResourceGroupName <source-resource-group-name> -Resource $sourceVNETID -IncludeParameterDefaultValue
   ```

1. İndirilen dosya, kaynağın dışa aktarıldığı kaynak grubuyla aynı ada sahiptir. Komutla * \<dışa aktardığınız kaynak grubu>.json* dosyasını bulun ve ardından düzenleyicinizde açın:
   
   ```azurepowershell
   notepad <source-resource-group-name>.json
   ```

1. Sanal ağ adının parametresini değiştirmek için, kaynak sanal ağ adının **varsayılan Değer** özelliğini hedef sanal ağınızın adıyla değiştirin. Adı tırnak işaretlerine eklediğinden emin olun.
    
    ```json
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentmyResourceGroupVNET.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "virtualNetworks_myVNET1_name": {
        "defaultValue": "<target-virtual-network-name>",
        "type": "String"
        }
    ```

1. Sanal ağın taşınacağı hedef bölgeyi değiştirmek için, kaynakların altındaki **konum** özelliğini değiştirin:

    ```json
    "resources": [
                {
                    "type": "Microsoft.Network/virtualNetworks",
                    "apiVersion": "2019-06-01",
                    "name": "[parameters('virtualNetworks_myVNET1_name')]",
                    "location": "<target-region>",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "resourceGuid": "6e2652be-35ac-4e68-8c70-621b9ec87dcb",
                        "addressSpace": {
                            "addressPrefixes": [
                                "10.0.0.0/16"
                            ]
                        },

    ```
  
1. Bölge konum kodlarını elde etmek için aşağıdaki komutu çalıştırarak Azure PowerShell cmdlet [Get-AzLocation'ı](https://docs.microsoft.com/powershell/module/az.resources/get-azlocation?view=azps-1.8.0) kullanabilirsiniz:

    ```azurepowershell-interactive

    Get-AzLocation | format-table
    ```

1. (İsteğe bağlı) Gereksinimlerinize bağlı olarak * \<kaynak grubu adı>.json* dosyasındaki diğer parametreleri de değiştirebilirsiniz:

    * **Adres Alanı**: Dosyayı kaydetmeden **önce, kaynak** > **adresiSpace** bölümünü değiştirerek ve **adresPrefixes** özelliğini değiştirerek sanal ağın adres alanını değiştirebilirsiniz:

        ```json
                "resources": [
                    {
                    "type": "Microsoft.Network/virtualNetworks",
                    "apiVersion": "2019-06-01",
                    "name": "[parameters('virtualNetworks_myVNET1_name')]",
                    "location": "<target-region",
                    "properties": {
                    "provisioningState": "Succeeded",
                    "resourceGuid": "6e2652be-35ac-4e68-8c70-621b9ec87dcb",
                    "addressSpace": {
                        "addressPrefixes": [
                        "10.0.0.0/16"
                        ]
                    },
        ```

    * **Alt Net**: Dosyanın **alt ağlar** bölümünü değiştirerek alt net adını ve alt net adres alanını değiştirebilir veya ekleyebilirsiniz. **Ad** özelliğini değiştirerek alt ağın adını değiştirebilirsiniz. Ve **adresPrefix** özelliğini değiştirerek alt net adres alanını değiştirebilirsiniz:

        ```json
                "subnets": [
                    {
                    "name": "subnet-1",
                    "etag": "W/\"d9f6e6d6-2c15-4f7c-b01f-bed40f748dea\"",
                    "properties": {
                    "provisioningState": "Succeeded",
                    "addressPrefix": "10.0.0.0/24",
                    "delegations": [],
                    "privateEndpointNetworkPolicies": "Enabled",
                    "privateLinkServiceNetworkPolicies": "Enabled"
                    }
                    },
                    {
                    "name": "GatewaySubnet",
                    "etag": "W/\"d9f6e6d6-2c15-4f7c-b01f-bed40f748dea\"",
                    "properties": {
                    "provisioningState": "Succeeded",
                    "addressPrefix": "10.0.1.0/29",
                    "serviceEndpoints": [],
                    "delegations": [],
                    "privateEndpointNetworkPolicies": "Enabled",
                    "privateLinkServiceNetworkPolicies": "Enabled"
                    }
                    }

                ]
        ```

        Adres önekini değiştirmek için dosyayı iki yerde edin: önceki bölümdeki kodda ve aşağıdaki kodun **tür** bölümünde. Yukarıdaki bölümde koddaki **adresPrefix** özelliğiyle eşleşecek şekilde aşağıdaki koddaki **prefix** özelliğini değiştirin.

        ```json
         "type": "Microsoft.Network/virtualNetworks/subnets",
           "apiVersion": "2019-06-01",
           "name": "[concat(parameters('virtualNetworks_myVNET1_name'), '/GatewaySubnet')]",
              "dependsOn": [
                 "[resourceId('Microsoft.Network/virtualNetworks', parameters('virtualNetworks_myVNET1_name'))]"
                   ],
              "properties": {
                 "provisioningState": "Succeeded",
                 "addressPrefix": "10.0.1.0/29",
                 "serviceEndpoints": [],
                 "delegations": [],
                 "privateEndpointNetworkPolicies": "Enabled",
                 "privateLinkServiceNetworkPolicies": "Enabled"
                  }
                 },
                  {
                  "type": "Microsoft.Network/virtualNetworks/subnets",
                  "apiVersion": "2019-06-01",
                  "name": "[concat(parameters('virtualNetworks_myVNET1_name'), '/subnet-1')]",
                     "dependsOn": [
                        "[resourceId('Microsoft.Network/virtualNetworks', parameters('virtualNetworks_myVNET1_name'))]"
                          ],
                     "properties": {
                        "provisioningState": "Succeeded",
                        "addressPrefix": "10.0.0.0/24",
                        "delegations": [],
                        "privateEndpointNetworkPolicies": "Enabled",
                        "privateLinkServiceNetworkPolicies": "Enabled"
                         }
                  }
         ]
        ```

1. Kaynak * \<grubu adı>.json* dosyasını kaydedin.

1. [Yeni-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup?view=azps-2.6.0)kullanarak dağıtılacak hedef sanal ağ için hedef bölgede bir kaynak grubu oluşturun:
    
    ```azurepowershell-interactive
    New-AzResourceGroup -Name <target-resource-group-name> -location <target-region>
    ```
    
1. Düzenlenen * \<kaynak grubu adı>.json* [dosyasını, Yeni-AzResourceGroupDeployment'ı](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroupdeployment?view=azps-2.6.0)kullanarak önceki adımda oluşturduğunuz kaynak grubuna dağıtın:

    ```azurepowershell-interactive

    New-AzResourceGroupDeployment -ResourceGroupName <target-resource-group-name> -TemplateFile <source-resource-group-name>.json
    ```

1. Kaynakların hedef bölgede oluşturulduğunu doğrulamak için [Get-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/get-azresourcegroup?view=azps-2.6.0) ve [Get-AzVirtualNetwork'ü](https://docs.microsoft.com/powershell/module/az.network/get-azvirtualnetwork?view=azps-2.6.0)kullanın:
    
    ```azurepowershell-interactive

    Get-AzResourceGroup -Name <target-resource-group-name>
    ```

    ```azurepowershell-interactive

    Get-AzVirtualNetwork -Name <target-virtual-network-name> -ResourceGroupName <target-resource-group-name>
    ```

## <a name="delete-the-virtual-network-or-resource-group"></a>Sanal ağı veya kaynak grubunu silme 

Sanal ağı dağıttıktan sonra, hedef bölgedeki sanal ağı yeniden başlatmak veya atmak için, hedef bölgede oluşturduğunuz kaynak grubunu silin ve taşınan sanal ağ silinir. 

Kaynak grubunu kaldırmak için [Remove-AzResourceGroup'u](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup?view=azps-2.6.0)kullanın:

```azurepowershell-interactive

Remove-AzResourceGroup -Name <target-resource-group-name>
```

## <a name="clean-up"></a>Temizleme

Değişikliklerinizi işlemek ve sanal ağ hareketini tamamlamak için aşağıdakilerden birini yapın:

* [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup?view=azps-2.6.0)kullanarak kaynak grubunu silme:

    ```azurepowershell-interactive

    Remove-AzResourceGroup -Name <source-resource-group-name>
    ```

* [Remove-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/remove-azvirtualnetwork?view=azps-2.6.0)kullanarak kaynak sanal ağ silme:  
    ``` azurepowershell-interactive

    Remove-AzVirtualNetwork -Name <source-virtual-network-name> -ResourceGroupName <source-resource-group-name>
    ```

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, PowerShell'i kullanarak sanal bir ağı bir bölgeden diğerine taşıdınız ve ardından gereksiz kaynak kaynaklarını temizlediniz. Azure'da kaynakları bölgeler arasında taşıma ve olağanüstü durum kurtarma hakkında daha fazla bilgi edinmek için bkz:

- [Kaynakları yeni kaynak grubuna veya aboneliğe taşıma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)
- [Azure sanal makinelerini başka bir bölgeye taşıma](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-migrate)
