---
title: Bulut hizmeti projeme ne oldu?
description: Visual Studio'ya bağlı hizmetleri kullanarak bir Azure depolama hesabına bağlandıktan sonra bulut hizmetleri projesinde neler olduğunu açıklar
services: storage
author: ghogen
manager: jillfra
ms.assetid: ca0ea68d-f417-4ce8-9413-40d76f69cdea
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: ac5a635b687e51ddd34899717497548296c2fc23
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72298770"
---
# <a name="what-happened-to-my-cloud-services-project-visual-studio-azure-storage-connected-service"></a>Bulut hizmetleri projeme ne oldu (Visual Studio Azure Depolama’ya bağlı hizmet)?
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

## <a name="connection-string-for-azure-storage-added"></a>Azure Depolama için bağlantı dizesi eklendi
Öğeler, seçili depolama hesabının bağlantı dizesi ve anahtarıyla oluşturulmuştur. Aşağıdaki dosyalarda değişiklikler yapıldı:

* **ServiceDefinition.csdef**
* **ServiceConfiguration.Cloud.cscfg**
* **ServiceConfiguration.Local.cscfg**

