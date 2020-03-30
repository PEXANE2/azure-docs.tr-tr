---
title: Yönetilen kimliklerle ilgili SSS'ler ve bilinen sorunlar - Azure AD
description: Azure kaynakları için yönetilen kimliklerle ilgili bilinen sorunlar.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 2097381a-a7ec-4e3b-b4ff-5d2fb17403b6
ms.service: active-directory
ms.subservice: msi
ms.devlang: ''
ms.topic: conceptual
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 12/12/2017
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: f0f0c678f2426d9de58d2ab337c56243394b4d0f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79266539"
---
# <a name="faqs-and-known-issues-with-managed-identities-for-azure-resources"></a>Azure kaynakları için yönetilen kimliklerle ilgili SSS'ler ve bilinen sorunlar

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

## <a name="frequently-asked-questions-faqs"></a>Sık Sorulan Sorular (SSS)

> [!NOTE]
> Azure kaynakları için yönetilen kimlikler, Yönetilen Hizmet Kimliği (MSI) olarak bilinen hizmetin yeni adıdır.

### <a name="does-managed-identities-for-azure-resources-work-with-azure-cloud-services"></a>Azure kaynakları için yönetilen kimlikler Azure Bulut Hizmetleri ile çalışır mı?

Hayır, Azure Bulut Hizmetleri'nde Azure kaynakları için yönetilen kimlikleri destekleme planı yoktur.

### <a name="does-managed-identities-for-azure-resources-work-with-the-active-directory-authentication-library-adal-or-the-microsoft-authentication-library-msal"></a>Azure kaynakları için yönetilen kimlikler Active Directory Authentication Library (ADAL) veya Microsoft Kimlik Doğrulama Kitaplığı (MSAL) ile birlikte çalışır mı?

