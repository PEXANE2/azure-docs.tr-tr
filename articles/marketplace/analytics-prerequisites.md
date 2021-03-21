---
title: Analiz verilerine programlı olarak erişim önkoşulları
description: Ticari Market Analytics verilerine programlı bir şekilde erişebilmek için öncelikle karşılamanız gereken gereksinimleri öğrenin.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
author: sayantanroy83
ms.author: sroy
ms.date: 3/08/2021
ms.openlocfilehash: 3b109048be4a94990c26e31aa5bc2ad36fdd0211
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102584184"
---
# <a name="prerequisites-to-programmatically-access-analytics-data"></a>Analiz verilerine programlı olarak erişim önkoşulları

Ticari Market Analytics verilerine programlı bir şekilde erişebilmeniz için aşağıdaki gereksinimleri karşılamanız gerekir.

## <a name="commercial-marketplace-enrollment"></a>Ticari Market kaydı

Ticari Market Analytics verilerine programlı bir şekilde erişmek için ticari Market programına kaydolduktan ve bir Iş Ortağı Merkezi hesabına sahip olmanız gerekir. Nasıl yapılacağını öğrenmek için bkz. [Iş Ortağı Merkezi 'nde ticari Market hesabı oluşturma](./partner-center-portal/create-account.md).

## <a name="create-azure-active-directory-application"></a>Azure Active Directory uygulaması oluşturma

Normal Kullanıcı kimlik bilgileri, ticari Market Analytics verilerine programlı erişim için kullanılamaz. Analiz API 'Lerine erişmek için bir Azure Active Directory (Azure AD) uygulamasının bir gizli anahtar ile birlikte oluşturulması gerekir. Azure AD uygulaması ve gizli dizi oluşturmayı öğrenmek için bkz. [hızlı başlangıç: Microsoft Identity platformu ile uygulama kaydetme](https://docs.microsoft.com/azure/active-directory/develop/quickstart-register-app).

## <a name="associate-the-azure-ad-application-to-the-partner-center-tenant"></a>Azure AD uygulamasını Iş Ortağı Merkezi kiracısı ile ilişkilendir

Azure portal oluşturduğunuz Azure AD uygulamasının Iş Ortağı Merkezi hesabınıza bağlanması gerekir. Adımlar şu şekildedir:

1. [Iş Ortağı Merkezi](https://partner.microsoft.com/dashboard)' nde oturum açın.
1. Sağ üst köşedeki dişli simgesini seçin ve ardından **Hesap ayarları**' nı seçin.
1. **Hesap ayarları** menüsünde **Kullanıcı yönetimi**' ni seçin.
1. **Azure AD uygulamaları** ' nı seçin ve **+ Azure AD uygulaması oluştur**' u seçin.
1. Azure portal oluşturduğunuz Azure AD uygulamasını seçin ve ardından **İleri**' yi seçin.
1. **Yönetici (Windows)** onay kutusunu seçin ve ardından **Ekle**' yi seçin.

    :::image type="content" source="./media/analytics-programmatic-access/azure-ad-roles.png" alt-text="Rol seçme onay kutuları ile Azure AD uygulaması oluştur sayfasını gösterir.":::

## <a name="generate-an-azure-ad-token"></a>Azure AD belirteci oluşturma

Uygulama (istemci) KIMLIĞINI kullanarak bir Azure AD belirteci oluşturmanız gerekir. Bu KIMLIK, Microsoft Identity platformunda istemci uygulamanızı ve önceki adımdan istemci gizli anahtarını benzersiz olarak tanımlamanızı sağlar. Azure AD belirteci oluşturma adımları için bkz. [istemci kimlik bilgilerini (paylaşılan gizlilik veya sertifika) kullanarak hizmet çağrıları](https://docs.microsoft.com/azure/active-directory/azuread-dev/v1-oauth2-client-creds-grant-flow).

> [!NOTE]
> Belirteç bir saat için geçerlidir.

## <a name="next-steps"></a>Sonraki adımlar

- [Programlı erişim paradigması](analytics-programmatic-access.md)
