---
title: ASP.NET 5 projeme ne oldu (Visual Studio bağlantılı hizmetler)
description: Visual Studio'ya bağlı hizmetleri kullanarak Visual Studio ASP.NET 5 projesinde bir Azure depolama hesabına bağlandıktan sonra neler olduğunu açıklar
services: storage
author: ghogen
manager: jillfra
ms.assetid: e7caa9fa-c780-45eb-a546-299fc1c68455
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: be99465a48aaf680834f313e03384a9f0c211502
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72300007"
---
# <a name="what-happened-to-my-aspnet-5-project-visual-studio-azure-storage-connected-services"></a>ASP.NET 5 projeme (Visual Studio Azure Depolama bağlantılı hizmetler) ne oldu?
## <a name="references-added"></a>Eklenen referanslar
Azure Depolama NuGet paketi Visual Studio projenize eklendi.  
Bu paket aşağıdaki .NET referansları ekler:

* **Microsoft.Data.Edm**
* **Microsoft.Data.OData**
* **Microsoft.Data.Services.Client**
* **Microsoft.WindowsAzure.Configuration**
* **Microsoft.WindowsAzure.Storage**
* **Newtonsoft.Json**
* **System.Data**
* **System.Spatial**

Ayrıca, NuGet paketi **Microsoft.Framework.Configuration.Json** eklendi.

## <a name="connection-string-for-azure-storage-added"></a>Azure Depolama için bağlantı dizesi eklendi
Projenizin config.json dosyasında, seçili depolama hesabının bağlantı dizesi ve anahtarıyla bir öğe oluşturuldu.

Daha fazla bilgi için bkz: [ASP.NET 5.](https://www.asp.net/vnext)

