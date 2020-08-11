---
title: 'Öğretici: Mitel Connect ile Azure Active Directory Tümleştirmesi | Microsoft Docs'
description: Azure Active Directory ve Mitel Connect arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 204f540b-09f1-452b-a52f-78143710ef76
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 07/31/2020
ms.author: jeedes
ms.openlocfilehash: 60e81bed348289f560b6341398bf4fe9bdaff0eb
ms.sourcegitcommit: 269da970ef8d6fab1e0a5c1a781e4e550ffd2c55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2020
ms.locfileid: "88054866"
---
# <a name="tutorial-azure-active-directory-integration-with-mitel-micloud-connect-or-cloudlink-platform"></a>Öğretici: Mitel Micyüksek Connect veya CloudLink platformuyla Azure Active Directory tümleştirme

Bu öğreticide, Mitel Connect uygulamasının, Mitel Micyüksek Connect veya CloudLink platformuyla Azure Active Directory (Azure AD) bütünleştirmek için nasıl kullanılacağını öğreneceksiniz. Mitel Connect uygulaması Azure Galerisi 'nde kullanılabilir. Azure AD 'yi Micyüksek Connect veya CloudLink platformuyla tümleştirmek aşağıdaki avantajları sağlar:

* Kullanıcıların, kurumsal kimlik bilgilerini kullanarak uygulamalara, Azure AD 'de uygulama bağlama ve şirket yoluyla bağlantı uygulamaları için erişim denetimi yapabilirsiniz.
* Hesabınızdaki kullanıcıların Azure AD hesaplarını kullanarak, Micyüksek Connect veya CloudLink (çoklu oturum açma) için otomatik olarak oturum açmasını sağlayabilirsiniz.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

Azure aboneliğiniz yoksa, Mitel Micyüksek Connect veya CloudLink platformuyla Azure AD tümleştirmesi başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/) .

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesini, Micyüksek Connect ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Bir Azure AD aboneliği. Azure AD ortamınız yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Yapılandırmak istediğiniz uygulamaya bağlı olarak bir Mitel Micyüksek Connect hesabı veya Mitel CloudLink hesabı.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide Azure AD çoklu oturum açma 'yı (SSO) yapılandırıp test edeceksiniz.

* Mitel Connect, **SP** tarafından başlatılan SSO 'yu destekler
* Mitel Connect 'i yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve zaman korumasını koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletiliyor. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="add-mitel-connect-from-the-gallery"></a>Galeriden Mitel Connect ekleme

