---
title: Azure Logic Apps'ta entegrasyon hizmeti ortamlarını yönetme
description: Azure Logic Apps için ağ durumunu kontrol edin ve mantıksal uygulamaları, bağlantıları, özel bağlayıcıları ve tümleştirme hesaplarını entegrasyon hizmet ortamınızda (ISE) yönetin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 03/11/2020
ms.openlocfilehash: f48106be67763c093a183be01098cab74391752e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79284206"
---
# <a name="manage-your-integration-service-environment-ise-in-azure-logic-apps"></a>Azure Logic Apps'ta entegrasyon hizmet ortamınızı (ISE) yönetme

Bu makalede, [entegrasyon hizmet ortamınız (İmKB)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)için yönetim görevlerinin nasıl gerçekleştirildi gösterilmektedir, örneğin:

* İmKB'inizdeki mantık uygulamaları, bağlantılar, tümleştirme hesapları ve bağlayıcılar gibi kaynakları yönetin.
* İmKB ağ durumunu kontrol edin.
* Kapasite ekleyin, İmKB'nizi yeniden başlatın veya İmKB'nizi silin, bu konudaki adımları izleyin. Bu yapıları İmKB'nize eklemek [için](../logic-apps/add-artifacts-integration-service-environment-ise.md)bkz.

## <a name="view-your-ise"></a>İmKB'inizi görüntüleyin

