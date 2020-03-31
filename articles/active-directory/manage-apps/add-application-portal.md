---
title: Quickstart - Azure Etkin Dizin kiracınıza uygulama ekleme
description: Bu hızlı başlangıçta Azure portal kullanılarak Azure Active Directory (Azure AD) kiracınıza bir galeri uygulaması eklenmektedir.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: quickstart
ms.workload: identity
ms.date: 10/29/2019
ms.author: mimart
ms.collection: M365-identity-device-management
ms.openlocfilehash: 650a4f643f170fc64dd0e0643d645a7dcbffdfbc
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79240495"
---
# <a name="quickstart-add-an-application-to-your-azure-active-directory-tenant"></a>Hızlı başlangıç: Azure Active Directory kiracınıza uygulama ekleme

Azure Active Directory (Azure AD), önceden tümleştirilmiş binlerce uygulamanın yer aldığı bir galeriye sahiptir. Galeride kuruluşunuzun kullandığı uygulamaların bazıları da mevcuttur. Bu hızlı başlangıçta Azure portal kullanılarak Azure Active Directory (Azure AD) kiracınıza bir galeri uygulaması eklenmektedir.

Bir uygulamayı Azure AD kiracınıza ekledikten sonra aşağıdaki işlemleri gerçekleştirebilirsiniz:

- Koşullu Erişim ilkesiyle uygulamaya kullanıcı erişimini yönetin.
- Kullanıcıları Azure AD hesapları ile uygulamada çoklu oturum açma özelliğini kullanacak şekilde yapılandırma.

## <a name="before-you-begin"></a>Başlamadan önce

Kiracınıza uygulama eklemek için şunlara ihtiyacınız vardır:

- Azure AD aboneliği
- Uygulamanız için çoklu oturum açma özelliği etkinleştirilmiş abonelik

