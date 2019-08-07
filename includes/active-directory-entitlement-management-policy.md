---
title: include dosyası
description: include dosyası
services: active-directory
author: rolyon
ms.service: active-directory
ms.topic: include
ms.date: 07/31/2019
ms.author: rolyon
ms.custom: include file
ms.openlocfilehash: db16a2f122da1bf6c767e0a47c93c22f1882c406
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68817255"
---
### <a name="policy-for-users-in-your-directory"></a>İlke: Dizininizdeki kullanıcılar için

İlkenizin bu erişim paketini isteyebilmek için dizininizdeki kullanıcılar için olmasını istiyorsanız bu adımları izleyin.  **Dizininizdeki kullanıcılar** hem dahili kullanıcılara hem de daha önce dizine davet edilen dış kullanıcılara, başka bir erişim paketiyle ya da Azure AD İlkeyi tanımlarken, bireysel kullanıcıları veya daha yaygın kullanıcı gruplarını belirtebilirsiniz. Örneğin, kuruluşunuz **tüm çalışanlar**gibi bir gruba zaten sahip olabilir.  Bu grup, erişim isteyebilen kullanıcılar için ilkeye eklenirse, o grubun herhangi bir üyesi daha sonra erişim isteğinde bulunabilir.

1. **Erişim isteyebilen kullanıcılar** bölümünde, **dizininizdeki kullanıcılar için**öğesini seçin.

    **Dizininizdeki kullanıcılar için** , dizininize eklenen hem üye kullanıcıları hem de Konuk kullanıcıları içerdiğini unutmayın. Yalnızca üye kullanıcıları dahil etmek ve konuk kullanıcıları eklemek istiyorsanız, **dizininizdeki kullanıcılar için** öğesini seçin ve ardından üye kullanıcılarınızın bir grubunu seçin. Gerekirse, üye kullanıcılarınızın dinamik bir grubunu oluşturabilirsiniz (Kullanıcı. userType-EQ "üye"). Daha fazla bilgi için bkz. [Azure Active Directory gruplar Için dinamik üyelik kuralları](../articles/active-directory/users-groups-roles/groups-dynamic-membership.md).

1. **Kullanıcıları ve grupları seç** bölümünde, **Kullanıcı ve Grup Ekle**' ye tıklayın.

1. Kullanıcıları ve grupları seç bölmesinde, eklemek istediğiniz kullanıcıları ve grupları seçin.

    ![Erişim paketi-Ilke-kullanıcıları ve grupları seçin](./media/active-directory-entitlement-management-policy/policy-select-users-groups.png)

1. Kullanıcıları ve grupları eklemek için **Seç** ' e tıklayın.

