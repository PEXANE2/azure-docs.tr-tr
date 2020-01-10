---
title: Azure portal kullanarak Azure genel IP 'yi başka bir Azure bölgesine taşıma
description: Azure genel IP 'yi, Azure portal kullanarak bir Azure bölgesinden diğerine taşımak için Azure Resource Manager şablonu kullanın.
author: asudbring
ms.service: virtual-network
ms.topic: article
ms.date: 08/29/2019
ms.author: allensu
ms.openlocfilehash: 6d10265e8383b68ebe13c95d8b2a9632668e85da
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75641410"
---
# <a name="move-azure-public-ip-to-another-region-using-the-azure-portal"></a>Azure portal kullanarak Azure genel IP 'yi başka bir bölgeye taşıma

Mevcut Azure genel IP 'lerini bir bölgeden diğerine taşımak istediğiniz çeşitli senaryolar vardır. Örneğin, test için aynı yapılandırma ve SKU ile genel bir IP oluşturmak isteyebilirsiniz. Ayrıca, olağanüstü durum kurtarma planlamasının bir parçası olarak genel bir IP 'yi başka bir bölgeye taşımak isteyebilirsiniz.

Azure genel IP 'Leri bölgeye özeldir ve bir bölgeden diğerine taşınamaz. Bununla birlikte, genel bir IP 'nin var olan yapılandırmasını dışarı aktarmak için bir Azure Resource Manager şablonu kullanabilirsiniz.  Daha sonra, genel IP 'yi bir şablona dışarı aktararak, hedef bölgeyle eşleşecek parametreleri değiştirerek ve sonra şablonu yeni bölgeye dağıtabilmeniz için kaynağı başka bir bölgede da oluşturabilirsiniz.  Kaynak Yöneticisi ve şablonlar hakkında daha fazla bilgi için bkz. [hızlı başlangıç: Azure Portal kullanarak Azure Resource Manager şablonları oluşturma ve dağıtma](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal).


## <a name="prerequisites"></a>Ön koşullar

- Azure genel IP 'nin taşımak istediğiniz Azure bölgesinde olduğundan emin olun.

- Azure genel IP 'Leri bölgeler arasında taşınamaz.  Yeni genel IP 'yi hedef bölgedeki kaynaklarla ilişkilendirmeniz gerekir.

- Genel bir IP yapılandırmasını dışarı aktarmak ve başka bir bölgede genel IP oluşturmak üzere şablon dağıtmak için, ağ katılımcısı rolü veya daha yüksek bir sürümü gerekir.

