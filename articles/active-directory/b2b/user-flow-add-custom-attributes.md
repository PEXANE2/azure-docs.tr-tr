---
title: Azure AD Kullanıcı akışları için özel öznitelikler ekleme
description: Self Servis kaydolma Kullanıcı akışlarınızın özniteliklerini özelleştirme hakkında bilgi edinin.
services: active-directory
author: msmimart
manager: celestedg
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: mimart
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1f187cc47d9c64c8257cc097734fa41e10629f1c
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83597453"
---
# <a name="define-custom-attributes-for-user-flows-preview"></a>Kullanıcı akışları için özel öznitelikler tanımlayın (Önizleme)
|     |
| --- |
| Özel Kullanıcı öznitelikleri özelliği, Azure Active Directory genel önizleme özelliğidir. Önizlemeler hakkında daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

Her uygulama için, kayıt sırasında toplamak istediğiniz bilgiler için farklı gereksinimlerinize sahip olabilirsiniz. Azure AD, belirli ad, soyadı, şehir ve posta kodu gibi özniteliklerde depolanan yerleşik bir bilgi kümesiyle gelir. Azure AD ile, dış Kullanıcı Kullanıcı akışından kaydolduğunda Konuk hesapta depolanan özniteliklerin kümesini genişletebilirsiniz.

Azure portal özel öznitelikler oluşturabilir ve bunları self servis kaydolma Kullanıcı akışlarınızda kullanabilirsiniz. Ayrıca, [MICROSOFT Graph API](https://docs.microsoft.com/azure/active-directory-b2c/manage-user-accounts-graph-api)'sini kullanarak bu öznitelikleri okuyabilir ve yazabilirsiniz. Microsoft Graph API, uzantı öznitelikleri olan bir kullanıcının oluşturulmasını ve güncelleştirilmesini destekler. Graph API uzantı öznitelikleri, kuralı kullanılarak adlandırılır `extension_<Application-client-id>_attributename` . Örnek:

```JSON
"extension_831374b3bd5041bfaa54263ec9e050fc_loyaltyNumber": "212342"
```

Öğesini `<Application-client-id>` **uygulama (ISTEMCI) kimliği**' nin yanındaki **uygulama kayıtları** sayfasında bulabilirsiniz. Bu KIMLIK, kiracınıza özeldir.

## <a name="create-a-custom-attribute"></a>Özel öznitelik oluştur

1. [Azure portalda](https://portal.azure.com) Azure AD yöneticisi olarak oturum açın.
2. **Azure hizmetleri**altında **Azure Active Directory**' yi seçin.
3. Sol taraftaki menüden **dış kimlikler**' i seçin.
4. **Özel Kullanıcı öznitelikleri (Önizleme)** seçeneğini belirleyin. Kullanılabilir Kullanıcı öznitelikleri listelenir.

   ![Kaydolma için Kullanıcı özniteliklerini seçin](media/user-flow-add-custom-attributes/user-attributes.png)

5. Bir öznitelik eklemek için **Ekle**' yi seçin.
6. **Öznitelik Ekle** bölmesinde, aşağıdaki değerleri girin:

   - **Ad** -özel öznitelik için bir ad sağlayın (örneğin, "shoesize").
   - **Veri türü** -bir veri türü seçin (**dize**, **Boole**veya **Int**).
   - **Açıklama** -isteğe bağlı olarak, iç kullanım için özel özniteliğin bir açıklamasını girin. Bu açıklama kullanıcıya görünür değil.

   ![Öznitelik ekleme](media/user-flow-add-custom-attributes/add-an-attribute.png)

7. **Oluştur**’u seçin.

Özel öznitelik artık Kullanıcı öznitelikleri listesinde ve Kullanıcı akışlarınızda kullanılmak üzere kullanılabilir. Özel bir öznitelik, Kullanıcı öznitelikleri listesine eklediğinizde değil, yalnızca herhangi bir Kullanıcı akışında kullanıldığı zaman oluşturulur.

Yeni oluşturulan özel özniteliği kullanan bir Kullanıcı akışı kullanarak yeni bir kullanıcı oluşturduktan sonra, nesne [Microsoft Graph Explorer](https://developer.microsoft.com/graph/graph-explorer)'da sorgulanabilir. Şimdi kayıt yolculuğu sırasında toplanan öznitelikler listesinde **Showesize** ' i görmeniz ve uygulamanıza geri gönderilen belirteçte onu görmeniz gerekir. Bu talepleri uygulamanıza geri gönderilen belirtece ekleme hakkında daha fazla bilgi için bkz. [dizin uzantısı isteğe bağlı taleplerini yapılandırma](https://docs.microsoft.com/azure/active-directory/develop/active-directory-optional-claims#configuring-directory-extension-optional-claims)

## <a name="next-steps"></a>Sonraki adımlar

[Bir uygulamaya self servis kaydolma Kullanıcı akışı ekleme](self-service-sign-up-user-flow.md)