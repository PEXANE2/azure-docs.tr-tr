---
title: 'Hızlı başlangıç: Azure Active Directory (Azure AD) kiracınızdaki bir uygulama için özellikleri yapılandırma'
description: Bu hızlı başlangıç, Azure Active Directory (Azure AD) kiracınızla kaydedilmiş bir uygulamayı yapılandırmak için Azure portal kullanır.
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
ms.openlocfilehash: 82a3310f6fc2169a515b4b13d81c88d187bd0f9c
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2020
ms.locfileid: "85956161"
---
# <a name="quickstart-configure-properties-for-an-application-in-your-azure-active-directory-azure-ad-tenant"></a>Hızlı başlangıç: Azure Active Directory (Azure AD) kiracınızdaki bir uygulama için özellikleri yapılandırma

Önceki hızlı başlangıçta Azure AD kiracınıza bir uygulama eklediniz. Bir uygulama eklediğinizde, Azure AD kiracınıza uygulamanın kimlik sağlayıcısı olduğunu bildirmek için izin vermiş olursunuz. Artık uygulama için özelliklerden bazılarını yapılandıracaksınız.
 
## <a name="prerequisites"></a>Önkoşullar

Azure AD kiracınızdaki bir uygulamanın özelliklerini yapılandırmak için şunlar gerekir:

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Aşağıdaki rollerden biri: genel yönetici, bulut uygulaması Yöneticisi, uygulama Yöneticisi veya hizmet sorumlusu sahibi.
- (İsteğe bağlı: [uygulamalarınızı görüntüleme](view-applications-portal.md)işleminin tamamlanması).
- (İsteğe bağlı: [uygulama ekleme](add-application-portal.md)işleminin tamamlanması).

>[!IMPORTANT]
>Bu hızlı başlangıçtaki adımları test etmek için üretim dışı bir ortam kullanmanızı öneririz.

## <a name="configure-app-properties"></a>Uygulama özelliklerini yapılandırma

Azure AD kiracınıza bir uygulama eklemeyi bitirdiğinizde, bu sayfanın genel bakış sayfası hemen sunulur. Zaten eklenmiş olan bir uygulamayı yapılandırıyorsanız ilk hızlı başlangıca göz atın, kiracınıza eklenen uygulamaları görüntülemeyi adım adım gösterir. 

Uygulama özelliklerini düzenlemek için:

1. Azure AD portalında **Kurumsal uygulamalar** ' ı seçin ve ardından yapılandırmak istediğiniz uygulamayı bulun ve seçin.
2. Yönet bölümünde **Özellikler** ' i seçerek, düzenlenecek Özellikler bölmesini açın.
    ![Özellikler ekranını ve düzenlenebilir uygulama özelliklerini gösterir](media/add-application-portal/edit-properties.png)
3. Yapılandırmak için kullanılabilen seçenekleri anlamak için bir dakikanızı ayırın.
    - **Kullanıcıların oturum açması Için etkinleştirildi mi?** uygulamaya atanan kullanıcıların oturum açıp açamayacağını belirler.
    - **Kullanıcı Ataması gerekli mi?** uygulamaya atanmamış kullanıcıların oturum açıp açamayacağını belirler.
    - **Kullanıcılara görünebilir mi?** bir uygulamaya atanan kullanıcıların onu erişim panelinde ( https://myapps.microsoft.com) ve Office 365 veya Microsoft 365 web sitesinin sol üst kısmında bulunan waffle menüsü) göremeyeceğini belirler.
4. Gereksinimleriniz için en iyi seçenekleri seçmenize yardımcı olması için aşağıdaki tabloları kullanın.

   - **Atanan** kullanıcılar için davranış:

       | Uygulama özelliği | Uygulama özelliği | Uygulama özelliği | Atanan kullanıcı deneyimi | Atanan kullanıcı deneyimi |
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

       | Uygulama özelliği | Uygulama özelliği | Uygulama özelliği | Atanmayan kullanıcı deneyimi | Atanmayan kullanıcı deneyimi |
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
2. Azure AD portalında **Kurumsal uygulamalar** ' ı seçin ve ardından yapılandırmak istediğiniz uygulamayı bulun ve seçin.
3. Yönet bölümünde **Özellikler** ' i seçerek, düzenlenecek Özellikler bölmesini açın. 
4. Logoyu karşıya yüklemek için simgeyi seçin.
5. İşiniz bittiğinde **Kaydet**' i seçin. 
    ![Uygulamanın Özellikler sayfasından logonun nasıl değiştirileceğini gösterir](media/add-application-portal/change-logo.png)

   > [!NOTE]
   > Bu **Özellikler** bölmesinde görünen küçük resim, hemen güncelleştirmez. Güncelleştirilmiş simgesini görmek için özellikleri kapatıp yeniden açabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Artık bir uygulamanın özelliklerini yapılandırdığınıza göre, çoklu oturum açmayı ayarlamak için devam edebilirsiniz.

- [Çoklu oturum açmayı ayarlama](add-application-portal-setup-sso.md)
- [Uygulama silme](delete-application-portal.md)