---
title: Güvenlik duvarınızdaki veya proxy sunucunuzdaki Azure portal URL 'Lerini listeleyin
description: Azure portal ve hizmetleriyle iletişim kurmak için bu URL 'Leri ara sunucu geçişine ekleyin
services: azure-portal
keywords: ''
author: mgblythe
ms.author: mblythe
ms.date: 01/29/2020
ms.topic: conceptual
ms.service: azure-portal
manager: mtillman
ms.openlocfilehash: c5bba7296a05cfbb72698a991ece1ef298689bd1
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76900663"
---
# <a name="safelist-the-azure-portal-urls-on-your-firewall-or-proxy-server"></a>Güvenlik duvarınızdaki veya proxy sunucunuzdaki Azure portal URL 'Lerini listeleyin

Şirket içi güvenlik cihazlarını, Azure portal URL 'Lerinin güvenlik kısıtlamalarını atlayacak şekilde yapılandırabilirsiniz. Bu yapılandırma, yerel veya geniş alan ağınız ile Azure bulutu arasındaki performansı ve bağlantıyı iyileştirebilir.

Ağ yöneticileri genellikle proxy sunucuları, güvenlik duvarları veya diğer cihazları dağıtır. Bu cihazlar güvenli hale getirmeye yardımcı olur ve kullanıcıların İnternet 'e nasıl erişkullanılacağına yönelik denetim sağlar. Kullanıcıları korumak için tasarlanan kurallar, işle ilgili yasal internet trafiğini bazen engelleyebilir veya yavaşlatır. Bu trafik, siz ve Azure arasındaki iletişimleri içerir. Ağınızla Azure portal ve hizmetleri arasındaki bağlantıyı iyileştirmek için, SafeList verilerinize Azure portal URL eklemenizi öneririz.

## <a name="azure-portal-urls-for-proxy-bypass"></a>Proxy atlama için Azure portal URL 'Leri

Azure portal için güvenli listeye yönelik URL uç noktaları, kuruluşunuzun dağıtıldığı Azure bulutuna özgüdür. Bu uç noktalara yönelik ağ trafiğine kısıtlama atlama izni vermek için bulutunuzu seçin. Ardından, proxy sunucunuza veya güvenlik duvarına URL listesi ekleyin.

#### <a name="public-cloudtabpublic-cloud"></a>[Genel bulut](#tab/public-cloud)

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

#### <a name="us-government-cloudtabus-government-cloud"></a>[ABD kamu bulutu](#tab/us-government-cloud)

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

#### <a name="china-government-cloudtabchina-government-cloud"></a>[Çin Devlet bulutu](#tab/china-government-cloud)

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
## <a name="next-steps"></a>Sonraki adımlar

IP adreslerini güvenli listeye almanız mı gerekiyor? Bulutunuz için Microsoft Azure veri merkezi IP aralıklarının listesini indirin:

* [Dünya çapında](https://www.microsoft.com/download/details.aspx?id=56519)
* [ABD devleti](https://www.microsoft.com/download/details.aspx?id=57063)
* [Almanya](https://www.microsoft.com/download/details.aspx?id=57064)
* [Çin](https://www.microsoft.com/download/details.aspx?id=57062)

Diğer Microsoft Hizmetleri, bağlantı için ek URL 'Ler ve IP adresleri kullanır. Microsoft 365 hizmetleri için ağ bağlantısını iyileştirmek için bkz. [ağınızı Office 365 Için ayarlama](/office365/enterprise/set-up-network-for-office-365).
