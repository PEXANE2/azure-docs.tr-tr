---
title: 'Hızlı başlangıç: Azure Active Directory (Azure AD) kiracınıza uygulama ekleme'
description: Bu hızlı başlangıçta Azure portal kullanılarak Azure Active Directory (Azure AD) kiracınıza bir galeri uygulaması eklenmektedir.
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: quickstart
ms.workload: identity
ms.date: 10/29/2019
ms.author: kenwith
ms.collection: M365-identity-device-management
ms.openlocfilehash: f93a25854bc8b93e9bd75ea71037f6363d143914
ms.sourcegitcommit: 5cace04239f5efef4c1eed78144191a8b7d7fee8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86145719"
---
# <a name="quickstart-add-an-application-to-your-azure-active-directory-azure-ad-tenant"></a>Hızlı başlangıç: Azure Active Directory (Azure AD) kiracınıza uygulama ekleme

Azure Active Directory (Azure AD), önceden tümleştirilmiş binlerce uygulamanın yer aldığı bir galeriye sahiptir. Kuruluşunuzun kullandığı uygulamaların birçoğu büyük olasılıkla galeride zaten var.

Bir uygulamayı Azure AD kiracınıza ekledikten sonra aşağıdaki işlemleri gerçekleştirebilirsiniz:

- Uygulamanın özelliklerini yapılandırın.
- Koşullu erişim ilkesiyle uygulamaya yönelik Kullanıcı erişimini yönetin.
- Kullanıcıların Azure AD kimlik bilgileriyle uygulamada oturum açmasını sağlamak için çoklu oturum açmayı yapılandırın.

## <a name="prerequisites"></a>Önkoşullar

Azure AD kiracınıza bir uygulama eklemek için şunlar gerekir:

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Aşağıdaki rollerden biri: genel yönetici, bulut uygulaması Yöneticisi, uygulama Yöneticisi veya hizmet sorumlusu sahibi.
- (İsteğe bağlı: [uygulamalarınızı görüntüleme](view-applications-portal.md)işleminin tamamlanması).

>[!IMPORTANT]
>Bu hızlı başlangıçtaki adımları test etmek için üretim dışı bir ortam kullanmanızı öneririz.

## <a name="add-an-app-to-your-azure-ad-tenant"></a>Azure AD kiracınıza uygulama ekleme

Azure AD kiracınıza bir uygulama eklemek için:

1. [Azure Portal](https://portal.azure.com)sol gezinti panelinde **Azure Active Directory**' i seçin.
2. **Azure Active Directory** bölmesinde **Kurumsal uygulamalar**' ı seçin. **Tüm uygulamalar** bölmesi açılır ve Azure AD kiracınızdaki uygulamaların rastgele bir örneğini görüntüler.
3. **Kurumsal uygulamalar** bölmesinde **Yeni uygulama**' yı seçin. 
    ![Kiracınıza Galeri uygulaması eklemek için yeni uygulama ' yı seçin](media/add-application-portal/new-application.png)
4. Yeni Galeri önizleme deneyimine geç: **Uygulama Ekle sayfasının**en üstündeki başlıkta, **yeni ve geliştirilmiş uygulama galerisini denemek için buraya tıklayın**bağlantısını seçin.
5. **Azure AD galerisine gözatıp (Önizleme)** bölmesi açılır ve bulut platformları, şirket içi uygulamalar ve öne çıkan uygulamalar için kutucukları görüntüler. **Öne çıkan uygulamalar** bölümünde listelenen uygulamalar, federe çoklu oturum açma (SSO) ve sağlamayı destekleyip desteklemediğini gösteren simgelere sahiptir.
    ![Bir uygulamayı ada veya kategoriye göre ara](media/add-application-portal/browse-gallery.png)
6. Eklemek istediğiniz uygulamanın galerisine gözatıp, arama kutusuna adını girerek uygulamayı arayabilirsiniz. Sonra sonuçlardan uygulamayı seçin. Formunda, kuruluşunuzun gereksinimlerini karşılayacak şekilde uygulamanın adını düzenleyebilirsiniz. Bu örnekte GitHub ' ı seçtik ve adı **GitHub-test**olarak değiştirdi.
    ![Galeriden bir uygulamanın nasıl ekleneceğini gösterir](media/add-application-portal/create-application.png)
    >[!TIP]
    >Aradığınız uygulama galeride değilse, **kendi uygulamanızı oluşturma**bağlantısına tıklayabilirsiniz. Microsoft, Azure AD ile çalışacak şekilde önceden yapılandırmak için birçok uygulama geliştiricileriyle zaten çalıştık. Galeri 'de görüntülenen uygulamalar bu uygulamalardır. Ancak eklemek istediğiniz uygulama listede yoksa, yeni, genel bir uygulama oluşturabilir ve bunu kendiniz veya onu oluşturan geliştiricinin kılavuzumuzu ile yapılandırabilirsiniz.
7. **Oluştur**’u seçin. Uygulamayı kuruluşunuza göre yapılandırmak için kullanabileceğiniz seçeneklerin bulunduğu bir başlangıç sayfası açılır.

Uygulama eklemeyi tamamladınız. Sonraki hızlı başlangıçta, logosunun nasıl değiştirileceği ve uygulamanızın diğer özelliklerinin nasıl düzenleneceği gösterilmektedir.

## <a name="next-steps"></a>Sonraki adımlar

- [Bir uygulamayı yapılandırma](add-application-portal-configure.md)
- [Çoklu oturum açmayı ayarlama](add-application-portal-setup-sso.md)
- [Bir uygulamayı silme](delete-application-portal.md)