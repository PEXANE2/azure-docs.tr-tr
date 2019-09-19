---
title: Azure portal kullanarak Azure sanal ağını başka bir Azure bölgesine taşıyın.
description: Azure sanal ağını Azure portal kullanarak bir Azure bölgesinden diğerine taşımak için Azure Resource Manager şablonu kullanın.
author: asudbring
ms.service: virtual-network
ms.topic: article
ms.date: 08/26/2019
ms.author: allensu
ms.openlocfilehash: 2a1ee358a6c97b721ec6f0da3eb70269239b0737
ms.sourcegitcommit: 8ef0a2ddaece5e7b2ac678a73b605b2073b76e88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71077678"
---
# <a name="move-azure-virtual-network-to-another-region-using-the-azure-portal"></a>Azure portal kullanarak Azure sanal ağını başka bir bölgeye taşıma

Mevcut Azure sanal ağlarınızı (VNet) bir bölgeden diğerine taşımak istediğiniz çeşitli senaryolar vardır. Örneğin, var olan sanal ağınızın sınanması ve kullanılabilirliği için aynı yapılandırmaya sahip bir sanal ağ oluşturmak isteyebilirsiniz. Ayrıca, olağanüstü durum kurtarma planlamasının bir parçası olarak bir üretim sanal ağını başka bir bölgeye taşımak isteyebilirsiniz.

Sanal ağı başka bir bölgeye taşımayı gerçekleştirmek için bir Azure Resource Manager şablonu kullanabilirsiniz. Bunu, sanal ağı bir şablona vererek, parametreleri hedef bölgeyle eşleşecek şekilde değiştirerek ve sonra şablonu yeni bölgeye dağıtarak yapabilirsiniz.  Kaynak Yöneticisi ve şablonlar hakkında daha fazla bilgi için bkz [. hızlı başlangıç: Azure portal kullanarak Azure Resource Manager şablonları oluşturun ve dağıtın](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal)


## <a name="prerequisites"></a>Önkoşullar

- Azure sanal ağının, taşımak istediğiniz Azure bölgesinde olduğundan emin olun.

- Bir sanal ağı dışa aktarmak ve başka bir bölgede bir sanal ağ oluşturmak için şablon dağıtmak için, ağ katılımcısı rolü veya daha yüksek bir sürümü gerekir.

- Sanal ağ eşlemeleri yeniden oluşturulmaz ve şablonda hala mevcutsa başarısız olur.  Şablonu dışarı aktarmadan önce sanal ağ eşlerini kaldırmalı ve ardından sanal ağ taşıdıktan sonra eşleri yeniden kurmalısınız.
    
