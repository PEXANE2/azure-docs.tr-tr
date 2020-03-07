---
title: Azure Otomasyonu için PowerShell Iş akışını öğrenme
description: Bu makale, PowerShell ve PowerShell Iş akışı ile Otomasyon Runbook 'larına uygulanan kavramlar arasındaki belirli farklılıkları anlamak için PowerShell 'e alışkın olan yazarlar için hızlı bir ders olarak hazırlanmıştır.
services: automation
ms.subservice: process-automation
ms.date: 12/14/2018
ms.topic: conceptual
ms.openlocfilehash: 6e4c8057322b6208ea3b447b264e2bde1344540c
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78372437"
---
# <a name="learning-key-windows-powershell-workflow-concepts-for-automation-runbooks"></a>Otomasyon Runbook 'ları için öğrenme anahtarı Windows PowerShell Iş akışı kavramları

Azure Otomasyonu 'ndaki runbook 'lar Windows PowerShell Iş akışları olarak uygulanır.  Windows PowerShell Iş akışı, bir Windows PowerShell betiğine benzer ancak yeni bir kullanıcı için kafa karıştırıcı olabilecek bazı önemli farklılıklar içerir.  Bu makale PowerShell iş akışını kullanarak runbook yazmanıza yardımcı olmaya yönelik olsa da, denetim noktalarına ihtiyaç duymadığınız sürece PowerShell kullanarak runbook yazmanızı öneririz.  PowerShell Iş akışı runbook 'larını yazarken birkaç sözdizimi farkı vardır ve bu farklılıklar, etkili iş akışları yazmak için biraz daha iş gerektirir.

İş akışı uzun süre çalışan görevleri gerçekleştiren veya birden çok aygıttaki veya yönetilen düğümlerdeki birden çok adımın koordinasyonu için gereken programlanmış, bağlantılı adımlardan oluşan bir dizidir. İş akışının normal betiğe göre avantajları bir eylemi birden çok aygıtta aynı anda gerçekleştirebilme ve hatadan otomatik olarak kurtarma yetenekleridir. Windows PowerShell Iş akışı, Windows Workflow Foundation kullanan bir Windows PowerShell betiğtir. İş akışı Windows PowerShell sözdizimi kullanılarak yazılsa ve Windows PowerShell tarafından başlatılsa da, Windows Workflow Foundation tarafından işlenir.

