---
title: Microsoft uygulamasında oturum açma sorunları | Microsoft Dokümanlar
description: Azure AD'yi kullanarak birinci taraf Microsoft Uygulamaları'nda oturum açarken karşılaşılan sık karşılaşılan sorunları giderme (Office 365 gibi)
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7ee8802aeb2a760e255ab4f5e99010dfedc45e0d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67108310"
---
# <a name="problems-signing-in-to-a-microsoft-application"></a>Microsoft uygulamasında oturum açma sorunları

Microsoft Uygulamaları (Office 365 Exchange, SharePoint, Yammer, vb.) 3.

Bir kullanıcının Microsoft tarafından yayımlanan bir uygulamaya erişebilmek için üç ana yolu vardır.

-   Office 365 veya diğer ücretli paketlerindeki uygulamalarda, kullanıcılara **lisans ataması** yoluyla doğrudan kullanıcı hesabına veya grup tabanlı lisans atama özelliğimizi kullanan bir grup aracılığıyla erişim hakkı verilir.

-   Microsoft veya Üçüncü Tarafın herkesin kullanması için serbestçe yayımlayan uygulamalarda, kullanıcılara **kullanıcı onayı**ile erişim izni verilebilir. Bu, azure AD Work veya School hesaplarıyla uygulamada oturum açtıkları ve uygulamaların hesaplarındaki sınırlı veri kümesine erişmesine izin verdikleri anlamına gelir.

-   Microsoft veya üçüncü bir tarafın herkesin kullanması için serbestçe yayımlettiği uygulamalarda, kullanıcılara **yönetici onayı**ile erişim izni de verilebilir. Bu, bir yöneticinin uygulamanın kuruluştaki herkes tarafından kullanılabileceğini belirlediği anlamına gelir, bu nedenle başvuruda Global Administrator hesabıyla oturum açabilir ve kuruluştaki herkese erişim izni verir.

