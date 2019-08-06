---
title: Hizmet güncelleştirmelerini doğrulamak için bir Windows sanal masaüstü önizleme konak havuzu oluşturma-Azure
description: Üretime yönelik güncelleştirmeleri kullanıma almadan önce hizmet güncelleştirmelerini izlemek için bir doğrulama ana bilgisayarı havuzu oluşturma.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 05/08/2019
ms.author: helohr
ms.openlocfilehash: a23d9fe932556b0a685b373b060901d2c7fb8c85
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68816449"
---
# <a name="tutorial-create-a-host-pool-to-validate-service-updates"></a>Öğretici: Hizmet güncelleştirmelerini doğrulamak için konak havuzu oluşturma

Konak havuzları, Windows sanal masaüstü önizleme kiracı ortamlarında bir veya daha fazla özdeş sanal makine koleksiyonudur. Ana bilgisayar havuzlarını üretim ortamınıza dağıtılmadan önce, bir doğrulama ana bilgisayar havuzu oluşturmanızı kesinlikle öneririz. Güncelleştirmeler önce doğrulama ana bilgisayar havuzlarına uygulanır, böylece üretim ortamınıza göndermeden önce hizmet güncelleştirmelerini izlemenizi sağlar. Doğrulama ana bilgisayar havuzu olmadan, hata oluşturan değişiklikleri bulamamanıza neden olabilir ve bu da üretim ortamınızdaki kullanıcılar için kapalı kalma süresine yol açabilir.

Uygulamalarınızın en son güncelleştirmelerle çalışmasını sağlamak için, doğrulama ana bilgisayar havuzu, üretim ortamınızda mümkün olduğunca ana bilgisayar havuzlarına benzer olmalıdır. Kullanıcıların, üretim ana bilgisayar havuzunda çalıştıkları şekilde doğrulama ana bilgisayar havuzuna sık olarak bağlanması gerekir. Konak havuzunuzdaki otomatik test testiniz varsa, doğrulama ana bilgisayar havuzuna otomatik test eklemeniz gerekir.

[Tanılama özelliğiyle](diagnostics-role-service.md) veya [Windows sanal masaüstü sorunlarını giderme makaleleriyle](https://docs.microsoft.com/Azure/virtual-desktop/troubleshoot-set-up-overview)doğrulama konak havuzundaki sorunları ayıklayabilirsiniz.

>[!NOTE]
> Sonraki tüm güncelleştirmeleri test etmek için doğrulama ana bilgisayar havuzunu yerinde bırakmanız önerilir.

Başlamadan önce, henüz yapmadıysanız [Windows sanal masaüstü PowerShell modülünü indirip içeri aktarın](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview).

## <a name="create-your-host-pool"></a>Konak havuzunuzu oluşturma

Aşağıdaki makalelerdeki yönergeleri izleyerek bir konak havuzu oluşturabilirsiniz:
- [Öğretici: Azure Marketi ile bir konak havuzu oluşturma](create-host-pools-azure-marketplace.md)
- [Azure Resource Manager şablonuyla konak havuzu oluşturma](create-host-pools-arm-template.md)
- [PowerShell ile bir konak havuzu oluşturma](create-host-pools-powershell.md)

## <a name="define-your-host-pool-as-a-validation-host-pool"></a>Konak havuzunuzu doğrulama ana bilgisayar havuzu olarak tanımlama

Yeni konak havuzunu doğrulama konak havuzu olarak tanımlamak için aşağıdaki PowerShell cmdlet 'lerini çalıştırın. Tekliflerdeki değerleri oturumunuzla ilgili değerlere göre değiştirin:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
Set-RdsHostPool -TenantName $myTenantName -Name "contosoHostPool" -ValidationEnv $true
```

Doğrulama özelliğinin ayarlandığını onaylamak için aşağıdaki PowerShell cmdlet 'ini çalıştırın. Tekliflerdeki değerleri oturumunuzla ilgili değerlerle değiştirin.

```powershell
Get-RdsHostPool -TenantName $myTenantName -Name "contosoHostPool" -ValidationEnv $true
```

Cmdlet 'inin sonuçları şu çıkışa benzer görünmelidir:

```
    TenantName          : contoso 
    TenantGroupName     : Default Tenant Group
    HostPoolName        : contosoHostPool
    FriendlyName        :
    Description         :
    Persistent          : False 
    CustomRdpProperty   : use multimon:i:0;
    MaxSessionLimit     : 10
    LoadBalancerType    : BreadthFirst
    ValidationEnv       : True
    Ring                :
```

## <a name="update-schedule"></a>Zamanlamayı Güncelleştir

Önizlemede, hizmet güncelleştirmeleri yaklaşık bir aylık temposunda üzerinde gerçekleşmelidir. Önemli sorunlar varsa, kritik güncelleştirmeler daha sık görülen bir temposunda sunulacaktır.

## <a name="next-steps"></a>Sonraki adımlar

Bir doğrulama ana bilgisayar havuzu oluşturduğunuza göre, Microsoft sanal masaüstü kaynaklarını yönetmek için bir yönetim aracının nasıl dağıtılacağını ve bağlanacağını öğrenebilirsiniz.

> [!div class="nextstepaction"]
> [Yönetim Aracı öğreticisini dağıtma](./manage-resources-using-ui.md)
