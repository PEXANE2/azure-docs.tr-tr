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
ms.openlocfilehash: 1873cd14883e8b1a5174b8761e4a0bc9ba1ef872
ms.sourcegitcommit: 628be49d29421a638c8a479452d78ba1c9f7c8e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88641866"
---
# <a name="quickstart-configure-properties-for-an-application-in-your-azure-active-directory-azure-ad-tenant"></a>Hızlı başlangıç: Azure Active Directory (Azure AD) kiracınızdaki bir uygulama için özellikleri yapılandırma

Önceki hızlı başlangıçta, Azure Active Directory (Azure AD) kiracınıza bir uygulama eklediniz. Bir uygulama eklediğinizde, Azure AD kiracınıza uygulamanın kimlik sağlayıcısı olduğunu bildirmek için izin vermiş olursunuz. Artık uygulama için özelliklerden bazılarını yapılandıracaksınız.
 
## <a name="prerequisites"></a>Ön koşullar

Azure AD kiracınızdaki bir uygulamanın özelliklerini yapılandırmak için şunlar gerekir:

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Aşağıdaki rollerden biri: genel yönetici, bulut uygulaması Yöneticisi, uygulama Yöneticisi veya hizmet sorumlusu sahibi.
- İsteğe bağlı: [uygulamalarınızı görüntülemeyi](view-applications-portal.md)tamamlama.
- İsteğe bağlı: [uygulama ekleme](add-application-portal.md)işleminin tamamlanması.

>[!IMPORTANT]
>Bu hızlı başlangıçtaki adımları test etmek için üretim dışı bir ortam kullanın.

## <a name="configure-app-properties"></a>Uygulama özelliklerini yapılandırma

Azure AD kiracınıza bir uygulama eklemeyi bitirdiğinizde genel bakış sayfası görüntülenir. Zaten eklenmiş olan bir uygulamayı yapılandırıyorsanız ilk hızlı başlangıca bakın. Kiracınıza eklenen uygulamaları görüntülemeyi adım adım gösterir. 

Uygulama özelliklerini düzenlemek için:

1. Azure AD portalında **Kurumsal uygulamalar**' ı seçin. Ardından, yapılandırmak istediğiniz uygulamayı bulun ve seçin.
2. **Yönet** bölümünde **Özellikler** ' i seçerek, düzenlenecek **Özellikler** bölmesini açın.

    ![Düzenlenebilir uygulama özelliklerini gösteren Özellikler ekranının ekran görüntüsü.](media/add-application-portal/edit-properties.png)

