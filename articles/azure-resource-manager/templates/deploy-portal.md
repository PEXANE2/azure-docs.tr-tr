---
title: Azure portalı ile kaynakları dağıtma
description: Kaynaklarınızı aboneliğinizdeki bir kaynak grubuna dağıtmak için Azure portalını ve Azure Kaynak Yönetimi'ni kullanın.
ms.topic: conceptual
ms.date: 06/27/2019
ms.openlocfilehash: ea2faf810b3a5d1b8fa46575201022a501cc7d58
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80153446"
---
# <a name="deploy-resources-with-arm-templates-and-azure-portal"></a>ARM şablonları ve Azure portalı ile kaynakları dağıtma

Azure kaynaklarınızı dağıtmak için [Azure Kaynak Yöneticisi (ARM) şablonlarıyla](overview.md) Azure [portalını](https://portal.azure.com) nasıl kullanacağınızı öğrenin. Kaynaklarınızı yönetme hakkında bilgi edinmek için [Azure portalını kullanarak Azure kaynaklarını yönet'e](../management/manage-resources-portal.md)bakın.

Azure portalını kullanarak Azure kaynaklarını dağıtmak genellikle iki adım dan oluşur:

- Bir kaynak grubu oluşturun.
- Kaynakları kaynak grubuna dağıtın.

Ayrıca, Azure kaynakları oluşturmak için bir ARM şablonu da dağıtabilirsiniz.

Bu makalede, her iki yöntem de gösterilmektedir.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

1. Yeni bir kaynak grubu oluşturmak için [Azure portalından](https://portal.azure.com) **Kaynak grupları** seçin.

   ![Kaynak gruplarını seçme](./media/deploy-portal/select-resource-groups.png)

1. Kaynak grupları altında **Ekle'yi**seçin.

   ![Kaynak grubu ekleme](./media/deploy-portal/add-resource-group.png)

1. Aşağıdaki özellik değerlerini seçin veya girin:

    - **Abonelik**: Azure aboneliği seçin.
    - **Kaynak grubu**: Kaynak grubuna bir ad verin.
    - **Bölge**: Azure konumu belirtin. Kaynak grubunun kaynaklarla ilgili meta verileri depoladığı yer burasıdır. Uyumluluk nedenleriyle, bu meta verilerin nerede depolandığınızı belirtmek isteyebilirsiniz. Genel olarak, kaynaklarınızın çoğunun bulunduğu bir konum belirtmenizi öneririz. Aynı konumu kullanmak şablonunuzu basitleştirebilir.

   ![Grup değerlerini ayarlama](./media/deploy-portal/set-group-properties.png)

1. **İncele ve oluştur**’u seçin.
1. değerleri gözden geçirin ve ardından **Oluştur'u**seçin.
1. Listede yeni kaynak grubunu göremeden önce **Yenile'yi** seçin.

## <a name="deploy-resources-to-a-resource-group"></a>Kaynakları kaynak grubuna dağıtma

Bir kaynak grubu oluşturduktan sonra, kaynakları Pazar'dan gruba dağıtabilirsiniz. Market, yaygın senaryolar için önceden tanımlanmış çözümler sağlar.

1. Dağıtımı başlatmak için [Azure portalından](https://portal.azure.com) **kaynak oluştur'u** seçin.

   ![Yeni kaynak](./media/deploy-portal/new-resources.png)

1. Dağıtmak istediğiniz kaynak türünü bulun. Kaynaklar kategoriler halinde düzenlenir. Dağıtmak istediğiniz belirli çözümü görmüyorsanız, pazar yerinde arama yapabilirsiniz. Aşağıdaki ekran görüntüsü Ubuntu Server'ın seçildiğini gösterir.

   ![Kaynak türünü seçin](./media/deploy-portal/select-resource-type.png)

1. Seçili kaynağın türüne bağlı olarak, dağıtımdan önce ayarlanması gereken alakalı özellikler koleksiyonunuz vardır. Tüm türler için bir hedef kaynak grubu seçmeniz gerekir. Aşağıdaki resim, bir Linux sanal makinesinin nasıl oluşturulup oluşturduğunuz kaynak grubuna nasıl dağıtılanın caiz olduğunu gösterir.

   ![Kaynak grubu oluşturma](./media/deploy-portal/select-existing-group.png)

   Alternatif olarak, kaynaklarınızı dağıtırken bir kaynak grubu oluşturmaya karar verebilirsiniz. **Yeni Oluştur'u** seçin ve kaynak grubuna bir ad verin.

1. Dağıtımınız başlıyor. Dağıtım birkaç dakika sürebilir. Bazı kaynaklar diğer kaynaklardan daha uzun zaman alır. Dağıtım tamamlandığında, bir bildirim görürsünüz. Açmak **için kaynağa git'i** seçin

   ![Bildirimi görüntüleme](./media/deploy-portal/view-notification.png)

1. Kaynaklarınızı dağıttıktan sonra **Ekle'yi**seçerek kaynak grubuna daha fazla kaynak ekleyebilirsiniz.

   ![Kaynak ekleme](./media/deploy-portal/add-resource.png)

## <a name="deploy-resources-from-custom-template"></a>Özel şablondan kaynak dağıtma

Bir dağıtımı yürütmek, ancak Market'teki şablonlardan hiçbirini kullanmak istemiyorsanız, çözümünüzün altyapısını tanımlayan özelleştirilmiş bir şablon oluşturabilirsiniz. Şablon oluşturma hakkında bilgi edinmek için [bkz.](template-syntax.md)

> [!NOTE]
> Portal [arayüzü, Key Vault'tan](key-vault-parameter.md)bir sırrın referansını desteklemez. Bunun yerine, şablonunuzu yerel olarak veya harici bir URI'den dağıtmak için [PowerShell](deploy-powershell.md) veya [Azure CLI'yi](deploy-cli.md) kullanın.

1. Portal üzerinden özelleştirilmiş bir şablon dağıtmak için **kaynak oluştur,** **şablon**ara'yı seçin. ve ardından **Şablon dağıtımını**seçin.

   ![Arama şablonu dağıtımı](./media/deploy-portal/search-template.png)

1. **Oluştur'u**seçin.
1. Şablon oluşturmak için çeşitli seçenekler görürsünüz:

    - **Düzenleyicide kendi şablonunuzu oluşturun:** portal şablondüzenleyicisini kullanarak bir şablon oluşturun.  Düzenleyici bir kaynak şablonşeması eklemek için yeteneğine sahiptir.
    - **Ortak şablonlar**: Linux sanal makinesi, Windows sanal makinesi, bir web uygulaması ve Azure SQL veritabanı oluşturmak için dört ortak şablon vardır.
    - **GitHub hızlı başlatma şablonu yükleyin:** varolan [bir quickstart şablonu](https://azure.microsoft.com/resources/templates/)kullanın.

   ![Seçenekleri görüntüleme](./media/deploy-portal/see-options.png)

    Bu öğretici, hızlı başlangıç şablonu yükleme talimatını sağlar.

1. **GitHub hızlı başlat şablonu yükle**altında, **101-storage-account-create**yazın veya seçin.

    İki seçeneğiniz vardır:

    - **Şablonu seçin:** şablonu dağıtın.
    - **Şablonu edit:** Dağıtmadan önce hızlı başlat şablonu edin.

1. Portal şablonu düzenleyicisini keşfetmek için **Şablonu Edit'i** seçin. Şablon düzenleyiciye yüklenir. İki parametre olduğuna dikkat edin: **storageAccountType** ve **konumu**.

   ![Şablon oluşturma](./media/deploy-portal/show-json.png)

1. Şablonda küçük bir değişiklik yapın. Örneğin, **storageAccountName** değişkenini şu şekilde güncelleştirin:

    ```json
    "storageAccountName": "[concat('azstore', uniquestring(resourceGroup().id))]"
    ```

1. **Kaydet'i**seçin. Şimdi portal şablonu dağıtım arabirimini görüyorsunuz. Şablonda tanımladığınız iki parametreye dikkat edin.
1. Özellik değerlerini girin veya seçin:

    - **Abonelik**: Azure aboneliği seçin.
    - **Kaynak grubu**: **Yeni Oluştur'u** seçin ve bir ad verin.
    - **Konum**: Azure konumu seçin.
    - **Depolama Hesabı Türü**: Varsayılan değeri kullanın.
    - **Konum**: Varsayılan değeri kullanın.
    - **Yukarıda belirtilen hüküm ve koşulları kabul ediyorum**: (seçin)

1. **Satın al**'ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

- Denetim günlüklerini görüntülemek için [Kaynak Yöneticisi ile Denetim işlemlerine](../management/view-activity-logs.md)bakın.
- Dağıtım hatalarını gidermek için [bkz.](deployment-history.md)
- Bir dağıtım veya kaynak grubundan şablon dışa aktarmak için, [Dışa aktarma KOLU şablonlarına](export-template-portal.md)bakın.
- Hizmetinizi birden çok bölgede güvenli bir şekilde kullanıma sunmak için [Azure Dağıtım Yöneticisi'ne](deployment-manager-overview.md)bakın.
