---
title: Azure portal panoları oluşturma ve paylaşma | Microsoft Docs
description: Bu makalede, Azure portal panoları oluşturma, özelleştirme, yayımlama ve paylaşma açıklanır.
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
ms.date: 07/01/2019
ms.author: mblythe
ms.openlocfilehash: a3b4d7cb33bf0da0c4431d76a54644208ea6468f
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75640465"
---
# <a name="create-and-share-dashboards-in-the-azure-portal"></a>Azure portalında pano oluşturma ve paylaşma

Panolar, bulut kaynaklarınızın Azure portal odaklanmış ve düzenlenmiş bir görünüm oluşturmanız için bir yol sağlar. Panoları günlük işlemler için hızlı bir şekilde başlatabileceğiniz ve kaynakları izleyebileceğiniz bir çalışma alanı olarak panoları kullanın.  Örneğin, projeler, görevler veya Kullanıcı rollerine dayalı özel panolar oluşturun.  Azure portal, başlangıç noktası olarak varsayılan bir pano sağlar. Varsayılan panoyu düzenleyebilir, ek panolar oluşturabilir ve özelleştirebilir, diğer kullanıcıların kullanımına sunmak için panoları yayımlayabilir ve paylaşabilirsiniz. Bu makalede, yeni bir pano oluşturma, arabirimi özelleştirme ve panoları yayımlama ve paylaşma açıklanır.

## <a name="create-a-new-dashboard"></a>Yeni pano oluşturma

Bu örnekte, yeni, özel bir pano oluşturacağız ve bir ad atamalısınız. Kullanmaya başlamak için bu adımları izleyin:

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.
1. Sol kenar çubuğunun üst kısmından **Pano** ' yı seçin. Varsayılan görünüm zaten pano olarak ayarlanmış olabilir.
1. **+ Yeni Pano**' yı seçin.

    ![Varsayılan panonun ekran görüntüsü](./media/azure-portal-dashboards/dashboard-new.png)

4. Bu eylem, kutucukları seçtiğiniz **kutucuk galerisini**ve kutucukları düzenlediğiniz boş bir Kılavuzu açar.

    ![Kutucuk galerisinin ve boş kılavuzun ekran görüntüsü](./media/azure-portal-dashboards/dashboard-name.png)

5. Pano etiketindeki **My Dashboard** metnini seçin ve özel panoyu kolayca belirlemenize yardımcı olacak bir ad girin.
1. Düzenleme modundan çıkmak için sayfa üstbilgisindeki **özelleştirmeyi bitti** ' yi seçin.

Artık Pano görünümünde boş pano görüntülenir. Kullanılabilir panoları görmek için Pano adının yanındaki açılan listeyi seçin. listede, diğer kullanıcıların oluşturduğu ve paylaştığı panolar bulunabilir.

## <a name="edit-a-dashboard"></a>Panoyu düzenleme

Şimdi, Azure kaynaklarınızı temsil eden kutucukları eklemek, yeniden boyutlandırmak ve düzenlemek için panoyu düzenleyelim.

### <a name="add-tiles"></a>Kutucuk ekle

