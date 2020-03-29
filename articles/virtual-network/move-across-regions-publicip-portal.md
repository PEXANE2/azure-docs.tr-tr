---
title: Azure portalını kullanarak Azure Genel IP'sini başka bir Azure bölgesine taşıyın
description: Azure genel ipini azure portalını kullanarak bir Azure bölgesinden diğerine taşımak için Azure Kaynak Yöneticisi şablonunu kullanın.
author: asudbring
ms.service: virtual-network
ms.topic: article
ms.date: 08/29/2019
ms.author: allensu
ms.openlocfilehash: 6d10265e8383b68ebe13c95d8b2a9632668e85da
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75641410"
---
# <a name="move-azure-public-ip-to-another-region-using-the-azure-portal"></a>Azure Genel IP'sini Azure portalını kullanarak başka bir bölgeye taşıyın

Varolan Azure Genel IP'lerinizi bir bölgeden diğerine taşımak istediğiniz çeşitli senaryolar vardır. Örneğin, aynı yapılandırmaya ve sku sınamak için ortak bir IP oluşturmak isteyebilirsiniz. Ayrıca, olağanüstü durum kurtarma planlamasının bir parçası olarak genel bir IP'yi başka bir bölgeye taşımak isteyebilirsiniz.

Azure Genel Ip'leri bölgeye özgür ve bir bölgeden diğerine taşınamamaktadır. Ancak, genel bir IP'nin varolan yapılandırmasını dışa aktarmak için bir Azure Kaynak Yöneticisi şablonu kullanabilirsiniz.  Daha sonra, ortak IP'yi şablona dışa aktararak, parametreleri hedef bölgeyle eşleşecek şekilde değiştirerek ve ardından şablonu yeni bölgeye dağıtarak kaynağı başka bir bölgeye ayarlayabilirsiniz.  Kaynak Yöneticisi ve şablonlar hakkında daha fazla bilgi için [Bkz. Hızlı Başlangıç: Azure portalını kullanarak Azure Kaynak Yöneticisi şablonları oluşturun ve dağıtın.](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal)


## <a name="prerequisites"></a>Ön koşullar

- Azure Genel IP'sinin taşımak istediğiniz Azure bölgesinde olduğundan emin olun.

- Azure Genel IP'leri bölgeler arasında taşınamamaktadır.  Yeni kamu ip'ini hedef bölgedeki kaynaklarla ilişkilendirmeniz gerekir.

- Ortak bir IP yapılandırması dışa aktarmak ve başka bir bölgede genel BIR IP oluşturmak için bir şablon dağıtmak için Ağ Katılımcısı rolüne veya daha yükseğe ihtiyacınız olur.

