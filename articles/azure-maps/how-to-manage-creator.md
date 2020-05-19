---
title: Azure haritalar oluşturucuyu yönetme
description: Bu makalede, Azure haritalar Creator 'un nasıl yönetileceğini öğreneceksiniz.
author: anastasia-ms
ms.author: v-stharr
ms.date: 05/18/2020
ms.topic: how-to
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 945c7035c2eeada3fea358489460f80c40438950
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83598314"
---
# <a name="manage-azure-maps-creator"></a>Azure haritalar oluşturucuyu yönetme

Azure haritalar Creator, özel ınkapılı harita verileri oluşturmanızı sağlar. Azure Haritalar API 'sini ve ınkapıharitaları modülünü kullanarak etkileşimli ve dinamik bir ınkapılı harita Web uygulamaları geliştirebilirsiniz. Şu anda, Oluşturucu yalnızca S1 fiyatlandırma katmanını kullanarak Birleşik Devletler kullanılabilir.

Bu makale, bir Azure haritalar hesabındaki bir Oluşturucu kaynağı oluşturma ve silme adımlarında size kılavuzluk ediyor.

## <a name="create-creator-resource"></a>Oluşturucu kaynağı oluştur

1. [Azure Portal](https://portal.azure.com) oturum açın

2. Azure haritalar hesabınızı seçin. Azure haritalar hesabınızı **son kaynaklar**altında göremiyorsanız Azure Portal menüsüne gidin. **Tüm kaynaklar**’ı seçin. Azure haritalar hesabınızı bulun ve seçin.

    ![Azure haritalar Portalı giriş sayfası](./media/how-to-manage-creator/select-maps-account.png)

3. Azure haritalar hesabı sayfasından sonra **Oluşturucu**altındaki **genel bakış** seçeneğine gidin. Azure haritalar Oluşturucu kaynağı oluşturmak için **Oluştur** ' a tıklayın.

    ![Azure haritalar Oluşturucu oluşturma sayfası](./media/how-to-manage-creator/creator-blade-settings.png)

4. Oluşturucu kaynağınız için ad ve konum girin. Şu anda, Oluşturucu yalnızca Birleşik Devletler desteklenir. **Gözden geçir ve oluştur**’a tıklayın.

   ![Oluşturucu hesap bilgilerini girin sayfası](./media/how-to-manage-creator/creator-creation-dialog.png)

5. Ayarlarınızı gözden geçirin ve **Oluştur**' a tıklayın.

    ![Oluşturucu hesabı ayarları sayfasını Onayla](./media/how-to-manage-creator/creator-create.PNG)

6. Dağıtım tamamlandığında, başarı veya hata iletisi içeren bir sayfa görürsünüz.

   ![Kaynak dağıtım durumu sayfası](./media/how-to-manage-creator/creator-resource-created.png)

7. **Kaynağa git**'e tıklayın. Oluşturucu kaynak görünümü sayfanız, Oluşturucu kaynağınızın ve seçilen demografik bölgesinin durumunu gösterir.

    ![Oluşturan durum sayfası](./media/how-to-manage-creator/creator-resource-view.png)

   >[!NOTE]
   >Oluşturan kaynak sayfasından, Azure Maps hesabı ' na tıklayarak ait olduğu Azure Maps hesabına geri gidebilirsiniz.

## <a name="delete-creator-resource"></a>Oluşturucu kaynağını Sil

Oluşturan kaynağını silmek için Azure haritalar hesabınıza gidin. **Oluşturucu**altında **Genel Bakış ' ı** seçin. **Sil** düğmesine tıklayın.

>[!WARNING]
>Azure haritalar hesabınızın Oluşturucu kaynağını sildiğinizde, Oluşturucu Hizmetleri kullanılarak oluşturulan veri kümelerini, tilekümelerini ve özellik statesets 'leri de silersiniz.

![Sil düğmesi olan Oluşturucu sayfası](./media/how-to-manage-creator/creator-delete.png)

**Sil** düğmesine tıklayın ve silme işlemini onaylamak için Oluşturucu adınızı yazın. Kaynak silindikten sonra aşağıdaki görüntüde olduğu gibi bir onay sayfası görürsünüz:

![Silme onayı içeren Oluşturucu sayfası](./media/how-to-manage-creator/creator-confirmdelete.png)

## <a name="authentication"></a>Kimlik Doğrulaması

Creator, Azure Maps Access Control (ıAM) ayarlarını devralır. Veri erişimi için tüm API çağrılarının kimlik doğrulama ve yetkilendirme kurallarıyla gönderilmesi gerekir.

Oluşturucu kullanım verileri, Azure Haritalar kullanım grafiklerinize ve etkinlik günlüğüne eklenir.  Daha fazla bilgi için bkz. [Azure haritalar 'da kimlik doğrulamasını yönetme](https://docs.microsoft.com/azure/azure-maps/how-to-manage-authentication).

## <a name="access-to-creator-services"></a>Creator hizmetlerine erişim

Oluşturucu Hizmetleri yalnızca oluşturma sırasında seçilen konumun içinden erişilebilir. Seçili konum dışından Oluşturucu Hizmetleri için çağrılar yapılırsa, bir kullanıcı hatası iletisi döndürülür. Seçilen konumun dışından çağrı yapmak için, hizmet URL 'SI Seçilen konumların coğrafi önekini içermelidir. Örneğin, Oluşturucu Birleşik Devletler oluşturulduysa, dönüştürme hizmetine yapılan tüm çağrılar ' a gönderilmelidir `us.atlas.microsoft.com/conversion/convert` .

Ayrıca, Oluşturucu ile içeri aktarılan tüm veriler, oluşturan kaynakla aynı coğrafi konuma yüklenmelidir. Örneğin, Oluşturucu Birleşik olarak belirtilmişse, tüm ham verilerin aracılığıyla yüklenmesi gerekir `us.atlas.microsoft.com/mapData/upload` .

## <a name="next-steps"></a>Sonraki adımlar

Inkapıeşleme için oluşturucuya giriş:

> [!div class="nextstepaction"]
> [Karşıya veri yükleme](creator-indoor-maps.md#upload-a-drawing-package)

> [!div class="nextstepaction"]
> [Veri dönüştürme](creator-indoor-maps.md#convert-a-drawing-package)

> [!div class="nextstepaction"]
> [Veri kümesi](creator-indoor-maps.md#datasets)

> [!div class="nextstepaction"]
> [Tileset](creator-indoor-maps.md#tilesets)

> [!div class="nextstepaction"]
> [Özellik durumu kümesi](creator-indoor-maps.md#feature-statesets)

Uygulamanızdaki ınkapılı haritaları işlemek için oluşturucuyu nasıl kullanacağınızı öğrenin:

> [!div class="nextstepaction"]
> [Azure haritalar Oluşturucu öğreticisi](tutorial-creator-indoor-maps.md)

> [!div class="nextstepaction"]
> [Inkapılı harita dinamik stili](indoor-map-dynamic-styling.md)

> [!div class="nextstepaction"]
> [Inkapıharitaları modülünü kullanma](how-to-use-indoor-module.md)