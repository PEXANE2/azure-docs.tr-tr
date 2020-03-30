---
title: Azure portalını kullanarak azure harici yük bakiyesini başka bir Azure bölgesine taşıma
description: Azure portalını kullanarak harici bir yük bakiyesini bir Azure bölgesinden diğerine taşımak için Bir Azure Kaynak Yöneticisi şablonu kullanın.
author: asudbring
ms.service: load-balancer
ms.topic: article
ms.date: 09/17/2019
ms.author: allensu
ms.openlocfilehash: 5cd5ce2635ce05c4d5962f12ddc3945342897ecd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75638536"
---
# <a name="move-an-external-load-balancer-to-another-region-by-using-the-azure-portal"></a>Azure portalını kullanarak harici yük bakiyesini başka bir bölgeye taşıma

Harici yük dengeleyicisini bir bölgeden diğerine taşımak istediğiniz çeşitli senaryolar vardır. Örneğin, sınama için aynı yapılandırmaya sahip başka bir harici yük dengeleyicisi oluşturmak isteyebilirsiniz. Ayrıca, olağanüstü durum kurtarma planlamasının bir parçası olarak harici bir yük dengeleyicisini başka bir bölgeye taşımak isteyebilirsiniz.

Gerçek anlamda, bir Azure dış yük bakiyesini bir bölgeden diğerine taşıyamazsınız. Ancak, harici bir yük dengeleyicisinin varolan yapılandırmasını ve genel IP adresini dışa aktarmak için bir Azure Kaynak Yöneticisi şablonu kullanabilirsiniz. Daha sonra, yük bakiyesi ve genel IP'yi şablona dışa aktararak, parametreleri hedef bölgeyle eşleşecek şekilde değiştirerek ve ardından şablonu yeni bölgeye dağıtarak kaynağı başka bir bölgeye ayarlayabilirsiniz. Kaynak Yöneticisi ve şablonlar hakkında daha fazla bilgi için [kaynak gruplarını şablonlara dışa aktar'a](https://docs.microsoft.com/azure/azure-resource-manager/manage-resource-groups-powershell#export-resource-groups-to-templates)bakın.


## <a name="prerequisites"></a>Ön koşullar

- Azure dış yük bakiyeleyicisinin taşımak istediğiniz Azure bölgesinde olduğundan emin olun.

- Azure dış yük dengeleyicileri bölgeler arasında taşınama maktadır. Yeni yük dengeleyicisini hedef bölgedeki kaynaklarla ilişkilendirmeniz gerekir.

- Harici yük dengeleyici yapılandırması dışa aktarmak ve başka bir bölgede harici yük dengeleyicisi oluşturmak için bir şablon dağıtmak için, Ağ Katılımcısı rolü veya daha yüksek olarak atanması gerekir.

- Kaynak ağ düzenini ve şu anda kullanmakta olduğunuz tüm kaynakları tanımlayın. Bu düzen yük dengeleyicileri, ağ güvenlik gruplarını, genel IP'leri ve sanal ağları içerir, ancak bunlarla sınırlı değildir.

- Azure aboneliğinizin hedef bölgede harici yük dengeleyicileri oluşturmanıza izin verdiğini doğrulayın. Gerekli kotayı sağlamak için desteğe başvurun.

- Aboneliğinizin yük dengeleyicilerinin eklenmesini destekleyecek yeterli kaynağa sahip olduğundan emin olun. Bkz. [Azure aboneliği ve hizmet sınırları, kotalar ve kısıtlamalar](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#networking-limits).

## <a name="prepare-and-move"></a>Hazırlanın ve hareket edin
Aşağıdaki yordamlar, Kaynak Yöneticisi şablonu kullanarak dış yük dengeleyicisinin taşımaya nasıl hazırlanacağını ve Azure portalını kullanarak dış yük dengeleyici yapılandırmasını hedef bölgeye nasıl taşıyacağımı gösterir. Öncelikle harici yük dengeleyicisinin genel IP yapılandırmasını dışa aktarmanız gerekir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

### <a name="export-the-public-ip-template-and-deploy-the-public-ip-from-the-portal"></a>Ortak IP şablonuna dışa aktarma ve ortak IP'yi portaldan dağıtma

1. [Azure portalda](https://portal.azure.com) oturum açın ve **Kaynak grupları**’nı seçin.
2. Kaynak ortak IP içeren kaynak grubunu bulun ve seçin.
3. **Ayarlar** > **Dışa Aktarma şablonunu**seçin.
4. **Dışa Aktarma şablonu**altında **Dağıt'ı** seçin.
5. Çevrimiçi düzenleyicideki parametreleri.json dosyasını açmak için **TEMPLATE** > **Edit parametrelerini** seçin.
8. Ortak IP adının parametresini değiştirmek için, **parametreler** altındaki **değer** özelliğini kaynak genel IP adından hedef ortak IP'nizin adıyla değiştirin. Adı tırnak işaretlerine ekedin.

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

    Editörde **Kaydet'i** seçin.

9.  Çevrimiçi düzenleyicide template.json dosyasını açmak için **ŞABLON** > **Edit** şablonu'nu seçin.

10. Genel IP'nin taşınacağı hedef bölgeyi değiştirmek için, kaynakların altındaki **konum** özelliğini **değiştirin:**

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
  
    Bölge konum kodlarını almak için [Azure konumlarına](https://azure.microsoft.com/global-infrastructure/locations/)bakın. Bir bölgenin kodu boşluk olmayan bölge adıdır. Örneğin, Orta ABD için kod **centralus**olduğunu.
    
12. Gereksinimlerinize bağlı olarak istediğiniz veya ihtiyacınız varsa şablondaki diğer parametreleri de değiştirebilirsiniz:

    * **SKU**. Template.json dosyasındaki **sku** altındaki **ad** özelliğini değiştirerek yapılandırmadaki genel IP'nin SKU'yu standarttan temele veya temelden standarda değiştirebilirsiniz:

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

        Temel ve standart SKU genel IP'leri arasındaki farklar hakkında bilgi için genel [bir IP adresi oluştur, değiştir veya silme](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address)ye bakın.

    * **Genel IP ayırma yöntemi** ve **Boşalma zaman acısı.** **PublicIPAllocationMethod** özelliğini **Dinamik'ten** **Statik'e** veya **Statik'ten** **Dinamik'e**değiştirerek genel IP ayırma yöntemini değiştirebilirsiniz. **Boşta kalanTimeoutInMinutes** özelliğini istenilen değere değiştirerek boşta kalan zaman azamanı değiştirebilirsiniz. Varsayılan **değer 4'dür.**

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

        Ayırma yöntemleri ve boşta zaman ayırma değerleri hakkında bilgi için [bkz.](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address)

 
13. Çevrimiçi düzenleyicide **Kaydet'i** seçin.

14. Hedef genel IP'nin dağıtılanacağı aboneliği seçmek için **BASICS** > **Aboneliği'ni** seçin.

15. Hedef genel IP'nin dağıtılacayacağı kaynak grubunu seçmek için **BASICS** > **Kaynak grubunu** seçin. Hedef genel IP için yeni bir kaynak grubu oluşturmak için **yeni oluştur'u** seçebilirsiniz. Adın varolan kaynak public IP'nin kaynak kaynak grubuyla aynı olmadığından emin olun.

16. **BASICS** > **Konumunun,** genel IP'nin dağıtılmasını istediğiniz hedef konuma ayarlı olduğunu doğrulayın.

17. **AYARLAR**altında, adın parametreler düzenleyicisinde daha önce girdiğiniz adla eşleştiğini doğrulayın.

18. ŞARTLAR **VE KOŞULLAR** onay kutusunu seçin.

19. Hedef genel IP'yi dağıtmak için **Satın Alma'yı** seçin.

20. Taşınan yük dengeleyicisi için giden NAT için kullanılan başka bir genel IP'niz varsa, ikinci giden genel IP'yi dışa aktarmak ve hedef bölgeye dağıtmak için önceki adımları yineleyin.

### <a name="export-the-external-load-balancer-template-and-deploy-the-load-balancer-from-the-azure-portal"></a>Dış yük dengeleyicisi şablonu dışa aktarma ve yük bakiyesini Azure portalından dağıtma

1. [Azure portalda](https://portal.azure.com) oturum açın ve **Kaynak grupları**’nı seçin.
2. Kaynak dış yük dengeleyicisini içeren kaynak grubunu bulun ve seçin.
3. **Ayarlar** > **Dışa Aktarma şablonunu**seçin.
4. **Dışa Aktarma şablonu**altında **Dağıt'ı** seçin.
5. Çevrimiçi düzenleyicideki parametreleri.json dosyasını açmak için **TEMPLATE** > **Edit parametrelerini** seçin.

5. Harici yük dengeleyici adının parametresini değiştirmek için, kaynak dış yük dengeleyici adının **değer** özelliğini hedef dış yük bakiyeleyicinizin adıyla değiştirin. Adı tırnak işaretlerine ekedin.

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

6.  Önceki adımlarda taşıdığınız hedef genel IP değerini yeniden leştirmek için önce kaynak kimliğini edinmeniz ve ardından parameters.json dosyasına yapıştırmanız gerekir. Kimliği almak için:

    1. Başka bir tarayıcı sekmesinde veya penceresinde, [Azure portalında](https://portal.azure.com) oturum açın ve **Kaynak gruplarını**seçin.
    2. Önceki adımlarda taşıdığınız genel IP'yi içeren hedef kaynak grubunu bulun. Kalem simgesini seçin.
    3. **Ayarlar** > **Özelliklerini**seçin.
    4. Sağdaki bıçakta **Kaynak Kimliğini** vurgulayın ve panoya kopyalayın. Alternatif olarak, **Kaynak Kimliği** yolunun sağındaki **panoya kopyayı** seçebilirsiniz.
    5. Kaynak kimliğini, diğer tarayıcı penceresinde veya sekmesinde açık olan **Parametreleri Edit** düzenleyicisindeki **değer** özelliğine yapıştırın:

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
    6. Çevrimiçi düzenleyicide **Kaydet'i** seçin.


7.  Yük dengeleyicisi için giden NAT ve giden kuralları yapılandırmışsanız, bu dosyada giden genel IP'nin dış kimliği için üçüncü bir giriş görürsünüz. Giden genel IP'nin kimliğini almak için **hedef bölgedeki** önceki adımları yineleyin. Bu kimliği parameters.json dosyasına yapıştırın:

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

8.  Çevrimiçi düzenleyicide template.json dosyasını açmak için **ŞABLON** > **Edit** şablonu'nu seçin.
9.  Dış yük dengeleyici yapılandırmasının taşınacağı hedef bölgeyi düzenlemek için template.json dosyasındaki **kaynakların** altındaki **konum** özelliğini değiştirin:

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

10. Bölge konum kodlarını almak için [Azure konumlarına](https://azure.microsoft.com/global-infrastructure/locations/)bakın. Bir bölgenin kodu boşluk olmayan bölge adıdır. Örneğin, Orta ABD için kod **centralus**olduğunu.

11. Gereksinimlerinize bağlı olarak istediğiniz veya ihtiyacınız varsa şablondaki diğer parametreleri de değiştirebilirsiniz:

    * **SKU**. Template.json dosyasındaki **sku** altındaki **ad** özelliğini değiştirerek yapılandırmadaki harici yük dengeleyicisinin SKU'yu standarttan temele veya temelden standarda değiştirebilirsiniz:

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
      Temel ve standart SKU yük dengeleyicileri arasındaki farklar hakkında bilgi için Azure [Standart Yük Dengeleyicisi'ne genel bakış](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview)bakın.

    * **Yük dengeleme kuralları**. template.json dosyasının **loadBalancingRules** bölümüne girişler ekleyerek veya kaldırarak yapılandırmadaki yük dengeleme kuralları ekleyebilir veya kaldırabilirsiniz:

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
       Yük dengeleme kuralları hakkında bilgi için azure [yük dengeleyicisi nedir?](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview)

    * **Sondalar.** template.json dosyasının **problar** bölümüne girişler ekleyerek veya kaldırarak yapılandırmadaki yük dengeleyicisi için bir sonda ekleyebilir veya kaldırabilirsiniz:

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
       Daha fazla bilgi için [Load Balancer sağlık sondalarına](https://docs.microsoft.com/azure/load-balancer/load-balancer-custom-probe-overview)bakın.

    * **Gelen NAT kuralları**. Template.json dosyasının gelen Kurallar bölümüne girişler ekleyerek veya kaldırarak yük bakiyesi için gelen **NAT** kuralları ekleyebilir veya kaldırabilirsiniz:

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
        Gelen NAT kuralının eklenmesi veya kaldırılmasıiçin, kural template.json dosyasının sonunda bir **tür** özelliği olarak bulunmalıdır veya kaldırılmalıdır:

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
        Gelen NAT kuralları hakkında bilgi için azure [yük bakiyesi nedir?](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview)

    * **Giden kurallar.** Template.json dosyasındaki **giden Kurallar** özelliğini düzenleyerek yapılandırmada giden kuralları ekleyebilir veya kaldırabilirsiniz:

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

         Daha fazla bilgi için [Load Balancer giden kurallara](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-rules-overview)bakın.

12. Çevrimiçi düzenleyicide **Kaydet'i** seçin.

13. Hedef dış yük bakiyeleyicisinin dağıtılanacağı aboneliği seçmek için **BASICS** > **Aboneliği'ni** seçin.

15. Hedef yük bakiyesinin dağıtılacayacağı kaynak grubunu seçmek için **BASICS** > **Kaynak grubu'** nu seçin. Hedef dış yük dengeleyicisi için yeni bir kaynak grubu oluşturmak için **yeni oluştur'u** seçebilirsiniz. Veya genel IP için daha önce oluşturduğunuz varolan kaynak grubunu seçebilirsiniz. Adın varolan kaynak dış yük bakiyesinin kaynak kaynak grubuyla aynı olmadığından emin olun.

16. **BASICS** > **Konumunun** harici yük dengeleyicisinin dağıtılmasını istediğiniz hedef konuma ayarlı olduğunu doğrulayın.

17. **AYARLAR**altında, adın parametreler düzenleyicisinde daha önce girdiğiniz adla eşleştiğini doğrulayın. Kaynak iDp'lerinin yapılandırmadaki tüm genel IP'ler için doldurulan kaynağın olduğundan doğrulayın.

18. ŞARTLAR **VE KOŞULLAR** onay kutusunu seçin.

19. Hedef genel IP'yi dağıtmak için **Satın Alma'yı** seçin.

## <a name="discard"></a>Vazgeç

Hedef genel IP'yi ve harici yük bakiyesini atmak istiyorsanız, bunları içeren kaynak grubunu silin. Bunu yapmak için, portaldaki panonuzdaki kaynak grubunu **Delete** seçin ve ardından genel bakış sayfasının üst kısmındasil'i seçin.

## <a name="clean-up"></a>Temizleme

Değişiklikleri işlemek ve genel IP ve harici yük dengeleyicisinin hareketini tamamlamak için, kaynak genel IP'yi ve harici yük bakiyeciyi veya kaynak grubunu silin. Bunu yapmak için, portaldaki panonuzdaki kaynak grubunu seçin ve ardından her sayfanın üst kısmında **sil'i** seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, bir Azure dış yük dengeleyicisini bir bölgeden diğerine taşıdınız ve kaynak kaynaklarını temizlediniz. Azure'da kaynakları bölgeler arasında taşıma ve olağanüstü durum kurtarma hakkında daha fazla bilgi edinmek için bkz:


- [Kaynakları yeni kaynak grubuna veya aboneliğe taşıma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)
- [Azure VM’lerini başka bir bölgeye taşıma](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-migrate)
