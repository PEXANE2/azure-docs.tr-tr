---
title: Azure PowerShell kullanarak Azure sanal ağını başka bir Azure bölgesine taşıma
description: Azure sanal ağını Azure PowerShell kullanarak bir Azure bölgesinden diğerine taşımak için Azure Resource Manager şablonu kullanın.
author: asudbring
ms.service: virtual-network
ms.topic: article
ms.date: 08/26/2019
ms.author: allensu
ms.openlocfilehash: 0037419570ceffcd11dbc8dfe85b45d7ec0ee8ce
ms.sourcegitcommit: 1752581945226a748b3c7141bffeb1c0616ad720
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2019
ms.locfileid: "70997451"
---
# <a name="move-azure-virtual-network-to-another-region-using-azure-powershell"></a>Azure PowerShell kullanarak Azure sanal ağını başka bir bölgeye taşıma

Mevcut Azure sanal ağlarınızı (VNet) bir bölgeden diğerine taşımak istediğiniz çeşitli senaryolar vardır. Örneğin, var olan sanal ağınızın sınanması ve kullanılabilirliği için aynı yapılandırmaya sahip bir sanal ağ oluşturmak isteyebilirsiniz. Ayrıca, olağanüstü durum kurtarma planlamasının bir parçası olarak bir üretim sanal ağını başka bir bölgeye taşımak isteyebilirsiniz.

