---
title: PowerShell ile paylaşılan kendi kendine barındırılan tümleştirme çalışma süresi oluşturun
description: Birden çok veri fabrikasının tümleştirme çalışma süresine erişebilmeleri için Azure Veri Fabrikası'nda paylaşılan kendi barındırılan tümleştirme çalışma zamanı nasıl oluşturabileceğinizi öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.author: abnarain
author: nabhishek
manager: anansub
ms.custom: seo-lt-2019
ms.date: 10/31/2018
ms.openlocfilehash: a2f24d8203ac5fb9724370cbdf4309bdc43c166a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75444093"
---
# <a name="create-a-shared-self-hosted-integration-runtime-in-azure-data-factory"></a>Azure Veri Fabrikası'nda paylaşılan kendi barındırılan tümleştirme çalışma zamanı oluşturma

Bu kılavuz, Azure Veri Fabrikası'nda kendi kendine barındırılan paylaşılan bir tümleştirme çalışma süresini nasıl oluşturabileceğinizi gösterir. Ardından, paylaşılan kendi kendine barındırılan tümleştirme çalışma zamanını başka bir veri fabrikasında kullanabilirsiniz.

## <a name="create-a-shared-self-hosted-ir-using-azure-data-factory-ui"></a>Azure Veri Fabrikası Kullanıcı UI'yi kullanarak paylaşılan kendi kendine barındırılan bir IR oluşturun

Azure Veri Fabrikası Kullanıcı UI'sini kullanarak kendi kendine barındırılan paylaşılan bir IR oluşturmak için aşağıdaki adımları atabilirsiniz:

1. Paylaşılacak şekilde barındırılan IR'de, bağlantılı IR'yi oluşturmak istediğiniz veri fabrikasına izin verin.
      
    ![Paylaşım sekmesinde izin vermek için düğme](media/create-self-hosted-integration-runtime/grant-permissions-IR-sharing.png)
      
    ![İzin atama seçimleri](media/create-self-hosted-integration-runtime/3_rbac_permissions.png)     
    
2. Paylaşılacak kendi barındırılan IR'nin kaynak kimliğine dikkat edin.
      
   ![Kaynak kimliğinin konumu](media/create-self-hosted-integration-runtime/4_ResourceID_self-hostedIR.png)
    
3. İzinlerin verildiği veri fabrikasında, kendi kendine barındırılan yeni bir IR (bağlantılı) oluşturun ve kaynak kimliğini girin.
      
   ![Bağlantılı, barındırılan tümleştirme çalışma zamanı oluşturmak için düğme](media/create-self-hosted-integration-runtime/6_create-linkedIR_2.png)
      
    ![Ad ve kaynak kimliği için kutular](media/create-self-hosted-integration-runtime/6_create-linkedIR_3.png)

## <a name="create-a-shared-self-hosted-ir-using-azure-powershell"></a>Azure PowerShell'i kullanarak kendi kendine barındırılan paylaşılan bir IR oluşturun

Azure PowerShell'i kullanarak kendi kendine barındırılan paylaşılan bir IR oluşturmak için aşağıdaki adımları atabilirsiniz: 
1. Veri fabrikası oluşturma. 
1. Şirket içinde barındırılan tümleştirme çalışma zamanı oluşturma.
1. Kendi kendine barındırılan tümleştirme çalışma süresini diğer veri fabrikalarıyla paylaşın.
1. Bağlantılı bir tümleştirme çalışma zamanı oluşturun.
1. Paylaşımı iptal edin.

### <a name="prerequisites"></a>Ön koşullar 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

- **Azure aboneliği.** Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/) 

