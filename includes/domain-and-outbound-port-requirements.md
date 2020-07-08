---
title: dosya dahil etme
description: dosya dahil etme
services: data-factory
author: nabhishek
ms.service: data-factory
ms.topic: include
ms.date: 10/09/2019
ms.author: abnarain
ms.openlocfilehash: 6af6795fefb41f1d8f2b56e0aa1fb367fc18cee2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "74559294"
---
| Etki alanı adları                  | Giden bağlantı noktaları | Açıklama                              |
| ----------------------------- | -------------- | ---------------------------------------- |
| `*.servicebus.windows.net`    | 443            | Şirket içinde barındırılan tümleştirme çalışma zamanının Azure Data Factory içindeki veri taşıma hizmetlerine bağlanması için gereklidir. |
| `*.frontend.clouddatahub.net` | 443            | Şirket içinde barındırılan tümleştirme çalışma zamanı tarafından Data Factory hizmetine bağlanmak için gereklidir. |
| `download.microsoft.com`    | 443            | Güncelleştirmeleri indirmek için şirket içinde barındırılan tümleştirme çalışma zamanı için gereklidir. Otomatik güncelleştirmeyi devre dışı bırakırsanız, bu etki alanını yapılandırmayı atlayabilirsiniz. |
| `*.core.windows.net`          | 443            | Kendi kendine barındırılan tümleştirme çalışma zamanı tarafından, [hazırlanan kopyalama](https://docs.microsoft.com/azure/data-factory/copy-activity-performance#staged-copy) özelliğini kullandığınızda Azure depolama hesabına bağlanmak için kullanılır. |
| `*.database.windows.net`      | 1433           | Yalnızca, Azure SQL veritabanı veya Azure SQL veri ambarı 'ndan ya da isteğe bağlı olarak, aksi takdirde gerekir. SQL veritabanı veya SQL veri ambarı 'na veri kopyalamak için 1433 numaralı bağlantı noktasını açmadan hazırlanan kopyalama özelliğini kullanın. |
| `*.azuredatalakestore.net`<br>`login.microsoftonline.com/<tenant>/oauth2/token`    | 443            | Yalnızca Azure Data Lake Store 'den veya ' a kopyaladığınızda ve isteğe bağlı olarak, aksi takdirde gereklidir. |
