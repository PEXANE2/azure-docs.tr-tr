---
title: Azure Active Directory kiracınıza uygulama ekleme | Microsoft Docs
description: Bu hızlı başlangıçta Azure portal kullanılarak Azure Active Directory (Azure AD) kiracınıza bir galeri uygulaması eklenmektedir.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: quickstart
ms.workload: identity
ms.date: 04/09/2019
ms.author: mimart
ms.collection: M365-identity-device-management
ms.openlocfilehash: 466660a1e064ef41eb330b36107dbdcb1d097498
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68477314"
---
# <a name="quickstart-add-an-application-to-your-azure-active-directory-tenant"></a>Hızlı Başlangıç: Azure Active Directory kiracınıza uygulama ekleme

Azure Active Directory (Azure AD), önceden tümleştirilmiş binlerce uygulamanın yer aldığı bir galeriye sahiptir. Galeride kuruluşunuzun kullandığı uygulamaların bazıları da mevcuttur. Bu hızlı başlangıçta Azure portal kullanılarak Azure Active Directory (Azure AD) kiracınıza bir galeri uygulaması eklenmektedir.

Bir uygulamayı Azure AD kiracınıza ekledikten sonra aşağıdaki işlemleri gerçekleştirebilirsiniz:

- Koşullu erişim ilkesiyle uygulamaya yönelik Kullanıcı erişimini yönetin.
- Kullanıcıları Azure AD hesapları ile uygulamada çoklu oturum açma özelliğini kullanacak şekilde yapılandırma.

## <a name="before-you-begin"></a>Başlamadan önce

Kiracınıza uygulama eklemek için şunlara ihtiyacınız vardır:

- Azure AD aboneliği
- Uygulamanız için çoklu oturum açma özelliği etkinleştirilmiş abonelik

