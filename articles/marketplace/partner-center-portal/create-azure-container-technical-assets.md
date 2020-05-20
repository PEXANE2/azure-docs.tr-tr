---
title: Azure Container teklifleri için teknik kavramlar-Microsoft ticari Market
description: Azure Marketi 'nde bir kapsayıcı teklifi yapılandırmanıza yardımcı olacak teknik kaynak ve yönergeler.
author: anbene
ms.author: mingshen
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/09/2020
ms.openlocfilehash: 87c915330fdc0d59d0d32c0747c7dc7647c342ba
ms.sourcegitcommit: 595cde417684e3672e36f09fd4691fb6aa739733
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83700973"
---
# <a name="create-an-azure-container-offer"></a>Azure Container teklifi oluşturma

Bu makalede, Azure Marketi 'nde bir kapsayıcı teklifi oluşturmanıza yardımcı olacak teknik kaynaklar ve öneriler sunulmaktadır.

## <a name="before-you-begin"></a>Başlamadan önce

Hızlı başlangıç, öğreticiler ve örnekler için [Azure Container Instances belgelerine](https://docs.microsoft.com/azure/container-instances)bakın.

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

## <a name="next-steps"></a>Sonraki adımlar

- [Kapsayıcı teklifinizi oluşturun](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-azure-container-offer).
