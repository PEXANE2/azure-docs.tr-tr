---
title: Azure portalını kullanarak Azure dahili Yük Bakiyesini başka bir Azure bölgesine taşıyın
description: Azure portalını kullanarak Azure dahili Yük Bakiyeleyicisini bir Azure bölgesinden diğerine taşımak için Azure Kaynak Yöneticisi şablonunu kullanın
author: asudbring
ms.service: load-balancer
ms.topic: article
ms.date: 09/18/2019
ms.author: allensu
ms.openlocfilehash: f23923b9d847ef393ebd609eb5fbba530b1a07d6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75638818"
---
# <a name="move-azure-internal-load-balancer-to-another-region-using-the-azure-portal"></a>Azure portalını kullanarak Azure dahili Yük Bakiyesini başka bir bölgeye taşıyın

Varolan iç yük bakiyecinizi bir bölgeden diğerine taşımak istediğiniz çeşitli senaryolar vardır. Örneğin, sınama için aynı yapılandırmaya sahip bir dahili yük dengeleyicisi oluşturmak isteyebilirsiniz. Ayrıca, olağanüstü durum kurtarma planlamasının bir parçası olarak bir iç yük dengeleyicisini başka bir bölgeye taşımak isteyebilirsiniz.

Azure dahili yük dengeleyicileri bir bölgeden diğerine taşınamaz. Ancak, bir iç yük dengeleyicisinin varolan yapılandırmasını ve sanal ağını dışa aktarmak için bir Azure Kaynak Yöneticisi şablonu kullanabilirsiniz.  Daha sonra, yük bakiyesi ve sanal ağı şablona dışlayarak, parametreleri hedef bölgeyle eşleşecek şekilde değiştirerek ve ardından şablonları yeni bölgeye dağıtarak kaynağı başka bir bölgeye ayarlayabilirsiniz.  Kaynak Yöneticisi ve şablonlar hakkında daha fazla bilgi için [Bkz. Hızlı Başlangıç: Azure portalını kullanarak Azure Kaynak Yöneticisi şablonları oluşturun ve dağıtın.](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal)


## <a name="prerequisites"></a>Ön koşullar

- Azure dahili yük bakiyeleyicisinin taşımak istediğiniz Azure bölgesinde olduğundan emin olun.

- Azure dahili yük dengeleyicileri bölgeler arasında taşıt edilemez.  Yeni yük dengeleyicisini hedef bölgedeki kaynaklarla ilişkilendirmeniz gerekir.

- Bir dahili yük dengeleyici yapılandırması dışa aktarmak ve başka bir bölgede bir iç yük dengeleyicisi oluşturmak için bir şablon dağıtmak için, Ağ Katılımcısı rolü veya daha yüksek olması gerekir.

- Kaynak ağ düzenini ve şu anda kullanmakta olduğunuz tüm kaynakları tanımlayın. Bu düzen yük dengeleyicileri, ağ güvenlik grupları, sanal makineler ve sanal ağlar içerir, ancak bunlarla sınırlı değildir.

- Azure aboneliğinizin, kullanılan hedef bölgede dahili yük dengeleyicileri oluşturmanıza olanak sağladığını doğrulayın. Gerekli kotayı sağlamak için desteğe başvurun.

