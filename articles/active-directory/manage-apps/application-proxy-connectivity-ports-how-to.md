---
title: Uygulama proxy uygulaması için gerekli güvenlik duvarı bağlantı noktalarını açma
description: Azure AD Uygulama Ara Sunucusu 'nin düzgün çalışması için hangi bağlantı noktalarının açılacağını öğrenin
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/21/2018
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2b676508c73ff0233526f19b865bb9e4f7b80f12
ms.sourcegitcommit: b1e25a8a442656e98343463aca706f4fde629867
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "74275545"
---
# <a name="how-to-open-the-firewall-ports-required-for-an-application-proxy-application"></a>Uygulama proxy uygulaması için gereken güvenlik duvarı bağlantı noktalarını açma

Gerekli bağlantı noktalarının ve her bağlantı noktasının işlevinin tam listesini görmek için, [uygulama proxy 'si belgelerinin](application-proxy-add-on-premises-application.md)Önkoşullar bölümüne bakın. Uygulama proxy 'Sinin yalnızca giden bağlantı noktalarını kullandığını unutmayın.

Ayrıca, şirket içi ağınızdan [bağlayıcı bağlantı noktaları test aracını](https://aadap-portcheck.connectorporttest.msappproxy.net/) açarak gerekli tüm bağlantı noktalarının açık olup olmadığını kontrol edebilirsiniz. Daha fazla yeşil onay işareti daha fazla esneklik anlamına gelir. 

## <a name="app-proxy-regions"></a>Uygulama proxy bölgeleri

Sizin için bu bölgelerin hangisinin yeşil olması gerektiğini bilmenizi sağlayan bir yol üzerinde çalışıyoruz. Şimdilik tümünün olduklarından emin olun. Orta ABD, hangi bölgeden olduğunuza bakılmaksızın de gereklidir.

Aracın doğru sonuçları verdiğinden emin olmak için aşağıdakileri yaptığınızdan emin olun:

-   Aracı, bağlayıcıyı yüklediğiniz sunucudan bir tarayıcıda açın.

-   Bağlayıcınız için geçerli olan tüm proxy veya güvenlik duvarlarının da bu sayfaya uygulandığından emin olun. Bu, **Ayarlar**  - &gt; **Internet seçenekleri**  - &gt; **Connections** bağlantıları - LAN ayarları ' na giderek Internet Explorer 'da yapılabilir. **LAN Settings**&gt; Bu sayfada, "LAN için bir proxy sunucu kullanın" alanını görürsünüz. Bu kutuyu seçin ve proxy adresini "adres" alanına yerleştirin.

## <a name="next-steps"></a>Sonraki adımlar
[Azure AD Uygulama Ara Sunucusu bağlayıcılarını anlama](application-proxy-connectors.md)
