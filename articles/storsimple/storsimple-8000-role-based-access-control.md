---
title: StorSimple için rol tabanlı Access Control kullanma | Microsoft Docs
description: StorSimple bağlamında Azure rol tabanlı erişim denetimi 'nin (Azure RBAC) nasıl kullanılacağını açıklar.
services: storsimple
documentationcenter: ''
author: alkohli
manager: jconnoc
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/11/2017
ms.author: alkohli
ms.openlocfilehash: 38500edeca2241bfa9ab093e037af18159994b02
ms.sourcegitcommit: 4f1c7df04a03856a756856a75e033d90757bb635
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87920424"
---
# <a name="role-based-access-control-for-storsimple"></a>StorSimple için rol tabanlı Access Control

Bu makalede, Azure rol tabanlı erişim denetimi 'nin (Azure RBAC) StorSimple cihazınız için nasıl kullanılabileceği hakkında kısa bir açıklama sunulmaktadır. RBAC, Azure için ayrıntılı erişim yönetimi sağlar. Herkese sınırsız erişim vermek yerine, işlerini yapmak üzere yalnızca StorSimple kullanıcılarına doğru erişim miktarını vermek için RBAC 'yi kullanın. Azure 'da erişim yönetiminin temelleri hakkında daha fazla bilgi için bkz. [Azure Portal rol tabanlı Access Control kullanmaya başlama](../role-based-access-control/overview.md).

Bu makale, Azure portal güncelleştirme 3,0 veya sonraki bir sürümünü çalıştıran StorSimple 8000 serisi cihazlar için geçerlidir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="azure-roles-for-storsimple"></a>StorSimple için Azure rolleri

RBAC, rollere göre atanabilir. Roller, ortamdaki kullanılabilir kaynaklara bağlı olarak belirli izin düzeylerinin olmasını güvence altına alır. StorSimple kullanıcılarının aralarından seçim yapabileceğiniz iki tür rol vardır: yerleşik veya özel.

* **Yerleşik roller** -yerleşik roller sahip, katkıda bulunan, okuyucu veya Kullanıcı erişimi yöneticisi olabilir. Daha fazla bilgi için bkz. [Azure rol tabanlı Access Control Için yerleşik roller](../role-based-access-control/built-in-roles.md).

* **Özel roller** -yerleşik roller gereksinimlerinize uygun değilse, StorSimple için Azure özel rolleri oluşturabilirsiniz. Bir Azure özel rolü oluşturmak için, yerleşik bir rolle başlayın, düzenleyin ve sonra ortama geri alın. Rolü indirme ve karşıya yükleme Azure PowerShell ya da Azure CLı kullanılarak yönetilir. Daha fazla bilgi için bkz. [rol tabanlı Access Control için özel roller oluşturma](../role-based-access-control/custom-roles.md).

Azure portal StorSimple cihaz kullanıcısı için kullanılabilen farklı rolleri görüntülemek için, StorSimple Aygıt Yöneticisi hizmetinize gidin ve ardından **erişim denetimi (IAM) > rollere**gidin.


## <a name="create-a-custom-role-for-storsimple-infrastructure-administrator"></a>StorSimple altyapı yöneticisi için özel bir rol oluşturma

Aşağıdaki örnekte, kullanıcıların tüm kaynak kapsamlarını görüntülemesine izin veren, ancak yenilerini düzenleyemediğinden veya yenilerini oluşturmadığından, yerleşik rol **okuyucularıyla** başlayacağız. Daha sonra bu rolü, yeni bir özel rol StorSimple altyapı yöneticisi oluşturacak şekilde genişlettik. Bu rol, StorSimple cihazları için altyapıyı yönetebilen kullanıcılara atanır.

1. Windows PowerShell 'i yönetici olarak çalıştırın.

2. Azure'da oturum açın.

    `Connect-AzAccount`

3. Okuyucu rolünü bilgisayarınızda JSON şablonu olarak dışarı aktarın.

    ```powershell
    Get-AzRoleDefinition -Name "Reader"

    Get-AzRoleDefinition -Name "Reader" | ConvertTo-Json | Out-File C:\ssrbaccustom.json
    ```

4. JSON dosyasını Visual Studio 'da açın. Tipik bir Azure rolünün üç ana bölümden, **eylemlerden**, **NotActions**ve **astifblescopes**oluştuğunu görürsünüz.

    **Eylem** bölümünde, bu rol için izin verilen tüm işlemler listelenir. Her eylem bir kaynak sağlayıcısından atanır. StorSimple altyapı yöneticisi için `Microsoft.StorSimple` kaynak sağlayıcısını kullanın.

    Kullanılabilir ve aboneliğinizde kayıtlı tüm kaynak sağlayıcılarını görmek için PowerShell 'i kullanın.

    `Get-AzResourceProvider`

    Kaynak sağlayıcılarını yönetmek için kullanılabilir tüm PowerShell cmdlet 'lerini de denetleyebilirsiniz.

    **NotActions** bölümlerinde, belirli bir Azure rolü için tüm kısıtlı eylemler listelenir. Bu örnekte, hiçbir eylem kısıtlanamaz.
    
    **Astifblescopes**, abonelik kimlikleri listelenir. Azure rolünün, kullanıldığı açık abonelik KIMLIĞINI içerdiğinden emin olun. Doğru abonelik KIMLIĞI belirtilmemişse, aboneliğinizdeki rolü içeri aktarmaya izin verilmez.

    Önceki hususları göz önünde bulundurarak dosyayı düzenleyin.

    ```json
    {
        "Name":  "StorSimple Infrastructure Admin",
        "Id":  "<guid>",
        "IsCustom":  true,
        "Description":  "Lets you view everything, but not make any changes except for Clear alerts, Clear settings, install, download etc.",
        "Actions":  [
                        "Microsoft.StorSimple/managers/alerts/read",
                        "Microsoft.StorSimple/managers/devices/volumeContainers/read",
                        "Microsoft.StorSimple/managers/devices/jobs/read",
                        "Microsoft.StorSimple/managers/devices/alertSettings/read",
                        "Microsoft.StorSimple/managers/devices/alertSettings/write",
                        "Microsoft.StorSimple/managers/clearAlerts/action",
                        "Microsoft.StorSimple/managers/devices/networkSettings/read",
                        "Microsoft.StorSimple/managers/devices/publishSupportPackage/action",
                        "Microsoft.StorSimple/managers/devices/scanForUpdates/action",
                        "Microsoft.StorSimple/managers/devices/metrics/read"

                    ],
        "NotActions":  [

                    ],
        "AssignableScopes":  [
                                "/subscriptions/<subscription_ID>/"
                            ]
    }
    ```

