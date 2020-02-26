---
title: Hizmet kaynağınızı bölgeler arasında taşıma
titleSuffix: Azure Cognitive Search
description: Bu makalede, Azure Bilişsel Arama kaynaklarınızı Azure bulutundaki bir bölgeden diğerine nasıl taşıyacağınız gösterilmektedir.
manager: nitinme
author: tchristiani
ms.author: terrychr
ms.service: cognitive-search
ms.topic: how-to
ms.custom: subject-moving-resources
ms.date: 02/18/2020
ms.openlocfilehash: 392c86d8ea24e59d388926d4df581305ea2b531d
ms.sourcegitcommit: 99ac4a0150898ce9d3c6905cbd8b3a5537dd097e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77599307"
---
# <a name="move-your-azure-cognitive-search-service-to-another-azure-region"></a>Azure Bilişsel Arama hizmetinizi başka bir Azure bölgesine taşıyın

Azure bilişsel hizmet hesabınızı bir bölgeden diğerine taşımak için, aboneliklerinizi taşımak üzere bir dışarı aktarma şablonu oluşturacaksınız. Aboneliğinizi taşıdıktan sonra verilerinizi taşımanız ve hizmetinizi yeniden oluşturmanız gerekir.

Bu makalede aşağıdakileri nasıl yapacağınızı öğreneceksiniz:

> [!div class="checklist"]
> * Bir şablonu dışarı aktarın.
> * Şablonu değiştirme: hedef bölge, arama ve depolama hesabı adlarını ekleme.
> * Yeni arama ve depolama hesapları oluşturmak için şablonu dağıtın.
> * Yeni bölgede hizmet durumunuzu doğrulama
> * Kaynak bölgedeki kaynakları temizleyin.

## <a name="prerequisites"></a>Önkoşullar

- Hesabınızın kullandığı hizmet ve özelliklerin hedef bölgede desteklendiğinden emin olun.

