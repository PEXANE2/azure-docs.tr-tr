---
title: Güvenlik duvarınızda veya proxy sunucunuzda Azure portalı URL'lerini güvenli listeleyin
description: Azure portalı ve hizmetleriyle iletişim kurmak için bu URL'leri proxy sunucu bypass'ına ekleyin
services: azure-portal
keywords: ''
author: mgblythe
ms.author: mblythe
ms.date: 01/29/2020
ms.topic: conceptual
ms.service: azure-portal
manager: mtillman
ms.openlocfilehash: c5bba7296a05cfbb72698a991ece1ef298689bd1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76900663"
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
## <a name="next-steps"></a>Sonraki adımlar

IP adreslerini güvenli bir şekilde listelememi mi gerekiyor? Bulutunuzun Microsoft Azure veri merkezi IP aralıklarının listesini indirin:

* [Dünya](https://www.microsoft.com/download/details.aspx?id=56519)
* [ABD Hükümeti](https://www.microsoft.com/download/details.aspx?id=57063)
* [Almanya](https://www.microsoft.com/download/details.aspx?id=57064)
* [Çin](https://www.microsoft.com/download/details.aspx?id=57062)

Diğer Microsoft hizmetleri bağlantı için ek URL'ler ve IP adresleri kullanır. Microsoft 365 hizmetleri için ağ bağlantısını optimize etmek [için](/office365/enterprise/set-up-network-for-office-365)bkz.
