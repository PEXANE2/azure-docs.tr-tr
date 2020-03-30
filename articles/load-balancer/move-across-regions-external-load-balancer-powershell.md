---
title: Azure PowerShell'i kullanarak Azure harici Yük Bakiyesini başka bir Azure bölgesine taşıyın
description: Azure PowerShell'i kullanarak Azure harici Yük Bakiyeleyicisini bir Azure bölgesinden diğerine taşımak için Azure Kaynak Yöneticisi şablonunu kullanın.
author: asudbring
ms.service: load-balancer
ms.topic: article
ms.date: 09/17/2019
ms.author: allensu
ms.openlocfilehash: a24eb4608e7630d5b613751fa2120361eccd7672
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75644826"
---
# <a name="move-azure-external-load-balancer-to-another-region-using-azure-powershell"></a>Azure PowerShell'i kullanarak Azure harici Yük Bakiyesini başka bir bölgeye taşıyın

Varolan dış yük bakiyecinizi bir bölgeden diğerine taşımak istediğiniz çeşitli senaryolar vardır. Örneğin, sınama için aynı yapılandırmaya sahip harici bir yük dengeleyicisi oluşturmak isteyebilirsiniz. Ayrıca, olağanüstü durum kurtarma planlamasının bir parçası olarak harici bir yük dengeleyicisini başka bir bölgeye taşımak isteyebilirsiniz.

