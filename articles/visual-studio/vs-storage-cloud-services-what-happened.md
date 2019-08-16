---
title: Bulut hizmeti projem 'e ne oldu? | Microsoft Docs
description: Visual Studio bağlı hizmetleri kullanarak bir Azure depolama hesabına bağlandıktan sonra bir bulut hizmetleri projesinde ne olacağını açıklar.
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
ms.openlocfilehash: d96e523b6e4e060eaef33de0f961e7020e5dfb57
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69510643"
---
# <a name="what-happened-to-my-cloud-services-project-visual-studio-azure-storage-connected-service"></a>Bulut hizmetleri projem 'e ne oldu (Visual Studio Azure Storage bağlı hizmeti)?
## <a name="references-added"></a>Başvurular eklendi
Azure depolama NuGet paketi Visual Studio projenize eklendi.  
Bu paket, aşağıdaki .NET başvurularını ekler:

* **Microsoft.Data.Edm**
* **Microsoft.Data.OData**
* **Microsoft.Data.Services.Client**
* **Microsoft.WindowsAzure.Configuration**
* **Microsoft. WindowsAzure. Storage**
* **Newtonsoft.Json**
* **System. Data**
* **System. uzamsal**

## <a name="connection-string-for-azure-storage-added"></a>Azure depolama için bağlantı dizesi eklendi
Öğeler seçili depolama hesabının bağlantı dizesi ve anahtarıyla oluşturuldu. Aşağıdaki dosyalarda değişiklikler yapılmıştır:

* **ServiceDefinition.csdef**
* **ServiceConfiguration.Cloud.cscfg**
* **ServiceConfiguration.Local.cscfg**

