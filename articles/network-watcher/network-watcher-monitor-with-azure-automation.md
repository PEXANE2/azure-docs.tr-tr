---
title: Sorun giderme ve VPN ağ geçitlerini izleme - Azure Otomasyonu
titleSuffix: Azure Network Watcher
description: Bu makalede, Azure Otomasyonu ve Ağ İzleyicisi ile şirket içi bağlantı nın nasıl tanılandığı açıklanmaktadır
services: network-watcher
documentationcenter: na
author: damendo
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: damendo
ms.openlocfilehash: 74c9f44ff5fbbbb50bba1594d371633fd49857eb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76845043"
---
# <a name="monitor-vpn-gateways-with-network-watcher-troubleshooting"></a>Ağ İzleyicisorun giderme ile VPN ağ geçitlerini izleme

Müşterilere güvenilir hizmetler sağlamak için ağ performansınız hakkında derin bilgiler edinmek çok önemlidir. Bu nedenle, ağ kesintisi koşullarını hızlı bir şekilde algılamak ve kesinti durumunu azaltmak için düzeltici eylemde bulunmanız çok önemlidir. Azure Otomasyonu, bir görevi çalışma kitapları aracılığıyla programlı bir şekilde uygulamanızı ve çalıştırmanızı sağlar. Azure Otomasyonu'nun kullanılması, sürekli ve proaktif ağ izleme ve uyarı yapmak için mükemmel bir reçete oluşturur.

## <a name="scenario"></a>Senaryo

Aşağıdaki görüntüdeki senaryo, vpn ağ geçidi ve tünel kullanılarak şirket içi bağlantı nın kurulduğu çok katmanlı bir uygulamadır. VPN Ağ Geçidi'nin çalışır durumda olmasını sağlamak uygulamaların performansı için çok önemlidir.

Bağlantı tüneli durumunu denetlemek için Kaynak Sorun Giderme API'sini kullanarak VPN tünelinin bağlantı durumunu denetlemek için bir komut dosyasıyla bir çalışma kitabı oluşturulur. Durum sağlıklı değilse, yöneticilere bir e-posta tetikleyicisi gönderilir.

![Senaryo örneği][scenario]

Bu senaryo şunları yapacaktır:

- Bağlantı durumunu gidermek `Start-AzureRmNetworkWatcherResourceTroubleshooting` için cmdlet çağıran bir runbook oluşturma
- Zamanlamayı runbook'a bağlama

## <a name="before-you-begin"></a>Başlamadan önce

Bu senaryoyu başlatmadan önce aşağıdaki ön koşullara sahip olmalısınız:

- Azure'da bir Azure otomasyon hesabı. Otomasyon hesabının en son modüllere ve AzureRM.Network modülüne sahip olduğundan emin olun. AzureRM.Network modülü, otomasyon hesabınıza eklemeniz gerekiyorsa modül galerisinde kullanılabilir.
- Azure Otomasyonu'nda yapılandırılan bir kimlik kimliğiniz olmalıdır. [Azure Otomasyon güvenliği](../automation/automation-security-overview.md) hakkında daha fazla bilgi edinin
- Geçerli bir SMTP sunucusu (Office 365, şirket içi e-postanız veya başka bir kişi) ve Azure Otomasyonu'nda tanımlanan kimlik bilgileri
- Azure'da yapılandırılmış bir Sanal Ağ Ağ Geçidi.
- Oturum açmaları depolamak için varolan bir kapsayıcıile varolan bir depolama hesabı.

> [!NOTE]
> Önceki resimde gösterilen altyapı illüstrasyon amaçlıdır ve bu makalede yer alan adımlarla oluşturulmamıştır.

### <a name="create-the-runbook"></a>Runbook'u oluşturma

Örneği yapılandırmanın ilk adımı runbook'u oluşturmaktır. Bu örnekte run-as hesabı kullanır. Çalıştıran hesaplar hakkında bilgi edinmek için [Azure Run As hesabıyla Orijinalkimlik RunBook'ları](../automation/automation-create-runas-account.md) ziyaret edin

### <a name="step-1"></a>1. Adım

