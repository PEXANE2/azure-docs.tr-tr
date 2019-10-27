---
title: include dosyası
description: include dosyası
services: active-directory
author: msaburnley
ms.service: active-directory
ms.topic: include
ms.date: 10/15/2019
ms.author: ajburnle
ms.custom: include file
ms.openlocfilehash: 78a0dafeedc9aac4db69903b9f1193574cbd39c7
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2019
ms.locfileid: "72934700"
---
## <a name="for-users-in-your-directory"></a>Dizininizdeki kullanıcılar için

Dizininizdeki kullanıcıların bu erişim paketini isteyebilmesini sağlamak istiyorsanız bu adımları izleyin. İstek ilkesini tanımlarken, bireysel kullanıcıları veya daha yaygın kullanıcı gruplarını belirtebilirsiniz. Örneğin, kuruluşunuz **tüm çalışanlar**gibi bir gruba zaten sahip olabilir.  Bu grup, erişim isteyebilen kullanıcılar için ilkeye eklenirse, o grubun herhangi bir üyesi daha sonra erişim isteğinde bulunabilir.

1. **Erişim isteyebilen kullanıcılar** bölümünde, **dizininizdeki kullanıcılar için**öğesine tıklayın.

    Bu seçeneği belirlediğinizde, dizininizdeki kim tarafından bu erişim paketini isteyebileceğiniz konusunda daha fazla belirginleştirmek için yeni seçenekler görünür.

    ![Erişim paketi Istekleri-dizininizdeki kullanıcılar Için](./media/active-directory-entitlement-management-request-policy/for-users-in-your-directory.png)

1. Aşağıdaki seçeneklerden birini belirleyin:

    |  |  |
    | --- | --- |
    | **Belirli kullanıcılar ve gruplar** | Yalnızca dizininizdeki Kullanıcı ve grupların bu erişim paketini isteyebilmesini istiyorsanız bu seçeneği belirleyin. |
    | **Tüm Üyeler (konuklar hariç)** | Dizininizdeki tüm üye kullanıcılarının bu erişim paketini isteyebilmesini istiyorsanız bu seçeneği belirleyin. Bu seçenek, dizininize davet ettiğiniz herhangi bir konuk kullanıcıyı içermez. |
    | **Tüm kullanıcılar (konuklar dahil)** | Dizininizdeki tüm üye kullanıcılarının ve konuk kullanıcıların bu erişim paketini isteyebilmesini istiyorsanız bu seçeneği belirleyin. |

    Konuk kullanıcılar, [Azure AD B2B](../articles/active-directory/b2b/what-is-b2b.md)ile dizininize davet edilen dış kullanıcılara başvurur. Üye kullanıcılar ve Konuk kullanıcılar arasındaki farklar hakkında daha fazla bilgi için bkz. [Azure Active Directory varsayılan kullanıcı Izinleri nelerdir?](../articles/active-directory/fundamentals/users-default-permissions.md).

1. **Belirli kullanıcılar ve gruplar**' ı seçtiyseniz, **Kullanıcı ve Grup Ekle**' ye tıklayın.

1. Kullanıcıları ve grupları seç bölmesinde, eklemek istediğiniz kullanıcıları ve grupları seçin.

    ![Erişim paketi Istekleri-kullanıcıları ve grupları seçin](./media/active-directory-entitlement-management-request-policy/select-users-groups.png)

1. Kullanıcıları ve grupları eklemek için **Seç** ' e tıklayın.

