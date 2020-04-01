---
title: Öğretici - Erişim paketi oluşturma - Azure AD yetkilendirme yönetimi
description: Azure Active Directory yetkilendirme yönetiminde ilk erişim paketinizi oluşturma konusunda adım adım öğretici.
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
ms.date: 03/30/2020
ms.author: ajburnle
ms.reviewer: markwahl-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: c2d31ef46dfba31a8f217f68e8d5f98b67d58da5
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80410583"
---
# <a name="tutorial-create-your-first-access-package-in-azure-ad-entitlement-management"></a>Öğretici: Azure AD yetkilendirme yönetiminde ilk erişim paketinizi oluşturun

Çalışanların gruplar, uygulamalar ve siteler gibi ihtiyaç duyduğu tüm kaynaklara erişimi yönetmek kuruluşlar için önemli bir işlevdir. Çalışanlara üretken olmaları ve artık ihtiyaç duyulmadığında erişimlerini kaldırmaları için gereken doğru erişim düzeyini vermek istiyorsunuz.

Bu eğitimde, Woodgrove Bank için BT yöneticisi olarak çalışıyorsunuz. Dahili kullanıcıların self servis isteğinde bulunabileceği bir pazarlama kampanyası için bir kaynak paketi oluşturmanız istendi. İstekler onay gerektirmez ve kullanıcının erişimi 30 gün sonra sona erer. Bu öğretici için, pazarlama kampanyası kaynakları yalnızca tek bir gruba üyedir, ancak gruplar, uygulamalar veya SharePoint Online sitelerinin bir koleksiyonu olabilir.

![Senaryoya genel bakış](./media/entitlement-management-access-package-first/elm-scenario-overview.png)

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Kaynak olarak grup içeren bir erişim paketi oluşturma
> * Dizininizdeki bir kullanıcının erişim istemesine izin ver
> * Dahili bir kullanıcının erişim paketini nasıl isteyebileceğini gösterin

İlk erişim paketinizi oluşturma da dahil olmak üzere Azure Active Directory yetkilendirme yönetimini dağıtma işleminin adım adım gösterimi için aşağıdaki videoyu görüntüleyin:

