---
title: ASP.NET 5 projem 'e ne oldu (Visual Studio bağlı hizmetler) | Microsoft Docs
description: Visual Studio ASP.NET 5 projesinde, Visual Studio bağlı hizmetleri kullanılarak bir Azure depolama hesabına bağlandıktan sonra ne olacağı açıklanmaktadır
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
ms.openlocfilehash: e29c0302ecd703cb02199df95892e24917baf8e8
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69510766"
---
# <a name="what-happened-to-my-aspnet-5-project-visual-studio-azure-storage-connected-services"></a>ASP.NET 5 projem 'e ne oldu (Visual Studio Azure Storage bağlı Hizmetleri)?
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

Ayrıca, **Microsoft. Framework. Configuration. JSON** adlı NuGet paketi eklenmiştir.

## <a name="connection-string-for-azure-storage-added"></a>Azure depolama için bağlantı dizesi eklendi
Projenizin config. json dosyasında, seçili depolama hesabının bağlantı dizesi ve anahtarıyla bir öğe oluşturuldu.

Daha fazla bilgi için bkz. [ASP.NET 5](https://www.asp.net/vnext).

