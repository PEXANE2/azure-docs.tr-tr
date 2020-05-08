---
title: Azure Otomasyonu 'nda runbook 'ları yönetme
description: Bu makalede, Azure Otomasyonu 'nda runbook 'ların nasıl yönetileceği açıklanır. Temel işlemleri içerir ve bazı en iyi yöntemleri ekler.
services: automation
ms.subservice: process-automation
ms.date: 02/14/2019
ms.topic: conceptual
ms.openlocfilehash: 687579825c652888112ff8ddff7401b3305e3a8e
ms.sourcegitcommit: f57297af0ea729ab76081c98da2243d6b1f6fa63
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82871187"
---
# <a name="manage-runbooks-in-azure-automation"></a>Azure Otomasyonu 'nda runbook 'ları yönetme

Yeni bir tane oluşturarak veya var olan bir dosyayı ya da [runbook galerisinden](automation-runbook-gallery.md)bir runbook 'u içeri aktararak, Azure Otomasyonu 'na bir runbook ekleyebilirsiniz. Bu makalede, bir dosyadan içeri aktarılmış bir runbook 'u yönetmeye yönelik bilgiler sağlanmaktadır. [Azure Otomasyonu Için runbook ve modül galerilerinde](automation-runbook-gallery.md)topluluk runbook 'larına ve modüllerine erişmenin tüm ayrıntılarını bulabilirsiniz.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma runbook çalışanınız hakkında az Module yükleme yönergeleri için bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Otomasyon hesabınız için, [Azure Otomasyonu 'nda Azure PowerShell modüllerini güncelleştirme](automation-update-azure-modules.md)' yi kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="create-a-runbook"></a>Runbook oluşturma

Azure portal veya Windows PowerShell kullanarak Azure Otomasyonu 'nda yeni bir runbook oluşturun. Runbook oluşturulduktan sonra, içindeki bilgileri kullanarak düzenleyebilirsiniz.

* [Azure Otomasyonu 'nda metin runbook 'unu düzenleme](automation-edit-textual-runbook.md) 
* [Otomasyon Runbook 'ları için anahtar Windows PowerShell Iş akışı kavramlarını öğrenin](automation-powershell-workflow.md)
* [Azure Otomasyonu 'nda grafik yazma](automation-graphical-authoring-intro.md)
* [Azure Otomasyonu 'nda Python 2 paketlerini yönetme](python-packages.md)

### <a name="create-a-runbook-in-the-azure-portal"></a>Azure portal runbook oluşturma

1. Azure portalında, Otomasyon hesabınızı açın.
2. Hub 'dan runbook listesini açmak için **Işlem Otomasyonu** altında **runbook 'lar** ' ı seçin.
3. **Runbook oluştur ' a**tıklayın.
4. Runbook için bir ad girin ve [türünü](automation-runbook-types.md)seçin. Runbook adı bir harfle başlamalıdır ve harfler, rakamlar, alt çizgiler ve kısa çizgiler içerebilir.
5. Runbook 'u oluşturmak için **Oluştur** ' a tıklayın ve düzenleyiciyi açın.

### <a name="create-a-runbook-with-powershell"></a>PowerShell ile runbook oluşturma

Boş bir runbook oluşturmak için [New-AzAutomationRunbook](https://docs.microsoft.com/powershell/module/az.automation/new-azautomationrunbook?view=azps-3.5.0) cmdlet 'ini kullanın. İçin `New-AzAutomationRunbook`tanımlanan `Type` runbook türlerinden birini belirtmek için parametresini kullanın.

Aşağıdaki örnek, yeni bir boş runbook oluşturmayı gösterir.

```azurepowershell-interactive
New-AzAutomationRunbook -AutomationAccountName MyAccount `
-Name NewRunbook -ResourceGroupName MyResourceGroup -Type PowerShell
```

## <a name="import-a-runbook"></a>Runbook 'u içeri aktarma

Kendi runbook 'unuzu oluşturmak için bir PowerShell veya PowerShell Iş akışı (**. ps1**) betiği, bir grafik runbook 'u (. graphrunbook) veya bir Python 2 betiği (**.****çıgrafı**) içeri aktarabilirsiniz.  İçeri aktarma sırasında oluşturulan [runbook 'un türünü](automation-runbook-types.md) belirtmeniz gerekir ve aşağıdaki noktaları dikkate alın.

* Bir [PowerShell runbook](automation-runbook-types.md#powershell-runbooks) 'Una veya [PowerShell iş akışı runbook](automation-runbook-types.md#powershell-workflow-runbooks)'unda iş akışı içermeyen bir **. ps1** dosyasını içeri aktarabilirsiniz. Bir PowerShell Iş akışı runbook 'una içeri aktarırsanız, bir iş akışına dönüştürülür. Bu durumda, yapılan değişiklikleri anlatmak için Runbook 'a açıklamalar dahildir.

* PowerShell iş akışı [runbook 'una](automation-runbook-types.md#powershell-workflow-runbooks)yalnızca bir PowerShell iş akışı içeren bir **. ps1** dosyasını içeri aktarabilirsiniz. Dosya birden çok PowerShell iş akışı içeriyorsa içeri aktarma başarısız olur. Her bir iş akışını kendi dosyasına kaydetmeli ve ayrı olarak içeri aktarmanız gerekir.

* PowerShell betik altyapısı bunu tanıyamadığı için PowerShell Iş akışını içeren bir **. ps1** dosyasını [PowerShell runbook 'una](automation-runbook-types.md#powershell-runbooks)içeri aktarmayın.

* Yalnızca bir **. graphrunbook** dosyasını yeni bir [grafik runbook 'una](automation-runbook-types.md#graphical-runbooks)aktarın. 

### <a name="import-a-runbook-from-the-azure-portal"></a>Azure portal runbook 'u içeri aktarma

Bir betik dosyasını Azure Otomasyonu 'na aktarmak için aşağıdaki yordamı kullanabilirsiniz.

> [!NOTE]
> Portalı kullanarak bir PowerShell Iş akışı runbook 'unda yalnızca bir **. ps1** dosyasını içeri aktarabilirsiniz.

1. Azure portalında, Otomasyon hesabınızı açın.
2. Runbook 'ların listesini açmak için **Işlem Otomasyonu** altında **runbook 'lar** ' ı seçin.
3. **Runbook 'U Içeri Aktar ' a**tıklayın.
4. **Runbook dosyası** ' na tıklayın ve içeri aktarılacak dosyayı seçin.
5. **Ad** alanı etkinleştirilirse, runbook adını değiştirme seçeneğiniz vardır. Ad bir harfle başlamalıdır ve harfler, rakamlar, alt çizgiler ve kısa çizgiler içerebilir.
6. [Runbook türü](automation-runbook-types.md) otomatik olarak seçilir, ancak ilgili kısıtlamaları hesaba aldıktan sonra türü değiştirebilirsiniz.
7. **Oluştur**' a tıklayın. Yeni runbook, Otomasyon hesabı için Runbook 'lar listesinde görüntülenir.
8. Çalıştırmak için [runbook 'u yayımlamanız](#publish-a-runbook) gerekir.

> [!NOTE]
> Grafik runbook 'unu içeri aktardıktan sonra, başka bir türe dönüştürebilirsiniz. Ancak, grafik bir runbook 'u metin temelli bir runbook 'a dönüştüremezsiniz.

### <a name="import-a-runbook-with-windows-powershell"></a>Windows PowerShell ile bir runbook 'u içeri aktarma

Bir betik dosyasını taslak runbook olarak içeri aktarmak için [Import-AzAutomationRunbook](https://docs.microsoft.com/powershell/module/az.automation/import-azautomationrunbook?view=azps-3.5.0) cmdlet 'ini kullanın. Runbook zaten mevcutsa, `Force` parametresini cmdlet ile kullanmadığınız müddetçe içeri aktarma başarısız olur.

Aşağıdaki örnek bir runbook 'a bir betik dosyasının nasıl içeri aktarılacağını gösterir.

```azurepowershell-interactive
$automationAccountName =  "AutomationAccount"
$runbookName = "Sample_TestRunbook"
$scriptPath = "C:\Runbooks\Sample_TestRunbook.ps1"
$RGName = "ResourceGroup"

Import-AzAutomationRunbook -Name $runbookName -Path $scriptPath `
-ResourceGroupName $RGName -AutomationAccountName $automationAccountName `
-Type PowerShellWorkflow
```

## <a name="handle-resources"></a>Kaynakları işle

Runbook 'ağınız bir [kaynak](automation-runbook-execution.md#resources)oluşturursa, betiği oluşturmayı denemeden önce kaynağın zaten mevcut olup olmadığını kontrol etmelidir. Temel bir örnek aşağıda verilmiştir.

```powershell
$vmName = "WindowsVM1"
$resourceGroupName = "myResourceGroup"
$myCred = Get-AutomationPSCredential "MyCredential"
$vmExists = Get-AzResource -Name $vmName -ResourceGroupName $resourceGroupName

if(!$vmExists)
    {
    Write-Output "VM $vmName does not exist, creating"
    New-AzVM -Name $vmName -ResourceGroupName $resourceGroupName -Credential $myCred
    }
else
    {
    Write-Output "VM $vmName already exists, skipping"
    }
```

## <a name="retrieve-details-from-activity-log"></a>Etkinlik günlüğünden ayrıntıları alma

Otomasyon hesabı için [etkinlik günlüğünden](automation-runbook-execution.md#activity-logging) runbook 'u başlatan kişi veya hesap gibi runbook ayrıntılarını alabilirsiniz. Aşağıdaki PowerShell örneği, son kullanıcının belirtilen runbook 'u çalıştırmasını sağlar.

```powershell-interactive
$SubID = "00000000-0000-0000-0000-000000000000"
$AutomationResourceGroupName = "MyResourceGroup"
$AutomationAccountName = "MyAutomationAccount"
$RunbookName = "MyRunbook"
$StartTime = (Get-Date).AddDays(-1)
$JobActivityLogs = Get-AzLog -ResourceGroupName $AutomationResourceGroupName -StartTime $StartTime `
                                | Where-Object {$_.Authorization.Action -eq "Microsoft.Automation/automationAccounts/jobs/write"}

$JobInfo = @{}
foreach ($log in $JobActivityLogs)
{
    # Get job resource
    $JobResource = Get-AzResource -ResourceId $log.ResourceId

    if ($JobInfo[$log.SubmissionTimestamp] -eq $null -and $JobResource.Properties.runbook.name -eq $RunbookName)
    {
        # Get runbook
        $Runbook = Get-AzAutomationJob -ResourceGroupName $AutomationResourceGroupName -AutomationAccountName $AutomationAccountName `
                                            -Id $JobResource.Properties.jobId | ? {$_.RunbookName -eq $RunbookName}

        # Add job information to hashtable
        $JobInfo.Add($log.SubmissionTimestamp, @($Runbook.RunbookName,$Log.Caller, $JobResource.Properties.jobId))
    }
}
$JobInfo.GetEnumerator() | sort key -Descending | Select-Object -First 1
```

## <a name="track-progress"></a>İlerleme izleme

Runbook 'larınızın daha kolay bir şekilde yeniden kullanılabilmesi ve yeniden başlatılması için, runbook 'larınızı doğası gereği modüler olarak yazmak iyi bir uygulamadır. Bir runbook 'ta ilerlemeyi izlemek, sorunlar varsa runbook mantığının doğru şekilde yürütüldüğünü sağlar. 

Bir runbook 'un ilerlemesini depolama hesabı, veritabanı veya paylaşılan dosyalar gibi bir dış kaynak kullanarak izleyebilirsiniz. İlk olarak gerçekleştirilen son eylemin durumunu denetlemek için Runbook 'ınızdaki mantığı oluşturun. Daha sonra, denetim sonuçlarına göre, mantıksal runbook 'taki belirli görevleri atlayabilir veya devam edebilir.

## <a name="prevent-concurrent-jobs"></a>Eş zamanlı işleri engelle

Bazı runbook 'lar aynı anda birden çok iş üzerinde çalışıyorsa Strangely davranır. Bu durumda, bir runbook 'un zaten çalışmakta olan bir işin olup olmadığını belirlemede Logic uygulaması için önem taşır. Temel bir örnek aşağıda verilmiştir.

```powershell
# Authenticate to Azure
$connection = Get-AutomationConnection -Name AzureRunAsConnection
Connect-AzAccount -ServicePrincipal -Tenant $connection.TenantID `
-ApplicationId $connection.ApplicationID -CertificateThumbprint $connection.CertificateThumbprint

$AzContext = Select-AzSubscription -SubscriptionId $connection.SubscriptionID

# Check for already running or new runbooks
$runbookName = "<RunbookName>"
$rgName = "<ResourceGroupName>"
$aaName = "<AutomationAccountName>"
$jobs = Get-AzAutomationJob -ResourceGroupName $rgName -AutomationAccountName $aaName -RunbookName $runbookName -AzContext $AzureContext

# Check to see if it is already running
$runningCount = ($jobs | ? {$_.Status -eq "Running"}).count

If (($jobs.status -contains "Running" -And $runningCount -gt 1 ) -Or ($jobs.Status -eq "New")) {
    # Exit code
    Write-Output "Runbook is already running"
    Exit 1
} else {
    # Insert Your code here
}
```

## <a name="handle-transient-errors-in-a-time-dependent-script"></a>Zamana bağlı bir betikte geçici hataları işleme

Runbook 'larınızın, yeniden başlatılmasına veya başarısız olmasına neden olabilecek geçici hatalar da dahil olmak üzere sağlam ve [hataları](automation-runbook-execution.md#errors)işleyebilme yeteneğine sahip olması gerekir. Bir runbook başarısız olursa, Azure Otomasyonu bunu yeniden dener.

Runbook 'larınız normalde bir zaman kısıtlaması içinde çalışıyorsa, yürütme süresini denetlemek için komut dosyasının Logic uygular. Bu denetim, yalnızca belirli zamanlarda başlatma, kapatma veya genişleme gibi işlemlerin çalıştırılmasını sağlar.

> [!NOTE]
> Azure korumalı alan işlemindeki yerel saat UTC olarak ayarlanır. Runbook 'larınızda tarih ve saat hesaplamaları bu olguyu göz önünde bulundurmanız gerekir.

## <a name="work-with-multiple-subscriptions"></a>Birden fazla abonelik ile çalışma

Runbook 'larınızın [aboneliklerle](automation-runbook-execution.md#subscriptions)çalışabilebilmesi gerekir. Örneğin, birden çok aboneliği işlemek için Runbook [devre dışı bırak-AzContextAutosave](https://docs.microsoft.com/powershell/module/Az.Accounts/Disable-AzContextAutosave?view=azps-3.5.0) cmdlet 'ini kullanır. Bu cmdlet, kimlik doğrulama bağlamının aynı korumalı alanda çalışan başka bir runbook 'tan alınmamasını sağlar. Runbook, az Module cmdlet`AzContext` 'lerinde parametresini de kullanır ve uygun bağlamı geçirir.

```powershell
# Ensures that you do not inherit an AzContext in your runbook
Disable-AzContextAutosave –Scope Process

$Conn = Get-AutomationConnection -Name AzureRunAsConnection
Connect-AzAccount -ServicePrincipal `
-Tenant $Conn.TenantID `
-ApplicationId $Conn.ApplicationID `
-CertificateThumbprint $Conn.CertificateThumbprint

$context = Get-AzContext

$ChildRunbookName = 'ChildRunbookDemo'
$AutomationAccountName = 'myAutomationAccount'
$ResourceGroupName = 'myResourceGroup'

Start-AzAutomationRunbook `
    -ResourceGroupName $ResourceGroupName `
    -AutomationAccountName $AutomationAccountName `
    -Name $ChildRunbookName `
    -DefaultProfile $context
```

## <a name="work-with-a-custom-script"></a>Özel bir komut dosyasıyla çalışma

> [!NOTE]
> Normalde Log Analytics Aracısı yüklü olan konakta özel komut dosyaları ve Runbook 'lar çalıştıramazsınız. 

Özel bir komut dosyası kullanmak için:

1. Bir Otomasyon hesabı oluşturun ve [katkıda bulunan rolü](automation-role-based-access-control.md)alın.
2. [Hesabı Azure çalışma alanına bağlayın](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection.md).
3. [Karma Runbook Worker](automation-hybrid-runbook-worker.md), [güncelleştirme yönetimi](automation-update-management.md)veya başka bir Otomasyon özelliğini etkinleştirin. 
4. Bir Linux makinesinde, yüksek izinlere sahip olmanız gerekir. [İmza denetimlerini](automation-linux-hrw-install.md#turn-off-signature-validation)kapatmak için oturum açın.

## <a name="test-a-runbook"></a>Bir runbook'u test etme

Bir runbook 'u test ettiğinizde [taslak sürümü](#publish-a-runbook) yürütülür ve gerçekleştirdiği tüm işlemler tamamlanır. İş geçmişi oluşturulmaz, ancak [Çıkış](automation-runbook-output-and-messages.md#output-stream) ve [uyarı ve hata](automation-runbook-output-and-messages.md#message-streams) akışları test çıkış bölmesinde görüntülenir. [Ayrıntılı akışa](automation-runbook-output-and-messages.md#message-streams) Iletiler yalnızca [VerbosePreference](automation-runbook-output-and-messages.md#preference-variables) değişkeni olarak `Continue`ayarlandıysa çıkış bölmesinde görüntülenir.

Taslak sürümü çalıştırılsa da runbook hala normal şekilde yürütülür ve ortamdaki kaynaklara karşı herhangi bir eylem gerçekleştirir. Bu nedenle, yalnızca üretim dışı kaynaklarda runbook 'ları test etmelisiniz.

Her [runbook türünü](automation-runbook-types.md) test etme yordamı aynıdır. Azure portal metin Düzenleyicisi ile grafik Düzenleyicisi arasındaki sınamada fark yoktur.

1. Runbook 'un taslak sürümünü [metin düzenleyicisinde](automation-edit-textual-runbook.md) veya [grafik düzenleyicide](automation-graphical-authoring-intro.md)açın.
1. Test sayfasını açmak için **Test** ' e tıklayın.
1. Runbook 'ta parametreler varsa, bunlar sol bölmede listelenir ve burada test için kullanılacak değerleri sağlayabilirsiniz.
1. Bir [karma runbook çalışanı](automation-hybrid-runbook-worker.md)üzerinde testi çalıştırmak Istiyorsanız, **çalışma ayarlarını** **karma çalışan** olarak değiştirin ve hedef grubun adını seçin.  Aksi takdirde, testi bulutta çalıştırmak için varsayılan **Azure** 'u saklayın.
1. Testi başlatmak için **Başlat** ' a tıklayın.
1. Test edilirken bir [PowerShell Iş akışını](automation-runbook-types.md#powershell-workflow-runbooks) veya [grafik](automation-runbook-types.md#graphical-runbooks) runbook 'u durdurmak veya askıya almak için çıkış bölmesi altındaki düğmeleri kullanabilirsiniz. Bir runbook'u askıya aldığınızda, askıya alınmadan önce geçerli etkinliği tamamlar. Runbook askıya alındığında, durdurabilir veya yeniden başlatabilirsiniz.
1. Çıkış bölmesinde runbook 'tan çıktıyı inceleyin.

## <a name="publish-a-runbook"></a>Runbook yayımlama

Yeni bir runbook oluşturduğunuzda veya içeri aktardığınızda, çalıştırmadan önce onu yayımlamanız gerekir. Azure Otomasyonu 'ndaki her runbook 'un bir taslak sürümü ve yayımlanmış bir sürümü vardır. Yalnızca Yayımlanan sürüm çalıştırılabilir ve yalnızca Taslak sürüm düzenlenebilir. Yayımlanan sürüm Taslak sürümdeki herhangi bir değişiklikten etkilenmez. Taslak sürümü kullanılabilir hale getirilmeli hale geldiğinde, geçerli yayımlanmış sürümün üzerine bir taslak sürümü yazarak onu yayımlayın.

### <a name="publish-a-runbook-in-the-azure-portal"></a>Azure portal runbook yayımlama

1. Runbook 'u Azure portal açın.
2. **Düzenle**’ye tıklayın.
3. Doğrulama iletisine yanıt olarak **Yayımla** ' ya ve ardından **Evet** ' e tıklayın.

### <a name="publish-a-runbook-using-powershell"></a>PowerShell kullanarak runbook yayımlama

Runbook 'unuzu yayımlamak için [Publish-AzAutomationRunbook](https://docs.microsoft.com/powershell/module/Az.Automation/Publish-AzAutomationRunbook?view=azps-3.5.0) cmdlet 'ini kullanın. 

```azurepowershell-interactive
$automationAccountName =  "AutomationAccount"
$runbookName = "Sample_TestRunbook"
$RGName = "ResourceGroup"

Publish-AzAutomationRunbook -AutomationAccountName $automationAccountName `
-Name $runbookName -ResourceGroupName $RGName
```

## <a name="schedule-a-runbook-in-the-azure-portal"></a>Azure portal runbook 'u zamanlama

Runbook 'u yayımlandığında, işlem için zamanlayabilirsiniz:

1. Runbook 'u Azure portal açın.
2. **Kaynaklar**altında **zamanlamalar** ' ı seçin.
3. **Zamanlama Ekle ' yi**seçin.
4. Runbook 'U zamanla bölmesinde, **runbook 'una bir zamanlama bağla**' yı seçin.
5. Zamanlama bölmesinde **Yeni bir zamanlama oluştur ' a** tıklayın.
6. Yeni zamanlama bölmesine bir ad, açıklama ve diğer parametreleri girin. 
7. Zamanlama oluşturulduktan sonra, vurgulayın ve **Tamam**' a tıklayın. Artık runbook 'ıza bağlanmalıdır.
8. Runbook 'un durumunu bildirmek için posta kutunuzda bir e-posta arayın.

## <a name="obtain-job-statuses"></a>İş durumlarını al

### <a name="view-statuses-in-the-azure-portal"></a>Azure portal durumları görüntüleme

Azure Automation 'da iş işleme ayrıntıları [işler](automation-runbook-execution.md#jobs)' de verilmiştir. Runbook işlerinizi görmeyi hazırsanız Azure portal kullanın ve otomasyon hesabınıza erişin. Sağ tarafta, **Iş istatistikleri**' nde tüm runbook işlerinin özetini görebilirsiniz. 

![İş Istatistikleri kutucuğu](./media/manage-runbooks/automation-account-job-status-summary.png)

Özet, yürütülen her iş için iş durumunun bir sayısını ve grafik temsilini görüntüler.

Kutucuğa tıkladığınızda, yürütülen tüm işlerin özetlenen listesini içeren Işler sayfası sunulmaktadır. Bu sayfada her iş için durum, Runbook adı, başlangıç saati ve tamamlanma zamanı gösterilir.

![Otomasyon hesabı Işleri sayfası](./media/manage-runbooks/automation-account-jobs-status-blade.png)

İş listesini **filtre işleri**' ni seçerek filtreleyebilirsiniz. Belirli bir runbook 'u, iş durumunu veya açılan listeden bir seçimi filtreleyin ve arama için zaman aralığını belirtin.

![İş durumunu filtrele](./media/manage-runbooks/automation-account-jobs-filter.png)

Alternatif olarak, belirli bir runbook için iş Özeti ayrıntılarını Otomasyon hesabınızdaki runbook 'Lar sayfasından seçip **işler**' i seçerek görüntüleyebilirsiniz. Bu eylem Işler sayfasını gösterir. Buradan, ayrıntılarını ve çıktısını görüntülemek için bir iş kaydına tıklayabilirsiniz.

![Otomasyon hesabı Işleri sayfası](./media/manage-runbooks/automation-runbook-job-summary-blade.png)

### <a name="retrieve-job-statuses-using-powershell"></a>PowerShell kullanarak iş durumlarını alma

Bir runbook için oluşturulan işleri ve belirli bir işin ayrıntılarını almak için [Get-AzAutomationJob](https://docs.microsoft.com/powershell/module/Az.Automation/Get-AzAutomationJob?view=azps-3.7.0) cmdlet 'ini kullanın. Kullanarak `Start-AzAutomationRunbook`bir runbook başlatırsanız, sonuçta elde edilen işi döndürür. İş çıktısını almak için [Get-Azautomationjoi put](https://docs.microsoft.com/powershell/module/Az.Automation/Get-AzAutomationJobOutput?view=azps-3.5.0) komutunu kullanın.

Aşağıdaki örnek, örnek bir runbook 'un son işini alır ve durumunu, runbook parametreleri için belirtilen değerleri ve iş çıktısını görüntüler.

```azurepowershell-interactive
$job = (Get-AzAutomationJob –AutomationAccountName "MyAutomationAccount" `
–RunbookName "Test-Runbook" -ResourceGroupName "ResourceGroup01" | sort LastModifiedDate –desc)[0]
$job.Status
$job.JobParameters
Get-AzAutomationJobOutput -ResourceGroupName "ResourceGroup01" `
–AutomationAccountName "MyAutomationAcct" -Id $job.JobId –Stream Output
```

Aşağıdaki örnek, belirli bir işin çıkışını alır ve her bir kaydı döndürür. Kayıtlardan biri için bir [özel durum](automation-runbook-execution.md#exceptions) varsa, komut dosyası değeri yerine özel durumu yazar. Bu davranış, özel durumlar çıkış sırasında normalde günlüğe kaydedilmeyeceğini daha fazla bilgi sağlayamadığı için yararlıdır.

```azurepowershell-interactive
$output = Get-AzAutomationJobOutput -AutomationAccountName <AutomationAccountName> -Id <jobID> -ResourceGroupName <ResourceGroupName> -Stream "Any"
foreach($item in $output)
{
    $fullRecord = Get-AzAutomationJobOutputRecord -AutomationAccountName <AutomationAccountName> -ResourceGroupName <ResourceGroupName> -JobId <jobID> -Id $item.StreamRecordId
    if ($fullRecord.Type -eq "Error")
    {
        $fullRecord.Value.Exception
    }
    else
    {
    $fullRecord.Value
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

* Runbook işlemesi hakkında bilgi edinmek için bkz. [Azure Otomasyonu 'Nda runbook yürütmesi](automation-runbook-execution.md).
* PowerShell ve PowerShell Iş akışı runbook 'larını metinsel düzenleyiciyle düzenleme hakkında daha fazla bilgi edinmek için bkz. [Azure Automation 'da metin runbook 'Larını düzenleme](automation-edit-textual-runbook.md).
* Grafik runbook 'u yazma hakkında daha fazla bilgi için bkz. [Azure Otomasyonu 'Nda grafik yazma](automation-graphical-authoring-intro.md).
