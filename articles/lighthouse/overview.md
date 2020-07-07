---
title: Azure Lighthouse nedir?
description: Azure Athouse, hizmet sağlayıcılarının daha yüksek otomasyon ve verimlilik özelliklerine sahip müşterileri için yönetilen hizmetler sunmasına olanak tanır.
ms.date: 05/28/2020
ms.topic: overview
ms.openlocfilehash: d4eea306341c690d5dea040d5ae3b20ef8d83084
ms.sourcegitcommit: dee7b84104741ddf74b660c3c0a291adf11ed349
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85919109"
---
# <a name="what-is-azure-lighthouse"></a>Azure Lighthouse nedir?

Azure Athouse hizmeti sağlayıcıları, daha yüksek otomasyon, ölçek ve gelişmiş idare özelliklerine sahip tüm müşterilerine Azure 'u görüntülemek ve yönetmek için tek bir denetim düzlemi sunmaktadır. Azure açık, hizmet sağlayıcıları, Azure platformunda yerleşik kapsamlı ve güçlü yönetim araçlarını kullanarak yönetilen Hizmetleri sunabilir. Bu teklif, birden fazla kiracıda kaynakları yöneten kurumsal BT kuruluşları için de avantaj sağlayabilir.

![Azure açık kullanım için genel bakış Diyagramı](media/azure-lighthouse-overview.jpg)

## <a name="benefits"></a>Yararları

Azure Kathouse, müşterileriniz için yönetilen Hizmetleri öngörülebilir ve verimli bir şekilde oluşturup sunmanıza yardımcı olur. Avantajlara şunlar dahildir:

- **Ölçekteki yönetim**: müşteri kaynaklarını yönetmeye yönelik müşteri katılımı ve yaşam döngüsü işlemleri daha kolay ve ölçeklenebilir. Mevcut API 'Ler, yönetim araçları ve iş akışları, bulunan bölgeler ne olursa olsun, temsilcili müşteri kaynakları ile kullanılabilir.
- **Müşteriler Için daha fazla görünürlük ve duyarlık**: MÜŞTERILER, IP 'niz korunurken, erişimi tamamen kaldırma özelliği de dahil olmak üzere, eylemleriniz üzerinde daha fazla görünürlük ve yönetim için temsilci seçtikleri kapsam üzerinde tam denetim sahibi olacaktır.
- **Kapsamlı ve Birleşik platform araçları**: alet oluşturma DENEYIMIMIZ, EA, CSP ve kullandıkça öde gibi birden fazla lisanslama modeli de dahil olmak üzere temel hizmet sağlayıcısı senaryolarına yöneliktir. Yeni yetenekler, mevcut araçlar ve API 'Ler, lisanslama modelleri ve [bulut çözümü sağlayıcısı programı (CSP)](/partner-center/csp-overview)gibi iş ortağı programlarıyla çalışır. Azure ışıklı kullanımı, mevcut iş akışlarınızla ve uygulamalarınızla tümleştirilebilir ve [iş ortağı kimliğinizi bağlayarak](../billing/billing-partner-admin-link-started.md)müşteri görevlendirmelerinizi takip edebilirsiniz.

Müşterilerinizin Azure kaynaklarını yönetmek için Azure açık Thouse kullanımıyla ilişkili ek maliyet yoktur. Tüm Azure müşterileri veya iş ortakları, Azure Use 'ı kullanabilir.

## <a name="capabilities"></a>Özellikler

Azure ışıklı kullanımı, müşteri katılımı ve yönetimini kolaylaştırmaya yardımcı olmak için birden çok yol içerir:

- **Azure Temsilcili kaynak yönetimi**: içerik ve denetim düzlemleri arasında geçiş yapmak zorunda kalmadan müşterilerinizin Azure kaynaklarını kendi kiracınızın içinden güvenli bir şekilde yönetin. Abonelikler ve kaynak grupları, yönetim kiracısındaki belirtilen kullanıcılar ve roller için, gerektiğinde erişimi kaldırabilme olanağı temsil edebilir. Daha fazla bilgi için bkz. [Azure temsilcisi kaynak yönetimi](concepts/azure-delegated-resource-management.md).
- **Yeni Azure Portal deneyimler**: [Azure Portal](https://portal.azure.com)içindeki yeni **müşterilerimiz** sayfasında çapraz kiracı bilgilerini görüntüleyin. Karşılık gelen bir **hizmet** sağlayıcı dikey penceresi, müşterilerinizin hizmet sağlayıcı erişimini görüntülemesine ve yönetmesine olanak tanır. Daha fazla bilgi için bkz. [müşterileri görüntüleme ve yönetme](./how-to/view-manage-customers.md) ve [hizmet sağlayıcılarını görüntüleme ve yönetme](how-to/view-manage-service-providers.md).
- **Azure Resource Manager şablonlar**: Azure tarafından atanan kaynak yönetimine yönelik ekleme müşterileri dahil olmak üzere yönetim görevlerini daha kolay bir şekilde gerçekleştirin. Daha fazla bilgi için bkz. [örnek](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates) depomız ve [bir müşteriyi Azure tarafından atanan kaynak yönetimine ekleme](how-to/onboard-customer.md).
- **Azure Market 'Te yönetilen hizmet teklifleri**: özel veya genel tekliflerdeki hizmetlerinizi müşterilere sunun ve Azure Resource Manager şablonlarını kullanarak hazırlama için alternatif olarak Azure tarafından atanan kaynak yönetimine otomatik olarak eklendi. Daha fazla bilgi için bkz. [Azure Marketi 'Nde yönetilen hizmet teklifleri](concepts/managed-services-offers.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure tarafından atanan temsilcinin kaynak yönetimi](concepts/azure-delegated-resource-management.md) hakkında bilgi edinin.
- [Çapraz kiracı yönetim deneyimleri](concepts/cross-tenant-management-experience.md)hakkında bilgi edinin.
