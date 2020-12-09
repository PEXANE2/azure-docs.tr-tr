---
title: Azure Güvenlik Merkezi için kiracı genelinde görünürlük elde edin | Microsoft Docs
description: Bu makalede, Azure Active Directory kiracınızla bağlantılı tüm aboneliklere ilke uygulayarak güvenlik duruşunuzu ölçekte nasıl yöneteceğiniz açıklanmaktadır.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: b85c0e93-9982-48ad-b23f-53b367f22b10
ms.service: security-center
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/07/2020
ms.author: memildin
ms.openlocfilehash: d03177e3224bbd3f53320871efc6a0d6b3ea479d
ms.sourcegitcommit: fec60094b829270387c104cc6c21257826fccc54
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2020
ms.locfileid: "96922645"
---
# <a name="organize-management-groups-subscriptions-and-tenant-wide-visibility"></a>Yönetim gruplarını, abonelikleri ve kiracı genelinde görünürlüğü düzenleme

Bu makalede, Azure Active Directory kiracınızla bağlantılı tüm Azure aboneliklerine güvenlik ilkeleri uygulayarak kuruluşunuzun güvenlik duruşunu nasıl yöneteceğiniz açıklanmaktadır.

Azure AD kiracısında kayıtlı olan tüm aboneliklerin güvenlik duruşunu görünürlüğünü almak için, kök yönetim grubuna yeterli okuma izinleri olan bir Azure rolünün atanması gerekir.


## <a name="organize-your-subscriptions-into-management-groups"></a>Aboneliklerinizi yönetim gruplarında düzenleme

### <a name="introduction-to-management-groups"></a>Yönetim gruplarına giriş

Azure Yönetim grupları, abonelik gruplarında erişimi, ilkeleri ve raporlamayı verimli bir şekilde yönetebilir ve kök yönetim grubu üzerinde eylemler gerçekleştirerek Azure 'un tüm özelliklerini etkin bir şekilde yönetebilme olanağı sağlar. Abonelikleri yönetim gruplarında düzenleyebilir ve idare ilkelerinizi yönetim gruplarına uygulayabilirsiniz. Bir yönetim grubu içindeki aboneliklerin tümü otomatik olarak yönetim grubuna uygulanmış olan ilkeleri devralır. 

Her Azure AD kiracısına **kök yönetim grubu** adlı tek bir en üst düzey yönetim grubu verilir. Diğer tüm yönetim grupları ve abonelikler hiyerarşide en üstte yer alan bu kök yönetim grubunun altındadır. Bu grup, genel ilkelerin ve Azure rol atamalarının dizin düzeyinde uygulanmasını sağlar. 