Sorununuzu gidermek için, [Uygulama Erişimi'ne sahip Genel Sorun Alanları'nı gözden geçirin](#general-problem-areas-with-application-access-to-consider) ve ardından Ayrıntılara girmek için Microsoft Application erişimini giderme adımlarını okuyun.

## <a name="general-problem-areas-with-application-access-to-consider"></a>Dikkate Alınması Gereken Uygulama Erişimi olan Genel Sorun Alanları

Nereden başlayacağınızı bir fikriniz varsa, ayrıntılı olarak kullanabileceğiniz genel sorunlu alanların bir listesi aşağıdadır, ancak hızlı bir şekilde gitmek için izgeçidi okumanızı öneririz: Walkthrough: Microsoft Application erişimini giderme adımları.

-   [Kullanıcının hesabındaki sorunlar](#problems-with-the-users-account)

-   [Gruplarla ilgili sorunlar](#problems-with-groups)

-   [Koşullu Erişim ilkeleriyle ilgili sorunlar](#problems-with-conditional-access-policies)

-   [Uygulama onayı ile ilgili sorunlar](#problems-with-application-consent)

## <a name="steps-to-troubleshoot-microsoft-application-access"></a>Microsoft Application erişimini giderme adımları

Aşağıda, kullanıcıları bir Microsoft uygulamasında oturum açamadığı zaman kullanıcıların karşısına çıkan bazı sık karşılaşılan sorunlar yer alıyor.

- Önce kontrol etmek için genel konular

  * Kullanıcının yerel bir uygulama URL'si değil, **doğru URL'de** oturum açmıştından emin olun.

  * Kullanıcının hesabının **kilitlendirolmadığından** emin olun.

  * **Kullanıcının hesabının** Azure Etkin Dizin'de bulunduğundan emin olun. [Azure Etkin Dizini'nde bir kullanıcı hesabı olup olmadığını denetleme](#problems-with-the-users-account)

  * Oturum açma için kullanıcının hesabının **etkin** olduğundan emin olun. [Kullanıcının hesap durumunu kontrol edin](#problems-with-the-users-account)

  * Kullanıcının **parolasının süresinin dolmadığından veya unutulmadığından** emin olun. [Kullanıcının parolasını sıfırlama](#reset-a-users-password) veya [self servis parola sıfırlamayı etkinleştirme](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started)

  * Çok **Faktörlü Kimlik Doğrulama'nın** kullanıcı erişimini engellemediğinden emin olun. [Kullanıcının çok faktörlü kimlik doğrulama durumunu denetleme](#check-a-users-multi-factor-authentication-status) veya [kullanıcının kimlik doğrulama iletişim bilgilerini denetleme](#check-a-users-authentication-contact-info)

  * **Koşullu Erişim ilkesinin** veya **Kimlik Koruma** ilkesinin kullanıcı erişimini engellemediğinden emin olun. [Belirli bir Koşullu Erişim ilkesini kontrol edin](#problems-with-conditional-access-policies) veya [belirli bir uygulamanın Koşullu Erişim ilkesini denetleyin](#check-a-specific-applications-conditional-access-policy) veya [belirli bir Koşullu Erişim ilkesini devre dışı kaldırın](#disable-a-specific-conditional-access-policy)

  * Çok Faktörlü Kimlik Doğrulama veya Koşullu Erişim ilkelerinin uygulanmasına izin vermek için kullanıcının **kimlik doğrulama iletişim bilgisinin** güncel olduğundan emin olun. [Kullanıcının çok faktörlü kimlik doğrulama durumunu denetleme](#check-a-users-multi-factor-authentication-status) veya [kullanıcının kimlik doğrulama iletişim bilgilerini denetleme](#check-a-users-authentication-contact-info)

- **Lisans gerektiren** **Microsoft** uygulamaları (Office365 gibi) için, yukarıdaki genel sorunları ekarte ettiğinizde kontrol etmek için bazı özel sorunlar şunlardır:

  * Kullanıcının veya atanmış bir lisansa sahip olduğundan emin **olun.** [Kullanıcının atanmış lisanslarını denetleme](#check-a-users-assigned-licenses) veya [grubun atanmış lisanslarını denetleme](#check-a-groups-assigned-licenses)

  * Lisans statik bir **gruba** **atanmışsa,** **kullanıcının** bu grubun bir üyesi olduğundan emin olun. [Kullanıcının grup üyeliklerini kontrol edin](#check-a-users-group-memberships)

  * Lisans **dinamik**bir gruba **atanmışsa,** **dinamik grup kuralının doğru ayarlandığından**emin olun. [Dinamik bir grubun üyelik ölçütlerini kontrol edin](#check-a-dynamic-groups-membership-criteria)

  * Lisans **dinamik**bir gruba **atanmışsa,** dinamik grubun üyeliğini **işlemeyi tamamladığını** ve **kullanıcının üye olduğundan** emin olun (bu biraz zaman alabilir). [Kullanıcının grup üyeliklerini kontrol edin](#check-a-users-group-memberships)

  *  Lisansın atandığından emin olduktan sonra, lisansın **süresinin dolmadığından**emin olun.

  *  Lisansın erişen **uygulama için** olduğundan emin olun.

- **Lisans gerektirmeyen** **Microsoft** uygulamaları için, denetlenebilen diğer bazı şeyler şunlardır:

  * Uygulama kullanıcı düzeyinde **izinler** talep ediyorsa (örneğin, "Bu kullanıcının posta kutusuna erişin"), kullanıcının uygulamada oturum açmış olduğundan ve uygulamanın verilerine erişmesini sağlamak için kullanıcı düzeyinde bir **onay işlemi** gerçekleştirdiğinden emin olun.

  * Uygulama yönetici düzeyinde **izinler** istiyorsa (örneğin,"Tüm kullanıcının posta kutularına erişin"), Bir Genel Yöneticinin kuruluştaki **tüm kullanıcılar adına yönetici düzeyinde** bir onay işlemi gerçekleştirdiğinden emin olun.

## <a name="problems-with-the-users-account"></a>Kullanıcının hesabındaki sorunlar

Uygulamaya atanan bir kullanıcıyla ilgili bir sorun nedeniyle uygulama erişimi engellenebilir. Aşağıda, kullanıcılarla ve hesap ayarlarıyla ilgili sorunları gidermenin ve çözebileceğiniz bazı yollar verilmiştir:

-   [Azure Etkin Dizini'nde bir kullanıcı hesabı olup olmadığını denetleme](#check-if-a-user-account-exists-in-azure-active-directory)

-   [Kullanıcının hesap durumunu denetleme](#check-a-users-account-status)

-   [Kullanıcının parolanı sıfırlama](#reset-a-users-password)

-   [Kendi kendine parola sıfırlamayı etkinleştirme](#enable-self-service-password-reset)

-   [Kullanıcının çok faktörlü kimlik doğrulama durumunu denetleme](#check-a-users-multi-factor-authentication-status)

-   [Kullanıcının kimlik doğrulama kişi bilgilerini kontrol edin](#check-a-users-authentication-contact-info)

-   [Kullanıcının grup üyeliklerini kontrol edin](#check-a-users-group-memberships)

-   [Kullanıcının atanmış lisanslarını denetleme](#check-a-users-assigned-licenses)

-   [Kullanıcıya lisans atama](#assign-a-user-a-license)

### <a name="check-if-a-user-account-exists-in-azure-active-directory"></a>Azure Etkin Dizini'nde bir kullanıcı hesabı olup olmadığını denetleme

Bir kullanıcının hesabının olup olmadığını kontrol etmek için aşağıdaki adımları izleyin:

1.  Azure [**portalını**](https://portal.azure.com/) açın ve Global Administrator olarak oturum **açın.**

2.  Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3.  Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4.  gezinti menüsünde **Kullanıcılar ve gruplar'ı** tıklatın.

5.  **tüm kullanıcıları**tıklatın.

6.  İlgilendiğiniz kullanıcıyı **arayın** ve seçmek için **satırı tıklatın.**

7.  Kullanıcı nesnesinin özelliklerini kontrol edin, beklediğiniz gibi göründüklerinden ve hiçbir veri eksik olmadığından emin olun.

### <a name="check-a-users-account-status"></a>Kullanıcının hesap durumunu denetleme

Bir kullanıcının hesap durumunu denetlemek için aşağıdaki adımları izleyin:

1.  Azure [**portalını**](https://portal.azure.com/) açın ve Global Administrator olarak oturum **açın.**

2.  Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3.  Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4.  gezinti menüsünde **Kullanıcılar ve gruplar'ı** tıklatın.

5.  **tüm kullanıcıları**tıklatın.

6.  İlgilendiğiniz kullanıcıyı **arayın** ve seçmek için **satırı tıklatın.**

7.  **Profili**tıklatın.

8.  **Ayarlar** **altında, Oturum Aç'ı Oturum Aç'ın** **Hayır**olarak ayarlı olduğundan emin olun.

### <a name="reset-a-users-password"></a>Kullanıcının parolanı sıfırlama

Bir kullanıcının parolasını sıfırlamak için aşağıdaki adımları izleyin:

1.  Azure [**portalını**](https://portal.azure.com/) açın ve Global Administrator olarak oturum **açın.**

2.  Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3.  Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4.  gezinti menüsünde **Kullanıcılar ve gruplar'ı** tıklatın.

5.  **tüm kullanıcıları**tıklatın.

6.  İlgilendiğiniz kullanıcıyı **arayın** ve seçmek için **satırı tıklatın.**

7.  kullanıcı bölmesinin üst kısmındaki **parolayı sıfırla** düğmesini tıklatın.

8.  görünen **parola** bölmesindeki **Parolayı Sıfırla** düğmesini tıklatın.

9.  Geçici **parolayı** kopyalayın veya kullanıcı için **yeni bir parola girin.**

10. Bu yeni parolayı kullanıcıya iletin, bir sonraki oturum açma sırasında bu parolayı Azure Active Directory'de değiştirmeleri gerekir.

### <a name="enable-self-service-password-reset"></a>Kendi kendine parola sıfırlamayı etkinleştirme

Self servis parola sıfırlamayı etkinleştirmek için aşağıdaki dağıtım adımlarını izleyin:

-   [Kullanıcıların Azure Etkin Dizin parolalarını sıfırlamalarını sağlama](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started)

-   [Kullanıcıların Etkin Dizini şirket içi parolalarını sıfırlamalarına veya değiştirmelerine olanak sağlama](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started)

### <a name="check-a-users-multi-factor-authentication-status"></a>Kullanıcının çok faktörlü kimlik doğrulama durumunu denetleme

Bir kullanıcının çok faktörlü kimlik doğrulama durumunu denetlemek için aşağıdaki adımları izleyin:

1. Azure [**portalını**](https://portal.azure.com/) açın ve Global Administrator olarak oturum **açın.**

2. Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3. Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4. gezinti menüsünde **Kullanıcılar ve gruplar'ı** tıklatın.

5. **tüm kullanıcıları**tıklatın.

6. bölmenin üst kısmındaki **Çok Faktörlü Kimlik Doğrulama** düğmesini tıklatın.

7. Çok **Faktörlü Kimlik Doğrulama Yönetimi portalı** yüklendikten sonra **Kullanıcılar** sekmesinde olduğundan emin olun.

8. Arama yaparak, filtreleyerek veya sıralayarak kullanıcıyı kullanıcı listesinde bulun.

9. Kullanıcı listesinden kullanıcıyı seçin **ve**çok faktörlü kimlik doğrulamasını istediğiniz gibi etkinleştirin veya **zorleyin.** **Disable**

   * **Not**: Bir kullanıcı **Zorlanmış** durumdaysa, hesabına geri dönmelerine izin vermek için geçici olarak **Devre Dışı Bırak'a** ayarlayabilirsiniz. Geri döndüklerinde, bir sonraki oturum açma sırasında kişi bilgilerini yeniden kaydetmelerini istemek için durumlarını yeniden **Enabled** olarak değiştirebilirsiniz. Alternatif olarak, bu verileri doğrulamak veya onlar için ayarlamak için [kullanıcının kimlik doğrulama iletişim bilgilerini](#check-a-users-authentication-contact-info) denetle'deki adımları izleyebilirsiniz.

### <a name="check-a-users-authentication-contact-info"></a>Kullanıcının kimlik doğrulama kişi bilgilerini kontrol edin

Çok faktörlü kimlik doğrulama, Koşullu Erişim, Kimlik Koruması ve Parola Sıfırlama için kullanılan kullanıcının kimlik doğrulama iletişim bilgilerini denetlemek için aşağıdaki adımları izleyin:

1.  Azure [**portalını**](https://portal.azure.com/) açın ve Global Administrator olarak oturum **açın.**

2.  Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3.  Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4.  gezinti menüsünde **Kullanıcılar ve gruplar'ı** tıklatın.

5.  **tüm kullanıcıları**tıklatın.

6.  İlgilendiğiniz kullanıcıyı **arayın** ve seçmek için **satırı tıklatın.**

7.  **Profili**tıklatın.

8.  **Kimlik Doğrulama iletişim bilgilerine**aşağı kaydırın.

9.  Kullanıcı için kayıtlı verileri **gözden geçirin** ve gerektiğinde güncelleştirin.

### <a name="check-a-users-group-memberships"></a>Kullanıcının grup üyeliklerini kontrol edin

Bir kullanıcının grup üyeliklerini denetlemek için aşağıdaki adımları izleyin:

1.  Azure [**portalını**](https://portal.azure.com/) açın ve Global Administrator olarak oturum **açın.**

2.  Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3.  Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4.  gezinti menüsünde **Kullanıcılar ve gruplar'ı** tıklatın.

5.  **tüm kullanıcıları**tıklatın.

6.  İlgilendiğiniz kullanıcıyı **arayın** ve seçmek için **satırı tıklatın.**

7.  kullanıcının hangi gruplara üye olduğunu görmek için **Gruplar'ı** tıklatın.

### <a name="check-a-users-assigned-licenses"></a>Kullanıcının atanmış lisanslarını denetleme

Bir kullanıcının atanmış lisanslarını denetlemek için aşağıdaki adımları izleyin:

1.  Azure [**portalını**](https://portal.azure.com/) açın ve Global Administrator olarak oturum **açın.**

2.  Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3.  Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4.  gezinti menüsünde **Kullanıcılar ve gruplar'ı** tıklatın.

5.  **tüm kullanıcıları**tıklatın.

6.  İlgilendiğiniz kullanıcıyı **arayın** ve seçmek için **satırı tıklatın.**

7.  kullanıcının şu anda hangi lisansları atadığını görmek için **Lisanslar'ı** tıklatın.

### <a name="assign-a-user-a-license"></a>Kullanıcıya lisans atama 

Bir kullanıcıya lisans atamak için aşağıdaki adımları izleyin:

1.  Azure [**portalını**](https://portal.azure.com/) açın ve Global Administrator olarak oturum **açın.**

2.  Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3.  Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4.  gezinti menüsünde **Kullanıcılar ve gruplar'ı** tıklatın.

5.  **tüm kullanıcıları**tıklatın.

6.  İlgilendiğiniz kullanıcıyı **arayın** ve seçmek için **satırı tıklatın.**

7.  kullanıcının şu anda hangi lisansları atadığını görmek için **Lisanslar'ı** tıklatın.

8.  **Atla** düğmesini tıklatın.

9.  Kullanılabilir ürünler listesinden **bir veya daha fazla ürün** seçin.

10. Ürünleri parçalı olarak atamak için **atama seçenekleri** öğesini **isteğe bağlı** olarak tıklatın. Bu tamamlandığında **Tamam'ı** tıklatın.

11. Bu lisansları bu kullanıcıya atamak için **Atla** düğmesini tıklatın.

## <a name="problems-with-groups"></a>Gruplarla ilgili sorunlar

Uygulamaya atanan bir grupla ilgili bir sorun nedeniyle uygulama erişimi engellenebilir. Gruplar ve grup üyelikleriyle ilgili sorunları gidermenin ve çözebileceğiniz bazı yollar şunlardır:

-   [Grubun üyeliğini denetleme](#check-a-groups-membership)

-   [Dinamik bir grubun üyelik ölçütlerini kontrol edin](#check-a-dynamic-groups-membership-criteria)

-   [Grubun atanmış lisanslarını denetleme](#check-a-groups-assigned-licenses)

-   [Grubun lisanslarını yeniden işleme](#reprocess-a-groups-licenses)

-   [Gruba lisans atama](#assign-a-group-a-license)

### <a name="check-a-groups-membership"></a>Grubun üyeliğini denetleme

Bir grubun üyeliğini denetlemek için aşağıdaki adımları izleyin:

1.  Azure [**portalını**](https://portal.azure.com/) açın ve Global Administrator olarak oturum **açın.**

2.  Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3.  Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4.  gezinti menüsünde **Kullanıcılar ve gruplar'ı** tıklatın.

5.  **tüm grupları**tıklatın.

6.  İlgilendiğiniz grubu **arayın** ve seçmek için **satırı tıklatın.**

7.  bu gruba atanan kullanıcıların listesini gözden geçirmek için **Üyeler'i** tıklatın.

### <a name="check-a-dynamic-groups-membership-criteria"></a>Dinamik bir grubun üyelik ölçütlerini kontrol edin 

Dinamik bir grubun üyelik ölçütlerini denetlemek için aşağıdaki adımları izleyin:

1.  Azure [**portalını**](https://portal.azure.com/) açın ve Global Administrator olarak oturum **açın.**

2.  Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3.  Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4.  gezinti menüsünde **Kullanıcılar ve gruplar'ı** tıklatın.

5.  **tüm grupları**tıklatın.

6.  İlgilendiğiniz grubu **arayın** ve seçmek için **satırı tıklatın.**

7.  **Dinamik üyelik kurallarını tıklatın.**

8.  Bu grup için tanımlanan **basit** veya **gelişmiş** kuralı gözden geçirin ve bu grubun üyesi olmak istediğiniz kullanıcının bu ölçütleri karşıladığından emin olun.

### <a name="check-a-groups-assigned-licenses"></a>Grubun atanmış lisanslarını denetleme

Bir grubun atanmış lisanslarını denetlemek için aşağıdaki adımları izleyin:

1.  Azure [**portalını**](https://portal.azure.com/) açın ve Global Administrator olarak oturum **açın.**

2.  Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3.  Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4.  gezinti menüsünde **Kullanıcılar ve gruplar'ı** tıklatın.

5.  **tüm grupları**tıklatın.

6.  İlgilendiğiniz grubu **arayın** ve seçmek için **satırı tıklatın.**

7.  grubun şu anda hangi lisansları atadığını görmek için **Lisanslar'ı** tıklatın.

### <a name="reprocess-a-groups-licenses"></a>Grubun lisanslarını yeniden işleme

Bir grubun atanan lisanslarını yeniden işlemek için aşağıdaki adımları izleyin:

1. Azure [**portalını**](https://portal.azure.com/) açın ve Global Administrator olarak oturum **açın.**

2. Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3. Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4. gezinti menüsünde **Kullanıcılar ve gruplar'ı** tıklatın.

5. **tüm grupları**tıklatın.

6. İlgilendiğiniz grubu **arayın** ve seçmek için **satırı tıklatın.**

7. grubun şu anda hangi lisansları atadığını görmek için **Lisanslar'ı** tıklatın.

8. bu grubun üyelerine atanan lisansların güncel olduğundan emin olmak için **Yeniden İşlem** düğmesini tıklatın. Bu, grubun boyutuna ve karmaşıklığına bağlı olarak uzun zaman alabilir.

   >[!NOTE]
   >Bunu daha hızlı yapmak için, kullanıcıya doğrudan geçici olarak lisans atamayı düşünün. [Kullanıcıya lisans atayın.](#problems-with-application-consent)
   >
   >

### <a name="assign-a-group-a-license"></a>Gruba lisans atama

Bir gruba lisans atamak için aşağıdaki adımları izleyin:

1. Azure [**portalını**](https://portal.azure.com/) açın ve Global Administrator olarak oturum **açın.**

2. Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3. Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4. gezinti menüsünde **Kullanıcılar ve gruplar'ı** tıklatın.

5. **tüm grupları**tıklatın.

6. İlgilendiğiniz grubu **arayın** ve seçmek için **satırı tıklatın.**

7. grubun şu anda hangi lisansları atadığını görmek için **Lisanslar'ı** tıklatın.

8. **Atla** düğmesini tıklatın.

9. Kullanılabilir ürünler listesinden **bir veya daha fazla ürün** seçin.

10. Ürünleri parçalı olarak atamak için **atama seçenekleri** öğesini **isteğe bağlı** olarak tıklatın. Bu tamamlandığında **Tamam'ı** tıklatın.

11. Bu lisansları bu gruba atamak için **Atla** düğmesini tıklatın. Bu, grubun boyutuna ve karmaşıklığına bağlı olarak uzun zaman alabilir.

    >[!NOTE]
    >Bunu daha hızlı yapmak için, kullanıcıya doğrudan geçici olarak lisans atamayı düşünün. [Kullanıcıya lisans atayın.](#problems-with-application-consent)
    > 
    >

## <a name="problems-with-conditional-access-policies"></a>Koşullu Erişim ilkeleriyle ilgili sorunlar

### <a name="check-a-specific-conditional-access-policy"></a>Belirli bir Koşullu Erişim ilkesini denetleme

Tek bir Koşullu Erişim ilkesini denetlemek veya doğrulamak için:

1. Azure [**portalını**](https://portal.azure.com/) açın ve Global Administrator olarak oturum **açın.**

2. Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3. Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4. gezinti menüsünde **Kurumsal uygulamaları** tıklatın.

5. **Koşullu Erişim** gezinti öğesini tıklatın.

6. incelemek istediğiniz politikayı tıklatın.

7. Kullanıcı erişimini engelleyen belirli koşullar, atamalar veya diğer ayarlar olmadığını gözden geçirin.

   >[!NOTE]
   >Oturum açma ları etkilemediğinden emin olmak için bu ilkeyi geçici olarak devre dışı kılabilir. Bunu yapmak **için, Etkinleştir leilkesini** **Hayır'a** ayarlayın ve **Kaydet** düğmesini tıklatın.
   >
   >

### <a name="check-a-specific-applications-conditional-access-policy"></a>Belirli bir uygulamanın Koşullu Erişim ilkesini denetleme

Tek bir uygulamanın şu anda yapılandırılmış Koşullu Erişim ilkesini denetlemek veya doğrulamak için:

1.  Azure [**portalını**](https://portal.azure.com/) açın ve Global Administrator olarak oturum **açın.**

2.  Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3.  Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4.  gezinti menüsünde **Kurumsal uygulamaları** tıklatın.

5.  **tüm uygulamaları**tıklatın.

6.  İlgilendiğiniz uygulamayı arayın veya kullanıcı uygulama görüntülü adı veya uygulama kimliğiyle oturum açmaya çalışıyor.

     >[!NOTE]
     >Aradığınız uygulamayı görmüyorsanız, **Filtre düğmesini** tıklatın ve listenin kapsamını **Tüm uygulamalara**genişletin. Daha fazla sütun görmek istiyorsanız, uygulamalarınız için ek ayrıntılar eklemek için **Sütunlar** düğmesini tıklatın.
     >
     >

7.  **Koşullu Erişim** gezinti öğesini tıklatın.

8.  incelemek istediğiniz politikayı tıklatın.

9.  Kullanıcı erişimini engelleyen belirli koşullar, atamalar veya diğer ayarlar olmadığını gözden geçirin.

     >[!NOTE]
     >Oturum açma ları etkilemediğinden emin olmak için bu ilkeyi geçici olarak devre dışı kılabilir. Bunu yapmak **için, Etkinleştir leilkesini** **Hayır'a** ayarlayın ve **Kaydet** düğmesini tıklatın.
     >
     >

### <a name="disable-a-specific-conditional-access-policy"></a>Belirli bir Koşullu Erişim ilkesini devre dışı

Tek bir Koşullu Erişim ilkesini denetlemek veya doğrulamak için:

1.  Azure [**portalını**](https://portal.azure.com/) açın ve Global Administrator olarak oturum **açın.**

2.  Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3.  Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4.  gezinti menüsünde **Kurumsal uygulamaları** tıklatın.

5.  **Koşullu Erişim** gezinti öğesini tıklatın.

6.  incelemek istediğiniz politikayı tıklatın.

7.  **Etkinleştir lekme ilkesini** **Hayır'a** ayarlayarak ilkeyi devre dışı bırakıp **Kaydet** düğmesini tıklatın.

## <a name="problems-with-application-consent"></a>Uygulama onayı ile ilgili sorunlar

Uygun izinler onay işlemi gerçekleşmediği için uygulama erişimi engellenebilir. Uygulama onayı sorunlarını gidermenin ve çözebileceğiniz bazı yollar şunlardır:

-   [Kullanıcı düzeyinde bir onay işlemi gerçekleştirme](#perform-a-user-level-consent-operation)

-   [Herhangi bir uygulama için yönetici düzeyinde onay işlemi gerçekleştirin](#perform-administrator-level-consent-operation-for-any-application)

-   [Tek kiracılı bir uygulama için yönetici düzeyinde onay gerçekleştirme](#perform-administrator-level-consent-for-a-single-tenant-application)

-   [Çok kiracılı bir uygulama için yönetici düzeyinde onay gerçekleştirme](#perform-administrator-level-consent-for-a-multi-tenant-application)

### <a name="perform-a-user-level-consent-operation"></a>Kullanıcı düzeyinde bir onay işlemi gerçekleştirme

-   İzin isteyen açık kimlik bağlantısı etkin olan herhangi bir uygulama için, uygulamanın oturum açma ekranında gezinmek, oturum açmış kullanıcı için uygulamaya kullanıcı düzeyinde onay verir.

-   Bunu programlı bir şekilde yapmak istiyorsanız, [bkz.](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#requesting-individual-user-consent)

### <a name="perform-administrator-level-consent-operation-for-any-application"></a>Herhangi bir uygulama için yönetici düzeyinde onay işlemi gerçekleştirin

-   **Yalnızca V1 uygulama modeli kullanılarak geliştirilen uygulamalarda,** bir uygulamanın URL'deki işaretinin sonuna "**\_?prompt=admin consent**" ekleyerek bu yönetici düzeyindeki onayı almaya zorlayabilirsiniz.

-   **V2 uygulama modeli kullanılarak geliştirilen herhangi**bir uygulama için, yönetici onayı bitiş noktasını **kullanmanın bir dizin yöneticisi bölümünden izinleri talep** etme altındaki yönergeleri izleyerek bu yönetici düzeyinde ki [onayı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint)uygulayabilirsiniz.

### <a name="perform-administrator-level-consent-for-a-single-tenant-application"></a>Tek kiracılı bir uygulama için yönetici düzeyinde onay gerçekleştirme

-   İzin isteyen **tek kiracılı uygulamalar** için (kuruluşunuzda geliştirdiğiniz veya sahip olduğunuz uygulamalar gibi), Global Administrator olarak oturum açarak ve Uygulama Kayıt Defteri'nin üst kısmındaki **Hibe izinleri** düğmesini tıklatarak tüm kullanıcılar adına **yönetim düzeyinde** bir onay işlemi gerçekleştirebilirsiniz **&gt; - Tüm Uygulamalar&gt; - Bir Uygulama Seçin -&gt; Gerekli İzinler** bölmesi.

-   **V1 veya V2 uygulama modeli kullanılarak geliştirilen herhangi**bir uygulama için, yönetici onayı bitiş noktasını **kullanmanın bir dizin yöneticisi bölümünden izinleri Talep** altında yönergeleri izleyerek bu yönetici düzeyinde [onay](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint)ını uygulayabilirsiniz.

### <a name="perform-administrator-level-consent-for-a-multi-tenant-application"></a>Çok kiracılı bir uygulama için yönetici düzeyinde onay gerçekleştirme

-   İzin isteyen **çok kiracılı uygulamalar** için (üçüncü bir tarafın geliştirdiği bir uygulama veya Microsoft gibi), **yönetim düzeyinde** bir onay işlemi gerçekleştirebilirsiniz. Global Yönetici olarak oturum açın ve Kurumsal Uygulamalar altında **Hibe izinleri** düğmesini tıklatarak **-&gt; Tüm Uygulamalar&gt; - Bir Uygulama Seçin -&gt; İzinler** bölmesi (yakında kullanılabilir).

-   Yönetici [onayı bitiş noktasını kullanmanın](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint) **bir dizin yöneticisi bölümünden izinleri İste yönergesini** izleyerek bu yönetici düzeyindeki onayı da uygulayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
[Yönetici onayı bitiş noktasını kullanma](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint)