- Aboneliğinizin bu işlem için yük dengeleyicileri eklenmesini destekleyecek yeterli kaynağa sahip olduğundan emin olun.  Bkz. [Azure abonelik ve hizmet sınırları, kotalar ve kısıtlamalar](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#networking-limits)


## <a name="prepare-and-move"></a>Hazırlanın ve hareket edin
Aşağıdaki adımlar, Kaynak Yöneticisi şablonu kullanarak iç yük dengeleyicisinin taşımaya nasıl hazırlanacağını ve azure portalını kullanarak iç yük dengeleyici yapılandırmasını hedef bölgeye nasıl taşıyacağımı gösterir.  Bu işlemin bir parçası olarak, dahili yük dengeleyicisinin sanal ağ yapılandırması dahil edilmeli ve iç yük dengeleyicisini hareket ettirmeden önce yapılmalıdır.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

### <a name="export-the-virtual-network-template-and-deploy-from-the-azure-portal"></a>Sanal ağ şablonu dışa aktarma ve Azure portalından dağıtma

1. [Azure portalı](https://portal.azure.com) > **Kaynak Gruplarına**giriş yapın.
2. Kaynak sanal ağı içeren Kaynak Grubunu bulun ve üzerine tıklayın.
3. **Ayarlar** > **> Verme şablonunu**seçin.
4. **Dışa Aktarma şablonu** bıçak'ta **Dağıt'ı** seçin.
5. Çevrimiçi düzenleyicideki **parametreleri.json** dosyasını açmak için **ŞABLON** > Parametrelerini**Edit'i** tıklatın.
6. Sanal ağ adının parametresini değiştirmek **için, parametreler**altındaki **değer** özelliğini değiştirin:

    ```json
    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "virtualNetworks_myVNET1_name": {
                "value": "<target-virtual-network-name>"
            }
        }
    }
    ```
7. Düzenleyicideki kaynak sanal ağ adı değerini hedef VNET için seçtiğiniz bir adla değiştirin. Adı tırnak içine aldığınızdan emin olun.

8. Editörde **Kaydet'i** tıklatın.

9. Çevrimiçi düzenleyicide **template.json** dosyasını açmak için **ŞABLON** > **Edit şablonuna** tıklayın.

10. VNET'in taşınacağı hedef bölgeyi değiştirmek için kaynakların altındaki **konum** özelliğini değiştirin:

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

11. Bölge konum kodları edinmek için [Azure Konumları'na](https://azure.microsoft.com/global-infrastructure/locations/)bakın.  Bir bölgenin kodu boşluksuz bölge adıdır, **Orta ABD** = **centralus.**

12. İsterseniz **template.json** dosyasındaki diğer parametreleri de değiştirebilirsiniz ve gereksinimlerinize bağlı olarak isteğe bağlıdır:

    * **Adres Alanı** - VNET'in adres **alanı, kaynak** > **adresiSpace** bölümünü değiştirerek ve **template.json** dosyasındaki **adresPrefixes** özelliğini değiştirerek kaydetmeden önce değiştirilebilir:

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

    * **Subnet** - **template.json** dosyasının **alt ağları** bölümü değiştirilerek alt net adı ve alt net adresi alanı değiştirilebilir veya eklenebilir. Alt ağın **adı, ad** özelliği değiştirilerek değiştirilebilir. Alt net adres alanı **template.json** dosyasındaki **adresPrefix** özelliği değiştirilerek değiştirilebilir:

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

         **template.json** dosyasında, adres önekini değiştirmek için, yukarıda listelenen bölüm ve aşağıda listelenen **tür** bölümü olmak üzere iki yerde düzenlenmelidir.  Yukarıdakiyle eşleşecek şekilde **adresPrefix** özelliğini değiştirin:

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

13. Çevrimiçi düzenleyicide **Kaydet'i** tıklatın.

14. Hedef VNET'in dağıtılanacağı aboneliği seçmek için **BASICS** > **Aboneliği'ni** tıklatın.

15. Hedef VNET'in dağıtılacayacağı kaynak grubunu seçmek için **BASICS** > **Kaynak grubunu** tıklatın.  Hedef VNET için yeni bir kaynak grubu oluşturmak için **yeni oluştur'u** tıklatabilirsiniz.  Adın varolan VNET'in kaynak kaynak grubuyla aynı olmadığından emin olun.

16. **BASICS** > **Konumunu Doğrulayın,** VNET'in dağıtılmasını istediğiniz hedef konuma ayarlanır.

17. **SETTINGS** altında, adın yukarıdaki parametre düzenleyicisinde girdiğiniz adla eşleştiğini doğrulayın.

18. ŞARTLAR VE **KOŞULLAR**altında kutuyu işaretleyin.

19. Hedef sanal ağı dağıtmak için **Satın Alma** düğmesini tıklatın.

### <a name="export-the-internal-load-balancer-template-and-deploy-from-azure-powershell"></a>Dahili yük dengeleyicisi şablonu dışa aktarma ve Azure PowerShell'den dağıtma

1. [Azure portalı](https://portal.azure.com) > **Kaynak Gruplarına**giriş yapın.
2. Kaynak iç yük dengeleyicisini içeren Kaynak Grubunu bulun ve üzerine tıklayın.
3. **Ayarlar** > **> Verme şablonunu**seçin.
4. **Dışa Aktarma şablonu** bıçak'ta **Dağıt'ı** seçin.
5. Çevrimiçi düzenleyicideki **parametreleri.json** dosyasını açmak için **ŞABLON** > Parametrelerini**Edit'i** tıklatın.

6. İç yük bakiyesi adının parametresini değiştirmek için, kaynak iç yük bakiyesi adının özellik **varsayılan** değerini hedef iç yük bakiyeleyicinizin adıyla değiştirin, adın tırnak içinde olduğundan emin olun:

    ```json
         "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
         "contentVersion": "1.0.0.0",
         "parameters": {
            "loadBalancers_myLoadBalancer_name": {
            "defaultValue": "<target-internal-lb-name>",
            "type": "String"
             },
            "virtualNetworks_myVNET2_internalid": {
             "defaultValue": "<target-vnet-resource-ID>",
             "type": "String"
             }
    ```

6. Yukarıda taşınan hedef sanal ağın değerini yeniden leştirmek için önce kaynak kimliğini edinmeniz ve ardından **parametre.json** dosyasına kopyalayıp yapıştırmalısınız. Kimliği almak için:

    1. Başka bir tarayıcı sekmesinde veya penceresinde [Azure portalı](https://portal.azure.com) > **Kaynak Gruplarına** giriş yapın.
    2. Yukarıdaki adımlardan taşınan sanal ağı içeren hedef kaynak grubunu bulun ve üzerine tıklayın.
    3. **ayarlar** > >**Özelliklerini**seçin.
    4. Sağdaki bıçakta **Kaynak Kimliğini** vurgulayın ve panoya kopyalayın.  Alternatif olarak, **Kaynak Kimliği** yolunun sağındaki **pano** düğmesine kopyayı tıklatabilirsiniz.
    5. Kaynak kimliğini **varsayılanDeğer** özelliğine diğer tarayıcı penceresinde veya sekmesinde açık olan **Parametreleri Edit** düzenleyicisine yapıştırın:

        ```json
         "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
         "contentVersion": "1.0.0.0",
         "parameters": {
            "loadBalancers_myLoadBalancer_name": {
            "defaultValue": "<target-internal-lb-name>",
            "type": "String"
             },
            "virtualNetworks_myVNET2_internalid": {
             "defaultValue": "<target-vnet-resource-ID>",
             "type": "String"
             }
        ```
    6. Çevrimiçi düzenleyicide **Kaydet'i** tıklatın.

7. Çevrimiçi düzenleyicide **template.json** dosyasını açmak için **ŞABLON** > **Edit şablonuna** tıklayın.
8. İç yük dengeleyici yapılandırmasının taşınacağı hedef bölgeyi düzenlemek için **template.json** dosyasındaki **kaynakların** altındaki **konum** özelliğini değiştirin:

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

9.  Bölge konum kodları edinmek için [Azure Konumları'na](https://azure.microsoft.com/global-infrastructure/locations/)bakın.  Bir bölgenin kodu boşluksuz bölge adıdır, **Orta ABD** = **centralus.**

10. İsterseniz şablondaki diğer parametreleri de değiştirebilirsiniz ve gereksinimlerinize bağlı olarak isteğe bağlıdır:

    * **Sku** - **Template.json** dosyasındaki **sku** > **ad** özelliğini değiştirerek yapılandırmadaki iç yük dengeleyicisinin sku'yu standarttan temele veya temelden standarda değiştirebilirsiniz:

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

    * **Yük dengeleme kuralları** - **template.json** dosyasının **loadBalancingRules** bölümüne girişler ekleyerek veya kaldırarak yapılandırmadaki yük dengeleme kurallarını ekleyebilir veya kaldırabilirsiniz:

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

    * **Problar** - **Template.json** dosyasının **problar** bölümüne girişler ekleyerek veya kaldırarak yapılandırmadaki yük dengeleyicisi için bir sonda ekleyebilir veya kaldırabilirsiniz:

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

    * **Gelen NAT kuralları** - **template.json** dosyasının **gelen NatRules** bölümüne girişleri ekleyerek veya kaldırarak yük bakiyesi için gelen NAT kurallarıek veya kaldırabilirsiniz:

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
        Gelen NAT kuralının eklenmesi veya kaldırılmasıiçin, kural **template.json** dosyasının sonunda bir **tür** özelliği olarak bulunmalıdır veya kaldırılmalıdır:

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

12. Çevrimiçi düzenleyicide **Kaydet'i** tıklatın.

13. Hedef iç yük bakiyesinin dağıtılanacağı aboneliği seçmek için **BASICS** > **Aboneliği'ni** tıklatın.

15. Hedef yük bakiyesinin dağıtılacayacağı kaynak grubunu seçmek için **BASICS** > **Kaynak grubunu** tıklatın.  Hedef iç yük dengeleyicisi için yeni bir kaynak grubu oluşturmak için **yeni** oluştur'u tıklatabilir veya sanal ağ için yukarıda oluşturulan varolan kaynak grubunu seçebilirsiniz.  Adın varolan kaynak iç yük bakiyesinin kaynak kaynak grubuyla aynı olmadığından emin olun.

16. **TEMELS** > **Konumunu** doğrulayın, iç yük dengeleyicisinin dağıtılmasını istediğiniz hedef konuma ayarlanır.

17. **SETTINGS** altında, adın yukarıdaki parametre düzenleyicisinde girdiğiniz adla eşleştiğini doğrulayın.  Kaynak iD'lerinin yapılandırmadaki tüm sanal ağlar için doldurulan kaynağı doğrulayın.

18. ŞARTLAR VE **KOŞULLAR**altında kutuyu işaretleyin.

19. Hedef sanal ağı dağıtmak için **Satın Alma** düğmesini tıklatın.

## <a name="discard"></a>Vazgeç

Hedef sanal ağı ve dahili yük bakiyesini atmak istiyorsanız, hedef sanal ağı ve dahili yük bakiyesini içeren kaynak grubunu silin.  Bunu yapmak için, portaldaki panonuzdaki kaynak **Delete** grubunu seçin ve genel bakış sayfasının üst kısmındasil'i seçin.

## <a name="clean-up"></a>Temizleme

Değişiklikleri işlemek ve sanal ağ ve iç yük dengeleyicisinin hareketini tamamlamak için, kaynak sanal ağı ve dahili yük bakiyeciyi veya kaynak grubunu silin. Bunu yapmak için, portaldaki panonuzdan sanal ağı ve dahili yük dengeleyicisini veya kaynak grubunu seçin ve her sayfanın üst kısmında **Sil'i** seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, bir Azure iç yük dengeleyicisini bir bölgeden diğerine taşıdınız ve kaynak kaynaklarını temizlediniz.  Azure'da kaynakları bölgeler arasında taşıma ve olağanüstü durum kurtarma hakkında daha fazla bilgi edinmek için şu na bakın:


- [Kaynakları yeni kaynak grubuna veya aboneliğe taşıma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)
- [Azure VM’lerini başka bir bölgeye taşıma](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-migrate)
