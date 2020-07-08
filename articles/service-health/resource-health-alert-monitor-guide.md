---
title: Azure portalını kullanarak Kaynak Durumu Uyarıları oluşturma
description: Azure kaynaklarınızın kullanılamadığı durumlarda size bildirimde bulunan Azure portal kullanarak uyarı oluşturun.
ms.topic: conceptual
ms.date: 6/23/2020
ms.openlocfilehash: 2ba7597283ed254b8c03e8ea58c3c7b1257250ee
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85218077"
---
# <a name="configure-resource-health-alerts-using-azure-portal"></a>Azure portalı kullanarak kaynak sistem durumu uyarılarını yapılandırma

Bu makalede, Azure portal kullanarak kaynak sistem durumu bildirimleri için etkinlik günlüğü uyarılarını ayarlama konusu gösterilmektedir.

Azure Kaynak Durumu, Azure kaynaklarınızın geçerli ve geçmiş sistem durumu hakkında bilgi sahibi olmanızı önler. Azure Kaynak Durumu uyarılar, bu kaynakların sistem durumunda bir değişiklik olduğunda sizi neredeyse gerçek zamanlı olarak uyarabilirler. Kaynak Durumu uyarılar oluşturma, kullanıcıların uyarıları toplu olarak oluşturmalarına ve özelleştirmesine izin verir.

> [!NOTE]
> Kaynak Durumu uyarılar Şu anda önizlemededir.

Kaynak sistem durumu bildirimleri [Azure etkinlik günlüğünde](https://docs.microsoft.com/azure/azure-monitor/platform/platform-logs-overview) , etkinlik günlüğünde depolanan büyük miktarda bilgi hacmi verildiğinde, kaynak sistem durumu bildirimlerinde uyarıları görüntülemeyi ve ayarlamayı kolaylaştıran ayrı bir kullanıcı arabirimi vardır.
Azure kaynağı, Azure aboneliğinize kaynak sistem durumu bildirimleri gönderdiğinde bir uyarı alabilirsiniz. Uyarıyı temel alarak yapılandırabilirsiniz:

* Abonelik etkilendi.
* Kaynak (ler) tür etkilendi.
* Kaynak grupları etkilendi.
* Kaynak (ler) i etkilendi.
* Etkilenen kaynakların olay durumu (ler) i.
* Kaynak (ler) i etkilenen durumlar.
* Kaynak (ler) in etkilenen nedenleri.

Ayrıca, uyarının kime gönderileceğini de yapılandırabilirsiniz:

* Mevcut bir eylem grubu seçin.
* Yeni bir eylem grubu oluşturun (gelecekteki uyarılar için kullanılabilir).

Eylem grupları hakkında daha fazla bilgi edinmek için bkz. [Eylem grupları oluşturma ve yönetme](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups).

Azure Resource Manager şablonları kullanarak kaynak durumu bildirim uyarılarını yapılandırma hakkında daha fazla bilgi için, bkz. [Kaynak Yöneticisi Templates](https://docs.microsoft.com/azure/service-health/resource-health-alert-arm-template-guide).
Azure portal kullanarak uyarı Kaynak Durumu

## <a name="resource-health-alert-using-azure-portal"></a>Azure portalını kullanarak uyarı Kaynak Durumu

1. Azure [portalında](https://portal.azure.com/) **hizmet durumu**' nu seçin.

    ![Hizmet durumu seçimi](./media/resource-health-alert-monitor-guide/service-health-selection.png)
2. **Kaynak durumu** bölümünde **kaynak durumu**' ı seçin.
3. **Kaynak sistem durumu uyarısı Ekle** ' yi seçin ve alanları girin.
4. Uyarı hedefi altında, uyarılmak istediğiniz **aboneliği**, **kaynak türlerini**, **kaynak gruplarını** ve **kaynağı** seçin.

    ![Hedef seçim seçimi](./media/resource-health-alert-monitor-guide/alert-target.png)

5. Uyarı koşulu altında şunları seçin:
    1. Uyarı almak istediğiniz **olay durumu** . Etkinliğin önem derecesi: etkin, çözümlenmiş, devam ediyor, güncelleştirildi
    2. Uyarı almak istediğiniz **kaynak durumu** . Olayın kaynak durumu: kullanılabilir, kullanılamaz, bilinmiyor, düşürülmüş
    3. Uyarı almak istediğiniz **neden türü** . Olayın nedeni: platform başlatıldı, Kullanıcı tarafından başlatılan ![ uyarı koşulu seçimi sistem durumu seçimi](./media/resource-health-alert-monitor-guide/alert-condition.png)
6. Uyarı ayrıntılarını tanımla altında aşağıdaki ayrıntıları sağlayın:
    1. **Uyarı kuralı adı**: yeni uyarı kuralının adı.
    2. **Açıklama**: yeni uyarı kuralının açıklaması.
    3. **Uyarıyı kaynak grubuna kaydet**: Bu yeni kuralı kaydetmek istediğiniz kaynak grubunu seçin.
7. **Eylem grubu**altında, açılan menüden, bu yeni uyarı kuralına atamak istediğiniz eylem grubunu belirtin. Ya da [Yeni bir eylem grubu oluşturun](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups) ve yeni kurala atayın. Yeni bir grup oluşturmak için + **Yeni Grup**' u seçin.
8. Kuralları oluşturduktan sonra etkinleştirmek için, **oluşturma sırasında kuralı etkinleştir** **seçeneğini belirleyin.**
9. **Uyarı kuralı oluştur**’u seçin.

Etkinlik günlüğü için yeni uyarı kuralı oluşturulur ve pencerenin sağ üst köşesinde bir onay iletisi görüntülenir.
Bir kuralı etkinleştirebilir, devre dışı bırakabilir, düzenleyebilir veya silebilirsiniz. [Etkinlik günlüğü kurallarını yönetme](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log#view-and-manage-in-the-azure-portal)hakkında daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

Kaynak Durumu hakkında daha fazla bilgi edinin:

* [Azure Kaynak Durumu genel bakış](Resource-health-overview.md)
* [Azure Kaynak Durumu aracılığıyla kullanılabilen kaynak türleri ve durum denetimleri](resource-health-checks-resource-types.md)

Hizmet durumu uyarıları oluşturma:

* [Hizmet durumu için uyarıları yapılandırma](../azure-monitor/platform/alerts-activity-log-service-notifications.md) 
* [Azure etkinlik günlüğü olay şeması](../azure-monitor/platform/activity-log-schema.md)
* [Resource Manager şablonlarını kullanarak kaynak sistem durumu uyarılarını yapılandırma](https://docs.microsoft.com/azure/service-health/resource-health-alert-arm-template-guide)