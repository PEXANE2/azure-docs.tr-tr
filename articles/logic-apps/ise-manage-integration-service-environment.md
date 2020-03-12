---
title: Azure Logic Apps tümleştirme hizmeti ortamlarını yönetme
description: Azure Logic Apps için tümleştirme hizmeti ortamınızda (ıSE) ağ durumunu kontrol edin ve Logic Apps, bağlantılar, özel bağlayıcılar ve tümleştirme hesaplarını yönetin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 03/11/2020
ms.openlocfilehash: f48106be67763c093a183be01098cab74391752e
ms.sourcegitcommit: f97d3d1faf56fb80e5f901cd82c02189f95b3486
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79126856"
---
# <a name="manage-your-integration-service-environment-ise-in-azure-logic-apps"></a>Azure Logic Apps tümleştirme hizmeti ortamınızı (ıSE) yönetme

Bu makalede, [tümleştirme hizmeti ortamınız (ıSE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)için yönetim görevlerinin nasıl gerçekleştirileceği gösterilmektedir, örneğin:

* Mantıksal uygulamalar, bağlantılar, tümleştirme hesapları ve çalışma ortamınızdaki bağlayıcılar gibi kaynakları yönetin.
* ISE 'nin ağ sistem durumunu kontrol edin.
* Kapasite ekleyin, ıSE 'nizi yeniden başlatın veya ıSE 'yi silin, bu konudaki adımları izleyin. Bu yapıtları ıSE 'nize eklemek için bkz. [Integration Service ortamınıza yapıt ekleme](../logic-apps/add-artifacts-integration-service-environment-ise.md).

## <a name="view-your-ise"></a>ISE 'nizi görüntüleme

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Portalın arama kutusuna "tümleştirme hizmeti ortamları" yazın ve ardından **tümleştirme hizmeti ortamları**' nı seçin.

   ![Tümleştirme hizmeti ortamlarını bulun](./media/ise-manage-integration-service-environment/find-integration-service-environment.png)

1. Sonuçlar listesinden tümleştirme hizmeti ortamınızı seçin.

   ![Tümleştirme hizmeti ortamını seçin](./media/ise-manage-integration-service-environment/select-integration-service-environment.png)

1. Şirket içinde Logic Apps, bağlantılar, bağlayıcılar veya tümleştirme hesapları bulmak için sonraki bölümlere devam edin.

<a name="check-network-health"></a>

## <a name="check-network-health"></a>Ağ durumunu denetle

ISE menüsünde **Ayarlar**' ın altında **ağ durumu**' nu seçin. Bu bölmede, alt ağlarınızın sistem durumu ve diğer hizmetlere giden bağımlılıklarınız gösterilir.

![Ağ durumunu denetle](./media/ise-manage-integration-service-environment/ise-check-network-health.png)

<a name="find-logic-apps"></a>

## <a name="manage-your-logic-apps"></a>Mantıksal uygulamalarınızı yönetin

ISE 'de olan Logic Apps 'i görüntüleyebilir ve yönetebilirsiniz.

1. ISE menüsünde, **Ayarlar**' ın altında **Logic Apps**' i seçin.

   ![Mantıksal uygulamaları görüntüleme](./media/ise-manage-integration-service-environment/ise-find-logic-apps.png)

1. Artık ihtiyacınız olmayan Logic Apps 'i kaldırmak için bu Logic Apps ' i seçin ve **Sil**' i seçin. Silmek istediğinizi onaylamak için **Evet**' i seçin.

<a name="find-api-connections"></a>

## <a name="manage-api-connections"></a>API bağlantılarını yönetme

ISE 'de çalışan Logic Apps tarafından oluşturulan bağlantıları görüntüleyebilir ve yönetebilirsiniz.

1. ISE menüsünde **Ayarlar**' ın altında **API bağlantıları**' nı seçin.

   ![API bağlantılarını görüntüle](./media/ise-manage-integration-service-environment/ise-find-api-connections.png)

1. Artık ihtiyacınız olmayan bağlantıları kaldırmak için bu bağlantıları seçin ve **Sil**' i seçin. Silmek istediğinizi onaylamak için **Evet**' i seçin.

<a name="manage-api-connectors"></a>

## <a name="manage-ise-connectors"></a>ISE bağlayıcılarını yönetme

ISE 'nize dağıtılan API bağlayıcılarını görüntüleyebilir ve yönetebilirsiniz.

1. ISE menüsünde, **Ayarlar**' ın altında, **yönetilen bağlayıcılar**' ı seçin.

   ![Yönetilen bağlayıcıları görüntüleme](./media/ise-manage-integration-service-environment/ise-view-managed-connectors.png)

1. ISE 'de kullanılabilir olmasını istemediğiniz bağlayıcıları kaldırmak için bu bağlayıcıları seçin ve **Sil**' i seçin. Silmek istediğinizi onaylamak için **Evet**' i seçin.

<a name="find-custom-connectors"></a>

## <a name="manage-custom-connectors"></a>Özel bağlayıcıları yönetme

ISE 'nize dağıttığınız özel bağlayıcıları görüntüleyebilir ve yönetebilirsiniz.

1. ISE menüsünde **Ayarlar**' ın altında **özel bağlayıcılar**' ı seçin.

   ![Özel bağlayıcıları bulma](./media/ise-manage-integration-service-environment/ise-find-custom-connectors.png)

1. Artık ihtiyacınız olmayan özel bağlayıcıları kaldırmak için bu Bağlayıcılar ' ı seçin ve **Sil**' i seçin. Silmek istediğinizi onaylamak için **Evet**' i seçin.

<a name="find-integration-accounts"></a>

## <a name="manage-integration-accounts"></a>Tümleştirme hesaplarını yönetme

1. ISE menüsünde **Ayarlar**' ın altında **tümleştirme hesapları**' nı seçin.

   ![Tümleştirme hesaplarını bul](./media/ise-manage-integration-service-environment/ise-find-integration-accounts.png)

1. Artık gerekmeyen tümleştirme hesaplarını ıSE bilgisayarınızdan kaldırmak için bu tümleştirme hesaplarını seçin ve **Sil**' i seçin.

<a name="add-capacity"></a>

## <a name="add-ise-capacity"></a>ISE kapasitesi ekleme

Premium ıSE temel birimi sabit kapasiteye sahiptir, bu nedenle daha fazla işleme ihtiyacınız varsa, oluşturma sırasında veya daha sonra daha fazla ölçek birimi ekleyebilirsiniz. Geliştirici SKU 'SU ölçek birimleri ekleme özelliğini içermez.

1. [Azure Portal](https://portal.azure.com), Ise 'ye gidin.

1. ISE 'nizin kullanım ve performans ölçümlerini gözden geçirmek için, ıSE menüsünde **genel bakış**' ı seçin.

   ![ISE için kullanımı görüntüleme](./media/ise-manage-integration-service-environment/integration-service-environment-usage.png)

1. **Ayarlar**altında **ölçeği Genişlet**' i seçin. **Yapılandır** bölmesinde, şu seçeneklerden birini seçin:

   * [**El ile ölçeklendirme**](#manual-scale): kullanmak istediğiniz işleme birimi sayısına göre ölçeklendirin.
   * [**Özel otomatik ölçeklendirme**](#custom-autoscale): çeşitli ölçütlerden seçerek ve bu ölçütü karşılamak için eşik koşullarını belirterek performans ölçümlerine göre ölçeklendirin.

   ![İstediğiniz ölçekleme türünü seçin](./media/ise-manage-integration-service-environment/select-scale-out-options.png)

<a name="manual-scale"></a>

### <a name="manual-scale"></a>El ile ölçeklendirme

1. **El ile ölçek**seçeneğini belirledikten sonra **ek kapasite**için, kullanmak istediğiniz ölçekleme birimi sayısını seçin.

   ![İstediğiniz ölçekleme türünü seçin](./media/ise-manage-integration-service-environment/select-manual-scale-out-units.png)

1. İşiniz bittiğinde **Kaydet**'i seçin.

<a name="custom-autoscale"></a>

### <a name="custom-autoscale"></a>Özel otomatik ölçeklendirme

1. Otomatik **Otomatik ölçeklendirme**seçeneğini belirledikten sonra **Otomatik ölçeklendirme ayarı adı**için, ayarınız için bir ad girin ve isteğe bağlı olarak, ayarın ait olduğu Azure kaynak grubunu seçin.

   ![Otomatik ölçeklendirme ayarı için ad belirtin ve kaynak grubunu seçin](./media/ise-manage-integration-service-environment/select-custom-autoscale.png)

1. **Varsayılan** koşul için, **ölçüm temelinde ölçek** ' i veya **belirli bir örnek sayısına göre ölçeklendirin**' ı seçin.

   * Örnek tabanlı ' i seçerseniz, işleme birimlerinin numarasını 0 ile 10 arasında bir değer olacak şekilde girin.

   * Ölçüm tabanlı ' i seçerseniz, aşağıdaki adımları izleyin:

     1. **Kurallar** bölümünde **Kural Ekle**' yi seçin.

     1. **Kural ölçekleme** bölmesinde, kural tetiklendiğinde gerçekleştirilecek ölçütlerinizi ve eyleminizi ayarlayın.

     1. **Örnek sınırları**için şu değerleri belirtin:

        * **En az**: kullanılacak en düşük işleme birimi sayısı
        * **Maksimum**: kullanılacak en fazla işleme birimi sayısı
        * **Varsayılan**: kaynak ölçümleri okunurken herhangi bir sorun meydana geliyorsa ve geçerli kapasite varsayılan kapasitenin altındaysa, otomatik ölçeklendirme varsayılan işlem birimi sayısına göre ölçeklendirilir. Ancak, geçerli kapasite varsayılan kapasiteyi aşarsa, otomatik ölçeklendirme içinde ölçeklenmez.

1. Başka bir koşul eklemek için **Ölçek koşulu Ekle**' yi seçin.

1. Otomatik ölçeklendirme ayarlarınızla işiniz bittiğinde değişikliklerinizi kaydedin.

<a name="restart-ISE"></a>

## <a name="restart-ise"></a>ISE 'yi yeniden Başlat

DNS sunucunuzu veya DNS sunucusu ayarlarını değiştirirseniz, ıSE 'nin bu değişiklikleri görebilmesi için ıSE 'nizi yeniden başlatmanız gerekir. Bir Premium SKU 'nun yeniden başlatılması, geri dönüştürme sırasında tek seferde yeniden başlayan artıklık ve bileşenler nedeniyle kapalı kalma süresine neden olmaz. Ancak, bir geliştirici SKU 'SU mevcut olmadığından kesinti yaşar. Daha fazla bilgi için bkz. [Ise SKU 'ları](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#ise-level).

1. [Azure Portal](https://portal.azure.com), Ise 'ye gidin.

1. ISE menüsünde **genel bakış**' ı seçin. Genel Bakış araç çubuğunda **yeniden başlatın**.

   ![Tümleştirme hizmeti ortamını yeniden Başlat](./media/connect-virtual-network-vnet-isolated-environment/restart-integration-service-environment.png)

<a name="delete-ise"></a>

## <a name="delete-ise"></a>ISE 'yi Sil

Artık ihtiyacınız olmayan bir ıSE veya bir ıSE içeren bir Azure Kaynak grubu silmeden önce, bu öğeler silmeyi engelleyebileceğinden, bu kaynakları içeren Azure Kaynak grubunda veya Azure sanal ağınızda ilke veya kilit olmadığından emin olun.

ISE 'nizi sildikten sonra, Azure Sanal ağınızı veya alt ağlarını silmeyi denemeden önce 9 saate kadar beklemeniz gerekebilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Tümleştirme hizmeti ortamlarına kaynak ekleme](../logic-apps/add-artifacts-integration-service-environment-ise.md)