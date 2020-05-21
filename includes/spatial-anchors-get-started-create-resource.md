---
author: craigktreasure
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 12/13/2018
ms.author: crtreasu
ms.openlocfilehash: b2b3ca886359a0b4c906b89ed76f57486fc2c368
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83638982"
---
## <a name="create-a-spatial-anchors-resource"></a>Uzamsal bağlayıcı kaynağı oluşturma

<a href="https://portal.azure.com" target="_blank">Azure Portal</a>gidin.

Azure portal sol gezinti bölmesinde **kaynak oluştur**' u seçin.

**Uzamsal bağlantıları**aramak için arama kutusunu kullanın.

   ![Uzamsal bağlayıcıları ara](./media/spatial-anchors-get-started-create-resource/portal-search.png)

**Uzamsal bağlantıları**seçin. İletişim kutusunda **Oluştur**' u seçin.

**Uzamsal bağlayıcı hesabı** iletişim kutusunda:

- Normal alfasayısal karakterleri kullanarak benzersiz bir kaynak adı girin.
- Kaynağı iliştirmek istediğiniz aboneliği seçin.
- **Yeni oluştur**seçeneğini belirleyerek bir kaynak grubu oluşturun. **Myresourcegroup** olarak adlandırın ve **Tamam**' ı seçin.
      [!INCLUDE [resource group intro text](resource-group.md)]
- Kaynağın yerleştirileceği bir konum (bölge) seçin.
- Kaynağı oluşturmaya başlamak için **Yeni** ' yi seçin.

   ![Kaynak oluşturma](./media/spatial-anchors-get-started-create-resource/create-resource-form.png)

Kaynak oluşturulduktan sonra Azure Portal, dağıtımınızın tamamlandığını gösterir. **Kaynağa git**'e tıklayın.

![Dağıtım tamamlandı](./media/spatial-anchors-get-started-create-resource/deployment-complete.png)

Daha sonra kaynak özelliklerini görüntüleyebilirsiniz. Daha sonra ihtiyacınız olacak şekilde, kaynağın **hesap kimliği** değerini bir metin düzenleyicisine kopyalayın.

   ![Kaynak özellikleri](./media/spatial-anchors-get-started-create-resource/view-resource-properties.png)

Ayrıca, daha sonra ihtiyacınız olacak şekilde kaynağın **hesap etki alanı** değerini bir metin düzenleyicisine kopyalayın.

   ![Hesap etki alanı](./media/spatial-anchors-get-started-create-resource/view-resource-domain.png)

**Ayarlar**altında **anahtar**' ı seçin. **Birincil anahtar** değerini bir metin düzenleyicisine kopyalayın. Bu değer, `Account Key` . Buna daha sonra ihtiyacınız olacak.

   ![Hesap anahtarı](./media/spatial-anchors-get-started-create-resource/view-account-key.png)
