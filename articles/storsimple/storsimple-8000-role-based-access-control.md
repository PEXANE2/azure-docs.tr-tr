---
title: StorSimple için Rol Tabanlı Erişim Denetimini Kullanma | Microsoft Dokümanlar
description: StorSimple bağlamında Azure Role tabanlı Erişim Denetimi'nin (RBAC) nasıl kullanılacağını açıklar.
services: storsimple
documentationcenter: ''
author: alkohli
manager: jconnoc
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/11/2017
ms.author: alkohli
ms.openlocfilehash: a79753a897a62e194a759c23a9c0acc45c5f36c1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "66159084"
---
# <a name="role-based-access-control-for-storsimple"></a>StorSimple için Rol Tabanlı Erişim Kontrolü

Bu makalede, StorSimple aygıtınız için Azure Role Tabanlı Erişim Denetimi'nin (RBAC) nasıl kullanılabileceğinin kısa bir açıklaması yer alır. RBAC, Azure için ince taneli erişim yönetimi sunar. Herkese sınırsız erişim sağlamak yerine, StorSimple kullanıcılarına işlerini yapmaları için doğru miktarda erişim sağlamak için RBAC'ı kullanın. Azure'da erişim yönetiminin temelleri hakkında daha fazla bilgi için azure [portalında Rol Tabanlı Erişim Denetimi'ne başlayın'a](../role-based-access-control/overview.md)bakın.

Bu makale, Azure portalında Güncelleştirme 3.0 veya daha sonra çalıştıran StorSimple 8000 serisi aygıtlar için geçerlidir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="rbac-roles-for-storsimple"></a>StorSimple için RBAC rolleri

RBAC rollere göre atanabilir. Roller, ortamdaki kullanılabilir kaynaklara dayalı belirli izin düzeylerini sağlar. StorSimple kullanıcılarının aralarından seçim yapabileceği iki tür rol vardır: yerleşik veya özel.

* **Yerleşik roller** - Yerleşik roller sahibi, katkıda bulunan, okuyucu veya kullanıcı erişim yöneticisi olabilir. Daha fazla bilgi için Azure [Rol Tabanlı Erişim Denetimi için Yerleşik rollere](../role-based-access-control/built-in-roles.md)bakın.

* **Özel roller** - Yerleşik roller gereksinimlerinize uymuyorsa, StorSimple için özel RBAC rolleri oluşturabilirsiniz. Özel bir RBAC rolü oluşturmak için yerleşik bir rolile başlayın, onu edin ve sonra ortamda geri aktarın. Rolün indirilip yüklenmesi Azure PowerShell veya Azure CLI kullanılarak yönetilir. Daha fazla bilgi için [bkz.](../role-based-access-control/custom-roles.md)

Azure portalında bir StorSimple aygıt kullanıcısı için kullanılabilen farklı rolleri görüntülemek için StorSimple Device Manager hizmetinize gidin ve ardından **Access denetimine (IAM) > Roller'e**gidin.


## <a name="create-a-custom-role-for-storsimple-infrastructure-administrator"></a>StorSimple Infrastructure Administrator için özel bir rol oluşturma

Aşağıdaki örnekte, kullanıcıların tüm kaynak kapsamlarını görüntülemesine izin veren ancak bunları yeniden oluşturmamalarını veya yenilerini oluşturmamalarını sağlayan yerleşik rol **Reader** ile başlarız. Daha sonra yeni bir özel rol StorSimple Infrastructure admin oluşturmak için bu rolü genişletmek. Bu rol, StorSimple aygıtları için altyapıyı yönetebilen kullanıcılara atanır.

1. Windows PowerShell'i yönetici olarak çalıştırın.

2. Azure'da oturum açın.

    `Connect-AzAccount`

3. Bilgisayarınızda JSON şablonu olarak Reader rolünü dışa aktarın.

    ```powershell
    Get-AzRoleDefinition -Name "Reader"

    Get-AzRoleDefinition -Name "Reader" | ConvertTo-Json | Out-File C:\ssrbaccustom.json
    ```

