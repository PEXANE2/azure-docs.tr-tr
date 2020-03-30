---
title: Azure Güvenlik Merkezi için kiracı çapında görünürlük kazanın | Microsoft Dokümanlar
description: Bu makalede, Azure Etkin Dizin kiracınıza bağlı tüm aboneliklere ilkeler uygulayarak güvenlik duruşunuzu ölçekte nasıl yönetacağınızı açıklanmaktadır.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: b85c0e93-9982-48ad-b23f-53b367f22b10
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/19/2018
ms.author: memildin
ms.openlocfilehash: 734876380d22f5d4d6dae0dd375b238fd5f6ffed
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74559341"
---
# <a name="gain-tenant-wide-visibility-for-azure-security-center"></a>Azure Güvenlik Merkezi için kiracı çapında görünürlük elde edin
Bu makalede, Azure Etkin Dizin kiracınızla bağlantılı tüm Azure aboneliklerine güvenlik ilkeleri uygulayarak kuruluşunuzun güvenlik duruşunu ölçekte nasıl yönetacağı açıklanmaktadır.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="management-groups"></a>Yönetim grupları
Azure yönetim grupları, abonelik grupları yla ilgili erişimi, ilkeleri ve raporlamayı verimli bir şekilde yönetme ve kök yönetim grubunda eylemler gerçekleştirerek tüm Azure mülkünü etkin bir şekilde yönetme olanağı sağlar. Her Azure REKLAM kiracıya kök yönetim grubu adı verilen tek bir üst düzey yönetim grubu verilir. Diğer tüm yönetim grupları ve abonelikler hiyerarşide en üstte yer alan bu kök yönetim grubunun altındadır. Bu grup, genel ilkelerin ve RBAC atamalarının dizin düzeyinde uygulanmasına izin verir. 

