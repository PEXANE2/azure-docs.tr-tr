---
title: Azure ağ güvenlik grubu (NSG) Azure portal kullanarak başka bir Azure bölgesine taşıma
description: Azure portal kullanarak Azure ağ güvenlik grubunu bir Azure bölgesinden diğerine taşımak için Azure Resource Manager şablonu kullanın.
author: asudbring
ms.service: virtual-network
ms.topic: article
ms.date: 08/31/2019
ms.author: allensu
ms.openlocfilehash: dce267178c3caf813ccdcac4bba86ccfde3f3421
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75647195"
---
# <a name="move-azure-network-security-group-nsg-to-another-region-using-the-azure-portal"></a>Azure ağ güvenlik grubu (NSG) Azure portal kullanarak başka bir bölgeye taşıma

Mevcut NSG 'lerinizi bir bölgeden diğerine taşımak istediğiniz çeşitli senaryolar vardır. Örneğin, test için aynı yapılandırma ve güvenlik kurallarına sahip bir NSG oluşturmak isteyebilirsiniz. Ayrıca, olağanüstü durum kurtarma planlamasının bir parçası olarak bir NSG 'yi başka bir bölgeye taşımak isteyebilirsiniz.

Azure Güvenlik grupları bir bölgeden diğerine taşınamaz. Bununla birlikte, bir NSG 'nin mevcut yapılandırma ve güvenlik kurallarını dışarı aktarmak için bir Azure Resource Manager şablonu kullanabilirsiniz.  Daha sonra, NSG 'yi bir şablona dışarı aktararak, parametreleri hedef bölgeyle eşleşecek şekilde değiştirerek ve sonra şablonu yeni bölgeye dağıtabilmeniz için kaynağı başka bir bölgede aşamalandırın.  Kaynak Yöneticisi ve şablonlar hakkında daha fazla bilgi için bkz. [hızlı başlangıç: Azure Portal kullanarak Azure Resource Manager şablonları oluşturma ve dağıtma](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal).


## <a name="prerequisites"></a>Ön koşullar

- Azure ağ güvenlik grubunun, taşımak istediğiniz Azure bölgesinde olduğundan emin olun.

- Azure ağ güvenlik grupları bölgeler arasında taşınamaz.  Yeni NSG 'yi hedef bölgedeki kaynaklarla ilişkilendirmeniz gerekir.

- Bir NSG yapılandırmasını dışarı aktarmak ve başka bir bölgede NSG oluşturmak üzere bir şablon dağıtmak için, ağ katılımcısı rolü veya daha yüksek bir sürümü gerekir.

- Kaynak ağ düzeni ve şu anda kullanmakta olduğunuz tüm kaynakları belirler. Bu düzen, yük dengeleyiciler, genel IP 'Ler ve sanal ağlar dahil değildir ancak bunlarla sınırlı değildir.

- Azure aboneliğinizin, kullanılan hedef bölgede NSG 'ler oluşturmanıza izin verdiğini doğrulayın. Gerekli kotayı sağlamak için desteğe başvurun.

- Aboneliğinizin bu işlem için NSG 'lerin eklenmesini desteklemek için yeterli kaynağa sahip olduğundan emin olun.  Bkz. [Azure aboneliği ve hizmet sınırları, kotalar ve kısıtlamalar](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#networking-limits).


## <a name="prepare-and-move"></a>Hazırlama ve taşıma
Aşağıdaki adımlar, yapılandırma ve güvenlik kuralı için ağ güvenlik grubunun nasıl hazırlanacağını bir Kaynak Yöneticisi şablonu kullanarak taşıma ve NSG yapılandırma ve güvenlik kurallarını Portal kullanarak hedef bölgeye taşıma işlemlerinin nasıl yapılacağını göstermektedir.


### <a name="export-the-template-and-deploy-from-the-portal"></a>Şablonu dışarı aktarma ve portaldan dağıtma