>[!VIDEO https://www.youtube.com/embed/zaaKvaaYwI4]

## <a name="prerequisites"></a>Ön koşullar

Azure AD yetkilendirme yönetimini kullanmak için aşağıdaki lisanslardan birine sahip olmalısınız:

- Azure AD Premium P2
- Kurumsal Mobilite + Güvenlik (EMS) E5 lisansı

Daha fazla bilgi için Lisans [gereksinimlerine](entitlement-management-overview.md#license-requirements)bakın.

## <a name="step-1-set-up-users-and-group"></a>Adım 1: Kullanıcıları ve grubu ayarlama

Kaynak dizininin paylaşılaması gereken bir veya daha fazla kaynak vardır. Bu adımda, Woodgrove Bank dizininde yetkilendirme yönetimi için hedef kaynak olan **Pazarlama kaynakları** adlı bir grup oluşturursunuz. Ayrıca bir dahili istekte bulundurabilirsiniz.

**Önkoşul rolü:** Genel yönetici veya Kullanıcı yöneticisi

![Kullanıcı ve gruplar oluşturma](./media/entitlement-management-access-package-first/elm-users-groups.png)

1. [Azure portalında](https://portal.azure.com) Global yönetici veya Kullanıcı yöneticisi olarak oturum açın.  

1. Sol gezintide **Azure Etkin Dizin'i**tıklatın.

1. Aşağıdaki iki kullanıcıyı oluşturun veya yapılandırır. Bu adları veya farklı adları kullanabilirsiniz. **Admin1** şu anda oturum açmış olduğunuz kullanıcı olabilir.

    | Adı | Dizin rolü |
    | --- | --- |
    | **Yönetici1** | Genel yönetici<br/>-veya-<br/>Kullanıcı yöneticisi |
    | **İstekli1** | Kullanıcı |

1. **Atanmış**üyelik türüne sahip **Pazarlama kaynakları** adlı bir Azure AD güvenlik grubu oluşturun.

    Bu grup, yetkilendirme yönetimi için hedef kaynak olacaktır. Grup başlamak için üye boş olmalıdır.

## <a name="step-2-create-an-access-package"></a>Adım 2: Erişim paketi oluşturma

*Erişim paketi,* bir ekibin veya projenin ihtiyaç duyduğu ve ilkelerle yönetilen bir kaynak paketidir. Erişim paketleri katalog adı verilen *kapsayıcılarda*tanımlanır. Bu adımda, **Genel** katalogda bir **Pazarlama Kampanyası** erişim paketi oluşturursunuz.

**Önkoşul rolü:** Genel yönetici, Kullanıcı yöneticisi, Katalog sahibi veya Access paket yöneticisi

![Erişim paketi oluşturma](./media/entitlement-management-access-package-first/elm-access-package.png)

1. Azure portalında, sol gezintide **Azure Etkin Dizin'i**tıklatın.

2. Sol menüde **Kimlik Yönetimi'ni** tıklatın

3. Sol menüde, **Access paketlerine**tıklayın.  **Access'in reddedildiğini**görürseniz, dizininizde bir Azure AD Premium P2 lisansı nın bulunduğundan emin olun.

4. **Yeni erişim paketini**tıklatın.

    ![Azure portalında yetkilendirme yönetimi](./media/entitlement-management-shared/access-packages-list.png)

5. Temel **bilgiler** sekmesinde, **Pazarlama Kampanyası** erişim paketini yazın ve **kampanyanın kaynaklarına erişim**açıklamasını yazın.

6. **Katalog** açılır liste kümesini **Genel'e**bırakın.

    ![Yeni erişim paketi - Temel ler sekmesi](./media/entitlement-management-access-package-first/basics.png)

7. **Kaynak rolleri** sekmesini açmak için **İleri'yi** tıklatın.

    Bu sekmede, erişim paketine dahil etmek için kaynakları ve kaynak rolünü seçersiniz.

8. **Gruplar ve Takımlar'ı**tıklatın.

9. Grupları Seç bölmesinde, daha önce oluşturduğunuz **Pazarlama kaynakları** grubunu bulun ve seçin.

    Varsayılan olarak, **Genel** kataloğun içinde ve dışında gruplar görürsünüz. **Genel** kataloğun dışında bir grup seçtiğinizde, **genel** kataloğuna eklenir.

    ![Yeni erişim paketi - Kaynak rolleri sekmesi](./media/entitlement-management-access-package-first/resource-roles-select-groups.png)

10. Grubu listeye eklemek için **Seç'i** tıklatın.

11. **Rol** açılır listesinde **Üye'yi**seçin.

    ![Yeni erişim paketi - Kaynak rolleri sekmesi](./media/entitlement-management-access-package-first/resource-roles.png)

    >[!NOTE]
    > Dinamik [grupları](../users-groups-roles/groups-create-rule.md) kullanırken, sahibi dışında başka roller görmezsiniz. Bu tasarım gereğidir.
    > ![Senaryoya genel bakış](./media/entitlement-management-access-package-first/dynamic-group-warning.png)

12. **İstekler** sekmesini açmak için **İleri'yi** tıklatın.

    Bu sekmede, bir istek ilkesi oluşturursunuz. Bir *ilke,* bir erişim paketine erişmek için kuralları veya korkulukları tanımlar. Kaynak dizinindeki belirli bir kullanıcının bu erişim paketini istemesine izin veren bir ilke oluşturursunuz.

13. Erişim **isteğinde bulunabilecek Kullanıcılar** bölümünde, **dizininizdeki kullanıcılar için'i** tıklatın ve ardından **Belirli kullanıcılar ve gruplar'ı**tıklatın.

    ![Yeni erişim paketi - İstekler sekmesi](./media/entitlement-management-access-package-first/requests.png)

14. **Kullanıcı ve gruplar ekle'yi**tıklatın.

15. Kullanıcıları ve grupları seç bölmesinde, daha önce oluşturduğunuz **İstekor1** kullanıcısını seçin.

    ![Yeni erişim paketi - İstekler sekmesi - Kullanıcıları ve grupları seçin](./media/entitlement-management-access-package-first/requests-select-users-groups.png)

16. **Seç'i**tıklatın.

17. **Onay** ve Etkinleştir istekleri bölümlerine aşağı **kaydırın.**

18. Bırak **Onay** No **olarak**ayarlandı.

19. **İstekleri Etkinleştir**için, bu erişim paketinin oluşturulur oluşturulmaz istenmesini sağlamak için **Evet'i** tıklatın.

    ![Yeni erişim paketi - İstekler sekmesi - İstekleri Onayla ve Etkinleştir](./media/entitlement-management-access-package-first/requests-approval-enable.png)

20. **Yaşam Döngüsü** sekmesini açmak için **İleri'yi** tıklatın.

21. Sona **Erme** bölümünde, **Access paketi atamalarının gün sayısıyla sona ermesini** ayarlayın. **Number of days**

22. Ayar atamaları **30** gün **sonra sona erer.**

    ![Yeni erişim paketi - Yaşam Döngüsü sekmesi](./media/entitlement-management-access-package-first/lifecycle.png)

23. **Gözden Geçir + Oluştur** sekmesini açmak için **İleri'yi** tıklatın.

    ![Yeni erişim paketi - Gözden Geçirin + Oluştur sekmesi](./media/entitlement-management-access-package-first/review-create.png)

    Birkaç dakika sonra, erişim paketinin başarıyla oluşturulduğuna dair bir bildirim görmeniz gerekir.

24. Pazarlama Kampanyası erişim paketinin sol menüsünde **Genel Bakış'ı**tıklatın.

25. Erişim **portalım bağlantısını**kopyala.

    Bir sonraki adım için bu bağlantıyı kullanırsınız.

    ![Erişim paketine genel bakış - Erişim portalı bağlantım](./media/entitlement-management-shared/my-access-portal-link.png)

## <a name="step-3-request-access"></a>Adım 3: Erişim isteği

Bu adımda, adımları **dahili istekçi** olarak gerçekleştirir ve erişim paketine erişim isteğinde bulunun. İstekliler isteklerini Erişimim portalı adlı bir siteyi kullanarak gönderirler. Erişimim portalı, istek sahiplerinin erişim paketleri için istek göndermelerine, zaten erişebildikleri erişim paketlerini görmelerine ve istek geçmişlerini görüntülemelerine olanak tanır.

**Önkoşul rolü:** İç istekte

1. Azure portalından çıkış kaydedin.

1. Yeni bir tarayıcı penceresinde, önceki adımda kopyaladığınız Erişim portalım bağlantısına gidin.

1. Access portalında **İstek1**olarak oturum açın.

    **Pazarlama Kampanyası** erişim paketini görmelisiniz.

1. Gerekirse, **Açıklama** sütununda, erişim paketiyle ilgili ayrıntıları görüntülemek için oku tıklatın.

    ![Erişim portalım - Erişim paketleri](./media/entitlement-management-shared/my-access-access-packages.png)

1. Paketi seçmek için onay işaretini tıklatın.

1. İstek erişim bölmesini açmak için **Erişim İsteği'ni** tıklatın.

    ![Erişim portalım - Erişim düğmesi iste](./media/entitlement-management-access-package-first/my-access-request-access-button.png)

1. İş **gerekçesi** kutusunda, yeni **pazarlama kampanyası üzerinde çalıştığım gerekçeyi**yazın.

    ![Erişim portalım - Erişim isteği](./media/entitlement-management-shared/my-access-request-access.png)

1. **Gönder'i**tıklatın.

1. Sol menüde, isteğinizin gönderildiğini doğrulamak için **Geçmiş İstek'i** tıklatın.

## <a name="step-4-validate-that-access-has-been-assigned"></a>Adım 4: Erişimin atandığını doğrulama

Bu adımda, **dahili isteksahibinin** erişim paketine atandığını ve artık **Pazarlama kaynakları** grubunun bir üyesi olduklarını onaylarsınız.

**Önkoşul rolü:** Genel yönetici, Kullanıcı yöneticisi, Katalog sahibi veya Access paket yöneticisi

1. Erişim portalımdan çıkış.

1. [Azure portalında](https://portal.azure.com) **Yönetici1**olarak oturum açın.

1. **Azure Etkin Dizini'ni** tıklatın ve ardından **Kimlik Yönetimi'ni**tıklatın.

1. Sol menüde, **Access paketlerine**tıklayın.

1. **Pazarlama Kampanyası** erişim paketini bulun ve tıklatın.

1. Sol menüde **İstekler'i**tıklatın.

    **Teslim**durumu olan İstek1'i ve İlk ilkeyi görmeniz gerekir.

1. İstek ayrıntılarını görmek için isteği tıklatın.

    ![Erişim paketi - İstek ayrıntıları](./media/entitlement-management-access-package-first/request-details.png)

1. Sol gezintide **Azure Etkin Dizin'i**tıklatın.

1. **Gruplar'ı** tıklatın ve **Pazarlama kaynakları** grubunu açın.

1. **Üyeler'i**tıklatın.

    Üye olarak listelenen **Requestor1'i** görmelisiniz.

    ![Pazarlama kaynakları üyeleri](./media/entitlement-management-access-package-first/group-members.png)

## <a name="step-5-clean-up-resources"></a>Adım 5: Kaynakları temizleme

Bu adımda, yaptığınız değişiklikleri kaldırır ve **Pazarlama Kampanyası** erişim paketini silersiniz.

**Önkoşul rolü:**  Genel yönetici veya Kullanıcı yöneticisi

1. Azure portalında **Azure Etkin Dizin'i** tıklatın ve ardından **Kimlik Yönetimi'ni**tıklatın.

1. Pazarlama **Kampanyası** erişim paketini açın.

1. **Atamalar'ı**tıklatın.

1. **İstekçi1**için , elipsis **(...**)'yi tıklatın ve ardından **erişimi kaldır'ı**tıklatın. Görünen iletide **Evet'i**tıklatın.

    Birkaç dakika sonra, durum Teslim'den Süresi Doldu'na değişecektir.

1. **Kaynak rolleri'ni**tıklatın.

1. **Pazarlama kaynakları**için, elipsleri (**...**) tıklatın ve ardından kaynak **rolünü kaldır'ı**tıklatın. Görünen iletide **Evet'i**tıklatın.

1. Erişim paketlerilistesini açın.

1. **Pazarlama Kampanyası**için , elipsis **(...**)'yi tıklatın ve sonra **Sil'i**tıklatın. Görünen iletide **Evet'i**tıklatın.

1. Azure Etkin Dizini'nde, **Oluşturduğunuz Requestor1** ve **Admin1**gibi tüm kullanıcıları silin.

1. Pazarlama **kaynakları** grubunu silin.

## <a name="next-steps"></a>Sonraki adımlar

Yetkilendirme yönetiminde sık karşılaşılan senaryo adımları hakkında bilgi edinmek için bir sonraki makaleye ilerleyin.
> [!div class="nextstepaction"]
> [Genel senaryolar](entitlement-management-scenarios.md)
