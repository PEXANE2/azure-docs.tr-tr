---
title: Azure Event Hubs ad alanını başka bir bölgeye taşıma | Microsoft Docs
description: Bu makalede, Azure Event Hubs ad alanını geçerli bölgeden başka bir bölgeye nasıl taşıyacağınız gösterilmektedir.
ms.topic: how-to
ms.date: 09/01/2020
ms.openlocfilehash: b177c3916919e3d97325f9d8c6b6027c00cb476f
ms.sourcegitcommit: 3246e278d094f0ae435c2393ebf278914ec7b97b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89375202"
---
# <a name="move-an-azure-event-hubs-namespace-to-another-region"></a>Azure Event Hubs ad alanını başka bir bölgeye taşıma
Bu makalede, var olan bir Event Hubs ad alanı için Azure Resource Manager şablonu dışarı aktarma ve ardından şablonu kullanarak başka bir bölgede aynı yapılandırma ayarlarına sahip bir ad alanı oluşturma işlemi gösterilmektedir. Ancak, bu işlem henüz işlenmemiş olayları taşımaz. Silmeden önce özgün ad alanındaki olayları işlemek gerekir.
 
Azure Kaynak grubunda Event Hubs ad alanını içeren başka kaynaklarınız varsa, tüm ilgili kaynakların tek bir adımda yeni bölgeye taşınabilmesi için şablonu kaynak grubu düzeyinde dışarı aktarmak isteyebilirsiniz. Bu makaledeki adımlarda, şablona nasıl **ad alanı** dışarı aktarılacağı gösterilmektedir. Bir **kaynak grubunu** şablona aktarma adımları benzerdir. 

## <a name="prerequisites"></a>Ön koşullar

