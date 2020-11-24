---
title: Azure Container teknik varlıklarınızı hazırlayın
description: Azure Marketi 'nde bir kapsayıcı teklifi yapılandırmanıza yardımcı olacak teknik kaynak ve yönergeler.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: keferna
ms.author: keferna
ms.date: 11/30/2020
ms.openlocfilehash: 30f9eac0a0e88092a243b4d9421d2a4f33b07841
ms.sourcegitcommit: 1bf144dc5d7c496c4abeb95fc2f473cfa0bbed43
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2020
ms.locfileid: "95759035"
---
# <a name="prepare-your-azure-container-technical-assets"></a>Azure Container teknik varlıklarınızı hazırlayın

Bu makalede, Azure Marketi 'nde bir kapsayıcı teklifi oluşturmanıza yardımcı olacak teknik kaynaklar ve öneriler sunulmaktadır.

## <a name="before-you-begin"></a>Başlamadan önce

Hızlı başlangıç, öğreticiler ve örnekler için [Azure Container Instances belgelerine](/azure/container-instances/)bakın.

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

- [Azure PowerShell](/powershell/azure/)
- [Azure CLI](/cli/azure/).

Bu araçları geliştirme ortamınıza eklemeniz önerilir:

- [Azure Depolama Gezgini](/azure/vs-azure-tools-storage-manage-with-storage-explorer.md?tabs=windows)
- [Visual Studio Code](https://code.visualstudio.com/)
  - Uzantı: [Azure Resource Manager araçları](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)
  - Uzantı: [Beautify](https://marketplace.visualstudio.com/items?itemName=HookyQR.beautify)
  - Uzantı: [prettify JSON](https://marketplace.visualstudio.com/items?itemName=mohsen1.prettify-json).

[Azure Geliştirici Araçları](https://azure.microsoft.com/) sayfasındaki kullanılabilir araçları gözden geçirin. Visual Studio kullanıyorsanız, [Visual Studio Market](https://marketplace.visualstudio.com/)bulunan araçları gözden geçirin.

## <a name="create-the-container-image"></a>Kapsayıcı görüntüsünü oluşturma

Bir görüntüyü şirket içi kayıt defterinden Azure Container Instances dağıtamazsınız.

- Yerel kayıt defterinizde zaten bir çalışma kapsayıcınız varsa, bir Azure kayıt defteri oluşturun ve kapsayıcı görüntünüzü Azure Container Registry yükleyin. Daha fazla bilgi edinmek için bkz. [öğretici: bulutta kapsayıcı görüntülerini derlemek ve Azure Container Registry görevlerle dağıtmak](/azure/container-registry/container-registry-tutorial-quick-task.md).

- Henüz bir kapsayıcı görüntüsü yoksa ve var olan uygulamanızı kapsayıcınıza veya yeni bir kapsayıcı tabanlı uygulama oluşturup, uygulama kaynak kodunu GitHub 'dan klonlamanız, uygulama kaynağından bir kapsayıcı görüntüsü oluşturmanız ve görüntüyü yerel bir Docker ortamında test etmeniz gerekir. Daha fazla bilgi edinmek için bkz. [öğretici: Azure Container Instances dağıtım için kapsayıcı görüntüsü oluşturma](/azure/container-instances/container-instances-tutorial-prepare-app.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Kapsayıcı Teklifinizi oluşturma](create-azure-container-offer.md)
