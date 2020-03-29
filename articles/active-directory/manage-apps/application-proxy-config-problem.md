---
title: Uygulama Proxy uygulaması oluşturma sorunu | Microsoft Dokümanlar
description: Azure AD Yöneticisi portalında Uygulama Proxy uygulamaları oluşturma sorunları giderme
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
ms.openlocfilehash: 104b98cba0948ec5d0896877e54eab1e7cd4049f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "65825812"
---
# <a name="problem-creating-an-application-proxy-application"></a>Uygulama Proxy uygulaması oluşturma sorunu 

Aşağıda, yeni bir uygulama proxy uygulaması oluştururken insanların karşılaştığı ortak sorunlardan bazıları verilmiştir.

## <a name="recommended-documents"></a>Önerilen belgeler 

Yönetici Portalı üzerinden Uygulama Proxy uygulaması oluşturma hakkında daha fazla bilgi edinmek için Azure [AD Application Proxy kullanarak uygulama yayımla'ya](application-proxy-add-on-premises-application.md)bakın.

Bu belgedeki adımları takip ediyorsanız ve uygulamayı oluştururken bir hata alıyorsanız, uygulamayı nasıl düzelteceğimiz le ilgili bilgi ve öneriler için hata ayrıntılarına bakın. Çoğu hata iletisi önerilen düzeltmeyi içerir. 

## <a name="specific-things-to-check"></a>Kontrol etmek için belirli şeyler

Sık karşılaşılan hataları önlemek için şu leri doğrulayın:

-   Bir Uygulama Proxy uygulaması oluşturmak için izni olan bir yönetici vardır

-   Dahili URL benzersizdir

-   Harici URL benzersizdir

-   URL'ler http veya https ile başlar ve "/" ile sona erer

-   URL bir etki alanı adı değil, bir IP adresi olmalıdır

Uygulamayı oluştururken hata iletisi sağ üst köşede görüntülenmelidir. Hata iletilerini görmek için bildirim simgesini de seçebilirsiniz.

   ![Bildirim istemi](./media/application-proxy-config-problem/error-message.png)

## <a name="next-steps"></a>Sonraki adımlar
[Azure portalında Uygulama Ara Sunucusunu etkinleştirme](application-proxy-add-on-premises-application.md)
