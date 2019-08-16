---
title: WebJob projem için ne oldu (Visual Studio Azure Storage bağlı hizmeti)? | Microsoft Docs
description: Visual Studio bağlı hizmetleri kullanılarak bir depolama hesabına bağlandıktan sonra bir Azure WebJob projesinde ne olduğunu açıklar
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
ms.openlocfilehash: c54214e0b919ddaa60403a1c986a144100143577
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69510466"
---
# <a name="what-happened-to-my-webjob-project-visual-studio-azure-storage-connected-service"></a>WebJob projem için ne oldu (Visual Studio Azure Storage bağlı hizmeti)?
## <a name="references-added"></a>Başvurular eklendi
Azure depolama NuGet paketi Visual Studio projenize eklendi veya güncelleştirildi.  
Bu paket, aşağıdaki .NET başvurularını ekler:

* **Microsoft.Data.Edm**
* **Microsoft.Data.OData**
* **Microsoft.Data.Services.Client**
* **Microsoft.WindowsAzure.ConfigurationManager**
* **Microsoft. WindowsAzure. Storage**
* **Newtonsoft.Json**
* **System. Data**
* **System. uzamsal**

## <a name="connection-string-for-azure-storage-added"></a>Azure depolama için bağlantı dizesi eklendi
Projenizin App. config dosyasında, **AzureWebJobsStorage** ve **AzureWebJobsDashboard** girdileri seçili depolama hesabının bağlantı dizesi ve anahtarıyla güncelleştirildi.

Daha fazla bilgi için bkz. [Azure WebJobs belge kaynakları](https://go.microsoft.com/fwlink/?linkid=390226).

