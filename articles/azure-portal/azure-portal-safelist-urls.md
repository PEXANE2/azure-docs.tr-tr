---
title: Güvenlik duvarınızda veya proxy sunucunuzda Azure portalı URL'lerini güvenli listeleyin
description: Azure portalı ve hizmetleriyle iletişim kurmak için bu URL'leri proxy sunucu bypass'ına ekleyin
services: azure-portal
keywords: ''
author: mgblythe
ms.author: mblythe
ms.date: 04/10/2020
ms.topic: conceptual
ms.service: azure-portal
manager: mtillman
ms.openlocfilehash: 399c64c88e78079432fcf7c09dafd199da83358b
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81255058"
---
# <a name="safelist-the-azure-portal-urls-on-your-firewall-or-proxy-server"></a>Güvenlik duvarınızda veya proxy sunucunuzda Azure portalı URL'lerini güvenli listeleyin

Azure portalı URL'leri için güvenlik kısıtlamalarını atlatırmak için şirket içi güvenlik aygıtlarını yapılandırabilirsiniz. Bu yapılandırma, yerel veya geniş alanağınızla Azure bulutu arasındaki performansı ve bağlantıyı artırabilir.

Ağ yöneticileri genellikle proxy sunucuları, güvenlik duvarları veya diğer aygıtları dağıtır. Bu aygıtlar, kullanıcıların internete nasıl erişiciler eve erişmelerini güvence altına alma ve kontrol sağlamaya yardımcı olur. Kullanıcıları korumak için tasarlanmış kurallar bazen işle ilgili yasal internet trafiğini engelleyebilir veya yavaşlatabilir. Bu trafik, siz ve Azure arasındaki iletişimi içerir. Ağınızla Azure portalı ve hizmetleri arasındaki bağlantıyı optimize etmek için, güvenli listenize Azure portal URL'leri eklemenizi öneririz.

## <a name="azure-portal-urls-for-proxy-bypass"></a>Proxy baypas için Azure portal URL'leri

Azure portalı için güvenli liste listesi için URL uç noktaları, kuruluşunuzun dağıtıldığı Azure bulutuna özgüdür. Bu uç noktalardaki ağ trafiğinin kısıtlamaları atlamasına izin vermek için bulutunuzu seçin. Ardından URL'lerin listesini proxy sunucunuza veya güvenlik duvarınıza ekleyin.

#### <a name="public-cloud"></a>[Genel Bulut](#tab/public-cloud)

```
*.aadcdn.microsoftonline-p.com
*.aka.ms
*.applicationinsights.io
*.azure.com
*.azure.net
*.azureafd.net
*.azure-api.net
*.azuredatalakestore.net
*.azureedge.net
*.loganalytics.io
*.microsoft.com
*.microsoftonline.com
*.microsoftonline-p.com
*.msauth.net
*.msftauth.net
*.trafficmanager.net
*.visualstudio.com
*.windows.net
*.windows-int.net
```

#### <a name="us-government-cloud"></a>[ABD Hükümeti Bulut](#tab/us-government-cloud)

```
*.azure.us
*.loganalytics.us
*.microsoft.us
*.microsoftonline.us
*.msauth.net
*.usgovcloudapi.net
*.usgovtrafficmanager.net
*.windowsazure.us
```

#### <a name="china-government-cloud"></a>[Çin Hükümeti Bulut](#tab/china-government-cloud)

```
*.azure.cn
*.microsoft.cn
*.microsoftonline.cn
*.chinacloudapi.cn
*.trafficmanager.cn
*.chinacloudsites.cn
*.windowsazure.cn
```
---

> [!NOTE]
> Bu uç noktalara trafik, HTTP (80) ve HTTPS (443) için standart TCP bağlantı noktalarını kullanır.
>
>
