---
title: Kapsayıcılar için geçici silmeyi etkinleştirme ve yönetme (Önizleme)
titleSuffix: Azure Storage
description: Yanlışlıkla değiştirildiğinde veya silindiğinde verilerinizi daha kolay kurtarmak için kapsayıcı geçici silme (Önizleme) özelliğini etkinleştirin.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 07/21/2020
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: 8d8b558ebc49a009548afe9a8527985b9e4f5c75
ms.sourcegitcommit: 269da970ef8d6fab1e0a5c1a781e4e550ffd2c55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2020
ms.locfileid: "88057392"
---
# <a name="enable-and-manage-soft-delete-for-containers-preview"></a>Kapsayıcılar için geçici silmeyi etkinleştirme ve yönetme (Önizleme)

Kapsayıcı geçici silme (Önizleme), verilerinizin yanlışlıkla veya yanlışlıkla değiştirilmesini veya silinmesini önler. Bir depolama hesabı için kapsayıcı geçici silme etkinleştirildiğinde, bir kapsayıcı ve içeriği silindikten sonra, belirttiğiniz bir bekletme dönemi içinde kurtarılabilir.

Verilerinizin yanlışlıkla bir uygulama veya başka bir depolama hesabı kullanıcısı tarafından değiştirilmesi veya silinmesi olasılığı varsa, Microsoft kapsayıcı geçici silme özelliğini etkinleştirmeyi önerir. Bu makalede, kapsayıcılar için geçici silmenin nasıl etkinleştirileceği gösterilmektedir. Kapsayıcı geçici silme hakkında daha fazla bilgi için bkz. [kapsayıcılar Için geçici silme (Önizleme)](soft-delete-container-overview.md).

Microsoft, uçtan uca veri koruması için, Bloblar ve BLOB sürümü oluşturma (Önizleme) için geçici silme özelliğini de etkinleştirmenizi önerir. Blob 'lar için geçici silme özelliğini etkinleştirmeyi öğrenmek için bkz. [Bloblar için geçici silmeyi etkinleştirme ve yönetme](soft-delete-blob-enable.md). Blob sürüm oluşturmayı nasıl etkinleştireceğinizi öğrenmek için bkz. [BLOB sürüm oluşturma (Önizleme)](versioning-overview.md).

## <a name="enable-container-soft-delete"></a>Kapsayıcı geçici silmeyi etkinleştir

Azure portal veya Azure Resource Manager şablonunu kullanarak istediğiniz zaman depolama hesabı için kapsayıcı geçici silme özelliğini etkinleştirebilir veya devre dışı bırakabilirsiniz.

# <a name="portal"></a>[Portal](#tab/azure-portal)

Azure portal kullanarak depolama hesabınızda kapsayıcı geçici silme özelliğini etkinleştirmek için şu adımları izleyin:

1. [Azure portalında](https://portal.azure.com/) depolama hesabınıza gidin.
1. **BLOB hizmeti**altında **veri koruma** ayarlarını bulun.
1. **Kapsayıcı geçici silme** özelliğini *etkin*olarak ayarlayın.
1. **Bekletme ilkeleri**altında, geçici olarak silinen kapsayıcıların Azure depolama tarafından ne kadar süreyle korunacağını belirtin.
1. Yaptığınız değişiklikleri kaydedin.

:::image type="content" source="media/soft-delete-container-enable/soft-delete-container-portal-configure.png" alt-text="Azure portal kapsayıcıda geçici silme özelliğini etkinleştirmeyi gösteren ekran görüntüsü":::

# <a name="template"></a>[Şablon](#tab/template)

Azure Resource Manager şablonuyla kapsayıcı geçici silmeyi etkinleştirmek için, **Containerdeleteretentionpolicy** özelliğini ayarlayan bir şablon oluşturun. Aşağıdaki adımlar Azure portal bir şablonun nasıl oluşturulacağını açıklamaktadır.

1. Azure portal **kaynak oluştur**' u seçin.
1.  **Market 'Te ara**' te, **şablon dağıtımı**yazın ve ardından **ENTER**tuşuna basın.
1.  **Şablon dağıtımı**öğesini seçin, **Oluştur**' u seçin ve ardından **düzenleyicide kendi şablonunuzu oluştur**' u seçin.
1. Şablon Düzenleyicisi 'nde aşağıdaki JSON öğesine yapıştırın.  `<account-name>`   Yer tutucusunu depolama hesabınızın adıyla değiştirin.

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {},
      "variables": {},
      "resources": [
          {
              "type": "Microsoft.Storage/storageAccounts/blobServices",
              "apiVersion": "2019-06-01",
              "name": "<account-name>/default",
              "properties": {
                  "containerDeleteRetentionPolicy": {
                      "enabled": true,
                      "days": 7
                  }
              }
          }
      ]
    }
    ```

---

1. Saklama süresini belirtin. Varsayılan değer 7 ' dir.
1. Şablonu kaydedin.
1. Hesabın kaynak grubunu belirtin ve ardından **Review + create**   şablonu dağıtmak ve kapsayıcı geçici silme özelliğini etkinleştirmek için gözden geçir + Oluştur düğmesini seçin.

## <a name="view-soft-deleted-containers"></a>Geçici olarak silinen kapsayıcıları görüntüleme

Geçici silme etkinleştirildiğinde, Azure portal geçici olarak silinen kapsayıcıları görüntüleyebilirsiniz. Geçici olarak silinen kapsayıcılar, belirtilen bekletme döneminde görülebilir. Saklama süresi dolduktan sonra, geçici olarak silinen bir kapsayıcı kalıcı olarak silinir ve artık görünmez.

Azure portal geçici olarak silinen kapsayıcıları görüntülemek için aşağıdaki adımları izleyin:

1. Azure portal depolama hesabınıza gidin ve kapsayıcılarınızın listesini görüntüleyin.
1. Silinen kapsayıcıları göster anahtarını listede silinen kapsayıcıları içerecek şekilde değiştirin.

    :::image type="content" source="media/soft-delete-container-enable/soft-delete-container-portal-list.png" alt-text="Azure portal geçici olarak silinen kapsayıcıları görüntülemeyi gösteren ekran görüntüsü":::

## <a name="restore-a-soft-deleted-container"></a>Geçici olarak silinen kapsayıcıyı geri yükleme

Geçici olarak silinen bir kapsayıcıyı ve içeriğini saklama dönemi içinde geri yükleyebilirsiniz. Azure portal geçici olarak silinen kapsayıcıyı geri yüklemek için şu adımları izleyin:

1. Azure portal depolama hesabınıza gidin ve kapsayıcılarınızın listesini görüntüleyin.
1. Silmek istediğiniz kapsayıcının bağlam menüsünü görüntüleyin ve menüden **silmeyi geri al** ' ı seçin.

    :::image type="content" source="media/soft-delete-container-enable/soft-delete-container-portal-restore.png" alt-text="Azure portal ' de geçici olarak silinen kapsayıcının nasıl geri yükleneceğini gösteren ekran görüntüsü":::

## <a name="next-steps"></a>Sonraki adımlar

- [Kapsayıcılar için geçici silme (Önizleme)](soft-delete-container-overview.md)
- [Bloblar için geçici silme](soft-delete-blob-overview.md)
- [Blob sürümü oluşturma (Önizleme)](versioning-overview.md)