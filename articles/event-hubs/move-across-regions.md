---
title: Azure Event Hubs ad alanını başka bir bölgeye taşıma | Microsoft Docs
description: Bu makalede, Azure Event Hubs ad alanını geçerli bölgeden başka bir bölgeye nasıl taşıyacağınız gösterilmektedir.
ms.topic: how-to
ms.date: 06/23/2020
ms.openlocfilehash: 51b02c34b0c28420a7e27da56b107ed3925a761b
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86537080"
---
# <a name="move-an-azure-event-hubs-namespace-to-another-region"></a>Azure Event Hubs ad alanını başka bir bölgeye taşıma
Mevcut Event Hubs ad alanınızı bir bölgeden diğerine taşımak istediğiniz çeşitli senaryolar vardır. Örneğin, test için aynı yapılandırmaya sahip bir ad alanı oluşturmak isteyebilirsiniz. Ayrıca [olağanüstü durum kurtarma planlamasının](event-hubs-geo-dr.md#setup-and-failover-flow)bir parçası olarak başka bir bölgede ikincil bir ad alanı oluşturmak isteyebilirsiniz.

> [!NOTE]
> Bu makalede, var olan bir Event Hubs ad alanı için Azure Resource Manager şablonu dışarı aktarma ve ardından şablonu kullanarak başka bir bölgede aynı yapılandırma ayarlarına sahip bir ad alanı oluşturma işlemi gösterilmektedir. Ancak, bu işlem henüz işlenmemiş olayları taşımaz. Silmeden önce özgün ad alanındaki olayları işlemek gerekir.

## <a name="prerequisites"></a>Önkoşullar

- Hesabınızın kullandığı hizmetlerin ve özelliklerin hedef bölgede desteklendiğinden emin olun.
- Önizleme özellikleri için aboneliğinizin hedef bölgede beyaz listeye eklendiğinden emin olun.
- Ad alanındaki Olay Hub 'ları için etkin **yakalama özelliği** varsa, Event Hubs ad alanını taşımadan önce [Azure Storage veya Azure Data Lake Store gen 2](../storage/common/storage-account-move.md) veya [Azure Data Lake Store Gen 1](../data-lake-store/data-lake-store-migration-cross-region.md) hesaplarını taşıyın. Ayrıca, bu makalede açıklananlara benzer adımları izleyerek, hem depolama hem de Event Hubs ad alanlarını içeren kaynak grubunu diğer bölgeye taşıyabilirsiniz. 
- Event Hubs ad alanı bir **Event Hubs kümedeyse**, bu makaledeki adımlara geçmeden önce **hedef bölgede** [ayrılmış bir küme oluşturun](event-hubs-dedicated-cluster-create-portal.md) . Ayrıca bir Event Hubs kümesi oluşturmak için [GitHub 'daki hızlı başlangıç şablonunu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-cluster-namespace-eventhub/) da kullanabilirsiniz. Şablonda, yalnızca kümeyi oluşturmak için JSON 'ın ad alanı bölümünü kaldırın. 

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

2. **Market 'Te ara**' te, **şablon dağıtımı**yazın ve ardından **ENTER**tuşuna basın.

3. **Şablon dağıtımı**seçin.

4. **Oluştur**’u seçin.

5. **Düzenleyicide kendi şablonunuzu oluşturun öğesini**seçin.

6. **Dosya Yükle**' yi seçin ve ardından son bölümde indirdiğiniz dosyanın **template.js** yüklemek için yönergeleri izleyin.

7. Şablonu kaydetmek için **Kaydet** ' i seçin. 

8. **Özel dağıtım** sayfasında, aşağıdaki adımları izleyin: 

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
    5. **Yukarıda belirtilen hüküm ve koşulları kabul ediyorum** onay kutusunu seçin. 
    
    6. Şimdi dağıtım işlemini başlatmak için **satın al** ' ı seçin. 

## <a name="discard-or-clean-up"></a>Atma veya temizleme
Dağıtımdan sonra, baştan başlamak istiyorsanız, **hedef Event Hubs ad alanını**silebilir ve bu makalenin [hazırlama](#prepare) ve [taşıma](#move) bölümünde açıklanan adımları yineleyebilirsiniz.

Değişiklikleri uygulamak ve bir Event Hubs ad alanını taşımayı tamamlamak için, **kaynak Event Hubs ad alanını**silin. Ad alanını silmeden önce ad alanındaki tüm olayları işlediğinizden emin olun. 

Azure portal kullanarak bir Event Hubs ad alanını (kaynak veya hedef) silmek için:

1. Azure portal en üstündeki ara penceresinde **Event Hubs**yazın ve arama sonuçlarından **Event Hubs** ' yı seçin. Event Hubs ad alanlarını bir listede görürsünüz.

2. Silinecek hedef ad alanını seçin ve araç çubuğundan **Sil** ' i seçin. 

    ![Ad alanını sil-düğme](./media/move-across-regions/delete-namespace-button.png)

3. **Kaynakları Sil*** sayfasında, seçilen kaynakları doğrulayın ve **Evet**yazarak silme Işlemini onaylayın ve sonra **Sil**' i seçin. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir Azure Event Hubs ad alanını bir bölgeden diğerine taşımış ve kaynak kaynakları temizledi.  Azure 'da bölgeler ve olağanüstü durum kurtarma arasında kaynakları taşıma hakkında daha fazla bilgi edinmek için bkz:


- [Kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../azure-resource-manager/management/move-resource-group-and-subscription.md)
- [Azure VM’lerini başka bir bölgeye taşıma](../site-recovery/azure-to-azure-tutorial-migrate.md)