- Kaynak ağ düzeni ve şu anda kullanmakta olduğunuz tüm kaynakları belirler. Bu düzen, yük dengeleyiciler, ağ güvenlik grupları (NSG 'ler) ve sanal ağlar dahil değildir ancak bunlarla sınırlı değildir.

- Azure aboneliğinizin, kullanılan hedef bölgede ortak IP 'Ler oluşturmanıza izin verdiğini doğrulayın. Gerekli kotayı sağlamak için desteğe başvurun.

- Aboneliğinizin bu işlem için genel IP 'lerin eklenmesini desteklemek için yeterli kaynağa sahip olduğundan emin olun.  Bkz. [Azure aboneliği ve hizmet sınırları, kotalar ve kısıtlamalar](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#networking-limits).


## <a name="prepare-and-move"></a>Hazırlama ve taşıma
Aşağıdaki adımlarda, bir Kaynak Yöneticisi şablonu kullanarak yapılandırma taşıma için genel IP 'nin nasıl hazırlanacağı ve Azure portal kullanarak genel IP yapılandırmasını hedef bölgeye nasıl taşıyacağınız gösterilmektedir.

### <a name="export-the-template-and-deploy-from-a-script"></a>Şablonu dışarı aktarma ve bir betikten dağıtma

1. [Azure portal](https://portal.azure.com) > **kaynak gruplarında**oturum açın.
2. Kaynak ortak IP 'yi içeren kaynak grubunu bulun ve üzerine tıklayın.
3. > **Ayarları** > **şablonu dışarı aktar**' ı seçin.
4. **Şablonu dışarı aktar** dikey penceresinde **Dağıt** ' ı seçin.
5. Çevrimiçi düzenleyicide **Parameters. JSON** dosyasını açmak için **şablon** > **parametreleri Düzenle** ' ye tıklayın.
8. Genel IP adının parametresini düzenlemek için kaynak genel IP adındaki **parametreler** > **DEĞERINI** hedef ortak IP 'nizin adına değiştirin, adın tırnak içinde olduğundan emin olun:

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
8.  Düzenleyicide **Kaydet** ' e tıklayın.

9.  Çevrimiçi düzenleyicide **Template. JSON** dosyasını açmak için **şablon > şablonu** **Düzenle** ' ye tıklayın.

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

11. Bölge konum kodlarını almak için bkz. [Azure konumları](https://azure.microsoft.com/global-infrastructure/locations/).  Bölge için kod, boşluk içermeyen bölge adıdır, **Orta ABD** = **merkezileştirme**.

12. Ayrıca, isterseniz şablondaki diğer parametreleri değiştirebilir ve gereksinimlerinize bağlı olarak isteğe bağlıdır:

    * **SKU** - **Template. json** dosyasındaki **SKU** > **adı** özelliğini değiştirerek, yapılandırmadaki genel IP 'yi standart iken Basic veya Basic 'e dönüştürebilirsiniz:

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

    * **Ortak IP ayırma yöntemi** ve **boşta kalma zaman aşımı** - **publicıpallocationmethod** özelliğini **dinamik** veya **statik** olarak **dinamik**olarak değiştirerek şablonda bu seçeneklerin her ikisini de değiştirebilirsiniz . Boşta **kalma zaman aşımı özelliği,** istediğiniz tutara göre değiştirilebilir.  Varsayılan değer **4**' dir:

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

14. Hedef genel IP 'nin dağıtılacağı aboneliği seçmek için > **temel bilgiler** **aboneliğine** tıklayın.

15. Hedef genel IP 'nin dağıtılacağı kaynak grubunu seçmek için **temel bilgiler** > **kaynak grubu** ' na tıklayın.  Hedef genel IP 'si için yeni bir kaynak grubu oluşturmak için **Yeni oluştur** ' a tıklayabilirsiniz.  Adın mevcut kaynak genel IP 'nin kaynak kaynak grubuyla aynı olmadığından emin olun.

16.  > **konumun** , genel IP 'nin dağıtılmasını istediğiniz hedef **konuma ayarlandığını doğrulayın** .

17. **Ayarların** , yukarıdaki parametreler düzenleyicisinde girdiğiniz adla eşleştiğini doğrulayın.

18. **Hüküm ve koşullar**altındaki kutuyu işaretleyin.

19. Hedef ortak IP 'yi dağıtmak için **satın al** düğmesine tıklayın.

## <a name="discard"></a>At

Hedef ortak IP 'yi atmak isterseniz, hedef ortak IP 'yi içeren kaynak grubunu silin.  Bunu yapmak için, portalda panodaki kaynak grubunu seçin ve genel bakış sayfasının en üstünde **Sil** ' i seçin.

## <a name="clean-up"></a>Temizleme

Değişiklikleri uygulamak ve genel IP 'nin taşınmasını tamamlamak için kaynak genel IP veya kaynak grubunu silin. Bunu yapmak için, portalda panodaki genel IP veya kaynak grubunu seçin ve her sayfanın en üstünde **Sil** ' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir Azure genel IP 'sini bir bölgeden diğerine taşımış ve kaynak kaynakları temizledi.  Azure 'da bölgeler ve olağanüstü durum kurtarma arasında kaynakları taşıma hakkında daha fazla bilgi edinmek için bkz:


- [Kaynakları yeni kaynak grubuna veya aboneliğe taşıma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)
- [Azure VM’lerini başka bir bölgeye taşıma](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-migrate)
