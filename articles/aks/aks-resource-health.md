---
title: AKS kümenizi etkileyen Kaynak Durumu olaylarını kontrol edin (Önizleme)
description: Azure Kaynak Durumu kullanarak AKS kümenizin sistem durumunu kontrol edin.
services: container-service
author: yunjchoi
ms.topic: troubleshooting
ms.date: 08/18/2020
ms.author: yunjchoi
ms.openlocfilehash: a409ac944c51d893fc344f82ae83d97559e055ef
ms.sourcegitcommit: a92fbc09b859941ed64128db6ff72b7a7bcec6ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2020
ms.locfileid: "92070665"
---
# <a name="check-for-resource-health-events-impacting-your-aks-cluster-preview"></a>AKS kümenizi etkileyen Kaynak Durumu olaylarını kontrol edin (Önizleme)


AKS üzerinde kapsayıcı iş yüklerinizi çalıştırırken, iş yüklerinizin kullanılabilirliği üzerindeki etkiyi en aza indirmek için ortaya çıkan sorunları giderebileceğinizden ve giderebileceğinizden emin olmak istersiniz. [Azure Kaynak durumu](../service-health/resource-health-overview.md) , aks kümenizin kullanılamaz olmasına neden olabilecek çeşitli sistem durumu olaylarına ilişkin görünürlük sağlar.

[!INCLUDE [preview features callout](./includes/preview/preview-callout.md)]

## <a name="open-resource-health"></a>Kaynak Durumu açın

### <a name="to-access-resource-health-for-your-aks-cluster"></a>AKS kümeniz için Kaynak Durumu erişmek için:

- [Azure Portal](https://portal.azure.com)aks kümenize gidin.
- Sol gezinti bölmesinde **kaynak durumu** seçin.

### <a name="to-access-resource-health-for-all-clusters-on-your-subscription"></a>Aboneliğinizdeki tüm kümeler için Kaynak Durumu erişmek için:

- [Azure Portal](https://portal.azure.com) **hizmet durumunu** arayın ve bu sayfaya gidin.
- Sol gezinti bölmesinde **kaynak durumu** ' nu seçin.
- Aboneliğinizi seçin ve kaynak türünü Azure Kubernetes hizmeti (AKS) olarak ayarlayın.

![Ekran görüntüsü, bir K S kümeniz için kaynak sistem durumunu gösterir.](./media/aks-resource-health/resource-health-check.png)

## <a name="check-the-health-status"></a>Sistem durumunu denetleme

Azure Kaynak Durumu, Azure kaynaklarınızı etkileyen hizmet sorunlarını tanılamanıza ve destek almanıza yardımcı olur. Kaynaklarınızın geçerli ve geçmiş durumu hakkında raporlar Kaynak Durumu ve sorunun Kullanıcı tarafından başlatılan bir eylem veya platform olayından kaynaklanıp kaynaklanmadığını belirlemenize yardımcı olur.

Kaynak Durumu, kümenin sistem durumunu tespit etmek üzere yönetilen kümeniz için sinyaller alır. AKS kümenizin sistem durumu ve her bir sistem durumu sinyali için gereken raporlar eylemleri incelenir. Bu sinyaller, otomatik çözüm sorunları, planlı güncelleştirmeler, planlanmamış sistem olayları ve Kullanıcı tarafından başlatılan eylemlerden kaynaklanan kullanım dışı durumlardır. Bu sinyaller Azure Kaynak Durumu sistem durumu kullanılarak sınıflandırılır: *kullanılabilir*, *kullanılamaz*, *bilinmiyor*ve *düşürülmüş*.

- **Kullanılabilir**: kümenizin sistem durumunu etkileyen bilinen bir sorun yoksa kaynak durumu kümenizi *kullanılabilir*olarak raporlar.

- **Kullanılamaz**: kümenizin sistem durumunu etkileyen bir platform veya platform dışı olay olduğunda, kümenizi *kullanılamıyor*olarak raporlar kaynak durumu.

- **Bilinmiyor**: kümenizin sistem durumu ölçümlerine geçici bir bağlantı kaybı olduğunda, kümenizi *bilinmiyor*olarak raporlar kaynak durumu.

- **Düşürülmüş**: eyleminizi gerektiren bir sistem durumu sorunu olduğunda, kümenizi *düşürülmüş*olarak rapor kaynak durumu.

Her sistem durumunun gösterdiği diğer ayrıntılar için [kaynak durumu genel bakış](../service-health/resource-health-overview.md#health-status)' ı ziyaret edin.

### <a name="view-historical-data"></a>Geçmiş verileri görüntüleme

Geçmiş Kaynak Durumu bilgisinin son 30 gün sonra **durum geçmişi** bölümüne de görüntüleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Aks tanılamayı](./concepts-diagnostics.md)kullanarak küme sorunlarını gidermek için kümenizdeki denetimleri çalıştırın.