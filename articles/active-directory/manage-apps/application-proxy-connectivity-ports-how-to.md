---
title: Uygulama Proxy uygulaması için gerekli güvenlik duvarı bağlantı noktaları nasıl açılır?
description: Azure AD Uygulama Proxy'sinin düzgün çalışması için hangi bağlantı noktalarını açacağımı öğrenin
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74275545"
---
# <a name="how-to-open-the-firewall-ports-required-for-an-application-proxy-application"></a>Uygulama Proxy uygulaması için gerekli güvenlik duvarı bağlantı noktaları nasıl açılır?

Gerekli bağlantı noktalarının tam listesini ve her bağlantı noktasının işlevini görmek [için, Uygulama Proxy belgelerinin](application-proxy-add-on-premises-application.md)ön koşullar bölümüne bakın. Uygulama Proxy'nin yalnızca giden bağlantı noktalarını kullandığını unutmayın.

[Konektör Bağlantı Noktaları Test Aracı'nı](https://aadap-portcheck.connectorporttest.msappproxy.net/) şirket içi ağınızdan açarak gerekli tüm bağlantı noktalarını açıp açmadığınızı da kontrol edebilirsiniz. Daha fazla yeşil onay işareti daha fazla esneklik anlamına gelir. 

## <a name="app-proxy-regions"></a>App Proxy bölgeleri

Bu bölgelerden hangisinin sizin için yeşil olması gerektiğini size bildirmek için bir yol üzerinde çalışıyoruz. Şimdilik, hepsinin emin ol. Merkezi ABD'de hangi bölgede olduğunuza bakılmaksızın da gereklidir.

Aracın size doğru sonuçları verdiğinden emin olmak için aşağıdakilerden emin olun:

-   Aracı Bağlayıcıyı yüklediğiniz sunucudan bir tarayıcıda açın.

-   Bağlayıcınız için geçerli olan ek vekillerin veya güvenlik duvarlarının da bu sayfaya uygulandığından emin olun. Bu **Ayarlar**  - &gt; **Internet Seçenekleri**  - &gt; **Bağlantıları**  - &gt; LAN **Ayarları**giderek Internet Explorer yapılabilir. Bu sayfada, "LAN'ınız için Proxy Sunucusu kullanın" alanını görürsünüz. Bu kutuyu seçin ve proxy adresini "Adres" alanına koyun.

## <a name="next-steps"></a>Sonraki adımlar
[Azure AD Uygulama Proxy bağlayıcılarını anlama](application-proxy-connectors.md)
