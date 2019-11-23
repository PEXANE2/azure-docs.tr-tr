---
title: Quickstart - View tenant applications using Azure Active Directory
description: In this Quickstart, use the Azure portal to view the applications in your Azure Active Directory (Azure AD) tenant.
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
ms.sourcegitcommit: 4c831e768bb43e232de9738b363063590faa0472
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2019
ms.locfileid: "74421808"
---
# <a name="quickstart-view-your-azure-active-directory-tenant-applications"></a>Quickstart: View your Azure Active Directory tenant applications

Bu hızlı başlangıçta Azure Active Directory (Azure AD) kiracınızdaki uygulamaları görüntülemek için Azure portal kullanılmaktadır.

## <a name="before-you-begin"></a>Başlamadan önce

Sonuçları görmek için Azure AD kiracınızda en az bir uygulamanız olması gerekir. Uygulama eklemek için [Uygulama ekleme](add-application-portal.md) hızlı başlangıcına bakın.

[Azure portalda](https://portal.azure.com) Azure AD kiracınızın genel yönetici, bulut uygulaması yöneticisi veya uygulama yöneticisi hesabıyla oturum açın.

## <a name="find-the-list-of-tenant-applications"></a>Kiracı uygulamaları listesini bulma

Azure AD kiracınızdaki uygulamaları Azure portalın **Kurumsal uygulamalar** bölümünden görüntüleyebilirsiniz.

Kiracı uygulamalarınızı bulmak için:

1. In the **[Azure portal](https://portal.azure.com)** , on the left navigation panel, select **Azure Active Directory**.
1. In the **Azure Active Directory** pane, select **Enterprise applications**.
1. From the **Application Type** drop-down menu, select **All Applications**, and choose **Apply**. Kiracınızdaki uygulamalardan rastgele seçilenler burada görünür.
1. To view more applications, select **Load more** at the bottom of the list. Kiracınızdaki uygulama sayısına bağlı olarak listeyi kaydırmak yerine [belirli bir uygulamayı aramak](#search-for-a-tenant-application) daha kolay olabilir.

## <a name="select-viewing-options"></a>Görüntüleme seçeneklerini belirleme

Select options according to what you're looking for.

1. You can view the applications by **Application Type**, **Application Status**, and **Application visibility**.
1. **Uygulama Türü** bölümünde aşağıdaki seçeneklerden birini belirleyin:

    - **Kurumsal Uygulamalar** seçeneği Microsoft harici uygulamaları gösterir.
    - **Microsoft Uygulamaları** seçeneği Microsoft uygulamalarını gösterir.
    - **Tüm Uygulamalar** seçeneği hem Microsoft harici uygulamaları hem de Microsoft uygulamalarını gösterir.

1. **Uygulama Durumu** bölümünde **Tümü**, **Devre dışı** veya **Etkin** seçeneğini belirleyin. **Tümü** seçeneği hem devre dışı hem de etkin uygulamaları gösterir.
1. **Uygulama Görünürlüğü** bölümünde **Tümü** veya **Gizli** seçeneğini belirleyin. The **Hidden** option shows applications that are in the tenant, but aren't visible to users.
1. After choosing the options you want, select **Apply**.

## <a name="search-for-a-tenant-application"></a>Belirli bir kiracı uygulamasını arama

To search for a particular application:

1. In the **Application Type** menu, select **All applications**, and choose **Apply**.
1. Bulmak istediğiniz uygulamanın adını girin. If the application has been added to your Azure AD tenant, it appears in the search results. This example shows that GitHub hasn't been added to the tenant applications.

    ![Example shows an app hasn't been added to the tenant](media/view-applications-portal/search-for-tenant-application.png)

1. Uygulama adının ilk birkaç harfini girmeyi deneyin. Bu örnek **Sales** ile başlayan tüm uygulamaları gösterir.

    ![Example shows all apps that start with Sales](media/view-applications-portal/search-by-prefix.png)

## <a name="next-steps"></a>Sonraki adımlar

In this quickstart, you learned how to view the applications in your Azure AD tenant. You learned how to filter the list of applications by application type, status, and visibility. Ayrıca belirli bir uygulamayı aramayı da öğrendiniz.

Now that you've found the application you were looking for, you can continue to [Add more applications to your tenant](add-application-portal.md). Or, you can select the application to view or edit properties and configuration options. Örneğin çoklu oturum açmayı yapılandırabilirsiniz.

> [!div class="nextstepaction"]
> [Çoklu oturum açmayı yapılandırma](configure-single-sign-on-non-gallery-applications.md)
