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
ms.openlocfilehash: 044c55e64c6532bf08d31b2c052e4ee9f5069ff1
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2020
ms.locfileid: "85956109"
---
# <a name="quickstart-add-an-application-to-your-azure-active-directory-azure-ad-tenant"></a>Hızlı başlangıç: Azure Active Directory (Azure AD) kiracınıza uygulama ekleme

Azure Active Directory (Azure AD), önceden tümleştirilmiş binlerce uygulamanın yer aldığı bir galeriye sahiptir. Kuruluşunuzun kullandığı uygulamaların birçoğu büyük olasılıkla galeride zaten var. Bu hızlı başlangıç, Azure AD kiracınıza bir galeri uygulaması eklemek için Azure portal kullanır. Bu hızlı başlangıç, zaten galerinin bir parçası olan bir uygulama eklemeye odaklanır. Galeriye dahil olmayan uygulamalar da Azure AD ile tümleştirilebilir, ancak bu hızlı başlangıçta kapsanmaz. 

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

Azure AD kiracınıza galeri uygulaması eklemek için:

1. [Azure Portal](https://portal.azure.com)sol gezinti panelinde **Azure Active Directory**' i seçin.
2. **Azure Active Directory** bölmesinde **Kurumsal uygulamalar**' ı seçin. **Tüm uygulamalar** bölmesi açılır ve Azure AD kiracınızdaki uygulamaların rastgele bir örneğini görüntüler.
3. **Kurumsal uygulamalar** bölmesinde **Yeni uygulama**' yı seçin. 
    ![Kiracınıza Galeri uygulaması eklemek için yeni uygulama ' yı seçin](media/add-application-portal/new-application.png)
4. Yeni Galeri önizleme deneyimine geç: **Uygulama Ekle sayfasının**en üstündeki başlıkta, **yeni ve geliştirilmiş uygulama galerisini denemek için buraya tıklayın**bağlantısını seçin.
5. **Azure AD galerisine gözatıp (Önizleme)** bölmesi açılır ve bulut platformları, şirket içi uygulamalar ve öne çıkan uygulamalar için kutucukları görüntüler. **Öne çıkan uygulamalar** bölümünde listelenen uygulamalar, federe çoklu oturum açma (SSO) ve sağlamayı destekleyip desteklemediğini gösteren simgelere sahiptir.
    ![Bir uygulamayı ada veya kategoriye göre ara](media/add-application-portal/browse-gallery.png)
6. Eklemek istediğiniz uygulamanın galerisine gözatıp, arama kutusuna adını girerek uygulamayı arayabilirsiniz. Sonra sonuçlardan uygulamayı seçin. Formunda, kuruluşunuzun gereksinimlerini karşılayacak şekilde uygulamanın adını düzenleyebilirsiniz. Bu örnekte, adı **GitHub-test**olarak değiştirdik.
    ![Galeriden bir uygulamanın nasıl ekleneceğini gösterir](media/add-application-portal/create-application.png)
7. **Oluştur**'u seçin. Uygulamayı kuruluşunuza göre yapılandırmak için kullanabileceğiniz seçeneklerin bulunduğu bir başlangıç sayfası açılır.

Uygulama eklemeyi tamamladınız. Sonraki hızlı başlangıçta, logosunun nasıl değiştirileceği ve uygulamanızın diğer özelliklerinin nasıl düzenleneceği gösterilmektedir.

## <a name="next-steps"></a>Sonraki adımlar

- [Uygulama yapılandırma](add-application-portal-configure.md)
- [Çoklu oturum açmayı ayarlama](add-application-portal-setup-sso.md)
- [Uygulama silme](delete-application-portal.md)