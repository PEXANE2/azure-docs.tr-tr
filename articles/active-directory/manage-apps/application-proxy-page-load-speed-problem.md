---
title: Bir Uygulama Proxy uygulaması yüklemek için çok uzun sürer | Microsoft Dokümanlar
description: Azure AD Uygulama Proxy ile sayfa yükleme performansı sorunlarını giderme
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
ms.date: 07/11/2017
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4ac1182d719d7c90129115e1fadf94f4f86a28e8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "65782640"
---
# <a name="an-application-proxy-application-takes-too-long-to-load"></a>Bir Uygulama Proxy uygulaması yüklemek için çok uzun sürer

Bu makale, Bir Azure AD Uygulama Proxy uygulamasının neden yüklenmesinin uzun sürebileceğini anlamanıza yardımcı olur. Ayrıca, bu sorunu çözmek için neler yapabileceğinizi de açıklar.

## <a name="overview"></a>Genel Bakış
Uygulamalarınız çalışıyor olsa da, uzun bir gecikme süresi yaşayabilirler. Hızı artırmak için yapabileceğiniz ağ topoloji tweaks olabilir. Farklı topolojilerin değerlendirilmesi [için, ağ hususları belgesine](application-proxy-network-topology.md)bakın.

Ağ topolojisi yanı sıra, şu anda performans atonlama için başka öneriler vardır. Uygulama Proxy hizmeti genişledikçe fiziksel olarak daha yakın bir veri merkezine gelebilir. Yakın yakınlık gecikme ye yardımcı olabilir. Azure veri merkezlerinin listesi için [gecikme testi sayfasına](http://www.azurespeed.com/Azure/Latency)bakın. 

Application Proxy hizmeti ile veri merkezleri [Bağlayıcı Bağlantı Noktaları Test Aracı](https://aadap-portcheck.connectorporttest.msappproxy.net/)ile bulunabilir. 

## <a name="feedback-on-application-proxy-data-center-locations"></a>Uygulama Proxy veri merkezi konumları hakkında geri bildirim 
Henüz Uygulama Proxy'sini içermeyen, ancak sizin için harika bir gecikme süresi iyileştirmesine yol açacak Azure veri merkezleri olabilir. Veri merkezi konumunu aadapfeedback@microsoft.com' ya gönder. Microsoft, geri bildiriminizi genişletme planları için kullanır.

Microsoft gecikmeyi artırmak için ek özellikler üzerinde çalışıyor. Bu geliştirmeler kullanılabilir olur olmaz, belgeler güncelleştirilir.

## <a name="next-steps"></a>Sonraki adımlar
[Varolan şirket içi proxy sunucularıyla çalışma](application-proxy-configure-connectors-with-proxy-servers.md)
