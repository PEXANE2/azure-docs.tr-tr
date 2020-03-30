---
title: include dosyası
description: include dosyası
services: data-factory
author: nabhishek
ms.service: data-factory
ms.topic: include
ms.date: 10/09/2019
ms.author: abnarain
ms.openlocfilehash: 6af6795fefb41f1d8f2b56e0aa1fb367fc18cee2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74559294"
---
| Etki alanı adları                  | Giden bağlantı noktaları | Açıklama                              |
| ----------------------------- | -------------- | ---------------------------------------- |
| `*.servicebus.windows.net`    | 443            | Azure Veri Fabrikası'ndaki veri hareketi hizmetlerine bağlanmak için kendi barındırılan tümleştirme çalışma süresi nin gerekli olması gerekir. |
| `*.frontend.clouddatahub.net` | 443            | Veri Fabrikası hizmetine bağlanmak için kendi barındırılan tümleştirme çalışma süresi nin gerektirdiği. |
| `download.microsoft.com`    | 443            | Güncelleştirmeleri indirmek için kendi barındırılan tümleştirme çalışma zamanı tarafından gereklidir. Otomatik güncelleştirmeyi devre dışı bıraktıysanız, bu etki alanını yapılandırmayı atlayabilirsiniz. |
| `*.core.windows.net`          | 443            | [Sahnelenen kopyalama](https://docs.microsoft.com/azure/data-factory/copy-activity-performance#staged-copy) özelliğini kullandığınızda Azure depolama hesabına bağlanmak için kendi barındırılan tümleştirme çalışma zamanı tarafından kullanılır. |
| `*.database.windows.net`      | 1433           | Yalnızca Azure SQL Veritabanı'ndan veya Azure SQL Veri Ambarı'ndan kopya ladiğinizde ve isteğe bağlı olarak gereklidir. 1433 bağlantı noktasını açmadan verileri SQL Database veya SQL Veri Ambarı'na kopyalamak için aşamalı kopyalama özelliğini kullanın. |
| `*.azuredatalakestore.net`<br>`login.microsoftonline.com/<tenant>/oauth2/token`    | 443            | Yalnızca Azure Veri Gölü Deposu'ndan kopya ladiğinizde veya azure veri gölü deposuna kopya ladiğinizde ve isteğe bağlı olarak gereklidir. |
