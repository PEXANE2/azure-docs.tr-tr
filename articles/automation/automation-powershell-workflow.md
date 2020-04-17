---
title: Azure Otomasyonu için PowerShell İş Akışını Öğrenme
description: Bu makale, PowerShell ve PowerShell İş Akışı arasındaki belirli farkları ve Otomasyon runbook'ları için geçerli olan kavramları anlamak için PowerShell'e aşina yazarlar için hızlı bir ders olarak tasarlanmıştır.
services: automation
ms.subservice: process-automation
ms.date: 12/14/2018
ms.topic: conceptual
ms.openlocfilehash: 1b275239c19584bc11472711a32972aa3ebea1ab
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81457544"
---
# <a name="learning-key-windows-powershell-workflow-concepts-for-automation-runbooks"></a>Otomasyon runbook'ları için temel Windows PowerShell İş Akışı kavramlarını öğrenme

Azure Otomasyonundaki runbook'lar Windows PowerShell İş Akışları olarak uygulanır.  Windows PowerShell İş Akışı, Windows PowerShell komut dosyasına benzer, ancak yeni bir kullanıcı için kafa karıştırıcı olabilecek bazı önemli farklılıklara sahiptir.  Bu makalede, PowerShell iş akışını kullanarak runbook yazmanıza yardımcı olmak için tasarlanmış olsa da, denetim noktalarına ihtiyacınız olmadığı sürece PowerShell kullanarak runbook yazmanızı öneririz.  PowerShell İş Akışı runbook'ları yazarken çeşitli sözdizimi farklılıkları vardır ve bu farklar etkili iş akışları yazmak için biraz daha fazla çalışma gerektirir.

İş akışı uzun süre çalışan görevleri gerçekleştiren veya birden çok aygıttaki veya yönetilen düğümlerdeki birden çok adımın koordinasyonu için gereken programlanmış, bağlantılı adımlardan oluşan bir dizidir. İş akışının normal betiğe göre avantajları bir eylemi birden çok aygıtta aynı anda gerçekleştirebilme ve hatadan otomatik olarak kurtarma yetenekleridir. Windows PowerShell İş Akışı, Windows İş Akışı Temeli'ni kullanan bir Windows PowerShell komut dosyasıdır. İş akışı Windows PowerShell sözdizimi kullanılarak yazılsa ve Windows PowerShell tarafından başlatılsa da, Windows Workflow Foundation tarafından işlenir.

