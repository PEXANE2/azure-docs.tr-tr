---
title: Kaynak Yöneticisi şablonu örnekleri-Azure ön kapısı
description: Azure ön kapısı için sunulan örnek Azure Resource Manager şablonları hakkında bilgiler.
services: frontdoor
author: johndowns
ms.author: jodowns
ms.service: frontdoor
ms.topic: sample
ms.date: 04/16/2021
ms.openlocfilehash: 3559270710f56d43fe486e2e3d3e41c63e2a619b
ms.sourcegitcommit: 590f14d35e831a2dbb803fc12ebbd3ed2046abff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2021
ms.locfileid: "107565840"
---
# <a name="azure-resource-manager-templates-for-azure-front-door"></a>Azure ön kapısına yönelik Azure Resource Manager şablonları

> [!Note]
> Bu belge, Azure ön kapısının Standart/Premium (Önizleme) içindir. Azure ön kapısı hakkında bilgi mi arıyorsunuz? [Burada](../front-door-overview.md)görüntüleyin.

Aşağıdaki tabloda, diğer Azure hizmetleri dahil olmak üzere başvuru mimarilerine sahip Azure ön kapısının Azure Resource Manager şablonlarının bağlantıları yer almaktadır.

| Örnek | Açıklama |
|-|-|
| [Ön kapı (hızlı oluşturma)](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-standard-premium/) | Uç nokta, kaynak grubu, kaynak ve yol dahil olmak üzere temel bir ön kapı profili oluşturur.  |
| [Kural kümesi](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-standard-premium-rule-set/) | Ön kapı profili ve kural kümesi oluşturur.  |
| [Özel kuralla WAF ilkesi](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-standard-premium-waf-custom/) | Özel kuralla bir ön kapı profili ve WAF oluşturur.  |
|**App Service kaynakları**| **Açıklama** |
| [App Service](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-standard-premium-app-service-public) | Ortak uç nokta ve bir ön kapı profili olan App Service bir uygulama oluşturur.  |
| [Özel bağlantıyla birlikte App Service](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-premium-app-service-private-link) | Özel bir uç nokta ve bir ön kapı profili olan App Service bir uygulama oluşturur.  |
| [Özel bağlantıyla App Service ortam](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-premium-app-service-environment-internal-private-link) | Bir App Service ortamı, özel bir uç noktası olan bir uygulama ve bir ön kapı profili oluşturur.  |
|**Azure Işlevleri kaynakları**| **Açıklama** |
| [Azure İşlevleri](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-standard-premium-function-public/) | Genel bir uç nokta ve bir ön kapı profili içeren bir Azure Işlevleri uygulaması oluşturur.  |
| [Özel bağlantısı olan Azure Işlevleri](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-premium-function-private-link) | Özel bir uç nokta ve bir ön kapı profili içeren bir Azure Işlevleri uygulaması oluşturur.  |
|**API Management kaynakları**| **Açıklama** |
| [API Management (harici)](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-standard-premium-api-management-external) | Dış VNet tümleştirmesiyle bir API Management örneği ve bir ön kapı profili oluşturur.  |
|**Depolama kaynakları**| **Açıklama** |
| [Depolama statik Web sitesi](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-standard-premium-storage-static-website) | Bir Azure depolama hesabı ve statik Web sitesini ortak bir uç nokta ve bir ön kapı profili oluşturur.  |
| [Özel bağlantısı olan depolama Blobları](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-premium-storage-blobs-private-link) | Bir Azure depolama hesabı ve bir özel uç nokta ile blob kapsayıcısı ve bir ön kapı profili oluşturur.  |
|**Application Gateway kaynakları**| **Açıklama** |
| [Application Gateway](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-standard-premium-application-gateway-public) | Bir Application Gateway ve bir ön kapı profili oluşturur. |
|**Sanal makine kaynakları**| **Açıklama** |
| [Özel bağlantı hizmeti olan sanal makine](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-premium-vm-private-link) | Bir sanal makine ve özel bağlantı hizmeti ve bir ön kapı profili oluşturur. |
| | |
