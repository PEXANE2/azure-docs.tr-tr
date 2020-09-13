---
title: dosya dahil etme
description: dosya dahil etme
services: data-factory
author: nabhishek
ms.service: data-factory
ms.topic: include
ms.date: 10/09/2019
ms.author: abnarain
ms.openlocfilehash: 0b2831321c9f4d855a19605c1ce5ace9fa11c88b
ms.sourcegitcommit: 1b320bc7863707a07e98644fbaed9faa0108da97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2020
ms.locfileid: "89596132"
---
| Etki alanı adları                  | Giden bağlantı noktaları | Description                              |
| ----------------------------- | -------------- | ---------------------------------------- |
| `*.servicebus.windows.net`    | 443            | Şirket içinde barındırılan tümleştirme çalışma zamanının Azure Data Factory içindeki veri taşıma hizmetlerine bağlanması için gereklidir. |
| `{datafactory}.{region}.datafactory.azure.net`<br> veya `*.frontend.clouddatahub.net` | 443            | Şirket içinde barındırılan tümleştirme çalışma zamanı tarafından Data Factory hizmetine bağlanmak için gereklidir. <br>Yeni oluşturulan Data Factory için, lütfen {DataFactory} biçiminde olan şirket içinde barındırılan Integration Runtime anahtarınızdan FQDN 'yi bulun. {Region}. DataFactory. Azure. net. Eski Veri Fabrikası için, FQDN 'yi şirket içinde barındırılan tümleştirme anahtarınıza görmüyorsanız lütfen bunun yerine *. frontend.clouddatahub.net kullanın. |
| `download.microsoft.com`    | 443            | Güncelleştirmeleri indirmek için şirket içinde barındırılan tümleştirme çalışma zamanı için gereklidir. Otomatik güncelleştirmeyi devre dışı bırakırsanız, bu etki alanını yapılandırmayı atlayabilirsiniz. |
| `*.core.windows.net`          | 443            | Kendi kendine barındırılan tümleştirme çalışma zamanı tarafından, [hazırlanan kopyalama](https://docs.microsoft.com/azure/data-factory/copy-activity-performance#staged-copy) özelliğini kullandığınızda Azure depolama hesabına bağlanmak için kullanılır. |
| `*.database.windows.net`      | 1433           | Yalnızca, Azure SQL veritabanı veya Azure SYNAPSE Analytics 'ten ya da isteğe bağlı olarak, aksi takdirde gerekir. 1433 numaralı bağlantı noktasını açmadan verileri SQL veritabanı veya SYNAPSE Analytics 'e kopyalamak için hazırlanan kopyalama özelliğini kullanın. |
| `*.azuredatalakestore.net`<br>`login.microsoftonline.com/<tenant>/oauth2/token`    | 443            | Yalnızca Azure Data Lake Store 'den veya ' a kopyaladığınızda ve isteğe bağlı olarak, aksi takdirde gereklidir. |
