---
title: Event Hubs kullanarak günlükleri Azure Key Vault tümleştirin | Microsoft Docs
description: Azure günlük tümleştirmesi kullanarak Key Vault günlüklerinin bir SıEM tarafından kullanılabilmesini sağlamak için gerekli adımları sağlayan öğretici
services: security
author: barclayn
manager: barbkess
editor: TomShinder
ms.assetid: ''
ms.service: security
ms.subservice: security-fundamentals
ms.topic: article
ms.date: 05/28/2019
ms.author: Barclayn
ms.custom: AzLog
ms.openlocfilehash: 01ab4f6f0535f137c7ffeb99c36ecd1e831de6f7
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68727673"
---
# <a name="azure-log-integration-tutorial-process-azure-key-vault-events-by-using-event-hubs"></a>Azure günlük tümleştirme öğreticisi: Event Hubs kullanarak Azure Key Vault olaylarını işleme

>[!IMPORTANT]
> Azure günlük tümleştirme özelliği 06/15/2019 tarafından kullanım dışı bırakılacak. AzLog İndirmeleri 27 Haz 2018 ' de devre dışı bırakıldı. İleriye doğru iletme hakkında yönergeler için gönderiyi [SıEM araçlarıyla tümleştirme Için Azure Izleyicisini kullanma](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/) 

Günlüğe kaydedilen olayları almak ve güvenlik bilgileriniz ve olay yönetimi (SıEM) sisteminizde kullanılabilir hale getirmek için Azure günlük tümleştirmesini kullanabilirsiniz. Bu öğreticide, Azure günlük tümleştirmesinin Azure Event Hubs aracılığıyla elde edilen günlükleri işlemek için nasıl kullanılabileceği hakkında bir örnek gösterilmektedir.

Azure günlüklerini tümleştirmek için tercih edilen yöntem, SıEM satıcınızın Azure Izleyici bağlayıcısını kullanarak ve bu [yönergeleri](../../azure-monitor/platform/stream-monitoring-data-event-hubs.md)izleyerek kullanmaktır. Ancak, SıEM satıcınız Azure Izleyici 'ye bağlayıcı sağlamıyorsa, bu tür bir bağlayıcı kullanılabilir olana kadar Azure günlük tümleştirmesini geçici bir çözüm (Azure günlük tümleştirmesi tarafından destekleniyorsa) kullanabilirsiniz.

 
Bu öğreticiyi kullanarak Azure günlük tümleştirmesinin ve Event Hubs birlikte çalışarak örnek adımları izleyerek ve her adımın çözümü nasıl desteklediğini anlayacaksınız. Ardından, şirketinizin benzersiz gereksinimlerini desteklemek için kendi adımlarınızı oluşturmak üzere burada öğrendiklerinizi öğrenebilirsiniz.

> [!WARNING]
> Bu öğreticideki adımların ve komutların kopyalanması ve yapıştırılabilmesi amaçlanmamıştır. Bunlar yalnızca örnektir. Canlı ortamınızda "olduğu gibi" PowerShell komutlarını kullanmayın. Bunları benzersiz ortamınıza göre özelleştirmeniz gerekir.


Bu öğreticide, bir olay hub 'ına kaydedilen Azure Key Vault etkinlik alma ve onu SıEM sisteminize JSON dosyaları olarak kullanılabilir hale getirme işlemi adım adım gösterilmektedir. Ardından, SıEM sisteminizi JSON dosyalarını işleyecek şekilde yapılandırabilirsiniz.

>[!NOTE]
>Bu öğreticideki adımların çoğu, anahtar kasalarını, depolama hesaplarını ve Olay Hub 'larını yapılandırmayı içerir. Belirli Azure günlük tümleştirme adımları Bu öğreticinin sonunda bulunur. Bu adımları bir üretim ortamında gerçekleştirmeyin. Yalnızca bir laboratuvar ortamı için tasarlanmıştır. Adımları üretimde kullanmadan önce özelleştirmeniz gerekir.