- Önizleme özellikleri için, aboneliğinizin hedef bölge için beyaz listeye kaydedildiğinden emin olun. Önizleme özellikleri hakkında daha fazla bilgi için bkz. [bilgi depoları](https://docs.microsoft.com/azure/search/knowledge-store-concept-intro), [artımlı zenginleştirme](https://docs.microsoft.com/azure/search/cognitive-search-incremental-indexing-conceptual)ve [Özel uç nokta](https://docs.microsoft.com/azure/search/service-create-private-endpoint).

## <a name="assessment-and-planning"></a>Değerlendirme ve planlama

Arama hizmetinizi yeni bölgeye taşıdığınızda, [verilerinizi yeni depolama hizmetine taşımanız](https://docs.microsoft.com/azure/storage/common/storage-account-move?tabs=azure-portal#configure-the-new-storage-account) ve ardından dizinlerinizi, becerileri ve bilgi depolarını yeniden oluşturmanız gerekecektir. Hizmeti yeniden oluşturma işlemini daha kolay ve hızlı hale getirmek için geçerli ayarları kaydetmeniz ve JSON dosyalarını kopyalamanız gerekir.

## <a name="moving-your-search-services-resources"></a>Arama hizmetinizin kaynaklarını taşıma

Başlamak için Kaynak Yöneticisi şablonu dışarı ve sonra değiştirebilirsiniz.

### <a name="export-a-template"></a>Bir şablonu dışarı aktarma

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Kaynak grubu sayfanıza gidin.

> [!div class="mx-imgBorder"]
> ![kaynak grubu sayfası örneği](./media/search-move-resource/export-template-sample.png)

3. **Tüm kaynaklar**’ı seçin.

3. Sol taraftaki gezinti menüsünde **şablonu dışarı aktar**' ı seçin.

4. **Şablonu dışarı aktar** sayfasında **İndir** ' i seçin.

5. Portaldan indirdiğiniz. zip dosyasını bulun ve bu dosyayı seçtiğiniz bir klasöre ayıklayın.

ZIP dosyası, şablonu dağıtmak için şablon ve betikleri oluşturan. JSON dosyalarını içerir.

### <a name="modify-the-template"></a>Şablonu değiştirme

Arama ve depolama hesabı adlarını ve bölgelerini değiştirerek şablonu değiştirirsiniz. Adlar her hizmet ve bölge adlandırma kuralları için kurallara uymalıdır. 

Bölge konum kodlarını almak için bkz. [Azure konumları](https://azure.microsoft.com/global-infrastructure/locations/).  Bölge için kod, boşluk içermeyen bölge adıdır, **Orta ABD** = **merkezileştirme**.

1. Azure portalda **Kaynak oluştur**’u seçin.

2. **Market içinde ara** alanına **şablon dağıtımı** yazın ve **ENTER** tuşuna basın.

3. **Şablon dağıtımı**'nı seçin.

4. **Oluştur**’u seçin.

5. **Düzenleyicide kendi şablonunuzu oluşturun**'u seçin.

6. **Dosya Yükle**' yi seçin ve ardından önceki bölümde indirdiğiniz ve sıkıştırmışın **Template. JSON** dosyasını yüklemek için yönergeleri izleyin.

7. **Template. JSON** dosyasında, arama ve depolama hesabı adlarının varsayılan değerini ayarlayarak hedef arama ve depolama hesaplarını adlandırın. 

8. Hem arama hem de depolama hizmetlerinizin hedef bölgesine **Template. JSON** dosyasındaki **Location** özelliğini düzenleyin. Bu örnekte, hedef bölge `centralus`olarak ayarlanır.

```json
},
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Search/searchServices",
            "apiVersion": "2015-08-19",
            "name": "[parameters('searchServices_target_region_search_name')]",
            "location": "centralus",
            "sku": {
                "name": "standard"
            },
            "properties": {
                "replicaCount": 1,
                "partitionCount": 1,
                "hostingMode": "Default"
            }
        },
        {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2019-06-01",
            "name": "[parameters('storageAccounts_tagetstorageregion_name')]",
            "location": "centralus",
            "sku": {
                "name": "Standard_RAGRS",
                "tier": "Standard"
            },
```

### <a name="deploy-the-template"></a>Şablonu dağıtma

1. **Template. JSON** dosyasını kaydedin.

2. Özellik değerlerini girin veya seçin:

- **Abonelik**: Bir Azure aboneliği seçin.

- **Kaynak grubu**: **Yeni Oluştur**'u seçin ve kaynak grubuna bir ad verin.

- **Konum**: bir Azure konumu seçin.

3. **Yukarıda belirtilen hüküm ve koşulları kabul ediyorum** onay kutusuna tıklayın ve ardından **satın al düğmesini seçin** .

## <a name="verifying-your-services-status-in-new-region"></a>Yeni bölgedeki hizmetlerinizin durumu doğrulanıyor

Taşımayı doğrulamak için yeni kaynak grubunu açın ve hizmetleriniz yeni bölge ile listelenir.

Verilerinizi kaynak bölgeinizden hedef bölgeye taşımak için lütfen bu makaleye, [verilerinizi yeni depolama hesabına taşımaya](https://docs.microsoft.com/azure/storage/common/storage-account-move?tabs=azure-portal#move-data-to-the-new-storage-account)yönelik yönergelere bakın.

## <a name="clean-up-resources-in-your-original-region"></a>Özgün bölgenizdeki Kaynakları Temizleme

Değişiklikleri uygulamak ve hizmet hesabınızın taşınmasını tamamlamak için kaynak hizmet hesabını silin.

## <a name="next-steps"></a>Sonraki adımlar

[Dizin oluşturma](https://docs.microsoft.com/azure/search/search-get-started-portal)

[Beceri oluşturma](https://docs.microsoft.com/azure/search/cognitive-search-quickstart-blob)

[Bilgi deposu oluşturma](https://docs.microsoft.com/azure/search/knowledge-store-create-portal)