3. Yapılandırmak için kullanılabilen seçenekleri anlamak için bir dakikanızı ayırın:
    - **Kullanıcıların oturum açması Için etkinleştirildi mi?** uygulamaya atanan kullanıcıların oturum açıp açamayacağını belirler.
    - **Kullanıcı Ataması gerekli mi?** uygulamaya atanmamış kullanıcıların oturum açıp açamayacağını belirler.
    - **Kullanıcılara görünebilir mi?** bir uygulamaya atanan kullanıcıların bu dosyayı [uygulamalarım](https://myapps.microsoft.com) ve Office 365 uygulama başlatıcısı 'nda göremeyeceğini belirler. (Office 365 veya Microsoft 365 web sitesinin sol üst köşesindeki waffle menüsüne bakın.)
    
    > [!TIP]
    > Kullanıcıları atamak, gezinme 'nin **Kullanıcılar ve gruplar** bölümünde gerçekleşir.

    Üç seçenek birbirinden bağımsız olarak değiştirilebilir ve sonuçta ortaya çıkan davranış her zaman belirgin değildir. Yardımcı olabilecek bir tablo aşağıda verilmiştir:
    
    | Kullanıcıların oturum açması için etkinleştirildi mi? | Kullanıcı ataması gerekli mi? | Kullanıcılara görünür mü? | Uygulamaya atanmış olan veya olmayan kullanıcılar için davranış. |
    |---|---|---|---|
    | Yes | Yes | Yes | Atanan kullanıcılar uygulamayı görebilir ve oturum açabilir.<br>Atanmamış kullanıcılar uygulamayı göremez ve oturum açılamıyor. |
    | Yes | Yes | Hayır  | Atanan kullanımlar uygulamayı göremez, ancak oturum açabilir.<br>Atanmamış kullanıcılar uygulamayı göremez ve oturum açılamıyor. |
    | Yes | Hayır  | Yes | Atanan kullanıcılar uygulamayı görebilir ve oturum açabilir.<br>Atanmamış kullanıcılar uygulamayı göremez, ancak oturum açabilir. |
    | Yes | Hayır  | Hayır  | Atanan kullanıcılar uygulamayı göremez, ancak oturum açabilir.<br>Atanmamış kullanıcılar uygulamayı göremez, ancak oturum açabilir. |
    | Hayır  | Yes | Yes | Atanan kullanıcılar uygulamayı göremez ve oturum açılamıyor.<br>Atanmamış kullanıcılar uygulamayı göremez ve oturum açılamıyor. |
    | Hayır  | Yes | Hayır  | Atanan kullanıcılar uygulamayı göremez ve oturum açılamıyor.<br>Atanmamış kullanıcılar uygulamayı göremez ve oturum açılamıyor. |
    | Hayır  | Hayır  | Yes | Atanan kullanıcılar uygulamayı göremez ve oturum açılamıyor.<br>Atanmamış kullanıcılar uygulamayı göremez ve oturum açılamıyor. |
    | Hayır  | Hayır  | Hayır  | Atanan kullanıcılar uygulamayı göremez ve oturum açılamıyor.<br>Atanmamış kullanıcılar uygulamayı göremez ve oturum açılamıyor. |

4. İşiniz bittiğinde **Kaydet**' i seçin.

## <a name="use-a-custom-logo"></a>Özel logo kullanma

Özel logo kullanmak için:

1. 215 215 piksel olan bir logo oluşturun ve. png biçiminde kaydedin.
2. Azure AD portalında **Kurumsal uygulamalar**' ı seçin. Ardından, yapılandırmak istediğiniz uygulamayı bulun ve seçin.
3. **Yönet** bölümünde **Özellikler** ' i seçerek, düzenlenecek **Özellikler** bölmesini açın. 
4. Logoyu karşıya yüklemek için simgeyi seçin.
5. İşiniz bittiğinde **Kaydet**' i seçin.

    ![Logo 'nin nasıl değiştirileceğini gösteren Özellikler ekranının ekran görüntüsü.](media/add-application-portal/change-logo.png)

   > [!NOTE]
   > Bu **Özellikler** bölmesinde görünen küçük resim, hemen güncelleştirmez. Güncelleştirilmiş simgesini görmek için **Özellikler** bölmesini kapatıp yeniden açabilirsiniz.


> [!TIP]
> Graph API kullanarak uygulama yönetimini otomatik hale getirebilirsiniz, bkz. [MICROSOFT Graph API ile uygulama yönetimini otomatikleştirme](https://docs.microsoft.com/graph/application-saml-sso-configure-api).


## <a name="clean-up-resources"></a>Kaynakları temizleme

Hızlı başlangıç serisine devam edemeyecekseniz, test kiracınızı temizlemek için uygulamayı silmeyi göz önünde bulundurun. Bu serinin son hızlı başlangıçta uygulamanın silinmesi, bkz. [uygulamayı silme](delete-application-portal.md).

## <a name="next-steps"></a>Sonraki adımlar

Bir uygulama için çoklu oturum açmayı ayarlamayı öğrenmek için bir sonraki makaleye ilerleyin.
> [!div class="nextstepaction"]
> [Çoklu oturum açmayı ayarlama](add-application-portal-setup-sso.md)
