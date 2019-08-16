---
title: ASP.NET projeme ne oldu? | Microsoft Docs
description: Visual Studio bağlı hizmetleri kullanarak bir ASP.NET projesine Azure Storage eklendikten sonra ne olacağını açıklar.
services: storage
author: ghogen
manager: jillfra
ms.assetid: e1fe1b6d-4e3d-476d-8b2f-f7ade050515e
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ms.openlocfilehash: 74db2e1e15c282f4393715c67561b2ed61bbcdfb
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69512075"
---
# <a name="what-happened-to-my-aspnet-project-visual-studio-azure-storage-connected-service"></a>ASP.NET projem 'e ne oldu (Visual Studio Azure Storage bağlı hizmeti)?
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
Projenizin Web. config dosyasında, seçili depolama hesabının bağlantı dizesi ve anahtarıyla bir öğe oluşturuldu.

Daha fazla bilgi için bkz. [ASP.net](https://www.asp.net).

