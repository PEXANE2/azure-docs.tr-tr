---
title: Uzantıları kullanarak dağıtım sonrası yapılandırma
description: Dağıtım sonrası konfigürasyonlar sağlamak için Azure Resource Manager Şablon Uzantıları kullanmayı öğrenin.
author: mumian
ms.topic: conceptual
ms.date: 12/14/2018
ms.author: jgao
ms.openlocfilehash: be55138a2aa6dc0552c7556438ffd43705687c87
ms.sourcegitcommit: bcb962e74ee5302d0b9242b1ee006f769a94cfb8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86055060"
---
# <a name="provide-post-deployment-configurations-by-using-extensions"></a>Uzantıları kullanarak dağıtım sonrası yapılandırma sağlama

Şablon Uzantıları, Azure kaynakları üzerinde dağıtım sonrası yapılandırma ve otomasyon görevleri sağlayan küçük uygulamalardır. En popüler bir sanal makine uzantılarıdır. Bkz. [Windows Için sanal makine uzantıları ve özellikleri](../../virtual-machines/extensions/features-windows.md)ve [Linux için sanal makine uzantıları ve özellikleri](../../virtual-machines/extensions/features-linux.md).

## <a name="extensions"></a>Uzantıları

Mevcut uzantılar şunlardır:

- [Microsoft. COMPUTE/virtualMachines/uzantıları](/azure/templates/microsoft.compute/2018-10-01/virtualmachines/extensions)
- [Microsoft. COMPUTE virtualMachineScaleSets/uzantıları](/azure/templates/microsoft.compute/2018-10-01/virtualmachinescalesets/extensions)
- [Microsoft. HDInsight kümeleri/uzantıları](/azure/templates/microsoft.hdinsight/2018-06-01-preview/clusters)
- [Microsoft. SQL sunucuları/veritabanları/uzantıları](/azure/templates/microsoft.sql/2014-04-01/servers/databases/extensions) 
- [Microsoft. Web/Sites/siteextensions](/azure/templates/microsoft.web/2016-08-01/sites/siteextensions)

Kullanılabilir uzantıları bulmak için [şablon başvurusuna](/azure/templates/)gidin. **Başlığa göre filtrele**' de **uzantı**girin.

Bu uzantıların nasıl kullanılacağını öğrenmek için bkz.:

- [Öğretici: Azure Resource Manager şablonlarıyla sanal makine uzantıları dağıtın](template-tutorial-deploy-vm-extensions.md).
- [Öğretici: Azure Resource Manager şablonlarıyla SQL BACPAC dosyalarını içeri aktarma](template-tutorial-deploy-sql-extensions-bacpac.md)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: Azure Resource Manager şablonlarıyla sanal makine uzantılarını dağıtma](template-tutorial-deploy-vm-extensions.md)
