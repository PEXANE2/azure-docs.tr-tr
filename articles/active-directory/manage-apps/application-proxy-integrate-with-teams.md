---
title: Ekipler'de Azure AD App Proxy uygulamalarına erişin | Microsoft Dokümanlar
description: Şirket içi uygulamanıza Microsoft Teams aracılığıyla erişmek için Azure AD Application Proxy'yi kullanın.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/05/2017
ms.author: mimart
ms.reviewer: harshja
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 037e005993a54e525560571a6d893197af99b6a0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67807768"
---
# <a name="access-your-on-premises-applications-through-microsoft-teams"></a>Microsoft Teams aracılığıyla şirket içi uygulamalarınıza erişin

Azure Active Directory Application Proxy, nerede olursanız olun şirket içi uygulamalarda tek oturum açma olanağı sağlar. Microsoft Teams, ortak çabalarınızı tek bir yerde kolaylaştırır. Bu ikisini bir araya getirmek, kullanıcılarınızın her durumda takım arkadaşlarıyla üretken olabileceği anlamına gelir.

Kullanıcılarınız [sekmeleri kullanarak](https://support.office.com/article/Video-Using-Tabs-7350a03e-017a-4a00-a6ae-1c9fe8c497b3?ui=en-US&rs=en-US&ad=US)Ekipleri kanallarına bulut uygulamaları ekleyebilir, ancak SharePoint siteleri veya şirket içinde barındırılan planlama aracı ne olacak? Uygulama Proxy çözümdür. Uygulamalarına uzaktan erişmek için her zaman kullandıkları aynı harici URL'leri kullanarak Uygulamalarına Application Proxy aracılığıyla yayınlanan uygulamaları kanallarına ekleyebilirler. Uygulama Proxy'si Azure Active Directory aracılığıyla kimlik doğrulaması yaptığı için, kullanıcılarınız tek bir oturum açma deneyimi elde eder.

## <a name="install-the-application-proxy-connector-and-publish-your-app"></a>Uygulama Proxy bağlayıcısını yükleyin ve uygulamanızı yayınlayın

Henüz yapmadıysanız, [kiracınız için Uygulama Proxy'sini yapılandırın ve bağlayıcıyı yükleyin.](application-proxy-add-on-premises-application.md) Ardından, uzaktan erişim için [şirket içi uygulamanızı yayınlayın.](application-proxy-add-on-premises-application.md) Uygulamayı yayınlarken, uygulamayı Takımlar'a eklemek için kullanıldığından harici URL'ye dikkat edin.

Uygulamalarınızın zaten yayınlanması varsa ancak harici URL'lerini hatırlamıyorsanız, bunları [Azure portalında](https://portal.azure.com)arayın. Oturum açın, ardından **Azure Active Directory** > **Enterprise uygulamalarına** > gidin**Tüm uygulamalar** uygulamanızı seçin > uygulamanızı > **Uygulama proxy'nizi**seçin.

## <a name="add-your-app-to-teams"></a>Uygulamanızı Takımlara Ekleyin

Uygulamayı Uygulama Proxy aracılığıyla yayımladıktan sonra, kullanıcılarınıza uygulamayı doğrudan Takımlar kanallarına sekme olarak ekleyebileceğini ve ardından uygulamanın takımdaki herkesin kullanabileceği şekilde kullanılabildiğini bildirin. Bu üç adımı izlemelerini söylesinler:

1. Bu uygulamayı eklemek istediğiniz Takımlar kanalına gidin **+** ve sekme eklemeyi seçin.

   ![Takımlar'da sekme eklemek için + seçin](./media/application-proxy-integrate-with-teams/add-tab.png)

1. Sekme seçeneklerinden **Web Sitesi'ni** seçin.

   ![Sekme ekle ekranından Web Sitesini seçin](./media/application-proxy-integrate-with-teams/website.png)

1. Sekmeye bir ad verin ve URL'yi Uygulama Proxy'si harici URL'sine ayarlayın.

   ![Sekmeyi adlandırın ve harici URL ekleyin](./media/application-proxy-integrate-with-teams/tab-name-url.png)

Bir takımın bir üyesi sekmeyi ekledikten sonra, kanaldaki herkes için gösterir. Uygulamaya erişimi olan tüm kullanıcılar, Microsoft Teams için kullandıkları kimlik bilgileriyle tek oturum açma erişimine erişir. Uygulamaya erişimi olmayan tüm kullanıcılar Sekmeyi Takımlar'da görebilir, ancak siz şirket içi uygulamaya ve uygulamanın yayınlanan Azure portalı sürümüne izin verene kadar engellenir.

## <a name="next-steps"></a>Sonraki adımlar

- Uygulama Proxy ile [şirket içi SharePoint sitelerini](application-proxy-integrate-with-sharepoint-server.md) nasıl yayınlayacağınızı öğrenin.
- Uygulamalarınızı harici [URL'leri](application-proxy-configure-custom-domain.md) için özel etki alanlarını kullanacak şekilde yapılandırın.
