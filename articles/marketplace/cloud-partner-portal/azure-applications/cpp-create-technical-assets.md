---
title: Azure uygulama teknik varlıkları oluşturma | Azure Marketi
description: Azure uygulama teklifi için teknik varlıkları oluşturun.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 12/13/2018
ms.author: dsindona
ms.openlocfilehash: 041b2133ed63a906d3fea3ab67890a0057151b9b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80285274"
---
# <a name="prepare-your-azure-application-technical-assets"></a>Azure uygulama teknik varlıklarınızı hazırlayın

Bu makalede, Azure uygulama teklifiniz için teknik varlıkları hazırlamak için kaynaklar açıklanmaktadır.

## <a name="before-you-begin"></a>Başlamadan önce

Azure Uygulaması çözümlerini tanımlamak için azure kaynak yöneticisi şablonu nasıl yazılanın ve uygulama teklifini azure marketinde nasıl yayınlayacağına ilişkin aşağıdaki videoyu, Çözüm [Şablonları Oluşturmayı ve Yönetilen Uygulamaları](https://channel9.msdn.com/Events/Build/2018/BRK3603)gözden geçirin.

>[!VIDEO https://channel9.msdn.com/Events/Build/2018/BRK3603/player]


Hızlı Başlangıçlar, Öğreticiler ve Örnekler sağlayan aşağıdaki Azure uygulama belgelerini gözden geçirin.

- [Azure Kaynak Yöneticisi Şablonlarını Anlama](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates)
- Hızlı Başlangıçlar:

  - [Azure Quickstart şablonları](https://azure.microsoft.com/documentation/templates/)
  - [GitHub Azure Quickstart şablonları](https://github.com/azure/azure-quickstart-templates)
  - [Uygulama tanımını yayımlama](https://docs.microsoft.com/azure/managed-applications/publish-managed-app-definition-quickstart)
  - [Hizmet kataloğu uygulaması dağıtma](https://docs.microsoft.com/azure/managed-applications/deploy-service-catalog-quickstart)

  
- Öğreticiler:

  - [Tanım dosyaları oluşturma](https://docs.microsoft.com/azure/managed-applications/publish-service-catalog-app)
  - [Market uygulaması yayımlama](https://docs.microsoft.com/azure/managed-applications/publish-marketplace-app)

  - Örnekleri:

    - [Azure CLI](https://docs.microsoft.com/azure/managed-applications/cli-samples)
    - [Azure PowerShell](https://docs.microsoft.com/azure/managed-applications/powershell-samples)
    - [Yönetilen uygulama çözümleri](https://docs.microsoft.com/azure/managed-applications/sample-projects)

## <a name="fundamental-technical-knowledge"></a>Temel teknik bilgi

Bu varlıkların tasarlanması, oluşturulması ve test edilmesi zaman alır ve hem Azure platformu hem de teklifi oluşturmak için kullanılan teknolojiler hakkında teknik bilgi gerektirir.

Mühendislik ekibiniz aşağıdaki Microsoft teknolojileri hakkında bilgi sahibi olmalıdır:

- [Azure Hizmetlerinin](https://azure.microsoft.com/services/) temel anlayışı
- Azure uygulamaları nasıl [tasarlar ve tasarlar](https://azure.microsoft.com/solutions/architecture/)
- [Azure Sanal Makineleri,](https://azure.microsoft.com/services/virtual-machines/) [Azure Depolama](https://azure.microsoft.com/services/?filter=storage)ve [Azure Ağı](https://azure.microsoft.com/services/?filter=networking) çalışma bilgisi
- [Azure Kaynak Yöneticisi'nin](https://azure.microsoft.com/features/resource-manager/) çalışma bilgisi
- [JSON](https://www.json.org/) çalışma bilgisi

## <a name="suggested-tools"></a>Önerilen araçlar

Azure uygulamanızı yönetmenize yardımcı olmak için aşağıdaki komut dosyası ortamlarından birini veya her ikisini seçin:

- [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)
- [Azure CLI](https://docs.microsoft.com/cli/azure)

Geliştirme ortamınıza aşağıdaki araçları eklemenizi öneririz:

- [Azure Storage Gezgini](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer)
- Aşağıdaki uzantıları ile [Görsel Studio Kodu:](https://code.visualstudio.com/)

  - Uzantı: [Azure Kaynak Yöneticisi Araçları](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)
  - Uzantısı: [Güzelleştirmek](https://marketplace.visualstudio.com/items?itemName=HookyQR.beautify)
  - Uzatma: [Prettify JSON](https://marketplace.visualstudio.com/items?itemName=mohsen1.prettify-json)

[Ayrıca Azure Geliştirici Araçları](https://azure.microsoft.com/tools/) sayfasında ve Visual Studio kullanıyorsanız, Visual Studio [Marketplace'te](https://marketplace.visualstudio.com/)mevcut araçları incelemenizi öneririz.

## <a name="next-steps"></a>Sonraki adımlar

[Azure uygulama teklifi oluşturma](./cpp-create-offer.md)