Azure dış yük dengeleyicileri bir bölgeden diğerine taşınamaz. Ancak, harici bir yük dengeleyicisinin varolan yapılandırmasını ve genel IP'sini dışa aktarmak için bir Azure Kaynak Yöneticisi şablonu kullanabilirsiniz.  Daha sonra, yük bakiyesi ve genel IP'yi şablona dışlayarak, parametreleri hedef bölgeyle eşleşecek şekilde değiştirerek ve ardından şablonları yeni bölgeye dağıtarak kaynağı başka bir bölgeye ayarlayabilirsiniz.  Kaynak Yöneticisi ve şablonlar hakkında daha fazla bilgi için kaynak [gruplarını şablonlara dışa aktarma](https://docs.microsoft.com/azure/azure-resource-manager/manage-resource-groups-powershell#export-resource-groups-to-templates)


## <a name="prerequisites"></a>Ön koşullar

- Azure dış yük bakiyeleyicisinin taşımak istediğiniz Azure bölgesinde olduğundan emin olun.

- Azure dış yük dengeleyicileri bölgeler arasında taşınama maktadır.  Yeni yük dengeleyicisini hedef bölgedeki kaynaklarla ilişkilendirmeniz gerekir.

- Harici yük dengeleyici yapılandırması dışa aktarmak ve başka bir bölgede harici yük dengeleyicisi oluşturmak için bir şablon dağıtmak için Ağ Katılımcısı rolüne veya daha yükseğe ihtiyacınız olacak.
   
- Kaynak ağ düzenini ve şu anda kullanmakta olduğunuz tüm kaynakları tanımlayın. Bu düzen yük dengeleyicileri, ağ güvenlik gruplarını, genel IP'leri ve sanal ağları içerir, ancak bunlarla sınırlı değildir.

- Azure aboneliğinizin hedef bölgede harici yük dengeleyicileri oluşturmanıza olanak sağladığını doğrulayın. Gerekli kotayı sağlamak için desteğe başvurun.

- Aboneliğinizin bu işlem için yük dengeleyicileri eklenmesini destekleyecek yeterli kaynağa sahip olduğundan emin olun.  Bkz. [Azure abonelik ve hizmet sınırları, kotalar ve kısıtlamalar](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#networking-limits)


## <a name="prepare-and-move"></a>Hazırlanın ve hareket edin
Aşağıdaki adımlar, Kaynak Yöneticisi şablonu kullanarak dış yük dengeleyicisinin taşımaya nasıl hazırlanacağını ve azure PowerShell kullanarak dış yük dengeleyici yapılandırmasını hedef bölgeye nasıl taşıyacağımı gösterir.  Bu işlemin bir parçası olarak, harici yük dengeleyicisinin genel IP yapılandırması dahil edilmeli ve harici yük dengeleyicisini hareket ettirmeden önce bana yapılmalıdır.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

### <a name="export-the-public-ip-template-and-deploy-from-azure-powershell"></a>Ortak IP şablonuna dışa aktarma ve Azure PowerShell'den dağıtma

1. [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-2.5.0) komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin:
    
    ```azurepowershell-interactive
    Connect-AzAccount
    ```
2. Hedef bölgeye taşımak istediğiniz genel IP'nin kaynak kimliğini alın ve [Get-AzPublicIPAddress](https://docs.microsoft.com/powershell/module/az.network/get-azpublicipaddress?view=azps-2.6.0)kullanarak bir değişkene yerleştirin:

    ```azurepowershell-interactive
    $sourcePubIPID = (Get-AzPublicIPaddress -Name <source-public-ip-name> -ResourceGroupName <source-resource-group-name>).Id

    ```
3. Kaynak genel IP'yi [Export-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/export-azresourcegroup?view=azps-2.6.0)komutunu çalıştırdığınız dizine .json dosyasına dışa aktarın:
   
   ```azurepowershell-interactive
   Export-AzResourceGroup -ResourceGroupName <source-resource-group-name> -Resource $sourceVNETID -IncludeParameterDefaultValue
   ```

4. İndirilen dosya, kaynağın dışa aktarılan kaynak grubundan sonra adlandırılacaktır.  ** \<kaynak grubu adı>.json** adlı komuttan dışa aktarılan dosyayı bulun ve seçtiğiniz bir editörde açın:
   
   ```azurepowershell
   notepad.exe <source-resource-group-name>.json
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

### <a name="export-the-external-load-balancer-template-and-deploy-from-azure-powershell"></a>Dış yük bakiyesi şablonu dışa aktarma ve Azure PowerShell'den dağıtma

1. [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-2.5.0) komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin:
    
    ```azurepowershell-interactive
    Connect-AzAccount
    ```

2. Hedef bölgeye taşımak istediğiniz harici yük dengeleyicisinin kaynak kimliğini edinin ve [Get-AzLoadBalancer](https://docs.microsoft.com/powershell/module/az.network/get-azloadbalancer?view=azps-2.6.0)kullanarak bir değişkene yerleştirin:

    ```azurepowershell-interactive
    $sourceExtLBID = (Get-AzLoadBalancer -Name <source-external-lb-name> -ResourceGroupName <source-resource-group-name>).Id

    ```
3. Kaynak dış yük dengeleyici yapılandırmasını [Export-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/export-azresourcegroup?view=azps-2.6.0)komutunu uyguladığınız dizine .json dosyasına dışa aktarma:
   
   ```azurepowershell-interactive
   Export-AzResourceGroup -ResourceGroupName <source-resource-group-name> -Resource $sourceExtLBID -IncludeParameterDefaultValue
   ```
4. İndirilen dosya, kaynağın dışa aktarılan kaynak grubundan sonra adlandırılacaktır.  ** \<kaynak grubu adı>.json** adlı komuttan dışa aktarılan dosyayı bulun ve seçtiğiniz bir editörde açın:
   
   ```azurepowershell
   notepad.exe <source-resource-group-name>.json
   ```

5. Harici yük dengeleyici adının parametresini değiştirmek için, kaynak dış yük bakiyesi adının özellik **varsayılan** değerini hedef dış yük bakiyeleyicinizin adıyla değiştirin, adın tırnak içinde olduğundan emin olun:

    ```json
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        "loadBalancers_myLoadbalancer_ext_name": {
        "defaultValue": "<target-external-lb-name>",
        "type": "String"
            },
        "publicIPAddresses_myPubIP_in_externalid": {
        "defaultValue": "<target-publicIP-resource-ID>",
        "type": "String"
            },

    ```

6.  Yukarıda taşınan hedef genel IP değerini yeniden leştirmek için önce kaynak kimliğini edinmeniz ve ardından ** \<kaynak grubu adı>.json** dosyasına kopyalayıp yapıştırmanız gerekir.  Kimliği almak için [Get-AzPublicIPAddress](https://docs.microsoft.com/powershell/module/az.network/get-azpublicipaddress?view=azps-2.6.0)adresini kullanın:

    ```azurepowershell-interactive
    $targetPubIPID = (Get-AzPublicIPaddress -Name <target-public-ip-name> -ResourceGroupName <target-resource-group-name>).Id
    ```
    Kaynak kimliğini görüntülemek için değişkeni ve isabet girini yazın.  Kimlik yolunu vurgulayın ve panoya kopyalayın:

    ```powershell
    PS C:\> $targetPubIPID
    /subscriptions/7668d659-17fc-4ffd-85ba-9de61fe977e8/resourceGroups/myResourceGroupLB-Move/providers/Microsoft.Network/publicIPAddresses/myPubIP-in-move
    ```

7.  Kaynak grubu adı>.json dosyasında, ortak IP dış kimliği için ikinci parametrede **varsayılan Değer** yerine kaynak **kimliğini** değişkenden yapıştırın, yolu tırnak içine aldığınızdan emin olun: ** \<**

    ```json
            "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
            "contentVersion": "1.0.0.0",
            "parameters": {
            "loadBalancers_myLoadbalancer_ext_name": {
            "defaultValue": "<target-external-lb-name>",
            "type": "String"
            },
            "publicIPAddresses_myPubIP_in_externalid": {
            "defaultValue": "<target-publicIP-resource-ID>",
            "type": "String"
            },

    ```

8.  Yük dengeleyicisi için giden NAT ve giden kuralları yapılandırmışsanız, bu dosyada giden genel IP'nin dış kimliği için üçüncü bir giriş bulunur.  Giden genel iP kimliğini almak için **hedef bölgede** yukarıdaki adımları yineleyin ve ** \<kaynak grubu adı>.json** dosyasına bu girişi yapıştırın:

    ```json
            "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
            "contentVersion": "1.0.0.0",
            "parameters": {
                "loadBalancers_myLoadbalancer_ext_name": {
                "defaultValue": "<target-external-lb-name>",
                "type": "String"
            },
                "publicIPAddresses_myPubIP_in_externalid": {
                "defaultValue": "<target-publicIP-resource-ID>",
                "type": "String"
            },
                "publicIPAddresses_myPubIP_out_externalid": {
                "defaultValue": "<target-publicIP-outbound-resource-ID>",
                "type": "String"
            }
        },
    ```

10. Dış yük dengeleyici yapılandırmasının taşınacağı hedef bölgeyi düzenlemek için, kaynak ** \<grubu adı>.json** dosyasındaki **kaynakların** altındaki **konum** özelliğini değiştirin:

    ```json
        "resources": [
            {
                "type": "Microsoft.Network/loadBalancers",
                "apiVersion": "2019-06-01",
                "name": "[parameters('loadBalancers_myLoadBalancer_name')]",
                "location": "<target-external-lb-region>",
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
    
    * **Sku** - ** \<Kaynak grubu adı>.json** dosyasındaki **sku** > **adı** özelliğini değiştirerek yapılandırmadaki harici yük dengeleyicisinin sku'yu standarttan temele veya temelden standarda değiştirebilirsiniz:

        ```json
        "resources": [
        {
            "type": "Microsoft.Network/loadBalancers",
            "apiVersion": "2019-06-01",
            "name": "[parameters('loadBalancers_myLoadBalancer_name')]",
            "location": "<target-external-lb-region>",
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

    * **Giden kurallar** - ** \<Kaynak grubu adı>.json** dosyasındaki **giden Kurallar** özelliğini düzenleyerek yapılandırmada giden kuralları ekleyebilir veya kaldırabilirsiniz:

        ```json
        "outboundRules": [
                    {
                        "name": "myOutboundRule",
                        "etag": "W/\"39e5e9cd-2d6d-491f-83cf-b37a259d86b6\"",
                        "properties": {
                            "provisioningState": "Succeeded",
                            "allocatedOutboundPorts": 10000,
                            "protocol": "All",
                            "enableTcpReset": false,
                            "idleTimeoutInMinutes": 15,
                            "backendAddressPool": {
                                "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/backendAddressPools/myBEPoolOutbound')]"
                            },
                            "frontendIPConfigurations": [
                                {
                                    "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/frontendIPConfigurations/myfrontendIPoutbound')]"
                                }
                            ]
                        }
                    }
                ]
        ```

         Giden kurallar hakkında daha fazla bilgi için [Load Balancer giden kurallarına](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-rules-overview) bakın

13. Kaynak ** \<grubu adı>.json** dosyasını kaydedin.
    
10. [Yeni-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup?view=azps-2.6.0)kullanılarak dağıtılacak hedef dış yük dengeleyicisi için hedef bölgede bir kaynak grubu oluşturun veya oluşturun. Yukarıdan gelen varolan kaynak grubu da bu işlemin bir parçası olarak yeniden kullanılabilir:
    
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

Dağıtımdan sonra, yeniden başlamak veya hedefteki genel IP ve yük bakiyesini atmak isterseniz, hedefte oluşturulan kaynak grubunu silin ve taşınan genel IP ve yük dengeleyicisi silinir.  Kaynak grubunu kaldırmak için [Remove-AzResourceGroup'u](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup?view=azps-2.6.0)kullanın:

```azurepowershell-interactive

Remove-AzResourceGroup -Name <resource-group-name>

```

## <a name="clean-up"></a>Temizleme

Değişiklikleri işlemek ve NSG'nin hareketini tamamlamak için, kaynak NSG'yi veya kaynak grubunu silmek için [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup?view=azps-2.6.0) veya [Remove-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/remove-azpublicipaddress?view=azps-2.6.0) ve [Remove-AzLoadBalancer'ı](https://docs.microsoft.com/powershell/module/az.network/remove-azloadbalancer?view=azps-2.6.0) kullanın

```azurepowershell-interactive

Remove-AzResourceGroup -Name <resource-group-name>

```

``` azurepowershell-interactive

Remove-AzLoadBalancer -name <load-balancer> -ResourceGroupName <resource-group-name>

Remove-AzPublicIpAddress -Name <public-ip> -ResourceGroupName <resource-group-name>


```

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, bir Azure ağ güvenlik grubunu bir bölgeden diğerine taşıdınız ve kaynak kaynaklarını temizlediniz.  Azure'da kaynakları bölgeler arasında taşıma ve olağanüstü durum kurtarma hakkında daha fazla bilgi edinmek için şu na bakın:


- [Kaynakları yeni kaynak grubuna veya aboneliğe taşıma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)
- [Azure VM’lerini başka bir bölgeye taşıma](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-migrate)