1. [Onay](#approval) bölümüne atlayın.

## <a name="for-users-not-in-your-directory"></a>Dizininizde bulunmayan kullanıcılar için

Dizininizde bulunmayan kullanıcıların bu erişim paketini isteyebilmesini sağlamak istiyorsanız bu adımları izleyin. **Dizininizde olmayan kullanıcılar** , başka BIR Azure ad dizini veya etki alanındaki kullanıcılara başvurur ve henüz dizininizden davet edilmemiş olabilir. Azure AD dizinleri **işbirliği kısıtlamalarında**davetlere izin verecek şekilde yapılandırılmalıdır. Daha fazla bilgi için bkz. [B2B dış Işbirliğini etkinleştirme ve konukları kimlerin davet edebileceğini yönetme](../articles/active-directory/b2b/delegate-invitations.md).

> [!NOTE]
> Henüz, isteğiniz onaylanmış veya otomatik olarak onaylanan dizininizde olmayan bir kullanıcı için bir Konuk Kullanıcı hesabı oluşturulur. Konuk davet edilecek, ancak davet e-postası almacaktır. Bunun yerine, bunlara erişim paketi ataması teslim edildiğinde bir e-posta gönderilir. Varsayılan olarak, Konuk kullanıcının artık herhangi bir erişim paketi ataması olmadığında, son atamasının süresi dolduğunda veya iptal edildiğinden, bu Konuk Kullanıcı hesabının oturum açması engellenir ve daha sonra silinir. Konuk kullanıcılarınızın, hiçbir erişim paketi ataması olmasa bile dizininizden sonsuza kadar kalmasını istiyorsanız, Yetkilendirme Yönetimi yapılandırmanızın ayarlarını değiştirebilirsiniz. Konuk Kullanıcı nesnesi hakkında daha fazla bilgi için, [Azure ACTIVE DIRECTORY B2B işbirliği kullanıcısının özelliklerine](../articles/active-directory/b2b/user-properties.md)bakın.

1. **Erişim isteyebilen kullanıcılar** bölümünde, **dizininizde olmayan kullanıcılar için**öğesine tıklayın.

    Bu seçeneği belirlediğinizde yeni seçenekler görüntülenir.

    ![Erişim paketi Istekleri-dizininizde olmayan kullanıcılar Için](./media/active-directory-entitlement-management-request-policy/for-users-not-in-your-directory.png)

1. Aşağıdaki seçeneklerden birini belirleyin:

    |  |  |
    | --- | --- |
    | **Belirli bağlı kuruluşlar** | Yöneticinizin daha önce eklediği kuruluşların listesinden seçim yapmak istiyorsanız bu seçeneği belirleyin. Seçili kuruluşların tüm kullanıcıları, bu erişim paketini isteyebilir. |
    | **Tüm bağlı kuruluşlar** | Tüm bağlı kuruluşlardaki tüm kullanıcıların bu erişim paketini isteyebilmesini istiyorsanız bu seçeneği belirleyin. |

    Bağlı bir kuruluş, ile ilişkiniz olan bir dış Azure AD dizini veya etki alanıdır.

1. **Belirli bağlı kuruluşları**seçtiyseniz, yöneticinizin daha önce eklediği bağlı kuruluşların listesinden seçmek Için **Dizin Ekle** ' ye tıklayın.

1. Daha önce bağlı bir kuruluşun aranacağı adı veya etki alanı adını yazın.

    ![Erişim paketi-Istekler-Dizin Seçme](./media/active-directory-entitlement-management-request-policy/select-directories.png)

    İşbirliği yapmak istediğiniz kuruluş listede yoksa, yöneticinizden onu bağlı bir kuruluş olarak eklemesini isteyebilirsiniz. Daha fazla bilgi için bkz. [bağlı kuruluş ekleme](../articles/active-directory/governance/entitlement-management-organization.md).

1. Bağlı Kuruluşlarınızın tümünü seçtikten sonra **Seç**' e tıklayın.

    > [!NOTE]
    > Seçili bağlı kuruluşların tüm kullanıcıları bu erişim paketini isteyebilecektir. Bu, Azure AD 'deki kullanıcıları, bu etki alanları Azure B2B izin verilenler veya reddetme listesi tarafından engellenmediği durumlar dışında, kuruluş ile ilişkili tüm alt etki alanlarından de içerir. Daha fazla bilgi için, bkz. [belirli kuruluşlardan B2B kullanıcılarına Izin verme veya bu kullanıcıların davetlerini engelleme](../articles/active-directory/b2b/allow-deny-list.md).

1. [Onay](#approval) bölümüne atlayın.

## <a name="none-administrator-direct-assignments-only"></a>Hiçbiri (yalnızca yönetici doğrudan atamaları)

Erişim isteklerini atlamak ve yöneticilerin bu erişim paketine belirli kullanıcıları doğrudan atamasını sağlamak istiyorsanız bu adımları izleyin. Kullanıcıların erişim paketini istemesi gerekmez. Yaşam döngüsü ayarlarını ayarlamaya devam edebilirsiniz, ancak hiçbir istek ayarı yoktur.

1. **Erişim Isteyebilen kullanıcılar** bölümünde Hiçbiri ' ne tıklayın **(yalnızca yönetici doğrudan atamaları**).

    ![Erişim paketi-Istekler-hiçbiri yönetici yalnızca doğrudan atamalar](./media/active-directory-entitlement-management-request-policy/none-admin-direct-assignments-only.png)

    Erişim paketini oluşturduktan sonra, erişim paketine belirli iç ve dış kullanıcıları doğrudan atayabilirsiniz. Bir dış Kullanıcı belirtirseniz, dizininizde bir Konuk Kullanıcı hesabı oluşturulur. Bir kullanıcıyı doğrudan atama hakkında daha fazla bilgi için bkz. [bir erişim paketi için atamaları görüntüleme, ekleme ve kaldırma](../articles/active-directory/governance/entitlement-management-access-package-assignments.md).

1. [Istekleri etkinleştir](#enable-requests) bölümüne atlayın.

## <a name="approval"></a>Onay

Onay bölümünde, kullanıcılar bu erişim paketini istemesi durumunda bir onayın gerekli olup olmadığını belirtirsiniz. Onay ayarları aşağıdaki şekilde çalışır:

- Seçili onaylayanlardan veya geri dönüş onaylayanlardan yalnızca birinin bir isteği onaylaması gerekir. Tüm onaylayanlardan onay gerekli değildir.
- Onay kararı, onaylayan kullanıcının isteği gözden geçirmelerine göre belirlenir.

Daha önce seçtiğiniz kullanıcıların onay ayarlarını belirtmek için bu adımları izleyin.

1. Seçili kullanıcılardan gelen isteklere onay gerektirmek için **onay gerektir** seçeneğini **Evet**olarak ayarlayın. İsteklerin otomatik olarak onaylanmasını sağlamak için, geçiş seçeneğini **Hayır**olarak ayarlayın.

    ![Erişim paketi Istekleri-onay ayarları](./media/active-directory-entitlement-management-request-policy/approval.png)

1. Kullanıcıların erişim paketi istemek için bir gerekçe sağlamasını gerektirmek için, **istek sahibine iste** modunu **Evet**olarak ayarlayın.

1. İsteğin tek veya çok aşamalı onay gerektirdiğini belirleme. Tek aşamalı için **kaç aşamanın** **1** ' e geçiş yapıldığını ayarlayın.

1. Onaylayanlar için, **onaylayan olarak yönetici** ' yi seçin veya **belirli onaylayanları**seçin.

    Yönetici, kullanıcının Azure AD profilinde **yönetici** özniteliği tarafından belirlenir. Daha fazla bilgi için, bkz. [Azure Active Directory kullanarak kullanıcının profil bilgilerini ekleme veya güncelleştirme](../articles/active-directory/fundamentals/active-directory-users-profile-azure-portal.md).

    ![Kullanıcı profili-Yöneticisi özniteliği Azure Active Directory](./media/active-directory-entitlement-management-request-policy/profile-manager.png)

1. Yöneticiyi onaylayan olarak seçtiyseniz, yetkilendirme yönetiminin yöneticiyi bulamamasını sağlamak için dizininizdeki bir veya daha fazla Kullanıcı ya da Grup seçmek üzere **geri dönüş Ekle** ' ye tıklayın.

1. Belirli onaylayanları Seç ' i seçtiyseniz, dizininizdeki bir veya daha fazla Kullanıcı ya da grubun onaylayanlarını seçmek için **onaylayan Ekle** ' ye tıklayın.

1. **Kararın kaç gün içinde yapılması gerekir?** , bir onaylayanın bu erişim paketi için bir isteği gözden geçirmesi gereken gün sayısını belirtin.

    Bu süre içinde bir istek onaylanmamışsa, otomatik olarak reddedilir. Kullanıcının erişim paketi için başka bir istek göndermesi gerekir.

1. Kullanıcıların erişim paketi istemek için bir gerekçe sağlamasını gerektirmek için, **gerekçe iste** seçeneğini **Evet**olarak ayarlayın.

    Bir gerekçe diğer onaylayanlara ve istek sahibine görünür.

## <a name="enable-requests"></a>İstekleri etkinleştir

1. Erişim paketinin istek ilkesindeki kullanıcılar için hemen kullanılabilir olmasını istiyorsanız, etkinleştirmek için **Evet** ' i tıklatın.

    Erişim paketini oluşturma işlemi bittikten sonra bunu gelecekte etkinleştirebilirsiniz.

    Hiçbiri ' ni seçtiyseniz **(yalnızca yönetici doğrudan atamaları)** ve Etkinleştir ' i ayarlarsanız **, Yöneticiler**bu erişim paketini doğrudan atayamazlar.

    ![Erişim paketi-Ilke-ilke ayarını etkinleştir](./media/active-directory-entitlement-management-request-policy/enable-requests.png)

1. **İleri**’ye tıklayın.
