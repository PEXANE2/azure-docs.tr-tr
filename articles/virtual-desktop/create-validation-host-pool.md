---
title: Windows sanal masaüstü konak havuzu hizmet güncelleştirmeleri-Azure
description: Üretime yönelik güncelleştirmeleri kullanıma almadan önce hizmet güncelleştirmelerini izlemek için bir doğrulama ana bilgisayarı havuzu oluşturma.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 03/13/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: b74b7f0b79ad4064d7133a19316d6aec6bd5ba3a
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2020
ms.locfileid: "82611577"
---
# <a name="tutorial-create-a-host-pool-to-validate-service-updates"></a>Öğretici: hizmet güncelleştirmelerini doğrulamak için bir konak havuzu oluşturma

>[!IMPORTANT]
>Bu içerik, Azure Resource Manager Windows sanal masaüstü nesneleriyle Spring 2020 güncelleştirmesine yöneliktir. Windows sanal masaüstü Fall 2019 sürümünü Azure Resource Manager nesneleri olmadan kullanıyorsanız, [Bu makaleye](./virtual-desktop-fall-2019/create-validation-host-pool-2019.md)bakın.
>
> Windows sanal masaüstü Spring 2020 güncelleştirmesi şu anda genel önizlemededir. Bu önizleme sürümü, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve bunu üretim iş yükleri için kullanmanızı önermiyoruz. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. 
> Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Konak havuzları, Windows sanal masaüstü kiracı ortamlarında bir veya daha fazla özdeş sanal makine koleksiyonudur. Ana bilgisayar havuzlarını üretim ortamınıza dağıtılmadan önce, bir doğrulama ana bilgisayar havuzu oluşturmanızı kesinlikle öneririz. Güncelleştirmeler önce doğrulama ana bilgisayar havuzlarına uygulanır, böylece üretim ortamınıza göndermeden önce hizmet güncelleştirmelerini izlemenizi sağlar. Doğrulama ana bilgisayar havuzu olmadan, hata oluşturan değişiklikleri bulamamanıza neden olabilir ve bu da üretim ortamınızdaki kullanıcılar için kapalı kalma süresine yol açabilir.

Uygulamalarınızın en son güncelleştirmelerle çalışmasını sağlamak için, doğrulama ana bilgisayar havuzu, üretim ortamınızda mümkün olduğunca ana bilgisayar havuzlarına benzer olmalıdır. Kullanıcıların, üretim ana bilgisayar havuzunda çalıştıkları şekilde doğrulama ana bilgisayar havuzuna sık olarak bağlanması gerekir. Konak havuzunuzdaki otomatik test testiniz varsa, doğrulama ana bilgisayar havuzuna otomatik test eklemeniz gerekir.

[Tanılama özelliğiyle](diagnostics-role-service.md) veya [Windows sanal masaüstü sorunlarını giderme makaleleriyle](troubleshoot-set-up-overview.md)doğrulama konak havuzundaki sorunları ayıklayabilirsiniz.

>[!NOTE]
> Sonraki tüm güncelleştirmeleri test etmek için doğrulama ana bilgisayar havuzunu yerinde bırakmanız önerilir.

>[!IMPORTANT]
>Windows sanal masaüstü Spring 2020 sürümü şu anda doğrulama ortamını etkinleştirme ve devre dışı bırakma konusunda sorun yaşıyor. Sorunu çöztiğimiz zaman bu makaleyi güncelleştireceğiz.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce, PowerShell modülünüzü ayarlamak ve Azure 'da oturum açmak için [Windows sanal masaüstü PowerShell modülünü ayarlama](powershell-module.md) bölümündeki yönergeleri izleyin.

## <a name="create-your-host-pool"></a>Konak havuzunuzu oluşturma

Aşağıdaki makalelerdeki yönergeleri izleyerek bir konak havuzu oluşturabilirsiniz:
- [Öğretici: Azure Marketi ile bir konak havuzu oluşturma](create-host-pools-azure-marketplace.md)
- [PowerShell ile ana bilgisayar havuzu oluşturma](create-host-pools-powershell.md)

## <a name="define-your-host-pool-as-a-validation-host-pool"></a>Konak havuzunuzu doğrulama ana bilgisayar havuzu olarak tanımlama

Yeni konak havuzunu doğrulama konak havuzu olarak tanımlamak için aşağıdaki PowerShell cmdlet 'lerini çalıştırın. Köşeli ayraçlar içindeki değerleri oturumunuzla ilgili değerlerle değiştirin:

```powershell
Update-AzWvdHostPool -ResourceGroupName <resourcegroupname> -Name <hostpoolname> -ValidationEnvironment:$true
```

Doğrulama özelliğinin ayarlandığını onaylamak için aşağıdaki PowerShell cmdlet 'ini çalıştırın. Köşeli ayraçlar içindeki değerleri oturumunuzla ilgili değerlerle değiştirin.

```powershell
Get-AzWvdHostPool -ResourceGroupName <resourcegroupname> -Name <hostpoolname> | Format-List
```

Cmdlet 'inin sonuçları şu çıkışa benzer görünmelidir:

```powershell
    HostPoolName        : hostpoolname
    FriendlyName        :
    Description         :
    Persistent          : False 
    CustomRdpProperty   : use multimon:i:0;
    MaxSessionLimit     : 10
    LoadBalancerType    : BreadthFirst
    ValidationEnvironment : True
```

## <a name="update-schedule"></a>Zamanlamayı Güncelleştir

Hizmet güncelleştirmeleri ayda bir gerçekleşir. Önemli sorunlar varsa, kritik güncelleştirmeler daha sık bir hızda sağlanacaktır.

Herhangi bir hizmet güncelleştirmesi varsa, ortamı doğrulamak için her gün en az küçük bir kullanıcı grubuna kaydolduğunuzdan emin olun. Hizmet güncelleştirmeleri hakkında bilgi sahibi olmak için [Techcommunity sitemizi](https://techcommunity.microsoft.com/t5/forums/searchpage/tab/message?filter=location&q=wvdupdate&location=forum-board:WindowsVirtualDesktop&sort_by=-topicPostDate&collapse_discussion=true) düzenli olarak ziyaret etmenizi ve WVDUPdate ile tüm gönderileri izlemenizi öneririz.

## <a name="next-steps"></a>Sonraki adımlar

Bir doğrulama ana bilgisayar havuzu oluşturduğunuza göre, Windows sanal masaüstü dağıtımınızı izlemek için Azure hizmet durumunu nasıl kullanacağınızı öğrenebilirsiniz. 

> [!div class="nextstepaction"]
> [Hizmet uyarılarını ayarlama](./set-up-service-alerts.md)
