---
title: Azure Analiz Hizmetleri takma sunucu adları | Microsoft Dokümanlar
description: Azure Çözümleme Hizmetleri sunucu adı takma adlarını nasıl oluşturabileceğinizi öğrenin. Kullanıcılar daha sonra sunucu adı yerine daha kısa bir takma adla sunucunuza bağlanabilir.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 10/29/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 5e7017fad90e32cb8c4b952987fe248e463e4d03
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73572287"
---
# <a name="alias-server-names"></a>Diğer ad sunucu adları

Kullanıcılar, sunucu adı takma adını kullanarak Azure Çözümleme Hizmetleri sunucunuza sunucu adı yerine daha kısa bir *takma adla* bağlanabilir. İstemci uygulamasından bağlanırken, diğer ad **link://** iletişim kuralı biçimini kullanarak bitiş noktası olarak belirtilir. Bitiş noktası daha sonra bağlanmak için gerçek sunucu adını döndürür.

Diğer adsunucu adları aşağıdakiler için iyidir:

- Kullanıcıları etkilemeden modelleri sunucular arasında geçirme. 
- Dostu sunucu adlarının kullanıcıların hatırlaması daha kolaydır. 
- Kullanıcıları günün farklı saatlerinde farklı sunuculara yönlendirin. 
- Farklı bölgelerdeki kullanıcıları Azure Trafik Yöneticisi'ni kullanırken olduğu gibi coğrafi olarak daha yakın olan örneklere yönlendirin. 

Geçerli bir Azure Çözümleme Hizmetleri sunucu adını döndüren herhangi bir HTTPS bitiş noktası takma ad olarak hizmet verebilir. Bitiş noktası 443 bağlantı noktası üzerinden HTTPS'yi desteklemeli ve bağlantı noktası URI'de belirtilmemelidir.

![Bağlantı biçimini kullanarak diğer ad](media/analysis-services-alias/aas-alias-browser.png)

İstemciden bağlanırken, diğer ad sunucu adı **link://** iletişim kuralı biçimi kullanılarak girilir. Örneğin, Power BI Desktop'da:

![Power BI Masaüstü bağlantısı](media/analysis-services-alias/aas-alias-connect-pbid.png)

## <a name="create-an-alias"></a>Takma ad oluşturma

Takma ad bitiş noktası oluşturmak için, geçerli bir Azure Çözümleme Hizmetleri sunucu adını döndüren tüm yöntemi kullanabilirsiniz. Örneğin, Azure Blob Depolama'da gerçek sunucu adını içeren bir dosyaya başvuru veya ASP.NET bir Web Forms uygulaması oluşturup yayımlayın.

Bu örnekte, Visual Studio'da ASP.NET bir Web Formları Uygulaması oluşturulur. Ana sayfa başvurusu ve kullanıcı denetimi Varsayılan.aspx sayfasından kaldırılır. Default.aspx içeriği basitçe aşağıdaki Sayfa yönergesi vardır:

```
<%@ Page Title="Home Page" Language="C#" AutoEventWireup="true" CodeBehind="Default.aspx.cs" Inherits="FriendlyRedirect._Default" %>
```

Default.aspx.cs'daki Page_Load olayı, Azure Çözümleme Hizmetleri sunucu adını döndürmek için Yanıt.Write() yöntemini kullanır.

```
protected void Page_Load(object sender, EventArgs e)
{
    this.Response.Write("asazure://<region>.asazure.windows.net/<servername>");
}
```

## <a name="see-also"></a>Ayrıca bkz.

[İstemci kitaplıkları](analysis-services-data-providers.md)   
[Power BI Desktop'tan bağlanma](analysis-services-connect-pbi.md)