- **Azure PowerShell**. [PowerShellGet ile Windows'da Azure PowerShell'i Yükleyin](https://docs.microsoft.com/powershell/azure/install-az-ps)yönergeleriizleyin. PowerShell'i, diğer veri fabrikalarıyla paylaşılabilen, kendi kendine barındırılan bir tümleştirme çalışma süresi oluşturmak için bir komut dosyası çalıştırmak için kullanırsınız. 

> [!NOTE]  
> Veri Fabrikası'nın şu anda kullanılabildiği Azure bölgelerinin listesi için, [bölgeye göre kullanılabilen Ürünler'de](https://azure.microsoft.com/global-infrastructure/services/?products=data-factory)ilginizi çeken bölgeleri seçin.

### <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. Windows PowerShell Tümleşik Komut Dosyası Ortamı’nı (ISE) başlatın.

1. Değişkenler oluşturun. Aşağıdaki komut dosyasını kopyalayın ve yapıştırın. **SubscriptionName** ve **ResourceGroupName**gibi değişkenleri gerçek değerlerle değiştirin: 

    ```powershell
    # If input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$". 
    $SubscriptionName = "[Azure subscription name]" 
    $ResourceGroupName = "[Azure resource group name]" 
    $DataFactoryLocation = "EastUS" 

    # Shared Self-hosted integration runtime information. This is a Data Factory compute resource for running any activities 
    # Data factory name. Must be globally unique 
    $SharedDataFactoryName = "[Shared Data factory name]" 
    $SharedIntegrationRuntimeName = "[Shared Integration Runtime Name]" 
    $SharedIntegrationRuntimeDescription = "[Description for Shared Integration Runtime]"

    # Linked integration runtime information. This is a Data Factory compute resource for running any activities
    # Data factory name. Must be globally unique
    $LinkedDataFactoryName = "[Linked Data factory name]"
    $LinkedIntegrationRuntimeName = "[Linked Integration Runtime Name]"
    $LinkedIntegrationRuntimeDescription = "[Description for Linked Integration Runtime]"
    ```

1. Oturum açın ve bir abonelik seçin. Oturum açmak ve Azure aboneliğinizi seçmek için komut dosyasına aşağıdaki kodu ekleyin:

    ```powershell
    Connect-AzAccount
    Select-AzSubscription -SubscriptionName $SubscriptionName
    ```

1. Bir kaynak grubu ve veri fabrikası oluşturun.

    > [!NOTE]  
    > Bu adım isteğe bağlıdır. Zaten bir veri fabrikanız varsa, bu adımı atlayın. 

    [Yeni-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) komutunu kullanarak bir [Azure kaynak grubu](../azure-resource-manager/management/overview.md) oluşturun. Kaynak grubu, Azure kaynaklarının grup olarak dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnek, WestEurope konumunda `myResourceGroup` adı verilen bir kaynak grubu oluşturur: 

    ```powershell
    New-AzResourceGroup -Location $DataFactoryLocation -Name $ResourceGroupName
    ```

    Veri fabrikası oluşturmak için aşağıdaki komutu çalıştırın: 

    ```powershell
    Set-AzDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                             -Location $DataFactoryLocation `
                             -Name $SharedDataFactoryName
    ```

### <a name="create-a-self-hosted-integration-runtime"></a>Şirket içinde barındırılan tümleştirme çalışma zamanı oluşturma

> [!NOTE]  
> Bu adım isteğe bağlıdır. Diğer veri fabrikalarıyla paylaşmak istediğiniz kendi kendine barındırılan tümleştirme çalışma süresine zaten sahipseniz, bu adımı atlayın.

Kendi kendine barındırılan tümleştirme çalışma zamanı oluşturmak için aşağıdaki komutu çalıştırın:

```powershell
$SharedIR = Set-AzDataFactoryV2IntegrationRuntime `
    -ResourceGroupName $ResourceGroupName `
    -DataFactoryName $SharedDataFactoryName `
    -Name $SharedIntegrationRuntimeName `
    -Type SelfHosted `
    -Description $SharedIntegrationRuntimeDescription
```

#### <a name="get-the-integration-runtime-authentication-key-and-register-a-node"></a>Tümleştirme çalışma zamanı kimlik doğrulama anahtarını alın ve bir düğüm kaydettirin

Kendi kendine barındırılan tümleştirme çalışma zamanı için kimlik doğrulama anahtarını almak için aşağıdaki komutu çalıştırın:

```powershell
Get-AzDataFactoryV2IntegrationRuntimeKey `
    -ResourceGroupName $ResourceGroupName `
    -DataFactoryName $SharedDataFactoryName `
    -Name $SharedIntegrationRuntimeName
```

Yanıt, bu kendi kendine barındırılan tümleştirme çalışma zamanı için kimlik doğrulama anahtarını içerir. Tümleştirme çalışma zamanı düğümlerini kaydederken bu anahtarı kullanırsınız.

#### <a name="install-and-register-the-self-hosted-integration-runtime"></a>Kendi barındırılan tümleştirme çalışma süresini yükleme ve kaydetme

1. [Azure Veri Fabrikası Tümleştirme Çalışma Süresi'nden](https://aka.ms/dmg)kendi kendine barındırılan tümleştirme çalışma zamanı yükleyicisini indirin.

2. Kendi kendine barındırılan tümleştirmeyi yerel bir bilgisayara yüklemek için yükleyiciyi çalıştırın.

3. Önceki adımda aldığınız kimlik doğrulama anahtarıyla yeni kendi barındırılan tümleştirmeyi kaydedin.

### <a name="share-the-self-hosted-integration-runtime-with-another-data-factory"></a>Kendi kendine barındırılan tümleştirme çalışma süresini başka bir veri fabrikasıyla paylaşma

#### <a name="create-another-data-factory"></a>Başka bir veri fabrikası oluşturma

> [!NOTE]  
> Bu adım isteğe bağlıdır. Paylaşmak istediğiniz veri fabrikası zaten varsa, bu adımı atlayın.

```powershell
$factory = Set-AzDataFactoryV2 -ResourceGroupName $ResourceGroupName `
    -Location $DataFactoryLocation `
    -Name $LinkedDataFactoryName
```
#### <a name="grant-permission"></a>Hibe izni

Oluşturduğunuz ve kaydettiğiniz kendi kendine barındırılan tümleştirme çalışma süresine erişmesi gereken veri fabrikasına izin verin.

> [!IMPORTANT]  
> Bu adımı atlamayın!

```powershell
New-AzRoleAssignment `
    -ObjectId $factory.Identity.PrincipalId ` #MSI of the Data Factory with which it needs to be shared
    -RoleDefinitionId 'b24988ac-6180-42a0-ab88-20f7382dd24c' ` #This is the Contributor role
    -Scope $SharedIR.Id
```

### <a name="create-a-linked-self-hosted-integration-runtime"></a>Bağlantılı, barındırılan tümleştirme çalışma zamanı oluşturma

Bağlantılı, barındırılan tümleştirme çalışma zamanı oluşturmak için aşağıdaki komutu çalıştırın:

```powershell
Set-AzDataFactoryV2IntegrationRuntime `
    -ResourceGroupName $ResourceGroupName `
    -DataFactoryName $LinkedDataFactoryName `
    -Name $LinkedIntegrationRuntimeName `
    -Type SelfHosted `
    -SharedIntegrationRuntimeResourceId $SharedIR.Id `
    -Description $LinkedIntegrationRuntimeDescription
```

Artık bu bağlantılı tümleştirme çalışma zamanını bağlantılı herhangi bir hizmette kullanabilirsiniz. Bağlı tümleştirme çalışma süresi, etkinlikleri çalıştırmak için paylaşılan tümleştirme çalışma zamanını kullanır.

### <a name="revoke-integration-runtime-sharing-from-a-data-factory"></a>Veri fabrikasından tümleştirme çalışma zamanı paylaşımını iptal edin

Paylaşılan tümleştirme çalışma zamanından bir veri fabrikasının erişimini iptal etmek için aşağıdaki komutu çalıştırın:

```powershell
Remove-AzRoleAssignment `
    -ObjectId $factory.Identity.PrincipalId `
    -RoleDefinitionId 'b24988ac-6180-42a0-ab88-20f7382dd24c' `
    -Scope $SharedIR.Id
```

Varolan bağlı tümleştirme çalışma süresini kaldırmak için paylaşılan tümleştirme çalışma süresine karşı aşağıdaki komutu çalıştırın:

```powershell
Remove-AzDataFactoryV2IntegrationRuntime `
    -ResourceGroupName $ResourceGroupName `
    -DataFactoryName $SharedDataFactoryName `
    -Name $SharedIntegrationRuntimeName `
    -Links `
    -LinkedDataFactoryName $LinkedDataFactoryName
```

### <a name="next-steps"></a>Sonraki adımlar

- [Azure Veri Fabrikası'nda tümleştirme çalışma zamanı kavramlarını](https://docs.microsoft.com/azure/data-factory/concepts-integration-runtime)gözden geçirin.

- [Azure portalında kendi kendine barındırılan bir tümleştirme çalışma zamanı oluşturmayı](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime)öğrenin.