4. Visual Studio'da JSON dosyasını açın. Tipik bir RBAC rolünün üç ana bölümden oluştuğunu görürsünüz, **Eylemler,** **NotActions**ve **AtamaKTaOlan Kapsamlar.**

    **Eylem** bölümünde, bu rol için izin verilen tüm işlemler listelenir. Her eylem bir kaynak sağlayıcıdan atanır. StorSimple altyapı yöneticisi için `Microsoft.StorSimple` kaynak sağlayıcısını kullanın.

    Aboneliğinizde mevcut ve kayıtlı tüm kaynak sağlayıcılarını görmek için PowerShell'i kullanın.

    `Get-AzResourceProvider`

    Ayrıca, kaynak sağlayıcılarını yönetmek için mevcut tüm PowerShell cmdlets'leri denetleyebilirsiniz.

    **NotActions** bölümlerinde, belirli bir RBAC rolü için kısıtlanmış tüm eylemler listelenir. Bu örnekte, hiçbir eylem kısıtlanmaz.
    
    **AtamaKapsamı**altında, abonelik teşekkülleri listelenir. RBAC rolünün kullanıldığı yerde açık abonelik kimliği içerdiğinden emin olun. Doğru abonelik kimliği belirtilmemişse, aboneliğinizdeki rolü içe aktarmanıza izin verilmez.

    Dosyayı önceki hususları göz önünde bulundurarak edin.

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

6. Özel RBAC rolünü ortama geri aktarın.

    `New-AzRoleDefinition -InputFile "C:\ssrbaccustom.json"`


Bu rol artık **Access denetim** bıçağındaki roller listesinde görünmelidir.

![RBAC rollerini görüntüleyin](./media/storsimple-8000-role-based-access-control/rbac-role-types.png)

Daha fazla bilgi için [Özel rollere](../role-based-access-control/custom-roles.md)gidin.

### <a name="sample-output-for-custom-role-creation-via-the-powershell"></a>PowerShell üzerinden özel rol oluşturma için örnek çıktı

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

## <a name="add-users-to-the-custom-role"></a>Kullanıcıları özel role ekleme

Rol atamasının kapsamı olan kaynak, kaynak grubu veya abonelik içinden erişim verebilirsiniz. Erişim sağlarken, üst düğümde verilen erişimin çocuk tarafından devredildiğini unutmayın. Daha fazla bilgi için [rol tabanlı erişim denetimine](../role-based-access-control/overview.md)gidin.

1. Access **denetimine (IAM)** gidin. Erişim kontrol bıçağına **+ Ekle'yi** tıklatın.

    ![RBAC rolüne erişim ekleme](./media/storsimple-8000-role-based-access-control/rbac-add-role.png)

2. Atamak istediğiniz rolü seçin, bu durumda **StorSimple Infrastructure Admin**' dir.

3. Dizininizde, erişim vermek istediğiniz kullanıcı, grup veya uygulamayı seçin. Görünen adlar, e-posta adresleri ve nesne tanımlayıcıları ile dizinde arama yapabilirsiniz.

4. Atamayı oluşturmak için **Kaydet'i** seçin.

    ![RBAC rolüne izin ekleme](./media/storsimple-8000-role-based-access-control/rbac-create-role-infra-admin.png)

**Bir Ekleme kullanıcı** bildirimi ilerlemeyi izler. Kullanıcı başarıyla eklendikten sonra Access denetimindeki kullanıcıların listesi güncelleştirilir.

## <a name="view-permissions-for-the-custom-role"></a>Özel rol için izinleri görüntüleme

Bu rol oluşturulduktan sonra, Azure portalında bu rolile ilişkili izinleri görüntüleyebilirsiniz.

1. Bu rolile ilişkili izinleri görüntülemek için **Access denetimine (IAM)**> Rolleri > StorSimple Infrastructure Admin'e gidin. Bu roldeki kullanıcıların listesi görüntülenir.

2. StorSimple Infrastructure Admin kullanıcısını seçin ve **İzinler'i**tıklatın.

    ![StorSimple Infra Admin rolü için izinleri görüntüleyin](./media/storsimple-8000-role-based-access-control/rbac-roles-view-permissions.png)

3. Bu rolile ilişkili izinler görüntülenir.

    ![StorSimple Infra Admin rolündeki kullanıcıları görüntüleyin](./media/storsimple-8000-role-based-access-control/rbac-infra-admin-permissions1.png)


## <a name="next-steps"></a>Sonraki adımlar

[Dahili ve harici kullanıcılar için özel roller atamayı](../role-based-access-control/role-assignments-external-users.md)öğrenin.
