---
title: Azure ağ güvenlik grubunu (NSG) Azure portalını kullanarak başka bir Azure bölgesine taşıyın
description: Azure ağ güvenlik grubunu Azure portalını kullanarak bir Azure bölgesinden diğerine taşımak için Azure Kaynak Yöneticisi şablonunu kullanın.
author: asudbring
ms.service: virtual-network
ms.topic: article
ms.date: 08/31/2019
ms.author: allensu
ms.openlocfilehash: dce267178c3caf813ccdcac4bba86ccfde3f3421
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75647195"
---
# <a name="move-azure-network-security-group-nsg-to-another-region-using-the-azure-portal"></a>Azure ağ güvenlik grubunu (NSG) Azure portalını kullanarak başka bir bölgeye taşıyın

Varolan NSG'lerinizi bir bölgeden diğerine taşımak istediğiniz çeşitli senaryolar vardır. Örneğin, sınama için aynı yapılandırma ve güvenlik kurallarına sahip bir NSG oluşturmak isteyebilirsiniz. Ayrıca, olağanüstü durum kurtarma planlamasının bir parçası olarak bir NSG'yi başka bir bölgeye taşımak isteyebilirsiniz.

Azure güvenlik grupları bir bölgeden diğerine taşınamaz. Ancak, bir NSG'nin varolan yapılandırma ve güvenlik kurallarını dışa aktarmak için bir Azure Kaynak Yöneticisi şablonu kullanabilirsiniz.  Daha sonra NSG'yi şablona dışa aktararak, parametreleri hedef bölgeyle eşleşecek şekilde değiştirerek ve ardından şablonu yeni bölgeye dağıtarak kaynağı başka bir bölgeye ayarlayabilirsiniz.  Kaynak Yöneticisi ve şablonlar hakkında daha fazla bilgi için [Bkz. Hızlı Başlangıç: Azure portalını kullanarak Azure Kaynak Yöneticisi şablonları oluşturun ve dağıtın.](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal)


## <a name="prerequisites"></a>Ön koşullar

- Azure ağ güvenlik grubunun taşımak istediğiniz Azure bölgesinde olduğundan emin olun.

- Azure ağ güvenlik grupları bölgeler arasında taşıtılamıyor.  Yeni NSG'yi hedef bölgedeki kaynaklarla ilişkilendirmeniz gerekir.

- Bir NSG yapılandırması dışa aktarmak ve başka bir bölgede NSG oluşturmak için bir şablon dağıtmak için Ağ Katılımcısı rolüne veya daha yükseğe ihtiyacınız vardır.

- Kaynak ağ düzenini ve şu anda kullanmakta olduğunuz tüm kaynakları tanımlayın. Bu düzen, yük dengeleyicileri, genel IP'leri ve sanal ağları içerir, ancak bunlarla sınırlı değildir.

- Azure aboneliğinizin, kullanılan hedef bölgede NSG oluşturmanıza izin verdiğini doğrulayın. Gerekli kotayı sağlamak için desteğe başvurun.

