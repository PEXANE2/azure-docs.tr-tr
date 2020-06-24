---
title: Uygulama proxy uygulaması için gerekli güvenlik duvarı bağlantı noktalarını açma
description: Azure AD Uygulama Ara Sunucusu 'nin düzgün çalışması için hangi bağlantı noktalarının açılacağını öğrenin
services: active-directory
documentationcenter: ''
author: kenwith
manager: celestedg
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 05/21/2018
ms.author: kenwith
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 192e721e25612019649fed86b44135fb6bfef89c
ms.sourcegitcommit: bc943dc048d9ab98caf4706b022eb5c6421ec459
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/14/2020
ms.locfileid: "84764715"
---
# <a name="how-to-open-the-firewall-ports-required-for-an-application-proxy-application"></a>Uygulama proxy uygulaması için gereken güvenlik duvarı bağlantı noktalarını açma

Gerekli bağlantı noktalarının ve her bağlantı noktasının işlevinin tam listesini görmek için, [uygulama proxy 'si belgelerinin](application-proxy-add-on-premises-application.md)Önkoşullar bölümüne bakın. Uygulama proxy 'Sinin yalnızca giden bağlantı noktalarını kullandığını unutmayın.

Ayrıca, şirket içi ağınızdan [bağlayıcı bağlantı noktaları test aracını](https://aadap-portcheck.connectorporttest.msappproxy.net/) açarak gerekli tüm bağlantı noktalarının açık olup olmadığını kontrol edebilirsiniz. Daha fazla yeşil onay işareti daha fazla esneklik anlamına gelir. 

## <a name="app-proxy-regions"></a>Uygulama proxy bölgeleri

Sizin için bu bölgelerin hangisinin yeşil olması gerektiğini bilmenizi sağlayan bir yol üzerinde çalışıyoruz. Şimdilik tümünün olduklarından emin olun. Orta ABD, hangi bölgeden olduğunuza bakılmaksızın de gereklidir.

Aracın doğru sonuçları verdiğinden emin olmak için aşağıdakileri yaptığınızdan emin olun:

-   Aracı, bağlayıcıyı yüklediğiniz sunucudan bir tarayıcıda açın.

-   Bağlayıcınız için geçerli olan tüm proxy veya güvenlik duvarlarının da bu sayfaya uygulandığından emin olun. Bu, **Ayarlar**  - &gt; **Internet seçenekleri**  - &gt; **bağlantıları**  - &gt; **LAN ayarları**' na giderek Internet Explorer 'da yapılabilir. Bu sayfada, "LAN için bir proxy sunucu kullanın" alanını görürsünüz. Bu kutuyu seçin ve proxy adresini "adres" alanına yerleştirin.

## <a name="next-steps"></a>Sonraki adımlar
[Azure AD Uygulama Ara Sunucusu bağlayıcılarını anlama](application-proxy-connectors.md)