Kök yönetim grubu, aşağıdaki eylemlerden herhangi birini gerçekleştirdiğinizde otomatik olarak oluşturulur: 
- Azure portal **Yönetim grupları** açın. [Azure portal](https://portal.azure.com)
- API çağrısıyla bir yönetim grubu oluşturun.
- PowerShell ile bir yönetim grubu oluşturun. PowerShell yönergeleri için bkz. [kaynak ve kuruluş yönetimi için yönetim grupları oluşturma](../governance/management-groups/create-management-group-portal.md).

Güvenlik Merkezi 'ni eklemek için yönetim grupları gerekmez, ancak kök yönetim grubunun oluşturulması için en az bir tane oluşturmanız önerilir. Grup oluşturulduktan sonra, Azure AD kiracınız kapsamındaki tüm abonelikler buna bağlanır. 


Yönetim gruplarına ayrıntılı bir genel bakış için bkz. [Azure Yönetim gruplarıyla kaynaklarınızı düzenleme](../governance/management-groups/overview.md) makalesi.

### <a name="view-and-create-management-groups-in-the-azure-portal"></a>Azure portal Yönetim gruplarını görüntüleyin ve oluşturun

1. [Azure portal](https://portal.azure.com) **Yönetim grupları** bulmak ve açmak için üstteki çubukta arama kutusunu kullanın.

    :::image type="content" source="./media/security-center-management-groups/open-management-groups-service.png" alt-text="Yönetim gruplarınıza erişme":::

    Yönetim gruplarınızın listesi görüntülenir.

1. Bir yönetim grubu oluşturmak için **Yönetim grubu Ekle**' yi seçin, ilgili ayrıntıları girin ve **Kaydet**' i seçin.

    :::image type="content" source="media/security-center-management-groups/add-management-group.png" alt-text="Azure 'a bir yönetim grubu ekleme":::

    - **Yönetim grubu kimliği** , bu yönetim grubundaki komutları göndermek için kullanılan dizin benzersiz tanımlayıcısıdır. Bu tanımlayıcı, bu grubu tanımlamak için Azure sistem genelinde kullanıldığından oluşturulduktan sonra düzenlenebilir değildir. 
    - Görünen ad alanı Azure portal içinde görüntülenen addır. Ayrı bir görünen ad, yönetim grubu oluşturulurken isteğe bağlı bir alandır ve herhangi bir zamanda değiştirilebilir.  


### <a name="add-subscriptions-to-a-management-group"></a>Bir yönetim grubuna abonelik ekleme
Oluşturduğunuz yönetim grubuna abonelikler ekleyebilirsiniz.

1. **Yönetim grupları** altında, aboneliğiniz için yönetim grubunu seçin.

    :::image type="content" source="./media/security-center-management-groups/management-group-subscriptions.png" alt-text="Aboneliğiniz için bir yönetim grubu seçin":::

1. Grubun sayfası açıldığında, **Ayrıntılar** ' ı seçin.

    :::image type="content" source="./media/security-center-management-groups/management-group-details-page.png" alt-text="Yönetim grubunun Ayrıntılar sayfasını açma":::

1. Grubun ayrıntılar sayfasında, **Abonelik Ekle**' yi seçin, ardından aboneliklerinizi seçin ve **Kaydet**' i seçin. Kapsama içindeki tüm abonelikleri ekleyinceye kadar tekrarlayın.

    :::image type="content" source="./media/security-center-management-groups/management-group-add-subscriptions.png" alt-text="Bir yönetim grubuna abonelik ekleme":::
   > [!IMPORTANT]
   > Yönetim gruplarında hem abonelikler hem de alt yönetim grupları bulunabilir. Bir kullanıcıyı bir Azure rolünü üst yönetim grubuna atadığınızda, erişim alt yönetim grubunun abonelikleri tarafından devralınır. Üst yönetim grubunda ayarlanan ilkeler alt öğeler tarafından da devralınır. 


## <a name="grant-tenant-wide-permissions-to-yourself"></a>Kendi kendinize kiracı genelinde izinler verin

**Genel yönetici** Azure Active Directory rolüne sahip bir kullanıcının kiracı genelinde sorumlulukları olabilir, ancak Azure Güvenlik Merkezi 'nde kuruluş genelinde bilgileri görüntülemek için Azure izinlerinin olmaması gerekir. 

> [!TIP]
> Kuruluşunuz [Azure AD Privileged Identity Management (PIM)](../active-directory/privileged-identity-management/pim-configure.md)veya başka bir PIM aracıyla kaynak erişimini yönetirse, bu değişiklikleri yapan kullanıcı için genel yönetici rolü etkin olmalıdır.

Kendi kiracı düzeyi izinlerini atamak için:

1. Kiracının kök yönetim grubu üzerinde atama olmadan genel yönetici kullanıcı olarak, güvenlik merkezi 'nin **genel bakış** sayfasını açın ve başlıktaki **kiracı genelinde görünürlük** bağlantısını seçin. 

    :::image type="content" source="media/security-center-management-groups/enable-tenant-level-permissions-banner.png" alt-text="Azure Güvenlik Merkezi 'nde kiracı düzeyindeki izinleri etkinleştirme":::

1. Atanacak yeni Azure rolünü seçin. 

    :::image type="content" source="media/security-center-management-groups/enable-tenant-level-permissions-form.png" alt-text="Kullanıcıya atanacak kiracı düzeyi izinleri tanımlamak için form":::

    > [!TIP]
    > Genellikle güvenlik yöneticisi rolü kök düzeyinde uygulamak için gereklidir, ancak güvenlik okuyucusu kiracı düzeyinde görünürlük sağlamak için yeterli olacaktır. Bu rollerin verdiği izinler hakkında daha fazla bilgi için bkz. [Güvenlik Yöneticisi yerleşik rol açıklaması](../role-based-access-control/built-in-roles.md#security-admin) veya [güvenlik okuyucusu yerleşik rol açıklaması](../role-based-access-control/built-in-roles.md#security-reader).
    >
    > Güvenlik Merkezi 'ne özgü bu roller arasındaki farklar için, [rollerdeki tabloya ve izin verilen eylemlere](security-center-permissions.md#roles-and-allowed-actions)bakın.

    Kuruluş genelinde görünüm, kiracının kök yönetim grubu düzeyine roller verilerek elde edilir.  

1. Azure portal oturumunuzu kapatıp yeniden oturum açın.

1. Erişimi yükselttikten sonra Azure AD kiracınız kapsamındaki tüm aboneliklerde görünürlük olduğunu doğrulamak için Azure Güvenlik Merkezi 'ni açın veya yenileyin. 

## <a name="assign-azure-roles-to-other-users"></a>Diğer kullanıcılara Azure rolleri atama

### <a name="assign-azure-roles-to-users-through-the-azure-portal"></a>Azure portal aracılığıyla kullanıcılara Azure rolleri atama: 
1. [Azure portalında](https://portal.azure.com) oturum açın. 
1. Yönetim gruplarını görüntülemek için Azure ana menüsünde **tüm hizmetler** ' i seçin ve **Yönetim grupları**' yi seçin.
1.  Bir yönetim grubu seçin ve **Ayrıntılar**' ı seçin.

    :::image type="content" source="./media/security-center-management-groups/management-group-details.PNG" alt-text="Ayrıntılar Yönetim Grupları ekran görüntüsü":::

1. **Erişim denetimi (IAM)** ve **rol atamaları**' nı seçin.
1. **Rol ataması ekle**’yi seçin.
1. Atanacak rolü ve Kullanıcı ' yı seçin ve ardından **Kaydet**' i seçin.  
   
   ![Güvenlik okuyucusu rolü Ekle ekran görüntüsü](./media/security-center-management-groups/asc-security-reader.png)

### <a name="assign-azure-roles-to-users-with-powershell"></a>PowerShell ile kullanıcılara Azure rolleri atama: 
1. [Azure PowerShell](/powershell/azure/install-az-ps)'i yükler.
2. Aşağıdaki komutları çalıştırın: 

    ```azurepowershell
    # Login to Azure as a Global Administrator user
    Connect-AzAccount
    ```

3. İstendiğinde, genel yönetici kimlik bilgileriyle oturum açın. 

    ![Oturum açma istemi ekran görüntüsü](./media/security-center-management-groups/azurerm-sign-in.PNG)

4. Aşağıdaki komutu çalıştırarak okuyucu rolü izinleri verin:

    ```azurepowershell
    # Add Reader role to the required user on the Root Management Group
    # Replace "user@domian.com” with the user to grant access to
    New-AzRoleAssignment -SignInName "user@domain.com" -RoleDefinitionName "Reader" -Scope "/"
    ```
5. Rolü kaldırmak için aşağıdaki komutu kullanın: 

    ```azurepowershell
    Remove-AzRoleAssignment -SignInName "user@domain.com" -RoleDefinitionName "Reader" -Scope "/" 
    ```

## <a name="remove-elevated-access"></a>Yükseltilmiş erişimi kaldır 
Kullanıcılara Azure rolleri atandıktan sonra, kiracı yöneticisinin kendisini Kullanıcı erişimi yönetici rolünden kaldırması gerekir.

1. [Azure Portal](https://portal.azure.com) veya [Azure Active Directory Yönetim merkezinde](https://aad.portal.azure.com)oturum açın.

2. Gezinti listesinde **Azure Active Directory** ' yi seçin ve ardından **Özellikler**' i seçin.

3. **Azure kaynakları Için erişim yönetimi** altında, anahtarı **Hayır** olarak ayarlayın.

4. Ayarınızı kaydetmek için **Kaydet**' i seçin.



## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Azure Güvenlik Merkezi için kiracı genelinde görünürlük elde etme hakkında öğrendiniz. İlgili bilgiler için bkz.:

- [Azure Güvenlik Merkezi'nde İzinler](security-center-permissions.md)