- Hesabınızın kullandığı hizmetlerin ve özelliklerin hedef bölgede desteklendiğinden emin olun.
- Ad alanındaki Olay Hub 'ları için etkin **yakalama özelliği** varsa, Event Hubs ad alanını taşımadan önce [Azure Storage veya Azure Data Lake Store gen 2](../storage/common/storage-account-move.md) veya [Azure Data Lake Store Gen 1](../data-lake-store/data-lake-store-migration-cross-region.md) hesaplarını taşıyın. Ayrıca, bu makalede açıklananlara benzer adımları izleyerek, hem depolama hem de Event Hubs ad alanlarını içeren kaynak grubunu diğer bölgeye taşıyabilirsiniz. 
- Event Hubs ad alanı bir **Event Hubs kümedeyse**, bu makaledeki adımlara geçmeden önce [adanmış kümeyi](move-cluster-across-regions.md) **hedef bölgeye** taşıyın. Ayrıca bir Event Hubs kümesi oluşturmak için [GitHub 'daki hızlı başlangıç şablonunu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-cluster-namespace-eventhub/) da kullanabilirsiniz. Şablonda, yalnızca kümeyi oluşturmak için JSON 'ın ad alanı bölümünü kaldırın. 

## <a name="prepare"></a>Hazırlama
Başlamak için Kaynak Yöneticisi şablonu dışarı aktarın. Bu şablon Event Hubs ad alanınızı tanımlayan ayarları içerir.

1. [Azure portalında](https://portal.azure.com) oturum açın.
2. **Tüm kaynaklar** ' ı seçin ve sonra Event Hubs ad alanınızı seçin.
3. > **ayarları**  >  **dışarı aktarma şablonu**' nu seçin.
4. **Şablonu dışarı aktar** sayfasında **İndir** ' i seçin.

    ![Kaynak Yöneticisi şablonu indir](./media/move-across-regions/download-template.png)
5. Portaldan indirdiğiniz. zip dosyasını bulun ve bu dosyayı seçtiğiniz bir klasöre ayıklayın.

   Bu ZIP dosyası, şablonu dağıtmak için şablon ve betikleri içeren. JSON dosyalarını içerir.


## <a name="move"></a>Taşı

Hedef bölgede bir Event Hubs ad alanı oluşturmak için şablonu dağıtın. 


1. Azure portal **kaynak oluştur**' u seçin.
2. **Market 'Te arama**yapın, **şablon dağıtımı**yazın ve **şablon dağıtımı (özel şablonları kullanarak dağıtın)** öğesini seçin.
5. **Düzenleyicide kendi şablonunuzu oluşturun öğesini**seçin.
6. **Dosya Yükle**' yi seçin ve ardından son bölümde indirdiğiniz dosyanın **template.js** yüklemek için yönergeleri izleyin.
1. `location`Özelliğin değerini yeni bölgeyi gösterecek şekilde güncelleştirin. Konum kodlarını almak için bkz. [Azure konumları](https://azure.microsoft.com/global-infrastructure/locations/). Bir bölgenin kodu, boşluk içermeyen bölge adıdır, örneğin, `West US` eşittir `westus` .
1. Şablonu kaydetmek için **Kaydet** ' i seçin. 
1. **Özel dağıtım** sayfasında, aşağıdaki adımları izleyin: 
    1. Bir Azure **aboneliği**seçin. 
    2. Var olan bir **kaynak grubunu** seçin veya bir kaynak grubu oluşturun. Kaynak ad alanı bir Event Hubs kümedeyse hedef bölgede küme içeren kaynak grubunu seçin. 
    3. Hedef **konumu** veya bölgeyi seçin. Var olan bir kaynak grubunu seçtiyseniz, bu ayar salt okunurdur. 
    4. **Ayarlar** bölümünde aşağıdaki adımları uygulayın:    
        1. Yeni **ad alanı adını**girin. 

            ![Kaynak Yöneticisi şablonu dağıtma](./media/move-across-regions/deploy-template.png)
        2. Kaynak ad alanınız **Event Hubs bir kümede**yer alıyorsa, **dış kimliğin**bir parçası olarak **kaynak grubunun** ve **Event Hubs kümesinin** adlarını girin. 

              ```
              /subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<CLUSTER'S RESOURCE GROUP>/providers/Microsoft.EventHub/clusters/<CLUSTER NAME>
              ```   
        3. Ad alanındaki Olay Hub 'ı olayları yakalamak için bir depolama hesabı kullanıyorsa, alanı için kaynak grubu adını ve depolama hesabını belirtin `StorageAccounts_<original storage account name>_external` . 
            
            ```
            /subscriptions/0000000000-0000-0000-0000-0000000000000/resourceGroups/<STORAGE'S RESOURCE GROUP>/providers/Microsoft.Storage/storageAccounts/<STORAGE ACCOUNT NAME>
            ```    
    5. Sayfanın alt kısmındaki **gözden geçir + oluştur** ' u seçin. 
    1. **Gözden geçir + oluştur** sayfasında ayarları gözden geçirin ve ardından **Oluştur**' u seçin.   

## <a name="discard-or-clean-up"></a>Atma veya temizleme
Dağıtımdan sonra, baştan başlamak istiyorsanız, **hedef Event Hubs ad alanını**silebilir ve bu makalenin [hazırlama](#prepare) ve [taşıma](#move) bölümünde açıklanan adımları yineleyebilirsiniz.

Değişiklikleri uygulamak ve bir Event Hubs ad alanını taşımayı tamamlamak için özgün bölgedeki **Event Hubs ad alanını** silin. Ad alanını silmeden önce ad alanındaki tüm olayları işlediğinizden emin olun. 

Azure portal kullanarak bir Event Hubs ad alanını (kaynak veya hedef) silmek için:

1. Azure portal en üstündeki ara penceresinde **Event Hubs**yazın ve arama sonuçlarından **Event Hubs** ' yı seçin. Event Hubs ad alanlarını bir listede görürsünüz.
2. Silinecek hedef ad alanını seçin ve araç çubuğundan **Sil** ' i seçin. 

    ![Ad alanını sil-düğme](./media/move-across-regions/delete-namespace-button.png)
3. **Ad alanını sil** sayfasında, **ad alanı adını**yazarak silme Işlemini onaylayın ve sonra **Sil**' i seçin. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir Azure Event Hubs ad alanını bir bölgeden diğerine taşımış ve kaynak kaynakları temizledi.  Azure 'da bölgeler ve olağanüstü durum kurtarma arasında kaynakları taşıma hakkında daha fazla bilgi edinmek için bkz:


- [Kaynakları yeni bir kaynak grubuna veya aboneliğe taşıma](../azure-resource-manager/management/move-resource-group-and-subscription.md)
- [Azure VM’lerini başka bir bölgeye taşıma](../site-recovery/azure-to-azure-tutorial-migrate.md)
