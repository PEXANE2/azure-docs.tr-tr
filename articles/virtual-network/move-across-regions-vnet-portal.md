---
title: Azure portalını kullanarak bir Azure sanal ağını başka bir Azure bölgesine taşıyın.
description: Kaynak Yöneticisi şablonu ve Azure portalı kullanarak bir Azure sanal ağını bir Azure bölgesinden diğerine taşıyın.
author: asudbring
ms.service: virtual-network
ms.topic: article
ms.date: 08/26/2019
ms.author: allensu
ms.openlocfilehash: c54725d9a947b0c912a822686d7b2cffe1a7b5c9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75640797"
---
# <a name="move-an-azure-virtual-network-to-another-region-by-using-the-azure-portal"></a>Azure portalını kullanarak Azure sanal ağını başka bir bölgeye taşıma

Varolan bir Azure sanal ağını bir bölgeden diğerine taşımak için çeşitli senaryolar vardır. Örneğin, varolan sanal ağınızla test etmek ve kullanılabilirlik için aynı yapılandırmaya sahip bir sanal ağ oluşturmak isteyebilirsiniz. Veya olağanüstü durum kurtarma planlamanızın bir parçası olarak bir üretim sanal ağını başka bir bölgeye taşımak isteyebilirsiniz.

Sanal ağın başka bir bölgeye taşınmasını tamamlamak için bir Azure Kaynak Yöneticisi şablonu kullanabilirsiniz. Bunu, sanal ağı şablona dışa aktararak, parametreleri hedef bölgeyle eşleşecek şekilde değiştirerek ve ardından şablonu yeni bölgeye dağıtarak yaparsınız. Kaynak Yöneticisi şablonları hakkında daha fazla bilgi için Bkz. [Hızlı Başlangıç: Azure portalını kullanarak Azure Kaynak Yöneticisi şablonları oluşturun ve dağıtın.](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal)


## <a name="prerequisites"></a>Ön koşullar

- Sanal ağınızın taşınmak istediğiniz Azure bölgesinde olduğundan emin olun.

- Sanal ağ dışa aktarmak ve başka bir bölgede sanal ağ oluşturmak için şablon dağıtmak için Ağ Katılımcısı rolüne veya daha yüksek olması gerekir.

- Sanal ağ eşlemeleri yeniden oluşturulmaz ve şablonda hala bulunurlarsa başarısız olurlar. Şablonu dışa aktarmadan önce, sanal ağ eşlerini kaldırmanız gerekir. Daha sonra sanal ağ hareket ettikten sonra bunları yeniden oluşturabilirsiniz.

