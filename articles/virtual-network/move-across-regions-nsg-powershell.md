---
title: Azure Ağ Güvenlik Grubu'nu (NSG) Azure PowerShell'i kullanarak başka bir Azure bölgesine taşıyın
description: Azure PowerShell'i kullanarak Azure ağ güvenlik grubunu bir Azure bölgesinden diğerine taşımak için Azure Kaynak Yöneticisi şablonunu kullanın.
author: asudbring
ms.service: virtual-network
ms.topic: article
ms.date: 08/31/2019
ms.author: allensu
ms.openlocfilehash: 0cbd8f61cb1b4cb8eae6b30625fb3039ff75adde
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75641522"
---
# <a name="move-azure-network-security-group-nsg-to-another-region-using-azure-powershell"></a>Azure Ağ Güvenlik Grubu'nu (NSG) Azure PowerShell'i kullanarak başka bir bölgeye taşıyın

Varolan NSG'lerinizi bir bölgeden diğerine taşımak istediğiniz çeşitli senaryolar vardır. Örneğin, sınama için aynı yapılandırma ve güvenlik kurallarına sahip bir NSG oluşturmak isteyebilirsiniz. Ayrıca, olağanüstü durum kurtarma planlamasının bir parçası olarak bir NSG'yi başka bir bölgeye taşımak isteyebilirsiniz.

