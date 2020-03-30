---
title: Azure ve AppSource Market Teklifleri
description: Azure ve AppSource Pazar Yerleri'nin tekliflerini oluşturma ve yönetme
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 03/27/2019
ms.author: dsindona
ms.openlocfilehash: 7f6fd723355426a49cff032d51da0e09f13e295d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80278493"
---
# <a name="azure-and-appsource-marketplace-offers"></a>Azure ve AppSource Market Teklifleri

Bu bölümün bu ilk bölümü, Azure ve AppSource Pazar Yerleri için teklifler oluşturmak ve yönetmek için kullanılan genel işlemleri tanıtır.  Bu bölüm, belirli teklif türlerini yönetmek için anlamanız gereken arka planı ve tüm teklif türleri için ortak olan teknik bilgileri sağlar.  Bu bölümün çoğunluğu, belirli teklif türlerinin nasıl oluşturulacağına ve yönetileceklerine ilişkin ayrıntılı yönergeler içerir.  

Aşağıdaki videoda Azure Marketi veya AppSource'da bulunan çeşitli özellikler ve farklı teklif türleri tanıtıştır.  Ayrıca, bu pazarlarda bir uygulama veya hizmet yayınlamanın önemli teknik ve iş yönlerini de kapsar.

> [!VIDEO https://channel9.msdn.com/Events/Build/2018/BRK2513/player]

**Azure Marketi ve AppSource için Uygulama ve Hizmetler Oluşturma - Build 2018**

Bu pazar yerleri hakkında daha fazla bilgi için [Azure Marketi ve AppSource yayımlama kılavuzuna](../marketplace-publishers-guide.md)bakın.


## <a name="common-offer-operations"></a>Ortak teklif işlemleri

Yeni bir teklif oluşturma işlemi, örneğin [Azure uygulama teklifi](./azure-applications/cpp-azure-app-offer.md) ile danışmanlık hizmeti [teklifi](./consulting-services/cloud-partner-portal-consulting-services-publishing-offer.md)arasında, teklif türleri arasında büyük farklılıklar gösterir.  Buna karşılık, [Bulut İş Ortağı Portalı'nda](https://cloudpartner.azure.com) bir teklifte gerçekleştirdiğiniz diğer işlemlerin çoğu teklif türleri arasında oldukça standarthale getirilmektedir.  Yayımlama, görüntüleme durumu, güncelleştirme ve silme gibi bu yaygın [işlemler, Yönet'in sunduğu](./manage-offers/cpp-manage-offers.md) bölümde yer alır


## <a name="test-drive"></a>Test Sürüşü

*Test Drive,* müşterilere bu kadar etkin olan her teklif için "satın almadan önce deneyin" gösteri seçeneği sağlayan bir pazar özelliğidir.  Test Sürüşü özelliği teklif türlerinin aşağıdaki alt kümesiyle sınırlıdır: [Azure uygulamaları,](./azure-applications/cpp-azure-app-offer.md) [Dynamics 365 Business Central,](../cloud-partner-portal-orig/cpp-business-central-offer.md) [Dynamics 365 for Customer Engagement](./dyn365ce/cpp-customer-engagement-offer.md), Dynamics [365 for Finance and Operations](../cloud-partner-portal-orig/cpp-dynamics-365-operations-offer.md), [SaaS uygulamaları](./saas-app/cpp-saas-offer.md)ve sanal [makineler.](./virtual-machine/cpp-virtual-machine-offer.md)  Bu özellik, yayımcının teklifine göre özelleştirilmiş bir Test Sürücüsü şablonu oluşturmasını gerektirir.  Daha fazla bilgi için [Test Sürüşü](./test-drive/what-is-test-drive.md)bölümüne bakın.

[Test sürüşü filtresini](https://azuremarketplace.microsoft.com/marketplace/apps?filters=test-drive)uygulayarak Test Sürüşü gösterileri olan mevcut pazar tekliflerine göz atabilirsiniz. 


## <a name="azure-marketplace-and-appsource-offer-types"></a>Azure Marketi ve AppSource teklif türleri

Aşağıdaki tabloda [Bulut İş Ortağı Portalı](https://cloudpartner.azure.com)tarafından desteklenen geçerli teklif türleri listelenir.  Her teklif türü için, teklifin listelenebildiği pazar yeri(ler) ve teklif çözüm teknolojisinin genel bir açıklamasını listeler.

|                Teklif Türü                |  Market  |   Açıklama                                                           |
|                ----------                |  -----------  |   -----------                                                           |
| [Azure uygulaması](./azure-applications/cpp-azure-app-offer.md) | Azure | Çözüm, Azure Kaynak Yöneticisi şablonu aracılığıyla dağıtılan bir veya daha fazla sanal makineden (VM), isteğe bağlı özel Azure kodundan oluşur.  Dağıtım, bir çözüm şablonu aracılığıyla müşteri tarafından kullanılabilir veya yayımcı tarafından yönetilebilir. Bu tür, sağlanan sanal makine teklif türünden daha fazla esneklik sağlamak için kullanılır.  |
| [Danışmanlık hizmeti](./consulting-services/cloud-partner-portal-consulting-services-publishing-offer.md) | Hem | Microsoft nitelikli danışmanlar etki alanına özel hizmetlerini Azure Marketi veya AppSource'da listeleyebilir.  Uzmanlıkları, müşterilerin sorunlarını değerlendirmelerine ve iş hedeflerine ulaşmak için doğru çözümleri oluşturmalarına ve dağıtmalarına yardımcı olur.  |
| [Kapsayıcı](./containers/cpp-containers-offer.md)  | Azure | Çözüm, Kubernetes tabanlı bir hizmet veya Azure Kapsayıcı örnekleri olarak sağlanan docker kapsayıcı görüntüsüdür. |
| [Dynamics 365 Business Central](../cloud-partner-portal-orig/cpp-business-central-offer.md) | AppSource | Bu kurumsal kaynak planlama (ERP) ve iş yönetim sistemini genişleten bir paket. |
| [Müşteri Etkileşimi için Dynamics 365](./dyn365ce/cpp-customer-engagement-offer.md) | AppSource | Satış, hizmet, proje hizmeti ve saha servis modülleri aracılığıyla bu müşteri kaynak yönetimi (CRM) sistemini genişleten bir paket  |
| [Finans ve Operasyon için Dynamics 365](../cloud-partner-portal-orig/cpp-dynamics-365-operations-offer.md) | AppSource | Gelişmiş finans, operasyon, üretim ve tedarik zinciri yönetimini destekleyen bu kurumsal kaynak planlama (ERP) hizmetini genişleten bir paket |
| [IoT Edge modülü](./iot-edge-module/cpp-offer-process-parts.md) | Azure | IoT Edge aygıtında çalışan Docker uyumlu bir kapsayıcı.  Özel kod, diğer Azure hizmetleri ve üçüncü taraf hizmetlerinin bir birleşimini kullanan küçük hesaplamalı modüllerden oluşur. |
| [Power BI App](./power-bi/cpp-power-bi-offer.md) | AppSource | Veri kümeleri, raporlar ve panolar da dahil olmak üzere özelleştirilebilir Power BI içeriğini paketleyen bir Power BI uygulaması |
| [SaaS uygulaması](./saas-app/cpp-saas-offer.md) | Azure | Çözüm, kullanıcıların Azure Active Directory kullanan özelleştirilmiş bir arabirim aracılığıyla oturum açtığı yayıncı tarafından yönetilen hizmet olarak yazılım aboneliğidir. |
| [Sanal makine](./virtual-machine/cpp-virtual-machine-offer.md)  | Azure  | Çözüm, müşterinin aboneliğine dağıtılan tek bir sanal makinede bulunur.  |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |   |   |

Daha fazla bilgi için teklif [türüne göre Yayımlama kılavuzuna](../publisher-guide-by-offer-type.md)bakın.


## <a name="next-steps"></a>Sonraki adımlar

Pazar teklifleri üzerinde gerçekleştirebileceğiniz genel işlemler ve bunların ortak teknik özellikleri ve varlıkları hakkında makaleyönet [teklifleri](./manage-offers/cpp-manage-offers.md)öğreneceksiniz.