[Azure portalda](https://portal.azure.com) Azure AD kiracınızın genel yönetici, bulut uygulaması yöneticisi veya uygulama yöneticisi hesabıyla oturum açın.

Bu öğreticideki adımları test etmek için üretim ortamı harici bir ortam kullanmanızı öneririz. Üretim ortamı dışında bir Azure AD ortamınız yoksa [bir aylık deneme](https://azure.microsoft.com/pricing/free-trial/) aboneliği oluşturabilirsiniz.

## <a name="add-an-application-to-your-azure-ad-tenant"></a>Azure AD kiracınıza uygulama ekleme

Azure AD kiracınıza galeri uygulaması eklemek için:

1. Sol daki gezinti panelinde [bulunan Azure portalında](https://portal.azure.com) **Azure Etkin Dizin'i**seçin.

2. Azure **Etkin Dizin** bölmesinde **Kurumsal uygulamaları**seçin. **Tüm uygulamalar** bölmesi açılır ve Azure AD kiracınızda uygulamaların rasgele bir örneğini görüntüler.

3. Kiracınıza bir galeri uygulaması eklemek için **Yeni uygulamayı**seçin. 

    ![Kiracınıza galeri uygulaması eklemek için Yeni uygulama seçin](media/add-application-portal/new-application.png)

 4. Yeni galeri önizleme deneyimine geçin: Uygulama ekle **sayfasının**üst kısmındaki başlıkta, **yeni ve geliştirilmiş uygulama galerisini denemek için buraya tıklayın**yazan bağlantıyı seçin.

5. **Azure REKLAM Galerisi'ne Gözat (Önizleme)** bölmesi bulut platformları, şirket içi uygulamalar ve öne çıkan uygulamalar için kutucukları açar ve görüntüler. **Öne Çıkan uygulamalar** bölümünde listelenen uygulamaların federe tek oturum açma (SSO) ve sağlamayı destekleyip desteklemediklerini belirten simgelere sahip olduğunu unutmayın.

    ![Bir uygulamayı ada veya kategoriye göre arama](media/add-application-portal/browse-gallery.png)

6. Eklemek istediğiniz uygulama için galeriye göz atabilir veya arama kutusuna adını girerek uygulamayı arayabilirsiniz. Ardından sonuçlardan uygulamayı seçin. Formda, kuruluşunuzun gereksinimleriyle eşleşecek şekilde uygulamanın adını edinebilirsiniz. Bu örnekte, adı **GitHub-test**olarak değiştirdik.

    ![Galeriden uygulama eklemenin nasıl yapılacağını gösterir](media/add-application-portal/create-application.png)

7. **Oluştur'u**seçin. Uygulamayı kuruluşunuza göre yapılandırmak için kullanabileceğiniz seçeneklerin bulunduğu bir başlangıç sayfası açılır.

Başvurunuzu eklemeyi bitirdiniz. Aşağıdaki bölümlerde uygulamanızın logosunu değiştirme ve diğer özelliklerini düzenleme adımları gösterilmektedir.

## <a name="find-your-azure-ad-tenant-application"></a>Azure AD kiracısı uygulamanızı bulma

İşlemi burada bırakmak zorunda kaldığınızı ve uygulamayı yapılandırmaya devam etmek için daha sonra tekrar geldiğinizi düşünelim. Yapmanız gereken ilk şey, uygulamanızı bulmaktır.

1. Sol daki gezinti panelinde **[bulunan Azure portalında](https://portal.azure.com)** **Azure Etkin Dizin'i**seçin.
1. Azure **Etkin Dizin** bölmesinde **Kurumsal uygulamaları**seçin.
1. Uygulama **Türü** açılır menüsünden **Tüm Uygulamalar'ı**seçin ve sonra **Uygula'yı**seçin. Görüntüleme seçenekleri hakkında daha fazla bilgi edinmek için bkz. [Kiracı uygulamalarını görüntüleme](view-applications-portal.md).
1. Burada Azure AD kiracınızdaki tüm uygulamaların listesini görebilirsiniz. Bu listedeki uygulamalar rastgele seçilmiştir. Daha fazla uygulama görmek **için, daha fazla** kez göster'i seçin.
1. Kiracınızda bir uygulamayı hızla bulmak için, arama kutusuna uygulama adını girin ve **Uygula'yı**seçin. Bu örnek, daha önce eklenen GitHub test uygulamasını bulur.

    ![Arama kutusunu kullanarak uygulamanın nasıl bulunup bulunur unu gösterir](media/add-application-portal/find-application.png)

## <a name="configure-user-sign-in-properties"></a>Kullanıcı oturum açma özelliklerini yapılandırma

Uygulamayı bulduğunuzda, uygulamayı açabilir ve uygulama özelliklerini yapılandırabilirsiniz.

Uygulama özelliklerini deletmek için:

1. Açmak için uygulamayı seçin.
2. Düzenleme için özellikler bölmesini açmak için **Özellikler'i** seçin.

    ![Özellikler ekranını ve kullanılabilir uygulama özelliklerini gösterir](media/add-application-portal/edit-properties.png)

3. Birkaç dakikanızı ayırarak oturum açma seçeneklerini inceleyin. Seçenekler, uygulamaya atanan veya atanamayan kullanıcıların uygulamada nasıl oturum açabileceğini belirler. Ayrıca seçenekler, kullanıcının erişimi panelde uygulamayı görüp göremeyebileceğini de belirler.

    - **Kullanıcıların oturum açmaları için etkinleştirildi mi?** uygulamaya atanan kullanıcıların oturum açıp açamayacağını belirler.
    - **Kullanıcı ataması gerekli mi?** uygulamaya atanmamış kullanıcıların oturum açıp açamayacağını belirler.
    - **Kullanıcılar tarafından görülebilir mi?** bir uygulamaya atanan kullanıcıların uygulamanın erişim panelinde ve O365 başlatıcısında görüp göremeyeceğini belirler.

4. Gereksinimleriniz için en iyi seçenekleri seçmenize yardımcı olmak için aşağıdaki tabloları kullanın.

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

## <a name="use-a-custom-logo"></a>Özel logo kullanma

Özel logo kullanmak için:

1. 215x215 piksel boyutunda bir logo oluşturun ve PNG biçiminde kaydedin.
1. Başvurunuzu zaten bulduğunuza göre uygulamayı seçin.
1. Sol bölmede **Özellikler'i**seçin.
1. Logoyu yükleyin.
1. Bitirdikten sonra **Kaydet'i**seçin. 

    ![Uygulamanın Özellikler sayfasından logoyu nasıl değiştireceğinigösterir](media/add-application-portal/change-logo.png)

   > [!NOTE]
   > Bu **Özellikler** bölmesindeki küçük resim hemen güncellenmez. Güncelleştirilmiş simgeyi görmek için özellikleri kapatıp yeniden açabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Uygulamayı Azure REKLAM kuruluşunuza eklediğinize göre, kullanmak istediğiniz [tek bir oturum açma yöntemini seçin](what-is-single-sign-on.md#choosing-a-single-sign-on-method) ve aşağıdaki uygun makaleye bakın:

- [SAML tabanlı çoklu oturum açmayı yapılandırma](configure-single-sign-on-non-gallery-applications.md)
- [Parolayı tek oturum açma yapılandırma](configure-password-single-sign-on-non-gallery-applications.md)
- [Bağlantılı oturum açmayı yapılandırma](configure-linked-sign-on.md)