Aşağıdaki eylemlerden herhangi birini yaptığınızda kök yönetim grubu otomatik olarak oluşturulur: 
1. [Azure portalındaki](https://portal.azure.com) **Yönetim Grupları'na** giderek Azure yönetim gruplarını kullanmayı tercih edin.
2. API çağrısı yla bir yönetim grubu oluşturun.
3. PowerShell ile bir yönetim grubu oluşturun.

Yönetim gruplarına ayrıntılı bir genel bakış için, [kaynaklarınızı Azure yönetim grupları makalesiyle düzenleyin](../azure-resource-manager/management-groups-overview.md) makalesine bakın.

## <a name="create-a-management-group-in-the-azure-portal"></a>Azure portalında bir yönetim grubu oluşturma
Abonelikleri yönetim gruplarına göre düzenleyebilir ve yönetim ilkelerinizi yönetim gruplarına uygulayabilirsiniz. Bir yönetim grubu içindeki aboneliklerin tümü otomatik olarak yönetim grubuna uygulanmış olan ilkeleri devralır. Yönetim gruplarının Güvenlik Merkezi'nde olması gerekmezken, kök yönetim grubunun oluşturulması için en az bir yönetim grubu oluşturmanız önerilir. Grup oluşturulduktan sonra, Azure AD kiracınızın altındaki tüm abonelikler ona bağlanır. PowerShell yönergeleri ve daha fazla bilgi için [kaynak ve kuruluş yönetimi için yönetim grupları oluştur'a](../azure-resource-manager/management-groups-create.md)bakın.

 
1. [Azure portalında](https://portal.azure.com)oturum açın.
2. **Tüm hizmet** > **Yönetimi gruplarını**seçin.
3. Ana sayfada Yeni **Yönetim grubunu seçin.** 

    ![Ana Grup](./media/security-center-management-groups/main.png) 
4.  Yönetim grubu kimliği alanını doldurun. 
    - **Yönetim Grubu Kimliği,** bu yönetim grubunda komut göndermek için kullanılan dizin benzersiz tanımlayıcısındır. Bu tanımlayıcı, bu grubu tanımlamak için Azure sisteminde kullanıldığından, oluşturulduktan sonra düzenlenemez. 
    - Görüntü adı alanı, Azure portalında görüntülenen addır. Yönetim grubu oluşturulurken ayrı bir görüntü adı isteğe bağlı bir alandır ve herhangi bir zamanda değiştirilebilir.  

      ![Oluşturma](./media/security-center-management-groups/create_context_menu.png)  
5.  **Kaydet**’i seçin

### <a name="view-management-groups-in-the-azure-portal"></a>Azure portalındaki yönetim gruplarını görüntüleme
1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Yönetim gruplarını görüntülemek için Azure ana menüsü altındaki **Tüm hizmetleri** seçin.
3. **Genel**altında, **Yönetim Grupları**seçin.

    ![Yönetim grubu oluşturma](./media/security-center-management-groups/all-services.png)

## <a name="grant-tenant-level-visibility-and-the-ability-to-assign-policies"></a>Kiracı düzeyinde görünürlük ve ilkeler atama olanağı tanıyın

Azure AD kiracısına kayıtlı tüm aboneliklerin güvenlik duruşunu görebilmek için, kök yönetim grubuna yeterli okuma izinlerine sahip bir RBAC rolünün atanması gerekir.

### <a name="elevate-access-for-a-global-administrator-in-azure-active-directory"></a>Azure Etkin Dizini'nde küresel bir yönetici için erişimi yükseltme
Azure Etkin Dizin kiracı yöneticisinin Azure aboneliklerine doğrudan erişimi yoktur. Ancak, bir dizin yöneticisi olarak, erişim esahip bir role kendilerini yükseltmek hakkına sahiptir. Azure AD kiracı yöneticisinin RBAC rollerini atayabilmesi için kendisini kök yönetim grubu düzeyindeki kullanıcı erişim yöneticisine yükseltmesi gerekir. PowerShell yönergeleri ve ek bilgiler için Azure [Etkin Dizini'nde Global yönetici erişimi yükselt'e](../role-based-access-control/elevate-access-global-admin.md)bakın. 


1. [Azure portalında](https://portal.azure.com) veya [Azure Etkin Dizin yönetici merkezinde](https://aad.portal.azure.com)oturum açın.

2. Gezinti listesinde Azure **Etkin Dizin'i** tıklatın ve ardından **Özellikler'i**tıklatın.

   ![Azure AD Özellikleri - ekran görüntüsü](./media/security-center-management-groups/aad-properties.png)

3. **Azure kaynakları için Access yönetimi**altında, anahtarı **Evet**olarak ayarlayın.

   ![Global yönetici Azure Aboneliklerini ve Yönetim Gruplarını yönetebilir - ekran görüntüsü](./media/security-center-management-groups/aad-properties-global-admin-setting.png)

   - Anahtarı Evet olarak ayarladığınızda, Azure RBAC'da kullanıcı erişim yöneticisi rolünü kök kapsamda (/) atanırsınız. Bu, bu Azure AD diziniyle ilişkili tüm Azure aboneliklerinde ve yönetim gruplarında rol atama izni verir. Bu anahtar yalnızca Azure AD'de Genel Yönetici rolü atanan kullanıcılar tarafından kullanılabilir.

   - Anahtarı Hayır olarak ayarladığınızda, Azure RBAC'daki Kullanıcı Erişim Yöneticisi rolü kullanıcı hesabınızdan kaldırılır. Bu Azure AD diziniyle ilişkili tüm Azure aboneliklerinde ve yönetim gruplarında artık rol atayamaz. Yalnızca size erişim izni verilen Azure aboneliklerini ve yönetim gruplarını görüntüleyebilir ve yönetebilirsiniz.

4. Ayarınızı kaydetmek için **Kaydet'i** tıklatın.

    - Bu ayar genel bir özellik değildir ve yalnızca şu anda oturum açmış olan kullanıcı için geçerlidir.

5. Yükseltilmiş erişimde yapmanız gereken görevleri gerçekleştirin. Bitirdiğinde, anahtarı **No**olarak geri ayarlayın.


### <a name="assign-rbac-roles-to-users"></a>RBAC rollerini kullanıcılara atama
Tüm abonelikleri görünürlük kazanmak için, kiracı yöneticilerin inandıkları kullanıcıya, kendileri de dahil olmak üzere, kök yönetim grubu düzeyinde uygun RBAC rolünü atamaları gerekir. Atamak için önerilen roller **Güvenlik Yöneticisi** veya Güvenlik **Okuyucu'dur.** Genel olarak, Güvenlik Yöneticisi rolü temel düzeyde ilkeleri uygulamak için gereklidir, Güvenlik Okuyucu kiracı düzeyinde görünürlük sağlamak için yeterli olacaktır. Bu roller tarafından verilen izinler hakkında daha fazla bilgi için, [Güvenlik Yöneticisi yerleşik rol açıklamasına](../role-based-access-control/built-in-roles.md#security-admin) veya Güvenlik [Okuyucu'nun yerleşik rol açıklamasına](../role-based-access-control/built-in-roles.md#security-reader)bakın.


#### <a name="assign-rbac-roles-to-users-through-the-azure-portal"></a>Azure portalı aracılığıyla kullanıcılara RBAC rolleri atama: 

1. [Azure portalında](https://portal.azure.com)oturum açın. 
1. Yönetim gruplarını görüntülemek için Azure ana menüsü altındaki **Tüm hizmetleri** seçin ve ardından **Yönetim Grupları'nı**seçin.
1.  Bir yönetim grubu seçin ve **ayrıntıları**tıklatın.

    ![Yönetim Grupları ayrıntıları ekran görüntüsü](./media/security-center-management-groups/management-group-details.PNG)
 
1. **Erişim denetimi (IAM)** sonra **Rol atamaları**tıklatın.

1. **Rol Atama ekle'yi**tıklatın.

1. Atamak için rolü ve kullanıcıyı seçin ve ardından **Kaydet'i**tıklatın.  
   
   ![Güvenlik Okuyucu rol ekran görüntüsü ekle](./media/security-center-management-groups/asc-security-reader.png)


#### <a name="assign-rbac-roles-to-users-with-powershell"></a>PowerShell'e sahip kullanıcılara RBAC rolleri atama: 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

1. [Azure PowerShell'i](/powershell/azure/install-az-ps)yükleyin.
2. Aşağıdaki komutları çalıştırın: 

    ```azurepowershell
    # Login to Azure as a Global Administrator user
    Connect-AzAccount
    ```

3. İstendiğinde, genel yönetici kimlik bilgileriyle oturum açın. 

    ![Oturum açma istemi ekran görüntüsü](./media/security-center-management-groups/azurerm-sign-in.PNG)

4. Aşağıdaki komutu çalıştırarak okuyucu rol izinleri ver:

    ```azurepowershell
    # Add Reader role to the required user on the Root Management Group
    # Replace "user@domian.com” with the user to grant access to
    New-AzRoleAssignment -SignInName "user@domain.com" -RoleDefinitionName "Reader" -Scope "/"
    ```
5. Rolü kaldırmak için aşağıdaki komutu kullanın: 

    ```azurepowershell
    Remove-AzRoleAssignment -SignInName "user@domain.com" -RoleDefinitionName "Reader" -Scope "/" 
    ```

### <a name="open-or-refresh-security-center"></a>Güvenlik Merkezi'ni açma veya yenileme
Azure AD kiracınızın altındaki tüm aboneliklerde görünürlüğe sahip olduğunuzu doğrulamak için yüksek erişime sahip olduktan sonra Azure Güvenlik Merkezi'ni açın veya yenileyin. 

1. [Azure portalında](https://portal.azure.com)oturum açın. 
2. Güvenlik Merkezi'nde görüntülemek istediğiniz abonelik seçicideki tüm abonelikleri seçtiğinizden emin olun.

    ![Abonelik seçici ekran görüntüsü](./media/security-center-management-groups/subscription-selector.png)

1. Azure ana menüsü altındaki **Tüm hizmetleri** seçin ve ardından **Güvenlik Merkezi'ni**seçin.
2. Genel **Bakış'ta**bir abonelik kapsama grafiği vardır.

    ![Abonelik kapsama grafiği ekran görüntüsü](./media/security-center-management-groups/security-center-subscription-coverage.png)

3. Kapsanan aboneliklistesini görmek için **Kapsam'a** tıklayın. 

    ![Abonelik kapsama listesi ekran görüntüsü](./media/security-center-management-groups/security-center-coverage.png)

### <a name="remove-elevated-access"></a>Yükseltilmiş erişimi kaldırma 
RBAC rolleri kullanıcılara atandıktan sonra, kiracı yöneticinin kendisini kullanıcı erişim yöneticisi rolünden kaldırması gerekir.

1. [Azure portalında](https://portal.azure.com) veya [Azure Etkin Dizin yönetici merkezinde](https://aad.portal.azure.com)oturum açın.

2. Gezinti listesinde Azure **Etkin Dizin'i** tıklatın ve ardından **Özellikler'i**tıklatın.

3. Global yönetici altında **Azure Abonelikleri ve Yönetim Grupları yönetebilirsiniz,** **No**için geçiş ayarlayın.

4. Ayarınızı kaydetmek için **Kaydet'i** tıklatın.



## <a name="adding-subscriptions-to-a-management-group"></a>Yönetim grubuna abonelik ekleme
Oluşturduğunuz yönetim grubuna abonelik ekleyebilirsiniz. Bu adımlar, kiracı çapında görünürlük ve küresel politika ve erişim yönetimi kazanmak için zorunlu değildir.

1. **Yönetim Grupları**altında, aboneliğinizi eklemek için bir yönetim grubu seçin.

    ![Abonelik eklemek için bir yönetim grubu seçin](./media/security-center-management-groups/management-group-subscriptions.png)

2. **Varolan Ekle'yi**seçin.

    ![Varolan ekle](./media/security-center-management-groups/add-existing.png)

3. **Varolan kaynağı ekle'nin** altına abonelik girin ve **Kaydet'i**tıklatın.

4. Kapsamdaki tüm abonelikleri ekleyene kadar 1'den 3'e kadar olan adımları yineleyin.

   > [!NOTE]
   > Yönetim grupları hem abonelikleri hem de alt yönetim gruplarını içerebilir. Bir kullanıcıya üst yönetim grubuna bir RBAC rolü atadığınızda, erişim alt yönetim grubunun abonelikleri tarafından devralır. Üst yönetim grubunda ayarlanan ilkeler de çocuklar tarafından devralınır. 

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Azure Güvenlik Merkezi için kiracı çapında görünürlük kazanmayı öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Azure Güvenlik Merkezi'nde güvenlik durumu izleme](security-center-monitoring.md)

> [!div class="nextstepaction"]
> [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md)

