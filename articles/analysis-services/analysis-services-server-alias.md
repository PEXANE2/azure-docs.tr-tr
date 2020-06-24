---
title: Azure Analysis Services diğer ad sunucu adları | Microsoft Docs
description: Azure Analysis Services sunucu adı diğer adları oluşturmayı öğrenin. Kullanıcılar daha sonra sunucu adı yerine daha kısa bir diğer ad ile sunucunuza bağlanabilir.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 06/16/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 42055762c9c0a642b3efd05af841d70f3f91413c
ms.sourcegitcommit: 1383842d1ea4044e1e90bd3ca8a7dc9f1b439a54
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/16/2020
ms.locfileid: "84816877"
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

Bu örnekte, Visual Studio 'da bir ASP.NET Web Forms uygulaması oluşturulur. Sayfa başvurusu ve Kullanıcı denetimi varsayılan. aspx sayfasından kaldırılır. Default. aspx ' in içeriği yalnızca aşağıdaki sayfa yönergedir:

```
<%@ Page Title="Home Page" Language="C#" AutoEventWireup="true" CodeBehind="Default.aspx.cs" Inherits="FriendlyRedirect._Default" %>
```

Default.aspx.cs ' deki Page_Load olayı Azure Analysis Services sunucu adını döndürmek için Response. Write () yöntemini kullanır.

```
protected void Page_Load(object sender, EventArgs e)
{
    this.Response.Write("asazure://<region>.asazure.windows.net/<servername>");
}
```

## <a name="see-also"></a>Ayrıca bkz.

[İstemci kitaplıkları](analysis-services-data-providers.md)   
[Power BI Desktop'tan bağlanma](analysis-services-connect-pbi.md)
