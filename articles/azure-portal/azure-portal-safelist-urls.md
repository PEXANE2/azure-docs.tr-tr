---
title: Azure portal URL 'Lerini güvenli listeleyin | Microsoft Docs
description: Azure portal ve hizmetleriyle iletişim kurmak için bu URL 'Leri ara sunucu geçişine ekleyin
services: azure-portal
keywords: ''
author: kfollis
ms.author: kfollis
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-portal
manager: mtillman
ms.openlocfilehash: 3747ca7504e1a8a6bbeb6237c1b3cb2e5e4afb5b
ms.sourcegitcommit: fecb6bae3f29633c222f0b2680475f8f7d7a8885
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68667481"
---
# <a name="safelist-the-azure-portal-urls-on-your-firewall-or-proxy-server"></a>Güvenlik duvarınızdaki veya proxy sunucunuzdaki Azure portal URL 'Lerini listeleyin

Yerel veya geniş alan ağınız ile Azure bulutu arasında iyi performans ve bağlantı sağlamak için şirket içi güvenlik cihazlarını, Azure portal URL 'Lerinin güvenlik kısıtlamalarını atlayacak şekilde yapılandırın. Ağ yöneticileri, kullanıcıların İnternet 'e nasıl erişebileceği konusunda güvenli hale getirmek ve denetim sağlamak için genellikle proxy sunucuları, güvenlik duvarları veya diğer cihazları dağıtır. Ancak, kullanıcıları korumak için tasarlanan kurallar, siz ve Azure arasındaki iletişimler de dahil olmak üzere, işletmeyle ilgili yasal internet trafiğini engelleyebilir veya yavaşlatır. Ağınızla Azure portal ve hizmetleri arasındaki bağlantıyı iyileştirmek için, SafeList verilerinize Azure portal URL eklemenizi öneririz.

## <a name="azure-portal-urls-for-proxy-bypass"></a>Proxy atlama için Azure portal URL 'Leri

Kısıtlamaları atlamak için bu uç noktalara yönelik ağ trafiğine izin vermek üzere proxy sunucunuza veya güvenlik duvarına aşağıdaki URL listesini ekleyin:

* *.aadcdn.microsoftonline-p.com
* *.aimon.applicationinsights.io
* *.azure.com
* *.azuredatalakestore.net
* *.azureedge.net
* *.exp.azure.com
* *.ext.azure.com
* *.gfx.ms
* *.account.microsoft.com
* *.hosting.portal.azure.net
* *.marketplaceapi.microsoft.com
* *.microsoftonline.com
* *.msauth.net
* *.msftauth.net
* *.portal.azure.com
* *.portalext.visualstudio.com
* *.sts.microsoft.com
* *.vortex.data.microsoft.com
* *.vscommerce.visualstudio.com
* *.vssps.visualstudio.com
* *.windows.net
* *.wpc.azureedge.net

> [!NOTE]
> Bu uç noktalara giden trafik, HTTP (80) ve HTTPS (443) için standart TCP bağlantı noktalarını kullanır.
>
>
## <a name="next-steps"></a>Sonraki adımlar

* IP adreslerini güvenli listeye almanız mı gerekiyor? [Microsoft Azure veri MERKEZI IP aralıklarının](https://www.microsoft.com/download/details.aspx?id=41653)listesini indirin.
* Diğer Microsoft Hizmetleri, bağlantı için ek URL 'Ler ve IP adresleri kullanır. Microsoft 365 hizmetleri için ağ bağlantısını iyileştirmek için bkz. [ağınızı Office 365 Için ayarlama](/office365/enterprise/set-up-network-for-office-365).
