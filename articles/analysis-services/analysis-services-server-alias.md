---
title: Azure Analysis Services diğer ad sunucu adları | Microsoft Docs
description: Sunucu adı diğer adlarının nasıl oluşturulacağını ve kullanılacağını açıklar.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 10/29/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: ea618ecb29451650cbb01e9c95d263f42d406555
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73146342"
---
# <a name="alias-server-names"></a>Diğer ad sunucu adları

Bir sunucu adı diğer adı kullanarak, kullanıcılar sunucu adı yerine daha kısa bir *diğer adla* Azure Analysis Services sunucunuza bağlanabilir. Bir istemci uygulamasından bağlanırken, diğer ad **link://** protokol biçimi kullanılarak bir uç nokta olarak belirtilir. Bitiş noktası daha sonra bağlanmak için gerçek sunucu adını döndürür.

Diğer ad sunucu adları için uygundur:

- Kullanıcıları etkilemeden sunucular arasında modeller geçirme. 
- Kolay sunucu adları kullanıcıların anımsamasını kolaylaştırır. 
- Kullanıcıları günün farklı saatlerinde farklı sunuculara yönlendirin. 
- Azure Traffic Manager kullanırken olduğu gibi, farklı bölgelerdeki kullanıcıları coğrafi olarak daha yakın örneklere doğrudan yönlendirin. 

Geçerli bir Azure Analysis Services sunucu adı döndüren herhangi bir HTTPS uç noktası, diğer ad olarak görev yapabilir. Uç noktanın 443 bağlantı noktası üzerinden HTTPS desteklemesi gerekir ve bağlantı noktası URI 'de belirtilmemelidir.

![Bağlantı biçimi kullanan diğer ad](media/analysis-services-alias/aas-alias-browser.png)

Bir istemciden bağlanırken, diğer ad sunucu adı **link://** protokol biçimi kullanılarak girilir. Örneğin, Power BI Desktop:

![Power BI Desktop bağlantısı](media/analysis-services-alias/aas-alias-connect-pbid.png)

## <a name="create-an-alias"></a>Diğer ad oluştur

Bir diğer ad uç noktası oluşturmak için, geçerli bir Azure Analysis Services sunucu adı döndüren herhangi bir yöntemi kullanabilirsiniz. Örneğin, Azure Blob depolamada gerçek sunucu adını içeren bir dosyaya başvuru veya bir ASP.NET Web Forms uygulaması oluşturup yayımlayacaksınız.

Bu örnekte, Visual Studio 'da bir ASP.NET Web Forms uygulaması oluşturulur. Ana sayfa başvurusu ve Kullanıcı denetimi varsayılan. aspx sayfasından kaldırılır. Default. aspx ' in içeriği yalnızca aşağıdaki sayfa yönergedir:

```
<%@ Page Title="Home Page" Language="C#" AutoEventWireup="true" CodeBehind="Default.aspx.cs" Inherits="FriendlyRedirect._Default" %>
```

Default.aspx.cs ' deki Page_Load olayı, Azure Analysis Services sunucu adını döndürmek için Response. Write () yöntemini kullanır.

```
protected void Page_Load(object sender, EventArgs e)
{
    this.Response.Write("asazure://<region>.asazure.windows.net/<servername>");
}
```

## <a name="see-also"></a>Ayrıca bkz.

[İstemci kitaplıkları](analysis-services-data-providers.md)   
[Power BI Desktop Bağlan](analysis-services-connect-pbi.md)
