---
title: Azure Event Grid sistem konularını başka bir bölgeye taşıyın
description: Bu makalede, Azure Event Grid sistem konularını bir bölgeden başka bir bölgeye nasıl taşıyacağınız gösterilmektedir.
ms.topic: how-to
ms.custom: subject-moving-resources
ms.date: 08/28/2020
ms.openlocfilehash: eb6029b206e7d47789371ee81e75c4e05c69ee65
ms.sourcegitcommit: 656c0c38cf550327a9ee10cc936029378bc7b5a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89087202"
---
# <a name="move-azure-event-grid-system-topics-to-another-region"></a>Azure Event Grid sistem konularını başka bir bölgeye taşıyın
Birkaç nedenden dolayı kaynaklarınızı başka bir bölgeye taşımak isteyebilirsiniz. Örneğin, yeni bir Azure bölgesinden yararlanarak iç ilke ve idare gereksinimlerini veya kapasite planlama gereksinimlerine yanıt olarak tanışın. 

Bu makalede ele alınan üst düzey adımlar aşağıda verilmiştir: 

- Azure Depolama hesabını ve onunla ilişkili sistem konusunu içeren kaynak grubunu bir Kaynak Yöneticisi şablonuna **dışarı aktarın** . Ayrıca, bir şablonu yalnızca sistem konusu için de dışa aktarabilirsiniz. Bu rotaya giderseniz, Sistem konusunu taşımadan önce Azure olay kaynağını (Bu örnekte, bir Azure Storage hesabı) diğer bölgeye taşımayı unutmayın. Daha sonra, sistem konusunun dışarıya aktarılmış şablonunda, hedef bölgedeki depolama hesabının dış KIMLIĞINI güncelleştirin. 
- **Modify the template** `endpointUrl` Özelliği, sistem konusuna abone olan bir Web kancasına işaret etmek üzere eklemek için şablonu değiştirin. Sistem konusu verildiğinde, aboneliği (Bu durumda bir Web kancası) şablona de aktarılabilir, ancak `endpointUrl` özellik dahil değildir. Bu nedenle, konuya abone olan uç noktayı işaret etmek için güncelleştirmeniz gerekir. Ayrıca, `location` özelliğin değerini yeni konum veya bölge olarak güncelleştirin. Diğer olay işleyici türleri için yalnızca konumu güncelleştirmeniz gerekir. 
- Hedef bölgeye **kaynak dağıtmak için şablonu kullanın** . Depolama hesabı ve hedef bölgede oluşturulacak sistem konusunun adlarını belirtirsiniz. 
- **Dağıtımı doğrulayın**. Hedef bölgedeki blob depolamaya bir dosya yüklerken Web kancasının çağrılmasını doğrulayın. 
- **Taşıma işleminin tamamlanabilmesi**için kaynak bölgeden kaynakları (olay kaynağı ve sistem konusu) silin. 

