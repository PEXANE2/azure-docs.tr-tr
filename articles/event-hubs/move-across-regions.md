---
title: Azure Etkinlik Hub'ları ad alanını başka bir bölgeye taşıma | Microsoft Dokümanlar
description: Bu makalede, bir Azure Olay Hub'ları ad alanını geçerli bölgeden başka bir bölgeye nasıl taşıyabileceğinizi gösterilmektedir.
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.topic: how-to
ms.custom: subject-moving-resources
ms.date: 04/14/2020
ms.author: spelluru
ms.reviewer: shvija
ms.openlocfilehash: 2dfc9c517605bbb48bee0b306fb275464cfebe39
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81606814"
---
# <a name="move-an-azure-event-hubs-namespace-to-another-region"></a>Azure Etkinlik Hub'ları ad alanını başka bir bölgeye taşıma
Varolan Olay Hub'ları ad alanınızı bir bölgeden diğerine taşımak istediğiniz çeşitli senaryolar vardır. Örneğin, sınama için aynı yapılandırmaya sahip bir ad alanı oluşturmak isteyebilirsiniz. [Ayrıca, olağanüstü durum kurtarma planlamasının](event-hubs-geo-dr.md#setup-and-failover-flow)bir parçası olarak başka bir bölgede ikincil bir ad alanı oluşturmak isteyebilirsiniz.

> [!NOTE]
> Bu makalede, varolan bir Olay Hub'ları ad alanı için azure kaynak yöneticisi şablonu nasıl dışa aktarılabildiğinizi ve ardından şablonu kullanarak başka bir bölgede aynı yapılandırma ayarlarına sahip bir ad alanı oluşturabileceğinizi gösterir. Ancak, bu işlem henüz işlenmemiş olayları taşımaz. Silmeden önce olayları özgün ad alanından işlemeniz gerekir.

## <a name="prerequisites"></a>Ön koşullar

- Hesabınızın kullandığı hizmetlerin ve özelliklerin hedef bölgede desteklendirilmesini sağlayın.
- Önizleme özellikleri için, aboneliğinizin hedef bölge için beyaz listeye alındığından emin olun.
- Ad alanındaki etkinlik hub'ları için etkinleştirilen **yakalama özelliğiniz** varsa, Olay Hub'ları ad alanını taşımadan önce [Azure Depolama veya Azure Veri Gölü Deposu Gen 2](../storage/common/storage-account-move.md) veya Azure Veri Gölü Deposu Gen [1](../data-lake-store/data-lake-store-migration-cross-region.md) hesaplarını taşıyın. Ayrıca, hem Depolama hem de Olay Hub'ları ad alanlarını içeren kaynak grubunu, bu makalede açıklananlara benzer adımları izleyerek diğer bölgeye taşıyabilirsiniz. 
- Olay Hub'ları ad alanı bir **Olay Hub'ları**kümesindeyse, bu makaledeki adımları geçmeden önce **hedef bölgede** özel bir [küme oluşturun.](event-hubs-dedicated-cluster-create-portal.md) 

## <a name="prepare"></a>Hazırlama
Başlamak için Kaynak Yöneticisi şablonu dışa aktarın. Bu şablon, Olay Hub'ları ad alanınızı açıklayan ayarlar içerir.

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. **Tüm kaynakları** seçin ve ardından Olay Hub'ları ad alanınızı seçin.

3. **Ayarlar** > **> Verme şablonunu**seçin.

4. **Dışa Aktarma şablonu** sayfasında **İndir'i** seçin.

    ![Kaynak Yöneticisi şablonu indir](./media/move-across-regions/download-template.png)

5. Portaldan indirdiğiniz .zip dosyasını bulun ve dosyayı istediğiniz klasöre çekin.

   Bu zip dosyası şablonu dağıtmak için şablon ve komut dosyaları içeren .json dosyaları içerir.


## <a name="move"></a>Taşı

Hedef bölgede bir Olay Hub'ları ad alanı oluşturmak için şablonu dağıtın. 


1. Azure portalda **Kaynak oluştur**’u seçin.

2. **Market içinde ara** alanına **şablon dağıtımı** yazın ve **ENTER** tuşuna basın.

