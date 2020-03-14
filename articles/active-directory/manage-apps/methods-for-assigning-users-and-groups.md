---
title: Kullanıcıları ve grupları bir uygulamaya atama | Microsoft Docs
description: Erişim vermek için uygulamaya kullanıcı atama
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/26/2019
ms.author: mimart
ms.collection: M365-identity-device-management
ms.openlocfilehash: abd6b13dc56f8f948d50e2b3564712ed8f5b1476
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79253396"
---
# <a name="assign-users-and-groups-to-an-application-in-azure-active-directory"></a>Kullanıcıları ve grupları Azure Active Directory'de bir uygulamaya atama
Bu makalede bir uygulamayı Azure Active Directory (Azure AD) kullanıcıları veya grupları atama gösterilmektedir. Yöneticinin bunları aşağıdakileri yapmak için erişim izni verebilmeniz için önce kullanıcıların önce uygulamaya atanması gerekir:

-   **UYGULAMANıN URL 'sine doğrudan** (SP tarafından başlatılan oturum açma olarak da bilinir) giderek uygulamaya erişin.

-   Uygulamanın **Özellikler** sayfasında (IDP tarafından başlatılan oturum açma olarak da bilinir) **Kullanıcı erişim URL** 'sini kullanarak bir uygulamaya erişin.

