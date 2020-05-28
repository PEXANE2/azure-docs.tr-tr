---
title: Windows sanal masaüstü konak havuzu hizmet güncelleştirmeleri-Azure
description: Üretime yönelik güncelleştirmeleri kullanıma almadan önce hizmet güncelleştirmelerini izlemek için bir doğrulama ana bilgisayarı havuzu oluşturma.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 05/27/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: fe6b047d10b8aef8faf81365cae940b05b3d798c
ms.sourcegitcommit: 6a9f01bbef4b442d474747773b2ae6ce7c428c1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84117733"
---
# <a name="tutorial-create-a-host-pool-to-validate-service-updates"></a>Öğretici: hizmet güncelleştirmelerini doğrulamak için bir konak havuzu oluşturma

>[!IMPORTANT]
>Bu içerik, Windows sanal masaüstü nesneleri Azure Resource Manager desteklemeyen sonbahar 2019 sürümü için geçerlidir. Spring 2020 güncelleştirmesinde tanıtılan Azure Resource Manager Windows sanal masaüstü nesnelerini yönetmeye çalışıyorsanız, [Bu makaleye](../create-validation-host-pool.md)bakın.

Konak havuzları, Windows sanal masaüstü kiracı ortamlarında bir veya daha fazla özdeş sanal makine koleksiyonudur. Ana bilgisayar havuzlarını üretim ortamınıza dağıtılmadan önce, bir doğrulama ana bilgisayar havuzu oluşturmanızı kesinlikle öneririz. Güncelleştirmeler önce doğrulama ana bilgisayar havuzlarına uygulanır, böylece üretim ortamınıza göndermeden önce hizmet güncelleştirmelerini izlemenizi sağlar. Doğrulama ana bilgisayar havuzu olmadan, hata oluşturan değişiklikleri bulamamanıza neden olabilir ve bu da üretim ortamınızdaki kullanıcılar için kapalı kalma süresine yol açabilir.

Uygulamalarınızın en son güncelleştirmelerle çalışmasını sağlamak için, doğrulama ana bilgisayar havuzu, üretim ortamınızda mümkün olduğunca ana bilgisayar havuzlarına benzer olmalıdır. Kullanıcıların, üretim ana bilgisayar havuzunda çalıştıkları şekilde doğrulama ana bilgisayar havuzuna sık olarak bağlanması gerekir. Konak havuzunuzdaki otomatik test testiniz varsa, doğrulama ana bilgisayar havuzuna otomatik test eklemeniz gerekir.

[Tanılama özelliğiyle](diagnostics-role-service-2019.md) veya [Windows sanal masaüstü sorunlarını giderme makaleleriyle](troubleshoot-set-up-overview-2019.md)doğrulama konak havuzundaki sorunları ayıklayabilirsiniz.

>[!NOTE]
> Sonraki tüm güncelleştirmeleri test etmek için doğrulama ana bilgisayar havuzunu yerinde bırakmanız önerilir.

Başlamadan önce, henüz yapmadıysanız [Windows sanal masaüstü PowerShell modülünü indirip içeri aktarın](/powershell/windows-virtual-desktop/overview/). Bundan sonra hesabınızda oturum açmak için aşağıdaki cmdlet 'i çalıştırın:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
```

## <a name="create-your-host-pool"></a>Konak havuzunuzu oluşturma

Aşağıdaki makalelerdeki yönergeleri izleyerek bir konak havuzu oluşturabilirsiniz:
- [Öğretici: Azure Marketi ile bir konak havuzu oluşturma](create-host-pools-azure-marketplace-2019.md)
- [Azure Resource Manager şablonuyla ana bilgisayar havuzu oluşturma](create-host-pools-arm-template.md)
- [PowerShell ile ana bilgisayar havuzu oluşturma](create-host-pools-powershell-2019.md)

## <a name="define-your-host-pool-as-a-validation-host-pool"></a>Konak havuzunuzu doğrulama ana bilgisayar havuzu olarak tanımlama

Yeni konak havuzunu doğrulama konak havuzu olarak tanımlamak için aşağıdaki PowerShell cmdlet 'lerini çalıştırın. Tekliflerdeki değerleri oturumunuzla ilgili değerlere göre değiştirin:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
Set-RdsHostPool -TenantName $myTenantName -Name "contosoHostPool" -ValidationEnv $true
```

Doğrulama özelliğinin ayarlandığını onaylamak için aşağıdaki PowerShell cmdlet 'ini çalıştırın. Tekliflerdeki değerleri oturumunuzla ilgili değerlerle değiştirin.

```powershell
Get-RdsHostPool -TenantName $myTenantName -Name "contosoHostPool"
```

Cmdlet 'inin sonuçları şu çıkışa benzer görünmelidir:

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

## <a name="update-schedule"></a>Zamanlamayı Güncelleştir

Hizmet güncelleştirmeleri ayda bir gerçekleşir. Önemli sorunlar varsa, kritik güncelleştirmeler daha sık bir hızda sağlanacaktır.

## <a name="next-steps"></a>Sonraki adımlar

Bir doğrulama ana bilgisayar havuzu oluşturduğunuza göre, Windows sanal masaüstü dağıtımınızı izlemek için Azure hizmet durumunu nasıl kullanacağınızı öğrenebilirsiniz. 

> [!div class="nextstepaction"]
> [Hizmet uyarılarını ayarlama](set-up-service-alerts-2019.md)
