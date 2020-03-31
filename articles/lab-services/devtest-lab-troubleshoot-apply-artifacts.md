---
title: Azure DevTest Labs'daki yapıtlarla ilgili sorunları giderme | Microsoft Dokümanlar
description: Azure DevTest Labs sanal makinesinde yapı talimi uygularken oluşan sorunları nasıl gideracağınızı öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75456981"
---
# <a name="troubleshoot-issues-when-applying-artifacts-in-an-azure-devtest-labs-virtual-machine"></a>Azure DevTest Labs sanal makinesinde yapıtları uygularken sorunları giderme sorunları
Sanal bir makinede eserler uygulamak çeşitli nedenlerle başarısız olabilir. Bu makalede, olası nedenleri belirlemeye yardımcı olmak için bazı yöntemler size rehberlik eder.

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Yığın Taşma forumlarında](https://azure.microsoft.com/support/forums/)Azure Engel Laboratuvarları (DTL) uzmanlarıyla iletişime geçebilirsiniz. Alternatif olarak, bir Azure destek olayı dosyalayabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve Destek Al'ı seçin.   

> [!NOTE]
> Bu makale, hem Windows hem de Windows olmayan sanal makineler için geçerlidir. Bazı farklılıklar olsa da, bu makalede açıkça çağrılacaktır.

## <a name="quick-troubleshooting-steps"></a>Hızlı sorun giderme adımları
VM'nin çalışır durumda olup olmadığını kontrol edin. DevTest Labs, VM'nin çalışmasını ve [Microsoft Azure Sanal Makine Aracısı'nın (VM Agent)](../virtual-machines/extensions/agent-windows.md) yüklü ve hazır olmasını gerektirir.

> [!TIP]
> Azure **portalında,** VM'nin yapıtları uygulamaya hazır olup olmadığını görmek için VM'nin **yapılarını yönet** sayfasına gidin. Bu sayfanın en üstünde bir ileti görüyorsunuz. 
> 
> **Azure PowerShell'i**kullanarak, yalnızca GET işlemini genişletdiğinizde döndürülen **canApplyArts**bayrağını inceleyin. Aşağıdaki örnek komutuna bakın:

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

## <a name="ways-to-troubleshoot"></a>Sorun giderme yolları 
DevTest Labs ve Kaynak Yöneticisi dağıtım modeli kullanılarak oluşturulan VM'leri aşağıdaki yöntemlerden birini kullanarak sorun giderebilirsiniz:

- **Azure portalı** - soruna neyin neden olabileceğine dair görsel bir ipucunu hızlı bir şekilde almanız gerekiyorsa harika.
- **Azure PowerShell** - PowerShell komut istemiyle rahatsanız, Azure PowerShell cmdlets'i kullanarak DevTest Labs kaynaklarını hızlı bir şekilde sorgulayın.

> [!TIP]
> Bir VM içinde yapı yürütme gözden geçirmek için nasıl daha fazla bilgi için, [laboratuarda yapı hataları tanıla](devtest-lab-troubleshoot-artifact-failure.md)bakın.

## <a name="symptoms-causes-and-potential-resolutions"></a>Belirtiler, nedenler ve olası çözümler 

### <a name="artifact-appears-to-hang"></a>Artefakt askıda görünüyor   
Bir yapı, önceden tanımlanmış bir zaman aşımı süresi sona erene kadar askıda görünüyor ve yapı **Başarısız**olarak işaretlenir.

Bir obje asılmış gibi göründüğünde, önce nerede sıkıştığını belirleyin. Bir yapı yürütme sırasında aşağıdaki adımlardan herhangi birinde engellenebilir:

- **İlk istek sırasında**. DevTest Labs, Özel Komut Dosyası Uzantısı'nın (CSE) kullanımını istemek için bir Azure Kaynak Yöneticisi şablonu oluşturur. Bu nedenle, arka planda, bir kaynak grubu dağıtımı tetiklenir. Bu düzeyde bir hata olduğunda, söz konusu VM için kaynak grubunun **Etkinlik Günlükleri** ayrıntıları alırsınız.  
    - Etkinlik günlüğüne laboratuvar VM sayfa gezinti çubuğundan erişebilirsiniz. Bunu seçtiğinizde, **ya sanal makineye yapıtuygulamak** için bir giriş görürsünüz (uygulamalı yapılar işlemi doğrudan tetiklendiyse) veya **sanal makineleri ekleme veya değiştirme** (uygulanan yapılar işlemi VM oluşturma işleminin bir parçasıysa).
    - Bu girişlerin altındaki hataları arayın. Bazen hata buna göre etiketlendirilmez ve her girişi araştırmanız gerekir.
    - Her girişin ayrıntılarını araştırırken, JSON yükünün içeriğini gözden geçirdiğinizden emin olun. Bu belgenin alt kısmında bir hata görebilirsiniz.
- **Yapıyı yürütmeye çalışırken.** Ağ veya depolama sorunları nedeniyle olabilir. Ayrıntılar için bu makalenin ilerleyen bölümlerinde ilgili bölüme bakın. Ayrıca, komut dosyasının yazarlık biçimi nedeniyle de olabilir. Örnek:
    - PowerShell komut dosyasının **zorunlu parametreleri**vardır, ancak kullanıcının boş bırakmasına izin verdiğiniz veya artifactfile.json tanım dosyasındaki özellik için varsayılan bir değere sahip olmadığınız için bir değer geçirilemez. Komut dosyası askıda kalır, çünkü kullanıcı girişi bekliyor.
    - PowerShell komut dosyası yürütmenin bir parçası olarak **kullanıcı girişi gerektirir.** Komut dosyaları herhangi bir kullanıcı müdahalesi gerektirmeden sessizce çalışmak için yazılmalıdır.
- **VM Agent hazır olması uzun sürer.** VM ilk başlatıldığında veya özel komut dosyası uzantısı, yapıtları uygulama isteğini sunmak için ilk yüklendiğinde, VM VM Aracısını yükseltmeyi veya VM Aracısının başlatılmasını bekleyebilir. VM Aracısı'nın bağlı olduğu ve başlatılması uzun zaman alan hizmetler olabilir. Bu gibi durumlarda, daha fazla sorun giderme için [Azure Sanal Makine Aracısı genel görünümüne](../virtual-machines/extensions/agent-windows.md) bakın.

### <a name="to-verify-if-the-artifact-appears-to-hang-because-of-the-script"></a>Yapının komut dosyası nedeniyle askıda görünip görünmediği doğrulamak için

1. Söz konusu sanal makineye giriş yapın.
2. Komut dosyasını sanal makinede yerel olarak kopyalayın `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\<version>`veya altındaki sanal makinede bulun. Eser komut dosyalarının indirildiği yer.
3. Yükseltilmiş bir komut istemi kullanarak, soruna neden olmak için kullanılan aynı parametre değerlerini sağlayarak komut dosyasını yerel olarak çalıştırın.
4. Komut dosyasının istenmeyen davranışlardan muzdarip olup olmadığını belirleyin. Bu nedenle, yapı için bir güncelleştirme isteyin (genel repo'dan geliyorsa); veya düzeltmeleri kendiniz yapın (özel repo'nuzdan ise).

> [!TIP]
> [Genel repo'muzda](https://github.com/Azure/azure-devtestlab) barındırılan eserlerle ilgili sorunları düzeltebilir ve değişiklikleri incelememiz ve onayımız için gönderebilirsiniz. [README.md](https://github.com/Azure/azure-devtestlab/blob/master/Artifacts/README.md) belgedeki **Katkılar** bölümüne bakın.
> 
> Kendi yapılarınızı yazma hakkında bilgi için [belgeAUTHORING.md](https://github.com/Azure/azure-devtestlab/blob/master/Artifacts/AUTHORING.md) bakın.

### <a name="to-verify-if-the-artifact-appears-to-hang-because-of-the-vm-agent"></a>El yapının VM Aracısı yüzünden askıda görünip takılmadığı doğrulamak için:
1. Söz konusu sanal makineye giriş yapın.
2. Dosya Gezgini'ni kullanarak **C:\WindowsAzure\günlüklerine**gidin.
3. Bul ve dosya **WaAppAgent.log**açın.
4. VM Aracısı'nın ne zaman başladığını ve başlatmayı ne zaman bitirdiğini gösteren girişleri arayın (diğer bir süre önce ilk sinyal gönderilir). Yeni girişleri veya özellikle sorunu yaşadığınız zaman dilimindekileri tercih edin.

    ```
    [00000006] [11/14/2019 05:52:13.44] [INFO]  WindowsAzureGuestAgent starting. Version 2.7.41491.949
    ...
    [00000006] [11/14/2019 05:52:31.77] [WARN]  Waiting for OOBE to Complete ...
    ...
    [00000006] [11/14/2019 06:02:30.43] [WARN]  Waiting for OOBE to Complete ...
    [00000006] [11/14/2019 06:02:33.43] [INFO]  StateExecutor initialization completed.
    [00000020] [11/14/2019 06:02:33.43] [HEART] WindowsAzureGuestAgent Heartbeat.
    ```
    Bu örnekte, vm aracısı başlangıç zamanının 10 dakika 20 saniye sürdüğünü görebilirsiniz, çünkü bir sinyal gönderildi. Bu durumda nedeni BAŞLATMAK için uzun bir zaman alan OOBE hizmeti oldu.

> [!TIP]
> Azure uzantıları hakkında genel bilgi için [Azure sanal makine uzantıları ve özelliklerine](../virtual-machines/extensions/overview.md)bakın.

## <a name="storage-errors"></a>Depolama hataları
DevTest Labs, yapıları önbelleğe almak için oluşturulan laboratuvarın depolama hesabına erişim gerektirir. DevTest Labs bir yapı yı uyguladığında, yapı yapılandırmasını ve dosyalarını yapılandırılan depolardan okur. Varsayılan olarak, DevTest Labs **ortak yapı düzeltme repo'ya**erişimi yapılandırır.

VM'nin nasıl yapılandırıldığına bağlı olarak, bu repo'ya doğrudan erişimi olmayabilir. Bu nedenle, tasarım gereği, DevTest Labs, laboratuvar ilk kez başlatılmasında oluşturulan bir depolama hesabında ki yapıları önbelleğe alar.

Bu depolama hesabına erişim herhangi bir şekilde engellenirse, vm'den Azure Depolama hizmetine trafik engellendiğinde, aşağıdakilere benzer bir hata görebilirsiniz:

```shell
CSE Error: Failed to download all specified files. Exiting. Exception: Microsoft.WindowsAzure.Storage.StorageException: The remote server returned an error: (403) Forbidden. ---> System.Net.WebException: The remote server returned an error: (403) Forbidden.
```

Yukarıdaki hata, Yapı denetimleri **Artifact results** **sayfasındaki Dağıtım İletisi** bölümünde **görünür.** Ayrıca, söz konusu sanal makinenin kaynak grubu altında **Etkinlik Günlükleri** görünür.

### <a name="to-ensure-communication-to-the-azure-storage-service-isnt-being-blocked"></a>Azure Depolama hizmetiyle iletişimin engellenmediğinden emin olmak için:

- **Eklenen ağ güvenlik grupları (NSG) için denetleyin.** NSG'lerin tüm sanal ağlarda otomatik olarak yapılandırıldığı bir abonelik ilkesi eklenmiş olabilir. Ayrıca, kullanılırsa laboratuvarın varsayılan sanal ağını veya laboratuarınızda VM'lerin oluşturulması için kullanılan diğer sanal ağı da etkiler.
- **Varsayılan laboratuvarın depolama hesabını kontrol edin** (diğer bir tarihte, laboratuvar oluşturulduğunda oluşturulan, adı genellikle "a" harfiyle başlayan ve\<çok\>basamaklı bir sayı yla biten ilk depolama hesabı #).
    1. Laboratuvarın kaynak grubuna gidin.
    2. Adı sözleşmeyle eşleşen tür **depolama hesabının**kaynağını bulun.
    3. **Güvenlik Duvarları ve sanal ağlar**adı verilen depolama hesabı sayfasına gidin.
    4. **Tüm ağlara**ayarlandığından emin olun. **Seçili ağlar** seçeneği seçilirse, laboratuvarda VM oluşturmak için kullanılan sanal ağların listeye eklenmiştir.

Daha ayrıntılı sorun giderme için Azure [Depolama güvenlik duvarlarını ve sanal ağları Yapılandırma'ya](../storage/common/storage-network-security.md)bakın.

> [!TIP]
> **Ağ güvenlik grubu kurallarını doğrulayın.** Ağ güvenlik grubundaki bir kuralın sanal makineye veya sanal makineden gelen trafiği engellediğini doğrulamak için [IP akışı doğrulamayı](../network-watcher/diagnose-vm-network-traffic-filtering-problem.md#use-ip-flow-verify) kullanın. Gelen **NSG** kuralının var olduğundan emin olmak için etkili güvenlik grubu kurallarını da gözden geçirebilirsiniz. Daha fazla bilgi için [vm trafik akışını gidermek için etkili güvenlik kurallarını kullanma'ya](../virtual-network/diagnose-network-traffic-filter-problem.md)bakın.

## <a name="other-sources-of-error"></a>Diğer hata kaynakları
Daha az sıklıkta hata olası kaynakları vardır. Servis talebinize uygun olup olmadığını görmek için her birinin değerlendirmesinden emin olun. İşte bunlardan biri: 

- **Özel repo için süresi dolmuş kişisel erişim belirteci.** Süresi dolduğunda, yapı listelenmez ve süresi dolmuş özel erişim belirteci olan bir depodaki yapılara atıfta bulunan tüm komut dosyaları buna göre başarısız olur.

## <a name="next-steps"></a>Sonraki adımlar
Bu hataların hiçbiri oluştuysa ve hala yapıtları uygulayamıyorsanız, bir Azure destek olayı dosyalayabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve Destek **Al'ı**seçin.

