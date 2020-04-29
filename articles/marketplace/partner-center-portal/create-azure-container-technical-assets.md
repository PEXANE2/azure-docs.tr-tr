---
title: Azure VM teknik varlığı oluşturma
description: Bu makalede, Azure Marketi 'nde bir kapsayıcı teklifini yapılandırmaya yönelik adımlar ve gereksinimler açıklanmaktadır.
author: anbene
ms.author: mingshen
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/09/2020
ms.openlocfilehash: e51b8c705533fd74a5e46eaa2570563fef485309
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81730634"
---
# <a name="create-an-azure-vm-technical-asset"></a>Azure VM teknik varlığı oluşturma

> [!IMPORTANT]
> Azure Container tekliflerinizin yönetimini Bulut İş Ortağı Portalı Iş Ortağı Merkezi 'ne taşıdık. Teklifleriniz geçirilene kadar, tekliflerinizi yönetmek için lütfen [kapsayıcınızı hazırlama teknik varlıklarını](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/containers/cpp-create-technical-assets) bulut iş ortağı portalı için hazırlama bölümündeki yönergeleri izleyin.

Bu makalede, Azure Marketi 'nde bir kapsayıcı teklifini yapılandırmaya yönelik adımlar ve gereksinimler açıklanmaktadır.

## <a name="before-you-begin"></a>Başlamadan önce

Hızlı başlangıç, öğreticiler ve örnekler için bkz. [Azure Container Instances](https://docs.microsoft.com/azure/container-instances).

## <a name="fundamental-technical-knowledge"></a>Temel teknik bilgi

Bu varlıkların tasarlanmasına, oluşturulmasına ve test edilmesine zaman alır ve hem Azure platformu hem de teklifi oluşturmak için kullanılan teknolojiler hakkında teknik bilgi gerektirir.

Çözüm etki alanınız ' ne ek olarak, mühendislik takımınız aşağıdaki Microsoft teknolojileri hakkında bilgi sahibi olmalıdır:

- [Azure hizmetlerini](https://azure.microsoft.com/services/) temel olarak anlama
- [Azure uygulamalarını tasarlama ve mimarın](https://azure.microsoft.com/solutions/architecture/)
- [Azure sanal makineler](https://azure.microsoft.com/services/virtual-machines/), [Azure depolama](https://azure.microsoft.com/services/?filter=storage)ve [Azure ağı](https://azure.microsoft.com/services/?filter=networking) ile çalışma hakkında bilgi
- [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/) çalışma bilgileri
- [JSON](https://www.json.org/)hakkında çalışma bilgisi.

## <a name="suggested-tools"></a>Önerilen araçlar

Kapsayıcı görüntünüzü yönetmeye yardımcı olması için aşağıdaki komut dosyası ortamlarının birini veya her ikisini birden seçin:

- [Azure PowerShell](https://docs.microsoft.com/powershell/azure/?view=azps-3.7.0&viewFallbackFrom=azps-3.6.1)
- [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest).

Bu araçları geliştirme ortamınıza eklemeniz önerilir:

- [Azure Depolama Gezgini](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows)
- [Visual Studio Code](https://code.visualstudio.com/)
  - Uzantı: [Azure Resource Manager araçları](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)
  - Uzantı: [Beautify](https://marketplace.visualstudio.com/items?itemName=HookyQR.beautify)
  - Uzantı: [prettify JSON](https://marketplace.visualstudio.com/items?itemName=mohsen1.prettify-json).

[Azure Geliştirici Araçları](https://azure.microsoft.com/) sayfasındaki kullanılabilir araçları gözden geçirin. Visual Studio kullanıyorsanız, [Visual Studio Market](https://marketplace.visualstudio.com/)bulunan araçları gözden geçirin.

## <a name="create-the-container-image"></a>Kapsayıcı görüntüsünü oluşturma

Daha fazla bilgi için aşağıdaki öğreticilere bakın:

- [Öğretici: Azure Container Instances dağıtım için bir kapsayıcı görüntüsü oluşturma](https://docs.microsoft.com/azure/container-instances/container-instances-tutorial-prepare-app)
- [Öğretici: Azure Container Registry görevlerle bulutta kapsayıcı görüntüleri oluşturun ve dağıtın](https://docs.microsoft.com/azure/container-registry/container-registry-tutorial-quick-task).

## <a name="next-step"></a>Sonraki adım

- [Kapsayıcı teklifinizi oluşturun](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-azure-container-offer).