1. [Azure portalında](https://portal.azure.com)oturum açın.

1. Portalın arama kutusuna "tümleştirme hizmeti ortamları" girin ve ardından **Tümleştirme Hizmeti Ortamlarını**seçin.

   ![Entegrasyon hizmeti ortamlarını bulma](./media/ise-manage-integration-service-environment/find-integration-service-environment.png)

1. Sonuç listesinden, entegrasyon hizmeti ortamınızı seçin.

   ![Entegrasyon hizmeti ortamını seçin](./media/ise-manage-integration-service-environment/select-integration-service-environment.png)

1. İmKB'inizde mantık uygulamaları, bağlantılar, bağlayıcılar veya tümleştirme hesaplarını bulmak için sonraki bölümlere devam edin.

<a name="check-network-health"></a>

## <a name="check-network-health"></a>Ağ durumunu denetleme

İmKB menünüzde **Ayarlar**altında **Ağ durumunu**seçin. Bu bölme, alt ağlarınız ve diğer hizmetlere giden bağımlılıklarınızın sistem durumunu gösterir.

![Ağ durumunu denetleme](./media/ise-manage-integration-service-environment/ise-check-network-health.png)

<a name="find-logic-apps"></a>

## <a name="manage-your-logic-apps"></a>Mantık uygulamalarınızı yönetme

İmKB'inizdeki mantık uygulamalarını görüntüleyebilir ve yönetebilirsiniz.

1. İmKB menünüzde **Ayarlar**altında **Mantık uygulamalarını**seçin.

   ![Mantık uygulamalarını görüntüleme](./media/ise-manage-integration-service-environment/ise-find-logic-apps.png)

1. İmKB'nizde artık ihtiyacınız olmayan mantık uygulamalarını kaldırmak için bu mantık uygulamalarını seçin ve ardından **Sil'i**seçin. Silmek istediğinizi doğrulamak için **Evet'i**seçin.

<a name="find-api-connections"></a>

## <a name="manage-api-connections"></a>API bağlantılarını yönetme

İmKB'nizde çalışan mantık uygulamaları tarafından oluşturulan bağlantıları görüntüleyebilir ve yönetebilirsiniz.

1. İmKB menünüzde **Ayarlar**altında **API bağlantılarını**seçin.

   ![API bağlantılarını görüntüleme](./media/ise-manage-integration-service-environment/ise-find-api-connections.png)

1. İmKB'nizde artık ihtiyacınız olmayan bağlantıları kaldırmak için bu bağlantıları seçin ve ardından **Sil'i**seçin. Silmek istediğinizi doğrulamak için **Evet'i**seçin.

<a name="manage-api-connectors"></a>

## <a name="manage-ise-connectors"></a>İmKB konektörlerini yönetme

İmKB'nize dağıtılan API bağlayıcılarını görüntüleyebilir ve yönetebilirsiniz.

1. İmKB menünüzde **Ayarlar**altında **Yönetilen bağlayıcıları**seçin.

   ![Yönetilen konektörleri görüntüleme](./media/ise-manage-integration-service-environment/ise-view-managed-connectors.png)

1. İmKB'nizde bulunmasını istemediğiniz bağlayıcıları kaldırmak için bu bağlayıcıları seçin ve ardından **Sil'i**seçin. Silmek istediğinizi doğrulamak için **Evet'i**seçin.

<a name="find-custom-connectors"></a>

## <a name="manage-custom-connectors"></a>Özel konektörleri yönetme

İmKB'nize dağıttığınız özel konektörleri görüntüleyebilir ve yönetebilirsiniz.

1. Ise menünüzde **Ayarlar**altında **Özel bağlayıcıları**seçin.

   ![Özel bağlayıcıları bulma](./media/ise-manage-integration-service-environment/ise-find-custom-connectors.png)

1. İmKB'nizde artık ihtiyacınız olmayan özel bağlayıcıları kaldırmak için bu bağlayıcıları seçin ve ardından **Sil'i**seçin. Silmek istediğinizi doğrulamak için **Evet'i**seçin.

<a name="find-integration-accounts"></a>

## <a name="manage-integration-accounts"></a>Tümleştirme hesaplarını yönetme

1. İmKB menünüzde **Ayarlar**altında **Tümleştirme hesaplarını**seçin.

   ![Tümleştirme hesaplarını bulma](./media/ise-manage-integration-service-environment/ise-find-integration-accounts.png)

1. Artık gerekmediğinde bayrışmama hesaplarını İmKB'nizden kaldırmak için bu tümleştirme hesaplarını seçin ve ardından **Sil'i**seçin.

<a name="add-capacity"></a>

## <a name="add-ise-capacity"></a>ISE kapasitesi ekleyin

Premium ISE taban ünitesi sabit kapasiteye sahiptir, bu nedenle daha fazla iş elde etme gereksinimi duyarsanız, oluşturma sırasında veya sonrasında daha fazla ölçek birimi ekleyebilirsiniz. Geliştirici SKU ölçek birimleri ekleme özelliğiiçermez.

1. Azure [portalında](https://portal.azure.com)ISE'nize gidin.

1. İmKB'nizin kullanım ve performans ölçümlerini gözden geçirmek için İmKB menünüzde **Genel Bakış'ı**seçin.

   ![İmKB için kullanımı görüntüleyin](./media/ise-manage-integration-service-environment/integration-service-environment-usage.png)

1. **Ayarlar**altında, **Ölçeklendir'i**seçin. **Yapıla** bölmede şu seçeneklerden birini seçin:

   * [**El ile ölçek**](#manual-scale): Kullanmak istediğiniz işlem birimlerinin sayısına göre ölçeklendirin.
   * [**Özel otomatik ölçek**](#custom-autoscale): Çeşitli ölçütlerden seçerek ve bu ölçütleri karşılamak için eşik koşullarını belirterek performans ölçümlerini temel alın.

   ![İstediğiniz ölçekleme türünü seçin](./media/ise-manage-integration-service-environment/select-scale-out-options.png)

<a name="manual-scale"></a>

### <a name="manual-scale"></a>Manuel ölçek

1. **Ek kapasite**için El **Ile ölçeklendirmeyi**seçtikten sonra, kullanmak istediğiniz ölçeklendirme birimlerinin sayısını seçin.

   ![İstediğiniz ölçekleme türünü seçin](./media/ise-manage-integration-service-environment/select-manual-scale-out-units.png)

1. İşiniz bittiğinde **Kaydet**'i seçin.

<a name="custom-autoscale"></a>

### <a name="custom-autoscale"></a>Özel otomatik ölçeklendirme

1. **Otomatik Ölçek ayarı adı**için Özel otomatik **ölçeklendirmeyi**seçtikten sonra, ayarınız için bir ad sağlayın ve isteğe bağlı olarak, ayarın ait olduğu Azure kaynak grubunu seçin.

   ![Otomatik ölçek lendirme ayarı için ad sağlayın ve kaynak grubunu seçin](./media/ise-manage-integration-service-environment/select-custom-autoscale.png)

1. **Varsayılan** koşul için, bir metrik veya ölçek belirli bir **örnek sayısına** **dayalı Ölçek** seçin.

   * Örnek tabanlı seçerseniz, 0'dan 10'a kadar bir değer olan işlem birimlerinin numarasını girin.

   * Metrik tabanlı seçerseniz, aşağıdaki adımları izleyin:

     1. **Kurallar** bölümünde kural **ekle'yi**seçin.

     1. Ölçek **kuralı** bölmesinde, kural tetiklendiğinde ölçütlerinizi ve eyleminizi ayarlayın.

     1. **Örneğin sınırları**için, şu değerleri belirtin:

        * **Minimum**: Kullanılacak minimum işlem birimi sayısı
        * **Maksimum**: Kullanılacak maksimum işlem birimi sayısı
        * **Varsayılan**: Kaynak ölçümlerini okurken herhangi bir sorun meydana gelirse ve geçerli kapasite varsayılan kapasitenin altındaysa, otomatik ölçeklendirme varsayılan işlem birimi sayısına ölçeklenir. Ancak, geçerli kapasite varsayılan kapasiteyi aşarsa, otomatik ölçeklendirme ölçeklenmez.

1. Başka bir koşul eklemek için **ölçek koşulekle'yi**seçin.

1. Otomatik ölçek ayarlarınızı bitirdiğinizde, değişikliklerinizi kaydedin.

<a name="restart-ISE"></a>

## <a name="restart-ise"></a>İmKB'yi Yeniden Başlat

DNS sunucu veya DNS sunucu ayarlarınızı değiştirirseniz, İmKB'nin bu değişiklikleri alabilmeleri için İmKB'nizi yeniden başlatmanız gerekir. Premium SKU ISE'nin yeniden başlatılması, artıklık ve geri dönüşüm sırasında birer birer yeniden başlayan bileşenler nedeniyle kesintiye neden olmaz. Ancak, bir Geliştirici SKU ISE, fazlalık olmadığı için kapalı kalma süresiyle karşılar. Daha fazla bilgi için Ise [SUS'lara](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#ise-level)bakın.

1. Azure [portalında](https://portal.azure.com)ISE'nize gidin.

1. İmKB menüsünde **Genel Bakış'ı**seçin. Genel Bakış araç çubuğunda, **Yeniden Başlat.**

   ![Tümleştirme hizmet ortamını yeniden başlatın](./media/connect-virtual-network-vnet-isolated-environment/restart-integration-service-environment.png)

<a name="delete-ise"></a>

## <a name="delete-ise"></a>ISE'yi silin

Artık ihtiyacınız olmayan bir İmKB'yi veya Bir EŞey içeren bir Azure kaynak grubunu silmeden önce, bu kaynakları içeren Azure kaynak grubunda veya Azure sanal ağınızda bu öğelersilinmesini engelleyebildiği için hiçbir ilkeniz veya kilitleriniz olup olmadığını denetleyin.

İmKB'nizi sildikten sonra, Azure sanal ağınızı veya alt ağlarınızı silmeyi denemeden önce 9 saat kadar beklemeniz gerekebilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Entegrasyon hizmeti ortamlarına kaynak ekleme](../logic-apps/add-artifacts-integration-service-environment-ise.md)