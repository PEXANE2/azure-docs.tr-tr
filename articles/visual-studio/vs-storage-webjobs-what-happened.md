---
title: WebJob projeme (Visual Studio Azure Depolama) ne oldu?
description: Visual Studio'ya bağlı hizmetleri kullanarak bir depolama hesabına bağlandıktan sonra bir Azure Webİş projesinde neler olduğunu açıklar
services: storage
author: ghogen
manager: jillfra
ms.assetid: 36ae7ff7-c22c-47eb-b220-049d61618c74
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 17861b7c25dfaf9bc9399e5261cdf2a5b43caf21
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72298736"
---
# <a name="what-happened-to-my-webjob-project-visual-studio-azure-storage-connected-service"></a>WebJob projeme (Visual Studio Azure Depolama bağlantılı hizmet) ne oldu?
## <a name="references-added"></a>Eklenen Referanslar
Azure Depolama NuGet paketi Visual Studio projenize eklendi veya güncelleştirildi.  
Bu paket aşağıdaki .NET referansları ekler:

* **Microsoft.Data.Edm**
* **Microsoft.Data.OData**
* **Microsoft.Data.Services.Client**
* **Microsoft.WindowsAzure.ConfigurationManager**
* **Microsoft.WindowsAzure.Storage**
* **Newtonsoft.Json**
* **System.Data**
* **System.Spatial**

## <a name="connection-string-for-azure-storage-added"></a>Azure Depolama için bağlantı dizesi eklendi
Projenizin App.config dosyasında, **AzureWebJobsStorage** ve **AzureWebJobsDashboard** girişleri seçili depolama hesabının bağlantı dizesi ve anahtarıyla güncelleştirildi.

Daha fazla bilgi için Azure [Web İşleri dokümantasyon kaynaklarına](https://go.microsoft.com/fwlink/?linkid=390226)bakın.