- Kaynak ağ düzeni ve şu anda kullanmakta olduğunuz tüm kaynakları belirler. Bu düzen, yük dengeleyiciler, ağ güvenlik grupları (NSG 'Ler) ve genel IP 'Leri içerir ancak bunlarla sınırlı değildir.

- Azure aboneliğinizin, kullanılan hedef bölgede sanal ağlar oluşturmanıza izin verdiğini doğrulayın. Gerekli kotayı sağlamak için desteğe başvurun.

- Aboneliğinizin bu işleme yönelik sanal ağların eklenmesini desteklemek için yeterli kaynağa sahip olduğundan emin olun.  Bkz. [Azure aboneliği ve hizmet limitleri, Kotalar ve kısıtlamalar](https://docs.microsoft.com/azure/azure-subscription-service-limits#networking-limits)


## <a name="prepare-and-move"></a>Hazırlama ve taşıma
Aşağıdaki adımlarda, sanal ağın bir Kaynak Yöneticisi şablonu kullanarak taşıma için nasıl hazırlanacağı ve Portal kullanılarak sanal ağın hedef bölgeye taşınması gösterilmektedir.

### <a name="export-the-template-and-deploy-from-the-portal"></a>Şablonu dışarı aktarma ve portaldan dağıtma

1. [Azure Portal](http://portal.azure.com) > **kaynak gruplarında**oturum açın.
2. Kaynak sanal ağı içeren kaynak grubunu bulun ve üzerine tıklayın.
3. > **Ayarları** > **dışarı aktarma şablonu**' nu seçin.
4. **Şablonu dışarı aktar** dikey penceresinde **Dağıt** ' ı seçin.
5. **Şablon** > **düzenleme parametreleri** ' ne tıklayarak **Parameters. JSON** dosyasını çevrimiçi düzenleyicide açın.
6. Sanal ağ adının parametresini düzenlemek için, **Parametreler**altındaki **değer** özelliğini değiştirin:
    
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

8.  Düzenleyicide **Kaydet** ' e tıklayın.

9.  Çevrimiçi düzenleyicide **Template. JSON** dosyasını açmak için **şablon** > **düzenleme** şablonu ' na tıklayın. 

10. VNET 'in taşınacağı hedef bölgeyi düzenlemek için, çevrimiçi düzenleyicideki **kaynaklar** altındaki **Location** özelliğini değiştirin:

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
 
11. Bölge konum kodlarını almak için bkz. [Azure konumları](https://azure.microsoft.com/global-infrastructure/locations/).  Bir bölgenin kodu, alanı olmayan bölge adıdır, **Orta ABD** = **merkezileştirme**.
 
12. Ayrıca, isterseniz şablondaki diğer parametreleri değiştirebilir ve gereksinimlerinize bağlı olarak isteğe bağlıdır:
    
    * **Adres alanı** -sanal ağın adres alanı, **Resources** > **AddressSpace** bölümü değiştirilerek ve **Template. JSON** dosyasındaki **addresspredüzeltmelerinin** özelliği değiştirilerek kaydedilmeden önce değiştirilebilir:

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

    * **Alt ağ** -alt ağ adı ve alt ağ adres alanı, **Template. JSON** dosyasının **alt ağlar** bölümü değiştirilerek değiştirilebilir veya öğesine eklenebilir. Alt ağın adı, **ad** özelliğini değiştirerek değiştirilebilir. Alt ağ adres alanı, **Template. JSON** dosyasındaki **addresspredüzeltmesini** özelliğini değiştirerek değiştirilebilir:

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

         **Şablon. JSON** dosyasında, adres önekini değiştirmek için, yukarıda listelenen bölüm ve aşağıda listelenen **tür** bölümü olmak üzere iki yerde düzenlenmelidir.  **Addresspredüzeltmesini** özelliğini, yukarıdaki bir ile eşleşecek şekilde değiştirin:

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

13. Çevrimiçi düzenleyicide **Kaydet** ' e tıklayın.

14. Hedef VNET 'in dağıtılacağı aboneliği seçmek için **temel bilgiler** > **aboneliği** ' ne tıklayın.

15. Hedef VNET 'in dağıtılacağı kaynak grubunu seçmek için **temel bilgiler** > **kaynak grubu** ' na tıklayın.  Hedef sanal ağ için yeni bir kaynak grubu oluşturmak için **Yeni oluştur** ' a tıklayabilirsiniz.  Adın mevcut VNET 'in kaynak kaynak grubuyla aynı olmadığından emin olun. 

16. **Temel bilgilerin** >  **, sanal** ağın dağıtılmasını istediğiniz hedef konuma ayarlandığını doğrulayın.

17. **Ayarların** , yukarıdaki parametreler düzenleyicisinde girdiğiniz adla eşleştiğini doğrulayın.

18. **Hüküm ve koşullar**altındaki kutuyu işaretleyin.

19. Hedef sanal ağı dağıtmak için **satın al** düğmesine tıklayın.

## <a name="discard"></a>At 

Hedef sanal ağı atmak istiyorsanız hedef sanal ağı içeren kaynak grubunu silin.  Bunu yapmak için, portalda panodaki kaynak grubunu seçin ve genel bakış sayfasının en üstünde **Sil** ' i seçin.

## <a name="clean-up"></a>Temizleme

Değişiklikleri uygulamak ve sanal ağı taşımayı tamamlamak için kaynak sanal ağını veya kaynak grubunu silin. Bunu yapmak için, portalda panodaki sanal ağı veya kaynak grubunu seçin ve her sayfanın en üstünde **Sil** ' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir Azure sanal ağını bir bölgeden diğerine taşıdı ve kaynak kaynakları temizledi.  Azure 'da bölgeler ve olağanüstü durum kurtarma arasında kaynakları taşıma hakkında daha fazla bilgi edinmek için bkz:


- [Kaynakları yeni kaynak grubuna veya aboneliğe taşıma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)
- [Azure VM’lerini başka bir bölgeye taşıma](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-migrate)
