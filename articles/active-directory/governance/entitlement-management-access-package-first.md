---
title: Öğretici-Azure AD Yetkilendirme Yönetimi 'nde (Önizleme) ilk erişim paketinizi oluşturun-Azure Active Directory
description: Azure Active Directory yetkilendirme yönetimi 'nde (Önizleme) ilk erişim paketinizi oluşturma konusunda adım adım öğretici.
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.subservice: compliance
ms.date: 07/23/2019
ms.author: ajburnle
ms.reviewer: markwahl-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: 76ba284ec1a30322a24c762a1829b399f2583c6c
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69032914"
---
# <a name="tutorial-create-your-first-access-package-in-azure-ad-entitlement-management-preview"></a>Öğretici: Azure AD Yetkilendirme Yönetimi 'nde (Önizleme) ilk erişim paketinizi oluşturma

> [!IMPORTANT]
> Azure Active Directory (Azure AD) yetkilendirme yönetimi şu anda genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Gruplar, uygulamalar ve siteler gibi çalışanların ihtiyaç duyduğu tüm kaynaklara erişimi yönetmek, kuruluşlar için önemli bir işlevdir. Çalışanlara, üretken olmaları gereken doğru erişim düzeyini vermek ve artık gerekli olmadığında erişimleri kaldırmak istiyorsunuz.

Bu öğreticide, Woodgrove Bank for It Administrator olarak çalışırsınız. Bir Web projesi için, iç kullanıcıların Self Servis isteğine yönelik bir kaynak paketi oluşturmanız istendi. İstekler onay gerektirir ve kullanıcının erişimi 30 gün sonra dolar. Bu öğreticide, Web proje kaynakları yalnızca tek bir gruba üyedir, ancak gruplar, uygulamalar veya SharePoint Online siteleri koleksiyonu olabilir.

![Senaryoya genel bakış](./media/entitlement-management-access-package-first/elm-scenario-overview.png)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Kaynak olarak grupla bir erişim paketi oluşturma
> * Onaylayan belirleyin
> * Dahili bir kullanıcının erişim paketini nasıl isteyebileceğini gösterir
> * Erişim isteğini onaylama

İlk erişim paketinizi oluşturma da dahil olmak üzere Azure Active Directory yetkilendirme yönetimi dağıtma işleminin adım adım bir gösterimi için aşağıdaki videoyu görüntüleyin:

>[!VIDEO https://www.youtube.com/embed/zaaKvaaYwI4]

## <a name="prerequisites"></a>Önkoşullar

Azure AD Yetkilendirme Yönetimi 'ni (Önizleme) kullanmak için aşağıdaki lisanslardan birine sahip olmanız gerekir:

- Azure AD Premium P2
- Enterprise Mobility + Security (EMS) E5 lisansı

Bir Azure AD Premium P2 veya Enterprise Mobility + Security E5 lisansınız yoksa ücretsiz bir [Enterprise Mobility + Security E5 denemesi](https://signup.microsoft.com/Signup?OfferId=87dd2714-d452-48a0-a809-d2f58c4f68b7&ali=1)oluşturun.

## <a name="step-1-set-up-users-and-group"></a>1\. adım: Kullanıcıları ve grubu ayarlama

Kaynak dizininde paylaşılacak bir veya daha fazla kaynak bulunur. Bu adımda, yetkilendirme yönetimi için hedef kaynak olan Woodgrove Bank dizininde **mühendislik grubu** adlı bir grup oluşturacaksınız. Ayrıca, dahili bir istek sahibi ayarlarsınız.

**Önkoşul rolü:** Genel yönetici veya Kullanıcı Yöneticisi

![Kullanıcı ve grup oluşturma](./media/entitlement-management-access-package-first/elm-users-groups.png)

1. [Azure Portal](https://portal.azure.com) genel yönetici veya Kullanıcı Yöneticisi olarak oturum açın.  

1. Sol gezinti bölmesinde **Azure Active Directory**' ye tıklayın.

1. Aşağıdaki iki kullanıcıyı oluşturun veya yapılandırın. Bu adları veya farklı adları kullanabilirsiniz. **Admin1** Şu anda oturum açmış olduğunuz Kullanıcı olabilir.

    | Name | Dizin rolü | Açıklama |
    | --- | --- | --- |
    | **Admin1** | Genel yönetici<br/>-veya-<br/>Sınırlı yönetici (Kullanıcı Yöneticisi) | Yönetici ve onaylayan |
    | **Requestor1** | Kullanıcı | İç istek sahibi |

    Bu öğreticide, yönetici ve onaylayan aynı kişidir, ancak genellikle bir veya daha fazla kişiyi onaylayan olacak şekilde belirlersiniz.

1. **Atanmış**bir üyelik türüyle **mühendislik grubu** adlı bir Azure AD güvenlik grubu oluşturun.

    Bu grup, yetkilendirme yönetimi için hedef kaynak olacaktır. Grup, başlamak için üyelerin boş olması gerekir.

## <a name="step-2-create-an-access-package"></a>2\. adım: Erişim paketi oluşturma

*Erişim paketi* , kullanıcının bir projede çalışması veya işini gerçekleştirmesi için gereken tüm kaynakların bir paketidir. Erişim paketleri, *kataloglar*olarak adlandırılan kapsayıcılar içinde tanımlanır. Bu adımda, **genel** katalogda bir **Web projesi erişim paketi** oluşturacaksınız.

**Önkoşul rolü:** Genel yönetici veya Kullanıcı Yöneticisi

![Erişim paketi oluşturma](./media/entitlement-management-access-package-first/elm-access-package.png)

1. Azure portal, sol gezinti bölmesinde **Azure Active Directory**' e tıklayın.

1. Sol taraftaki menüden **kimlik** Yönetimi ' ne tıklayın.

1. Sol menüde, **erişim paketleri**' ne tıklayın.  **Erişim reddedildi**görürseniz, bu dizinde bir Azure AD Premium P2 lisansının bulunduğundan emin olun.

1. **Yeni erişim paketi**' ne tıklayın.

    ![Azure portal Yetkilendirme Yönetimi](./media/entitlement-management-access-package-first/access-packages-list.png)

1. **Temel bilgiler** sekmesinde, **Mühendislik Web projesi için** **Web projesi erişim paketi** ve açıklama erişim paketi adını yazın.

1. **Katalog** açılan listesinden **genel**' e ayarlı bırakın.

    ![Yeni erişim paketi-temel bilgiler sekmesi](./media/entitlement-management-access-package-first/basics.png)

1. **İleri** ' ye tıklayarak **kaynak rolleri** sekmesini açın.

    Bu sekmede, erişim paketine dahil edilecek izinleri seçersiniz.

1. **Gruplar**' a tıklayın.

1. Grupları seçin bölmesinde, daha önce oluşturduğunuz **mühendislik grubu** grubunu bulun ve seçin.

    Varsayılan olarak, **genel** kataloğun içinde ve dışında gruplar görürsünüz. **Genel** kataloğun dışında bir grup seçtiğinizde, **genel** kataloğa eklenir.

    ![Yeni erişim paketi-kaynak rolleri sekmesi](./media/entitlement-management-access-package-first/resource-roles-select-groups.png)

1. Grubu listeye eklemek için **Seç** ' e tıklayın.

1. **Rol** açılan listesinde **üye**' i seçin.

    ![Yeni erişim paketi-kaynak rolleri sekmesi](./media/entitlement-management-access-package-first/resource-roles.png)

1. **İleri** ' ye tıklayarak **ilke** sekmesini açın.

1. **İlk Ilkeyi oluştur** ' a geç geçişyapın.

    İlkeyi bir sonraki bölümde oluşturacaksınız.

    ![Yeni erişim paketi-Ilke sekmesi](./media/entitlement-management-access-package-first/policy.png)

1. **İleri** ' ye tıklayarak **gözden geçir + oluştur** sekmesini açın.

    ![Yeni erişim paketi-gözden geçir + Oluştur sekmesi](./media/entitlement-management-access-package-first/review-create.png)

1. Erişim paketi ayarlarını gözden geçirin ve ardından **Oluştur**' a tıklayın.

    Katalog henüz etkinleştirilmediği için erişim paketinin kullanıcılara görünür olmadığını belirten bir ileti görebilirsiniz.

    ![Yeni erişim paketi-görünür olmayan ileti](./media/entitlement-management-access-package-first/not-visible.png)

1.           **Tamam**'ı tıklatın.

    Birkaç dakika sonra, erişim paketinin başarıyla oluşturulduğunu belirten bir bildirim görmeniz gerekir.

## <a name="step-3-create-a-policy"></a>3\. adım: İlke oluşturma

Bir *ilke* , erişim paketine erişmek için kuralları veya guardrayları tanımlar. Bu adımda, kaynak dizinindeki belirli bir kullanıcının erişim paketini istemesine izin veren bir ilke oluşturacaksınız. Ayrıca, isteklerin onaylanması gerektiğini ve onaylayan olacağını da belirtirsiniz.

![Erişim paketi ilkesi oluşturma](./media/entitlement-management-access-package-first/elm-access-package-policy.png)

**Önkoşul rolü:** Genel yönetici veya Kullanıcı Yöneticisi

1. **Web projesi erişim paketinde**, sol menüsünde **ilkeler**' e tıklayın.

    ![Paket ilkeleri listesine erişim](./media/entitlement-management-access-package-first/policies-list.png)

1. İlke **Ekle** ' ye tıklayarak ilke oluştur ' u açın.

1. **İç istek sahibi ilkesi** adı yazın ve açıklama **Bu dizindeki kullanıcıların Web projesi kaynaklarına erişim istemesine izin verir**.

1. **Erişim isteyebilen kullanıcılar** bölümünde, **dizininizdeki kullanıcılar için**öğesine tıklayın.

    ![İlke oluştur](./media/entitlement-management-access-package-first/policy-create.png)

1. **Kullanıcıları ve grupları seçin** bölümüne aşağı kaydırın ve **Kullanıcı ve Grup Ekle**' ye tıklayın.

1. Kullanıcıları ve grupları seç bölmesinde, daha önce oluşturduğunuz **Requestor1** kullanıcısını seçin ve ardından **Seç**' e tıklayın.

1. **İstek** bölümünde, **onay gerektir** seçeneğini **Evet**olarak ayarlayın.

1. **Onaylayanları Seç** bölümünde, **onaylayan Ekle**' ye tıklayın.

1. Onaylayanları seç bölmesinde, daha önce oluşturduğunuz **admin1** seçin ve ardından **Seç**' e tıklayın.

    Bu öğreticide, yönetici ve onaylayan aynı kişidir, ancak başka bir kişiyi onaylayan olarak belirleyebilirsiniz.

1. **Süre sonu** bölümünde, **erişim paketi süre sonu** ' nu **gün sayısı**olarak ayarlayın.

1. Erişimin süresi **30** gün **sonra dolacak** .

1. **Etkinleştirme İlkesi**için **Evet**' e tıklayın.

    ![İlke ayarları oluşturma](./media/entitlement-management-access-package-first/policy-create-settings.png)

1. **İç istek sahibi ilkesi**oluşturmak için **Oluştur** ' a tıklayın.

1. Web projesi erişim paketinin sol menüsünde **Genel Bakış ' a**tıklayın.

1. **Erişim portalı bağlantısını**Kopyala.

    Bu bağlantıyı bir sonraki adım için kullanacaksınız.

    ![Erişim paketine genel bakış-erişim portalı bağlantısı](./media/entitlement-management-shared/my-access-portal-link.png)

## <a name="step-4-request-access"></a>4\. Adım: Erişim izni iste

Bu adımda, adımları **iç istek sahibi** olarak gerçekleştirirsiniz ve erişim paketine erişim isteği alırsınız. İstek sahipleri erişim portalı adlı bir site kullanarak isteklerini gönderir. Erişim portalı, Isteklerimin erişim paketleri isteklerini göndermesini sağlar, zaten erişimi olan erişim paketlerine bakın ve istek geçmişini görüntüler.

**Önkoşul rolü:** İç istek sahibi

1. Azure portal oturumunuzu kapatın.

1. Yeni bir tarayıcı penceresinde, önceki adımda kopyaladığınız erişimim portalı bağlantısına gidin.

1. Erişim portalından **Requestor1**olarak oturum açın.

    **Web projesi erişim paketini**görmeniz gerekir.

1. Gerekirse, **Açıklama** sütununda, erişim paketiyle ilgili ayrıntıları görüntülemek için oka tıklayın.

    ![Erişim Portalı-erişim paketlerim](./media/entitlement-management-shared/my-access-access-packages.png)

1. Paketi seçmek için onay işaretine tıklayın.

1. Istek erişimi bölmesini açmak için **erişim iste** ' ye tıklayın.

1. **İş gerekçe** kutusuna **Web projesi üzerinde çalışan**gerekçe yazın.

1. Belirli bir **Dönem Için Isteği** **Evet**olarak ayarlayın.

1. **Başlangıç tarihini** bugün ve **bitiş tarihi** olarak yarın olarak ayarlayın.

    ![Erişim Portalı-erişim ISTEME](./media/entitlement-management-shared/my-access-request-access.png)

1. **Gönder**'e tıklayın.

1. İsteğinizin gönderildiğini doğrulamak için sol taraftaki menüden **istek geçmişi** ' ne tıklayın.

## <a name="step-5-approve-access-request"></a>5\. Adım: Erişim isteğini onayla

Bu adımda, **onaylayan** Kullanıcı olarak oturum açıp iç istek sahibine yönelik erişim isteğini onaylarsınız. Onaylayanlar istekleri göndermek için kullandığı için, istek erişim portallarımı kullanır. Onaylayanlar, erişim portalı 'nı kullanarak bekleyen onayları görüntüleyebilir ve istekleri onaylayabilir veya reddedebilir.

**Önkoşul rolü:** Onaylayan

1. Erişim portalımın oturumunu kapatın.

1. [Erişim portalından](https://myaccess.microsoft.com) **admin1**olarak oturum açın.

1. Sol taraftaki menüden **onaylar**' a tıklayın.

1. **Bekleyen** sekmesinde **Requestor1**bulun.

    İsteği Requestor1 'tan görmüyorsanız, birkaç dakika bekleyip yeniden deneyin.

1. Erişim isteği bölmesini açmak için **Görünüm** bağlantısına tıklayın.

1. **Onayla**' ya tıklayın.

1. **Neden** kutusuna **Web projesi için onaylanan erişim**nedenini yazın.

    ![Erişim Portalı-erişim isteği](./media/entitlement-management-shared/my-access-approve-request.png)

1. Kararınızı göndermek için **Gönder** ' e tıklayın.

    Başarıyla onaylanan bir ileti görmeniz gerekir.

## <a name="step-6-validate-that-access-has-been-assigned"></a>6\. Adım: Erişimin atandığını doğrulama

Erişim isteğini onayladığınıza göre, bu adımda, **iç istek sahibine** erişim paketi atandığını ve artık **mühendislik grubu** grubunun bir üyesi olduğunu onaylamış olursunuz.

**Önkoşul rolü:** Genel yönetici veya Kullanıcı Yöneticisi

1. Erişim portalımın oturumunu kapatın.

1. **Admin1**olarak [Azure Portal](https://portal.azure.com) oturum açın.

1. **Azure Active Directory** ' a ve ardından **kimlik**Yönetimi ' ne tıklayın.

1. Sol menüde, **erişim paketleri**' ne tıklayın.

1. **Web projesi erişim paketini**bulun ve tıklatın.

1. Sol menüde **istekler**' e tıklayın.

    Requestor1 ve Iç istek sahibi ilkesini **teslim edildi**durumuyla görmeniz gerekir.

1. İstek ayrıntılarını görmek için isteğe tıklayın.

    ![Erişim paketi-Istek ayrıntıları](./media/entitlement-management-access-package-first/request-details.png)

1. Sol gezinti bölmesinde **Azure Active Directory**' ye tıklayın.

1. **Gruplar** ' a tıklayın ve **mühendislik grubu** grubunu açın.

1. **Üyeler**' e tıklayın.

    Üye olarak listelenmiş **Requestor1** görmeniz gerekir.

    ![Mühendislik grubu üyeleri](./media/entitlement-management-access-package-first/group-members.png)

## <a name="step-7-clean-up-resources"></a>7\. Adım: Kaynakları temizleme

Bu adımda, yaptığınız değişiklikleri kaldırır ve **Web projesi erişim paketi** erişim paketini silebilirsiniz.

**Önkoşul rolü:**  Genel yönetici veya Kullanıcı Yöneticisi

1. Azure portal, **Azure Active Directory** ' a ve ardından **kimlik**Yönetimi ' ne tıklayın.

1. **Web projesi erişim paketini**açın.

1. **Atamalar**' a tıklayın.

1. **Requestor1**için üç nokta ( **...** ) simgesini ve ardından **erişimi kaldır**' ı tıklatın.

    Durum, teslim edilen durumundan süre dolmayacak şekilde değişir.

1. **İlkeler**' e tıklayın.

1. **İç istek sahibi ilkesi**için üç noktaya ( **...** ) ve ardından **Sil**' e tıklayın.

1. **Kaynak rolleri**' ne tıklayın.

1. **Mühendislik grubu**için üç nokta ( **...** ) simgesini ve ardından **kaynak rolünü kaldır**' ı tıklatın.

1. Erişim paketleri listesini açın.

1. **Web projesi erişimi projesi**için üç nokta işaretine ( **...** ) ve ardından **Sil**' e tıklayın.

1. Azure Active Directory, **Requestor1** ve **admin1**gibi oluşturduğunuz tüm kullanıcıları silin.

1. **Mühendislik grubu** grubunu silin.

## <a name="next-steps"></a>Sonraki adımlar

Yetkilendirme Yönetimi 'ndeki yaygın senaryo adımları hakkında bilgi edinmek için sonraki makaleye ilerleyin.
> [!div class="nextstepaction"]
> [Yaygın senaryolar](entitlement-management-scenarios.md)