Bir panoya kutucuk eklemek için şu adımları izleyin:
1. Sayfa başlığından **düzenlemek**](./media/azure-portal-dashboards/dashboard-edit-icon.png) ![Düzenle simgesini seçin.

    ![Pano vurgulama düzenleme ekran görüntüsü](./media/azure-portal-dashboards/dashboard-edit.png)

2. **Kutucuk galerisine** gidin veya istediğiniz kutucuğu bulmak için arama alanını kullanın.
1. Kutucuğu varsayılan bir boyut ve konum ile panoya otomatik olarak eklemek için **Ekle** ' yi seçin. Ya da kutucuğu kılavuza sürükleyin ve istediğiniz yere yerleştirin.

Birçok kaynak sayfası ("Blade" olarak da bilinir), komut çubuğunda bir raptiye simgesi içerir. Simgeyi seçerseniz, kaynak sayfayı temsil eden bir kutucuk Şu anda etkin olan panoya sabitlenmiştir. Bu yöntem, panonuza kutucuk eklemenin alternatif bir yoludur.

![PIN simgesiyle sayfa komut çubuğunun ekran görüntüsü](./media/azure-portal-dashboards/dashboard-pin-blade.png)

> [!TIP]
> Birden fazla kuruluşla çalışıyorsanız, kaynakların hangi kuruluşa ait olduğunu açıkça göstermek için panonuza **kuruluş kimliği** kutucuğunu ekleyin.
>
>
### <a name="resize-or-rearrange-tiles"></a>Kutucukları yeniden boyutlandır veya yeniden Düzenle
Bir kutucuğun boyutunu değiştirmek veya bir panodaki kutucukları yeniden düzenlemek için aşağıdaki adımları izleyin:

1. Sayfa başlığından **düzenlemek**](./media/azure-portal-dashboards/dashboard-edit-icon.png) ![Düzenle simgesini seçin.
1. Bir kutucuğun sağ üst köşesindeki bağlam menüsünü seçin. Sonra bir kutucuk boyutu seçin. Herhangi bir boyutu destekleyen Kutucuklar, Kutucuğu istediğiniz boyuta sürüklemenize imkan tanıyan sağ alt köşedeki bir "tanıtıcı" da içerir.

   ![Kutucuk boyutu menüsü açık olan panonun ekran görüntüsü](./media/azure-portal-dashboards/dashboard-tile-resize.png)

3. Bir kutucuk seçin ve panonuzu düzenlemek için Kılavuzu kılavuz üzerinde yeni bir konuma sürükleyin.

### <a name="additional-tile-configuration"></a>Ek kutucuk yapılandırması

Bazı kutucukların istediğiniz bilgileri göstermesi için daha fazla yapılandırma gerekebilir. Örneğin, **ölçüm grafiği** kutucuğunun **Azure izleyici**'den bir ölçüm görüntülemesi için ayarlanması gerekir. Ayrıca, kutucuk verilerini panonun varsayılan saat ayarlarını geçersiz kılmak için özelleştirebilirsiniz.

Ayarlanması gereken her kutucuk, kutucuğu özelleştirene kadar **kutucuk başlığını Yapılandır** sayfasını görüntüler. Bu başlığı seçin, sonra gerekli Kurulumu yapın.

![Yapılandırma gerektiren kutucuğun ekran görüntüsü](./media/azure-portal-dashboards/dashboard-configure-tile.png)

> [!NOTE]
> Markaşağı kutucuk, panonuzda özel ve statik içerik görüntülemenize imkan tanır. Bu temel yönergeler, bir görüntü, bir köprü kümesi veya hatta iletişim bilgileri olabilir. Markaşağı kutucuğu kullanma hakkında daha fazla bilgi için bkz. [özel markı kutucuğu kullanma](azure-portal-markdown-tile.md).
>
>
### <a name="customize-tile-data"></a>Kutucuk verilerini özelleştirme

Panodaki veriler, son 24 saat için etkinliği otomatik olarak gösterir. Yalnızca bu kutucuk için farklı bir zaman aralığı göstermek için şu adımları izleyin:

1. Kutucuğun sol üst köşesinden ![filtre simgesine](./media/azure-portal-dashboards/dashboard-filter.png) filtre simgesini seçin ya da bağlam menüsünde **kutucuk verilerini Özelleştir** ' i seçin.

   ![Kutucuk bağlam menüsünün ekran görüntüsü](./media/azure-portal-dashboards/dashboard-customize-tile-data.png)

2. **Kutucuk düzeyinde Pano zaman ayarlarını geçersiz kılmak**için onay kutusunu seçin.

   ![Kutucuk zaman ayarlarını yapılandırmak için iletişim kutusunun ekran görüntüsü](./media/azure-portal-dashboards/dashboard-override-time-settings.png)

3. Bu kutucuk için gösterilecek zaman aralığını seçin. Son 30 dakika ile son 30 gün arasında seçim yapabilir veya özel bir Aralık tanımlayabilirsiniz.
1. Görüntülenecek zaman parçalı yapısını seçin. Tek dakikalık artışlarla bir ay boyunca herhangi bir yere gösterebilirsiniz.
1. **Uygula**’yı seçin.

## <a name="delete-a-tile"></a>Kutucuğu silme

Bir panodaki kutucuğu kaldırmak için şu adımları izleyin:

* Kutucuğun sağ üst köşesindeki bağlam menüsünü seçin ve **panodan kaldır**' ı seçin. Veya
* Özelleştirme moduna girmek için **düzenle**](./media/azure-portal-dashboards/dashboard-edit-icon.png) ![Düzenle simgesini seçin. Kutucuğun sağ üst köşesine gelin, ardından panoyu panodan kaldırmak için ![Sil simgesini](./media/azure-portal-dashboards/dashboard-delete-icon.png) Sil simgesini seçin.

   ![Panodan kutucuğun nasıl kaldırılacağını gösteren ekran görüntüsü](./media/azure-portal-dashboards/dashboard-delete-tile.png)

## <a name="clone-a-dashboard"></a>Bir panoyu kopyalama

Mevcut bir panoyu yeni bir Pano için şablon olarak kullanmak için şu adımları izleyin:

1. Pano görünümünün kopyalamak istediğiniz panoyu göstermesini sağlayın.
1. Sayfa üstbilgisinde ![Kopyala simgesini](./media/azure-portal-dashboards/dashboard-clone.png) **Kopyala**' yı seçin.
1. Panonun " *Pano adınızın*kopyası" adlı bir kopyası düzenleme modunda açılır. Panoyu yeniden adlandırmak ve özelleştirmek için bu makaledeki yukarıdaki adımları kullanın.

## <a name="publish-and-share-a-dashboard"></a>Bir panoyu yayımlama ve paylaşma

Bir pano oluşturduğunuzda, bu, varsayılan olarak özeldir. Bu, onu görebilen tek bir tane olduğu anlamına gelir. Panoları başkaları için kullanılabilir hale getirmek için, bunları diğer kullanıcılarla paylaşabilirsiniz. İlk olarak, Panoyu bir Azure kaynağı olarak yayımlamanız gerekir. Özel bir panoyu yayımlamak ve paylaşmak için aşağıdaki adımları izleyin:

1. Sayfa üstbilgisindeki ![paylaşma simgesini](./media/azure-portal-dashboards/dashboard-share-icon.png) **Share** ' u seçin. **Paylaşım ve erişim denetimi** formu görüntülenir.
1. Doğru Pano adının gösterildiğini doğrulayın.
1. **Abonelik adı**seçin. Aboneliğe erişimi olan kullanıcılar paylaşılan panoyu kullanabilir. Tek tek kutucukları temsil eden kaynaklara erişim, Azure rol tabanlı erişim denetimi tarafından belirlenir.
1. Bu panoyu seçili abonelik için ' panolar ' kaynak grubunda yayımlamak için onay kutusunu seçin. İsterseniz onay kutusunu temizleyin ve bunun yerine var olan bir kaynak grubuna yayımlamayı seçin.
1. Pano kaynağı için bir konum seçin. Panoyu diğer kaynaklarla bulmanızı öneririz. Note: mevcut kaynak gruplarından seçim yaparsanız, pano bu kaynak grubuyla otomatik olarak bulunur.
1. **Yayımla**’yı seçin.

   ![Pano yayımlama iletişim kutusunun ekran görüntüsü](./media/azure-portal-dashboards/dashboard-publish.png)

### <a name="set-access-control-on-a-shared-dashboard"></a>Paylaşılan bir panoda erişim denetimini ayarlama

Pano yayımlandıktan sonra, aşağıdaki adımları izleyerek panoya kimlerin erişebileceğini yönetin:

1. **Paylaşım + erişim denetim** bölmesinde **Kullanıcıları Yönet**' i seçin.

   ![Pano paylaşımı + erişim denetimi iletişim kutusunun ekran görüntüsü](./media/azure-portal-dashboards/dashboard-share-access-control.png)

2. **Access Control** sayfası açılır. Bu sayfada, birisi için erişim düzeyini gözden geçirebilir veya yeni bir rol ataması ekleyebilirsiniz. Buraya bir rol ataması eklediğinizde, panoya izin vermiş olursunuz.

> [!NOTE]
> Kutucuklar, kuruluşunuzdaki kaynakların temsili görünümleridir. Kaynaklara erişim rol tabanlı erişim denetimi ataması aracılığıyla yönetilir ve izinler abonelikten kaynağa doğru devralınır. Bir panoya erişim verilmesi, panoda gösterilen kaynaklara otomatik olarak izin atamaz. Paylaşılan panolar ve kaynaklar için rol tabanlı erişim denetimi izinleri hakkında daha fazla bilgi için bkz. [rol tabanlı erişim denetimi ile panoları paylaşma](azure-portal-dashboard-share-access.md).

### <a name="open-a-shared-dashboard"></a>Paylaşılan bir panoyu açma

Paylaşılan bir panoyu bulup açmak için şu adımları izleyin:

1. Pano adının yanındaki açılan seçimi seçin.
1. Açmak istediğiniz Pano listede yoksa, görünen panolar listesinden seçin veya **Tüm panolara gidin** .

   ![Pano seçim menüsünün ekran görüntüsü](./media/azure-portal-dashboards/dashboard-browse.png)

3. **Tür** alanında, **paylaşılan panolar**' ı seçin.
1. Bir veya daha fazla abonelik seçin. Panoları ada göre filtrelemek için de metin girebilirsiniz.
1. Paylaşılan panolar listesinden bir pano seçin.

## <a name="delete-a-dashboard"></a>Panoyu silme

Özel veya paylaşılan bir panoyu kalıcı olarak silmek için şu adımları izleyin:

1. Pano adının yanındaki aşağı açılan listeden silmek istediğiniz panoyu seçin.
1. Sayfa başlığından **silmek**](./media/azure-portal-dashboards/dashboard-delete-icon.png) ![Sil simgesini seçin.
1. Özel bir Pano için onay iletişim kutusunda **Tamam** ' ı seçerek panoyu kaldırın. Paylaşılan bir Pano için, onay iletişim kutusunda, yayımlanan panonun artık başkaları tarafından görüntülenemeyeceği onaylanacak onay kutusunu seçin. Sonra **Tamam**’ı seçin.

   ![Silme onayı ekran görüntüsü](./media/azure-portal-dashboards/dashboard-delete-dash.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Rol tabanlı erişim denetimi ile panoları paylaşma](azure-portal-dashboard-share-access.md)
* [Program aracılığıyla Azure panoları oluşturma](azure-portal-dashboards-create-programmatically.md)