1. [Azure portal](https://portal.azure.com) > **kaynak gruplarında**oturum açın.
2. Kaynak NSG 'yi içeren kaynak grubunu bulun ve üzerine tıklayın.
3. > **Ayarları** > **şablonu dışarı aktar**' ı seçin.
4. **Şablonu dışarı aktar** dikey penceresinde **Dağıt** ' ı seçin.
5. Çevrimiçi düzenleyicide **Parameters. JSON** dosyasını açmak için **şablon** > **parametreleri Düzenle** ' ye tıklayın.
6. NSG adının parametresini düzenlemek için, **Parametreler**altındaki **Value** özelliğini değiştirin:

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

7. Düzenleyicideki kaynak NSG değerini hedef NSG için seçtiğiniz bir adla değiştirin. Adı tırnak içine aldığınızdan emin olun.

8.  Düzenleyicide **Kaydet** ' e tıklayın.

9.  Çevrimiçi düzenleyicide **Template. JSON** dosyasını açmak için **şablon > şablonu** **Düzenle** ' ye tıklayın.

10. NSG yapılandırma ve güvenlik kurallarının taşınacağı hedef bölgeyi düzenlemek için, çevrimiçi düzenleyicideki **kaynaklar** altındaki **Location** özelliğini değiştirin:

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

11. Bölge konum kodlarını almak için bkz. [Azure konumları](https://azure.microsoft.com/global-infrastructure/locations/).  Bölge için kod, boşluk içermeyen bölge adıdır, **Orta ABD** = **merkezileştirme**.

12. Ayrıca, isterseniz şablondaki diğer parametreleri değiştirebilir ve gereksinimlerinize bağlı olarak isteğe bağlıdır:

    * **Güvenlik kuralları** - **Template. JSON** dosyasındaki **SecurityRules** bölümüne kural ekleyerek veya kaldırarak hedef NSG 'ye dağıtılan kuralları düzenleyebilirsiniz:

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

      Hedef NSG 'deki kuralların eklenmesini veya kaldırılmasını bitirmek için, **şablon. JSON** dosyasının sonundaki özel kural türlerini aşağıdaki örnekte yer alarak da düzenlemeniz gerekir:

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

13. Çevrimiçi düzenleyicide **Kaydet** ' e tıklayın.

14. Hedef NSG 'nin dağıtılacağı aboneliği seçmek için > **temel bilgiler** **aboneliğine** tıklayın.

15. Hedef NSG 'nin dağıtılacağı kaynak grubunu seçmek için **temel bilgiler** > **kaynak grubu** ' na tıklayın.  Hedef NSG için yeni bir kaynak grubu oluşturmak için **Yeni oluştur** ' a tıklayabilirsiniz.  Adın mevcut NSG kaynak kaynak grubuyla aynı olmadığından emin olun.

16. **Temel** > **konumun** , NSG 'nin dağıtılmasını istediğiniz hedef konuma ayarlandığını doğrulayın.

17. **Ayarların** , yukarıdaki parametreler düzenleyicisinde girdiğiniz adla eşleştiğini doğrulayın.

18. **Hüküm ve koşullar**altındaki kutuyu işaretleyin.

19. Hedef ağ güvenlik grubunu dağıtmak için **satın al** düğmesine tıklayın.

## <a name="discard"></a>At

Hedef NSG 'yi atmak istiyorsanız hedef NSG 'yi içeren kaynak grubunu silin.  Bunu yapmak için, portalda panodaki kaynak grubunu seçin ve genel bakış sayfasının en üstünde **Sil** ' i seçin.

## <a name="clean-up"></a>Temizleme

Değişiklikleri uygulamak ve NSG 'nin taşınmasını tamamlamak için kaynak NSG 'yi veya kaynak grubunu silin. Bunu yapmak için, portalda panodaki ağ güvenlik grubunu veya kaynak grubunu seçin ve her sayfanın en üstünde **Sil** ' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir Azure ağ güvenlik grubunu bir bölgeden diğerine taşıdı ve kaynak kaynakları temizledi.  Azure 'da bölgeler ve olağanüstü durum kurtarma arasında kaynakları taşıma hakkında daha fazla bilgi edinmek için bkz:


- [Kaynakları yeni kaynak grubuna veya aboneliğe taşıma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)
- [Azure VM’lerini başka bir bölgeye taşıma](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-migrate)