[Azure portalda](https://portal.azure.com) Azure AD kiracınızın genel yönetici, bulut uygulaması yöneticisi veya uygulama yöneticisi hesabıyla oturum açın.

Bu öğreticideki adımları test etmek için üretim ortamı harici bir ortam kullanmanızı öneririz. Üretim ortamı dışında bir Azure AD ortamınız yoksa [bir aylık deneme](https://azure.microsoft.com/pricing/free-trial/) aboneliği oluşturabilirsiniz.

## <a name="add-an-application-to-your-azure-ad-tenant"></a>Azure AD kiracınıza uygulama ekleme

Azure AD kiracınıza galeri uygulaması eklemek için:

1. [Azure Portal](https://portal.azure.com)sol gezinti panelinde **Azure Active Directory**' i seçin.
1. **Azure Active Directory** bölmesinde **Kurumsal uygulamalar**' ı seçin.
1. Azure AD kiracınızdaki uygulamaların rastgele bir örneğini göstermek için **tüm uygulamalar** bölmesi açılır. Kiracınıza bir galeri uygulaması eklemek için **tüm uygulamalar** bölmesinin en üstündeki **Yeni uygulama** ' yı seçin.

    ![Kiracınıza Galeri uygulaması eklemek için yeni uygulama ' yı seçin](media/add-application-portal/new-application.png)

1. **Kategoriler** bölmesinde, Galeri uygulamalarının rastgele bir örneği olan **öne çıkan uygulamalar** alanının altında simgeler görürsünüz. Daha fazla uygulama görmek için **daha fazla göster** ' i seçebilirsiniz ancak galeride binlerce uygulama olduğundan bu şekilde arama yapmanız önerilmez.

    ![Bir uygulamayı ada veya kategoriye göre ara](media/add-application-portal/categories.png)

1. Bir uygulamayı aramak için, **Galeriden Ekle**altında, eklemek istediğiniz uygulamanın adını girin. Sonuçlardan uygulamayı seçin ve **Ekle**' yi seçin. Aşağıdaki örnek, github.com arama sonrasında görünen **Uygulama Ekle** formunu gösterir.

    ![Galeriden bir uygulamanın nasıl ekleneceğini gösterir](media/add-application-portal/add-an-application.png)

1. Uygulamaya özgü formda özellik bilgilerini değiştirebilirsiniz. Örneğin uygulamanızın adını kuruluşunuzun gereksinimlerine göre düzenleyebilirsiniz. Bu örnekte **GitHub-test** adı kullanılmıştır.
1. Özelliklerde değişiklik yapmayı bitirdiğinizde **Ekle**' yi seçin.
1. Uygulamayı kuruluşunuza göre yapılandırmak için kullanabileceğiniz seçeneklerin bulunduğu bir başlangıç sayfası açılır.

Uygulamanızı eklemeyi tamamladınız. Şimdi biraz ara verebilirsiniz. Aşağıdaki bölümlerde uygulamanızın logosunu değiştirme ve diğer özelliklerini düzenleme adımları gösterilmektedir.

## <a name="find-your-azure-ad-tenant-application"></a>Azure AD kiracısı uygulamanızı bulma

İşlemi burada bırakmak zorunda kaldığınızı ve uygulamayı yapılandırmaya devam etmek için daha sonra tekrar geldiğinizi düşünelim. Yapmanız gereken ilk şey, uygulamanızı bulmledir.

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory**' i seçin.
1. **Azure Active Directory** bölmesinde **Kurumsal uygulamalar**' ı seçin.
1. **Uygulama türü** açılan menüsünde, **tüm uygulamalar**' ı seçin ve ardından **Uygula**' yı seçin. Görüntüleme seçenekleri hakkında daha fazla bilgi edinmek için bkz. [Kiracı uygulamalarını görüntüleme](view-applications-portal.md).
1. Burada Azure AD kiracınızdaki tüm uygulamaların listesini görebilirsiniz. Bu listedeki uygulamalar rastgele seçilmiştir. Daha fazla uygulama görmek için **daha fazla** bir veya daha fazla kez göster ' i seçin.
1. Kiracınızda bir uygulamayı hızlı bir şekilde bulmak için, arama kutusuna uygulamanın adını girin ve **Uygula**' yı seçin. Bu örnek, daha önce eklenen GitHub-test uygulamasını bulur.

    ![Arama kutusunu kullanarak bir uygulamanın nasıl bulunacağını gösterir](media/add-application-portal/find-application.png)

## <a name="configure-user-sign-in-properties"></a>Kullanıcı oturum açma özelliklerini yapılandırma

Uygulamayı buldığınıza göre, onu açabilir ve uygulama özelliklerini yapılandırabilirsiniz.

Uygulama özelliklerini düzenlemek için:

1. Açmak için uygulamayı seçin.
1. Özellikler bölmesini düzenlenmek üzere açmak için **Özellikler** ' i seçin.

    ![Özellikler ekranını ve düzenlenebilir uygulama özelliklerini gösterir](media/add-application-portal/edit-properties.png)

1. Birkaç dakikanızı ayırarak oturum açma seçeneklerini inceleyin. Seçenekler, uygulamaya atanan veya atanmamış kullanıcıların uygulamada oturum açıp atamalayamadığını belirleyebilir. Ayrıca, seçenekler de kullanıcının erişim panelinde uygulamayı görüp görmediğine de sahiptir.

    - **Kullanıcıların oturum açması için etkinleştirildi**, uygulamaya atanan kullanıcıların oturum açıp açamayacağını belirler.
    - **Gerekli Kullanıcı Ataması** , uygulamaya atanmamış kullanıcıların oturum açıp açamayacağını belirler.
    - **Kullanıcıya görünür**, uygulamaya atanan kullanıcıların uygulamayı erişim panelinde ve O365 başlatıcısında görüp göremeyeceğini belirler.

1. Gereksinimleriniz için en iyi seçenekleri seçmenize yardımcı olması için aşağıdaki tabloları kullanın.

   - **Atanan** kullanıcılar için davranış:

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

   - **Atanmayan** kullanıcılar için davranış:

       | Uygulama özelliği ayarları | | | Atanmayan kullanıcı deneyimi | |
       |---|---|---|---|---|
       | Kullanıcıların oturum açması için etkinleştirildi mi? | Kullanıcı ataması gerekli mi? | Kullanıcılara görünür mü? | Atanmayan kullanıcılar oturum açabilir mi? | Atanmayan kullanıcılar uygulamayı görebilir mi?* |
       | evet | evet | evet | hayır  | hayır   |
       | evet | evet | hayır  | hayır  | hayır   |
       | evet | hayır  | evet | evet | hayır   |
       | evet | hayır  | hayır  | evet | hayır   |
       | hayır  | evet | evet | hayır  | hayır   |
       | hayır  | evet | hayır  | hayır  | hayır   |
       | hayır  | hayır  | evet | hayır  | hayır   |
       | hayır  | hayır  | hayır  | hayır  | hayır   |

     *Kullanıcı uygulamayı erişim panelinde ve Office 365 uygulama başlatıcıda görebilir mi?

## <a name="use-a-custom-logo"></a>Özel logo kullanma

Özel logo kullanmak için:

1. 215x215 piksel boyutunda bir logo oluşturun ve PNG biçiminde kaydedin.
1. Uygulamanızı zaten bulduğundan, uygulamayı seçin.
1. Sol bölmede **Özellikler**' i seçin.
1. Logoyu yükleyin.
1. İşiniz bittiğinde **Kaydet**' i seçin.

    ![Uygulamanın Özellikler sayfasından logonun nasıl değiştirileceğini gösterir](media/add-application-portal/change-logo.png)

## <a name="next-steps"></a>Sonraki adımlar

Artık uygulamayı Azure AD kuruluşunuza eklemişseniz, kullanmak istediğiniz [tek bir oturum açma yöntemi seçin](what-is-single-sign-on.md#choosing-a-single-sign-on-method) ve aşağıdaki ilgili makaleye başvurun:

- [SAML tabanlı çoklu oturum açmayı yapılandırma](configure-single-sign-on-non-gallery-applications.md)
- [Parola çoklu oturum açmayı yapılandırma](configure-password-single-sign-on-non-gallery-applications.md)
- [Bağlantılı oturum açmayı Yapılandır](configure-linked-sign-on.md)
