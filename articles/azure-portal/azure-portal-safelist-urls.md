---
title: Güvenlik duvarınızdaki veya proxy sunucunuzdaki Azure portal URL 'Lerini listeleyin
description: Azure portal ve hizmetleriyle iletişim kurmak için bu URL 'Leri ara sunucu geçişine ekleyin
services: azure-portal
keywords: ''
author: mgblythe
ms.author: mblythe
ms.date: 04/10/2020
ms.topic: conceptual
ms.service: azure-portal
manager: mtillman
ms.openlocfilehash: 399c64c88e78079432fcf7c09dafd199da83358b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81255058"
---
# <a name="safelist-the-azure-portal-urls-on-your-firewall-or-proxy-server"></a>Güvenlik duvarınızdaki veya proxy sunucunuzdaki Azure portal URL 'Lerini listeleyin

Şirket içi güvenlik cihazlarını, Azure portal URL 'Lerinin güvenlik kısıtlamalarını atlayacak şekilde yapılandırabilirsiniz. Bu yapılandırma, yerel veya geniş alan ağınız ile Azure bulutu arasındaki performansı ve bağlantıyı iyileştirebilir.

Ağ yöneticileri genellikle proxy sunucuları, güvenlik duvarları veya diğer cihazları dağıtır. Bu cihazlar güvenli hale getirmeye yardımcı olur ve kullanıcıların İnternet 'e nasıl erişkullanılacağına yönelik denetim sağlar. Kullanıcıları korumak için tasarlanan kurallar, işle ilgili yasal internet trafiğini bazen engelleyebilir veya yavaşlatır. Bu trafik, siz ve Azure arasındaki iletişimleri içerir. Ağınızla Azure portal ve hizmetleri arasındaki bağlantıyı iyileştirmek için, SafeList verilerinize Azure portal URL eklemenizi öneririz.

## <a name="azure-portal-urls-for-proxy-bypass"></a>Proxy atlama için Azure portal URL 'Leri

Azure portal için güvenli listeye yönelik URL uç noktaları, kuruluşunuzun dağıtıldığı Azure bulutuna özgüdür. Bu uç noktalara yönelik ağ trafiğine kısıtlama atlama izni vermek için bulutunuzu seçin. Ardından, proxy sunucunuza veya güvenlik duvarına URL listesi ekleyin.

#### <a name="public-cloud"></a>[Genel bulut](#tab/public-cloud)

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

#### <a name="us-government-cloud"></a>[ABD kamu bulutu](#tab/us-government-cloud)

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

#### <a name="china-government-cloud"></a>[Çin Devlet bulutu](#tab/china-government-cloud)

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
> Bu uç noktalara giden trafik, HTTP (80) ve HTTPS (443) için standart TCP bağlantı noktalarını kullanır.
>
>
