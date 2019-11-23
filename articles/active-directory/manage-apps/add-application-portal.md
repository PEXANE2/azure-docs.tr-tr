---
title: Quickstart - Add an app to your Azure Active Directory tenant
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
ms.sourcegitcommit: 4c831e768bb43e232de9738b363063590faa0472
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2019
ms.locfileid: "74420472"
---
# <a name="quickstart-add-an-application-to-your-azure-active-directory-tenant"></a>Hızlı başlangıç: Azure Active Directory kiracınıza uygulama ekleme

Azure Active Directory (Azure AD), önceden tümleştirilmiş binlerce uygulamanın yer aldığı bir galeriye sahiptir. Galeride kuruluşunuzun kullandığı uygulamaların bazıları da mevcuttur. Bu hızlı başlangıçta Azure portal kullanılarak Azure Active Directory (Azure AD) kiracınıza bir galeri uygulaması eklenmektedir.

Bir uygulamayı Azure AD kiracınıza ekledikten sonra aşağıdaki işlemleri gerçekleştirebilirsiniz:

- Manage user access to the application with a Conditional Access policy.
- Kullanıcıları Azure AD hesapları ile uygulamada çoklu oturum açma özelliğini kullanacak şekilde yapılandırma.

## <a name="before-you-begin"></a>Başlamadan önce

Kiracınıza uygulama eklemek için şunlara ihtiyacınız vardır:

- Azure AD aboneliği
- Uygulamanız için çoklu oturum açma özelliği etkinleştirilmiş abonelik

[Azure portalda](https://portal.azure.com) Azure AD kiracınızın genel yönetici, bulut uygulaması yöneticisi veya uygulama yöneticisi hesabıyla oturum açın.

Bu öğreticideki adımları test etmek için üretim ortamı harici bir ortam kullanmanızı öneririz. Üretim ortamı dışında bir Azure AD ortamınız yoksa [bir aylık deneme](https://azure.microsoft.com/pricing/free-trial/) aboneliği oluşturabilirsiniz.

## <a name="add-an-application-to-your-azure-ad-tenant"></a>Azure AD kiracınıza uygulama ekleme

Azure AD kiracınıza galeri uygulaması eklemek için:

1. In the [Azure portal](https://portal.azure.com), on the left navigation panel, select **Azure Active Directory**.

2. In the **Azure Active Directory** pane, select **Enterprise applications**. The **All applications** pane opens and displays a random sample of the applications in your Azure AD tenant.

3. To add a gallery app to your tenant, select **New application**. 

    ![Select New application to add a gallery app to your tenant](media/add-application-portal/new-application.png)

 4. Switch to the new gallery preview experience: In the banner at the top of the **Add an application page**, select the link that says **Click here to try out the new and improved app gallery**.

5. The **Browse Azure AD Gallery (Preview)** pane opens and displays tiles for cloud platforms, on-premises applications, and featured applications. Note that the applications listed in the **Featured applications** section have icons indicating whether they support federated single sign-on (SSO) and provisioning.

    ![Search for an app by name or category](media/add-application-portal/browse-gallery.png)

6. You can browse the gallery for the application you want to add, or search for the application by entering its name in the search box. Then select the application from the results. In the form, you can edit the name of the application to match the needs of your organization. In this example we've changed the name to **GitHub-test**.

    ![Shows how to add an application from the gallery](media/add-application-portal/create-application.png)

7. **Oluştur**'u seçin. Uygulamayı kuruluşunuza göre yapılandırmak için kullanabileceğiniz seçeneklerin bulunduğu bir başlangıç sayfası açılır.

You've finished adding your application. Aşağıdaki bölümlerde uygulamanızın logosunu değiştirme ve diğer özelliklerini düzenleme adımları gösterilmektedir.

## <a name="find-your-azure-ad-tenant-application"></a>Azure AD kiracısı uygulamanızı bulma

İşlemi burada bırakmak zorunda kaldığınızı ve uygulamayı yapılandırmaya devam etmek için daha sonra tekrar geldiğinizi düşünelim. The first thing to do is find your application.

1. In the **[Azure portal](https://portal.azure.com)** , on the left navigation panel, select **Azure Active Directory**.
1. In the **Azure Active Directory** pane, select **Enterprise applications**.
1. From the **Application Type** drop-down menu, select **All Applications**, and then select **Apply**. Görüntüleme seçenekleri hakkında daha fazla bilgi edinmek için bkz. [Kiracı uygulamalarını görüntüleme](view-applications-portal.md).
1. Burada Azure AD kiracınızdaki tüm uygulamaların listesini görebilirsiniz. Bu listedeki uygulamalar rastgele seçilmiştir. To see more applications, select **Show more** one or more times.
1. To quickly find an application in your tenant, enter the application name in the search box and select **Apply**. This example finds the GitHub-test application added previously.

    ![Shows how to find an application using the search box](media/add-application-portal/find-application.png)

## <a name="configure-user-sign-in-properties"></a>Kullanıcı oturum açma özelliklerini yapılandırma

Now that you've found the application, you can open it and configure application properties.

To edit the application properties:

1. Select the application to open it.
2. Select **Properties** to open the properties pane for editing.

    ![Shows the Properties screen and editable app properties](media/add-application-portal/edit-properties.png)

3. Birkaç dakikanızı ayırarak oturum açma seçeneklerini inceleyin. The options determine how users who are assigned or unassigned to the application can sign into the application. And, the options also determine if a user can see the application in the access panel.

    - **Enabled for users to sign-in?** determines whether users assigned to the application can sign in.
    - **User assignment required?** determines whether users who aren't assigned to the application can sign in.
    - **Visible to users?** determines whether users assigned to an app can see it in the access panel and O365 launcher.

4. Use the following tables to help you choose the best options for your needs.

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
       | Enabled for users to sign in? | Kullanıcı ataması gerekli mi? | Kullanıcılara görünür mü? | Atanmayan kullanıcılar oturum açabilir mi? | Atanmayan kullanıcılar uygulamayı görebilir mi?* |
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
1. Since you've already found your application, select the application.
1. In the left pane, select **Properties**.
1. Logoyu yükleyin.
1. When you're finished, select **Save**. 

    ![Shows how to change the logo from the app's Properties page](media/add-application-portal/change-logo.png)

   > [!NOTE]
   > The thumbnail displayed on this **Properties** pane doesn't update right away. You can close and reopen the properties to see the updated icon.

## <a name="next-steps"></a>Sonraki adımlar

Now that you've added the application to your Azure AD organization, [choose a single sign-on method](what-is-single-sign-on.md#choosing-a-single-sign-on-method) you want to use and refer to the appropriate article below:

- [Configure SAML-based single sign-on](configure-single-sign-on-non-gallery-applications.md)
- [Configure password single sign-on](configure-password-single-sign-on-non-gallery-applications.md)
- [Configure linked sign-on](configure-linked-sign-on.md)