Bu makaledeki konular la ilgili tüm ayrıntılar için Windows [PowerShell İş Akışıyla Başlarken'e](https://technet.microsoft.com/library/jj134242.aspx)bakın.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma Runbook Worker'ınızdaki Az modül yükleme yönergeleri için Azure [PowerShell Modül'üne](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)bakın. Otomasyon hesabınız için, Azure Otomasyonu'nda Azure [PowerShell modüllerini nasıl güncelleştirebileceğinizi](automation-update-azure-modules.md)kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="basic-structure-of-a-workflow"></a>İş akışının temel yapısı

PowerShell komut dosyasını PowerShell iş akışına dönüştürmenin ilk adımı, komut dosyasını `Workflow` anahtar kelimeyle çevrelemektir.  İş `Workflow` akışı, parantez içinde bulunan komut dosyasının gövdesitarafından izlenen anahtar sözcük ile başlar. İş akışının adı, `Workflow` aşağıdaki sözdiziminde gösterildiği gibi anahtar kelimeyi izler:

```powershell
Workflow Test-Workflow
{
    <Commands>
}
```

İş akışının adı Otomasyon runbook'un adı ile eşleşmelidir. Runbook içe aktarılıyorsa, dosya adı iş akışı adı yla eşleşmeli ve *.ps1*ile bitmelidir.

İş akışına parametreler eklemek için, anahtar kelimeyi `Param` tıpkı bir komut dosyasında olduğu gibi kullanın.

## <a name="code-changes"></a>Kod değişiklikleri

PowerShell iş akışı kodu, birkaç önemli değişiklik dışında PowerShell komut dosyası koduyla hemen hemen aynı görünür.  Aşağıdaki bölümlerde, bir iş akışında çalışması için PowerShell komut dosyasında yapmanız gereken değişiklikler açıklayınız.

### <a name="activities"></a>Etkinlikler

Etkinlik bir iş akışındaki belirli bir görevdir. Betiğin bir veya daha çok komuttan oluşmasına benzer şekilde, iş akışı da belirli bir sırayla yürütülen bir veya daha çok etkinlikten oluşur. Windows PowerShell Workflow birçok Windows PowerShell cmdlet'ini bir iş akışı içinde çalıştıklarında otomatik olarak etkinliklere dönüştürür. Runbook'unuzda bu cmdlets'lerden birini belirttiğinizde, ilgili etkinlik Windows İş Akışı Temeli tarafından çalıştırılır. Karşılık gelen bir etkinliği olmayan cmdlet'ler için, Windows PowerShell İş Akışı cmdlet'i bir [InlineScript](#inlinescript) etkinliğinde otomatik olarak çalıştırır. Siz özel olarak bir InlineScript bloğunda eklemedikçe hariç tutulan ve bir iş akışında kullanılamayan cmdlet'ler kümesi vardır. Bu kavramlarla ilgili daha fazla bilgi için bkz. [Betik İş Akışlarında Etkinlikleri Kullanma](https://technet.microsoft.com/library/jj574194.aspx).

İş akışı etkinlikleri çalışmalarını yapılandıran ortak parametreler kümesini paylaşır. Ortak iş akışı parametreleriyle ilgili daha ayrıntılı bilgi için bkz. [about_WorkflowCommonParameters](https://technet.microsoft.com/library/jj129719.aspx).

### <a name="positional-parameters"></a>Konumsal parametreler

İş akışında etkinlikler ve cmdletlerle konumsal parametreleri kullanamazsınız.  Tüm bu parametre adları kullanmanız gerektiği anlamına gelir.

Örneğin, tüm çalışan hizmetleri alan aşağıdaki kodu göz önünde bulundurun.

```azurepowershell-interactive
Get-Service | Where-Object {$_.Status -eq "Running"}
```

Aynı kodu bir iş akışında çalıştırmayı denerseniz, "Parametre kümesi belirtilen adlandırılmış parametreler kullanılarak çözülemez" gibi bir ileti alırsınız.  Bunu düzeltmek için parametre adını aşağıdaki gibi sağlayın.

```powershell
Workflow Get-RunningServices
{
    Get-Service | Where-Object -FilterScript {$_.Status -eq "Running"}
}
```

### <a name="deserialized-objects"></a>Deserialized nesneler

İş akışlarındaki nesneler deserialized vardır.  Bu, özelliklerinin hala kullanılabilir olduğu, ancak yöntemlerinin bulunmadığı anlamına gelir.  Örneğin, Hizmet nesnesinin Durdur yöntemini kullanarak bir hizmeti durduran aşağıdaki PowerShell kodunu düşünün.

```azurepowershell-interactive
$Service = Get-Service -Name MyService
$Service.Stop()
```

Bunu bir iş akışında çalıştırmayı denerseniz, "Yöntem çağırmawindows PowerShell İş Akışında desteklenmez" diyerek bir hata alırsınız.

Bir seçenek, bu iki kod satırını [inlineScript](#inlinescript) bloğuna $Service durumda blok içinde bir hizmet nesnesi olarak sarmaktır.

```powershell
Workflow Stop-Service
{
    InlineScript {
        $Service = Get-Service -Name MyService
        $Service.Stop()
    }
}
```

Başka bir seçenek, varsa, yöntemle aynı işlevselliği gerçekleştiren başka bir cmdlet kullanmaktır.  Örneğimizde, Stop-Service cmdlet Stop yöntemi ile aynı işlevselliği sağlar ve bir iş akışı için aşağıdakileri kullanabilirsiniz.

```powershell
Workflow Stop-MyService
{
    $Service = Get-Service -Name MyService
    Stop-Service -Name $Service.Name
}
```

## <a name="inlinescript"></a>InlineScript

Etkinlik,`InlineScript` PowerShell iş akışı yerine geleneksel PowerShell komut dosyası olarak bir veya daha fazla komut çalıştırmanız gerektiğinde yararlıdır.  Bir iş akışındaki komutlar işlenmek üzere Windows Workflow Foundation'a gönderilirken, bir InlineScript bloğundaki komutlar Windows PowerShell tarafından işlenir.

InlineScript aşağıda gösterilen aşağıdaki sözdizimini kullanır.

```powershell
InlineScript
{
    <Script Block>
} <Common Parameters>
```

Çıktıyı bir değişkene atayarak inlineScript'den çıktı döndürebilirsiniz. Aşağıdaki örnek, bir hizmeti durdurur ve ardından hizmet adını çıkarır.

```powershell
Workflow Stop-MyService
{
    $Output = InlineScript {
        $Service = Get-Service -Name MyService
        $Service.Stop()
        $Service
    }

    $Output.Name
}
```

Değerleri InlineScript bloğuna geçirebilirsiniz, ancak kapsam değiştirici **$Using** kullanmanız gerekir.  Aşağıdaki örnek, hizmet adının bir değişken tarafından sağlandığı dışında önceki örnekle aynıdır.

```powershell
Workflow Stop-MyService
{
    $ServiceName = "MyService"

    $Output = InlineScript {
        $Service = Get-Service -Name $Using:ServiceName
        $Service.Stop()
        $Service
    }

    $Output.Name
}
```

InlineScript etkinlikleri belirli iş akışlarında kritik olabilir, ancak iş akışı yapılarını desteklemez ve yalnızca aşağıdaki nedenlerle gerektiğinde kullanılmalıdır:

* InlineScript bloğu içinde [denetim noktalarını](#checkpoints) kullanamazsınız. Blok içinde bir hata oluşursa, bloğun başından itibaren devam ettirilmelidir.
* InlineScriptBlock içinde [paralel yürütme](#parallel-processing) kullanamazsınız.
* InlineScript, InlineScript bloğunun tüm uzunluğu boyunca Windows PowerShell oturumunu tuttuğundan iş akışının ölçeklenebilirliğini etkiler.

InlineScript'i kullanma hakkında daha fazla bilgi için, [Bir İş Akışında Windows PowerShell Komutlarını Çalıştırma](https://technet.microsoft.com/library/jj574197.aspx) ve [about_InlineScript.](https://technet.microsoft.com/library/jj649082.aspx)

## <a name="parallel-processing"></a>Paralel işleme

Windows PowerShell İş Akışlarının bir avantajı da bir komutlar kümesini tipik bir betikteki gibi sırayla yürütmek yerine paralel olarak gerçekleştirebilmesidir.

Aynı anda `Parallel` çalışan birden çok komutiçeren bir komut dosyası bloğu oluşturmak için anahtar kelimeyi kullanabilirsiniz. Bu, aşağıda gösterilen aşağıdaki sözdizimini kullanır. Bu durumda, Etkinlik1 ve Etkinlik2 aynı anda başlar. Etkinlik3 yalnızca Hem Etkinlik1 hem de Etkinlik2 tamamlandıktan sonra başlar.

```powershell
Parallel
{
    <Activity1>
    <Activity2>
}
<Activity3>
```

Örneğin, birden çok dosyayı ağ hedefine kopyalayan aşağıdaki PowerShell komutlarını göz önünde bulundurun.  Bu komutlar, bir dosyanın sonraki başlatılmadan önce kopyalamayı bitirmesi için sırayla çalıştırılır.

```azurepowershell-interactive
Copy-Item -Path C:\LocalPath\File1.txt -Destination \\NetworkPath\File1.txt
Copy-Item -Path C:\LocalPath\File2.txt -Destination \\NetworkPath\File2.txt
Copy-Item -Path C:\LocalPath\File3.txt -Destination \\NetworkPath\File3.txt
```

Aşağıdaki iş akışı, aynı komutları paralel olarak çalıştırarak hepsinin aynı anda kopyalamaya başlamasını sağlar.  Yalnızca hepsi kopyalandıktan sonra görüntülenen tamamlama iletisi görüntülenir.

```powershell
Workflow Copy-Files
{
    Parallel
    {
        Copy-Item -Path "C:\LocalPath\File1.txt" -Destination "\\NetworkPath"
        Copy-Item -Path "C:\LocalPath\File2.txt" -Destination "\\NetworkPath"
        Copy-Item -Path "C:\LocalPath\File3.txt" -Destination "\\NetworkPath"
    }

    Write-Output "Files copied."
}
```

Bir koleksiyondaki her öğe için komutları eşzamanlı olarak işlemek için `ForEach -Parallel` yapısını kullanabilirsiniz. Betik bloğundaki komutlar sırayla yürütülürken koleksiyondaki öğeler paralel olarak işlenir. Bu, aşağıda gösterilen aşağıdaki sözdizimini kullanır. Bu durumda, Etkinlik1 koleksiyondaki tüm öğeler için aynı anda başlar. Her öğe için Etkinlik2 Etkinlik1 tamamlandıktan sonra başlar. Etkinlik3, yalnızca tüm öğeler için Hem Etkinlik1 hem de Etkinlik2 tamamlandıktan sonra başlar. `ThrottleLimit` Paralelliği sınırlamak için parametreyi kullanırız. Çok yüksek `ThrottleLimit` bir soruna neden olabilir. `ThrottleLimit` Parametre için ideal değer ortamınızdaki birçok etkene bağlıdır. Düşük bir değerle başlamayı denemeli ve belirli durumunuziçin uygun bir değer bulana kadar farklı artan değerleri denemelisiniz.

```powershell
ForEach -Parallel -ThrottleLimit 10 ($<item> in $<collection>)
{
    <Activity1>
    <Activity2>
}
<Activity3>
```

Aşağıdaki örnek, dosyaları paralel olarak kopyalama önceki örneğe benzer.  Bu durumda, kopyaladıktan sonra her dosya için bir ileti görüntülenir.  Sadece hepsi tamamen kopyalandıktan sonra görüntülenen son tamamlama iletisidir.

```powershell
Workflow Copy-Files
{
    $files = @("C:\LocalPath\File1.txt","C:\LocalPath\File2.txt","C:\LocalPath\File3.txt")

    ForEach -Parallel -ThrottleLimit 10 ($File in $Files)
    {
        Copy-Item -Path $File -Destination \\NetworkPath
        Write-Output "$File copied."
    }

    Write-Output "All files copied."
}
```

> [!NOTE]
> Bu güvenilmez sonuçlar vermek için gösterilmiştir beri paralel olarak çocuk runbooks çalışan önermiyoruz. Alt çalışma kitabından çıkan çıktı bazen görünmüyor ve bir alt çalışma kitabındaki ayarlar diğer paralel alt çalışma kitaplarını etkileyebilir. $VerbosePreference, $WarningPreference ve diğerleri gibi değişkenler alt çalışma defterlerine yayılamaz. Ve alt runbook bu değerleri değiştirirse, bu değişikliklerden sonra düzgün geri olmayabilir.

## <a name="checkpoints"></a>Kontrol noktaları

*Denetim noktası,* değişkenlerin geçerli değerini ve bu noktaya kadar oluşturulan çıktıyı içeren iş akışının geçerli durumunun anlık görüntüsüdür. Bir iş akışı yanlışlıkla biterse veya askıya alınırsa, bir sonraki çalıştırılında iş akışının başlangıcı yerine son denetim noktasından başlar.  `Checkpoint-Workflow` Etkinlikle birlikte iş akışında bir denetim noktası ayarlayabilirsiniz. Azure Otomasyonu, diğer runbook'ların çalışmasına izin vermek için 3 saat çalışan tüm runbook'ların boşaltıldığı [adil paylaşım](automation-runbook-execution.md#fair-share)adı verilen bir özelliğe sahiptir. Sonunda, boşaltılan runbook yeniden yüklenir ve olduğunda, runbook alınan son denetim noktasından yürütme devam edecektir. Runbook'un sonunda tamamlaşacağını garanti etmek için, 3 saatten az çalışan aralıklarla denetim noktaları eklemeniz gerekir. Her çalıştırma sırasında yeni bir denetim noktası eklenirse ve runbook bir hata nedeniyle 3 saat sonra tahliye edilirse, runbook süresiz olarak devam ettirilir.

Aşağıdaki örnek kodda, Etkinlik2'den sonra iş akışının sona ermesine neden olan bir özel durum oluşur. İş akışı yeniden çalıştırıldığında, son denetim noktası kümesinden hemen sonra olduğundan Etkinlik2'yi çalıştırarak başlar.

```powershell
<Activity1>
Checkpoint-Workflow
<Activity2>
<Exception>
<Activity3>
```

Özel durum lara yatkın olabilecek etkinliklerden sonra iş akışında denetim noktaları ayarlamanız ve iş akışı devam ettirilirse yinelenmemesi gerekir. Örneğin, iş akışınız sanal bir makine oluşturabilir. Sanal makine oluşturma komutlarının öncesinde ve sonrasında bir denetim noktası ayarlayabilirsiniz. Oluşturma başarısız olursa, iş akışı yeniden başlatılırsa komutları yinelenir. Oluşturma başarılı olduktan sonra iş akışı başarısız olursa, iş akışı devam ettirildiğinde sanal makine yeniden oluşturulmaz.

Aşağıdaki örnek, birden çok dosyayı ağ konumuna kopyalar ve her dosyadan sonra bir denetim noktası ayarlar.  Ağ konumu kaybolursa, iş akışı yanlışlıkla sona erer.  Yeniden başlatıldığında, yalnızca zaten kopyalanmış olan dosyaların atlandığı anlamına gelen son denetim noktasında devam edecektir.

```powershell
Workflow Copy-Files
{
    $files = @("C:\LocalPath\File1.txt","C:\LocalPath\File2.txt","C:\LocalPath\File3.txt")

    ForEach ($File in $Files)
    {
        Copy-Item -Path $File -Destination \\NetworkPath
        Write-Output "$File copied."
        Checkpoint-Workflow
    }

    Write-Output "All files copied."
}
```

Kullanıcı adı kimlik bilgileri [Askıya Alma-İş Akışı](https://technet.microsoft.com/library/jj733586.aspx) etkinliğini aradıktan sonra veya son denetim noktasından sonra kalıcı olmadığından, kimlik `Suspend-Workflow` bilgilerini null'a ayarlamanız ve sonra veya denetim noktası çağrıldıktan sonra varlık deposundan yeniden almanız gerekir.  Aksi takdirde, aşağıdaki hata iletisini alabilirsiniz:`The workflow job cannot be resumed, either because persistence data could not be saved completely, or saved persistence data has been corrupted. You must restart the workflow.`

Aşağıdaki aynı kod, PowerShell İş Akışı runbook'larınızda bunu nasıl işleyeceğinizgösterilmektedir.

```powershell
workflow CreateTestVms
{
    $Cred = Get-AzAutomationCredential -Name "MyCredential"
    $null = Connect-AzAccount -Credential $Cred

    $VmsToCreate = Get-AzAutomationVariable -Name "VmsToCreate"

    foreach ($VmName in $VmsToCreate)
        {
        # Do work first to create the VM (code not shown)

        # Now add the VM
        New-AzVM -VM $Vm -Location "WestUs" -ResourceGroupName "ResourceGroup01"

        # Checkpoint so that VM creation is not repeated if workflow suspends
        $Cred = $null
        Checkpoint-Workflow
        $Cred = Get-AzAutomationCredential -Name "MyCredential"
        $null = Connect-AzAccount -Credential $Cred
        }
}
```

> [!NOTE]
> Grafiksel olmayan PowerShell runbook'ları için `Add-AzAccount` ve `Add-AzureRMAccount` [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-3.5.0)için diğer adlardır. Bu cmdlets'i kullanabilir veya Otomasyon hesabınızdaki [modüllerinizi](automation-update-azure-modules.md) en son sürümlere güncelleyebilirsiniz. Yeni bir Otomasyon hesabı oluşturmuş olsanız bile modüllerinizi güncellemeniz gerekebilir.


Bir hizmet sorumlusu ile yapılandırılan Bir Çalıştır Hesabı kullanarak kimlik doğrulaması kullanıyorsanız, bu gerekli değildir.

Denetim noktalarıyla ilgili daha fazla bilgi için bkz. [Betik İş Akışına Denetim Noktaları Ekleme](https://technet.microsoft.com/library/jj574114.aspx).

## <a name="next-steps"></a>Sonraki adımlar

* PowerShell iş akışı runbook'larına başlamak için [ilk PowerShell iş akışı çalışma kitabıma](automation-first-runbook-textual.md) bakın

