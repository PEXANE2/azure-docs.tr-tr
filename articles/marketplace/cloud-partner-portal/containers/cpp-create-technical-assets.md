---
title: Azure kapsayıcıları görüntüsü teknik varlıkları oluşturma | Azure Marketi
description: Azure kapsayıcısı için teknik varlıklar oluşturun.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 11/01/2018
ms.author: dsindona
ms.openlocfilehash: 68db606c9a01c4b1122f9b0cce620762485ca40a
ms.sourcegitcommit: f7fb9e7867798f46c80fe052b5ee73b9151b0e0b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82148272"
---
# <a name="prepare-your-container-technical-assets"></a>Kapsayıcınızın teknik varlıklarını hazırlama

> [!IMPORTANT]
> 13 Nisan 2020 ' den itibaren, Azure Container tekliflerinizin yönetimini Iş Ortağı Merkezi 'ne taşımaya başlayacağız. Geçişten sonra, Iş Ortağı Merkezi 'nde tekliflerinizi oluşturup yönetirsiniz. Geçirilen tekliflerinizi yönetmek için [Azure Container teknik varlıklarınızı hazırlama](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-azure-container-offer) bölümündeki yönergeleri izleyin.

Bu makalede, Azure Marketi 'ni sunan bir kapsayıcıyı yapılandırmaya yönelik adımlar ve gereksinimler açıklanmaktadır.

## <a name="before-you-begin"></a>Başlamadan önce

Hızlı başlangıç, öğreticiler ve örnekler sağlayan [Azure Container Instances](https://docs.microsoft.com/azure/container-instances) belgelerini gözden geçirin.

## <a name="fundamental-technical-knowledge"></a>Temel teknik bilgi

Bu varlıkları tasarlama, oluşturma ve test etme zaman alır ve hem Azure platformu hem de teklifi oluşturmak için kullanılan teknolojiler hakkında teknik bilgi gerektirir.
 
Çözüm etki alanınız ' ne ek olarak, mühendislik ekibiniz aşağıdaki Microsoft teknolojileri hakkında bilgi sahibi olmalıdır:

-    [Azure hizmetlerini](https://azure.microsoft.com/services/) temel olarak anlama 
-    [Azure uygulamalarını tasarlama ve mimarın](https://azure.microsoft.com/solutions/architecture/)
-    [Azure sanal makineler](https://azure.microsoft.com/services/virtual-machines/), [Azure depolama](https://azure.microsoft.com/services/?filter=storage) ve [Azure ağı](https://azure.microsoft.com/services/?filter=networking) ile çalışma hakkında bilgi
-    [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/) çalışma bilgileri
-    [JSON](https://www.json.org/) hakkında çalışma bilgisi

## <a name="suggested-tools"></a>Önerilen araçlar

Kapsayıcı görüntünüzü yönetmeye yardımcı olması için aşağıdaki komut dosyası ortamlarının birini veya her ikisini birden seçin:

-    [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)
-    [Azure CLI](https://docs.microsoft.com/cli/azure)

Ayrıca, aşağıdaki araçların geliştirme ortamınıza eklenmesini öneririz:

-    [Azure Depolama Gezgini](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer)
-    [Visual Studio Code](https://code.visualstudio.com/)
    *    Uzantı: [Azure Resource Manager araçları](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)
    *    Uzantı: [Beautify](https://marketplace.visualstudio.com/items?itemName=HookyQR.beautify)
    *    Uzantı: [prettify JSON](https://marketplace.visualstudio.com/items?itemName=mohsen1.prettify-json)

Ayrıca, [Azure Geliştirici Araçları](https://azure.microsoft.com/tools/) sayfasındaki kullanılabilir araçların incelenmesi önerilir ve Visual Studio kullanıyorsanız, [Visual Studio Market](https://marketplace.visualstudio.com/).

## <a name="create-the-container-image"></a>Kapsayıcı görüntüsünü oluşturma

Daha fazla bilgi için aşağıdakilere bakın:

* [Öğretici: Azure Container Instances dağıtım için bir kapsayıcı görüntüsü oluşturma](https://docs.microsoft.com/azure/container-instances/container-instances-tutorial-prepare-app)
* [Öğretici: Azure Container Registry görevlerle bulutta kapsayıcı görüntüleri oluşturun ve dağıtın](https://docs.microsoft.com/azure/container-registry/container-registry-tutorial-quick-task)

## <a name="next-steps"></a>Sonraki adımlar

[Kapsayıcı Teklifinizi oluşturma](./cpp-create-offer.md)
