---
title: PowerShell ile Azure Güvenlik Merkezi'ne dahil
description: Bu belge, PowerShell cmdlets kullanarak Azure Güvenlik Merkezi'ne binme sürecinde size yol göstersin.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: e400fcbf-f0a8-4e10-b571-5a0d0c3d0c67
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/02/2018
ms.author: memildin
ms.openlocfilehash: 5aaaf539c07a7ba2c2463d5bfd1f452853f52379
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77603682"
---
# <a name="automate-onboarding-of-azure-security-center-using-powershell"></a>PowerShell'i kullanarak Azure Güvenlik Merkezi'ne binme otomatikleştirin

Azure Güvenlik Merkezi PowerShell modüllerini kullanarak Azure iş yüklerinizi programlı bir şekilde güvene alabilirsiniz.
PowerShell'i kullanmak, görevleri otomatikleştirmenize ve el ile görevlerin doğasında bulunan insan hatalarından kaçınmanıza olanak tanır. Bu, özellikle yüzlerce ve binlerce kaynağa sahip düzinelerce abonelik içeren büyük ölçekli dağıtımlarda yararlıdır ve bunların tümü en başından itibaren güvence altına alınmalıdır.

PowerShell'i kullanarak Azure Güvenlik Merkezi'ne binme, Azure kaynaklarınızın biniş ve yönetimini programlı bir şekilde otomatikleştirmenize ve gerekli güvenlik denetimlerini eklemenize olanak tanır.

Bu makalede, güvenlik merkezini aboneliklerinizarasında kullanıma açmak için değiştirilebilen ve ortamınızda kullanılabilecek örnek bir PowerShell komut dosyası sağlar. 

Bu örnekte, Güvenlik Merkezi'ni kimliği olan bir abonelik teetkinleştireceğiz: d07c0080-170c-4c24-861d-9c817742786c ve yüksek düzeyde koruma sağlayan önerilen ayarları uygulayarak, Güvenlik Merkezi'nin Standart katmanını uygulayarak gelişmiş tehdit koruma ve algılama yetenekleri:

1. Güvenlik [Merkezi standart koruma düzeyini](https://azure.microsoft.com/pricing/details/security-center/)ayarlayın. 
 
2. Microsoft İzleme Aracısı'nın abonelikle ilişkili VM'lerde topladığı verileri göndereceği Log Analytics çalışma alanını ayarlayın – bu örnekte, mevcut bir kullanıcı tanımlı çalışma alanı (myWorkspace).

3. Güvenlik Merkezi'nin Microsoft İzleme [Aracısını dağıtan](security-center-enable-data-collection.md#auto-provision-mma)otomatik aracı sağlamasını etkinleştirin.

5. [Güvenlik Merkezi uyarıları ve önemli olaylar için güvenlik iletişim itibat olarak](security-center-provide-security-contact-details.md)kuruluşun CISO ayarlayın.

6. Güvenlik Merkezi'nin [varsayılan güvenlik ilkelerini](tutorial-security-policy.md)atayın.

## <a name="prerequisites"></a>Ön koşullar

Güvenlik Merkezi cmdlets çalıştırmadan önce bu adımlar yapılmalıdır:

1.  PowerShell'i yönetici olarak çalıştırın.
2.  PowerShell'de aşağıdaki komutları çalıştırın:
      
        Set-ExecutionPolicy -ExecutionPolicy AllSigned
        Install-Module -Name Az.Security -Force

## <a name="onboard-security-center-using-powershell"></a>PowerShell'i kullanarak Yerleşik Güvenlik Merkezi

1.  Aboneliklerinizi Güvenlik Merkezi Kaynak Sağlayıcısına kaydedin:

        Set-AzContext -Subscription "d07c0080-170c-4c24-861d-9c817742786c"
        Register-AzResourceProvider -ProviderNamespace 'Microsoft.Security' 

2.  İsteğe bağlı: Aboneliklerin kapsama düzeyini (fiyatlandırma katmanı) ayarlayın (Tanımlanmamışsa, fiyatlandırma katmanı Ücretsiz olarak ayarlanır):

        Set-AzContext -Subscription "d07c0080-170c-4c24-861d-9c817742786c"
        Set-AzSecurityPricing -Name "default" -PricingTier "Standard"

3.  Aracıların rapor edeceği bir Günlük Analizi çalışma alanını yapılandırın. Daha önce oluşturduğunuz ve aboneliğin VM'lerinin rapor edeceği bir Log Analytics çalışma alanına sahip olmalısınız. Aynı çalışma alanına rapor vermek için birden çok abonelik tanımlayabilirsiniz. Tanımlanmamışsa, varsayılan çalışma alanı kullanılır.

        Set-AzSecurityWorkspaceSetting -Name "default" -Scope
        "/subscriptions/d07c0080-170c-4c24-861d-9c817742786c" -WorkspaceId"/subscriptions/d07c0080-170c-4c24-861d-9c817742786c/resourceGroups/myRg/providers/Microsoft.OperationalInsights/workspaces/myWorkspace"

4.  Azure VM'lerinizde Microsoft İzleme Aracısı'nın otomatik sağlama yüklemesi:
    
        Set-AzContext -Subscription "d07c0080-170c-4c24-861d-9c817742786c"
    
        Set-AzSecurityAutoProvisioningSetting -Name "default" -EnableAutoProvision

    > [!NOTE]
    > Azure sanal makinelerinizin Azure Güvenlik Merkezi tarafından otomatik olarak korunduğundan emin olmak için otomatik sağlamayı etkinleştirme önerilir.
    >

5.  İsteğe bağlı: Güvenlik Merkezi tarafından oluşturulan uyarıların ve bildirimlerin alıcıları olarak kullanılacak olan, gemide bulunan aboneliklerin güvenlik iletişim bilgilerini tanımlamanız önerilir:

        Set-AzSecurityContact -Name "default1" -Email "CISO@my-org.com" -Phone "2142754038" -AlertAdmin -NotifyOnAlert 

6.  Varsayılan Güvenlik Merkezi ilkesi girişimini atama:

        Register-AzResourceProvider -ProviderNamespace 'Microsoft.PolicyInsights'
        $Policy = Get-AzPolicySetDefinition | where {$_.Properties.displayName -EQ '[Preview]: Enable Monitoring in Azure Security Center'}
        New-AzPolicyAssignment -Name 'ASC Default <d07c0080-170c-4c24-861d-9c817742786c>' -DisplayName 'Security Center Default <subscription ID>' -PolicySetDefinition $Policy -Scope '/subscriptions/d07c0080-170c-4c24-861d-9c817742786c'

PowerShell ile Azure Güvenlik Merkezi'ne başarılı bir şekilde binmişsiniz!

Artık bu PowerShell cmdlets otomasyon komut dosyaları ile programlı abonelikler ve kaynaklar arasında yeniden kullanılabilir. Bu zaman kazandırır ve insan hatası olasılığını azaltır. Bu örnek [komut dosyalarını](https://github.com/Microsoft/Azure-Security-Center/blob/master/quickstarts/ASC-Samples.ps1) başvuru olarak kullanabilirsiniz.






## <a name="see-also"></a>Ayrıca bkz.
PowerShell'i Güvenlik Merkezi'ne binmeyi otomatikleştirmek için nasıl kullanabileceğiniz hakkında daha fazla bilgi edinmek için aşağıdaki makaleye bakın:

* [Az.Güvenlik](https://docs.microsoft.com/powershell/module/az.security).

Güvenlik Merkezi hakkında daha fazla bilgi edinmek için aşağıdaki makaleye bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](tutorial-security-policy.md) -- Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'ndeki güvenlik uyarılarını yönetme ve yanıtla](security-center-managing-and-responding-alerts.md) -- Güvenlik uyarılarını nasıl yönetip yanıtlaştak yapılacağını öğrenin.