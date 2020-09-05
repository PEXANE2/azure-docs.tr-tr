---
title: Azure Lighthouse nedir?
description: Azure Athouse, hizmet sağlayıcılarının daha yüksek otomasyon ve verimlilik özelliklerine sahip müşterileri için yönetilen hizmetler sunmasına olanak tanır.
ms.date: 08/19/2020
ms.topic: overview
ms.openlocfilehash: 22bec7ec8944a11ce0cfdf51776f1f193a1aedaa
ms.sourcegitcommit: de2750163a601aae0c28506ba32be067e0068c0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2020
ms.locfileid: "89488822"
---
# <a name="what-is-azure-lighthouse"></a>Azure Lighthouse nedir?

Azure Lighthouse, kiracılar arası ve çok kiracılı yönetime olanak tanır. Bu sayede kaynaklar ve kiracılar daha yüksek otomasyon, ölçeklenebilirlik ve gelişmiş idare elde edilir. Azure açık, hizmet sağlayıcıları, Azure platformunda yerleşik kapsamlı ve güçlü yönetim araçlarını kullanarak yönetilen Hizmetleri sunabilir. Bu teklif, birden fazla kiracıda kaynakları yöneten kurumsal BT kuruluşları için de avantaj sağlayabilir.

![Azure açık kullanım için genel bakış Diyagramı](media/azure-lighthouse-overview.jpg)

## <a name="benefits"></a>Yararları

Azure Kathouse, yönetilen Hizmetleri öngörülebilir ve verimli bir şekilde oluşturup sunmanıza yardımcı olur. Avantajlara şunlar dahildir:

- **Ölçekteki yönetim**: müşteri kaynaklarını yönetmeye yönelik müşteri katılımı ve yaşam döngüsü işlemleri daha kolay ve ölçeklenebilir. Mevcut API 'Ler, yönetim araçları ve iş akışları, bulundukları bölgeler ne olursa olsun, temsilcili kaynaklarla kullanılabilir.
- **Müşteriler Için daha fazla görünürlük ve duyarlık**: MÜŞTERILER, IP 'niz korunurken, erişimi tamamen kaldırma özelliği de dahil olmak üzere, eylemleriniz üzerinde daha fazla görünürlük ve yönetim için temsilci seçtikleri kapsam üzerinde tam denetim sahibi olacaktır.
- **Kapsamlı ve Birleşik platform araçları**: alet oluşturma DENEYIMIMIZ, EA, CSP ve kullandıkça öde gibi birden fazla lisanslama modeli de dahil olmak üzere temel hizmet sağlayıcısı senaryolarına yöneliktir. Yeni yetenekler, mevcut araçlar ve API 'Ler, lisanslama modelleri ve [bulut çözümü sağlayıcısı programı (CSP)](/partner-center/csp-overview)gibi iş ortağı programlarıyla çalışır. Azure Althouse, mevcut iş akışlarınızla ve uygulamalarınızla tümleştirilebilir ve [iş ortağı kimliğinizi bağlayarak](./how-to/partner-earned-credit.md)müşteri görevlendirmelerinizi izleyebilir ve iş ortağının kazanıldığı krediyi elde edebilirsiniz.

Azure kaynaklarını yönetmek için Azure açık Thouse kullanımıyla ilişkili ek maliyet yoktur. Tüm Azure müşterileri veya iş ortakları, Azure Use 'ı kullanabilir.

## <a name="capabilities"></a>Özellikler

Azure ışıklı kullanımı, katılım ve yönetimin kolaylaştırılmasına yardımcı olmak için birden çok yol içerir:

- **Azure Temsilcili kaynak yönetimi**: içerik ve denetim düzlemleri arasında geçiş yapmak zorunda kalmadan müşterilerinizin Azure kaynaklarını kendi kiracınızın içinden güvenli bir şekilde yönetin. Abonelikler ve kaynak grupları, yönetim kiracısındaki belirtilen kullanıcılar ve roller için, gerektiğinde erişimi kaldırabilme olanağı temsil edebilir. Daha fazla bilgi için bkz. [Azure temsilcisi kaynak yönetimi](concepts/azure-delegated-resource-management.md).
- **Yeni Azure Portal deneyimler**: Azure Portal içindeki [ **müşteriler** sayfasında](./how-to/view-manage-customers.md) , platformlar arası bilgileri görüntüleyin. Karşılık gelen bir [ **hizmet** sağlayıcı sayfası](how-to/view-manage-service-providers.md) , müşterilerin hizmet sağlayıcı erişimini görüntülemesine ve yönetmesine olanak tanır.
- **Azure Resource Manager şablonlar**: şablonlarımız, çapraz kiracı yönetim görevleri gerçekleştirmenize ve Temsilcili müşteri kaynakları eklemenize yardımcı olur. Daha fazla bilgi için bkz. [örnek](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates) depomız ve [bir müşteriyi Azure ışıklı kullanmaya ekleme](how-to/onboard-customer.md).
- **Azure Market 'Te yönetilen hizmet teklifleri**: hizmetlerinizi müşterilere özel veya genel tekliflerle sunun ve Azure 'un açık bir şekilde eklendi otomatik olarak kullanmasını sağlayabilirsiniz. Daha fazla bilgi için bkz. [Azure Marketi 'Nde yönetilen hizmet teklifleri](concepts/managed-services-offers.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure tarafından atanan temsilcinin kaynak yönetimi](concepts/azure-delegated-resource-management.md) hakkında bilgi edinin.
- [Çapraz kiracı yönetim deneyimlerini](concepts/cross-tenant-management-experience.md)gezin.
- Bkz. [Azure açık thouse 'ı bir kuruluş içinde kullanma](concepts/enterprise.md).