- Kaynak ağ düzenini ve şu anda kullanmakta olduğunuz tüm kaynakları tanımlayın. Bu düzen, yük dengeleyicileri, ağ güvenlik gruplarını (NSG'ler) ve genel IP'leri içerir, ancak bunlarla sınırlı değildir.

- Azure aboneliğinizin hedef bölgede sanal ağlar oluşturmanıza olanak sağladığını doğrulayın. Gerekli kotayı etkinleştirmek için desteğe başvurun.

- Aboneliğinizin bu işlem için sanal ağların eklenmesini destekleyecek yeterli kaynağa sahip olduğundan emin olun. Daha fazla bilgi için bkz. [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#networking-limits).


## <a name="prepare-for-the-move"></a>Harekete hazırlanın
Bu bölümde, kaynak yöneticisi şablonu kullanarak sanal ağı taşımaya hazırlarsınız. Daha sonra Azure portalını kullanarak sanal ağı hedef bölgeye taşırsınız.

Sanal ağı dışa aktarmak ve Azure portalını kullanarak hedef sanal ağı dağıtmak için aşağıdakileri yapın:

1. [Azure portalında](https://portal.azure.com)oturum açın ve ardından **Kaynak Grupları'nı**seçin.
1. Kaynak sanal ağı içeren kaynak grubunu bulun ve sonra seçin.
1. **Ayarlar** > **Dışa Aktarma şablonunu**seçin.
1. **Dışa Aktarma şablonu** bölmesinde **Dağıt'ı**seçin.
1. Çevrimiçi düzenleyicinizdeki *parameters.json* dosyasını açmak için **Şablon** > **Edit parametrelerini**seçin.
1. Sanal ağ adının parametresini değiştirmek **için, parametreler**altındaki **değer** özelliğini değiştirin:

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

1. Düzenleyicide, düzenleyicideki kaynak sanal ağ adı değerini hedef sanal ağ için istediğiniz bir adla değiştirin. Adı tırnak işaretlerine eklediğinden emin olun.

1. Editörde **Kaydet'i** seçin.

1. çevrimiçi düzenleyicide *template.json* dosyasını açmak için **Şablon** > **Edit şablonu'nu**seçin.

1. Çevrimiçi düzenleyicide, sanal ağın taşınacağı hedef bölgeyi değiştirmek için kaynakların altındaki **konum** özelliğini **değiştirin:**

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

1. Bölge konum kodları edinmek için [Azure Konumları'na](https://azure.microsoft.com/global-infrastructure/locations/)bakın. Bir bölgenin kodu, boşluk suz bölge adıdır (örneğin, **Orta ABD** = **merkezi).**

1. (İsteğe bağlı) Gereksinimlerinize bağlı olarak şablondaki diğer parametreleri de değiştirebilirsiniz:

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

    * **Alt Net**: Şablonun **alt ağlar** bölümünü değiştirerek alt net adını ve alt net adres alanını değiştirebilir veya ekleyebilirsiniz. **Ad** özelliğini değiştirerek alt ağın adını değiştirebilirsiniz. Ve **adresPrefix** özelliğini değiştirerek alt net adres alanını değiştirebilirsiniz:

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

        *template.json* dosyasındaki adres önekisini değiştirmek için, iki yerde edin: önceki bölümdeki kodda ve aşağıdaki kodun **tür** bölümünde. Yukarıdaki bölümde koddaki **adresPrefix** özelliğiyle eşleşecek şekilde aşağıdaki koddaki **prefix** özelliğini değiştirin.

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

1. Çevrimiçi düzenleyicide **Kaydet'i**seçin.

1. Hedef sanal ağın dağıtılanacağı aboneliği seçmek için **Temel Abonelik'i** > **seçin.**

1. Hedef sanal ağın dağıtılacayacağı kaynak grubunu seçmek için **Temel Kaynak** > **grubu'** nu seçin. 

    Hedef sanal ağ için yeni bir kaynak grubu oluşturmanız gerekiyorsa, **yeni oluştur'u**seçin. Adın varolan sanal ağdaki kaynak kaynak grubu adı ile aynı olmadığından emin olun.

1. Temel **Konum** > **Konumunun** sanal ağın dağıtılmasını istediğiniz hedef konuma ayarlı olduğunu doğrulayın.

1. **Ayarlar**altında, adın parametreler düzenleyicisinde daha önce girdiğiniz adla eşleştiğini doğrulayın.

1. Şartlar **ve Koşullar** onay kutusunu seçin.

1. Hedef sanal ağı dağıtmak için **Satın Al'ı**seçin.

## <a name="delete-the-target-virtual-network"></a>Hedef sanal ağı silme

Hedef sanal ağı atmak için, hedef sanal ağı içeren kaynak grubunu silersiniz. Bunu yapmak için:
1. Azure portal panosunda kaynak grubunu seçin.
1. **Genel Bakış** bölmesinin üst kısmında **Sil'i**seçin.

## <a name="clean-up"></a>Temizleme

Değişiklikleri işlemek ve sanal ağ hareketini tamamlamak için kaynak sanal ağı veya kaynak grubunu silersiniz. Bunu yapmak için:
1. Azure portal panosunda sanal ağı veya kaynak grubunu seçin.
1. Her bölmenin üst kısmında **Sil'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, Azure portalını kullanarak bir Azure sanal ağını bir bölgeden diğerine taşıdınız ve ardından gereksiz kaynak kaynaklarını temizlediniz. Azure'da kaynakları bölgeler arasında taşıma ve olağanüstü durum kurtarma hakkında daha fazla bilgi edinmek için bkz:


- [Kaynakları yeni kaynak grubuna veya aboneliğe taşıma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)
- [Azure sanal makinelerini başka bir bölgeye taşıma](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-migrate)