Azure güvenlik grupları bir bölgeden diğerine taşınamaz. Ancak, bir NSG'nin varolan yapılandırma ve güvenlik kurallarını dışa aktarmak için bir Azure Kaynak Yöneticisi şablonu kullanabilirsiniz.  Daha sonra NSG'yi şablona dışa aktararak, parametreleri hedef bölgeyle eşleşecek şekilde değiştirerek ve ardından şablonu yeni bölgeye dağıtarak kaynağı başka bir bölgeye ayarlayabilirsiniz.  Kaynak Yöneticisi ve şablonlar hakkında daha fazla bilgi için [kaynak gruplarını şablonlara dışa aktar'a](https://docs.microsoft.com/azure/azure-resource-manager/manage-resource-groups-powershell#export-resource-groups-to-templates)bakın.


## <a name="prerequisites"></a>Ön koşullar

- Azure ağ güvenlik grubunun taşımak istediğiniz Azure bölgesinde olduğundan emin olun.

- Azure ağ güvenlik grupları bölgeler arasında taşıtılamıyor.  Yeni NSG'yi hedef bölgedeki kaynaklarla ilişkilendirmeniz gerekir.

- Bir NSG yapılandırması dışa aktarmak ve başka bir bölgede NSG oluşturmak için bir şablon dağıtmak için Ağ Katılımcısı rolüne veya daha yükseğe ihtiyacınız vardır.
   
- Kaynak ağ düzenini ve şu anda kullanmakta olduğunuz tüm kaynakları tanımlayın. Bu düzen, yük dengeleyicileri, genel IP'leri ve sanal ağları içerir, ancak bunlarla sınırlı değildir.

- Azure aboneliğinizin, kullanılan hedef bölgede NSG oluşturmanıza izin verdiğini doğrulayın. Gerekli kotayı sağlamak için desteğe başvurun.

- Aboneliğinizin bu işlem için NSG eklenmesini destekleyecek yeterli kaynağa sahip olduğundan emin olun.  Bkz. [Azure aboneliği ve hizmet sınırları, kotalar ve kısıtlamalar](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#networking-limits).


## <a name="prepare-and-move"></a>Hazırlanın ve hareket edin
Aşağıdaki adımlar, Kaynak Yöneticisi şablonu kullanarak ağ güvenlik grubunu yapılandırma ve güvenlik kuralı hareketine nasıl hazırlayacağımı ve NSG yapılandırma ve güvenlik kurallarını Azure PowerShell kullanarak hedef bölgeye nasıl taşıylayacağımı gösterir.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

### <a name="export-the-template-and-deploy-from-a-script"></a>Şablonu dışa aktarma ve komut dosyasından dağıtma

1. [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-2.5.0) komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin:
    
    ```azurepowershell-interactive
    Connect-AzAccount
    ```

2. Hedef bölgeye taşımak istediğiniz NSG'nin kaynak kimliğini edinin ve [Get-AzNetworkSecurityGroup'u](https://docs.microsoft.com/powershell/module/az.network/get-aznetworksecuritygroup?view=azps-2.6.0)kullanarak bir değişkene yerleştirin:

    ```azurepowershell-interactive
    $sourceNSGID = (Get-AzNetworkSecurityGroup -Name <source-nsg-name> -ResourceGroupName <source-resource-group-name>).Id

    ```
3. Kaynak NSG'yi [Export-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/export-azresourcegroup?view=azps-2.6.0)komutunu uyguladığınız dizine .json dosyasına dışa aktarın:
   
   ```azurepowershell-interactive
   Export-AzResourceGroup -ResourceGroupName <source-resource-group-name> -Resource $sourceNSGID -IncludeParameterDefaultValue
   ```

4. İndirilen dosya, kaynağın dışa aktarılan kaynak grubundan sonra adlandırılacaktır.  ** \<kaynak grubu adı>.json** adlı komuttan dışa aktarılan dosyayı bulun ve seçtiğiniz bir editörde açın:
   
   ```azurepowershell
   notepad <source-resource-group-name>.json
   ```

5. NSG adının parametresini değiştirmek için, kaynak NSG adının özellik **varsayılan değerini** hedef NSG'nin adıyla değiştirin, adın tırnak içinde olduğundan emin olun:
    
    ```json
            {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "networkSecurityGroups_myVM1_nsg_name": {
            "defaultValue": "<target-nsg-name>",
            "type": "String"
            }
        }

    ```


6. NSG yapılandırmave güvenlik kurallarının taşınacağı hedef bölgeyi düzenlemek için, kaynakların altındaki **konum** özelliğini **değiştirin:**

    ```json
            "resources": [
            {
            "type": "Microsoft.Network/networkSecurityGroups",
            "apiVersion": "2019-06-01",
            "name": "[parameters('networkSecurityGroups_myVM1_nsg_name')]",
            "location": "<target-region>",
            "properties": {
                "provisioningState": "Succeeded",
                "resourceGuid": "2c846acf-58c8-416d-be97-ccd00a4ccd78", 
             }
            }
    ```
  
7. Bölge konum kodlarını elde etmek için aşağıdaki komutu çalıştırarak Azure PowerShell cmdlet [Get-AzLocation'ı](https://docs.microsoft.com/powershell/module/az.resources/get-azlocation?view=azps-1.8.0) kullanabilirsiniz:

    ```azurepowershell-interactive

    Get-AzLocation | format-table
    
    ```
8. İsterseniz ** \<kaynak grubu adı>.json'daki** diğer parametreleri de değiştirebilirsiniz ve gereksinimlerinize bağlı olarak isteğe bağlı olarak aşağıdakileri yapabilirsiniz:

    * **Güvenlik kuralları** - ** \<Kaynak grubu adı>.json** dosyasındaki **güvenlik Kuralları** bölümüne kurallar ekleyerek veya kaldırarak hedef NSG'ye hangi kuralların dağıtılan kuralları düzenleme yapabilirsiniz:

        ```json
           "resources": [
                  {
                  "type": "Microsoft.Network/networkSecurityGroups",
                  "apiVersion": "2019-06-01",
                  "name": "[parameters('networkSecurityGroups_myVM1_nsg_name')]",
                  "location": "TARGET REGION",
                  "properties": {
                       "provisioningState": "Succeeded",
                       "resourceGuid": "2c846acf-58c8-416d-be97-ccd00a4ccd78",
                  "securityRules": [
                    {
                        "name": "RDP",
                        "etag": "W/\"c630c458-6b52-4202-8fd7-172b7ab49cf5\"",
                        "properties": {
                             "provisioningState": "Succeeded",
                             "protocol": "TCP",
                             "sourcePortRange": "*",
                             "destinationPortRange": "3389",
                             "sourceAddressPrefix": "*",
                             "destinationAddressPrefix": "*",
                             "access": "Allow",
                             "priority": 300,
                             "direction": "Inbound",
                             "sourcePortRanges": [],
                             "destinationPortRanges": [],
                             "sourceAddressPrefixes": [],
                             "destinationAddressPrefixes": []
                            }
                        ]
            }  
            
        ```

        Hedef NSG'deki kuralların eklenmesini veya kaldırılmasını tamamlamak için, ** \<kaynak grubu adı>.json** dosyasının sonundaki özel kural türlerini aşağıdaki örnek biçiminde de ayarlamanız gerekir:

        ```json
           {
            "type": "Microsoft.Network/networkSecurityGroups/securityRules",
            "apiVersion": "2019-06-01",
            "name": "[concat(parameters('networkSecurityGroups_myVM1_nsg_name'), '/Port_80')]",
            "dependsOn": [
                "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('networkSecurityGroups_myVM1_nsg_name'))]"
            ],
            "properties": {
                "provisioningState": "Succeeded",
                "protocol": "*",
                "sourcePortRange": "*",
                "destinationPortRange": "80",
                "sourceAddressPrefix": "*",
                "destinationAddressPrefix": "*",
                "access": "Allow",
                "priority": 310,
                "direction": "Inbound",
                "sourcePortRanges": [],
                "destinationPortRanges": [],
                "sourceAddressPrefixes": [],
                "destinationAddressPrefixes": []
            }
        ```

9. Kaynak ** \<grubu adı>.json** dosyasını kaydedin.

10. [Yeni-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup?view=azps-2.6.0)kullanarak dağıtılacak hedef NSG için hedef bölgede bir kaynak grubu oluşturun:
    
    ```azurepowershell-interactive
    New-AzResourceGroup -Name <target-resource-group-name> -location <target-region>
    ```
    
11. Düzenlenen ** \<kaynak grubu adı>.json** dosyasını [Yeni-AzResourceGroupDeployment](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroupdeployment?view=azps-2.6.0)kullanarak önceki adımda oluşturulan kaynak grubuna dağıtın:

    ```azurepowershell-interactive

    New-AzResourceGroupDeployment -ResourceGroupName <target-resource-group-name> -TemplateFile <source-resource-group-name>.json
    
    ```

12. Hedef bölgede oluşturulan kaynakları doğrulamak için [Get-AzResourceGroup ve Get-AzNetworkSecurityGroup'u](https://docs.microsoft.com/powershell/module/az.resources/get-azresourcegroup?view=azps-2.6.0) kullanın: [Get-AzNetworkSecurityGroup](https://docs.microsoft.com/powershell/module/az.network/get-aznetworksecuritygroup?view=azps-2.6.0)
    
    ```azurepowershell-interactive

    Get-AzResourceGroup -Name <target-resource-group-name>

    ```

    ```azurepowershell-interactive

    Get-AzNetworkSecurityGroup -Name <target-nsg-name> -ResourceGroupName <target-resource-group-name>

    ```

## <a name="discard"></a>Vazgeç 

Dağıtımdan sonra, hedefteki NSG'yi yeniden başlatmak veya atmak isterseniz, hedefte oluşturulan kaynak grubunu silin ve taşınan NSG silinir.  Kaynak grubunu kaldırmak için [Remove-AzResourceGroup'u](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup?view=azps-2.6.0)kullanın:

```azurepowershell-interactive

Remove-AzResourceGroup -Name <target-resource-group-name>

```

## <a name="clean-up"></a>Temizleme

Değişiklikleri işlemek ve NSG'nin hareketini tamamlamak için, kaynak NSG'yi veya kaynak grubunu silmek için [Remove-AzResourceGroup veya Remove-AzNetworkSecurityGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup?view=azps-2.6.0) kullanın: [Remove-AzNetworkSecurityGroup](https://docs.microsoft.com/powershell/module/az.network/remove-aznetworksecuritygroup?view=azps-2.6.0)

```azurepowershell-interactive

Remove-AzResourceGroup -Name <source-resource-group-name>

```

``` azurepowershell-interactive

Remove-AzNetworkSecurityGroup -Name <source-nsg-name> -ResourceGroupName <source-resource-group-name>

```

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, bir Azure ağ güvenlik grubunu bir bölgeden diğerine taşıdınız ve kaynak kaynaklarını temizlediniz.  Azure'da kaynakları bölgeler arasında taşıma ve olağanüstü durum kurtarma hakkında daha fazla bilgi edinmek için şu na bakın:


- [Kaynakları yeni kaynak grubuna veya aboneliğe taşıma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)
- [Azure VM’lerini başka bir bölgeye taşıma](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-migrate)