Mitel Connect 'in Azure AD 'ye tümleştirilmesini yapılandırmak için galerideki Mitel Connect 'i Azure portal yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory**' i seçin.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. **Yeni uygulama**’yı seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama alanına **Mitel Connect** yazın, sonuçlar panelinden **Mitel Connect** ' i seçin ve ardından **Ekle**' yi seçin.

     ![Sonuç listesinde Mitel bağlantısı](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, **_Britta Simon_** adlı bir test kullanıcısına bağlı olarak, Micyüksek Connect veya CloudLink platformuyla Azure AD SSO 'yu yapılandırıp test edersiniz. Çoklu oturum açma için Azure AD portalındaki Kullanıcı ile Mitel platformunda buna karşılık gelen kullanıcı arasında bir bağlantı kurulması gerekir. Azure AD SSO 'yu Micyüksek Connect veya CloudLink platformuyla yapılandırma ve test etme hakkında bilgi edinmek için aşağıdaki bölümlere bakın.
* Azure AD SSO 'yu, Micyüksek Connect ile yapılandırma ve test etme
* Azure AD SSO 'yu CloudLink platformuyla yapılandırma ve test etme

## <a name="configure-and-test-azure-ad-sso-with-micloud-connect"></a>Azure AD SSO 'yu, Micyüksek Connect ile yapılandırma ve test etme

Azure AD çoklu oturum açmayı, Micyüksek Connect ile yapılandırmak ve test etmek için:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak ve uygulama tarafında SSO ayarlarını yapılandırmak için **[Azure AD Ile SSO Için Micyüksek Connect 'ı yapılandırın](#configure-micloud-connect-for-sso-with-azure-ad)** .
2. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
3. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
4. Kullanıcının Azure AD gösterimine bağlı olan, Mictta SIG Connect hesabınızda Britta Simon 'ın bir karşılığı olacak şekilde **[bir Mitel Micyüksek Connect test kullanıcısı oluşturun](#create-a-mitel-micloud-connect-test-user)** .
5. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı sınayın](#test-single-sign-on)** .

## <a name="configure-micloud-connect-for-sso-with-azure-ad"></a>Azure AD ile SSO için Micyüksek Connect 'i yapılandırma

Bu bölümde, Azure portal Micyüksek Connect için Azure AD çoklu oturum açma 'yı etkinleştireceksiniz ve Azure AD 'yi kullanarak, Micyüksek Connect hesabınızı SSO 'ya izin verecek şekilde yapılandıracaksınız.

Azure AD için aynı SSO ile Micyüksek Connect 'i yapılandırmak için Azure portal ve Mitel hesabı portalını yan yana açmak en kolay yoldur. Azure portal bazı bilgileri Mitel Hesap portalına ve bir kısmını Mitel hesap portalından Azure portal kopyalamanız gerekir.


1. [Azure Portal](https://portal.azure.com/)yapılandırma sayfasını açmak için:

    1. **Mitel Connect** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

       ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

    1. **Çoklu oturum açma yöntemi seç** Iletişim kutusunda **SAML**' yi seçin.
    
       ![Çoklu oturum açma seçme modu](common/select-saml-option.png)
    
       SAML tabanlı oturum açma sayfası görüntülenir.

2. Mitel hesap portalındaki yapılandırma iletişim kutusunu açmak için:

    1. **Telefon sistemi** menüsünde, **eklenti özellikleri**' ni seçin.

    1. **Çoklu oturum açma**'Nın sağında **Etkinleştir** veya **Ayarlar**' ı seçin.
    
    Çoklu oturum açma ayarlarını bağla iletişim kutusu görüntülenir.
    
3. **Çoklu oturum açmayı etkinleştir** onay kutusunu seçin.
    
    ![Mitel Connect çoklu oturum açma ayarları sayfasını gösteren ve çoklu oturum açmayı etkinleştir onay kutusunun seçili olduğu ekran görüntüsü.](./media/mitel-connect-tutorial/mitel-connect-enable.png)

4. Azure portal, **temel SAML yapılandırması** bölümünde **Düzenle** simgesini seçin.
   
    ![image](common/edit-urls.png)

    Temel SAML yapılandırması iletişim kutusu görüntülenir.

5.  Mitel hesap portalındaki **Mitel tanımlayıcı (VARLıK kimliği)** alanından URL 'yi kopyalayın ve Azure Portal ' de **tanımlayıcı (varlık kimliği)** alanına yapıştırın.

6. URL 'YI, Mitel hesap portalındaki **yanıt URL 'si (onaylama Işlemi tüketici hizmeti URL 'si)** alanından kopyalayın ve Azure Portal, **yanıt URL 'Si (onaylama tüketici hizmeti URL 'si)** alanına yapıştırın.

   ![image](./media/mitel-connect-tutorial/mitel-azure-basic-configuration.png)

7. **Oturum açma URL 'si** metin kutusuna aşağıdaki URL 'lerden birini yazın:

    1. **https://portal.shoretelsky.com**-Mitel hesabı portalını varsayılan Mitel uygulamanız olarak kullanmak için
    1. **https://teamwork.shoretel.com**-Teamwork uygulamasını varsayılan Mitel uygulamanız olarak kullanmak için

    > [!NOTE]
    > Varsayılan Mitel uygulaması, bir Kullanıcı erişim panelinde Mitel Connect kutucuğunu seçtiğinde erişilen uygulamadır. Bu Ayrıca, Azure AD 'den bir test kurulumu gerçekleştirirken erişilen uygulamadır.

8. Azure portal **temel SAML yapılandırması** Iletişim kutusunda **Kaydet** ' i seçin.

9. Azure portal SAML **tabanlı oturum açma** sayfasındaki SAML **imzalama sertifikası** bölümünde, **imzalama sertifikasını** indirmek ve bilgisayarınıza kaydetmek Için **sertifika (base64)** yanındaki **İndir** ' i seçin.

    ![image](./media/mitel-connect-tutorial/azure-signing-certificate.png)

10. Imza sertifikası dosyasını bir metin düzenleyicisinde açın, dosyadaki tüm verileri kopyalayın ve ardından Mitel hesabı portalındaki **Imza sertifikası** alanından verileri yapıştırın. 

      ![image](./media/mitel-connect-tutorial/mitel-connect-signing-certificate.png)

11. Azure portal **SAML tabanlı oturum açma** sayfasının **Setup Mitel Connect** bölümünde:

     1. **Oturum açma URL** 'si alanından URL 'yi kopyalayın ve Mitel hesabı portalındaki **oturum açma URL 'si** alanına yapıştırın.

     1. **Azure AD tanımlayıcı** alanından URL 'yi kopyalayın ve Mitel hesabı PORTALıNDAKI **varlık kimliği** alanına yapıştırın.
         
         ![image](./media/mitel-connect-tutorial/mitel-azure-set-up-connect.png)

12. Mitel hesap portalındaki **tek oturum açma ayarlarını bağla** Iletişim kutusunda **Kaydet** ' i seçin.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Bu bölümde, Azure portal Britta Simon adlı bir test kullanıcısı oluşturacaksınız.

1. Azure portal, sol bölmedeki **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.

    !["Kullanıcılar ve gruplar" ve "tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı Özellikleri iletişim kutusunda, aşağıdaki adımları uygulayın:

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    1. **Ad** alanına **Brittasıon**yazın.
  
    1. **Kullanıcı adı** alanına brittasimon@ yazın \<yourcompanydomain\> . \<extension\>  Örneğin, BrittaSimon@contoso.com.

    1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.

    1. **Oluştur**’u seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Mitel Connect 'e erişim vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **Mitel Connect**' i seçin.

    ![Uygulamalar listesindeki Mitel Connect bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** Iletişim kutusunda **Kullanıcılar** listesinde **Britta Simon** ' ı seçin ve ardından ekranın alt kısmında **Seç** ' i seçin.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusundaki listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmında **Seç** ' i seçin.

7. **Atama Ekle** Iletişim kutusunda **ata**' yı seçin.

### <a name="create-a-mitel-micloud-connect-test-user"></a>Mitel Micyüksek Connect test kullanıcısı oluşturma

Bu bölümde, Mictta Simon adlı bir kullanıcıyı, Micyüksek bağlantı hesabınızda oluşturursunuz. Çoklu oturum açma kullanılmadan önce kullanıcıların oluşturulması ve etkinleştirilmesi gerekir.

Mitel Hesap portalına Kullanıcı ekleme hakkında daha fazla bilgi için Mitel Bilgi Bankası 'nda [Kullanıcı ekleme](https://oneview.mitel.com/s/article/Adding-a-User-092815) makalesine bakın.

Aşağıdaki ayrıntılarla, Micyüksek Connect hesabınızda bir kullanıcı oluşturun:

* **Ad:** Britta Simon
* **Iş e-posta adresi:**`brittasimon@<yourcompanydomain>.<extension>`   
  (Örnek: [brittasimon@contoso.com](mailto:brittasimon@contoso.com) )
* **Kullanıcı adı:**`brittasimon@<yourcompanydomain>.<extension>`  
  (Örnek: [brittasimon@contoso.com](mailto:brittasimon@contoso.com) ; kullanıcının Kullanıcı adı genellikle kullanıcının iş e-posta adresiyle aynıdır)

> [!NOTE]
> Kullanıcının Micyüksek Connect Kullanıcı adı, kullanıcının Azure 'daki e-posta adresiyle aynı olmalıdır.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edeceksiniz.

Erişim panelinde Mitel Connect kutucuğunu seçtiğinizde, **oturum açma URL 'si** alanında varsayılan olarak yapılandırdığınız Micyüksek Connect uygulamasında oturum açmak için otomatik olarak yeniden yönlendirilmelisiniz. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="configure-and-test-azure-ad-sso-with-cloudlink-platform"></a>Azure AD SSO 'yu CloudLink platformuyla yapılandırma ve test etme

Bu bölümde, Azure portal CloudLink platformu için Azure AD SSO 'yu etkinleştirme ve CloudLink platform hesabınızı Azure AD kullanarak çoklu oturum açmaya izin verecek şekilde yapılandırma açıklanmaktadır.

CloudLink platformunu Azure AD için çoklu oturum açma ile yapılandırmak için, Azure portal bazı bilgileri CloudLink accounts portalına kopyalamanız ve bunun tersini yapmanız gerekeceğinden Azure portal ve CloudLink hesapları portalını yan yana açmanız önerilir.

1. [Azure Portal](https://portal.azure.com/)yapılandırma sayfasını açmak için:

    1. **Mitel Connect** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

       ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

    1. **Çoklu oturum açma yöntemi seç** Iletişim kutusunda **SAML**' yi seçin.

       ![Çoklu oturum açma seçme modu](common/select-saml-option.png)
    
       **SAML tabanlı oturum açma** sayfası AÇıLARAK **temel SAML yapılandırması** bölümünü görüntüler.

       ![image](./media/mitel-connect-tutorial/mitel-azure-saml-settings.png)

2. CloudLink hesapları portalındaki **Azure AD çoklu oturum açma** yapılandırma paneline erişmek için:

    1. Tümleştirmeyi etkinleştirmek istediğiniz müşteri hesabının **hesap bilgileri** sayfasına gidin.

    1. **Tümleştirmeler** bölümünde **+ Yeni Ekle**' yi seçin. Bir açılır ekran, **tümleştirmeler** panelini görüntüler.

    1. 3. **taraf** sekmesini seçin. Desteklenen üçüncü taraf uygulamaların listesi görüntülenir. **Azure AD çoklu oturum açma**Ile ilişkili **Ekle** düğmesini seçin ve **bitti**' yi seçin.

       ![image](./media/mitel-connect-tutorial/mitel-cloudlink-integrations.png)

       **Azure AD çoklu oturum açma** , müşteri hesabı için etkinleştirilmiştir ve **hesap bilgileri** sayfasının **tümleştirmeler** bölümüne eklenir.   

   1. **Kurulumu Tamam**' ı seçin.
    
      ![image](./media/mitel-connect-tutorial/mitel-cloudlink-complete-setup.png)
      
      **Azure AD çoklu oturum açma** yapılandırma paneli açılır.
      
       ![image](./media/mitel-connect-tutorial/mitel-cloudlink-sso-setup.png)
       
       Mitel, **Isteğe bağlı Mitel kimlik bilgileri** bölümünde **Mitel kimlik bilgilerini etkinleştir (isteğe bağlı)** onay kutusunun seçili olmadığından emin olmanızı önerir. Yalnızca kullanıcının CloudLink uygulamasında çoklu oturum açma seçeneğine ek olarak Mitel kimlik bilgilerini kullanarak oturum açmasını istiyorsanız bu onay kutusunu işaretleyin.

3. Azure portal, **SAML tabanlı oturum açma** sayfasında, **temel SAML yapılandırması** bölümünde **Düzenle** simgesini seçin. **Temel SAML yapılandırma** paneli açılır.

    ![image](./media/mitel-connect-tutorial/mitel-azure-saml-basic.png)
 
 4. CloudLink accounts portalındaki **Mitel tanımlayıcı (VARLıK kimliği)** alanından URL 'yi kopyalayın ve Azure Portal içindeki **tanımlayıcı (varlık kimliği)** alanına yapıştırın.

 5. CloudLink accounts portalındaki **yanıt URL 'si (onaylama tüketici hizmeti URL 'si)** alanından URL 'yi kopyalayın ve Azure Portal, **yanıt URL 'Si (onaylama tüketici hizmeti URL 'si)** alanına yapıştırın.  
    
    ![image](./media/mitel-connect-tutorial/mitel-cloudlink-saml-mapping.png) 

 6. **Oturum açma URL 'si** metin kutusunda, `https://accounts.mitel.io` CloudLink accounts portalını varsayılan Mitel uygulamanız olarak kullanmak için URL 'yi yazın.
     
     ![image](./media/mitel-connect-tutorial/mitel-cloudlink-sign-on-url.png)
  
     > [!NOTE]
     > Varsayılan Mitel uygulaması, bir Kullanıcı erişim panelinde Mitel Connect kutucuğunu seçtiğinde açılan uygulamadır. Bu, Kullanıcı Azure AD 'den bir test kurulumu yapılandırdığında de uygulamaya erişilir.

7. **Temel SAML yapılandırması** Iletişim kutusunda **Kaydet** ' i seçin.

8. Azure portal SAML **tabanlı oturum açma** sayfasındaki **SAML Imzalama sertifikası** bölümünde **imzalama sertifikasını**indirmek Için **sertifika (base64)** yanındaki **İndir** ' i seçin. Sertifikayı bilgisayarınıza kaydedin.
  
    ![image](./media/mitel-connect-tutorial/mitel-cloudlink-save-certificate.png)

9. Imza sertifikası dosyasını bir metin düzenleyicisinde açın, dosyadaki tüm verileri kopyalayın ve ardından verileri CloudLink accounts portalındaki **Imza sertifikası** alanına yapıştırın.  

    > [!NOTE]
    > Birden fazla sertifikanız varsa, bunları diğerinin ardına yapıştırmanız önerilir. 
       
    ![image](./media/mitel-connect-tutorial/mitel-cloudlink-enter-certificate.png)

10. Azure portal **SAML tabanlı oturum açma** sayfasındaki **Mitel Connect** ' i ayarla bölümünde:

     1. **Oturum açma URL** 'si alanından URL 'yi kopyalayın ve CloudLink accounts portalındaki **oturum açma URL 'si** alanına yapıştırın.

     1. **Azure AD tanımlayıcı** alanından URL 'yi kopyalayın ve CloudLink accounts portalındaki **IDP tanımlayıcı (varlık kimliği)** alanına yapıştırın.
     
        ![image](./media/mitel-connect-tutorial/mitel-cloudlink-copy-settings.png)

11. CloudLink accounts portalındaki **Azure AD çoklu oturum açma** panelinde **Kaydet** ' i seçin.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Bu bölümde, Azure portal Britta Simon adlı bir test kullanıcısı oluşturacaksınız.

1. Azure portal, sol bölmedeki **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.

    !["Kullanıcılar ve gruplar" ve "tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı Özellikleri iletişim kutusunda, aşağıdaki adımları uygulayın:

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    1. **Ad** alanına **Brittasıon**yazın.
  
    1. **Kullanıcı adı** alanına brittasimon@ yazın \<yourcompanydomain\> . \<extension\>  Örneğin, BrittaSimon@contoso.com.

    1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.

    1. **Oluştur**’u seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Mitel Connect 'e erişim vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **Mitel Connect**' i seçin.

    ![Uygulamalar listesindeki Mitel Connect bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** Iletişim kutusunda **Kullanıcılar** listesinde **Britta Simon** ' ı seçin ve ardından ekranın alt kısmında **Seç** ' i seçin.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusundaki listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmında **Seç** ' i seçin.

7. **Atama Ekle** Iletişim kutusunda **ata**' yı seçin.

### <a name="create-a-cloudlink-test-user"></a>CloudLink test kullanıcısı oluşturma

Bu bölümde, CloudLink platformunda **_Britta Simon_** adlı bir test kullanıcısının nasıl oluşturulacağı açıklanmaktadır. Kullanıcıların çoklu oturum açma kullanabilmesi için önce oluşturulması ve etkinleştirilmesi gerekir.

CloudLink hesapları portalında Kullanıcı ekleme hakkında daha fazla bilgi için bkz. [CloudLink accounts belgelerindeki](https://www.mitel.com/document-center/technology/cloudlink/all-releases/en/cloudlink-accounts-html) **_kullanıcıları yönetme_** .

Aşağıdaki ayrıntılarla CloudLink accounts portalınızda bir kullanıcı oluşturun:

* Ad: Britta Simon
* Ad: Britta
* Soyadı: Simon
* E-posta: BrittaSimon@contoso.com

> [!NOTE]
> Kullanıcının CloudLink e-posta adresi, Azure portal **Kullanıcı asıl adı** ile aynı olmalıdır.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD SSO yapılandırmanızı test edeceksiniz.

Erişim panelinde Mitel Connect kutucuğunu seçtiğinizde, **oturum açma URL 'si** alanında varsayılan olarak yapılandırdığınız CloudLink uygulamasında oturum açmak için otomatik olarak yönlendirilirsiniz. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)