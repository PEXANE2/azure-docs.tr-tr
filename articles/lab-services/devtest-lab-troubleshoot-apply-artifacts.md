---
title: Azure DevTest Labs yapıtlarla ilgili sorunları giderme | Microsoft Docs
description: Azure DevTest Labs bir sanal makinede yapıtlar uygulanırken oluşan sorunları nasıl giderebileceğinizi öğrenin.
services: devtest-lab
documentationcenter: na
author: spelluru
editor: ''
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/03/2019
ms.author: spelluru
ms.openlocfilehash: fc5051667100a2ebaa01b7815f825fadd766b08f
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75456981"
---
# <a name="troubleshoot-issues-when-applying-artifacts-in-an-azure-devtest-labs-virtual-machine"></a>Azure DevTest Labs sanal makinesine yapıtlar uygulanırken sorunları giderme
Yapıları bir sanal makineye uygulamak çeşitli nedenlerle başarısız olabilir. Bu makale, olası nedenleri belirlemenize yardımcı olmak için bazı yöntemlerle size rehberlik eder.

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Stack Overflow forumlarında](https://azure.microsoft.com/support/forums/)Azure DevTest Labs (DTL) uzmanlarıyla iletişim kurun. Alternatif olarak, bir Azure destek olayına dosya. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve Destek Al ' ı seçin.   

> [!NOTE]
> Bu makale hem Windows hem de Windows dışı sanal makineler için geçerlidir. Bazı farklılıklar olsa da bu makalede açıkça çağrılacaktır.

## <a name="quick-troubleshooting-steps"></a>Hızlı sorun giderme adımları
VM 'nin çalıştığından emin olun. DevTest Labs, VM 'nin çalıştırılmasını ve [Microsoft Azure sanal makine aracısının (VM Aracısı)](../virtual-machines/extensions/agent-windows.md) yüklü ve kullanılabilir olmasını gerektirir.

> [!TIP]
> **Azure Portal**, VM 'nin yapıtları uygulamaya hazırlanma bölümüne bakmak için VM 'nin **yapıtları Yönet** sayfasına gidin. Bu sayfanın en üstünde bir ileti görürsünüz. 
> 
> **Azure PowerShell**kullanarak, yalnızca bir get işleminde genişlettikten sonra döndürülen **Canapplyartıtei**bayrağını inceleyin. Aşağıdaki örnek komutuna bakın:

```powershell
Select-AzSubscription -SubscriptionId $SubscriptionId | Out-Null
$vm = Get-AzResource `
        -Name "$LabName/$VmName" `
        -ResourceGroupName $LabRgName `
        -ResourceType 'microsoft.devtestlab/labs/virtualmachines' `
        -ApiVersion '2018-10-15-preview' `
        -ODataQuery '$expand=Properties($expand=ComputeVm)'
$vm.Properties.canApplyArtifacts
```

## <a name="ways-to-troubleshoot"></a>Sorun gidermeye yönelik yollar 
Aşağıdaki yöntemlerden birini kullanarak DevTest Labs ve Kaynak Yöneticisi dağıtım modeli kullanılarak oluşturulan sanal makinelerin sorunlarını giderebilirsiniz:

- **Azure Portal** , soruna neden olabilecek bir görsel ipucu hızla almanız gerekiyorsa harika.
- **Azure PowerShell** -bir PowerShell istemiyle rahat bir şekilde çalışıyorsanız, Azure PowerShell cmdlet 'Lerini kullanarak DevTest Labs kaynaklarını hızlıca sorgulayın.

> [!TIP]
> Bir VM içinde yapıt yürütmeyi İnceleme hakkında daha fazla bilgi için bkz. [laboratuvardaki yapıt başarısızlıklarını tanılama](devtest-lab-troubleshoot-artifact-failure.md).

## <a name="symptoms-causes-and-potential-resolutions"></a>Belirtiler, nedenler ve olası çözümler 

### <a name="artifact-appears-to-hang"></a>Yapıt askıda görünüyor   
Önceden tanımlanmış bir zaman aşımı süresi dolana kadar bir yapıt askıda kalır ve yapıt **başarısız**olarak işaretlenir.

Bir yapıtı asılı göründüğünde ilk olarak takılmış olduğunu saptayın. Yürütme sırasında aşağıdaki adımlardan birinde bir yapıt engellenebilir:

- **İlk Istek sırasında**. DevTest Labs özel betik uzantısının (CSE) kullanımını istemek için bir Azure Resource Manager şablonu oluşturur. Bu nedenle, arka planda bir kaynak grubu dağıtımı tetiklenir. Bu düzeydeki bir hata oluştuğunda, söz konusu VM için kaynak grubunun **etkinlik günlüklerinde** ayrıntılara ulaşabilirsiniz.  
    - Etkinlik günlüğüne laboratuvar VM sayfası gezinti çubuğundan erişebilirsiniz. Bunu seçtiğinizde, **sanal makineye yapıtları uygulama** (yapıtları Uygula işlemi doğrudan tetikleolduysa) veya **sanal makineler ekleme veya değiştirme** (yapıtlar işlemi VM oluşturma işleminin bir parçasıysa) için bir giriş görürsünüz.
    - Bu girişlerin altındaki hataları arayın. Bazen hata buna uygun şekilde etiketlenmez ve her bir girdiyi araştırmanız gerekir.
    - Her girişin ayrıntılarını incelerken, JSON yükünün içeriğini gözden geçirdiğinizden emin olun. Bu belgenin altında bir hata görebilirsiniz.
- **Yapıt yürütülmeye çalışılırken**. Bu, ağ veya depolama sorunlarından dolayı olabilir. Ayrıntılar için bu makalenin devamındaki ilgili bölüme bakın. Ayrıca, betiğin yazıldığı gibi bir durum da oluşabilir. Örneğin:
    - Bir PowerShell betiği **zorunlu parametrelere**sahiptir, ancak kullanıcının boş bırakmasını sağladığından veya artifactfile. JSON tanım dosyasında özellik için varsayılan bir değer olmadığından, bu değere bir değer geçirilemez. Komut dosyası, Kullanıcı girişi beklediği için askıda kalacak.
    - Bir PowerShell betiği, yürütmenin parçası olarak **Kullanıcı girişi gerektirir** . Betiklerin, herhangi bir kullanıcı müdahalesi gerektirmeden sessizce çalışacak şekilde yazılması gerekir.
- **VM aracısının hazırlanmaya uzun sürme**. VM ilk başlatıldığında veya özel Betik uzantısı ilk kez, yapıtları uygulamak için isteği sunacak şekilde yüklendiğinde, VM, VM Aracısı 'nı yükseltmeyi veya VM aracısının başlamasını beklemek isteyebilir. VM aracısının başlatılması uzun süren bir hizmet olabilir. Bu gibi durumlarda, daha fazla sorun giderme için bkz. [Azure sanal makine aracısına genel bakış](../virtual-machines/extensions/agent-windows.md) .

### <a name="to-verify-if-the-artifact-appears-to-hang-because-of-the-script"></a>Betik nedeniyle yapıtın askıda görünüp göründüğünü doğrulamak için

1. Söz konusu sanal makinede oturum açın.
2. Betiği sanal makinede yerel olarak kopyalayın veya `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\<version>`altında sanal makinede bulun. Bu, yapıt betiklerinin indirileceği konumdur.
3. Yükseltilmiş bir komut istemi kullanarak, soruna neden olmak için aynı parametre değerlerini sağlayarak betiği yerel olarak yürütün.
4. Betiğin istenmeyen davranışların olup olmadığını belirleme. Varsa, yapıt için bir güncelleştirme isteyin (ortak depodan ise); ya da düzeltmeleri kendiniz yapın (özel deponuz varsa).

> [!TIP]
> [Genel](https://github.com/Azure/azure-devtestlab) depolarımızda barındırılan yapıtlarla ilgili sorunları düzeltebilir ve gözden geçirdiğimiz ve onayımız için değişiklikleri gönderebilirsiniz. [README.MD](https://github.com/Azure/azure-devtestlab/blob/master/Artifacts/README.md) belgesindeki **katkıları** bölümüne bakın.
> 
> Kendi yapılarınızı yazma hakkında daha fazla bilgi için bkz. [AUTHORING.MD](https://github.com/Azure/azure-devtestlab/blob/master/Artifacts/AUTHORING.md) Document.

### <a name="to-verify-if-the-artifact-appears-to-hang-because-of-the-vm-agent"></a>VM Aracısı nedeniyle yapıtın askıda görünüp göründüğünü doğrulamak için:
1. Söz konusu sanal makinede oturum açın.
2. Dosya Gezgini 'ni kullanarak **C:\windowsazure\logs dizinine**gidin.
3. **Waappagent. log**dosyasını bulun ve açın.
4. VM aracısının başladığı zaman ve başlatmayı bitirmediği zaman (yani, ilk sinyal gönderildiğinde) gösteren girişleri arayın. Daha yeni girişler veya özellikle sorunu yaşayabileceğiniz zaman dilimiyle ilgili olanları tercih edin.

    ```
    [00000006] [11/14/2019 05:52:13.44] [INFO]  WindowsAzureGuestAgent starting. Version 2.7.41491.949
    ...
    [00000006] [11/14/2019 05:52:31.77] [WARN]  Waiting for OOBE to Complete ...
    ...
    [00000006] [11/14/2019 06:02:30.43] [WARN]  Waiting for OOBE to Complete ...
    [00000006] [11/14/2019 06:02:33.43] [INFO]  StateExecutor initialization completed.
    [00000020] [11/14/2019 06:02:33.43] [HEART] WindowsAzureGuestAgent Heartbeat.
    ```
    Bu örnekte, bir sinyal gönderildiği için VM Aracısı başlangıç zamanının 10 dakika ve 20 saniye sürdüğünü görebilirsiniz. Bu durumda, OOBE hizmetinin başlaması uzun sürüyor.

> [!TIP]
> Azure uzantıları hakkında genel bilgi için bkz. [Azure sanal makine uzantıları ve özellikleri](../virtual-machines/extensions/overview.md).

## <a name="storage-errors"></a>Depolama hataları
DevTest Labs, bir laboratuvarın önbellek yapıtları için oluşturulan depolama hesabına erişmesi gerekir. DevTest Labs bir yapıt uygularsa, yapı yapılandırmasını ve dosyalarını yapılandırılan depolardan okur. Varsayılan olarak, DevTest Labs **ortak yapıt**deposuna erişimi yapılandırır.

VM 'nin nasıl yapılandırıldığına bağlı olarak, bu depoya doğrudan erişimi olmayabilir. Bu nedenle, tasarım, DevTest Labs, yapıtları laboratuvar ilk başlatıldığında oluşturulan bir depolama hesabında önbelleğe alır.

Bu depolama hesabına erişim herhangi bir şekilde engellenirse, trafiğin VM 'den Azure Storage hizmetine engellenmesi gibi, aşağıdakine benzer bir hata görebilirsiniz:

```shell
CSE Error: Failed to download all specified files. Exiting. Exception: Microsoft.WindowsAzure.Storage.StorageException: The remote server returned an error: (403) Forbidden. ---> System.Net.WebException: The remote server returned an error: (403) Forbidden.
```

Yukarıdaki hata, **yapıtları Yönet**altındaki **yapıt sonuçları** sayfasının **dağıtım iletisi** bölümünde görüntülenir. Ayrıca, söz konusu sanal makinenin kaynak grubu altındaki **etkinlik günlüklerinde** de görüntülenir.

### <a name="to-ensure-communication-to-the-azure-storage-service-isnt-being-blocked"></a>Azure depolama hizmeti ile iletişimin engellenmediğinden emin olmak için:

- **Eklenen ağ güvenlik grupları (NSG) olup olmadığını denetleyin**. NSG 'lerin tüm sanal ağlarda otomatik olarak yapılandırıldığı bir abonelik ilkesi eklenmiş olabilir. Ayrıca, sanal makinelerin oluşturulması için kullanılan laboratuvar veya laboratuvarınızda yapılandırılmış başka bir sanal ağ olan varsayılan sanal ağı da etkiler.
- **Varsayılan laboratuvarın depolama hesabını** (diğer bir deyişle, laboratuvar oluşturulduğunda oluşturulan ilk depolama hesabı, adı genellikle "a" harfiyle başlar ve\<labname\>#) ile biten çok basamaklı bir sayıyla biter.
    1. Laboratuvarın kaynak grubuna gidin.
    2. Adı kuralıyla eşleşen **depolama hesabı**türü kaynağını bulun.
    3. **Güvenlik duvarları ve sanal ağlar**adlı depolama hesabı sayfasına gidin.
    4. **Tüm ağlara**ayarlandığından emin olun. **Seçili ağlar** seçeneği işaretliyse, sanal makineleri oluşturmak için kullanılan laboratuvarın sanal ağlarının listeye eklendiğinden emin olun.

Daha ayrıntılı sorun giderme için bkz. [Azure Storage güvenlik duvarlarını ve sanal ağları yapılandırma](../storage/common/storage-network-security.md).

> [!TIP]
> **Ağ güvenlik grubu kurallarını doğrulayın**. Bir ağ güvenlik grubundaki bir kuralın bir sanal makineye giden trafiği engellediğini onaylamak için [IP akışı doğrulama](../network-watcher/diagnose-vm-network-traffic-filtering-problem.md#use-ip-flow-verify) kullanın. Ayrıca, gelen **Izin verme** NSG kuralının mevcut olduğundan emin olmak için geçerli güvenlik grubu kurallarını gözden geçirebilirsiniz. Daha fazla bilgi için bkz. [sanal makine trafiği akışı sorunlarını gidermek için etkin güvenlik kurallarını kullanma](../virtual-network/diagnose-network-traffic-filter-problem.md).

## <a name="other-sources-of-error"></a>Diğer hata kaynakları
Daha az sık olası hata kaynakları vardır. Servis talebi için geçerli olup olmadığını görmek üzere her bir değerlendirme yaptığınızdan emin olun. Bunlardan biri aşağıdadır: 

- **Özel depo Için zaman aşımına uğradı kişisel erişim belirteci**. Süre dolduğunda, yapıt listelenmez ve zaman aşımına uğradı bir özel erişim belirtecine sahip bir depodan yapıtları ifade eden betikler buna göre başarısız olur.

## <a name="next-steps"></a>Sonraki adımlar
Bu hatalardan hiçbiri oluşmazsa ve yapıtları hala uygulayadıysanız, bir Azure destek olayı da oluşturabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve **Destek Al**' ı seçin.

