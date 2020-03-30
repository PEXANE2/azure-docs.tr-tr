---
title: Quickstart - Azure Etkin Dizini kullanarak kiracı uygulamalarını görüntüleme
description: Bu Quickstart'ta, Azure Etkin Dizininizdeki (Azure AD) kiracınızdaki uygulamaları görüntülemek için Azure portalını kullanın.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 04/09/2019
ms.author: mimart
ms.reviewer: arvinh
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1d40e968bb7079d50e3fa18889ae996c9b59c90f
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "74421808"
---
# <a name="quickstart-view-your-azure-active-directory-tenant-applications"></a>Hızlı başlangıç: Azure Etkin Dizin kiracı uygulamalarınızı görüntüleyin

Bu hızlı başlangıçta Azure Active Directory (Azure AD) kiracınızdaki uygulamaları görüntülemek için Azure portal kullanılmaktadır.

## <a name="before-you-begin"></a>Başlamadan önce

Sonuçları görmek için Azure AD kiracınızda en az bir uygulamanız olması gerekir. Uygulama eklemek için [Uygulama ekleme](add-application-portal.md) hızlı başlangıcına bakın.

[Azure portalda](https://portal.azure.com) Azure AD kiracınızın genel yönetici, bulut uygulaması yöneticisi veya uygulama yöneticisi hesabıyla oturum açın.

## <a name="find-the-list-of-tenant-applications"></a>Kiracı uygulamaları listesini bulma

Azure AD kiracınızdaki uygulamaları Azure portalın **Kurumsal uygulamalar** bölümünden görüntüleyebilirsiniz.

Kiracı uygulamalarınızı bulmak için:

1. Sol daki gezinti panelinde **[bulunan Azure portalında](https://portal.azure.com)** **Azure Etkin Dizin'i**seçin.
1. Azure **Etkin Dizin** bölmesinde **Kurumsal uygulamaları**seçin.
1. Uygulama **Türü** açılır menüsünden **Tüm Uygulamalar'ı**seçin ve **Uygula'yı**seçin. Kiracınızdaki uygulamalardan rastgele seçilenler burada görünür.
1. Daha fazla uygulamayı görüntülemek için listenin en altında **daha fazla yükle'yi** seçin. Kiracınızdaki uygulama sayısına bağlı olarak listeyi kaydırmak yerine [belirli bir uygulamayı aramak](#search-for-a-tenant-application) daha kolay olabilir.

## <a name="select-viewing-options"></a>Görüntüleme seçeneklerini belirleme

Aradığınıza göre seçenekleri seçin.

1. **Uygulamaları Uygulama Türüne,** Uygulama **Durumuna**ve **Uygulama görünürlüğüne**göre görüntüleyebilirsiniz.
1. **Uygulama Türü** bölümünde aşağıdaki seçeneklerden birini belirleyin:

    - **Kurumsal Uygulamalar** seçeneği Microsoft harici uygulamaları gösterir.
    - **Microsoft Uygulamaları** seçeneği Microsoft uygulamalarını gösterir.
    - **Tüm Uygulamalar** seçeneği hem Microsoft harici uygulamaları hem de Microsoft uygulamalarını gösterir.

1. **Uygulama Durumu** bölümünde **Tümü**, **Devre dışı** veya **Etkin** seçeneğini belirleyin. **Tümü** seçeneği hem devre dışı hem de etkin uygulamaları gösterir.
1. **Uygulama Görünürlüğü** bölümünde **Tümü** veya **Gizli** seçeneğini belirleyin. **Gizli** seçeneği, kiracıda bulunan ancak kullanıcılar tarafından görülemeyen uygulamaları gösterir.
1. İstediğiniz seçenekleri seçtikten sonra **Uygula'yı**seçin.

## <a name="search-for-a-tenant-application"></a>Belirli bir kiracı uygulamasını arama

Belirli bir uygulamayı aramak için:

1. Uygulama **Türü** menüsünde **Tüm uygulamaları**seçin ve **Uygula'yı**seçin.
1. Bulmak istediğiniz uygulamanın adını girin. Uygulama Azure AD kiracınıza eklendiyse, arama sonuçlarında görünür. Bu örnek, GitHub'ın kiracı uygulamalarına eklenmediğini gösterir.

    ![Örnek, bir uygulamanın kiracıya eklenmediğini gösterir](media/view-applications-portal/search-for-tenant-application.png)

1. Uygulama adının ilk birkaç harfini girmeyi deneyin. Bu örnek **Sales** ile başlayan tüm uygulamaları gösterir.

    ![Örnek, Satışla başlayan tüm uygulamaları gösterir](media/view-applications-portal/search-by-prefix.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure AD kiracınızdaki uygulamaları nasıl görüntülediğinizi öğrendiniz. Uygulama türüne, durumuna ve görünürlüğe göre uygulama listesini nasıl filtrelediğinizi öğrendiniz. Ayrıca belirli bir uygulamayı aramayı da öğrendiniz.

Artık aradığınız uygulamayı bulduğunuza göre, [kiracınıza daha fazla uygulama eklemeye](add-application-portal.md)devam edebilirsiniz. Veya özellikleri ve yapılandırma seçeneklerini görüntülemek veya düzenlemek için uygulamayı seçebilirsiniz. Örneğin çoklu oturum açmayı yapılandırabilirsiniz.

> [!div class="nextstepaction"]
> [Çoklu oturum açmayı yapılandırma](configure-single-sign-on-non-gallery-applications.md)
