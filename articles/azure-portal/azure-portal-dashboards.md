---
title: Azure portalında pano oluşturma ve paylaşma
description: Bu makalede, Azure portal panoları oluşturma, özelleştirme, yayımlama ve paylaşma açıklanır.
services: azure-portal
documentationcenter: ''
author: sewatson
manager: mtillman
ms.assetid: ff422f36-47d2-409b-8a19-02e24b03ffe7
ms.service: azure-portal
ms.devlang: NA
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 03/23/2020
ms.author: mblythe
ms.openlocfilehash: e6eda3b522a5b46cd82ef29ae493891a624d3272
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81459296"
---
# <a name="create-and-share-dashboards-in-the-azure-portal"></a>Azure portalında pano oluşturma ve paylaşma

Panolar Azure portal bulut kaynaklarınızın odaklanmış ve düzenlenmiş görünümüdür. Panoları günlük işlemler için hızlı bir şekilde başlatabileceğiniz ve kaynakları izleyebileceğiniz bir çalışma alanı olarak panoları kullanın. Örneğin, projeler, görevler veya Kullanıcı rollerine dayalı özel panolar oluşturun.

Azure portal, başlangıç noktası olarak varsayılan bir pano sağlar. Varsayılan panoyu düzenleyebilirsiniz. Ek panolar oluşturup özelleştirin ve diğer kullanıcıların kullanımına sunmak için panoları yayımlayın ve paylaşabilirsiniz. Bu makalede, yeni bir pano oluşturma, arabirimi özelleştirme ve panoları yayımlama ve paylaşma açıklanır.

## <a name="create-a-new-dashboard"></a>Yeni pano oluşturma

Bu örnekte, yeni, özel bir pano oluşturacağız ve bir ad atamalısınız. Kullanmaya başlamak için bu adımları izleyin:

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Azure portal menüsünde, **Pano**' yı seçin. Varsayılan görünüm zaten pano olarak ayarlanmış olabilir.

    ![Panoyu açma](./media/azure-portal-dashboards/portal-menu-dashboard.png)

1. **Yeni Pano**' yı seçin.

    ![Yeni panonun ekran görüntüsü](./media/azure-portal-dashboards/create-new-dashboard.png)

    Bu eylem, kutucukları seçtiğiniz **kutucuk galerisini**ve kutucukları düzenlediğiniz boş bir Kılavuzu açar.

    ![Kutucuk galerisinin ve boş kılavuzun ekran görüntüsü](./media/azure-portal-dashboards/dashboard-name.png)

1. Pano etiketindeki **My Dashboard** metnini seçin ve özel panoyu kolayca belirlemenize yardımcı olacak bir ad girin.

1. Düzenleme modundan çıkmak için sayfa üstbilgisindeki **özelleştirmeyi bitti** ' yi seçin.

Pano görünümü artık yeni panonuzu gösterir. Kullanılabilir panoları görmek için Pano adının yanındaki oku seçin. Listede, diğer kullanıcıların oluşturduğu ve paylaştığı panolar bulunabilir.

## <a name="edit-a-dashboard"></a>Bir panoyu düzenleme

Şimdi, Azure kaynaklarınızı temsil eden kutucukları eklemek, yeniden boyutlandırmak ve düzenlemek için panoyu düzenleyelim.

### <a name="add-tiles-from-the-dashboard"></a>Panodan kutucuk ekleme

Bir panoya kutucuk eklemek için şu adımları izleyin:

1. Sayfa ![başlığından Düzenle](./media/azure-portal-dashboards/dashboard-edit-icon.png) simgesini **Düzenle** ' yi seçin.

    ![Pano vurgulama düzenleme ekran görüntüsü](./media/azure-portal-dashboards/dashboard-edit.png)

1. **Kutucuk galerisine** gidin veya istediğiniz kutucuğu bulmak için arama alanını kullanın.

1. Kutucuğu varsayılan bir boyut ve konum ile panoya eklemek için **Ekle** ' yi seçin. Ya da kutucuğu kılavuza sürükleyin ve istediğiniz yere yerleştirin.

