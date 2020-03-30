---
title: Uygulama Proxy uygulaması için çalışan bağlayıcı grubu bulunamadı
description: Azure AD Uygulama Proxy'si ile uygulamanız için Bağlayıcı Grubunda çalışan bir Bağlayıcı olmadığında karşılaşabileceğiniz sorunları giderme
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
ms.openlocfilehash: 96ab0d1b3bbab9c97c04da4f918f3aaa2f1d07e4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74275612"
---
# <a name="no-working-connector-group-found-for-an-application-proxy-application"></a>Uygulama Proxy uygulaması için çalışan bağlayıcı grubu bulunamadı

Bu makale, Azure Etkin Dizini ile tümleşik bir Uygulama Proxy uygulaması için algılanan bir bağlayıcı olmadığında karşılaşılan sık karşılaşılan sorunların giderilmesine yardımcı olur.

## <a name="overview-of-steps"></a>Adımlara genel bakış
Uygulamanız için bağlayıcı grubunda çalışan bir Bağlayıcı yoksa, sorunu çözmenin birkaç yolu vardır:

-   Grupta bağlayıcınız yoksa şunları yapabilirsiniz:

    -   Sağdaki şirket sunucusundan yeni bir Bağlayıcı indirin ve bu gruba atayın

    -   Etkin bir Bağlayıcıyı gruba taşıma

-   Grupta etkin bağlayıcınız yoksa şunları yapabilirsiniz:

    -   Bağlayıcınızın neden etkin olmadığını belirleyin ve çözümkurun

    -   Etkin bir Bağlayıcıyı gruba taşıma

Sorunu çözmek için Uygulamanızdaki "Uygulama Proxy" menüsünü açın ve Bağlayıcı Grubu uyarı iletisine bakın. Grupta bağlayıcı yoksa, uyarı iletisinde grubun en az bir Bağlayıcıya ihtiyacı olduğunu belirtir. Etkin Bağlayıcınız yoksa, uyarı iletisi bunu açıklar. Etkin olmayan bağlayıcılara sahip olmak yaygındır. 

   ![Azure portalında bağlayıcı grup seçimi](./media/application-proxy-connectivity-no-working-connector/no-active-connector.png)

Bu seçeneklerin her biri hakkında ayrıntılı bilgi için aşağıdaki ilgili bölüme bakın. Yönergeler, Bağlayıcı yönetim sayfasından başladığınızı varsayar. Yukarıdaki hata iletisine bakıyorsanız, uyarı iletisine tıklayarak bu sayfaya gidebilirsiniz. Ayrıca **Azure Active Directory**'ye giderek, **Kurumsal Uygulamalar'a**, ardından **Uygulama Proxy'sine** tıklayarak da sayfaya ulaşabilirsiniz.

   ![Azure portalında bağlayıcı grup yönetimi](./media/application-proxy-connectivity-no-working-connector/app-proxy.png)

## <a name="download-a-new-connector"></a>Yeni bir Bağlayıcı Indirin

Yeni bir Bağlayıcı indirmek için sayfanın üst kısmındaki "İndir Bağlayıcısı" düğmesini kullanın.

Konektörü arka uç uygulamasına doğrudan görüş hattı olan bir makineye takın. Genellikle, bağlayıcı uygulamayla aynı sunucuya yüklenir. İndirdikten sonra Bağlayıcı bu menüde görünmelidir. Bağlayıcıyı tıklatın ve doğru gruba ait olduğundan emin olmak için "Bağlayıcı Grubu" açılır kullanın. Yaptığınız değişikliği kaydedin.

   ![Azure portalından konektörü indirin](./media/application-proxy-connectivity-no-working-connector/download-connector.png)
   
## <a name="move-an-active-connector"></a>Etkin Bağlayıcıyı Taşıma

Gruba ait olması gereken etkin bir Bağlayıcınız varsa ve hedef arka uç uygulamasına görüş hattı varsa, Bağlayıcıyı atanan gruba taşıyabilirsiniz. Bunu yapmak için Bağlayıcı'yı tıklatın. "Bağlayıcı Grubu" alanında, doğru grubu seçmek için açılır dosyayı kullanın ve Kaydet'i tıklatın.

## <a name="resolve-an-inactive-connector"></a>Etkin olmayan bir Bağlayıcıyı Çözme

Gruptaki tek Bağlayıcılar etkin değilse, büyük olasılıkla gerekli tüm bağlantı noktalarının engelini kaldırmayan bir makinede direnir.

bu sorunu araştırmaya ilişkin ayrıntılar için bağlantı noktaları Sorun Giderme belgesine bakın.

## <a name="next-steps"></a>Sonraki adımlar
[Azure AD Uygulama Proxy bağlayıcılarını anlama](application-proxy-connectors.md)