[Azure portalında](https://portal.azure.com) Azure Otomasyonuna gidin ve **RunBook'ları** tıklatın

![otomasyon hesabı genel bakış][1]

### <a name="step-2"></a>2. Adım

Runbook'un oluşturma işlemini başlatmak için **runbook ekle'yi** tıklatın.

![runbooks bıçak][2]

### <a name="step-3"></a>3. Adım

**Hızlı Oluştur**altında, runbook oluşturmak için yeni **bir runbook oluştur'u** tıklatın.

![runbook bıçak ekleme][3]

### <a name="step-4"></a>4. Adım

Bu adımda, runbook'a bir ad veririz, örnekte **Get-VPNGatewayStatus**olarak adlandırılır. Runbook açıklayıcı bir ad vermek önemlidir ve standart PowerShell adlandırma standartlarına uygun bir ad verilmesi önerilir. Bu örnek için runbook türü **PowerShell,** diğer seçenekler Grafik, PowerShell iş akışı ve Grafik PowerShell iş akışı vardır.

![runbook bıçak][4]

### <a name="step-5"></a>5. Adım

Bu adımda runbook oluşturulur, aşağıdaki kod örneği örnek için gerekli tüm kodu sağlar. Kodda değer \<\> içeren öğelerin aboneliğinizdeki değerlerle değiştirilmesi gerekir.

**Kaydet'i** tıklatırken aşağıdaki kodu kullanma

```powershell
# Set these variables to the proper values for your environment
$o365AutomationCredential = "<Office 365 account>"
$fromEmail = "<from email address>"
$toEmail = "<to email address>"
$smtpServer = "<smtp.office365.com>"
$smtpPort = 587
$runAsConnectionName = "<AzureRunAsConnection>"
$subscriptionId = "<subscription id>"
$region = "<Azure region>"
$vpnConnectionName = "<vpn connection name>"
$vpnConnectionResourceGroup = "<resource group name>"
$storageAccountName = "<storage account name>"
$storageAccountResourceGroup = "<resource group name>"
$storageAccountContainer = "<container name>"

# Get credentials for Office 365 account
$cred = Get-AutomationPSCredential -Name $o365AutomationCredential

# Get the connection "AzureRunAsConnection "
$servicePrincipalConnection=Get-AutomationConnection -Name $runAsConnectionName

"Logging in to Azure..."
Connect-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $servicePrincipalConnection.TenantId `
    -ApplicationId $servicePrincipalConnection.ApplicationId `
    -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint
"Setting context to a specific subscription"
Set-AzureRmContext -SubscriptionId $subscriptionId

$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq $region }
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName
$connection = Get-AzureRmVirtualNetworkGatewayConnection -Name $vpnConnectionName -ResourceGroupName $vpnConnectionResourceGroup
$sa = Get-AzureRmStorageAccount -Name $storageAccountName -ResourceGroupName $storageAccountResourceGroup 
$storagePath = "$($sa.PrimaryEndpoints.Blob)$($storageAccountContainer)"
$result = Start-AzureRmNetworkWatcherResourceTroubleshooting -NetworkWatcher $networkWatcher -TargetResourceId $connection.Id -StorageId $sa.Id -StoragePath $storagePath

if($result.code -ne "Healthy")
    {
        $body = "Connection for $($connection.name) is: $($result.code) `n$($result.results[0].summary) `nView the logs at $($storagePath) to learn more."
        Write-Output $body
        $subject = "$($connection.name) Status"
        Send-MailMessage `
        -To $toEmail `
        -Subject $subject `
        -Body $body `
        -UseSsl `
        -Port $smtpPort `
        -SmtpServer $smtpServer `
        -From $fromEmail `
        -BodyAsHtml `
        -Credential $cred
    }
else
    {
    Write-Output ("Connection Status is: $($result.code)")
    }
```

### <a name="step-6"></a>6. Adım

Runbook kaydedildikten sonra, runbook'un başlangıcını otomatikleştirmek için bir zamanlamanın ona bağlanması gerekir. İşlemi başlatmak için **Zamanlama'yı**tıklatın.

![6. Adım][6]

## <a name="link-a-schedule-to-the-runbook"></a>Zamanlamayı runbook'a bağlama

Yeni bir zamanlama oluşturulmalıdır. **Bir programı runbook'unuza Bağla'yı**tıklatın.

![7. Adım][7]

### <a name="step-1"></a>1. Adım

**Zamanlama** çubuğunda **yeni bir zamanlama oluştur'u** tıklatın

![8. Adım][8]

### <a name="step-2"></a>2. Adım

Yeni **Zamanlama'da** zamanlama bilgilerini doldurun. Ayarlanabilecek değerler aşağıdaki listededir:

- **Adı** - Programın dostu adı.
- **Açıklama** - Zamanlamanın açıklaması.
- **Başlangıçlar** - Bu değer, zamanlamanın tetiklediği zamanı oluşturan tarih, saat ve saat diliminin birleşimidir.
- **Yineleme** - Bu değer zamanlama yinelemesini belirler.  Geçerli değerler **Bir Kez** veya **Yinelenen.**
- Her - Saat, gün, hafta veya ay içinde zamanlamanın yineleme **aralığı.**
- **Son Kullanma Tarihi Ayarla** - Değer, zamanlamanın süresinin dolup bitmeyeceğini belirler. **Evet** veya **Hayır**olarak ayarlanabilir. Evet seçilirse geçerli bir tarih ve saat sağlanacaktır.

> [!NOTE]
> Her saatten daha sık çalışan bir runbook'un olması gerekiyorsa, farklı aralıklarla birden çok zamanlama oluşturulmalıdır (yani, 15, 30, 45 dakika sonra saat)

![9. Adım][9]

### <a name="step-3"></a>3. Adım

Zamanlamayı runbook'a kaydetmek için Kaydet'i tıklatın.

![Adım 10][10]

## <a name="next-steps"></a>Sonraki adımlar

Artık Ağ İzleyicisorun giderme olayını Azure Otomasyonu ile nasıl entegre edebilirsiniz, [Azure Ağ İzleyicisi ile uyarı tetiklenen paket yakalama oluştur'u](network-watcher-alert-triggered-packet-capture.md)ziyaret ederek VM uyarılarında paket yakalamaları nasıl tetikleyeceğimiz öğrenin.

<!-- images -->
[scenario]: ./media/network-watcher-monitor-with-azure-automation/scenario.png
[1]: ./media/network-watcher-monitor-with-azure-automation/figure1.png
[2]: ./media/network-watcher-monitor-with-azure-automation/figure2.png
[3]: ./media/network-watcher-monitor-with-azure-automation/figure3.png
[4]: ./media/network-watcher-monitor-with-azure-automation/figure4.png
[5]: ./media/network-watcher-monitor-with-azure-automation/figure5.png
[6]: ./media/network-watcher-monitor-with-azure-automation/figure6.png
[7]: ./media/network-watcher-monitor-with-azure-automation/figure7.png
[8]: ./media/network-watcher-monitor-with-azure-automation/figure8.png
[9]: ./media/network-watcher-monitor-with-azure-automation/figure9.png
[10]: ./media/network-watcher-monitor-with-azure-automation/figure10.png