## <a name="prerequisites"></a>Önkoşullar
- [Hızlı başlangıç: Blob Depolama olaylarını, kaynak bölgesindeki Azure Portal Web uç noktasına yönlendirin](blob-event-quickstart-portal.md) . Bu adım **isteğe bağlıdır**. Bu makaledeki adımları test etmek için bunu yapın. Depolama hesabını App Service ve App Service planı 'ndan ayrı bir kaynak grubunda saklayın. 
- Event Grid hizmetinin hedef bölgede kullanılabildiğinden emin olun. [Bölgeye göre kullanılabilen ürünleri](https://azure.microsoft.com/global-infrastructure/services/?products=event-grid&regions=all)görüntüleyin.

## <a name="prepare"></a>Hazırlama
Başlamak için, sistem olay kaynağını (Azure depolama hesabı) ve ilişkili sistem konusunu içeren kaynak grubu için bir Kaynak Yöneticisi şablonu dışarı aktarın. 

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.
1. Sol menüdeki **kaynak grupları** ' nı seçin. Ardından, sistem konusunun oluşturulduğu olay kaynağını içeren kaynak grubunu seçin. Aşağıdaki örnekte, **Azure Storage** hesabıdır. Kaynak grubu, depolama hesabını ve onunla ilişkili sistem konusunu içerir. 

    :::image type="content" source="./media/move-system-topics-across-regions/resource-group-page.png" alt-text="Kaynak grubu sayfası":::        
3. Sol taraftaki menüde, **Ayarlar**altında **şablonu dışarı aktar** ' ı seçin ve ardından araç çubuğunda **İndir** ' i seçin. 

    :::image type="content" source="./media/move-system-topics-across-regions/export-template-menu.png" alt-text="Stroage hesabı-şablonu dışarı aktarma sayfası":::        
5. Portaldan indirdiğiniz **. zip** dosyasını bulun ve bu dosyayı seçtiğiniz bir klasöre ayıklayın. Bu ZIP dosyası Template ve Parameters JSON dosyalarını içerir. 
1. **template.js** seçtiğiniz bir düzenleyicide açın. 
1. Web kancası URL 'SI şablona aktarılmaz. Bu nedenle, aşağıdaki adımları uygulayın:
    1. Şablon dosyasında **Web kancası**için arama yapın. 
    1. **Özellikler** bölümünde, son satırın sonuna bir virgül ( `,` ) karakteri ekleyin. Bu örnekte, `"preferredBatchSizeInKilobytes": 64` . 
    1. `endpointUrl`Aşağıdaki örnekte gösterildiği gibi, özelliği Web kancası URL 'sine ayarlanmış olacak şekilde ekleyin. 

        ```json
        "destination": {
            "properties": {
                "maxEventsPerBatch": 1,
                "preferredBatchSizeInKilobytes": 64,
                "endpointUrl": "https://mysite.azurewebsites.net/api/updates"
            },
            "endpointType": "WebHook"
        }
        ```

        > [!NOTE]
        > Diğer olay işleyici türleri için, tüm özellikler şablona verilir. Özelliği yalnızca bir `location` sonraki adımda gösterildiği gibi hedef bölgeye güncelleştirmeniz gerekir. 
7. `location` **Depolama hesabı** kaynağı için hedef bölgeye veya konuma güncelleştirin. Konum kodlarını almak için bkz. [Azure konumları](https://azure.microsoft.com/global-infrastructure/locations/). Bir bölgenin kodu, boşluk içermeyen bölge adıdır, örneğin, `West US` eşittir `westus` .

    ```json
    "type": "Microsoft.Storage/storageAccounts",
    "apiVersion": "2019-06-01",
    "name": "[parameters('storageAccounts_spegridstorage080420_name')]",
    "location": "westus",
    ```
8. `location`Şablondaki **Sistem konu** kaynağı için güncelleştirme adımını tekrarlayın. 

    ```json
    "type": "Microsoft.EventGrid/systemTopics",
    "apiVersion": "2020-04-01-preview",
    "name": "[parameters('systemTopics_spegridsystopic080420_name')]",
    "location": "westus",
    ```
1. Şablonu **kaydedin** . 

## <a name="recreate"></a>Oluştur 
Hedef bölgedeki depolama hesabı için bir depolama hesabı ve bir sistem konusu oluşturmak üzere şablonu dağıtın. 

1. Azure portal **kaynak oluştur**' u seçin.
2. **Market 'Te ara**' te, **şablon dağıtımı**yazın ve ardından **ENTER**tuşuna basın.
3. **Şablon dağıtımı**seçin.
4. **Oluştur**’u seçin.
5. **Düzenleyicide kendi şablonunuzu oluşturun öğesini**seçin.
6. **Dosya Yükle**' yi seçin ve ardından son bölümde indirdiğiniz dosyanın **template.js** yüklemek için yönergeleri izleyin.
7. Şablonu kaydetmek için **Kaydet** ' i seçin. 
8. **Özel dağıtım** sayfasında, aşağıdaki adımları izleyin. 
    1. Bir Azure **aboneliği**seçin. 
    1. Hedef bölgede var olan bir kaynak grubunu seçin veya bir **kaynak grubu** oluşturun. 
    1. **Bölge**için hedef bölgeyi seçin. Var olan bir kaynak grubunu seçtiyseniz, bu ayar salt okunurdur.
    1. **Sistem konu adı**için, depolama hesabıyla ilişkilendirilecek sistem konusu için bir ad girin.  
    1. **Depolama hesabı adı**için, hedef bölgede oluşturulacak depolama hesabı için bir ad girin. 

        :::image type="content" source="./media/move-system-topics-across-regions/deploy-template.png" alt-text="Kaynak Yöneticisi şablonu dağıtma":::
    5. Sayfanın alt kısmındaki **gözden geçir + oluştur** ' u seçin. 
    1. **Gözden geçir + oluştur** sayfasında ayarları gözden geçirin ve **Oluştur**' u seçin. 

## <a name="verify"></a>Doğrulama
1. Dağıtım başarılı olduktan sonra **kaynak grubunu git**' i seçin. 
1. **Kaynak grubu** sayfasında, olay kaynağının (Bu örnekte, Azure depolama hesabı) ve sistem konusunun oluşturulduğunu doğrulayın. 
1. Azure Blob depolama alanındaki bir kapsayıcıya dosya yükleyin ve Web kancasının olayı aldığını doğrulayın. Daha fazla bilgi için bkz. [uç noktanıza olay gönderme](blob-event-quickstart-portal.md#send-an-event-to-your-endpoint).

## <a name="discard-or-clean-up"></a>Atma veya temizleme
Taşıma işleminin tamamlanabilmesi için, depolama hesabını içeren kaynak grubunu ve kaynak bölgedeki ilişkili sistem konusunu silin.  

Baştan başlamak istiyorsanız, hedef bölgedeki kaynak grubunu silin ve bu makalenin [hazırlama](#prepare) ve [yeniden oluşturma](#recreate) bölümlerinde yer alan adımları yineleyin.

Azure portal kullanarak bir kaynak grubunu (kaynak veya hedef) silmek için:

1. Azure portal en üstündeki ara penceresinde, **kaynak grupları**yazın ve arama sonuçlarından **kaynak grupları** ' nı seçin. 
2. Silinecek kaynak grubunu seçin ve araç çubuğundan **Sil** ' i seçin. 

    :::image type="content" source="./media/move-system-topics-across-regions/delete-resource-group-button.png" alt-text="Kaynak grubunu silme":::
3. Onay sayfasında, kaynak grubunun adını girin ve **Sil**' i seçin.  

## <a name="next-steps"></a>Sonraki adımlar
Bir Azure olay kaynağını ve ilişkili sistem konusunu bir bölgeden başka bir bölgeye nasıl taşıyabileceğinizi öğrendiniz. Özel konuları, etki alanlarını ve iş ortağı ad alanlarını bölgeler arasında taşımak için aşağıdaki makalelere bakın.

- [Özel konuları bölgeler arasında taşıyın](move-custom-topics-across-regions.md). 
- [Etki alanlarını bölgeler arasında taşıyın](move-domains-across-regions.md). 
- [İş ortağı ad alanlarını bölgeler arasında taşıyın](move-partner-namespaces-across-regions.md). 

Azure 'da bölgeler ve olağanüstü durum kurtarma arasında kaynakları taşıma hakkında daha fazla bilgi edinmek için aşağıdaki makaleye bakın: [kaynakları yeni bir kaynak grubuna veya aboneliğe taşıma](../azure-resource-manager/management/move-resource-group-and-subscription.md).
