---
title: Azure dış Load Balancer Azure portal kullanarak başka bir Azure bölgesine taşıma
description: Azure portal kullanarak Azure dış Load Balancer bir Azure bölgesinden diğerine taşımak için Azure Resource Manager şablonu kullanın.
author: asudbring
ms.service: load-balancer
ms.topic: article
ms.date: 09/17/2019
ms.author: allensu
ms.openlocfilehash: eda0d6e8fe56b985c3b29fa80cee880444d63741
ms.sourcegitcommit: 1c9858eef5557a864a769c0a386d3c36ffc93ce4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71105302"
---
# <a name="move-azure-external-load-balancer-to-another-region-using-the-azure-portal"></a>Azure dış Load Balancer Azure portal kullanarak başka bir bölgeye taşıma

Mevcut dış yük dengeleyicinizi bir bölgeden diğerine taşımak istediğiniz çeşitli senaryolar vardır. Örneğin, test için aynı yapılandırmaya sahip bir dış yük dengeleyici oluşturmak isteyebilirsiniz. Ayrıca, olağanüstü durum kurtarma planlamasının bir parçası olarak bir dış yük dengeleyiciyi başka bir bölgeye taşımak isteyebilirsiniz.

Azure dış yük dengeleyiciler bir bölgeden diğerine taşınamaz. Ancak, bir dış yük dengeleyicinin mevcut yapılandırmasını ve genel IP 'sini dışarı aktarmak için bir Azure Resource Manager şablonu kullanabilirsiniz.  Daha sonra, yük dengeleyiciyi ve genel IP 'yi bir şablona dışarı aktararak ve parametreleri hedef bölgeyle eşleşecek şekilde değiştirerek ve sonra şablonları yeni bölgeye dağıtabilmeniz için kaynağı başka bir bölgede da oluşturabilirsiniz.  Kaynak Yöneticisi ve şablonlar hakkında daha fazla bilgi için bkz. [kaynak gruplarını şablonlara dışarı aktarma](https://docs.microsoft.com/azure/azure-resource-manager/manage-resource-groups-powershell#export-resource-groups-to-templates)


## <a name="prerequisites"></a>Önkoşullar

- Azure dış yük dengeleyicinin, taşımak istediğiniz Azure bölgesinde olduğundan emin olun.

- Azure dış yük dengeleyiciler bölgeler arasında taşınamaz.  Yeni Yük dengeleyiciyi hedef bölgedeki kaynaklarla ilişkilendirmeniz gerekir.

- Bir dış yük dengeleyici yapılandırmasını dışarı aktarmak ve başka bir bölgede dış yük dengeleyici oluşturmak üzere bir şablon dağıtmak için, ağ katılımcısı rolü veya daha yüksek bir sürüme sahip olmanız gerekir.
   
- Kaynak ağ düzeni ve şu anda kullanmakta olduğunuz tüm kaynakları belirler. Bu düzen, yük dengeleyiciler, ağ güvenlik grupları, genel IP 'Ler ve sanal ağlar dahil değildir ancak bunlarla sınırlı değildir.

- Azure aboneliğinizin, kullanılan hedef bölgede dış yük dengeleyiciler oluşturmanıza izin verdiğini doğrulayın. Gerekli kotayı sağlamak için desteğe başvurun.

- Aboneliğinizin bu işleme yönelik yük dengeleyiciler eklenmesini desteklemek için yeterli kaynağa sahip olduğundan emin olun.  Bkz. [Azure aboneliği ve hizmet limitleri, Kotalar ve kısıtlamalar](https://docs.microsoft.com/azure/azure-subscription-service-limits#networking-limits)


## <a name="prepare-and-move"></a>Hazırlama ve taşıma
Aşağıdaki adımlarda, bir Kaynak Yöneticisi şablonu kullanarak taşıma için dış yük dengeleyiciyi hazırlama ve Azure portal kullanarak dış yük dengeleyici yapılandırmasını hedef bölgeye taşıma işlemleri gösterilmektedir.  Bu işlemin bir parçası olarak dış yük dengeleyicinin genel IP yapılandırmasının dahil olması gerekir ve dış yük dengeleyiciyi taşımadan önce önce yapılması gerekir.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

### <a name="export-the-public-ip-template-and-deploy-from-the-portal"></a>Genel IP şablonunu dışarı aktarma ve portaldan dağıtma

1. [Azure Portal](http://portal.azure.com) > **kaynak gruplarında**oturum açın.
2. Kaynak ortak IP 'yi içeren kaynak grubunu bulun ve üzerine tıklayın.
3. > **Ayarları** > **dışarı aktarma şablonu**' nu seçin.
4. **Şablonu dışarı aktar** dikey penceresinde **Dağıt** ' ı seçin.
5. **Şablon** > **düzenleme parametreleri** ' ne tıklayarak **Parameters. JSON** dosyasını çevrimiçi düzenleyicide açın.
8. Genel IP adının parametresini düzenlemek için, **parametre** > **değeri** altındaki özelliği kaynak genel IP adından hedef ortak IP 'nizin adına değiştirin, adın tırnak içinde olduğundan emin olun:

    ```json
            {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "publicIPAddresses_myVM1pubIP_name": {
            "value": "<target-publicip-name>"
              }
             }
            }

    ```

    Düzenleyicide **Kaydet** ' e tıklayın.

9.  Çevrimiçi düzenleyicide **Template. JSON** dosyasını açmak için **şablon** > **düzenleme** şablonu ' na tıklayın. 

10. Genel IP 'nin taşınacağı hedef bölgeyi düzenlemek için **kaynaklar**altındaki **Location** özelliğini değiştirin:

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
  
11. Bölge konum kodlarını almak için bkz. [Azure konumları](https://azure.microsoft.com/global-infrastructure/locations/).  Bir bölgenin kodu, alanı olmayan bölge adıdır, **Orta ABD** = **merkezileştirme**.
    
12. Ayrıca, isterseniz şablondaki diğer parametreleri değiştirebilir ve gereksinimlerinize bağlı olarak isteğe bağlıdır:

    * **SKU** - **şablon. JSON** dosyasındaki **SKU** > **adı** özelliğini değiştirerek, yapılandırmadaki genel IP 'yi standart iken Basic veya Basic 'e dönüştürebilirsiniz:

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

        Temel ve standart SKU genel IP 'leri arasındaki farklılıklar hakkında daha fazla bilgi için bkz. [genel IP adresi oluşturma, değiştirme veya silme](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address).

    * **Genel IP ayırma yöntemi** ve **boşta kalma zaman aşımı** -, **publicıpallocationmethod** özelliğini **dinamik** veya **statik** ile dinamik olarak değiştirerek şablonda bu seçeneklerin her ikisini de değiştirebilirsiniz. Boşta **kalma zaman aşımı özelliği,** istediğiniz tutara göre değiştirilebilir.  Varsayılan değer **4**' dir:

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
        
        ```

        Ayırma yöntemleri ve boşta kalma zaman aşımı değerleri hakkında daha fazla bilgi için bkz. [genel IP adresi oluşturma, değiştirme veya silme](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address).

 
13. Çevrimiçi düzenleyicide **Kaydet** ' e tıklayın.

14. Hedef genel IP 'nin dağıtılacağı aboneliği seçmek için **temel bilgiler** > **aboneliği** ' ne tıklayın.

15. Hedef genel IP 'nin dağıtılacağı kaynak grubunu seçmek için **temel bilgiler** > **kaynak grubu** ' na tıklayın.  Hedef genel IP 'si için yeni bir kaynak grubu oluşturmak için **Yeni oluştur** ' a tıklayabilirsiniz.  Adın mevcut kaynak genel IP 'nin kaynak kaynak grubuyla aynı olmadığından emin olun. 

16. Temel IP 'nin dağıtılmasını istediğiniz hedef konuma göre, **temel bilgilerin** > bir**konum** olarak ayarlandığını doğrulayın.

17. **Ayarların** , yukarıdaki parametreler düzenleyicisinde girdiğiniz adla eşleştiğini doğrulayın.

18. **Hüküm ve koşullar**altındaki kutuyu işaretleyin.

19. Hedef ortak IP 'yi dağıtmak için **satın al** düğmesine tıklayın.
20. Taşınan yük dengeleyici için giden NAT için kullanılan başka bir genel IP varsa, ikinci giden genel IP 'yi dışarı aktarmak ve hedef bölgeye dağıtmak için yukarıdaki adımları tekrarlayın.

### <a name="export-the-external-load-balancer-template-and-deploy-from-the-azure-portal"></a>Dış yük dengeleyici şablonunu dışarı aktarın ve Azure portal dağıtın

1. [Azure Portal](http://portal.azure.com) > **kaynak gruplarında**oturum açın.
2. Kaynak dış yük dengeleyiciyi içeren kaynak grubunu bulun ve üzerine tıklayın.
3. > **Ayarları** > **dışarı aktarma şablonu**' nu seçin.
4. **Şablonu dışarı aktar** dikey penceresinde **Dağıt** ' ı seçin.
5. **Şablon** > **düzenleme parametreleri** ' ne tıklayarak **Parameters. JSON** dosyasını çevrimiçi düzenleyicide açın.

5. Dış yük dengeleyici adının parametresini düzenlemek için, kaynak dış yük dengeleyici adının özellik **değerini** hedef dış yük dengeleyicinizin adıyla değiştirin, adın tırnak içinde olduğundan emin olun:

    ```json
       "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
       "contentVersion": "1.0.0.0",
       "parameters": {
          "loadBalancers_myLoadbalancer_ext_name": {
          "value": "<target-external-lb-name>"
    },
          "publicIPAddresses_myPubIP_in_externalid": {
          "value": "<target-publicIP-resource-ID>"
    },

    ```

6.  Yukarıda taşınan hedef genel IP 'nin değerini düzenlemek için, önce kaynak KIMLIĞINI edinmeniz ve sonra **Parameters. JSON** dosyasına kopyalamanız ve yapıştırmanız gerekir. KIMLIĞI almak için:
    
    1. Başka bir tarayıcı sekmesi veya penceresinde [Azure Portal](http://portal.azure.com) > **kaynak gruplarında** oturum açın.
    2. Yukarıdaki adımlardan taşınan ortak IP 'yi içeren hedef kaynak grubunu bulun ve üzerine tıklayın.
    3. > **Ayarları** > **özelliklerini**seçin.
    4. Sağ taraftaki dikey pencerede, **kaynak kimliğini** vurgulayın ve panoya kopyalayın.  Alternatif olarak, **kaynak kimliği** yolunun sağ tarafındaki **Panoya Kopyala** düğmesine tıklayabilirsiniz.
    5. Kaynak KIMLIĞI ' ni, diğer tarayıcı penceresinde veya sekmesinde bulunan **parametreleri Düzenle** Düzenleyicisi ' ne **değer** özelliğine yapıştırın:

        ```json
           ```json
           "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
           "contentVersion": "1.0.0.0",
           "parameters": {
              "loadBalancers_myLoadbalancer_ext_name": {
              "value": "<target-external-lb-name>"
        },
              "publicIPAddresses_myPubIP_in_externalid": {
              "value": "<target-publicIP-resource-ID>"
        },

        ```
    6. Çevrimiçi düzenleyicide **Kaydet** ' e tıklayın.
   

7.  Yük Dengeleyici için giden NAT ve giden kurallarını yapılandırdıysanız, giden genel IP 'nin dış KIMLIĞI için bu dosyada üçüncü bir giriş bulunur.  Giden genel IP 'nin KIMLIĞINI edinmek ve bu girişi **Parameters. JSON** dosyasına yapıştırmak için **hedef bölgede** yukarıdaki adımları yineleyin:

    ```json
            "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
            "contentVersion": "1.0.0.0",
            "parameters": {
                "loadBalancers_myLoadbalancer_ext_name": {
                "value": "<target-external-lb-name>",
                
            },
                "publicIPAddresses_myPubIP_in_externalid": {
                "value": "<target-publicIP-resource-ID>",
                
            },
                "publicIPAddresses_myPubIP_out_externalid": {
                "defaultValue": "<target-publicIP-outbound-resource-ID>",
                
            }
        },
    ```

8.  Çevrimiçi düzenleyicide **Template. JSON** dosyasını açmak için **şablon** > **düzenleme** şablonu ' na tıklayın.
9.  Dış yük dengeleyici yapılandırmasının taşınacağı hedef bölgeyi düzenlemek için, **Template. JSON** dosyasındaki **kaynaklar** altındaki **Location** özelliğini değiştirin:

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

10. Bölge konum kodlarını almak için bkz. [Azure konumları](https://azure.microsoft.com/global-infrastructure/locations/).  Bir bölgenin kodu, alanı olmayan bölge adıdır, **Orta ABD** = **merkezileştirme**.

11. Ayrıca, isterseniz şablondaki diğer parametreleri değiştirebilir ve gereksinimlerinize bağlı olarak isteğe bağlıdır:
    
    * **SKU** - **Template. JSON** dosyasındaki **SKU** > **adı** özelliğini değiştirerek, yapılandırmadaki dış yük dengeleyicinin SKU 'sunu standart iken temel veya temel olarak değiştirebilirsiniz:

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
      Temel ve standart SKU yük dengeleyiciler arasındaki farklar hakkında daha fazla bilgi için bkz. [Azure Standart Load Balancer genel bakış](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview)

    * **Yük Dengeleme kuralları** : **Template. JSON** dosyasının **loadBalancingRules** bölümüne girdi ekleyerek veya kaldırarak, yapılandırmaya Yük Dengeleme kuralları ekleyebilir veya kaldırabilirsiniz.

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
       Yük Dengeleme kuralları hakkında daha fazla bilgi için bkz. [Azure Load Balancer nedir?](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview)

    * **Yoklamalar** - **Template. JSON** dosyasının **yoklamalar** bölümüne girdi ekleyerek veya kaldırarak, yapılandırmadaki yük dengeleyici için bir araştırma ekleyebilir veya kaldırabilirsiniz:

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
       Azure Load Balancer sistem durumu araştırmaları hakkında daha fazla bilgi için bkz. [Load Balancer sistem durumu araştırmaları](https://docs.microsoft.com/azure/load-balancer/load-balancer-custom-probe-overview)

    * **Gelen NAT kuralları** - **Template. JSON** dosyasının **ınboundnatrules** bölümüne girdi ekleyerek veya kaldırarak yük dengeleyici için gelen NAT kuralları ekleyebilir veya kaldırabilirsiniz:

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
        Bir gelen NAT kuralı ekleme veya kaldırma işlemini gerçekleştirmek için, kural var olmalıdır veya **Template. JSON** dosyasının sonundaki bir **tür** özelliği olarak kaldırılmalıdır:

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
        Gelen NAT kuralları hakkında daha fazla bilgi için bkz. [Azure Load Balancer nedir?](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview)

    * **Giden kuralları** - **Template. JSON** dosyasındaki **outboundrules** özelliğini düzenleyerek, yapılandırmada giden kuralları ekleyebilir veya kaldırabilirsiniz:

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

         Giden kuralları hakkında daha fazla bilgi için bkz. [Load Balancer giden kuralları](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-rules-overview)

12. Çevrimiçi düzenleyicide **Kaydet** ' e tıklayın.
    
13. Hedef dış yük dengeleyicinin dağıtılacağı aboneliği seçmek için **temel bilgiler** > **aboneliği** ' ne tıklayın.

15. Hedef yük dengeleyicinin dağıtılacağı kaynak grubunu seçmek için **temel bilgiler** > **kaynak grubu** ' na tıklayın.  Hedef dış yük dengeleyici için yeni bir kaynak grubu oluşturmak için **Yeni oluştur** ' a tıklayabilirsiniz veya genel IP için yukarıda oluşturulan mevcut kaynak grubunu seçebilirsiniz.  Adın mevcut kaynak dış yük dengeleyicinin kaynak kaynak grubuyla aynı olmadığından emin olun. 

16. **Temel bilgilerin** >  **, dış** yük dengeleyicinin dağıtılmasını istediğiniz hedef konuma ayarlandığını doğrulayın.

17. **Ayarların** , yukarıdaki parametreler düzenleyicisinde girdiğiniz adla eşleştiğini doğrulayın.  Yapılandırma içindeki genel IP 'Ler için kaynak kimliklerinin doldurulduğunu doğrulayın.

18. **Hüküm ve koşullar**altındaki kutuyu işaretleyin.

19. Hedef ortak IP 'yi dağıtmak için **satın al** düğmesine tıklayın.

## <a name="discard"></a>At 

Hedef genel IP ve dış yük dengeleyiciyi atmak istiyorsanız, hedef genel IP ve dış yük dengeleyiciyi içeren kaynak grubunu silin.  Bunu yapmak için, portalda panodaki kaynak grubunu seçin ve genel bakış sayfasının en üstünde **Sil** ' i seçin.

## <a name="clean-up"></a>Temizleme

Değişiklikleri uygulamak ve genel IP ve dış yük dengeleyiciyi tamamlamak için kaynak genel IP ve dış yük dengeleyiciyi veya kaynak grubunu silin. Bunu yapmak için, portalda panodaki genel IP ve dış yük dengeleyiciyi veya kaynak grubunu seçin ve her sayfanın en üstünde **Sil** ' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir Azure dış yük dengeleyiciyi bir bölgeden diğerine taşımış ve kaynak kaynakları temizledi.  Azure 'da bölgeler ve olağanüstü durum kurtarma arasında kaynakları taşıma hakkında daha fazla bilgi edinmek için bkz:


- [Kaynakları yeni kaynak grubuna veya aboneliğe taşıma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)
- [Azure VM’lerini başka bir bölgeye taşıma](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-migrate)
