---
title: Azure portal bir pano oluşturun
description: Bu makalede, Azure portal bir panonun nasıl oluşturulacağı ve özelleştirileceği açıklanır.
ms.assetid: ff422f36-47d2-409b-8a19-02e24b03ffe7
ms.topic: how-to
ms.date: 03/16/2021
ms.openlocfilehash: fa7f1813d86571b568d23d64cab5705f8a117faa
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "104774656"
---
# <a name="create-a-dashboard-in-the-azure-portal"></a>Azure portal bir pano oluşturun

Panolar Azure portal bulut kaynaklarınızın odaklanmış ve düzenlenmiş görünümüdür. Panoları, kaynakları izleyebileceğiniz ve günlük işlemlere yönelik görevleri hızlı bir şekilde başlatabileceği bir çalışma alanı olarak kullanın. Örneğin, projeler, görevler veya Kullanıcı rollerine dayalı özel panolar oluşturun.

Azure portal, başlangıç noktası olarak varsayılan bir pano sağlar. Varsayılan panoyu düzenleyebilir, ek panolar oluşturabilir ve özelleştirebilirsiniz. Bu makalede, yeni bir panonun nasıl oluşturulacağı ve özelleştirileceği açıklanır. Panoları paylaşma hakkında daha fazla bilgi için bkz. Azure ['un rol tabanlı erişim denetimi kullanarak Azure panoları paylaşma](azure-portal-dashboard-share-access.md).

## <a name="create-a-new-dashboard"></a>Yeni pano oluşturma

Bu örnekte, yeni bir özel Pano oluşturup bir ad atayacağız. Kullanmaya başlamak için bu adımları izleyin:

1. [Azure portalında](https://portal.azure.com) oturum açın.

1. Azure portal menüsünde, **Pano**' yı seçin. Varsayılan görünüm zaten pano olarak ayarlanmış olabilir.

    ![Panoyu açma](./media/azure-portal-dashboards/portal-menu-dashboard.png)

1. **Yeni Pano** ' yı ve **boş Pano**' yı seçin.

    ![Yeni panonun ekran görüntüsü](./media/azure-portal-dashboards/create-new-dashboard.png)

    Bu eylem, kutucukları seçtiğiniz **kutucuk galerisini** ve kutucukları düzenlediğiniz boş bir Kılavuzu açar.

1. Pano etiketindeki **My Dashboard** metnini seçin ve özel panoyu kolayca belirlemenize yardımcı olacak bir ad girin.

    ![Kutucuk galerisinin ve boş kılavuzun ekran görüntüsü](./media/azure-portal-dashboards/dashboard-name.png)

1. Sayfa üstbilgisinde düzenleme modundan çıkmak için **bitti** ' yi seçin ve ardından **Kaydet**' i seçin.

    :::image type="content" source="media/azure-portal-dashboards/dashboard-save.png" alt-text="Pano kaydetme işleminin ekran görüntüsü":::

Pano görünümü artık yeni panonuzu gösterir. Kullanılabilir panoları görmek için Pano adının yanındaki oku seçin. Listede, diğer kullanıcıların oluşturduğu ve paylaştığı panolar bulunabilir.

## <a name="edit-a-dashboard"></a>Bir panoyu düzenleme

Şimdi, Azure kaynaklarınızı temsil eden kutucukları eklemek, yeniden boyutlandırmak ve düzenlemek için panoyu düzenleyelim.

### <a name="add-tiles-from-the-tile-gallery"></a>Kutucuk galerisinden kutucuk ekleme

Bir panoya kutucuk eklemek için şu adımları izleyin:

1. ![Sayfa başlığından Düzenle simgesini ](./media/azure-portal-dashboards/dashboard-edit-icon.png) **Düzenle** ' yi seçin.

    ![Pano vurgulama düzenleme ekran görüntüsü](./media/azure-portal-dashboards/dashboard-edit.png)

1. **Kutucuk galerisine** gidin veya istediğiniz kutucuğu bulmak için arama alanını kullanın.

1. Kutucuğu varsayılan bir boyut ve konum ile panoya eklemek için **Ekle** ' yi seçin. Ya da kutucuğu kılavuza sürükleyin ve istediğiniz yere yerleştirin. İstediğiniz kutucukları ekleyin, ancak birkaç fikir aşağıda verilmiştir:

    - Önceden oluşturduğunuz kaynakları görmek için **tüm kaynakları** ekleyin.

    - Birden fazla kuruluşla çalışıyorsanız, kaynakların hangi kuruluşa ait olduğunu açıkça göstermek için panonuza **kuruluş kimliği** kutucuğunu ekleyin.

1. Sayfa üstbilgisinde **Kaydet**' i seçin.

### <a name="add-tiles-from-a-resource-page"></a>Kaynak sayfasından kutucuk ekleme

Panolarınıza kutucuk eklemenin alternatif bir yolu vardır. Birçok kaynak sayfası, komut çubuğunda raptiye simgesi içerir. Simgeyi seçerseniz, kaynak sayfayı temsil eden bir kutucuk Şu anda etkin olan panoya sabitlenmiştir. 

![PIN simgesiyle sayfa komut çubuğunun ekran görüntüsü](./media/azure-portal-dashboards/dashboard-pin-blade.png)

### <a name="resize-or-rearrange-tiles"></a>Kutucukları yeniden boyutlandır veya yeniden Düzenle

Bir kutucuğun boyutunu değiştirmek veya bir panodaki kutucukları yeniden düzenlemek için aşağıdaki adımları izleyin:

1. ![Sayfa başlığından Düzenle simgesini ](./media/azure-portal-dashboards/dashboard-edit-icon.png) **Düzenle** ' yi seçin.

1. Bir kutucuğun sağ üst köşesindeki bağlam menüsünü seçin. Sonra bir kutucuk boyutu seçin. Herhangi bir boyutu destekleyen Kutucuklar, Kutucuğu istediğiniz boyuta sürüklemenize imkan tanıyan sağ alt köşedeki bir "tanıtıcı" da içerir.

    ![Kutucuk boyutu menüsü açık olan panonun ekran görüntüsü](./media/azure-portal-dashboards/dashboard-tile-resize.png)

1. Bir kutucuk seçin ve panonuzu düzenlemek için Kılavuzu kılavuz üzerinde yeni bir konuma sürükleyin.

### <a name="additional-tile-configuration"></a>Ek kutucuk yapılandırması

Bazı Kutucuklar, istediğiniz bilgileri göstermek için daha fazla yapılandırma gerektirebilir. Örneğin, **ölçüm grafiği** kutucuğunun Azure izleyici 'den bir ölçüm görüntülemesi için ayarlanması gerekir. Ayrıca, kutucuk verilerini panonun varsayılan saat ayarlarını geçersiz kılmak için özelleştirebilirsiniz.

Ayarlanması gereken her kutucuk, kutucuğu özelleştirene kadar bir başlık görüntüler. **Ölçüm grafiğinde** başlık, **Ölçümler ' de düzenlenir**. Kutucuğu özelleştirmek için:

1. Sayfa üstbilgisinde, düzenleme modundan çıkmak için **Kaydet** ' i seçin.

1. Başlığı seçin, sonra gerekli Kurulumu yapın.

    ![Yapılandırma gerektiren kutucuğun ekran görüntüsü](./media/azure-portal-dashboards/dashboard-configure-tile.png)

> [!NOTE]
> Markaşağı kutucuk, panonuzda özel ve statik içerik görüntülemenize imkan tanır. Bu temel yönergeler, bir görüntü, bir köprü kümesi veya hatta iletişim bilgileri olabilir. Markaşağı kutucuğun kullanımı hakkında daha fazla bilgi için bkz. [Azure panoları üzerinde markaşağı kutucuk kullanarak özel içerik gösterme](azure-portal-markdown-tile.md).

### <a name="customize-tile-data"></a>Kutucuk verilerini özelleştirme

Panodaki veriler, son 24 saat için etkinliği otomatik olarak gösterir. Yalnızca bu kutucuk için farklı bir zaman aralığı göstermek için şu adımları izleyin:

1. Bağlam  menüsünden veya ![ ](./media/azure-portal-dashboards/dashboard-filter.png) kutucuğun sol üst köşesindeki filtre simgesi filtresinden kutucuk verilerini Özelleştir ' i seçin.

    ![Kutucuk bağlam menüsünün ekran görüntüsü](./media/azure-portal-dashboards/dashboard-customize-tile-data.png)

1. **Kutucuk düzeyinde Pano zaman ayarlarını geçersiz kılmak** için onay kutusunu seçin.

    ![Kutucuk zaman ayarlarını yapılandırmak için iletişim kutusunun ekran görüntüsü](./media/azure-portal-dashboards/dashboard-override-time-settings.png)

1. Bu kutucuk için gösterilecek zaman aralığını seçin. Son 30 dakika ile son 30 gün arasında seçim yapabilir veya özel bir Aralık tanımlayabilirsiniz.

1. Görüntülenecek zaman parçalı yapısını seçin. Tek dakikalık artışlarla bir ay boyunca herhangi bir yere gösterebilirsiniz.

1. **Uygula**’yı seçin.

## <a name="delete-a-tile"></a>Kutucukları silme

Bir panodaki kutucuğu kaldırmak için şu adımları izleyin:

* Kutucuğun sağ üst köşesindeki bağlam menüsünü seçin ve **panodan kaldır**' ı seçin. Veya

* ![ ](./media/azure-portal-dashboards/dashboard-edit-icon.png)  Özelleştirme modunu girmek için Düzenle simgesini Düzenle ' yi seçin. Kutucuğun sağ üst köşesine gelin, sonra ![ ](./media/azure-portal-dashboards/dashboard-delete-icon.png) kutucuğu panodan kaldırmak için simgeyi Sil simgesini seçin.

   ![Panodan kutucuğun nasıl kaldırılacağını gösteren ekran görüntüsü](./media/azure-portal-dashboards/dashboard-delete-tile.png)

## <a name="clone-a-dashboard"></a>Panoyu kopyalama

Mevcut bir panoyu yeni bir Pano için şablon olarak kullanmak için şu adımları izleyin:

1. Pano görünümünün kopyalamak istediğiniz panoyu göstermesini sağlayın.

1. Sayfa üstbilgisinde ![ Kopyala simgesini Kopyala ' yı seçin ](./media/azure-portal-dashboards/dashboard-clone.png) .

1. Panonun **bir kopyası,** *Pano adınızın* kopyası adlı düzenleme modunda açılır. Panoyu yeniden adlandırmak ve özelleştirmek için bu makaledeki yukarıdaki adımları kullanın.

## <a name="publish-and-share-a-dashboard"></a>Bir panoyu yayımlama ve paylaşma

Bir pano oluşturduğunuzda, bu, varsayılan olarak özeldir. Bu, onu görebilen tek bir tane olduğu anlamına gelir. Panoları başkaları için kullanılabilir hale getirmek için, bunları yayımlayabilir ve paylaşabilirsiniz. Daha fazla bilgi için bkz. [Azure 'un rol tabanlı erişim denetimi kullanarak Azure panoları paylaşma](azure-portal-dashboard-share-access.md).

### <a name="open-a-shared-dashboard"></a>Paylaşılan bir panoyu açma

Paylaşılan bir panoyu bulup açmak için şu adımları izleyin:

1. Pano adının yanındaki oku seçin.

1. Görüntülenmiş Pano listesinden seçim yapın. Açmak istediğiniz Pano listede yoksa:

    1. **Tüm panolara gözatamıyorum**' ı seçin.

        ![Pano seçim menüsünün ekran görüntüsü](./media/azure-portal-dashboards/dashboard-browse.png)

    1. **Tür** alanında, **paylaşılan panolar**' ı seçin.

        ![Tüm panolar seçim menüsünün ekran görüntüsü](./media/azure-portal-dashboards/dashboard-browse-all.png)

    1. Bir veya daha fazla abonelik seçin. Panoları ada göre filtrelemek için de metin girebilirsiniz.

    1. Paylaşılan panolar listesinden bir pano seçin.

## <a name="delete-a-dashboard"></a>Panoyu silme

Özel veya paylaşılan bir panoyu kalıcı olarak silmek için şu adımları izleyin:

1. Pano adının yanındaki listeden silmek istediğiniz panoyu seçin.

1. Sayfa üstbilgisindeki Sil simgesini Sil ' i seçin ![ ](./media/azure-portal-dashboards/dashboard-delete-icon.png)  .

1. Özel bir Pano için onay iletişim kutusunda **Tamam** ' ı seçerek panoyu kaldırın. Paylaşılan bir Pano için, onay iletişim kutusunda, yayımlanan panonun artık başkaları tarafından görüntülenemeyeceği onaylanacak onay kutusunu seçin. Ardından **Tamam**'ı seçin.

    ![Silme onayı ekran görüntüsü](./media/azure-portal-dashboards/dashboard-delete-dash.png)

## <a name="recover-a-deleted-dashboard"></a>Silinen bir panoyu kurtarma

Küresel Azure bulutunuz varsa ve Azure portal _yayımlanmış_ bir panoyu silerseniz, bu panoyu silmenin 14 gün içinde kurtarabilirsiniz. Bilgi için, [Azure Portal silinen bir panoyu kurtarma](recover-shared-deleted-dashboard.md)bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure rol tabanlı erişim denetimi kullanarak Azure panoları paylaşma](azure-portal-dashboard-share-access.md)
* [Program aracılığıyla Azure panoları oluşturma](azure-portal-dashboards-create-programmatically.md)
