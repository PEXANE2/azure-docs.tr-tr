---
title: Azure Lighthouse nedir?
description: Azure Deniz Feneri, hizmet sağlayıcıların müşterilerine daha yüksek otomasyon ve verimlilikle yönetilen hizmetler sunmalarına olanak tanır.
ms.date: 11/11/2019
ms.topic: overview
ms.openlocfilehash: 1d4d33238f30c2a579c6a0da6a39238207db4f4a
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80982795"
---
# <a name="what-is-azure-lighthouse"></a>Azure Lighthouse nedir?

Azure Deniz Feneri, hizmet sağlayıcılarına azure'u daha yüksek otomasyon, ölçek ve gelişmiş yönetimle tüm müşterilerine görüntülemeleri ve yönetmeleri için tek bir denetim düzlemi sunar. Azure Deniz Feneri ile hizmet sağlayıcıları, Azure platformunda yerleşik kapsamlı ve sağlam yönetim aracıkullanarak yönetilen hizmetler sunabilir. Bu teklif, birden çok kiracı arasında kaynakları yöneten kurumsal BT kuruluşlarına da fayda sağlayabilir.

![Azure Deniz Feneri'ne genel bakış diyagramı](media/azure-lighthouse-overview.jpg)

## <a name="benefits"></a>Yararları

Azure Deniz Feneri, müşterileriniz için yönetilen hizmetler oluşturmanıza ve sunmanıza kârlı ve verimli bir şekilde yardımcı olur. Avantajlara şunlar dahildir:

- **Ölçekte yönetim**: Müşteri kaynaklarını yönetmek için müşteri katılımı ve yaşam döngüsü işlemleri daha kolay ve ölçeklenebilir.
- **Müşteriler için daha fazla görünürlük ve kesinlik**: Kaynaklarını yönettiğiniz müşteriler, IP'niz korunurken eylemlerinizde daha fazla görünürlüğe ve yönetim için temsilcilik yaptıkları kapsam üzerinde hassas denetime sahip olacaktır.
- **Kapsamlı ve birleşik platform araçlama**: Araçlama deneyimimiz, EA, CSP ve kullangibi öde gibi birden fazla lisans modeli de dahil olmak üzere önemli hizmet sağlayıcı senaryolarını ele alarmaktadır. Yeni özellikler, varolan araçlar ve API'ler, lisanslama modelleri ve [Bulut Çözüm Sağlayıcısı programı (CSP)](https://docs.microsoft.com/partner-center/csp-overview)gibi iş ortağı programlarıyla çalışır. Seçtiğiniz Azure Deniz Feneri seçenekleri, mevcut iş akışlarınıza ve uygulamalarınıza entegre edilebilir ve [iş ortağı kimliğinizi bağlayarak](../billing/billing-partner-admin-link-started.md)müşteri etkileşimleri üzerindeki etkinizi izleyebilirsiniz.

Müşterilerinizin Azure kaynaklarını yönetmek için Azure Deniz Feneri'ni kullanmanın ek bir maliyeti yoktur.

## <a name="capabilities"></a>Özellikler

Azure Deniz Feneri, müşteri etkileşimini ve yönetimini kolaylaştırmaya yardımcı olmak için birden çok yol içerir:

- **Azure temsilci kaynak yönetimi**: Bağlam arasında geçiş yapmak ve düzlemleri denetlemek zorunda kalmadan müşterilerinizin Azure kaynaklarını kendi kiracınızın içinden güvenli bir şekilde yönetin. Daha fazla bilgi için Bkz. [Azure temsilcikaynak yönetimi.](concepts/azure-delegated-resource-management.md)
- **Yeni Azure portalı deneyimleri**: [Azure portalındaki](https://portal.azure.com)yeni **Müşterilerim** sayfasında kiracılar arası bilgileri görüntüleyin. İlgili **Servis sağlayıcıları,** müşterilerinizin servis sağlayıcı erişimini görüntülemesine ve yönetmesine olanak tanır. Daha fazla bilgi için, [müşterileri görüntüleyin ve yönetin](./how-to/view-manage-customers.md) ve [servis sağlayıcılarını görüntüleyin ve yönetin.](how-to/view-manage-service-providers.md)
- **Azure Kaynak Yöneticisi şablonları:** Azure temsilcikaynak yönetimi için biniş yapan müşteriler de dahil olmak üzere yönetim görevlerini daha kolay gerçekleştirin. Daha fazla bilgi [için, örneklerirepo](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates) ve [Onboard bir müşteri için Azure temsilci kaynak yönetimi](how-to/onboard-customer.md)bakın.
- **Azure Marketi'nde Yönetilen Hizmetler sunar**: Müşterilerinize özel veya genel teklifler aracılığıyla hizmet sunmak ve Azure Kaynak Yöneticisi şablonlarını kullanmaya alternatif olarak otomatik olarak Azure temsilcili kaynak yönetimine dahil edin. Daha fazla bilgi için Azure [Marketi'ndeki Yönetilen hizmetler tekliflerine](concepts/managed-services-offers.md)bakın.
- **Azure yönetilen uygulamalar**: Müşterilerinizin kendi aboneliklerinde dağıtması ve kullanması kolay paket uygulamaları. Uygulama, genel Azure Deniz Feneri deneyiminin bir parçası olarak hizmeti yönetmenize izin vererek kiracınızdan erişiğiniz olan bir kaynak grubuna dağıtılır. Daha fazla bilgi için Azure [yönetilen uygulamalara genel bakış](../azure-resource-manager/managed-applications/overview.md)bilgisine bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure tarafından atanan temsilcinin kaynak yönetimi](concepts/azure-delegated-resource-management.md) hakkında bilgi edinin.
- Kiracılar [arası yönetim deneyimleri](concepts/cross-tenant-management-experience.md)hakkında bilgi edinin.