Sanal ağı başka bir bölgeye taşımayı gerçekleştirmek için bir Azure Resource Manager şablonu kullanabilirsiniz. Bunu, sanal ağı bir şablona vererek, parametreleri hedef bölgeyle eşleşecek şekilde değiştirerek ve sonra şablonu yeni bölgeye dağıtarak yapabilirsiniz.  Kaynak Yöneticisi ve şablonlar hakkında daha fazla bilgi için bkz. [kaynak gruplarını şablonlara dışarı aktarma](https://docs.microsoft.com/azure/azure-resource-manager/manage-resource-groups-powershell#export-resource-groups-to-templates)


## <a name="prerequisites"></a>Önkoşullar

- Azure sanal ağının, taşımak istediğiniz Azure bölgesinde olduğundan emin olun.

- Bir sanal ağı dışa aktarmak ve başka bir bölgede bir sanal ağ oluşturmak için şablon dağıtmak için, ağ katılımcısı rolü veya daha yüksek bir sürümü gerekir.

- Sanal ağ eşlemeleri yeniden oluşturulmaz ve şablonda hala mevcutsa başarısız olur.  Şablonu dışarı aktarmadan önce sanal ağ eşlerini kaldırmalı ve ardından sanal ağ taşıdıktan sonra eşleri yeniden kurmalısınız.
    
- Kaynak ağ düzeni ve şu anda kullanmakta olduğunuz tüm kaynakları belirler. Bu düzen, yük dengeleyiciler, ağ güvenlik grupları (NSG 'Ler) ve genel IP 'Leri içerir ancak bunlarla sınırlı değildir.

- Azure aboneliğinizin, kullanılan hedef bölgede sanal ağlar oluşturmanıza izin verdiğini doğrulayın. Gerekli kotayı sağlamak için desteğe başvurun.

- Aboneliğinizin bu işleme yönelik sanal ağların eklenmesini desteklemek için yeterli kaynağa sahip olduğundan emin olun.  Bkz. [Azure aboneliği ve hizmet limitleri, Kotalar ve kısıtlamalar](https://docs.microsoft.com/azure/azure-subscription-service-limits#networking-limits)


## <a name="prepare-and-move"></a>Hazırlama ve taşıma
Aşağıdaki adımlarda, sanal ağı bir Kaynak Yöneticisi şablonu kullanarak taşıma için hazırlama ve Azure PowerShell komutlarını kullanarak sanal ağı hedef bölgeye taşıma işlemleri gösterilmektedir.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

### <a name="export-the-virtual-network-and-deploy-the-target-virtual-network-with-powershell"></a>Sanal ağı dışarı aktarma ve hedef sanal ağı PowerShell ile dağıtma

1. [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-2.5.0) komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin:
    
    ```azurepowershell-interactive
    Connect-AzAccount
    ```

2. Hedef sanal ağı dağıtmak istediğiniz abonelik KIMLIĞINI edinin ve [Get-AzSubscription](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-2.5.0)kullanarak bir değişkene yerleştirin:
   
    ```azurepowershell-interactive
    Get-AzSubscription | format-table

    $Subscription = "MySubscriptionID"
    ```
3. Hedef bölgeye taşımak istediğiniz sanal ağın kaynak KIMLIĞINI alın ve [Get-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/get-azvirtualnetwork?view=azps-2.6.0)kullanarak bir değişkene yerleştirin:

    ```azurepowershell-interactive
    $sourceVNETID = (Get-AzVirtualNetwork -Name <source-virtual-network-name> -ResourceGroupName <source-resource-group-name>).Id

    ```
4. Kaynak sanal ağı bir. JSON dosyasına dışarı aktarma [-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/export-azresourcegroup?view=azps-2.6.0)komutunu yürütebileceğiniz dizine aktarın:
   
   ```azurepowershell-interactive
   Export-AzResourceGroup -ResourceGroupName <source-resource-group-name> -Resource $sourceVNETID -IncludeParameterDefaultValue
   ```

5. İndirilen dosya, kaynağın öğesinden verildikten sonra adı alınacaktır.  **< Resource-Group-name >. JSON** adlı komuttan aktarılmış dosyayı bulun ve seçtiğiniz bir düzenleyicide açın:
   
   ```azurepowershell
   notepad <source-resource-group-name>.json
   ```

6. Sanal ağ adının parametresini düzenlemek için, kaynak sanal ağ adının **DefaultValue** özelliğini hedef sanal ağınızın adıyla değiştirin, adın tırnak içinde olduğundan emin olun:
    
    ```json
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentmyResourceGroupVNET.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "virtualNetworks_myVNET1_name": {
        "defaultValue": "<target-virtual-network-name>",
        "type": "String"
        }
    ```

7.  VNET 'in taşınacağı hedef bölgeyi düzenlemek için kaynaklar altındaki **Location** özelliğini değiştirin:

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
  
8. Bölge konum kodlarını almak için aşağıdaki komutu çalıştırarak [Get-AzLocation](https://docs.microsoft.com/powershell/module/az.resources/get-azlocation?view=azps-1.8.0) cmdlet 'ini Azure PowerShell kullanabilirsiniz:

    ```azurepowershell-interactive

    Get-AzLocation | format-table
    
    ```
9.  Ayrıca, **< Kaynak-Grup-adı >. JSON** dosyasındaki diğer parametreleri değiştirebilirsiniz ve gereksinimlerinize bağlı olarak isteğe bağlıdır:

    * **Adres alanı** -sanal ağın adres alanı, **Resources** > **AddressSpace** bölümü değiştirilerek ve < Resource-Group-Name içindeki **addresspredüzeltmelerinin** özelliği değiştirilerek kaydedilmeden önce değiştirilebilir  **>. JSON** dosyası:
    
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

    * **Alt ağ** -alt ağ adı ve alt ağ adres alanı, **< Resource-group-name >. JSON** dosyasının **alt ağlar** bölümü değiştirilerek değiştirilebilir veya öğesine eklenebilir. Alt ağın adı, **ad** özelliğini değiştirerek değiştirilebilir. **< Resource-Group-name >. JSON** dosyasındaki **addresspredüzeltmesini** özelliğini değiştirerek alt ağ adres alanı değiştirilebilir:
    
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
                    ],
    ```

    **< Resource-Group-name >. JSON** dosyasında, adres ön ekini değiştirmek için, yukarıda listelenen bölüm ve aşağıda listelenen **tür** bölümü olmak üzere iki yerde düzenlenmelidir.  **Addresspredüzeltmesini** özelliğini, yukarıdaki bir ile eşleşecek şekilde değiştirin:
                
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

10. **Resource-Group-name >. json dosyasını <** kaydedin.

11. Hedef sanal ağın [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup?view=azps-2.6.0) kullanılarak dağıtılması için hedef bölgede bir kaynak grubu oluşturun
    
    ```azurepowershell-interactive
    New-AzResourceGroup -Name <target-resource-group-name> -location <target-region>
    ```
    
12. Düzenlenmiş **< Resource-Group-name >. JSON** dosyasını, önceki adımda oluşturulan kaynak grubuna [New-AzResourceGroupDeployment](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroupdeployment?view=azps-2.6.0)kullanarak dağıtın:

    ```azurepowershell-interactive

    New-AzResourceGroupDeployment -ResourceGroupName <target-resource-group-name> -TemplateFile <source-resource-group-name>.json
    
    ```

13. Hedef bölgede kaynakların oluşturulduğunu doğrulamak için [Get-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/get-azresourcegroup?view=azps-2.6.0) ve [Get-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/get-azvirtualnetwork?view=azps-2.6.0)kullanın:
    
    ```azurepowershell-interactive

    Get-AzResourceGroup -Name <target-resource-group-name>

    ```

    ```azurepowershell-interactive

    Get-AzVirtualNetwork -Name <target-virtual-network-name> -ResourceGroupName <target-resource-group-name>

    ```

## <a name="discard"></a>At 

Dağıtımdan sonra, hedefteki sanal ağı baştan başlatmak veya atmak istiyorsanız hedefte oluşturulan kaynak grubunu silin ve taşınan sanal ağ silinir.  Kaynak grubunu kaldırmak için [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup?view=azps-2.6.0)komutunu kullanın:

```azurepowershell-interactive

Remove-AzResourceGroup -Name <target-resource-group-name>

```

## <a name="clean-up"></a>Temizleme

Değişiklikleri yürütmek ve sanal ağı taşımayı tamamlamak için, kaynak sanal ağını veya kaynak grubunu silin, [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup?view=azps-2.6.0) veya [Remove-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/remove-azvirtualnetwork?view=azps-2.6.0)komutunu kullanın:

```azurepowershell-interactive

Remove-AzResourceGroup -Name <source-resource-group-name>

```

``` azurepowershell-interactive

Remove-AzVirtualNetwork -Name <source-virtual-network-name> -ResourceGroupName <source-resource-group-name>

```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir Azure sanal ağını bir bölgeden diğerine taşıdı ve kaynak kaynakları temizledi.  Azure 'da bölgeler ve olağanüstü durum kurtarma arasında kaynakları taşıma hakkında daha fazla bilgi edinmek için bkz:


- [Kaynakları yeni kaynak grubuna veya aboneliğe taşıma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)
- [Azure VM’lerini başka bir bölgeye taşıma](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-migrate)
