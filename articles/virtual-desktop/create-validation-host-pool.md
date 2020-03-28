---
title: Windows Sanal Masaüstü ana bilgisayar havuzu hizmeti güncelleştirmeleri - Azure
description: Güncelleştirmeleri üretime dağıtmadan önce hizmet güncelleştirmelerini izlemek için bir doğrulama ana bilgisayar havuzu oluşturma.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 03/13/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: f2b51213dfc6d7e55f76e78b92d12111f84736be
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79365398"
---
# <a name="tutorial-create-a-host-pool-to-validate-service-updates"></a>Öğretici: Hizmet güncelleştirmelerini doğrulamak için ana bilgisayar havuzu oluşturma

Ana bilgisayar havuzları, Windows Sanal Masaüstü kiracı ortamlarında bir veya daha fazla aynı sanal makineden oluşan bir koleksiyondur. Ana bilgisayar havuzlarını üretim ortamınıza dağıtmadan önce, bir doğrulama ana bilgisayar havuzu oluşturmanızı öneririz. Güncelleştirmeler önce doğrulama ana bilgisayar havuzlarına uygulanır ve bunları üretim ortamınıza kaydetmeden önce servis güncelleştirmelerini izlemenize izin verir. Doğrulama ana bilgisayar havuzu olmadan, üretim ortamınızdaki kullanıcılar için kapalı kalma süresine neden olabilecek hatalara neden olan değişiklikleri keşfedemeyebilirsiniz.

Uygulamalarınızın en son güncelleştirmelerle çalışmasını sağlamak için doğrulama ana bilgisayar havuzu, üretim ortamınızdaki ana bilgisayar havuzlarına olabildiğince benzer olmalıdır. Kullanıcılar, üretim ana bilgisayar havuzuna olduğu gibi doğrulama ana bilgisayar havuzuna da sık sık bağlanmalıdır. Ana bilgisayar havuzunuzda otomatik test varsa, doğrulama ana bilgisayar havuzuna otomatik test eklemeniz gerekir.

Tanılama [özelliği](diagnostics-role-service.md) veya [Windows Sanal Masaüstü sorun giderme makaleleriyle](troubleshoot-set-up-overview.md)doğrulama ana bilgisayar havuzundaki sorunları hata ayıklayabilirsiniz.

>[!NOTE]
> Gelecekteki tüm güncelleştirmeleri test etmek için doğrulama ana bilgisayar havuzunu yerinde bırakmanızı öneririz.

Başlamadan önce, windows [sanal masaüstü powershell modülünü indirin ve içe aktarın](/powershell/windows-virtual-desktop/overview/), henüz yapmadıysanız. Bundan sonra, hesabınızda oturum açabilmek için aşağıdaki cmdlet'i çalıştırın:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
```

## <a name="create-your-host-pool"></a>Ana bilgisayar havuzunuzu oluşturun

Bu makalelerden herhangi birinde yönergeleri izleyerek bir ana bilgisayar havuzu oluşturabilirsiniz:
- [Öğretici: Azure Marketi ile ana bilgisayar havuzu oluşturma](create-host-pools-azure-marketplace.md)
- [Azure Resource Manager şablonuyla ana bilgisayar havuzu oluşturma](create-host-pools-arm-template.md)
- [PowerShell ile ana bilgisayar havuzu oluşturma](create-host-pools-powershell.md)

## <a name="define-your-host-pool-as-a-validation-host-pool"></a>Ana bilgisayar havuzunuzu doğrulama ana bilgisayar havuzu olarak tanımlayın

Yeni ana bilgisayar havuzunu doğrulama ana bilgisayarı olarak tanımlamak için aşağıdaki PowerShell cmdlets'i çalıştırın. Tırnak halindeki değerleri oturumunuzla ilgili değerlere göre değiştirin:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
Set-RdsHostPool -TenantName $myTenantName -Name "contosoHostPool" -ValidationEnv $true
```

Doğrulama özelliğinin ayarlandığını doğrulamak için aşağıdaki PowerShell cmdlet'i çalıştırın. Tırnak halindeki değerleri oturumunuzla ilgili değerlere göre değiştirin.

```powershell
Get-RdsHostPool -TenantName $myTenantName -Name "contosoHostPool"
```

Cmdlet sonuçları bu çıktıya benzer olmalıdır:

```
    TenantName          : contoso 
    TenantGroupName     : Default Tenant Group
    HostPoolName        : contosoHostPool
    FriendlyName        :
    Description         :
    Persistent          : False 
    CustomRdpProperty    : use multimon:i:0;
    MaxSessionLimit     : 10
    LoadBalancerType    : BreadthFirst
    ValidationEnv       : True
    Ring                :
```

## <a name="update-schedule"></a>Zamanlamayı güncelleştir

Hizmet güncelleştirmeleri aylık olarak gerçekleşir. Önemli sorunlar varsa, kritik güncelleştirmeler daha sık bir hızda sağlanacaktır.

## <a name="next-steps"></a>Sonraki adımlar

Artık bir doğrulama ana bilgisayar havuzu oluşturduğunuza göre, Windows Sanal Masaüstü dağıtımınızı izlemek için Azure Hizmet Durumu'nun nasıl kullanılacağını öğrenebilirsiniz. 

> [!div class="nextstepaction"]
> [Hizmet uyarılarını ayarlama](./set-up-service-alerts.md)