6. Azure özel rolünü ortama geri aktarın.

    `New-AzRoleDefinition -InputFile "C:\ssrbaccustom.json"`


Bu rol artık **erişim denetimi** dikey penceresindeki Roller listesinde görünmelidir.

![Azure rollerini görüntüleme](./media/storsimple-8000-role-based-access-control/rbac-role-types.png)

Daha fazla bilgi için [özel roller](../role-based-access-control/custom-roles.md)' e gidin.

### <a name="sample-output-for-custom-role-creation-via-the-powershell"></a>PowerShell aracılığıyla özel rol oluşturma için örnek çıkış

```powershell
Connect-AzAccount
```

```Output
Environment           : AzureCloud
Account               : john.doe@contoso.com
TenantId              : <tenant_ID>
SubscriptionId        : <subscription_ID>
SubscriptionName      : Internal Consumption
CurrentStorageAccount :
```

```powershell
Get-AzRoleDefinition -Name "Reader"
```

```Output
Name             : Reader
Id               : <guid>
IsCustom         : False
Description      : Lets you view everything, but not make any changes.
Actions          : {*/read}
NotActions       : {}
AssignableScopes : {/}
```

```powershell
Get-AzRoleDefinition -Name "Reader" | ConvertTo-Json | Out-File C:\ssrbaccustom.json
New-AzRoleDefinition -InputFile "C:\ssrbaccustom.json"
```

```Output
Name             : StorSimple Infrastructure Admin
Id               : <tenant_ID>
IsCustom         : True
Description      : Lets you view everything, but not make any changes except for Clear alerts, Clear settings, install,
                   download etc.
Actions          : {Microsoft.StorSimple/managers/alerts/read,
                   Microsoft.StorSimple/managers/devices/volumeContainers/read,
                   Microsoft.StorSimple/managers/devices/jobs/read,
                   Microsoft.StorSimple/managers/devices/alertSettings/read...}
NotActions       : {}
AssignableScopes : {/subscriptions/<subscription_ID>/}
```

## <a name="add-users-to-the-custom-role"></a>Özel Role Kullanıcı ekleme

Rol atamasının kapsamı olan kaynak, kaynak grubu veya abonelik içinden erişim verebilirsiniz. Erişim sağlarken, üst düğümde verilen erişimin alt öğe tarafından devralındığını göz önünde bulundurun. Daha fazla bilgi için [Azure rol tabanlı erişim denetimi 'ne (Azure RBAC)](../role-based-access-control/overview.md)gidin.

1. **Access Control (IAM)** sayfasına gidin. Erişim denetimi dikey penceresinde **+ Ekle** ' ye tıklayın.

    ![Azure rolüne erişim ekleme](./media/storsimple-8000-role-based-access-control/rbac-add-role.png)

2. Atamak istediğiniz rolü seçin, bu durumda **StorSimple altyapı yöneticisi**budur.

3. Dizininizde, erişim vermek istediğiniz kullanıcı, grup veya uygulamayı seçin. Görünen adlar, e-posta adresleri ve nesne tanımlayıcıları ile dizinde arama yapabilirsiniz.

4. Atamayı oluşturmak için **Kaydet** ' i seçin.

    ![Azure rolüne izinler ekleme](./media/storsimple-8000-role-based-access-control/rbac-create-role-infra-admin.png)

Bir **Kullanıcı bildirimi ekleme** ilerleme durumunu izler. Kullanıcı başarıyla eklendikten sonra, erişim denetimindeki kullanıcıların listesi güncellenir.

## <a name="view-permissions-for-the-custom-role"></a>Özel rol için izinleri görüntüle

Bu rol oluşturulduktan sonra, Azure portal bu rolle ilişkili izinleri görüntüleyebilirsiniz.

1. Bu rolle ilişkili izinleri görüntülemek için, **StorSimple altyapı yöneticisi > rol > erişim denetimi (IAM)** seçeneğine gidin. Bu roldeki kullanıcıların listesi görüntülenir.

2. Bir StorSimple altyapı yöneticisi kullanıcısı seçip **izinler**' e tıklayın.

    ![StorSimple Infra yönetici rolü için izinleri görüntüleme](./media/storsimple-8000-role-based-access-control/rbac-roles-view-permissions.png)

3. Bu rolle ilişkili izinler görüntülenir.

    ![StorSimple Infra yönetici rolündeki kullanıcıları görüntüleme](./media/storsimple-8000-role-based-access-control/rbac-infra-admin-permissions1.png)


## <a name="next-steps"></a>Sonraki adımlar

[İç ve dış kullanıcılar için özel roller atamayı](../role-based-access-control/role-assignments-external-users.md)öğrenin.
