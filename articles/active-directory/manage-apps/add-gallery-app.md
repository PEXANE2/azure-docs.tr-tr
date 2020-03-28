---
title: Galeri uygulaması ekleme - Azure Active Directory | Microsoft Dokümanlar
description: Azure REKLAM galerisinden Azure kurumsal uygulamalarınıza nasıl uygulama ekleyeceğinizi öğrenin.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: tutorial
ms.workload: identity
ms.date: 10/29/2019
ms.author: mimart
ms.reviewer: arvinh,luleon
ms.collection: M365-identity-device-management
ms.openlocfilehash: af0a826c499790c86a20ea7061075a6c3e66b3a4
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73062634"
---
# <a name="add-a-gallery-app-to-your-azure-ad-organization"></a>Azure REKLAM kuruluşunuza bir galeri uygulaması ekleme

Azure Active Directory (Azure AD), Enterprise tek oturum açma özelliğiyle etkinleştirilen binlerce önceden tümleşik uygulama içeren bir galeriye sahiptir. Bu makalede, galeriden Azure REKLAM kuruluşunuza bir uygulama eklemek için genel adımlar açıklanmaktadır.

> [!IMPORTANT]
> İlk olarak, [SaaS uygulamalarını Azure Active Directory ile nasıl tümleştirebilirsiniz le ilgili öğreticiler listesinde](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/)uygulamanızı kontrol edin. Eklemek istediğiniz galeri uygulamasını eklemek ve yapılandırmak için adım adım kılavuz luk bulabilirsiniz.

## <a name="add-a-gallery-application"></a>Galeri uygulaması ekleme

