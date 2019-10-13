---
title: Azure Analysis Services diğer ad sunucu adları | Microsoft Docs
description: Sunucu adı diğer adlarının nasıl oluşturulacağını ve kullanılacağını açıklar.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 161df354bae350df533f3991e551401fd56a4a65
ms.sourcegitcommit: 8b44498b922f7d7d34e4de7189b3ad5a9ba1488b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2019
ms.locfileid: "72301082"
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

@No__t [istemci kitaplıkları](analysis-services-data-providers.md)-1  
[Power BI Desktop Bağlan](analysis-services-connect-pbi.md)