- Aboneliğinizin bu işlem için NSG eklenmesini destekleyecek yeterli kaynağa sahip olduğundan emin olun.  Bkz. [Azure aboneliği ve hizmet sınırları, kotalar ve kısıtlamalar](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#networking-limits).


## <a name="prepare-and-move"></a>Hazırlanın ve hareket edin
Aşağıdaki adımlar, Kaynak Yöneticisi şablonu kullanarak ağ güvenlik grubunu yapılandırma ve güvenlik kuralı hareketine nasıl hazırlayacağımı ve NSG yapılandırma ve güvenlik kurallarını portalı kullanarak hedef bölgeye nasıl taşıylayacağımı gösterir.


### <a name="export-the-template-and-deploy-from-the-portal"></a>Şablonu dışa aktarma ve portaldan dağıtma

1. [Azure portalı](https://portal.azure.com) > **Kaynak Gruplarına**giriş yapın.
2. Kaynak NSG'yi içeren Kaynak Grubunu bulun ve üzerine tıklayın.
3. **Ayarlar** > **> Verme şablonunu**seçin.
4. **Dışa Aktarma şablonu** bıçak'ta **Dağıt'ı** seçin.
5. Çevrimiçi düzenleyicideki **parametreleri.json** dosyasını açmak için **ŞABLON** > Parametrelerini**Edit'i** tıklatın.
6. NSG adının parametresini değiştirmek **için, parametreler**altında **değer** özelliğini değiştirin:

    ```json
            {
            "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
            "contentVersion": "1.0.0.0",
            "parameters": {
            "networkSecurityGroups_myVM1_nsg_name": {
               "value": "<target-nsg-name>"
                }
               }
            }
    ```

7. Editördeki kaynak NSG değerini hedef NSG için seçtiğiniz bir adla değiştirin. Adı tırnak içine aldığınızdan emin olun.

8.  Editörde **Kaydet'i** tıklatın.

9.  Çevrimiçi düzenleyicide **template.json** dosyasını açmak için **ŞABLON** > **Edit şablonuna** tıklayın.

10. NSG yapılandırmave güvenlik kurallarının taşınacağı hedef bölgeyi düzenlemek için, çevrimiçi düzenleyicideki **kaynaklar** altında **konum** özelliğini değiştirin:

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
           ]

    ```

11. Bölge konum kodları edinmek için [Azure Konumları'na](https://azure.microsoft.com/global-infrastructure/locations/)bakın.  Bir bölgenin kodu boşluksuz bölge adıdır, **Orta ABD** = **centralus.**

12. İsterseniz şablondaki diğer parametreleri de değiştirebilirsiniz ve gereksinimlerinize bağlı olarak isteğe bağlıdır:

    * **Güvenlik kuralları** - **template.json** dosyasındaki **güvenlik Kuralları** bölümüne kurallar ekleyerek veya kaldırarak hedef NSG'ye hangi kuralların dağıtılankuralları düzenleme yapabilirsiniz:

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
                    },
                ]
            }
        ```

      Hedef NSG'deki kuralların eklenmesini veya kaldırılmasını tamamlamak için, **template.json** dosyasının sonundaki özel kural türlerini aşağıdaki örnek biçiminde de ayarlamanız gerekir:

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

13. Çevrimiçi düzenleyicide **Kaydet'i** tıklatın.

14. Hedef NSG'nin dağıtılağan aboneliği seçmek için **BASICS** > **Aboneliği'ni** tıklatın.

15. Hedef **NSG'nin** > dağıtılacayacağı kaynak grubunu seçmek için BASICS**Kaynak grubunu** tıklatın.  Hedef NSG için yeni bir kaynak grubu oluşturmak için **yeni oluştur'u** tıklatabilirsiniz.  Adın varolan NSG'nin kaynak kaynak grubuyla aynı olmadığından emin olun.

16. **TEMELS** > **Konumunu** doğrulayın, NSG'nin dağıtılmasını istediğiniz hedef konuma ayarlanır.

17. **SETTINGS** altında, adın yukarıdaki parametre düzenleyicisinde girdiğiniz adla eşleştiğini doğrulayın.

18. ŞARTLAR VE **KOŞULLAR**altında kutuyu işaretleyin.

19. Hedef ağ güvenlik grubunu dağıtmak için **Satın Alma** düğmesini tıklatın.

## <a name="discard"></a>Vazgeç

Hedef NSG'yi atmak istiyorsanız, hedef NSG'yi içeren kaynak grubunu silin.  Bunu yapmak için, portaldaki panonuzdaki kaynak **Delete** grubunu seçin ve genel bakış sayfasının üst kısmındasil'i seçin.

## <a name="clean-up"></a>Temizleme

Değişiklikleri işlemek ve NSG'nin hareketini tamamlamak için kaynak NSG'yi veya kaynak grubunu silin. Bunu yapmak için, portaldaki panonuzdan ağ güvenlik grubunu **Delete** veya kaynak grubunu seçin ve her sayfanın üst kısmındasil'i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, bir Azure ağ güvenlik grubunu bir bölgeden diğerine taşıdınız ve kaynak kaynaklarını temizlediniz.  Azure'da kaynakları bölgeler arasında taşıma ve olağanüstü durum kurtarma hakkında daha fazla bilgi edinmek için şu na bakın:


- [Kaynakları yeni kaynak grubuna veya aboneliğe taşıma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)
- [Azure VM’lerini başka bir bölgeye taşıma](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-migrate)