-   Uygulama [erişim panelinde](https://myapps.microsoft.com/) veya mobil uygulamada bir uygulamanın göründüğünü görün.

-   Bir uygulamanın [Office 365 uygulama başlatıcısı](https://support.office.com/article/Meet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a)'nda göründüğünü görün.

Grup tabanlı atamanın kullanılabilirliği lisans sözleşmeniz tarafından belirlenir. Grup tabanlı atama yalnızca güvenlik grupları için desteklenir. İç içe grup üyelikleri ve O365 grupları Şu anda desteklenmemektedir.

## <a name="configure-the-application-to-require-assignment"></a>Uygulamayı atama gerektirecek şekilde yapılandırma

Bir uygulama, erişilebilmesi için önce atama gerektirecek şekilde yapılandırılabilir. Atama gerektirmek için:

1. Azure portal, bir yönetici hesabıyla veya **Kurumsal uygulamalar**altında uygulamanın sahibi olarak oturum açın.
2. Ana menüdeki **tüm hizmetler** öğesine tıklayın.
3. Uygulama için kullandığınız dizine seçin.
4. **Kurumsal uygulamalar** sekmesine tıklayın.
5. Bu dizin ile ilişkili uygulamalar listesinden uygulamayı seçin.
6. **Özellikler** sekmesine tıklayın.
7. **Gerekli Kullanıcı atamasını değiştirin mi?** Evet 'e geçin.
8. Ekranın üst kısmındaki **Kaydet** düğmesine tıklayın.

## <a name="assign-users"></a>Kullanıcıları atama

Bir veya daha fazla kullanıcıları uygulamaya doğrudan atamak için aşağıdaki adımları izleyin:

1.  [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici olarak ya da yönetici olmayan bir uygulama sahibi olarak** oturum açın.

2.  Ana sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3.  Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4.  Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' a tıklayın.

5.  tüm uygulamalarınızın listesini görüntülemek için **tüm uygulamalar** ' a tıklayın.

    * Burada görünmesini istediğiniz uygulamayı görmüyorsanız, **tüm uygulamalar listesinin** en üstündeki **filtre** denetimini kullanın ve **göster** seçeneğini **tüm uygulamalar** olarak ayarlayın.

6.  Listeden bir kullanıcıya atamak istediğiniz uygulamayı seçin.

7.  Uygulama yüklendikten sonra, uygulamanın sol taraftaki gezinti menüsünden **Kullanıcılar ve gruplar** ' a tıklayın.

8.  **Atama Ekle** bölmesini açmak için **Kullanıcılar ve gruplar** listesinin üstündeki **Ekle** düğmesine tıklayın.

9.  **atama Ekle** bölmesinden **Kullanıcılar ve gruplar** seçicisini tıklatın.

10. **Ada veya e-posta adresine göre ara** arama kutusuna, ilgilendiğiniz kullanıcının **tam adını** veya **e-posta adresini** yazın.

11. Bir **onay kutusunu**açığa çıkarmak için listedeki **kullanıcının** üzerine gelin. Kullanıcıyı **Seçili** listeye eklemek için kullanıcının profil fotoğrafı veya logosu yanındaki onay kutusuna tıklayın.

12. **Isteğe bağlı:** Birden **fazla kullanıcı eklemek**istiyorsanız **ada veya e-posta adresine göre ara** kutusuna başka bir **tam ad** veya **e-posta adresi** yazın ve bu kullanıcıyı **Seçili** listeye eklemek için onay kutusuna tıklayın.

13. Kullanıcıları seçmeyi tamamladığınızda, uygulamayı atanacak kullanıcılar ve gruplar listesine eklemek için **Seç** düğmesine tıklayın.

14. **Isteğe bağlı:** seçtiğiniz kullanıcılara atanacak bir rol seçmek Için **atama Ekle** bölmesinde **rol seçicisini Seç** ' e tıklayın.

15. Uygulamayı seçili kullanıcılara atamak için **ata** düğmesine tıklayın.

Kısa bir süre sonra, seçtiğiniz kullanıcılar çözüm açıklaması bölümünde açıklanan yöntemleri kullanarak bu uygulamaları başlatabilecektir.

## <a name="assign-groups"></a>Grupları atama

Bir veya daha fazla grup, doğrudan uygulamaya atamak için aşağıdaki adımları izleyin:

1.  [**Azure Portal**](https://portal.azure.com/) açın ve bir **genel yönetici** olarak veya bir Azure AD Premium lisansı atanmış yönetici olmayan bir uygulama sahibi olarak oturum açın.

2.  Ana sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3.  Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4.  Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' a tıklayın.

5.  tüm uygulamalarınızın listesini görüntülemek için **tüm uygulamalar** ' a tıklayın.

    * Burada görünmesini istediğiniz uygulamayı görmüyorsanız, **tüm uygulamalar listesinin** en üstündeki **filtre** denetimini kullanın ve **göster** seçeneğini **tüm uygulamalar** olarak ayarlayın.

6.  Listeden bir kullanıcıya atamak istediğiniz uygulamayı seçin.

7.  Uygulama yüklendikten sonra, uygulamanın sol taraftaki gezinti menüsünden **Kullanıcılar ve gruplar** ' a tıklayın.

8.  **Atama Ekle** bölmesini açmak için **Kullanıcılar ve gruplar** listesinin üstündeki **Ekle** düğmesine tıklayın.

9.  **atama Ekle** bölmesinden **Kullanıcılar ve gruplar** seçicisini tıklatın.

10. **Ada veya e-posta adresine göre ara** arama kutusuna, ilgilendiğiniz grubun **tam grup adını** yazın.

11. Bir **onay kutusunu**açığa çıkarmak için listedeki **grubun** üzerine gelin. Kullanıcıyı **Seçili** listeye eklemek için grubun profil fotoğrafı veya logosu yanındaki onay kutusuna tıklayın.

12. **Isteğe bağlı:** Birden **fazla grup eklemek**istiyorsanız **ada veya e-posta adresine göre ara** kutusuna başka bir **tam grup adı** yazın ve bu grubu **Seçili** listeye eklemek için onay kutusuna tıklayın.

13. Grupları seçmeyi tamamladığınızda, uygulamayı atanacak kullanıcılar ve gruplar listesine eklemek için **Seç** düğmesine tıklayın.

14. **Isteğe bağlı:** seçtiğiniz gruplara atanacak bir rol seçmek Için **atama Ekle** bölmesinde **rol seçicisini Seç** ' e tıklayın.

15. Uygulamayı seçili gruplara atamak için **ata** düğmesine tıklayın.

Kısa bir süre sonra, seçtiğiniz gruplar içindeki kullanıcılar, çözüm açıklaması bölümünde açıklanan yöntemleri kullanarak bu uygulamaları başlatabilir. Dinamik gruplar bunlar, bazı ek işleme gecikme olabilir, kullanıcıların bu atanan gruplar içinde görünen bu atamaları.

## <a name="enable-self-service-application-access"></a>Self Servis uygulama erişimini etkinleştir

Self Servis uygulama erişiminin kullanıcıların uygulamaları kendi kendine bulmak harika bir yoludur bu uygulamalara erişimi onaylamak için iş grubuna isteğe bağlı olarak sağlar. Parola çoklu oturum uygulamalar'ın üzerinde sağ için kullanıcılara erişim panelleri üzerinden atanan kimlik bilgilerini yönetmek iş grubuna izin verebilirsiniz.

Bir uygulamaya Self Servis uygulama erişimini etkinleştirmek için aşağıdaki adımları izleyin:

1. [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** olarak oturum açın.

2. Ana sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3. Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4. Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' a tıklayın.

5. tüm uygulamalarınızın listesini görüntülemek için **tüm uygulamalar** ' a tıklayın.

   * Burada görünmesini istediğiniz uygulamayı görmüyorsanız, **tüm uygulamalar listesinin** en üstündeki **filtre** denetimini kullanın ve **göster** seçeneğini **tüm uygulamalar** olarak ayarlayın.

6. Self Servis etkinleştirmek istediğiniz uygulamayı seçin listeden erişim.

7. Uygulama yüklendikten sonra, uygulamanın sol taraftaki gezinti menüsünden **Self-Service** ' e tıklayın.

8. Bu uygulama için self servis uygulama erişimini etkinleştirmek üzere **kullanıcıların bu uygulamaya erişim Istemesine Izin ver** ' i açın **. Evet** ' e geçin.

9. Ardından, bu uygulamaya erişim isteyen kullanıcıların eklenmesi gereken grubu seçmek için, **grubun atanması** gereken etiketin yanındaki seçiciye tıklayın ve bir grup seçin.

10. **Isteğe bağlı:** Kullanıcıların erişimine izin verilmeden önce bir iş onayı gerektirmek istiyorsanız, **Bu uygulamaya erişim izni vermeden önce onay iste** seçeneğini belirleyin. **Evet**' e geçin.

11. **Isteğe bağlı: yalnızca parola çoklu oturum açma kullanan uygulamalar için,** bu iş onaylayanlara onaylanan kullanıcılar için bu uygulamaya gönderilen parolaları belirtmesini sağlamak istiyorsanız, **onaylayanlara bu uygulama için Kullanıcı parolalarını ayarlamaya izin ver** ' i ayarlayın. **Evet**' e geçin.

12. **Isteğe bağlı:** Bu uygulamaya erişimi onaylamaya izin verilen iş onaylayanlarını belirtmek için, **Bu uygulamaya erişimi onaylamaya izin verilen** etiketin yanındaki seçiciye tıklayın. en fazla 10 bireysel iş onaylayı seçmek için.

    >[!NOTE]
    >Grupları desteklenmez.
    >
    >

13. **Isteğe bağlı:** **rolleri kullanıma sunan uygulamalar için**, bir role self servis onaylı kullanıcılar atamak istiyorsanız, **Bu uygulamada hangi rolün hangi rolün atanması gerektiğini seçin?** seçeneğinin yanındaki seçiciye tıklayın. bu kullanıcıların atanması gereken rolü seçin.

14. Son olarak bölmenin en üstündeki **Kaydet** düğmesine tıklayın.

Self Servis uygulama yapılandırmasını tamamladıktan sonra, kullanıcılar kendi [uygulama erişim paneline](https://myapps.microsoft.com/) gidebilir ve self servis erişimini etkinleştirdiğiniz uygulamaları bulmak Için **+ Ekle** düğmesine tıklamanıza izin verebilir. İş onaylayanları Ayrıca [uygulama erişimi panelinde](https://myapps.microsoft.com/)bir bildirim görür. Kullanıcı onay gerektiren bir uygulama için erişim istedi gelmeyeceğini e-posta etkinleştirebilirsiniz. 

Bu onaylar yalnızca tek onay iş akışlarını destekler, yani birden çok onaylayan belirtirseniz, tek bir onaylayanın uygulamaya erişimi onaylayabileceği anlamına gelir.

## <a name="next-steps"></a>Sonraki adımlar
[Uygulama proxy 'Si ile uygulamalarınıza çoklu oturum açma sağlama](application-proxy-configure-single-sign-on-with-kcd.md)
