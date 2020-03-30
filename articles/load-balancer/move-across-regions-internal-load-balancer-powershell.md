---
title: Azure PowerShell'i kullanarak Azure dahili Yük Bakiyeleyicisini başka bir Azure bölgesine taşıyın
description: Azure PowerShell'i kullanarak Azure dahili Yük Bakiyeleyicisini bir Azure bölgesinden diğerine taşımak için Azure Kaynak Yöneticisi şablonunu kullanın
author: asudbring
ms.service: load-balancer
ms.topic: article
ms.date: 09/17/2019
ms.author: allensu
ms.openlocfilehash: f8e431124155fe23853fe61e985fe4db522c3f77
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75644282"
---
# <a name="move-azure-internal-load-balancer-to-another-region-using-powershell"></a>PowerShell'i kullanarak Azure dahili Yük Bakiyesini başka bir bölgeye taşıyın

Varolan iç yük bakiyecinizi bir bölgeden diğerine taşımak istediğiniz çeşitli senaryolar vardır. Örneğin, sınama için aynı yapılandırmaya sahip bir dahili yük dengeleyicisi oluşturmak isteyebilirsiniz. Ayrıca, olağanüstü durum kurtarma planlamasının bir parçası olarak bir iç yük dengeleyicisini başka bir bölgeye taşımak isteyebilirsiniz.