1. [İlkeye atlayın: İstek](#policy-request) bölümü.

### <a name="policy-for-users-not-in-your-directory"></a>İlke: Dizininizde bulunmayan kullanıcılar için

İlkenizin bu erişim paketini isteyebilmek için dizininizde olmayan kullanıcılar için olmasını istiyorsanız bu adımları izleyin. **Dizininizde olmayan kullanıcılar** , başka BIR Azure AD dizinindeki kullanıcılara başvurur ve henüz dizininizden davet edilmemiş olabilir.  Dizinler, **Kurumsal ilişkiler işbirliği kısıtlamaları** ayarlarında izin verilecek şekilde yapılandırılmalıdır.

> [!NOTE]
> Henüz, isteğiniz onaylanmış veya otomatik olarak onaylanan dizininizde olmayan bir kullanıcı için bir konuk dış Kullanıcı hesabı oluşturulacaktır. Konuk davet edilecek, ancak davet e-postası almacaktır. Bunun yerine, bunlara erişim paketi ataması teslim edildiğinde bir e-posta gönderilir. Varsayılan olarak, Konuk kullanıcının artık herhangi bir erişim paketi ataması olmadığında, son atamasının süresi dolduğunda veya iptal edildiğinden, bu Konuk Kullanıcı hesabının oturum açması engellenir ve daha sonra silinir. Konuk kullanıcılarınızın, hiçbir erişim paketi ataması olmasa bile dizininizden sonsuza kadar kalmasını istiyorsanız, Yetkilendirme Yönetimi yapılandırmanızın ayarlarını değiştirebilirsiniz.

1. **Erişim isteyebilen kullanıcılar** bölümünde, **dizininizde olmayan kullanıcılar için**öğesini seçin.

1. **Dış Azure ad dizini seçin** bölümünde, **Dizin Ekle**' ye tıklayın.

1. Bir etki alanı adı girin ve bu etki alanı adına sahip bir Azure AD dizini arayın.

1. Belirtilen dizin adı ve ilk etki alanı tarafından doğru dizin olduğunu doğrulayın.

    > [!NOTE]
    > Dizindeki tüm kullanıcılar bu erişim paketini isteyebilecektir. Bu, yalnızca aramada kullanılan etki alanını değil, dizinle ilişkili tüm alt etki alanlarından gelen kullanıcıları içerir.

    ![Erişim paketi-Ilke-dizinleri seçin](./media/active-directory-entitlement-management-policy/policy-select-directories.png)

1. Dizin eklemek için **Ekle** ' ye tıklayın.

1. Daha fazla dizin eklemek için bu adımı tekrarlayın.

1. İlkeye dahil etmek istediğiniz tüm dizinleri ekledikten sonra **Seç**' e tıklayın.

1. [İlkeye atlayın: İstek](#policy-request) bölümü.

### <a name="policy-none-administrator-direct-assignments-only"></a>İlke: Hiçbiri (yalnızca yönetici doğrudan atamaları)

İlkenizin erişim isteklerini atlayıp yöneticilerin erişim paketine doğrudan belirli kullanıcıları atamasını istiyorsanız bu adımları izleyin. Kullanıcıların erişim paketini istemesi gerekmez. Süre sonu ayarlarını ayarlamaya devam edebilirsiniz, ancak hiçbir istek ayarı yoktur.

1. **Erişim Isteyebilen kullanıcılar** bölümünde Hiçbiri ' ni **(yalnızca yönetici doğrudan atamaları**) seçin.

    Erişim paketini oluşturduktan sonra, erişim paketine belirli iç ve dış kullanıcıları doğrudan atayabilirsiniz. Bir dış Kullanıcı belirtirseniz, dizininizde bir Konuk Kullanıcı hesabı oluşturulur.

1. [İlkeye atlayın: Süre](#policy-expiration) sonu bölümü.

### <a name="policy-request"></a>İlke: İstek

Istek bölümünde, kullanıcıların erişim paketini istemesi durumunda onay ayarlarını belirtirsiniz.

1. Seçili kullanıcılardan gelen isteklere onay gerektirmek için **onay gerektir** seçeneğini **Evet**olarak ayarlayın. İsteklerin otomatik olarak onaylanmasını sağlamak için, geçiş seçeneğini **Hayır**olarak ayarlayın.

1. Onay gerekiyorsa, **onaylayanları Seç** bölümünde, **onaylayanları Ekle**' ye tıklayın.

1. Onaylayanları seç bölmesinde, bir veya daha fazla kullanıcıyı ve/veya grubu onaylayanlara seçin.

    Seçili onaylayanlardan yalnızca birinin bir isteği onaylaması gerekir. Tüm onaylayanlardan onay gerekli değildir. Onay kararı, onaylayan kullanıcının isteği gözden geçirmelerine göre belirlenir.

    ![Erişim paketi-Ilke-onaylayanları Seç](./media/active-directory-entitlement-management-policy/policy-select-approvers.png)

1. Onaylayanları eklemek için **Seç** ' e tıklayın.

1. Ek ayarları göstermek için **Gelişmiş istek ayarlarını göster** ' e tıklayın.

    ![Erişim paketi-Ilke-dizinleri seçin](./media/active-directory-entitlement-management-policy/policy-advanced-request.png)

1. Kullanıcıların erişim paketi istemek için bir gerekçe sağlamasını gerektirmek için, **gerekçe iste** seçeneğini **Evet**olarak ayarlayın.

1. Onaylayanın erişim paketine yönelik bir isteği onaylamaya yönelik bir gerekçe sağlamasını gerektirmek için, **onaylayan gerekçe iste** seçeneğini **Evet**olarak ayarlayın.

1. **Onay isteği zaman aşımı (gün)** kutusunda, onaylayanların bir isteği gözden geçirmesi gereken süre miktarını belirtin. Bu gün sayısında hiçbir onaylayan gözden geçiremez, isteğin süresi dolar ve kullanıcının erişim paketi için başka bir istek göndermesi gerekir.

### <a name="policy-expiration"></a>İlke: Süre sonu

Süre sonu bölümünde, bir kullanıcının erişim paketine atamanın ne zaman sona ereceğini belirtirsiniz.

1. **Süre sonu** bölümünde, **erişim paketi** bitiş tarihi ' ni **Tarih**, **gün sayısı**veya **hiçbir zaman**olarak ayarlayın.

    **Tarih**açısından, gelecekte bir sona erme tarihi seçin.

    **Gün sayısı**için 0 ila 3660 gün arasında bir sayı belirtin.

    Seçiminize bağlı olarak, bir kullanıcının erişim paketine atanması belirli bir tarihte, belirli bir gün sonra onaylandıktan sonra veya hiçbir zaman sona erer.

1. Ek ayarları göstermek için **Gelişmiş süre sonu ayarlarını göster** ' e tıklayın.

1. Kullanıcının atamalarını genişletmesine izin vermek için **kullanıcıların erişimi genişlemesine Izin ver** seçeneğini belirleyin.

    İlkede uzantılara izin veriliyorsa, Kullanıcı bir e-posta 14 gün ve ayrıca erişim paketi atamasının, atamayı genişletmesini istemek üzere süre sonu olarak ayarlanmayacak şekilde 1 gün alır.

    ![Erişim paketi-Ilke-süre sonu ayarları](./media/active-directory-entitlement-management-policy/policy-expiration.png)

### <a name="policy-enable-policy"></a>İlke: İlkeyi etkinleştirme

1. Erişim paketinin ilkedeki kullanıcılar için hemen kullanılabilir olmasını istiyorsanız, ilkeyi etkinleştirmek için **Evet** ' i tıklatın.

    Erişim paketini oluşturma işlemi bittikten sonra bunu gelecekte etkinleştirebilirsiniz.

    ![Erişim paketi-Ilke-ilke ayarını etkinleştir](./media/active-directory-entitlement-management-policy/policy-enable.png)

1. **İleri** veya **Oluştur**' a tıklayın.