- Kaynak ağ düzenini ve şu anda kullanmakta olduğunuz tüm kaynakları tanımlayın. Bu düzen, yük dengeleyicileri, ağ güvenlik gruplarını (NSG'ler) ve sanal ağlar içerir, ancak bunlarla sınırlı değildir.

- Azure aboneliğinizin, kullanılan hedef bölgede herkese açık IP'ler oluşturmanıza olanak sağladığını doğrulayın. Gerekli kotayı sağlamak için desteğe başvurun.

- Aboneliğinizin bu işlem için genel IP'lerin eklenmesini destekleyecek yeterli kaynağa sahip olduğundan emin olun.  Bkz. [Azure aboneliği ve hizmet sınırları, kotalar ve kısıtlamalar](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#networking-limits).


## <a name="prepare-and-move"></a>Hazırlanın ve hareket edin
Aşağıdaki adımlar, kaynak yöneticisi şablonu kullanarak ortak IP'yi yapılandırma hareketine nasıl hazırlayacağımı ve ortak IP yapılandırmasını Azure portalını kullanarak hedef bölgeye nasıl taşıyarak göstereceğimizdir.

### <a name="export-the-template-and-deploy-from-a-script"></a>Şablonu dışa aktarma ve komut dosyasından dağıtma

1. [Azure portalı](https://portal.azure.com) > **Kaynak Gruplarına**giriş yapın.
2. Kaynak genel IP'yi içeren Kaynak Grubu'nu bulun ve üzerine tıklayın.
3. **Ayarlar** > **> Verme şablonunu**seçin.
4. **Dışa Aktarma şablonu** bıçak'ta **Dağıt'ı** seçin.
5. Çevrimiçi düzenleyicideki **parametreleri.json** dosyasını açmak için **ŞABLON** > Parametrelerini**Edit'i** tıklatın.
8. Ortak IP adının parametresini değiştirmek için, kaynak genel IP adından hedef genel IP adının **parametreleri** > **altındaki** özelliği değiştirin, adın tırnak içinde olduğundan emin olun:

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
8.  Editörde **Kaydet'i** tıklatın.

9.  Çevrimiçi düzenleyicide **template.json** dosyasını açmak için **ŞABLON** > **Edit şablonuna** tıklayın.

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

11. Bölge konum kodları edinmek için [Azure Konumları'na](https://azure.microsoft.com/global-infrastructure/locations/)bakın.  Bir bölgenin kodu boşluksuz bölge adıdır, **Orta ABD** = **centralus.**

12. İsterseniz şablondaki diğer parametreleri de değiştirebilirsiniz ve gereksinimlerinize bağlı olarak isteğe bağlıdır:

    * **Sku** - **Template.json** dosyasındaki **sku** > **ad** özelliğini değiştirerek yapılandırmadaki genel IP'nin sku'yu standarttan temele veya temelden standarda değiştirebilirsiniz:

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

        ```

        Ayırma yöntemleri ve boşta kalma zaman ayırma değerleri hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address)


13. Çevrimiçi düzenleyicide **Kaydet'i** tıklatın.

14. Hedef genel IP'nin dağıtılanacağı aboneliği seçmek için **BASICS** > **Aboneliği'ni** tıklatın.

15. Hedef genel IP'nin dağıtılacayacağı kaynak grubunu seçmek için **BASICS** > **Kaynak grubunu** tıklatın.  Hedef genel IP için yeni bir kaynak grubu oluşturmak için **yeni oluştur'u** tıklatabilirsiniz.  Adın varolan kaynak public IP'nin kaynak kaynak grubuyla aynı olmadığından emin olun.

16. **TEMELS** > **Konumunu** doğrulayın, genel IP'nin dağıtılmasını istediğiniz hedef konuma ayarlanır.

17. **SETTINGS** altında, adın yukarıdaki parametre düzenleyicisinde girdiğiniz adla eşleştiğini doğrulayın.

18. ŞARTLAR VE **KOŞULLAR**altında kutuyu işaretleyin.

19. Hedef genel IP'yi dağıtmak için **Satın Alma** düğmesini tıklatın.

## <a name="discard"></a>Vazgeç

Hedef genel IP'yi atmak istiyorsanız, hedef ortak IP'yi içeren kaynak grubunu silin.  Bunu yapmak için, portaldaki panonuzdaki kaynak **Delete** grubunu seçin ve genel bakış sayfasının üst kısmındasil'i seçin.

## <a name="clean-up"></a>Temizleme

Değişiklikleri işlemek ve genel IP'nin hareketini tamamlamak için kaynak ortak IP veya kaynak grubunu silin. Bunu yapmak için, portaldaki panonuzdan genel IP veya **Delete** kaynak grubunu seçin ve her sayfanın üst kısmındasil'i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, bir Azure Genel IP'sini bir bölgeden diğerine taşıdınız ve kaynak kaynaklarını temizlediniz.  Azure'da kaynakları bölgeler arasında taşıma ve olağanüstü durum kurtarma hakkında daha fazla bilgi edinmek için şu na bakın:


- [Kaynakları yeni kaynak grubuna veya aboneliğe taşıma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)
- [Azure VM’lerini başka bir bölgeye taşıma](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-migrate)
