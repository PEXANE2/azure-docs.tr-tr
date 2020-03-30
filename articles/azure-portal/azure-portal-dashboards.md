---
title: Azure portalında pano oluşturma ve paylaşma
description: Bu makalede, Azure portalında panoların nasıl oluşturulup, özelleştirilen, yayımlanılacağı ve paylaşılacakları açıklanmaktadır.
services: azure-portal
documentationcenter: ''
author: sewatson
manager: mtillman
editor: tysonn
ms.assetid: ff422f36-47d2-409b-8a19-02e24b03ffe7
ms.service: azure-portal
ms.devlang: NA
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 03/23/2020
ms.author: mblythe
ms.openlocfilehash: 4c01321662b302103cdedfb5b78dadf89860fb8e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80132051"
---
# <a name="create-and-share-dashboards-in-the-azure-portal"></a>Azure portalında pano oluşturma ve paylaşma

Panolar, Azure portalındaki bulut kaynaklarınızın odaklanmış ve düzenli bir görünümüdür. Günlük işlemler için görevleri hızla başlatabileceğiniz ve kaynakları izleyebileceğiniz bir çalışma alanı olarak panoları kullanın. Örneğin, projelere, görevlere veya kullanıcı rollerine dayalı özel panolar oluşturun.

Azure portalı başlangıç noktası olarak varsayılan bir pano sağlar. Varsayılan panoyu edinebilirsiniz. Ek panolar oluşturun ve özelleştirin ve panoları diğer kullanıcıların kullanımına açmak için yayımlayın ve paylaşın. Bu makalede, yeni bir pano oluşturmak için nasıl açıklanır, arabirimi özelleştirmek ve yayımlamak ve panoları paylaşmak.

## <a name="create-a-new-dashboard"></a>Yeni pano oluşturma

Bu örnekte, yeni, özel bir pano oluşturur ve bir ad atayız. Kullanmaya başlamak için bu adımları izleyin:

1. [Azure portalında](https://portal.azure.com)oturum açın.

1. Azure portalı menüsünden **Pano'yu**seçin. Varsayılan görünümünüz zaten panoya ayarlanmış olabilir.

    ![Panoyu açma](./media/azure-portal-dashboards/portal-menu-dashboard.png)

1. **Yeni pano**seçin.

    ![Yeni pano ekran görüntüsü](./media/azure-portal-dashboards/create-new-dashboard.png)

    Bu eylem, kutucukları seçeceğiniz **Döşeme Galerisi'ni**ve kutucukları düzenleyeceğiniz boş bir ızgarayı açar.

    ![Döşeme galerisi ve boş ızgara ekran görüntüsü](./media/azure-portal-dashboards/dashboard-name.png)

1. Pano etiketindeki **Panom** metnini seçin ve özel panoyu kolayca tanımlamanıza yardımcı olacak bir ad girin.

1. Edit modundan çıkmak için sayfa üstbilgisinde **Özelleştir'i** seçin.

Pano görünümü artık yeni panonuzu gösterir. Kullanabileceğiniz panoları görmek için pano adının yanındaki oku seçin. Liste, diğer kullanıcıların oluşturduğu ve paylaştığı panolar içerebilir.

## <a name="edit-a-dashboard"></a>Bir panoyu düzenleme

Şimdi, Azure kaynaklarınızı temsil eden kutucuklar eklemek, yeniden boyutlandırmak ve düzenlemek için panonu düzenleyelim.

### <a name="add-tiles-from-the-dashboard"></a>Panodan kutucuk ekleme

Panoya kutucuk eklemek için aşağıdaki adımları izleyin:

1. Sayfa ![](./media/azure-portal-dashboards/dashboard-edit-icon.png) **üstbilgisinden** edit simgesini seçin.

    ![Pano vurgulama editi ekran görüntüsü](./media/azure-portal-dashboards/dashboard-edit.png)

1. **Döşeme Galerisi'ne** göz atın veya istediğiniz döşemeyi bulmak için arama alanını kullanın.

1. Varsayılan boyut ve konuma sahip döşemeyi panoya eklemek için **Ekle'yi** seçin. Veya döşemeyi ızgaraya sürükleyin ve istediğiniz yere yerleştirin.

> [!TIP]
> Birden fazla kuruluşla çalışıyorsanız, kaynakların hangi kuruluşa ait olduğunu açıkça göstermek için panonuza **Kuruluş kimlik** döşemesini ekleyin.

### <a name="add-tiles-from-a-resource-page"></a>Kaynak sayfasından kutucuk ekleme

Panonuza kutucuk eklemenin alternatif bir yolu vardır. Birçok kaynak sayfası komut çubuğunda bir toka simgesi içerir. Simgeyi seçerseniz, kaynak sayfayı temsil eden bir döşeme, şu anda etkin olan panoya sabitlenir. 

![Pin simgesi olan sayfa komut çubuğunun ekran görüntüsü](./media/azure-portal-dashboards/dashboard-pin-blade.png)

### <a name="resize-or-rearrange-tiles"></a>Kutucukları yeniden boyutlandırma veya yeniden düzenleme

Döşemenin boyutunu değiştirmek veya panodaki kutucukları yeniden düzenlemek için aşağıdaki adımları izleyin:

1. Sayfa ![](./media/azure-portal-dashboards/dashboard-edit-icon.png) **üstbilgisinden** edit simgesini seçin.

1. Döşemenin sağ üst köşesindeki bağlam menüsünü seçin. Ardından, bir kiremit boyutu seçin. Herhangi bir boyutu destekleyen kutucuklar, sağ alt köşede kutucuğu istediğiniz boyuta sürüklemenize olanak tanıyan bir "tutamaç" da içerir.

    ![Döşeme boyutu menüsü açık olan pano ekran görüntüsü](./media/azure-portal-dashboards/dashboard-tile-resize.png)

1. Bir döşeme seçin ve panonuzu düzenlemek için ızgaradaki yeni bir konuma sürükleyin.

### <a name="additional-tile-configuration"></a>Ek döşeme yapılandırması

Bazı döşemeler, istediğiniz bilgileri göstermek için daha fazla yapılandırma gerektirebilir. Örneğin, **Azure** Monitörü'nden bir metrik görüntülemek için Ölçümler **Azure Monitor**grafik döşemesinin ayarlanması gerekir. Pano'nun varsayılan zaman ayarlarını geçersiz kılmak için döşeme verilerini özelleştirebilirsiniz.

Ayarlanması gereken herhangi bir döşeme, döşemeyi özelleştirene kadar **Yapılandırılan bir döşeme** başlığı görüntüler. Döşemeyi özelleştirmek için:

1. Edit modundan çıkmak için sayfa üstbilgisinde **Özelleştir'i** seçin.

1. Banner'ı seçin ve gerekli kurulumu yapın.

    ![Yapılandırma gerektiren döşemenin ekran görüntüsü](./media/azure-portal-dashboards/dashboard-configure-tile.png)

> [!NOTE]
> İşaret aşağı döşemesi, panonuzda özel, statik içerik görüntülemenizi sağlar. Bu temel talimatlar, bir görüntü, bir köprü kümesi ve hatta iletişim bilgileri olabilir. İşaret aşağı döşemesi kullanma hakkında daha fazla bilgi için [bkz.](azure-portal-markdown-tile.md)

### <a name="customize-tile-data"></a>Döşeme verilerini özelleştirme

Panodaki veriler, son 24 saatteki etkinliği otomatik olarak gösterir. Sadece bu döşeme için farklı bir zaman aralığı göstermek için aşağıdaki adımları izleyin:

1. Bağlam menüsünden veya döşemenin ![sol üst](./media/azure-portal-dashboards/dashboard-filter.png) köşesinden filtre simgesi filtresinden **döşeme verilerini özelleştir'i** seçin.

    ![Döşeme bağlam menüsünün ekran görüntüsü](./media/azure-portal-dashboards/dashboard-customize-tile-data.png)

1. **Döşeme düzeyindepano zaman ayarlarını geçersiz kılmak**için onay kutusunu seçin.

    ![Döşeme zamanı ayarlarını yapılandırmak için iletişim ekranı](./media/azure-portal-dashboards/dashboard-override-time-settings.png)

1. Bu döşeme için göstermek için zaman aralığını seçin. Son 30 dakika ile son 30 gün arasında seçim yapabilir veya özel bir aralık tanımlayabilirsiniz.

1. Görüntülemek için zaman tanecikliliğini seçin. Bir dakikalık artışlardan bir aya kadar her yerde gösterebilirsiniz.

1. **Uygula**’yı seçin.

## <a name="delete-a-tile"></a>Kutucukları silme

Panodaki döşemeyi kaldırmak için aşağıdaki adımları izleyin:

* Döşemenin sağ üst köşesindeki bağlam menüsünü seçin ve ardından **panodan Kaldır'ı**seçin. Veya

* Özelleştirme ![moduna](./media/azure-portal-dashboards/dashboard-edit-icon.png) girmek için simgeyi **edit'i** seçin. Döşemenin sağ üst köşesinde, ardından panodaki ![döşemeyi](./media/azure-portal-dashboards/dashboard-delete-icon.png) kaldırmak için sil simgesi silme simgesini seçin.

   ![Panodan döşemenin nasıl kaldırılır olduğunu gösteren ekran görüntüsü](./media/azure-portal-dashboards/dashboard-delete-tile.png)

## <a name="clone-a-dashboard"></a>Bir panoyu kopyalama

Varolan bir panoyu yeni bir pano için şablon olarak kullanmak için aşağıdaki adımları izleyin:

1. Pano görünümünün kopyalamak istediğiniz panoyu gösterdiğinden emin olun.

1. Sayfa üstbilgisinde klon ![simgesi](./media/azure-portal-dashboards/dashboard-clone.png) **Klon'u**seçin.

1. *Pano adınızın* **Klon'ı** adlı pano kopyası, edit modunda açılır. Panonu yeniden adlandırmak ve özelleştirmek için bu makalede önceki adımları kullanın.

## <a name="publish-and-share-a-dashboard"></a>Pano yayınlama ve paylaşma

Bir pano oluşturduğunuzda, varsayılan olarak özeldir, bu da onu görebilen tek kişinin siz olduğunuz anlamına gelir. Panoları başkalarının kullanımına açmak için yayımlayabilir ve paylaşabilirsiniz. Daha fazla bilgi için Rol [Tabanlı Erişim Denetimi'ni kullanarak Azure panolarını paylaş'a](azure-portal-dashboard-share-access.md)bakın.

### <a name="open-a-shared-dashboard"></a>Paylaşılan bir pano açma

Paylaşılan bir panoyu bulmak ve açmak için aşağıdaki adımları izleyin:

1. Pano adının yanındaki oku seçin.

1. Görüntülenen panolar listesinden seçim belirleyin. Açmak istediğiniz pano listede yoksa:

    1. **tüm panolara Gözat'ı**seçin.

        ![Pano seçim menüsünün ekran görüntüsü](./media/azure-portal-dashboards/dashboard-browse.png)

    1. **Tür** alanında Paylaşılan **panoları**seçin.

        ![Tüm panoseçim menüsünün ekran görüntüsü](./media/azure-portal-dashboards/dashboard-browse-all.png)

    1. Bir veya daha fazla abonelik seçin. Panoları ada göre filtrelemek için metin de girebilirsiniz.

    1. Paylaşılan panolar listesinden bir pano seçin.

## <a name="delete-a-dashboard"></a>Bir panoyu silme

Özel veya paylaşılan bir panoyu kalıcı olarak silmek için aşağıdaki adımları izleyin:

1. Pano adının yanındaki listeden silmek istediğiniz panonu seçin.

1. Sil ![simgesisayfa](./media/azure-portal-dashboards/dashboard-delete-icon.png) üstbilgisinden **sil'i** seçin.

1. Özel bir pano için, panoyu kaldırmak için onay iletişim kutusunda **Tamam'ı** seçin. Paylaşılan bir pano için, onay iletişim kutusunda, yayınlanan panoartık başkaları tarafından görüntülenebilir olacağını onaylamak için onay kutusunu seçin. Ardından **Tamam'ı**seçin.

    ![Silme onayEkran görüntüsü](./media/azure-portal-dashboards/dashboard-delete-dash.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Rol Tabanlı Erişim Denetimi kullanarak Azure panolarını paylaşma](azure-portal-dashboard-share-access.md)
* [Program aracılığıyla Azure panoları oluşturma](azure-portal-dashboards-create-programmatically.md)
