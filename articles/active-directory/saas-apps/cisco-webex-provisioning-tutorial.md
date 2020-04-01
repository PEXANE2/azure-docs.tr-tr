---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Cisco Webex'i yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory'yi, kullanıcı hesaplarını Cisco Webex'e otomatik olarak sağlamak ve sağlamadan çıkarmak için nasıl yapılandırılamayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/12/2019
ms.author: zhchia
ms.openlocfilehash: 0075783c049e7f48645f768026dd9d5ec0ead821
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77058527"
---
# <a name="tutorial-configure-cisco-webex-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Cisco Webex'i yapılandırın

Bu öğreticinin amacı, Azure AD'yi otomatik olarak cisco webex'e sağlanacak ve kullanıcıları sağlamadan çıkaracak şekilde yapılandırmak için Cisco Webex ve Azure Active Directory 'de (Azure AD) gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.
>
> Bu bağlayıcı şu anda Önizleme'de. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için, [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracısı.
* [Bir Cisco Webex kiracı](https://www.webex.com/pricing/index.html).
* Cisco Webex'te Yönetici izinlerine sahip bir kullanıcı hesabı.

## <a name="adding-cisco-webex-from-the-gallery"></a>Galeriden Cisco Webex ekleme

Azure AD ile otomatik kullanıcı sağlama için Cisco Webex'i yapılandırmadan önce, Yönetilen SaaS uygulamaları listenize Azure AD uygulama galerisinden Cisco Webex eklemeniz gerekir.

**Azure AD uygulama galerisinden Cisco Webex eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda Cisco **Webex**yazın, sonuç panelinden **Cisco Webex'i** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![Sonuç listesinde Cisco Webex](common/search-new-app.png)

## <a name="assigning-users-to-cisco-webex"></a>Kullanıcıları Cisco Webex'e atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için "atamalar" adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya "atanmış" kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların Cisco Webex'e erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları cisco webex'e buradaki talimatları izleyerek atayabilirsiniz:

* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-cisco-webex"></a>Cisco Webex'e kullanıcı atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını sınamak için cisco Webex'e tek bir Azure AD kullanıcısı atanması önerilir. Ek kullanıcılar daha sonra atanabilir.

* Bir kullanıcıyı Cisco Webex'e atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="configuring-automatic-user-provisioning-to-cisco-webex"></a>Otomatik kullanıcı sağlamayı Cisco Webex'e yapılandırma

Bu bölüm, Azure AD'deki kullanıcı atamalarına dayalı olarak Cisco Webex'teki kullanıcıları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol gösterir.

### <a name="to-configure-automatic-user-provisioning-for-cisco-webex-in-azure-ad"></a>Azure AD'de Cisco Webex için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com) oturum açın ve **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin, ardından **Cisco Webex'i**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Cisco Webex'i**seçin.

    ![Uygulamalar listesindeki Cisco Webex bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Cisco Webex Sağlama](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Cisco Webex Sağlama](common/provisioning-automatic.png)

5. Yönetici **Kimlik Bilgileri** bölümü altında, Cisco Webex hesabınızın **Kiracı URL'sini**ve **Gizli Jetonunu** girdiniz.

    ![Cisco Webex Sağlama](./media/cisco-webex-provisioning-tutorial/secrettoken1.png)

6.  Kiracı **URL** alanına, `https://api.ciscoweb.com/v1/scim/[OrgId]`bir değer girin. Elde `[OrgId]`etmek için [Cisco Webex Denetim Merkezinizde](https://admin.webex.com/login)oturum açın. Sol alttaki kuruluş adınızı tıklatın ve **Kuruluş Kimliği'nden**değeri kopyalayın. 

    * **Gizli Belirteç**için değer elde etmek için bu [URL'ye](https://idbroker.webex.com/idb/saml2/jsp/doSSO.jsp?type=login&goto=https%3A%2F%2Fidbroker.webex.com%2Fidb%2Foauth2%2Fv1%2Fauthorize%3Fresponse_type%3Dtoken%26client_id%3DC4ca14fe00b0e51efb414ebd45aa88c1858c3bfb949b2405dba10b0ca4bc37402%26redirect_uri%3Dhttp%253A%252F%252Flocalhost%253A3000%252Fauth%252Fcode%26scope%3Dspark%253Apeople_read%2520spark%253Apeople_write%2520Identity%253ASCIM%26state%3Dthis-should-be-a-random-string-for-security-purpose)gidin. Görünen webex oturum açma sayfasından, kuruluşunuz için cisco webex yönetici hesabının tamamıyla oturum açın. Siteye ulaşılamadığını, ancak bu normal olduğunu belirten bir hata sayfası görüntülenir.

        ![Cisco Webex Sağlama](./media/cisco-webex-provisioning-tutorial/test.png)
 
    * Aşağıda vurgulandığı şekilde oluşturulan taşıyıcı belirteci değerini URL'den kopyalayın. Bu belirteç 365 gün için geçerlidir.
        
        ![Cisco Webex Sağlama](./media/cisco-webex-provisioning-tutorial/test1.png)

7. Adım 5'te gösterilen alanları doldurarak, Azure AD'nin Cisco Webex'e bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, Cisco Webex hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL + Belirteç](common/provisioning-testconnection-tenanturltoken.png)
   
8. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin - **Bir hata oluştuğunda e-posta bildirimi gönderin.**

    ![Bildirim E-postası](common/provisioning-notification-email.png)

9. **Kaydet**'e tıklayın.

10. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını Cisco Webex'e Senkronize Et'i**seçin.

    ![Cisco Webex Sağlama](./media/cisco-webex-provisioning-tutorial/usermapping.png)

11. Azure AD'den Cisco Webex'e eşitlenen kullanıcı özniteliklerini **Öznitelik Eşleme** bölümünde gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Cisco Webex'teki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Cisco Webex Sağlama](./media/cisco-webex-provisioning-tutorial/usermappingattributes.png)

12. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

13. Cisco Webex için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **Sağlama Durumunu** **Ayarı** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek Cisco Webex'e sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

15. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun süren ilk eşitlemenin gerçeklemi daha uzun sürer. Cisco Webex'teki Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan ilerlemeyi izlemek ve sağlama faaliyet raporuna bağlı bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="connector-limitations"></a>Konektör sınırlamaları

* Cisco Webex şu anda Cisco'nun Erken Alan Testi (EFT) aşamasındadır. Daha fazla bilgi için lütfen [Cisco'nun destek ekibine](https://www.webex.co.in/support/support-overview.html)başvurun. 
* Cisco Webex yapılandırması hakkında daha fazla bilgi için [cisco](https://help.webex.com/en-us/aumpbz/Synchronize-Azure-Active-Directory-Users-into-cisco-webex-Control-Hub)belgelerine buradan bakın.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)