Bu şekilde sunulan bilgiler, her adımın ardındaki nedenleri anlamanıza yardımcı olur. Diğer makalelere bağlantılar, belirli konularda daha fazla ayrıntı sağlar.

Bu öğreticinin bahsetme hizmetleri hakkında daha fazla bilgi için bkz.: 

- [Azure Anahtar Kasası.](/azure/key-vault/key-vault-whatis)
- [Azure Event Hubs](/azure/event-hubs/event-hubs-what-is-event-hubs)
- [Azure günlük tümleştirmesi](azure-log-integration-overview.md)


## <a name="initial-setup"></a>Başlangıç kurulumu

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Bu makaledeki adımları tamamlayabilmeniz için öncelikle şunlar gerekir:

* Bu abonelikte yönetici haklarına sahip bir Azure aboneliği ve hesabı. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)oluşturabilirsiniz.
 
* İnternet erişimi olan ve Azure günlük tümleştirmesi yükleme gereksinimlerini karşılayan bir sistem. Sistem bir bulut hizmetinde olabilir veya şirket içinde barındırılır.

* Azure günlük tümleştirmesi yüklendi. Yüklemek için:

   a. 2\. adımda bahsedilen sisteme bağlanmak için Uzak Masaüstü 'Nü kullanın.   
   b. Azure günlük tümleştirme yükleyicisini sisteme kopyalayın. c. Yükleyiciyi başlatın ve Microsoft yazılımı lisans koşulları 'nı kabul edin.

* Telemetri bilgilerini sağlayacaksınız onay kutusunu seçili bırakın. Kullanım bilgilerini Microsoft 'a göndermemeyi tercih ediyorsanız, onay kutusunun işaretini kaldırın.

   Azure günlük tümleştirmesi ve nasıl yükleneceğine ilişkin daha fazla bilgi için bkz. [Azure günlük tümleştirmesi Azure tanılama günlüğe kaydetme ve Windows olay iletme](azure-log-integration-get-started.md).