> [!TIP]
> Birden fazla kuruluşla çalışıyorsanız, kaynakların hangi kuruluşa ait olduğunu açıkça göstermek için panonuza **kuruluş kimliği** kutucuğunu ekleyin.

### <a name="add-tiles-from-a-resource-page"></a>Kaynak sayfasından kutucuk ekleme

Panolarınıza kutucuk eklemenin alternatif bir yolu vardır. Birçok kaynak sayfası, komut çubuğunda raptiye simgesi içerir. Simgeyi seçerseniz, kaynak sayfayı temsil eden bir kutucuk Şu anda etkin olan panoya sabitlenmiştir. 

![PIN simgesiyle sayfa komut çubuğunun ekran görüntüsü](./media/azure-portal-dashboards/dashboard-pin-blade.png)

### <a name="resize-or-rearrange-tiles"></a>Kutucukları yeniden boyutlandır veya yeniden Düzenle

Bir kutucuğun boyutunu değiştirmek veya bir panodaki kutucukları yeniden düzenlemek için aşağıdaki adımları izleyin:

1. Sayfa ![başlığından Düzenle](./media/azure-portal-dashboards/dashboard-edit-icon.png) simgesini **Düzenle** ' yi seçin.

1. Bir kutucuğun sağ üst köşesindeki bağlam menüsünü seçin. Sonra bir kutucuk boyutu seçin. Herhangi bir boyutu destekleyen Kutucuklar, Kutucuğu istediğiniz boyuta sürüklemenize imkan tanıyan sağ alt köşedeki bir "tanıtıcı" da içerir.

    ![Kutucuk boyutu menüsü açık olan panonun ekran görüntüsü](./media/azure-portal-dashboards/dashboard-tile-resize.png)

1. Bir kutucuk seçin ve panonuzu düzenlemek için Kılavuzu kılavuz üzerinde yeni bir konuma sürükleyin.

### <a name="additional-tile-configuration"></a>Ek kutucuk yapılandırması

Bazı Kutucuklar, istediğiniz bilgileri göstermek için daha fazla yapılandırma gerektirebilir. Örneğin, **ölçüm grafiği** kutucuğunun **Azure izleyici**'den bir ölçüm görüntülemesi için ayarlanması gerekir. Ayrıca, kutucuk verilerini panonun varsayılan saat ayarlarını geçersiz kılmak için özelleştirebilirsiniz.

Ayarlanması gereken her kutucuk, kutucuğu özelleştirene kadar **kutucuk başlığını Yapılandır** sayfasını görüntüler. Kutucuğu özelleştirmek için:

1. Düzenleme modundan çıkmak için sayfa üstbilgisindeki **özelleştirmeyi bitti** ' yi seçin.

1. Başlığı seçin, sonra gerekli Kurulumu yapın.

    ![Yapılandırma gerektiren kutucuğun ekran görüntüsü](./media/azure-portal-dashboards/dashboard-configure-tile.png)

> [!NOTE]
> Markaşağı kutucuk, panonuzda özel ve statik içerik görüntülemenize imkan tanır. Bu temel yönergeler, bir görüntü, bir köprü kümesi veya hatta iletişim bilgileri olabilir. Markaşağı kutucuğun kullanımı hakkında daha fazla bilgi için bkz. [Azure panoları üzerinde markaşağı kutucuk kullanarak özel içerik gösterme](azure-portal-markdown-tile.md).

### <a name="customize-tile-data"></a>Kutucuk verilerini özelleştirme

Panodaki veriler, son 24 saat için etkinliği otomatik olarak gösterir. Yalnızca bu kutucuk için farklı bir zaman aralığı göstermek için şu adımları izleyin:

1. Bağlam menüsünden **kutucuk verilerini Özelleştir** ' i veya kutucuğun ![sol üst](./media/azure-portal-dashboards/dashboard-filter.png) köşesinden filtre simgesi filtresini seçin.

    ![Kutucuk bağlam menüsünün ekran görüntüsü](./media/azure-portal-dashboards/dashboard-customize-tile-data.png)