Hayır, Azure kaynakları için yönetilen kimlikler henüz ADAL veya MSAL ile tümleşik değil. REST bitiş noktasını kullanarak Azure kaynakları için yönetilen kimlikler için bir belirteç edinme yle ilgili ayrıntılar için, [erişim belirteci elde etmek için Azure kaynaklarının yönetilen kimliklerinin azure vm'de nasıl kullanılacağını](how-to-use-vm-token.md)öğrenin.

### <a name="what-is-the-security-boundary-of-managed-identities-for-azure-resources"></a>Azure kaynakları için yönetilen kimliklerin güvenlik sınırı nedir?

Kimliğin güvenlik sınırı, bağlı olduğu kaynaktır. Örneğin, Azure kaynakları için yönetilen kimlikleri etkinleştirilmiş bir Sanal Makine'nin güvenlik sınırı Sanal Makine'dir. Bu VM'de çalışan herhangi bir kod, Azure kaynakları bitiş noktası ve istek belirteçleri için yönetilen kimlikleri arayabilir. Azure kaynakları için yönetilen kimlikleri destekleyen diğer kaynaklarla ilgili benzer bir deneyimdir.

### <a name="what-identity-will-imds-default-to-if-dont-specify-the-identity-in-the-request"></a>İstekte kimlik belirtmezse IMDS hangi kimliği varsayılan olarak ne olur?

- Yönetilen kimlik atanan sistem etkinleştirilir ve istekte kimlik belirtilmemişse, IMDS yönetilen kimlik atanan sisteme varsayılan olarak atanacaktır.
- Yönetilen kimlik atanan sistem etkinleştirilmezse ve yalnızca bir kullanıcı atanmış yönetilen kimlik varsa, IMDS varsayılan olarak yönetilen kimlik atanan tek bir kullanıcıya atanacaktır. 
- Yönetilen kimlik atanan sistem etkinleştirilmiyorsa ve birden çok kullanıcı atanan yönetilen kimlikler varsa, istekte yönetilen bir kimlik belirtilmesi gerekir.

### <a name="should-i-use-the-managed-identities-for-azure-resources-imds-endpoint-or-the-vm-extension-endpoint"></a>Azure kaynakları IMDS bitiş noktası veya VM uzantı bitiş noktası için yönetilen kimlikleri kullanmalı mıyım?

VM'lerle Azure kaynakları için yönetilen kimlikleri kullanırken, IMDS bitiş noktasını kullanmanızı öneririz. Azure Örneği Meta Veri Hizmeti, Azure Kaynak Yöneticisi aracılığıyla oluşturulan tüm IaaS VM'ler tarafından erişilebilen bir REST Bitiş Noktasıdır. 

IMDS üzerinden Azure kaynakları için yönetilen kimlikler kullanmanın avantajlarından bazıları şunlardır:
- Azure IaaS desteklenen tüm işletim sistemleri, IMDS üzerinden Azure kaynakları için yönetilen kimlikleri kullanabilir.
- Azure kaynakları için yönetilen kimlikleri etkinleştirmek için artık VM'nize bir uzantı yüklemeniz gerekmez. 
- Azure kaynakları için yönetilen kimlikler tarafından kullanılan sertifikalar artık VM'de bulunmaz.
- IMDS bitiş noktası, sadece VM'nin içinden temin edilebilen, iyi bilinen, routable olmayan bir IP adresidir.
- Tek bir VM'ye 1000 kullanıcı tarafından atanan yönetilen kimlikler atanabilir. 

Azure kaynakları VM uzantısı için yönetilen kimlikler hala kullanılabilir; ancak, artık yeni işlevsellik geliştirmiyoruz. IMDS bitiş noktasını kullanmak için geçiş yapmanızı öneririz. 

VM uzantı uç noktasını kullanmanın bazı sınırlamaları şunlardır:
- Linux dağıtımları için sınırlı destek: CoreOS Stable, CentOS 7.1, Red Hat 7.2, Ubuntu 15.04, Ubuntu 16.04
- VM'ye yalnızca 32 kullanıcı tarafından atanan yönetilen kimlik atanabilir.


Not: Azure kaynakları VM uzantısı için yönetilen kimlikler Ocak 2019'da destekten olmaz. 

Azure Örneği Meta veri Hizmeti hakkında daha fazla bilgi için [BKZ.](https://docs.microsoft.com/azure/virtual-machines/windows/instance-metadata-service)

### <a name="will-managed-identities-be-recreated-automatically-if-i-move-a-subscription-to-another-directory"></a>Bir aboneliği başka bir dizine taşırsam yönetilen kimlikler otomatik olarak yeniden oluşturulur mı?

Hayır. Bir aboneliği başka bir dizine taşırsanız, bunları el ile yeniden oluşturmanız ve Azure RBAC rol atamalarını yeniden vermeniz gerekir.
- Yönetilen kimlikler atanan sistem için: devre dışı ve yeniden etkinleştirin. 
- Yönetilen kimlikler atanan kullanıcı için: bunları silme, yeniden oluşturma ve gerekli kaynaklara yeniden iliştirme (örn. sanal makineler)

### <a name="can-i-use-a-managed-identity-to-access-a-resource-in-a-different-directorytenant"></a>Farklı bir dizindeki/kiracıdaki bir kaynağa erişmek için yönetilen bir kimliği kullanabilir miyim?

Hayır. Yönetilen kimlikler şu anda çapraz dizin senaryolarını desteklemiyor. 

### <a name="what-azure-rbac-permissions-are-required-to-managed-identity-on-a-resource"></a>Kaynak üzerinde yönetilen kimlik için hangi Azure RBAC izinleri gereklidir? 

- Sistem tarafından atanan yönetilen kimlik: Kaynak üzerinde yazma izinleri gerekir. Örneğin sanal makineler için Microsoft.Compute/virtualMachines/write iznine ihtiyaç duyulur. Bu eylem, [Sanal Makine Katılımcısı](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#virtual-machine-contributor)gibi kaynağa özel yerleşik rollere dahildir.
- Kullanıcı tarafından atanan yönetilen kimlik: Kaynak üzerinde yazma izinleri gerekir. Örneğin sanal makineler için Microsoft.Compute/virtualMachines/write iznine ihtiyaç duyulur. Yönetilen kimlik üzerinden [Yönetilen Kimlik İşlemi](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#managed-identity-operator) rol atamasına ek olarak.

### <a name="how-do-you-restart-the-managed-identities-for-azure-resources-extension"></a>Azure kaynakları uzantısı için yönetilen kimlikleri nasıl yeniden başlatabilirsiniz?
Windows'da ve Linux'un belirli sürümlerinde, uzantı durursa, aşağıdaki cmdlet el ile yeniden başlatmak için kullanılabilir:

```powershell
Set-AzVMExtension -Name <extension name>  -Type <extension Type>  -Location <location> -Publisher Microsoft.ManagedIdentity -VMName <vm name> -ResourceGroupName <resource group name> -ForceRerun <Any string different from any last value used>
```

Konumlar: 
- Windows için uzantı adı ve türü: ManagedIdentityExtensionForWindows
- Linux için uzantı adı ve türü: ManagedIdentityExtensionForLinux

## <a name="known-issues"></a>Bilinen sorunlar

### <a name="automation-script-fails-when-attempting-schema-export-for-managed-identities-for-azure-resources-extension"></a>Azure kaynakları uzantısı için yönetilen kimlikler için şema dışa aktarma girişiminde "otomasyon komut dosyası" başarısız olur

Bir VM'de Azure kaynakları için yönetilen kimlikler etkinleştirildiğinde, VM veya kaynak grubu için "Otomasyon komut dosyası" özelliğini kullanmaya çalışırken aşağıdaki hata gösterilir:

![Azure kaynakları otomasyonu komut dosyası dışa aktarma hatası için yönetilen kimlikler](./media/msi-known-issues/automation-script-export-error.png)

Azure kaynakları VM uzantısı için yönetilen kimlikler (Ocak 2019'da amortisman alabilmek için planlanmıştır) şu anda şemasını bir kaynak grubu şablonuna dışa aktarma özelliğini desteklemiyor. Sonuç olarak, oluşturulan şablon, kaynaktaki Azure kaynakları için yönetilen kimlikleri etkinleştirmek için yapılandırma parametrelerini göstermez. Bu bölümler, [bir Azure VM'de Azure kaynakları için yönetilen kimlikleri yapılandırma](qs-configure-template-windows-vm.md)örneklerinde şablonlar kullanılarak el ile eklenebilir.

Şema dışa aktarma işlevi Azure kaynakları VM uzantısı için yönetilen kimlikler için kullanılabilir olduğunda (Ocak 2019'da amortisman için planlanan), [VM uzantıları içeren Kaynak Grupları Dışa Aktarma'da](../../virtual-machines/extensions/export-templates.md#supported-virtual-machine-extensions)listelenir.

### <a name="vm-fails-to-start-after-being-moved-from-resource-group-or-subscription"></a>VM kaynak grubundan veya abonelikten taşındıktan sonra başlatılamıyor

Çalışan durumda bir VM taşırsanız, hareket sırasında çalışmaya devam ediyor. Ancak, taşımadan sonra, VM durdurulur ve yeniden başlatılırsa, başlatılamayan. Bu sorun, VM'nin Azure kaynakları kimliği için yönetilen kimliklere başvuruyu güncelleştirmemesi ve eski kaynak grubunda işaret etmeye devam etmesi nedeniyle ortaya çıkar.

**Geçi -ci çözüm** 
 
Azure kaynakları için yönetilen kimlikler için doğru değerleri elde edebilmek için VM'de bir güncelleştirmetetikleyin. Azure kaynakları kimliği için yönetilen kimliklere yapılan başvuruyu güncelleştirmek için VM özellik değişikliği yapabilirsiniz. Örneğin, VM'de aşağıdaki komutla yeni bir etiket değeri ayarlayabilirsiniz:

```azurecli-interactive
 az  vm update -n <VM Name> -g <Resource Group> --set tags.fixVM=1
```
 
Bu komut, VM'de 1 değeri olan yeni bir etiket "fixVM" ayarlar. 
 
Bu özelliği ayarlayarak, Azure kaynakları kaynağı URI için doğru yönetilen kimliklerle VM güncelleştirir ve ardından VM'yi başlatabilmelisiniz. 
 
VM başlatıldıktan sonra, etiket aşağıdaki komut kullanılarak kaldırılabilir:

```azurecli-interactive
az vm update -n <VM Name> -g <Resource Group> --remove tags.fixVM
```

### <a name="vm-extension-provisioning-fails"></a>VM uzantısı sağlama başarısız olur

VM uzantısının sağlanması DNS arama hataları nedeniyle başarısız olabilir. VM'yi yeniden başlatın ve yeniden deneyin.
 
> [!NOTE]
> VM uzantısı Ocak 2019'a kadar amortisman alabına kadar planlanmıştır. IMDS bitiş noktasını kullanmanızı öneririz.

### <a name="transferring-a-subscription-between-azure-ad-directories"></a>Azure REKLAM dizinleri arasında abonelik aktarma

Yönetilen kimlikler, abonelik taşındığında/başka bir dizine aktarıldığında güncelleştirilmez. Sonuç olarak, varolan sistem atanan veya kullanıcı tarafından atanan yönetilen kimlikler bozulur. 

Başka bir dizine taşınmış bir abonelikte yönetilen kimlikler için geçici çözüm:

 - Yönetilen kimlikler atanan sistem için: devre dışı ve yeniden etkinleştirin. 
 - Yönetilen kimlikler atanan kullanıcı için: bunları silme, yeniden oluşturma ve gerekli kaynaklara yeniden iliştirme (örn. sanal makineler)

### <a name="moving-a-user-assigned-managed-identity-to-a-different-resource-groupsubscription"></a>Kullanıcı tarafından atanan yönetilen kimliği farklı bir kaynak grubuna/aboneliğe taşıma

Kullanıcı tarafından atanan yönetilen bir kimliğin farklı bir kaynak grubuna taşınması, kimliğin kırılmasına neden olur. Sonuç olarak, bu kimliği kullanan kaynaklar (örneğin VM) bunun için belirteç isteyemez. 