* En son PowerShell sürümü.

   Windows Server 2016 yüklüyse, en az PowerShell 5,0 vardır. Windows Server 'ın başka bir sürümünü kullanıyorsanız, PowerShell 'in önceki bir sürümü yüklü olabilir. Bir PowerShell penceresine girerek ```get-host``` sürümü kontrol edebilirsiniz. PowerShell 5,0 yüklü değilse, [bu dosyayı indirebilirsiniz](https://www.microsoft.com/download/details.aspx?id=50395).

   En az PowerShell 5,0 sürümünü aldıktan sonra, [Azure PowerShell Install](/powershell/azure/install-az-ps)' daki yönergeleri izleyerek en son sürümü yükleyebilirsiniz.


## <a name="create-supporting-infrastructure-elements"></a>Destekleyici altyapı öğeleri oluşturma

1. Yükseltilmiş bir PowerShell penceresi açın ve **C:\Program Files\Microsoft Azure günlük tümleştirmesi**' ne gidin.
1. LoadAzLogModule. ps1 betiğini çalıştırarak AzLog cmdlet 'lerini içeri aktarın. `.\LoadAzLogModule.ps1` Komutunu girin. (". \" Bu komutta.) Şuna benzer bir şey görmeniz gerekir:</br>

   ![Modül listesi yüklendi](./media/azure-log-integration-keyvault-eventhub/loaded-modules.png)

1. `Connect-AzAccount` Komutunu girin. Oturum açma penceresinde, bu öğretici için kullanacağınız aboneliğin kimlik bilgisi bilgilerini girin.

   >[!NOTE]
   >Bu makineden Azure 'da ilk kez oturum açıyorsanız, Microsoft 'un PowerShell kullanım verilerini toplamasına izin verme hakkında bir ileti görürsünüz. Azure PowerShell geliştirmek için kullanılacağından bu veri toplamayı etkinleştirmenizi öneririz.

1. Başarılı kimlik doğrulamasından sonra oturumunuz açıldı. Abonelik KIMLIĞI ve abonelik adı ' nı, daha sonraki adımları tamamlayabilmeniz için ihtiyaç duyacağınız bir yere göz atın.

1. Daha sonra kullanılacak değerleri depolamak için değişkenler oluşturun. Aşağıdaki PowerShell çizgilerinin her birini girin. Değerleri ortamınızla eşleşecek şekilde ayarlamanız gerekebilir.
    - ```$subscriptionName = 'Visual Studio Ultimate with MSDN'```(Abonelik adınız farklı olabilir. Önceki komutun çıktısının bir parçası olarak görebilirsiniz.)
    - ```$location = 'West US'```(Bu değişken, kaynakların oluşturulması gereken konumu geçirmek için kullanılacaktır. Bu değişkeni, seçinizin herhangi bir konumu olacak şekilde değiştirebilirsiniz.)
    - ```$random = Get-Random```
    - ```$name = 'azlogtest' + $random```(Ad herhangi bir şey olabilir, ancak yalnızca küçük harf ve sayı içermelidir.)
    - ```$storageName = $name```(Bu değişken, depolama hesabı adı için kullanılacaktır.)
    - ```$rgname = $name```(Bu değişken, kaynak grubu adı için kullanılacaktır.)
    - ```$eventHubNameSpaceName = $name```(Bu, Olay Hub 'ı ad alanının adıdır.)
1. Üzerinde çalıştığınız aboneliği belirtin:
    
    ```Select-AzSubscription -SubscriptionName $subscriptionName```
1. Kaynak grubu oluşturun:
    
    ```$rg = New-AzResourceGroup -Name $rgname -Location $location```
    
   Bu noktaya girdiğinizde `$rg` , bu ekran görüntüsüne benzer bir çıktı görmeniz gerekir:

   ![Kaynak Grubu oluşturulduktan sonra çıkış](./media/azure-log-integration-keyvault-eventhub/create-rg.png)
1. Durum bilgilerini izlemek için kullanılacak bir depolama hesabı oluşturun:
    
    ```$storage = New-AzStorageAccount -ResourceGroupName $rgname -Name $storagename -Location $location -SkuName Standard_LRS```
1. Olay Hub 'ı ad alanını oluşturun. Bu, bir olay hub 'ı oluşturmak için gereklidir.
    
    ```$eventHubNameSpace = New-AzEventHubNamespace -ResourceGroupName $rgname -NamespaceName $eventHubnamespaceName -Location $location```
1. Öngörüler sağlayıcısıyla birlikte kullanılacak kural KIMLIĞINI alın:
    
    ```$sbruleid = $eventHubNameSpace.Id +'/authorizationrules/RootManageSharedAccessKey'```
1. Tüm olası Azure konumlarını alın ve adları sonraki bir adımda kullanılabilecek bir değişkene ekleyin:
    
    a. ```$locationObjects = Get-AzLocation```    
    b. ```$locations = @('global') + $locationobjects.location```
    
    Bu noktaya girerseniz `$locations` , Get-azlocation tarafından döndürülen ek bilgiler olmadan konum adlarını görürsünüz.
1. Azure Resource Manager günlük profili oluşturun: 
    
    ```Add-AzLogProfile -Name $name -ServiceBusRuleId $sbruleid -Locations $locations```
    
    Azure günlük profili hakkında daha fazla bilgi için bkz. [Azure etkinlik günlüğü 'Ne genel bakış](../../azure-monitor/platform/activity-logs-overview.md).

> [!NOTE]
> Bir günlük profili oluşturmaya çalıştığınızda bir hata iletisi alabilirsiniz. Daha sonra Get-AzLogProfile ve Remove-AzLogProfile belgelerini gözden geçirebilirsiniz. Get-AzLogProfile ' i çalıştırırsanız, günlük profiliyle ilgili bilgileri görürsünüz. Mevcut günlük profilini ```Remove-AzLogProfile -name 'Log Profile Name'``` komutu girerek silebilirsiniz.
>
>![Kaynak Yöneticisi profili hatası](./media/azure-log-integration-keyvault-eventhub/rm-profile-error.png)

## <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma

1. Anahtar kasasını oluşturun:

   ```$kv = New-AzKeyVault -VaultName $name -ResourceGroupName $rgname -Location $location```

1. Anahtar Kasası için günlüğü yapılandırın:

   ```Set-AzDiagnosticSetting -ResourceId $kv.ResourceId -ServiceBusRuleId $sbruleid -Enabled $true```

## <a name="generate-log-activity"></a>Günlük etkinliği oluştur

Günlük etkinliği oluşturmak için Key Vault gönderilen isteklerin gönderilmesi gerekir. Anahtar oluşturma, gizli dizileri depolama veya Key Vault gizli dizileri okuma gibi eylemler günlük girdileri oluşturacaktır.

1. Geçerli depolama anahtarlarını görüntüle:
    
   ```Get-AzStorageAccountKey -Name $storagename -ResourceGroupName $rgname  | ft -a```
1. Yeni bir **key2**oluşturun:
    
   ```New-AzStorageAccountKey -Name $storagename -ResourceGroupName $rgname -KeyName key2```
1. Anahtarları yeniden görüntüleyin ve **key2** 'in farklı bir değer bulundurduğunu görün:
    
   ```Get-AzStorageAccountKey -Name $storagename -ResourceGroupName $rgname  | ft -a```
1. Ek günlük girişleri oluşturmak için bir gizli dizi ayarlama ve okuma:
    
   a. ```Set-AzKeyVaultSecret -VaultName $name -Name TestSecret -SecretValue (ConvertTo-SecureString -String 'Hi There!' -AsPlainText -Force)``` b. ```(Get-AzKeyVaultSecret -VaultName $name -Name TestSecret).SecretValueText```

   ![Parola döndürüldü](./media/azure-log-integration-keyvault-eventhub/keyvaultsecret.png)


## <a name="configure-azure-log-integration"></a>Azure günlük tümleştirmesini yapılandırma

Bir olay hub 'ında Key Vault günlüğe kaydetmek için gerekli tüm öğeleri yapılandırdığınıza göre, Azure günlük tümleştirmesini yapılandırmanız gerekir:

1. ```$storage = Get-AzStorageAccount -ResourceGroupName $rgname -Name $storagename```
1. ```$eventHubKey = Get-AzEventHubNamespaceKey -ResourceGroupName $rgname -NamespaceName $eventHubNamespace.name -AuthorizationRuleName RootManageSharedAccessKey```
1. ```$storagekeys = Get-AzStorageAccountKey -ResourceGroupName $rgname -Name $storagename```
1. ```$storagekey = $storagekeys[0].Value```

Her olay hub 'ı için AzLog komutunu çalıştırın:

1. ```$eventhubs = Get-AzEventHub -ResourceGroupName $rgname -NamespaceName $eventHubNamespaceName```
1. ```$eventhubs.Name | %{Add-AzLogEventSource -Name $sub' - '$_ -StorageAccount $storage.StorageAccountName -StorageKey $storageKey -EventHubConnectionString $eventHubKey.PrimaryConnectionString -EventHubName $_}```

Son iki komutu çalıştırmanın bir dakikası veya daha sonra, oluşturulan JSON dosyalarını görmeniz gerekir. **C:\users\azlog\eventhubjson**dizinini izleyerek bunu doğrulayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure günlük tümleştirmesi hakkında SSS](azure-log-integration-faq.md)
- [Azure günlük tümleştirmesi ile çalışmaya başlama](azure-log-integration-get-started.md)
- [Günlükleri Azure kaynaklarından SıEM sistemlerinizle tümleştirin](azure-log-integration-overview.md)
