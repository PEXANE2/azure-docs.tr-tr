---
title: Azure eşleme hizmeti bağlantısı
description: Microsoft Azure eşleme hizmeti bağlantısı hakkında bilgi edinin
services: peering-service
author: derekolo
ms.service: peering-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Infrastructure-services
ms.date: 05/18/2020
ms.author: derekol
ms.openlocfilehash: df337736a676c40f1345310afb9a2e685ddc1d5c
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "84872025"
---
# <a name="peering-service-connection"></a>Eşleme hizmeti bağlantısı

Bir bağlantı, genellikle bir eşleme hizmetini tanımlayan bir mantıksal bilgi kümesine başvurur. Aşağıdaki öznitelikler belirtilerek tanımlanmıştır:

- Mantıksal ad
- Bağlantı iş ortağı
- Müşterinin fiziksel konumu
- IP ön ekleri

Müşteri, gereksinime göre tek bir bağlantı veya birden çok bağlantı kurabilir. Bir bağlantı, telemetri koleksiyonu birimi olarak da kullanılır. Örneğin, telemetri uyarılarını kabul etmek için müşterinin izlenecek bağlantıyı tanımlanması gerekir.

> [!Note]
> Eşleme hizmetine kaydolduğunuzda, seçtiğiniz önekleriniz için gecikme süreleri sağlamak üzere Windows ve Office 365 telemetrinizi çözümliyoruz.
>Bağlantı telemetrisi hakkında daha fazla bilgi için bkz. [eşleme hizmeti bağlantı telemetrisine](connection-telemetry.md)bakın.
>

>## <a name="how-to-register-a-connection"></a>Bir bağlantı nasıl kaydettirilsin?

**Senaryo** -bir şube ofisinin aşağıdaki şekilde gösterildiği gibi farklı coğrafi konumlara yayıldığını varsayalım. Burada, müşterinin tek bir bağlantıyla ilişkili bir mantıksal ad, hizmet sağlayıcısı (SP) adı, müşterinin fiziksel konumu ve (müşteriye ait veya hizmet sağlayıcısı tarafından ayrılan) IP öneklerini sağlaması gerekir. Ayrı coğrafi olarak yedekli bağlantılar için eşleme hizmetini kaydettirmek üzere bu işlemin tekrarlanması gerekir.

![Coğrafi olarak yedekli bağlantılar](./media/peering-service-connection/peering-service-connections.png)

> [!Note]
> Durum düzeyi-bağlantı Birleşik Devletler coğrafi olarak bulunduğunda müşterinin fiziksel konumu olarak değerlendirilir.
>

## <a name="next-steps"></a>Sonraki adımlar

Eşleme hizmeti bağlantısının nasıl kaydedileceği hakkında adım adım işlem hakkında bilgi edinmek için, bkz. [Azure Portal kullanarak eşleme hizmetini kaydetme](azure-portal.md).

Eşleme hizmeti bağlantı telemetrisi hakkında bilgi edinmek için bkz. [eşleme hizmeti bağlantı telemetrisi](connection-telemetry.md).

Telemetriyi ölçmek için bkz. [ölçüm bağlantı telemetrisi](measure-connection-telemetry.md).
