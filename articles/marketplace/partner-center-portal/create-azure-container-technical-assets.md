---
title: Azure Container teklifleri için teknik kavramlar-Microsoft ticari Market
description: Azure Marketi 'nde bir kapsayıcı teklifi yapılandırmanıza yardımcı olacak teknik kaynak ve yönergeler.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: anbene
ms.author: mingshen
ms.date: 04/09/2020
ms.openlocfilehash: da0fa53567e5c58db8e8394ef5562255d8a7142d
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86104290"
---
# <a name="create-an-azure-container-offer"></a>Azure Container teklifi oluşturma

Bu makalede, Azure Marketi 'nde bir kapsayıcı teklifi oluşturmanıza yardımcı olacak teknik kaynaklar ve öneriler sunulmaktadır.

## <a name="before-you-begin"></a>Başlamadan önce

Hızlı başlangıç, öğreticiler ve örnekler için [Azure Container Instances belgelerine](../../container-instances/index.yml)bakın.

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

- [Azure Depolama Gezgini](../../vs-azure-tools-storage-manage-with-storage-explorer.md?tabs=windows)
- [Visual Studio Code](https://code.visualstudio.com/)
  - Uzantı: [Azure Resource Manager araçları](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)
  - Uzantı: [Beautify](https://marketplace.visualstudio.com/items?itemName=HookyQR.beautify)
  - Uzantı: [prettify JSON](https://marketplace.visualstudio.com/items?itemName=mohsen1.prettify-json).

[Azure Geliştirici Araçları](https://azure.microsoft.com/) sayfasındaki kullanılabilir araçları gözden geçirin. Visual Studio kullanıyorsanız, [Visual Studio Market](https://marketplace.visualstudio.com/)bulunan araçları gözden geçirin.

## <a name="create-the-container-image"></a>Kapsayıcı görüntüsünü oluşturma

Daha fazla bilgi için aşağıdaki öğreticilere bakın:

- [Öğretici: Azure Container Instances dağıtım için bir kapsayıcı görüntüsü oluşturma](../../container-instances/container-instances-tutorial-prepare-app.md)
- [Öğretici: Azure Container Registry görevlerle bulutta kapsayıcı görüntüleri oluşturun ve dağıtın](../../container-registry/container-registry-tutorial-quick-task.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Kapsayıcı teklifinizi oluşturun](create-azure-container-offer.md).