3. **Şablon dağıtımı**'nı seçin.

4. **Oluştur**’u seçin.

5. **Düzenleyicide kendi şablonunuzu oluşturun**'u seçin.

6. **Dosyayı Yükle'yi**seçin ve ardından son bölümde indirdiğiniz **template.json** dosyasını yüklemek için yönergeleri izleyin.

7. Şablonu kaydetmek için **Kaydet'i** seçin. 

8. Özel **dağıtım** sayfasında aşağıdaki adımları izleyin: 

    1. Azure **aboneliği**seçin. 

    2. Varolan bir **kaynak grubu** seçin veya bir tane oluşturun. Kaynak ad alanı olay hub'ları kümesindeyse, hedef bölgede küme içeren kaynak grubunu seçin. 

    3. Hedef **konumu** veya bölgeyi seçin. Varolan bir kaynak grubu seçtiyseniz, bu ayar salt okunur. 

    4. **AYARLAR** bölümünde aşağıdaki adımları yapın:
    
        1. yeni **ad alanı adını**girin. 

            ![Kaynak Yöneticisi şablonu dağıtma](./media/move-across-regions/deploy-template.png)

        2. Kaynak ad alanınız bir **Olay Hub'ları**kümesindeyse, **dış kimliğin**bir parçası olarak **kaynak grubu** ve **Olay Hub'ları kümesinin** adlarını girin. 

              ```
              /subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<CLUSTER'S RESOURCE GROUP>/providers/Microsoft.EventHub/clusters/<CLUSTER NAME>
              ```   
        3. Ad alanınızdaki olay hub'ı olayları yakalamak için bir Depolama hesabı kullanıyorsa, alan için `StorageAccounts_<original storage account name>_external` kaynak grubu adını ve depolama hesabını belirtin. 
            
            ```
            /subscriptions/0000000000-0000-0000-0000-0000000000000/resourceGroups/<STORAGE'S RESOURCE GROUP>/providers/Microsoft.Storage/storageAccounts/<STORAGE ACCOUNT NAME>
            ```    
    5. Onay kutusunun **üzerinde belirtilen hüküm ve koşulları kabul** ediyorum seçin. 
    
    6. Şimdi, dağıtım işlemini başlatmak için **Satın Alma'yı seçin'i** seçin. 

## <a name="discard-or-clean-up"></a>Atın veya temizleme
Dağıtımdan sonra, baştan başlamak isterseniz, hedef **Olay Hub'ları ad alanını**silebilir ve bu makalenin [Hazırla](#prepare) ve [Taşı](#move) bölümlerinde açıklanan adımları yineleyebilirsiniz.

Değişiklikleri işlemek ve Olay Hub'ları ad alanının hareketini tamamlamak için **kaynak Olay Hub'ları ad alanını**silin. Ad alanını silmeden önce ad alanındaki tüm olayları işlediğinden emin olun. 

Azure portalını kullanarak bir Olay Hub'ı ad alanını (kaynak veya hedef) silmek için:

1. Azure portalının üst kısmındaki arama penceresinde **Olay Hub'ları**yazın ve arama sonuçlarından **Olay Hub'larını** seçin. Bir listede Olay Hub'ları ad alanlarını görürsünüz.

2. Silmek için hedef ad alanını seçin ve araç çubuğundan **Sil'i** seçin. 

    ![Ad alanını sil - düğmesi](./media/move-across-regions/delete-namespace-button.png)

3. Kaynakları **Sil*** sayfasında, seçili kaynakları doğrulayın ve **evet**yazarak silme işlemini onaylayın ve sonra **Sil'i**seçin. 

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, bir Azure Etkinlik Hub'ı ad alanını bir bölgeden diğerine taşıdınız ve kaynak kaynaklarını temizlediniz.  Azure'da kaynakları bölgeler arasında taşıma ve olağanüstü durum kurtarma hakkında daha fazla bilgi edinmek için şu na bakın:


- [Kaynakları yeni kaynak grubuna veya aboneliğe taşıma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)
- [Azure VM’lerini başka bir bölgeye taşıma](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-migrate)