1. **Kutucuk düzeyinde Pano zaman ayarlarını geçersiz kılmak**için onay kutusunu seçin.

    ![Kutucuk zaman ayarlarını yapılandırmak için iletişim kutusunun ekran görüntüsü](./media/azure-portal-dashboards/dashboard-override-time-settings.png)

1. Bu kutucuk için gösterilecek zaman aralığını seçin. Son 30 dakika ile son 30 gün arasında seçim yapabilir veya özel bir Aralık tanımlayabilirsiniz.

1. Görüntülenecek zaman parçalı yapısını seçin. Tek dakikalık artışlarla bir ay boyunca herhangi bir yere gösterebilirsiniz.

1. **Uygula**’yı seçin.

## <a name="delete-a-tile"></a>Kutucukları silme

Bir panodaki kutucuğu kaldırmak için şu adımları izleyin:

* Kutucuğun sağ üst köşesindeki bağlam menüsünü seçin ve **panodan kaldır**' ı seçin. Veya

* Özelleştirme ![modunu girmek](./media/azure-portal-dashboards/dashboard-edit-icon.png) **için Düzenle simgesini Düzenle '** yi seçin. Kutucuğun sağ üst köşesine gelin, sonra kutucuğu panodan kaldırmak için simgeyi ![](./media/azure-portal-dashboards/dashboard-delete-icon.png) Sil simgesini seçin.

   ![Panodan kutucuğun nasıl kaldırılacağını gösteren ekran görüntüsü](./media/azure-portal-dashboards/dashboard-delete-tile.png)

## <a name="clone-a-dashboard"></a>Bir panoyu kopyalama

Mevcut bir panoyu yeni bir Pano için şablon olarak kullanmak için şu adımları izleyin:

1. Pano görünümünün kopyalamak istediğiniz panoyu göstermesini sağlayın.

1. Sayfa üstbilgisinde Kopyala simgesini ![](./media/azure-portal-dashboards/dashboard-clone.png) **Kopyala**' yı seçin.

1. Panonun **bir kopyası,** *Pano adınızın* kopyası adlı düzenleme modunda açılır. Panoyu yeniden adlandırmak ve özelleştirmek için bu makaledeki yukarıdaki adımları kullanın.

## <a name="publish-and-share-a-dashboard"></a>Bir panoyu yayımlama ve paylaşma

Bir pano oluşturduğunuzda, bu, varsayılan olarak özeldir. Bu, onu görebilen tek bir tane olduğu anlamına gelir. Panoları başkaları için kullanılabilir hale getirmek için, bunları yayımlayabilir ve paylaşabilirsiniz. Daha fazla bilgi için bkz. [rol tabanlı Access Control kullanarak Azure panoları paylaşma](azure-portal-dashboard-share-access.md).

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

## <a name="delete-a-dashboard"></a>Bir panoyu silme

Özel veya paylaşılan bir panoyu kalıcı olarak silmek için şu adımları izleyin:

1. Pano adının yanındaki listeden silmek istediğiniz panoyu seçin.

1. Sayfa ![üstbilgisindeki Sil](./media/azure-portal-dashboards/dashboard-delete-icon.png) **simgesini Sil** ' i seçin.

1. Özel bir Pano için onay iletişim kutusunda **Tamam** ' ı seçerek panoyu kaldırın. Paylaşılan bir Pano için, onay iletişim kutusunda, yayımlanan panonun artık başkaları tarafından görüntülenemeyeceği onaylanacak onay kutusunu seçin. Ardından **Tamam**' ı seçin.

    ![Silme onayı ekran görüntüsü](./media/azure-portal-dashboards/dashboard-delete-dash.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Rol Tabanlı Erişim Denetimi kullanarak Azure panolarını paylaşma](azure-portal-dashboard-share-access.md)
* [Program aracılığıyla Azure panoları oluşturma](azure-portal-dashboards-create-programmatically.md)