Bu makaledeki konularla ilgili tüm ayrıntılar için bkz. [Windows PowerShell Iş akışını kullanmaya](https://technet.microsoft.com/library/jj134242.aspx)başlama.

## <a name="basic-structure-of-a-workflow"></a>Bir iş akışının temel yapısı

PowerShell betiğini bir PowerShell iş akışına dönüştürmenin ilk adımı, **Iş akışı** anahtar sözcüğüyle çevredir.  Bir iş akışı, **Iş akışı** anahtar sözcüğüyle başlar ve ardından, parantez içine alınmış betiğin gövdesini izler. İş akışının adı, aşağıdaki sözdiziminde gösterildiği gibi **Iş akışı** anahtar sözcüğünü izler:

```powershell
Workflow Test-Workflow
{
    <Commands>
}
```

İş akışı adını, Otomasyon runbook'u adıyla eşleşmelidir. Runbook içeri aktarıldıysa, dosya adının iş akışı adıyla eşleşmesi ve *. ps1*ile bitmesi gerekir.

İş akışına parametre eklemek için, komut dosyasında olduğu gibi **param** anahtar sözcüğünü kullanın.

## <a name="code-changes"></a>Kod değişiklikleri

PowerShell iş akışı kodu, birkaç önemli değişiklik haricinde PowerShell betiği kodu ile neredeyse aynı şekilde görünür.  Aşağıdaki bölümlerde, bir iş akışında çalışması için bir PowerShell betiğine yapmanız gereken değişiklikler açıklanır.

### <a name="activities"></a>Etkinlikler

Etkinlik bir iş akışındaki belirli bir görevdir. Betiğin bir veya daha çok komuttan oluşmasına benzer şekilde, iş akışı da belirli bir sırayla yürütülen bir veya daha çok etkinlikten oluşur. Windows PowerShell Workflow birçok Windows PowerShell cmdlet'ini bir iş akışı içinde çalıştıklarında otomatik olarak etkinliklere dönüştürür. Runbook 'inizdeki bu cmdlet 'lerden birini belirttiğinizde, ilgili etkinlik Windows Workflow Foundation tarafından çalıştırılır. Karşılık gelen bir etkinliği olmayan bu cmdlet 'ler için, Windows PowerShell Iş akışı cmdlet 'i bir [InlineScript](#inlinescript) etkinliğinde otomatik olarak çalıştırır. Siz özel olarak bir InlineScript bloğunda eklemedikçe hariç tutulan ve bir iş akışında kullanılamayan cmdlet'ler kümesi vardır. Bu kavramlar hakkında daha fazla bilgi için bkz. [betik Iş akışlarında etkinlikleri kullanma](https://technet.microsoft.com/library/jj574194.aspx).

İş akışı etkinlikleri çalışmalarını yapılandıran ortak parametreler kümesini paylaşır. İş akışı ortak parametreleri hakkında daha fazla bilgi için bkz. [about_WorkflowCommonParameters](https://technet.microsoft.com/library/jj129719.aspx).

### <a name="positional-parameters"></a>Konumsal parametreler

Bir iş akışındaki etkinlikler ve cmdlet 'lerle konumsal parametreleri kullanamazsınız.  Tüm bu anlamı parametre adlarını kullanmanız gerekir.

Örneğin, tüm çalışan hizmetleri alan aşağıdaki kodu göz önünde bulundurun.

```azurepowershell-interactive
Get-Service | Where-Object {$_.Status -eq "Running"}
```

Aynı kodu bir iş akışında çalıştırmayı denerseniz, "parametre kümesi belirtilen adlandırılmış parametreler kullanılarak çözümlenemiyor" gibi bir ileti alırsınız.  Bunu düzeltmek için aşağıdaki gibi parametre adını sağlayın.

```powershell
Workflow Get-RunningServices
{
    Get-Service | Where-Object -FilterScript {$_.Status -eq "Running"}
}
```

### <a name="deserialized-objects"></a>Seri durumdan çıkarılan nesneler

İş akışlarında nesnelerin serisi kaldırılır.  Bu, özelliklerinin hala kullanılabildiği, ancak yöntemleri olmadığı anlamına gelir.  Örneğin, hizmet nesnesinin Stop metodunu kullanarak bir hizmeti durduran aşağıdaki PowerShell kodunu göz önünde bulundurun.

```azurepowershell-interactive
$Service = Get-Service -Name MyService
$Service.Stop()
```

Bunu bir iş akışında çalıştırmayı denerseniz, "Windows PowerShell Iş akışında Yöntem çağırma desteklenmez." hatasını alıyorsunuz.

Tek bir seçenek, bu iki kod satırını bir [InlineScript](#inlinescript) bloğunda sarmalıdır ve bu durumda $Service blok içindeki bir hizmet nesnesi olur.

```powershell
Workflow Stop-Service
{
    InlineScript {
        $Service = Get-Service -Name MyService
        $Service.Stop()
    }
}
```

Başka bir seçenek de, varsa yöntemiyle aynı işlevleri gerçekleştiren başka bir cmdlet kullanmaktır.  Örneğimizde, Stop-Service cmdlet 'i stop yöntemiyle aynı işlevselliği sağlar ve bir iş akışı için aşağıdakileri kullanabilirsiniz.

```powershell
Workflow Stop-MyService
{
    $Service = Get-Service -Name MyService
    Stop-Service -Name $Service.Name
}
```

## <a name="inlinescript"></a>InlineScript

**InlineScript** etkinliği, PowerShell iş akışı yerine bir veya daha fazla komutu geleneksel PowerShell betiği olarak çalıştırmanız gerektiğinde faydalıdır.  Bir iş akışındaki komutlar işlenmek üzere Windows Workflow Foundation'a gönderilirken, bir InlineScript bloğundaki komutlar Windows PowerShell tarafından işlenir.

InlineScript aşağıda gösterilen aşağıdaki sözdizimini kullanır.

```powershell
InlineScript
{
    <Script Block>
} <Common Parameters>
```

Çıktıyı bir değişkene atayarak bir InlineScript öğesinden çıkış geri alabilirsiniz. Aşağıdaki örnek bir hizmeti durdurup hizmet adını verir.

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

Değerleri bir InlineScript bloğuna geçirebilirsiniz, ancak **$using** kapsam değiştiricisini kullanmanız gerekir.  Aşağıdaki örnek, hizmet adının bir değişken tarafından sağlanması dışında, önceki örnekle aynıdır.

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

InlineScript etkinlikleri belirli iş akışlarında önemli olabileceğinden, iş akışı yapılarını desteklemezler ve yalnızca aşağıdaki nedenlerle gerekli olduğunda kullanılmalıdır:

* Bir InlineScript bloğunun içinde [denetim noktaları](#checkpoints) kullanamazsınız. Blok içinde bir hata oluşursa, bloğun başından devam etmelidir.
* Bir ınlinescriptblock içinde [paralel yürütme](#parallel-processing) kullanamazsınız.
* InlineScript, InlineScript bloğunun tüm uzunluğu için Windows PowerShell oturumunu tutduğundan, iş akışının ölçeklenebilirliğini etkiler.

InlineScript kullanma hakkında daha fazla bilgi için bkz. [bir Iş akışında Windows PowerShell komutlarını çalıştırma](https://technet.microsoft.com/library/jj574197.aspx) ve [about_InlineScript](https://technet.microsoft.com/library/jj649082.aspx).

## <a name="parallel-processing"></a>Paralel işleme

Windows PowerShell İş Akışlarının bir avantajı da bir komutlar kümesini tipik bir betikteki gibi sırayla yürütmek yerine paralel olarak gerçekleştirebilmesidir.

Aynı anda çalışan birden çok komut içeren bir betik bloğu oluşturmak için **Parallel** anahtar sözcüğünü kullanabilirsiniz. Bu, aşağıda gösterildiği gibi aşağıdaki sözdizimini kullanır. Bu durumda, Activity1 ve Activity2 aynı anda başlar. Activity3 yalnızca Activity1 ve Activity2 tamamlandıktan sonra başlar.

```powershell
Parallel
{
    <Activity1>
    <Activity2>
}
<Activity3>
```

Örneğin, bir ağ hedefine birden çok dosya kopyalamak için aşağıdaki PowerShell komutlarını göz önünde bulundurun.  Bir sonraki başlamadan önce bir dosyanın kopyalamayı tamamlaması gereken bu komutlar sırayla çalıştırılır.

```azurepowershell-interactive
Copy-Item -Path C:\LocalPath\File1.txt -Destination \\NetworkPath\File1.txt
Copy-Item -Path C:\LocalPath\File2.txt -Destination \\NetworkPath\File2.txt
Copy-Item -Path C:\LocalPath\File3.txt -Destination \\NetworkPath\File3.txt
```

Aşağıdaki iş akışı, aynı komutları aynı anda kopyalamaya başlamak için paralel olarak çalıştırır.  Yalnızca tüm kopyalandıktan sonra, görüntülenen tamamlanma iletisi görüntülenir.

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

Bir koleksiyondaki her öğe için komutları eşzamanlı olarak işlemek için **ForEach-Parallel** yapısını kullanabilirsiniz. Betik bloğundaki komutlar sırayla yürütülürken koleksiyondaki öğeler paralel olarak işlenir. Bu, aşağıda gösterildiği gibi aşağıdaki sözdizimini kullanır. Bu durumda, Activity1 koleksiyondaki tüm öğeler için aynı anda başlar. Her öğe için Activity2, Activity1 tamamlandıktan sonra başlar. Activity3, yalnızca Activity1 ve Activity2 tüm öğeler için tamamlandıktan sonra başlar. Paralelliği sınırlamak için `ThrottleLimit` parametresini kullanıyoruz. Çok yüksek bir `ThrottleLimit` soruna neden olabilir. `ThrottleLimit` parametresinin ideal değeri, ortamınızdaki birçok faktöre bağlıdır. Daha düşük bir değerle başlayın ve belirli bir durumda çalışacak bir tane bulana kadar artan değerleri farklı şekilde deneyin.

```powershell
ForEach -Parallel -ThrottleLimit 10 ($<item> in $<collection>)
{
    <Activity1>
    <Activity2>
}
<Activity3>
```

Aşağıdaki örnek, dosyaları paralel olarak kopyalamaya yönelik bir önceki örneğe benzerdir.  Bu durumda, her dosya için kopyaladıktan sonra bir ileti görüntülenir.  Yalnızca tamamen kopyalandıklarında, son tamamlanma iletisi görüntülenir.

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
> Bu durum güvenilir olmayan sonuçlara izin vermek üzere gösterildiğinden, alt runbook 'ların paralel olarak çalıştırılmasını önermiyoruz. Alt runbook 'un çıkışı bazen gösterilmez ve bir alt runbook 'taki ayarlar diğer paralel alt runbook 'ları etkileyebilir. $VerbosePreference, $WarningPreference ve diğerleri gibi değişkenler alt runbook 'lara yayılmayabilir. Alt runbook bu değerleri değiştirirse, çağrıdan sonra düzgün şekilde geri yüklenemeyebilir.

## <a name="checkpoints"></a>Kontrol noktaları

*Denetim noktası* , iş akışının geçerli durumunun, değişkenler için geçerli değer ve bu noktaya oluşturulan tüm çıktılar içeren bir anlık görüntüsüdür. Bir iş akışı hatayla sona ererse veya askıya alınırsa, bir sonraki çalıştırılışında iş akışının başlangıcı yerine son denetim noktasından başlar.  **Denetim noktası-Iş akışı** etkinliğiyle bir iş akışında denetim noktası ayarlayabilirsiniz. Azure Otomasyonu, diğer runbook 'ların çalışmasına izin vermek için 3 saat boyunca çalışan runbook 'ların çalıştırıldığı, [dengeli bir Share](automation-runbook-execution.md#fair-share)adlı bir özelliğe sahiptir. Sonuç olarak, yüklenmeyen runbook yeniden yüklenir ve olduğunda runbook 'taki son denetim noktasından yürütme devam eder. Runbook 'un sonunda tamamlanabileceğini garantilemek için 3 saatten az bir süre çalışan aralıklarla kontrol noktaları eklemeniz gerekir. Her çalıştırma sırasında yeni bir kontrol noktası eklenirse ve bir hata nedeniyle runbook 3 saat sonra çıkarıldığında, runbook süresiz olarak sürdürülür.

Aşağıdaki örnek kodda, Activity2 sonrasında iş akışının bitmesini neden olan bir özel durum oluşur. İş akışı yeniden çalıştırıldığında, Activity2 çalıştırılarak başlatılır çünkü bu, son denetim noktası kümesinden hemen sonra.

```powershell
<Activity1>
Checkpoint-Workflow
<Activity2>
<Exception>
<Activity3>
```

Özel duruma neden olabilecek etkinliklerden sonra bir iş akışında denetim noktaları ayarlamanız gerekir ve iş akışı devam ettirirse yinelenmemelidir. Örneğin, iş akışınız bir sanal makine oluşturabilir. Sanal makine oluşturma komutlarının öncesinde ve sonrasında bir denetim noktası ayarlayabilirsiniz. Oluşturma başarısız olursa, iş akışı yeniden başlatılırsa komutlar yinelenir. Oluşturma başarılı olduktan sonra iş akışı başarısız olursa, iş akışı sürdürüldüğünde sanal makine yeniden oluşturulmaz.

Aşağıdaki örnekte birden çok dosya bir ağ konumuna kopyalanır ve her dosyadan sonra bir denetim noktası ayarlanır.  Ağ konumu kaybolursa, iş akışı hata ile sona erer.  Yeniden başlatıldığında, son denetim noktasında sürdürülecek ve yalnızca zaten kopyalanmış dosyaların atlandığı anlamına gelir.

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

[Askıya alma Iş akışı](https://technet.microsoft.com/library/jj733586.aspx) etkinliğini veya son kontrol noktasından sonra Kullanıcı adı kimlik bilgileri kalıcı olmadığından, kimlik bilgilerini null olarak ayarlamanız ve ardından **askıya alma iş akışı** veya denetim noktası çağrıldıktan sonra bunları varlık deposundan yeniden almanız gerekir.  Aksi takdirde, aşağıdaki hata iletisini alabilirsiniz: *Kalıcılık verileri tamamen kaydedilemediğinden ya da kayıtlı Kalıcılık verileri bozulduğundan iş akışı işi sürdürülemez. İş akışını yeniden başlatmanız gerekir.*

Aşağıdaki aynı kodda PowerShell Iş akışı runbook 'larınızda bunun nasıl işleneceği gösterilmektedir.

```powershell
workflow CreateTestVms
{
    $Cred = Get-AzureAutomationCredential -Name "MyCredential"
    $null = Connect-AzureRmAccount -Credential $Cred

    $VmsToCreate = Get-AzureAutomationVariable -Name "VmsToCreate"

    foreach ($VmName in $VmsToCreate)
        {
        # Do work first to create the VM (code not shown)

        # Now add the VM
        New-AzureRmVm -VM $Vm -Location "WestUs" -ResourceGroupName "ResourceGroup01"

        # Checkpoint so that VM creation is not repeated if workflow suspends
        $Cred = $null
        Checkpoint-Workflow
        $Cred = Get-AzureAutomationCredential -Name "MyCredential"
        $null = Connect-AzureRmAccount -Credential $Cred
        }
}
```

> [!IMPORTANT]
> **Add-azurermaccount** artık **Connect-azurermaccount**için bir diğer addır. Kitaplık öğelerinizi ararken, **Connect-AzureRMAccount**' u görmüyorsanız **Add-azurermaccount**komutunu kullanabilir veya bir Otomasyon hesabınızda modüllerinizi güncelleştirebilirsiniz.

Hizmet sorumlusu ile yapılandırılmış bir farklı çalıştır hesabı kullanarak kimlik doğrulaması yapıyorsanız bu gerekli değildir.

Kontrol noktaları hakkında daha fazla bilgi için bkz. [bir betik Iş akışına denetim noktaları ekleme](https://technet.microsoft.com/library/jj574114.aspx).

## <a name="next-steps"></a>Sonraki adımlar

* PowerShell iş akışı runbook'larını kullanmaya başlamak için bkz. [İlk PowerShell iş akışı runbook uygulamam](automation-first-runbook-textual.md)

