---
title: Azure VM teknik varlık oluşturma
description: Bu makalede, Azure Marketi'nde bir kapsayıcı teklifini yapılandırma adımları ve gereksinimleri açıklanmaktadır.
author: anbene
ms.author: mingshen
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/09/2020
ms.openlocfilehash: e51b8c705533fd74a5e46eaa2570563fef485309
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81730634"
---
# <a name="create-an-azure-vm-technical-asset"></a>Azure VM teknik varlık oluşturma

> [!IMPORTANT]
> Azure Konteyner tekliflerinizin yönetimini Bulut İş Ortağı Portalı'ndan İş Ortağı Merkezi'ne taşıyoruz. Tekliflerin izne gelene kadar, lütfen Tekliflerinizi yönetmek için Bulut İş Ortağı Portalı için [Konteyner teknik varlıklarınızı hazırlayın](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/containers/cpp-create-technical-assets) yönergeleri izleyin.

Bu makalede, Azure Marketi'nde bir Kapsayıcı teklifini yapılandırma adımları ve gereksinimleri açıklanmaktadır.

## <a name="before-you-begin"></a>Başlamadan önce

Hızlı Başlangıçlar, Öğreticiler ve Örnekler için [Azure Kapsayıcı Örnekleri'ne](https://docs.microsoft.com/azure/container-instances)bakın.

## <a name="fundamental-technical-knowledge"></a>Temel teknik bilgi

Bu varlıkların tasarlanması, oluşturulması ve test edilmesi zaman alır ve hem Azure platformu hem de teklifi oluşturmak için kullanılan teknolojiler hakkında teknik bilgi gerektirir.

Çözüm etki alanınıza ek olarak, mühendislik ekibiniz aşağıdaki Microsoft teknolojileri hakkında bilgi sahibi olmalıdır:

- [Azure Hizmetlerinin](https://azure.microsoft.com/services/) temel anlayışı
- Azure uygulamaları nasıl [tasarlar ve tasarlar](https://azure.microsoft.com/solutions/architecture/)
- [Azure Sanal Makineleri,](https://azure.microsoft.com/services/virtual-machines/) [Azure Depolama](https://azure.microsoft.com/services/?filter=storage)ve [Azure Ağı](https://azure.microsoft.com/services/?filter=networking) çalışma bilgisi
- [Azure Kaynak Yöneticisi'nin](https://azure.microsoft.com/features/resource-manager/) çalışma bilgisi
- [JSON](https://www.json.org/)Çalışma Bilgisi .

## <a name="suggested-tools"></a>Önerilen araçlar

Kapsayıcı imajınızı yönetmenize yardımcı olmak için aşağıdaki komut dosyası ortamlarından birini veya her ikisini seçin:

- [Azure PowerShell](https://docs.microsoft.com/powershell/azure/?view=azps-3.7.0&viewFallbackFrom=azps-3.6.1)
- [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest).

Bu araçları geliştirme ortamınıza eklemenizi öneririz:

- [Azure Depolama Gezgini](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows)
- [Visual Studio Code](https://code.visualstudio.com/)
  - Uzantı: [Azure Kaynak Yöneticisi Araçları](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)
  - Uzantısı: [Güzelleştirmek](https://marketplace.visualstudio.com/items?itemName=HookyQR.beautify)
  - Uzantısı: [Prettify JSON](https://marketplace.visualstudio.com/items?itemName=mohsen1.prettify-json).

[Azure Geliştirici Araçları](https://azure.microsoft.com/) sayfasındaki kullanılabilir araçları inceleyin. Visual Studio kullanıyorsanız, [Visual Studio Marketplace'te](https://marketplace.visualstudio.com/)bulunan araçları inceleyin.

## <a name="create-the-container-image"></a>Kapsayıcı görüntüsünü oluşturma

Daha fazla bilgi için aşağıdaki öğreticilere bakın:

- [Öğretici: Azure Kapsayıcı Örneklerine dağıtım için kapsayıcı görüntüsü oluşturma](https://docs.microsoft.com/azure/container-instances/container-instances-tutorial-prepare-app)
- [Öğretici: Azure Kapsayıcı Kayıt Defteri Görevleri ile bulutta kapsayıcı görüntüleri oluşturun ve dağıtın.](https://docs.microsoft.com/azure/container-registry/container-registry-tutorial-quick-task)

## <a name="next-step"></a>Sonraki adım

- [Konteyner teklifinizi oluşturun.](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-azure-container-offer)
