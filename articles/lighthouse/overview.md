---
title: Azure Lighthouse nedir?
description: Azure Athouse, hizmet sağlayıcılarının daha yüksek otomasyon ve verimlilik özelliklerine sahip müşterileri için yönetilen hizmetler sunmasına olanak tanır.
author: JnHs
ms.author: jenhayes
ms.date: 08/22/2019
ms.topic: overview
ms.service: lighthouse
manager: carmonm
ms.openlocfilehash: 05fa16504e25a6bf0f6aa1c0a348284abba6e1ed
ms.sourcegitcommit: dcf3e03ef228fcbdaf0c83ae1ec2ba996a4b1892
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/23/2019
ms.locfileid: "70011901"
---
# <a name="what-is-azure-lighthouse"></a>Azure Lighthouse nedir?

Azure Athouse hizmeti sağlayıcıları, daha yüksek otomasyon, ölçek ve gelişmiş idare özelliklerine sahip tüm müşterilerine Azure 'u görüntülemek ve yönetmek için tek bir denetim düzlemi sunmaktadır. Azure açık, hizmet sağlayıcıları, Azure platformunda yerleşik kapsamlı ve güçlü yönetim araçlarını kullanarak yönetilen Hizmetleri sunabilir. Bu teklif, birden fazla kiracıda kaynakları yöneten kurumsal BT kuruluşları için de avantaj sağlayabilir.

![Azure açık kullanım için genel bakış Diyagramı](media/azure-lighthouse-overview.jpg)

## <a name="benefits"></a>Avantajlar

Azure Kathouse, müşterileriniz için yönetilen Hizmetleri öngörülebilir ve verimli bir şekilde oluşturup sunmanıza yardımcı olur. Avantajları şunlardır:

- **Ölçekte yönetim**: Müşteri kaynaklarını yönetmeye yönelik müşteri katılımı ve yaşam döngüsü işlemleri daha kolay ve ölçeklenebilir.
- **Müşteriler Için daha fazla görünürlük ve duyarlık**: Kaynakları yönettiğiniz müşteriler, eylemleriniz üzerinde daha fazla görünürlük ve yönetim için temsilci oldukları kapsam üzerinde, IP 'niz korunurken tam denetim sahibi olacaktır.
- **Kapsamlı ve Birleşik platform araçları**: Araç deneyimimiz, EA, CSP ve kullandıkça öde gibi birden fazla lisanslama modeli de dahil olmak üzere temel hizmet sağlayıcısı senaryolarına yöneliktir. Yeni yetenekler, mevcut araçlar ve API 'Ler, lisanslama modelleri ve [bulut çözümü sağlayıcısı programı (CSP)](https://docs.microsoft.com/partner-center/csp-overview)gibi iş ortağı programlarıyla çalışır. Seçtiğiniz Azure hafif kullanım seçenekleri mevcut iş akışlarınızla ve uygulamalarınızla tümleştirilebilir ve [iş ortağı kimliğinizi bağlayarak](https://docs.microsoft.com/azure/billing/billing-partner-admin-link-started)müşteri görevlendirmelerinizi izleyebilirsiniz.

Müşterilerinizin Azure kaynaklarını yönetmek için Azure açık Thouse kullanımıyla ilişkili ek maliyet yoktur.

## <a name="capabilities"></a>Özellikler

Azure ışıklı kullanımı, müşteri katılımı ve yönetimini kolaylaştırmaya yardımcı olmak için birden çok yol içerir:

- **Azure Temsilcili kaynak yönetimi**: İçerik ve denetim düzlemleri arasında geçiş yapmak zorunda kalmadan müşterilerinizin Azure kaynaklarını kendi kiracınızın içinden güvenli bir şekilde yönetin. Daha fazla bilgi için bkz. [Azure temsilcisi kaynak yönetimi](./concepts/azure-delegated-resource-management.md).
- **Yeni Azure Portal deneyimler**: [Azure Portal](https://portal.azure.com)çapraz kiracı bilgilerini, yeni **My Customers** sayfasında görüntüleyin. Karşılık gelen bir **hizmet** sağlayıcı dikey penceresi, müşterilerinizin hizmet sağlayıcı erişimini görüntülemesine ve yönetmesine olanak tanır. Daha fazla bilgi için bkz. [müşterileri görüntüleme ve yönetme](./how-to/view-manage-customers.md) ve [hizmet sağlayıcılarını görüntüleme ve yönetme](./how-to/view-manage-service-providers.md).
- **Azure Resource Manager şablonları**: Azure tarafından atanan kaynak yönetimine yönelik ekleme müşterileri dahil olmak üzere yönetim görevlerini daha kolay bir şekilde gerçekleştirin. Daha fazla bilgi için bkz. [örnek](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates) depomız ve [bir müşteriyi Azure tarafından atanan kaynak yönetimine ekleme](how-to/onboard-customer.md).
- **Azure Market 'Te yönetilen hizmet teklifleri**: Hizmetlerinizi özel veya genel tekliflerle müşterilere sunun ve Azure Resource Manager şablonlarını kullanarak ekleme için alternatif olarak, Azure tarafından atanan kaynak yönetimine otomatik olarak eklendi edin. Daha fazla bilgi için bkz. [Azure Marketi 'Nde yönetilen hizmetler teklifleri](./concepts/managed-services-offers.md).
- **Azure yönetilen uygulamalar**: Müşterilerinizin kendi aboneliklerinde dağıtımı ve kullanması kolay olan uygulamaları paketleyin ve sunun. Uygulama, kiracınızdan erişebileceğiniz bir kaynak grubuna dağıtılır ve bu, hizmeti genel Azure Athouse deneyiminin bir parçası olarak yönetmenize olanak tanır. Daha fazla bilgi için bkz. [Azure yönetilen uygulamalara genel bakış](https://docs.microsoft.com/azure/managed-applications/overview).

> [!NOTE]
> Yukarıda açıklanan yetenekler Şu anda genel bulutlarda kullanılabilir. Bireysel hizmetlerin bölgesel kullanılabilirliği için bkz. [bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure tarafından atanan temsilcinin kaynak yönetimi](concepts/azure-delegated-resource-management.md) hakkında bilgi edinin.
- [Çapraz kiracı yönetim deneyimleri](concepts/cross-tenant-management-experience.md)hakkında bilgi edinin.