Azure dahili yük dengeleyicileri bir bölgeden diğerine taşınamaz. Ancak, bir iç yük dengeleyicisinin varolan yapılandırmasını ve sanal ağını dışa aktarmak için bir Azure Kaynak Yöneticisi şablonu kullanabilirsiniz.  Daha sonra, yük bakiyesi ve sanal ağı şablona dışlayarak, parametreleri hedef bölgeyle eşleşecek şekilde değiştirerek ve ardından şablonları yeni bölgeye dağıtarak kaynağı başka bir bölgeye ayarlayabilirsiniz.  Kaynak Yöneticisi ve şablonlar hakkında daha fazla bilgi için kaynak [gruplarını şablonlara dışa aktarma](https://docs.microsoft.com/azure/azure-resource-manager/manage-resource-groups-powershell#export-resource-groups-to-templates)


## <a name="prerequisites"></a>Ön koşullar

- Azure dahili yük bakiyeleyicisinin taşımak istediğiniz Azure bölgesinde olduğundan emin olun.

- Azure dahili yük dengeleyicileri bölgeler arasında taşıt edilemez.  Yeni yük dengeleyicisini hedef bölgedeki kaynaklarla ilişkilendirmeniz gerekir.

- Bir dahili yük dengeleyici yapılandırması dışa aktarmak ve başka bir bölgede bir iç yük dengeleyicisi oluşturmak için bir şablon dağıtmak için, Ağ Katılımcısı rolü veya daha yüksek olması gerekir.
   
- Kaynak ağ düzenini ve şu anda kullanmakta olduğunuz tüm kaynakları tanımlayın. Bu düzen yük dengeleyicileri, ağ güvenlik grupları, sanal makineler ve sanal ağlar içerir, ancak bunlarla sınırlı değildir.

- Azure aboneliğinizin, kullanılan hedef bölgede dahili yük dengeleyicileri oluşturmanıza olanak sağladığını doğrulayın. Gerekli kotayı sağlamak için desteğe başvurun.

- Aboneliğinizin bu işlem için yük dengeleyicileri eklenmesini destekleyecek yeterli kaynağa sahip olduğundan emin olun.  Bkz. [Azure abonelik ve hizmet sınırları, kotalar ve kısıtlamalar](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#networking-limits)


## <a name="prepare-and-move"></a>Hazırlanın ve hareket edin
Aşağıdaki adımlar, Kaynak Yöneticisi şablonu kullanarak iç yük dengeleyicisinin taşımaya nasıl hazırlanacağını ve azure PowerShell kullanarak iç yük dengeleyici yapılandırmasını hedef bölgeye nasıl taşıyacağımı gösterir.  Bu işlemin bir parçası olarak, dahili yük dengeleyicisinin sanal ağ yapılandırması dahil edilmeli ve iç yük dengeleyicisini hareket ettirmeden önce yapılmalıdır.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

### <a name="export-the-virtual-network-template-and-deploy-from-azure-powershell"></a>Sanal ağ şablonu dışa aktarın ve Azure PowerShell'den dağıtın

1. [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-2.5.0) komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin:
    
    ```azurepowershell-interactive
    Connect-AzAccount
    ```
2.  Hedef bölgeye taşımak istediğiniz sanal ağın kaynak kimliğini edinin ve [Get-AzVirtualNetwork'ü](https://docs.microsoft.com/powershell/module/az.network/get-azvirtualnetwork?view=azps-2.6.0)kullanarak bir değişkene yerleştirin:

    ```azurepowershell-interactive
    $sourceVNETID = (Get-AzVirtualNetwork -Name <source-virtual-network-name> -ResourceGroupName <source-resource-group-name>).Id

    ```
3. Kaynak sanal ağı [Export-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/export-azresourcegroup?view=azps-2.6.0)komutunu çalıştırdığınız dizine .json dosyasına dışa aktarın:
   
   ```azurepowershell-interactive
   Export-AzResourceGroup -ResourceGroupName <source-resource-group-name> -Resource $sourceVNETID -IncludeParameterDefaultValue
   ```

4. İndirilen dosya, kaynağın dışa aktarılan kaynak grubundan sonra adlandırılacaktır.  ** \<kaynak grubu adı>.json** adlı komuttan dışa aktarılan dosyayı bulun ve seçtiğiniz bir editörde açın:
   
   ```azurepowershell
   notepad.exe <source-resource-group-name>.json
   ```

5. Sanal ağ adının parametresini değiştirmek için, kaynak sanal ağ adının özellik **varsayılan** değerini hedef sanal ağınızın adıyla değiştirin, adın tırnak içinde olduğundan emin olun:
    
    ```json
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentmyResourceGroupVNET.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "virtualNetworks_myVNET1_name": {
        "defaultValue": "<target-virtual-network-name>",
        "type": "String"
        }
    ```

6.  VNET'in taşınacağı hedef bölgeyi değiştirmek için kaynakların altındaki **konum** özelliğini değiştirin:

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
  
7. Bölge konum kodlarını elde etmek için aşağıdaki komutu çalıştırarak Azure PowerShell cmdlet [Get-AzLocation'ı](https://docs.microsoft.com/powershell/module/az.resources/get-azlocation?view=azps-1.8.0) kullanabilirsiniz:

    ```azurepowershell-interactive

    Get-AzLocation | format-table
    
    ```
8.  İsterseniz kaynak ** \<grubu adı>.json** dosyasındaki diğer parametreleri de değiştirebilirsiniz ve gereksinimlerinize bağlı olarak isteğe bağlı olarak aşağıdakileri yapabilirsiniz:

    * **Adres Alanı** - Kaynak **resources** > **adresiSpace** bölümünü değiştirerek ve ** \<kaynak grubu adı>.json** dosyasındaki **adresPrefixes** özelliğini değiştirerek kaydetmeden önce VNET'in adres alanı değiştirilebilir:

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

    * **Subnet** - Alt ağ adı ve alt net adresi ** \<alanı, kaynak grubu adı>.json** dosyasının **alt ağlar** bölümü değiştirilerek değiştirilebilir veya eklenebilir. Alt ağın **adı, ad** özelliği değiştirilerek değiştirilebilir. Alt ağ adresi alanı ** \<kaynak grubu adı>.json** dosyasındaki **adresPrefix** özelliği değiştirilerek değiştirilebilir:

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

         Kaynak grubu adı>.json dosyasında, adres önekini değiştirmek için, yukarıda listelenen bölüm ve aşağıda listelenen **tür** bölümü olmak üzere iki yerde düzenlenmelidir. ** \<**  Yukarıdakiyle eşleşecek şekilde **adresPrefix** özelliğini değiştirin:

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

9.  Kaynak ** \<grubu adı>.json** dosyasını kaydedin.

10. Hedef VNET'in [Yeni-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup?view=azps-2.6.0) kullanılarak dağıtılması için hedef bölgede bir kaynak grubu oluşturma
    
    ```azurepowershell-interactive
    New-AzResourceGroup -Name <target-resource-group-name> -location <target-region>
    ```
    
11. Düzenlenen ** \<kaynak grubu adı>.json** dosyasını [Yeni-AzResourceGroupDeployment](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroupdeployment?view=azps-2.6.0)kullanarak önceki adımda oluşturulan kaynak grubuna dağıtın:

    ```azurepowershell-interactive

    New-AzResourceGroupDeployment -ResourceGroupName <target-resource-group-name> -TemplateFile <source-resource-group-name>.json
    
    ```
12. Hedef bölgede oluşturulan kaynakları doğrulamak için [Get-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/get-azresourcegroup?view=azps-2.6.0) ve [Get-AzVirtualNetwork'ü](https://docs.microsoft.com/powershell/module/az.network/get-azvirtualnetwork?view=azps-2.6.0)kullanın:
    
    ```azurepowershell-interactive

    Get-AzResourceGroup -Name <target-resource-group-name>

    ```

    ```azurepowershell-interactive

    Get-AzVirtualNetwork -Name <target-virtual-network-name> -ResourceGroupName <target-resource-group-name>

    ```
### <a name="export-the-internal-load-balancer-template-and-deploy-from-azure-powershell"></a>Dahili yük dengeleyicisi şablonu dışa aktarma ve Azure PowerShell'den dağıtma

1. [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-2.5.0) komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin:
    
    ```azurepowershell-interactive
    Connect-AzAccount
    ```

2. Hedef bölgeye taşımak istediğiniz dahili yük dengeleyicisinin kaynak kimliğini edinin ve [Get-AzLoadBalancer](https://docs.microsoft.com/powershell/module/az.network/get-azloadbalancer?view=azps-2.6.0)kullanarak bir değişkene yerleştirin:

    ```azurepowershell-interactive
    $sourceIntLBID = (Get-AzLoadBalancer -Name <source-internal-lb-name> -ResourceGroupName <source-resource-group-name>).Id

    ```
3. Kaynak iç yük dengeleyici yapılandırmasını [Export-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/export-azresourcegroup?view=azps-2.6.0)komutunu uyguladığınız dizine .json dosyasına dışa aktarma:
   
   ```azurepowershell-interactive
   Export-AzResourceGroup -ResourceGroupName <source-resource-group-name> -Resource $sourceIntLBID -IncludeParameterDefaultValue
   ```
4. İndirilen dosya, kaynağın dışa aktarılan kaynak grubundan sonra adlandırılacaktır.  ** \<kaynak grubu adı>.json** adlı komuttan dışa aktarılan dosyayı bulun ve seçtiğiniz bir editörde açın:
   
   ```azurepowershell
   notepad.exe <source-resource-group-name>.json
   ```

5. İç yük bakiyesi adının parametresini değiştirmek için, kaynak iç yük bakiyesi adının özellik **varsayılan** değerini hedef iç yük bakiyeleyicinizin adıyla değiştirin, adın tırnak içinde olduğundan emin olun:

    ```json
         "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
         "contentVersion": "1.0.0.0",
         "parameters": {
            "loadBalancers_myLoadBalancer_name": {
            "defaultValue": "<target-external-lb-name>",
            "type": "String"
             },
            "virtualNetworks_myVNET2_externalid": {
             "defaultValue": "<target-vnet-resource-ID>",
             "type": "String"
             }
    ```
 
6. Yukarıda taşınan hedef sanal ağın değerini yeniden leştirmek için önce kaynak kimliğini edinmeniz ve ardından ** \<kaynak grubu adı>.json** dosyasına kopyalayıp yapıştırmanız gerekir.  Kimliği almak için [Get-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/get-azvirtualnetwork?view=azps-2.6.0)kullanın:
   
   ```azurepowershell-interactive
    $targetVNETID = (Get-AzVirtualNetwork -Name <target-vnet-name> -ResourceGroupName <target-resource-group-name>).Id
    ```
    Kaynak kimliğini görüntülemek için değişkeni ve isabet girini yazın.  Kimlik yolunu vurgulayın ve panoya kopyalayın:

    ```powershell
    PS C:\> $targetVNETID
    /subscriptions/7668d659-17fc-4ffd-85ba-9de61fe977e8/resourceGroups/myResourceGroupVNET-Move/providers/Microsoft.Network/virtualNetworks/myVNET2-Move
    ```

7.  Kaynak grubu adı>.json dosyasında, hedef sanal ağ kimliği için ikinci parametrede **varsayılan Değer** yerine kaynak **kimliğini** değişkenden yapıştırın ve yolu tırnak içine aldığınızdan emin olun: ** \<**
   
    ```json
         "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
         "contentVersion": "1.0.0.0",
         "parameters": {
            "loadBalancers_myLoadBalancer_name": {
            "defaultValue": "<target-external-lb-name>",
            "type": "String"
             },
            "virtualNetworks_myVNET2_externalid": {
             "defaultValue": "<target-vnet-resource-ID>",
             "type": "String"
             }
    ```

8. İç yük dengeleyici yapılandırmasının taşınacağı hedef bölgeyi düzenlemek için, kaynak ** \<grubu adı>.json** dosyasındaki **kaynakların** altındaki **konum** özelliğini değiştirin:

    ```json
        "resources": [
            {
                "type": "Microsoft.Network/loadBalancers",
                "apiVersion": "2019-06-01",
                "name": "[parameters('loadBalancers_myLoadBalancer_name')]",
                "location": "<target-internal-lb-region>",
                "sku": {
                    "name": "Standard",
                    "tier": "Regional"
                },
    ```

11. Bölge konum kodlarını elde etmek için aşağıdaki komutu çalıştırarak Azure PowerShell cmdlet [Get-AzLocation'ı](https://docs.microsoft.com/powershell/module/az.resources/get-azlocation?view=azps-1.8.0) kullanabilirsiniz:

    ```azurepowershell-interactive

    Get-AzLocation | format-table
    
    ```
12. İsterseniz şablondaki diğer parametreleri de değiştirebilirsiniz ve gereksinimlerinize bağlı olarak isteğe bağlıdır:
    
    * **Sku** - ** \<Kaynak grubu adı>.json** dosyasındaki **sku** > **adı** özelliğini değiştirerek yapılandırmadaki iç yük dengeleyicisinin sku'yu standarttan temele veya temelden standarda değiştirebilirsiniz:

        ```json
        "resources": [
        {
            "type": "Microsoft.Network/loadBalancers",
            "apiVersion": "2019-06-01",
            "name": "[parameters('loadBalancers_myLoadBalancer_name')]",
            "location": "<target-internal-lb-region>",
            "sku": {
                "name": "Standard",
                "tier": "Regional"
            },
        ```
      Temel ve standart sku yük dengeleyicileri arasındaki farklar hakkında daha fazla bilgi için Azure [Standart Yük Dengeleyicisi genel bakış](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview)

    * **Yük dengeleme kuralları** - ** \<Kaynak grubu adı>.json** dosyasının **loadBalancingRules** bölümüne girişler ekleyerek veya kaldırarak yapılandırmadaki yük dengeleme kurallarını ekleyebilir veya kaldırabilirsiniz:

        ```json
        "loadBalancingRules": [
                    {
                        "name": "myInboundRule",
                        "etag": "W/\"39e5e9cd-2d6d-491f-83cf-b37a259d86b6\"",
                        "properties": {
                            "provisioningState": "Succeeded",
                            "frontendIPConfiguration": {
                                "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/frontendIPConfigurations/myfrontendIPinbound')]"
                            },
                            "frontendPort": 80,
                            "backendPort": 80,
                            "enableFloatingIP": false,
                            "idleTimeoutInMinutes": 4,
                            "protocol": "Tcp",
                            "enableTcpReset": false,
                            "loadDistribution": "Default",
                            "disableOutboundSnat": true,
                            "backendAddressPool": {
                                "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/backendAddressPools/myBEPoolInbound')]"
                            },
                            "probe": {
                                "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/probes/myHTTPProbe')]"
                            }
                        }
                    }
                ]
        ```
       Yük dengeleme kuralları hakkında daha fazla bilgi için Azure [Yük Dengeleyicisi nedir?](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview)

    * **Problar** - ** \<Kaynak grubu adı>.json** dosyasının **sondalar** bölümüne girişler ekleyerek veya kaldırarak yapılandırmadaki yük dengeleyicisi için bir sonda ekleyebilir veya kaldırabilirsiniz:

        ```json
        "probes": [
                    {
                        "name": "myHTTPProbe",
                        "etag": "W/\"39e5e9cd-2d6d-491f-83cf-b37a259d86b6\"",
                        "properties": {
                            "provisioningState": "Succeeded",
                            "protocol": "Http",
                            "port": 80,
                            "requestPath": "/",
                            "intervalInSeconds": 15,
                            "numberOfProbes": 2
                        }
                    }
                ],
        ```
       Azure Yük Dengeleyici sistem sondaları hakkında daha fazla bilgi için [Bkz. Yük Dengeleyici sistem sondaları](https://docs.microsoft.com/azure/load-balancer/load-balancer-custom-probe-overview)

    * **Gelen NAT kuralları** - ** \<Kaynak grubu adı>.json** dosyasının **gelen NatRules** bölümüne girişler ekleyerek veya kaldırarak yük bakiyesi için gelen NAT kurallarıek veya kaldırabilirsiniz:

        ```json
        "inboundNatRules": [
                    {
                        "name": "myInboundNATRule",
                        "etag": "W/\"39e5e9cd-2d6d-491f-83cf-b37a259d86b6\"",
                        "properties": {
                            "provisioningState": "Succeeded",
                            "frontendIPConfiguration": {
                                "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/frontendIPConfigurations/myfrontendIPinbound')]"
                            },
                            "frontendPort": 4422,
                            "backendPort": 3389,
                            "enableFloatingIP": false,
                            "idleTimeoutInMinutes": 4,
                            "protocol": "Tcp",
                            "enableTcpReset": false
                        }
                    }
                ]
        ```
        Gelen NAT kuralının eklenmesi veya kaldırılmasıiçin, kural ** \<kaynak grubu adı>.json** dosyasının sonunda bir **tür** özelliği olarak bulunmalıdır:

        ```json
        {
            "type": "Microsoft.Network/loadBalancers/inboundNatRules",
            "apiVersion": "2019-06-01",
            "name": "[concat(parameters('loadBalancers_myLoadBalancer_name'), '/myInboundNATRule')]",
            "dependsOn": [
                "[resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name'))]"
            ],
            "properties": {
                "provisioningState": "Succeeded",
                "frontendIPConfiguration": {
                    "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/frontendIPConfigurations/myfrontendIPinbound')]"
                },
                "frontendPort": 4422,
                "backendPort": 3389,
                "enableFloatingIP": false,
                "idleTimeoutInMinutes": 4,
                "protocol": "Tcp",
                "enableTcpReset": false
            }
        }
        ```
        Gelen NAT kuralları hakkında daha fazla bilgi için Azure [Yük Dengeleyicisi nedir?](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview)
    
13. Kaynak ** \<grubu adı>.json** dosyasını kaydedin.
    
10. [Yeni-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup?view=azps-2.6.0)kullanılarak dağıtılacak hedef iç yük dengeleyicisi için hedef bölgede bir kaynak grubu oluşturun veya oluşturun. Yukarıdan gelen varolan kaynak grubu da bu işlemin bir parçası olarak yeniden kullanılabilir:
    
    ```azurepowershell-interactive
    New-AzResourceGroup -Name <target-resource-group-name> -location <target-region>
    ```
11. Düzenlenen ** \<kaynak grubu adı>.json** dosyasını [Yeni-AzResourceGroupDeployment](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroupdeployment?view=azps-2.6.0)kullanarak önceki adımda oluşturulan kaynak grubuna dağıtın:

    ```azurepowershell-interactive

    New-AzResourceGroupDeployment -ResourceGroupName <target-resource-group-name> -TemplateFile <source-resource-group-name>.json
    
    ```

12. Hedef bölgede oluşturulan kaynakları doğrulamak için [Get-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/get-azresourcegroup?view=azps-2.6.0) ve [Get-AzLoadBalancer'ı](https://docs.microsoft.com/powershell/module/az.network/get-azloadbalancer?view=azps-2.6.0)kullanın:
    
    ```azurepowershell-interactive

    Get-AzResourceGroup -Name <target-resource-group-name>

    ```

    ```azurepowershell-interactive

    Get-AzLoadBalancer -Name <target-publicip-name> -ResourceGroupName <target-resource-group-name>

    ```

## <a name="discard"></a>Vazgeç 

Dağıtımdan sonra, hedefteki sanal ağı ve yük bakiyesini yeniden başlatmak veya atmak isterseniz, hedefte oluşturulan kaynak grubunu silin ve taşınan sanal ağ ve yük bakiyeleyicisi silinir.  Kaynak grubunu kaldırmak için [Remove-AzResourceGroup'u](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup?view=azps-2.6.0)kullanın:

```azurepowershell-interactive

Remove-AzResourceGroup -Name <resource-group-name>

```

## <a name="clean-up"></a>Temizleme

Değişiklikleri işlemek ve NSG'nin hareketini tamamlamak için, kaynak NSG'yi veya kaynak grubunu silin, [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup?view=azps-2.6.0) veya [Remove-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/remove-azvirtualnetwork?view=azps-2.6.0) ve [Remove-AzLoadBalancer](https://docs.microsoft.com/powershell/module/az.network/remove-azloadbalancer?view=azps-2.6.0) kullanın

```azurepowershell-interactive

Remove-AzResourceGroup -Name <resource-group-name>

```

``` azurepowershell-interactive

Remove-AzLoadBalancer -name <load-balancer> -ResourceGroupName <resource-group-name>

Remove-AzVirtualNetwork -Name <virtual-network-name> -ResourceGroupName <resource-group-name>


```

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, bir Azure iç yük dengeleyicisini bir bölgeden diğerine taşıdınız ve kaynak kaynaklarını temizlediniz.  Azure'da kaynakları bölgeler arasında taşıma ve olağanüstü durum kurtarma hakkında daha fazla bilgi edinmek için şu na bakın:


- [Kaynakları yeni kaynak grubuna veya aboneliğe taşıma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)
- [Azure VM’lerini başka bir bölgeye taşıma](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-migrate)
