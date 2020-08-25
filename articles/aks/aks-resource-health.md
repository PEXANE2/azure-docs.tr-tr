---
title: AKS kümenizi etkileyen Kaynak Durumu olaylarını kontrol edin (Önizleme)
description: Azure Kaynak Durumu kullanarak AKS kümenizin sistem durumunu kontrol edin.
services: container-service
author: yunjchoi
ms.topic: troubleshooting
ms.date: 08/18/2020
ms.author: yunjchoi
ms.openlocfilehash: d14ed7e714eac175dc7a17bbccd2150f9bb01ef9
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2020
ms.locfileid: "88752741"
---
# <a name="check-for-resource-health-events-impacting-your-aks-cluster-preview"></a>AKS kümenizi etkileyen Kaynak Durumu olaylarını kontrol edin (Önizleme)


AKS üzerinde kapsayıcı iş yüklerinizi çalıştırırken, iş yüklerinizin kullanılabilirliği üzerindeki etkiyi en aza indirmek için ortaya çıkan sorunları giderebileceğinizden ve giderebileceğinizden emin olmak istersiniz. [Azure Kaynak durumu](https://docs.microsoft.com/azure/service-health/resource-health-overview) , aks kümenizin kullanılamaz olmasına neden olabilecek çeşitli sistem durumu olaylarına ilişkin görünürlük sağlar.

[!INCLUDE [preview features callout](./includes/preview/preview-callout.md)]

## <a name="open-resource-health"></a>Kaynak Durumu açın

### <a name="to-access-resource-health-for-your-aks-cluster"></a>AKS kümeniz için Kaynak Durumu erişmek için:

- [Azure Portal](https://portal.azure.com)aks kümenize gidin.
- Sol gezinti bölmesinde **kaynak durumu** seçin.

### <a name="to-access-resource-health-for-all-clusters-on-your-subscription"></a>Aboneliğinizdeki tüm kümeler için Kaynak Durumu erişmek için:

- [Azure Portal](https://portal.azure.com) **hizmet durumunu** arayın ve bu sayfaya gidin.
- Sol gezinti bölmesinde **kaynak durumu** ' nu seçin.
- Aboneliğinizi seçin ve kaynak türünü Azure Kubernetes hizmeti (AKS) olarak ayarlayın.

![kaynak-sistem durumu denetimi](./media/aks-resource-health/resource-health-check.png)

## <a name="check-the-health-status"></a>Sistem durumunu denetleme

Azure Kaynak Durumu, Azure kaynaklarınızı etkileyen hizmet sorunlarını tanılamanıza ve destek almanıza yardımcı olur. Kaynaklarınızın geçerli ve geçmiş durumu hakkında raporlar Kaynak Durumu ve sorunun Kullanıcı tarafından başlatılan bir eylem veya platform olayından kaynaklanıp kaynaklanmadığını belirlemenize yardımcı olur.

Kaynak Durumu, kümenin sistem durumunu tespit etmek üzere yönetilen kümeniz için sinyaller alır. AKS kümenizin sistem durumu ve her bir sistem durumu sinyali için gereken raporlar eylemleri incelenir. Bu sinyaller, otomatik çözüm sorunları, planlı güncelleştirmeler, planlanmamış sistem olayları ve Kullanıcı tarafından başlatılan eylemlerden kaynaklanan kullanım dışı durumlardır. Bu sinyaller Azure Kaynak Durumu sistem durumu kullanılarak sınıflandırılır: *kullanılabilir*, *kullanılamaz*, *bilinmiyor*ve *düşürülmüş*.

- **Kullanılabilir**: kümenizin sistem durumunu etkileyen bilinen bir sorun yoksa kaynak durumu kümenizi *kullanılabilir*olarak raporlar.

- **Kullanılamaz**: kümenizin sistem durumunu etkileyen bir platform veya platform dışı olay olduğunda, kümenizi *kullanılamıyor*olarak raporlar kaynak durumu.

- **Bilinmiyor**: kümenizin sistem durumu ölçümlerine geçici bir bağlantı kaybı olduğunda, kümenizi *bilinmiyor*olarak raporlar kaynak durumu.

- **Düşürülmüş**: eyleminizi gerektiren bir sistem durumu sorunu olduğunda, kümenizi *düşürülmüş*olarak rapor kaynak durumu.

Her sistem durumunun gösterdiği diğer ayrıntılar için [kaynak durumu genel bakış](https://docs.microsoft.com/azure/service-health/resource-health-overview#health-status)' ı ziyaret edin.

### <a name="view-historical-data"></a>Geçmiş verileri görüntüleme

Geçmiş Kaynak Durumu bilgisinin son 30 gün sonra **durum geçmişi** bölümüne de görüntüleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Aks tanılamayı](https://docs.microsoft.com/azure/aks/concepts-diagnostics)kullanarak küme sorunlarını gidermek için kümenizdeki denetimleri çalıştırın.