1. [Azure portalda](https://portal.azure.com) Azure AD kiracınızın genel yönetici, bulut uygulaması yöneticisi veya uygulama yöneticisi hesabıyla oturum açın.

1. Sol daki gezinti panelinde [bulunan Azure portalında](https://portal.azure.com) **Azure Etkin Dizin'i**seçin.

1. Azure **Etkin Dizin** bölmesinde **Kurumsal uygulamaları**seçin.

    ![Kurumsal uygulamaları açın](media/add-gallery-app/open-enterprise-apps.png)


3. Kiracınıza bir galeri uygulaması eklemek için **Yeni uygulamayı**seçin.

    ![Kiracınıza galeri uygulaması eklemek için Yeni uygulama seçin](media/add-gallery-app/new-application.png)

 4. Yeni galeri önizleme deneyimine geçin: Uygulama ekle **sayfasının**üst kısmındaki başlıkta, **yeni ve geliştirilmiş uygulama galerisini denemek için buraya tıklayın**yazan bağlantıyı seçin.

5. **Azure REKLAM Galerisi'ne Gözat** bölmesi bulut platformları, şirket içi uygulamalar ve öne çıkan uygulamalar için kutucukları açar ve görüntüler. **Öne Çıkan uygulamalar** bölümünde listelenen uygulamaların federe tek oturum açma (SSO) ve sağlamayı destekleyip desteklemediklerini belirten simgelere sahip olduğunu unutmayın.

    ![Bir uygulamayı ada veya kategoriye göre arama](media/add-gallery-app/browse-gallery.png)

6. Eklemek istediğiniz uygulama için galeriye göz atın veya arama kutusuna adını girerek uygulamayı arayın. Ardından sonuçlardan uygulamayı seçin. (İsteğe bağlı) Formda, kuruluşunuzun gereksinimlerine uyacak şekilde uygulama adını edinebilirsiniz.

    ![Galeriden uygulama eklemenin nasıl yapılacağını gösterir](media/add-gallery-app/create-application.png)

7. **Oluştur'u**seçin. Uygulamayı kuruluşunuza göre yapılandırmak için kullanabileceğiniz seçeneklerin bulunduğu bir başlangıç sayfası açılır.

## <a name="configure-user-sign-in-properties"></a>Kullanıcı oturum açma özelliklerini yapılandırma

1. Düzenleme için özellikler bölmesini açmak için **Özellikler'i** seçin.

    ![Özellikleri ede: Bölme](media/add-gallery-app/edit-properties.png)

1. Uygulamaya atanan veya atanamayan kullanıcıların uygulamada nasıl oturum açabileceğini ve bir kullanıcının access panelinde uygulamayı görüp göremeyebileceğini belirlemek için aşağıdaki seçenekleri ayarlayın.

    - **Kullanıcıların oturum açması için etkinleştirildi**, uygulamaya atanan kullanıcıların oturum açıp açamayacağını belirler.
    - **Gerekli kullanıcı ataması,** uygulamaya atanamayan kullanıcıların oturum açıp açamayacağını belirler.
    - **Kullanıcıya görünür**, uygulamaya atanan kullanıcıların uygulamayı erişim panelinde ve O365 başlatıcısında görüp göremeyeceğini belirler.

      **Atanan** kullanıcılar için davranış:

       | Uygulama özelliği ayarları | | | Atanan kullanıcı deneyimi | |
       |---|---|---|---|---|
       | Kullanıcıların oturum açması için etkinleştirildi mi? | Kullanıcı ataması gerekli mi? | Kullanıcılara görünür mü? | Atanan kullanıcılar oturum açabilir mi? | Atanan kullanıcılar uygulamayı görebilir mi?* |
       | evet | evet | evet | evet | evet  |
       | evet | evet | hayır  | evet | hayır   |
       | evet | hayır  | evet | evet | evet  |
       | evet | hayır  | hayır  | evet | hayır   |
       | hayır  | evet | evet | hayır  | hayır   |
       | hayır  | evet | hayır  | hayır  | hayır   |
       | hayır  | hayır  | evet | hayır  | hayır   |
       | hayır  | hayır  | hayır  | hayır  | hayır   |

      **Atanmayan** kullanıcılar için davranış:

       | Uygulama özelliği ayarları | | | Atanmayan kullanıcı deneyimi | |
       |---|---|---|---|---|
       | Kullanıcıların oturum açabilmesi için etkinleştirildi mi? | Kullanıcı ataması gerekli mi? | Kullanıcılara görünür mü? | Atanmayan kullanıcılar oturum açabilir mi? | Atanmayan kullanıcılar uygulamayı görebilir mi?* |
       | evet | evet | evet | hayır  | hayır   |
       | evet | evet | hayır  | hayır  | hayır   |
       | evet | hayır  | evet | evet | hayır   |
       | evet | hayır  | hayır  | evet | hayır   |
       | hayır  | evet | evet | hayır  | hayır   |
       | hayır  | evet | hayır  | hayır  | hayır   |
       | hayır  | hayır  | evet | hayır  | hayır   |
       | hayır  | hayır  | hayır  | hayır  | hayır   |

     *Kullanıcı uygulamayı erişim panelinde ve Office 365 uygulama başlatıcıda görebilir mi?

1. Özel bir logo kullanmak için 215'e 215 piksel lik bir logo oluşturun ve PNG biçiminde kaydedin. Ardından logonuza göz atın ve yükleyin.

    ![Logoyu değiştirme](media/add-gallery-app/change-logo.png)

1. Bitirdikten sonra **Kaydet'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

Uygulamayı Azure REKLAM kuruluşunuza eklediğinize göre, kullanmak istediğiniz [tek bir oturum açma yöntemini seçin](what-is-single-sign-on.md#choosing-a-single-sign-on-method) ve aşağıdaki uygun makaleye bakın:

- [SAML tabanlı çoklu oturum açmayı yapılandırma](configure-single-sign-on-non-gallery-applications.md)
- [Parolayı tek oturum açma yapılandırma](configure-password-single-sign-on-non-gallery-applications.md)
- [Bağlantılı oturum açmayı yapılandırma](configure-linked-sign-on.md)

