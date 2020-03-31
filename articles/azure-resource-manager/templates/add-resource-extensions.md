---
title: Uzantıları kullanarak dağıtım sonrası yapılandırma
description: Dağıtım sonrası yapılandırmaları sağlamak için Azure Kaynak Yöneticisi şablon uzantılarını nasıl kullanacağınızı öğrenin.
author: mumian
ms.topic: conceptual
ms.date: 12/14/2018
ms.author: jgao
ms.openlocfilehash: b3c4110c8761b3e8daf324d65ac7fa1dcbcdf61f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77023506"
---
# <a name="provide-post-deployment-configurations-by-using-extensions"></a>Uzantıları kullanarak dağıtım sonrası yapılandırmaları sağlama

Şablon uzantıları, Azure kaynaklarında dağıtım sonrası yapılandırma ve otomasyon görevleri sağlayan küçük uygulamalardır. En popüler olanı sanal makine uzantıları. [Windows için Sanal makine uzantıları ve özellikleri](../../virtual-machines/extensions/features-windows.md)ve Linux için Sanal makine uzantıları ve [özellikleri](../../virtual-machines/extensions/features-linux.md)görün.

## <a name="extensions"></a>Uzantılar

Varolan uzantılar şunlardır:

- [Microsoft.Compute/virtualMachines/uzantılar](https://docs.microsoft.com/azure/templates/microsoft.compute/2018-10-01/virtualmachines/extensions)
- [Microsoft.Compute virtualMachineScaleSets/uzantılar](https://docs.microsoft.com/azure/templates/microsoft.compute/2018-10-01/virtualmachinescalesets/extensions)
- [Microsoft.HDInsight kümeleri/uzantıları](https://docs.microsoft.com/azure/templates/microsoft.hdinsight/2018-06-01-preview/clusters)
- [Microsoft.Sql sunucuları/veritabanları/uzantıları](https://docs.microsoft.com/azure/templates/microsoft.sql/2014-04-01/servers/databases/extensions) 
- [Microsoft.Web/siteler/site uzantıları](https://docs.microsoft.com/azure/templates/microsoft.web/2016-08-01/sites/siteextensions)

Kullanılabilir uzantıları bulmak için [şablon başvurusuna](https://docs.microsoft.com/azure/templates/)göz atın. **Başlığa göre**Filtrele'de, **uzantıyı**girin.

Bu uzantıları nasıl kullanacağınızı öğrenmek için bkz:

- [Öğretici: Azure Kaynak Yöneticisi şablonlarıyla sanal makine uzantılarını dağıtın.](template-tutorial-deploy-vm-extensions.md)
- [Öğretici: Azure Resource Manager şablonlarıyla SQL BACPAC dosyalarını içeri aktarma](template-tutorial-deploy-sql-extensions-bacpac.md)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: Azure Resource Manager şablonlarıyla sanal makine uzantılarını dağıtma](template-tutorial-deploy-vm-extensions.md)
