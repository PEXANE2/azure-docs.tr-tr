---
title: Azure Otomasyonu'nda PowerShell runbook oluşturma
description: Basit bir PowerShell runbook'u oluşturma, test etme ve yayımlama yı gösteren öğretici.
keywords: azure powershell, powershell betik öğreticisi, powershell otomasyonu
services: automation
ms.subservice: process-automation
ms.date: 04/19/2020
ms.topic: tutorial
ms.openlocfilehash: b94969ff0973f68b57a1f43aa9d3205901bb1436
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81726161"
---
# <a name="tutorial-create-a-powershell-runbook"></a>Öğretici: PowerShell runbook oluşturma

Bu öğretici, Azure Automation’da bir [PowerShell runbook](../automation-runbook-types.md#powershell-runbooks) oluşturulmasını adım adım göstermektedir. PowerShell runbook'ları Windows PowerShell'e dayanır. Azure portalındaki metin düzenleyicisini kullanarak runbook kodunu doğrudan siz düzenliyorsunuz.

> [!div class="checklist"]
> * Basit bir PowerShell runbook oluşturma
> * Çalışma kitabını test edin ve yayımlayın
> * Runbook işinin durumunu çalıştırma ve izleme
> * Runbook parametreleri ile bir Azure sanal makinesi başlatmak için runbook'u güncelleştirme

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma Runbook Worker'ınızdaki Az modül yükleme yönergeleri için Azure [PowerShell Modül'üne](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)bakın. Otomasyon hesabınız için, Azure Otomasyonu'nda Azure [PowerShell modüllerini nasıl güncelleştirebileceğinizi](../automation-update-azure-modules.md)kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Azure aboneliği. Henüz bir hesabınız yoksa, [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) veya ücretsiz bir [hesaba](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)kaydolabilirsiniz.
* Runbook’u tutacak ve Azure kaynaklarında kimlik doğrulamasını yapacak bir [Automation hesabı](../automation-quickstart-create-account.md). Bu hesabın sanal makineyi başlatma ve durdurma izni olmalıdır.
* Azure sanal makinesi. Bu makineyi durup çalıştırdığınıza göre, bu bir üretim VM'si olmamalı.
* Gerekirse Azure [modüllerini içe aktarın](../shared-resources/modules.md) veya modülleri kullandığınız cmdlet'lere göre [güncelleştirin.](../automation-update-azure-modules.md)

## <a name="differences-from-powershell-workflow-runbooks"></a>PowerShell İş Akışı çalışma kitaplarından farklar

PowerShell runbook'lar, PowerShell İş Akışı runbook'larıyla aynı yaşam döngüsüne, yeteneklere ve yönetime sahiptir. Ancak, bazı farklılıklar ve sınırlamalar vardır.

| Özellik  | PowerShell Runbooks | PowerShell İş Akışı Runbook'ları |
| ------ | ----- | ----- |
| Hız | Derleme adımı kullanmadıkları için hızlı çalıştırın. | Daha yavaş koş. |
| Kontrol noktaları | Kontrol noktalarını desteklemeyin. PowerShell runbook yalnızca en başından itibaren çalışmaya devam edebilir. | Çalışma kitabının herhangi bir noktadan çalışmaya devam etmesine izin veren denetim noktalarını kullanın. |
| Komut Yürütme | Yalnızca seri yürütmeyi destekleyin. | Hem seri hem de paralel yürütmeyi destekleyin.|
| Çalışma Alanı | Tek bir runspace her şeyi bir komut dosyasında çalıştırın. | Ayrı bir çalışma alanı bir etkinlik, komut veya komut dosyası bloğu için kullanılabilir. |

Bu farklılıklara ek olarak, PowerShell runbook'ların PowerShell İş Akışı runbook'larından bazı [sözdizimfarklılıkları](https://technet.microsoft.com/magazine/dn151046.aspx) vardır.

## <a name="step-1---create-runbook"></a>1. Adım - Runbook oluşturma

Metni `Hello World`çıktılayan basit bir runbook oluşturarak başlayın.

1. Azure portalında, Otomasyon hesabınızı açın.

2. Runbook listesini açmak için **Proses Otomasyonu** altındaki **Runbook'ları** seçin.

3. **Runbook Oluştur'u**seçerek yeni bir runbook oluşturun.

4. Runbook’a **MyFirstRunbook-PowerShell** adını verin.

5. Bu durumda, bir [PowerShell runbook](../automation-runbook-types.md#powershell-runbooks)oluşturmak için gidiyoruz. **Runbook türü**için **PowerShell'i** seçin.

6. Runbook’u oluşturmak için **Oluştur**’a tıklayın ve metin düzenleyicisini açın.

## <a name="step-2---add-code-to-the-runbook"></a>2. Adım - Runbook'a kod ekleme

Kodu doğrudan runbook’a yazabilir veya Kitaplık denetiminde cmdlet’leri, runbook’ları ve varlıkları seçebilir ve ilgili parametrelerle bunların runbook’a eklenmesini sağlayabilirsiniz. Bu öğretici için, kodu doğrudan runbook'a yazacaksın.

1. Runbook'unşu anda boş. Komut `Write-Output "Hello World"` dosyasının gövdesini yazın.

   ![Hello World](../media/automation-tutorial-runbook-textual-powershell/automation-helloworld.png)

2. **Kaydet**’e tıklayarak runbook’u kaydedin.

## <a name="step-3---test-the-runbook"></a><a name="step-3---test-the-runbook"> </a> Adım 3 - Runbook test edin

Üretimde kullanılabilir hale getirmek için runbook'u yayımlamadan önce, düzgün çalıştığından emin olmak için kitabı sınamalısınız. Bir runbook'u test etmek Taslak sürümünü çalıştırIr ve çıktısını etkileşimli olarak görüntülemenize olanak tanır.

1. Test bölmesini açmak için **Test bölmesi**’ne tıklayın.

2. Testi başlatmak için **Başlat**’a tıklayın. Etkinleştirilen tek seçenek bu olmalıdır.

3. [Runbook işinin](../automation-runbook-execution.md) oluşturulduğunu ve durumunun bölmede görüntülendiğini unutmayın.

   İş durumu, işin buluttaki bir runbook çalışanının kullanılabilir olmasını beklediğini belirten Sıraya olarak başlar. Durum, bir işçi işi talep ettiğinde Başlangıç olarak değişir. Son olarak, runbook gerçekten çalışmaya başladığında durum Çalışma olur.

4. Runbook işi tamamlandığında, Test bölmesi çıktısını görüntüler. Bu durumda, görüyorsunuz `Hello World`.

   ![Test Bölmesi Çıktısı](../media/automation-tutorial-runbook-textual-powershell/automation-testpane-output.png)

5. Tuvale geri dönmek için Test bölmesini kapatın.

## <a name="step-4---publish-and-start-the-runbook"></a>4. Adım - Runbook’u yayımlama ve başlatma

Oluşturduğunuz runbook hala Taslak modundadır. Üretimde çalıştırılabilmesi için yayımlanması gerekir. Bir runbook yayımladığınızda, Taslak sürümü mevcut Yayımlanmış sürümün üzerine yazarsınız. Örneğimizde, runbook’u henüz oluşturduğunuzdan, Yayımlanmış sürümünüz yok.

1. Runbook’u yayımlamak için **Yayımla**’ya tıklayın ve sorulduğunda **Evet**’e tıklayın.

2. Runbook sayfasındarunbook'u görüntülemek için sola kaydırın ve **Yazarlık Durumu** değerinin **Yayımlanmış**olarak ayarlandığına dikkat edin.

3. **MyFirstRunbook-PowerShell**için sayfayı görüntülemek için sağa geri kaydırın.
   
   Üstteki seçenekler, runbook'u şimdi başlatmanızı, gelecekteki bir başlangıç saatini zamanlamanızı veya runbook'un http araması yoluyla başlatılabilmesi için bir [webhook](../automation-webhooks.md) oluşturmanıza olanak sağlar.

4. **Runbook'u** başlatmak istendiğinde Başlat ve ardından **Evet'i** seçin. 

5. Oluşturulan runbook işi için bir İş bölmesi açılır. Bu bölmeyi kapatabilmenize rağmen, işin ilerlemesini izleyebilmeniz için hemen açık bırakın. İş durumu İş **Özeti'nde**gösterilir ve olası durumlar runbook'u sınatıniçin açıklandığı şekilde tanımlanır.

   ![İş Özeti](../media/automation-tutorial-runbook-textual-powershell/job-pane-status-blade-jobsummary.png)

6. Runbook durumu Tamamlandı'yı gösterdikten sonra, görüntülenen görmek `Hello World` için Çıktı sayfasını açmak için **Çıktı'yı** tıklatın.

   ![İş Çıktısı](../media/automation-tutorial-runbook-textual-powershell/job-pane-status-blade-outputtile.png)

7. Çıktı sayfasını kapatın.

8. Runbook işine ait Akışlar bölmesini açmak için **Tüm Günlükler**’e tıklayın. Yalnızca çıkış `Hello World` akışında görmeniz gerekir.

    Runbook onlara yazıyorsa, Akışlar bölmesinin verbose ve hata akışları gibi bir runbook işi için diğer akışları gösterebileceğini unutmayın.

   ![Tüm Günlükler](../media/automation-tutorial-runbook-textual-powershell/job-pane-status-blade-alllogstile.png)

9. MyFirstRunbook-PowerShell sayfasına dönmek için Akışlar bölmesini ve İş bölmesini kapatın.

10. **Ayrıntılar**altında, bu runbook için İşler sayfasını açmak için **İşler'i** tıklatın. Bu sayfa, runbook tarafından oluşturulan tüm işleri listeler. İşi yalnızca bir kez çalıştırdığınızdan, yalnızca bir iş listelenirken görmeniz gerekir.

   ![İş Listesi](../media/automation-tutorial-runbook-textual-powershell/runbook-control-job-tile.png)

11. Runbook'u başlattığınızda görüntülediğiniz aynı İş bölmesini açmak için iş adını tıklatın. Runbook için oluşturulan herhangi bir işin ayrıntılarını görüntülemek için bu bölmeyi kullanın.

## <a name="step-5---add-authentication-to-manage-azure-resources"></a>5. Adım- Azure kaynaklarını yönetmek için kimlik doğrulaması ekleme

Runbook uygulamanızı test ettiniz ve yayımladınız, ancak şu ana kadar faydalı bir şey yapmadı. Bu runbook’un Azure kaynaklarını yönetmesini istiyorsunuz. Bunu yapmak için runbook'un Otomasyon hesabınızı oluşturduğunuzda otomatik olarak oluşturulan Çalıştır hesabını kullanarak kimliğini doğrulayababilmesi gerekir.

Aşağıdaki örnekte gösterildiği gibi, [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-3.5.0) cmdlet ile Run As bağlantısı yapılır. Kaynakları birden çok abonelik te yönetiyorsanız, `AzContext` [Get-AzContext](https://docs.microsoft.com/powershell/module/Az.Accounts/Get-AzContext?view=azps-3.5.0)ile parametreyi kullanmanız gerekir.

> [!NOTE]
> PowerShell runbook'ları için `Add-AzAccount` ve `Add-AzureRMAccount` `Connect-AzAccount`diğer adlar . Bu cmdlets'i kullanabilir veya Otomasyon hesabınızdaki [modüllerinizi](../automation-update-azure-modules.md) en son sürümlere güncelleyebilirsiniz. Yeni bir Otomasyon hesabı oluşturmuş olsanız bile modüllerinizi güncellemeniz gerekebilir.

   ```powershell
   # Ensures you do not inherit an AzContext in your runbook
   Disable-AzContextAutosave –Scope Process

   $connection = Get-AutomationConnection -Name AzureRunAsConnection

   # Wrap authentication in retry logic for transient network failures
   $logonAttempt = 0
   while(!($connectionResult) -And ($logonAttempt -le 10))
   {
       $LogonAttempt++
       # Logging in to Azure...
       $connectionResult =    Connect-AzAccount `
                                  -ServicePrincipal `
                                  -Tenant $connection.TenantID `
                                  -ApplicationId $connection.ApplicationID `
                                  -CertificateThumbprint $connection.CertificateThumbprint

       Start-Sleep -Seconds 30
   }

   $AzureContext = Get-AzSubscription -SubscriptionId $connection.SubscriptionID

   Get-AzVM -ResourceGroupName myResourceGroup -AzContext $AzureContext
   ```

1. MyFirstRunbook-PowerShell sayfasında **Edit'e** tıklayarak metin düzenleyicisini açın.

2. Artık çizgiye `Write-Output` ihtiyacın yok. Devam et ve sil.

3. Otomasyon Çalıştır hesabınızla kimlik doğrulamasını işleyen aşağıdaki kodu yazın veya kopyalayın ve yapıştırın.

   ```powershell
   # Ensures you do not inherit an AzContext in your runbook
   Disable-AzContextAutosave –Scope Process

   $connection = Get-AutomationConnection -Name AzureRunAsConnection

   while(!($connectionResult) -And ($logonAttempt -le 10))
   {
       $LogonAttempt++
       # Logging in to Azure...
       $connectionResult =    Connect-AzAccount `
                                  -ServicePrincipal `
                                  -Tenant $connection.TenantID `
                                  -ApplicationId $connection.ApplicationID `
                                  -CertificateThumbprint $connection.CertificateThumbprint

       Start-Sleep -Seconds 30
   }
   ```

4. Çalışma kitabını test etmek için **Test bölmesini** tıklatın.

5. Testi başlatmak için **Başlat**’a tıklayın. Tamamlandıktan sonra, hesabınızdan temel bilgileri görüntüleyen aşağıdakilere benzer çıktıgörmeniz gerekir. Bu çıktı, Hesap Olarak Çalıştır'ın geçerli olduğunu doğrular.

   ![Kimlik doğrulaması](../media/automation-tutorial-runbook-textual-powershell/runbook-auth-output.png)

## <a name="step-6---add-code-to-start-a-virtual-machine"></a>6. Adım - Sanal makineyi başlatmak için kod ekleme

Runbook'unuzun Azure aboneliğiniz için kimliğinizi doğruladığınıza göre, kaynakları yönetebilirsiniz. Sanal bir makine başlatmak için bir komut ekleyelim. Azure aboneliğinizdeki herhangi bir sanal makineyi seçebilir ve şimdilik runbook'ta bu adı sabit kodlayabilirsiniz.

1. Runbook komut dosyanıza, sanal makineyi başlatmak için [Start-AzVM](https://docs.microsoft.com/powershell/module/Az.Compute/Start-AzVM?view=azps-3.5.0) cmdlet'i ekleyin. Aşağıda gösterildiği gibi, cmdlet adı `VMName` ve adlı `ResourceGroupName`bir kaynak grubu ile sanal bir makine başlar.

   ```powershell
   # Ensures you do not inherit an AzContext in your runbook
   Disable-AzContextAutosave –Scope Process

   $connection = Get-AutomationConnection -Name AzureRunAsConnection
   while(!($connectionResult) -And ($logonAttempt -le 10))
   {
       $LogonAttempt++
       # Logging in to Azure...
       $connectionResult =    Connect-AzAccount `
                                  -ServicePrincipal `
                                  -Tenant $connection.TenantID `
                                  -ApplicationId $connection.ApplicationID `
                                  -CertificateThumbprint $connection.CertificateThumbprint

       Start-Sleep -Seconds 30
   }

   Start-AzVM -Name 'VMName' -ResourceGroupName 'ResourceGroupName'
   ```

2. Runbook'u kaydedin ve test etmek için **Test bölmesini** tıklatın.

3. Teste başlamak için **Başlat'ı** tıklatın. Tamamlandıktan sonra, sanal makinenin başlatıldıktan emin olun.

## <a name="step-7---add-an-input-parameter"></a>Adım 7 - Giriş parametresi ekleme

Runbook'unuzun çalışma kitabında sabit kodladığınız sanal makineyi başlatması şu anda. Runbook başlatıldığında sanal makineyi belirtirseniz runbook daha kullanışlıdır. Bu işlevselliği sağlamak için runbook'a giriş parametreleri ekleyelim.

1. Metin düzenleyicisinde cmdlet'i parametreler `Start-AzVM` `VMName` için değişkenler kullanacak `ResourceGroupName`şekilde değiştirin ve . 

   ```powershell
   Param(
    [string]$VMName,
    [string]$ResourceGroupName
   )
   # Ensures you do not inherit an AzContext in your runbook
   Disable-AzContextAutosave –Scope Process

   $connection = Get-AutomationConnection -Name AzureRunAsConnection
   while(!($connectionResult) -And ($logonAttempt -le 10))
   {
       $LogonAttempt++
       # Logging in to Azure...
       $connectionResult =    Connect-AzAccount `
                                  -ServicePrincipal `
                                  -Tenant $connection.TenantID `
                                  -ApplicationId $connection.ApplicationID `
                                  -CertificateThumbprint $connection.CertificateThumbprint

       Start-Sleep -Seconds 30
   }

   Start-AzVM -Name $VMName -ResourceGroupName $ResourceGroupName
   ```

2. Runbook'u kaydedin ve Test bölmesini açın. Şimdi testte kullanılacak olan iki girdi değişkeni için değerleri sağlayabilirsiniz.

3. Test bölmesini kapatın.

4. Runbook’un yeni sürümünü yayımlamak için **Yayımla**’ya tıklayın.

5. Daha önce başlattığınız sanal makineyi durdurun.

6. Runbook'u başlatmak için **Başlat**’a tıklayın. 

7. Başlatacağınız sanal makine için **VMNAME** ve **RESOURCEGROUPNAME** değerlerini yazın ve **ardından Tamam'ı**tıklatın.

    ![Parametre Geçir](../media/automation-tutorial-runbook-textual-powershell/automation-pass-params.png)

8. Runbook tamamlandığında, sanal makinenin başlatıldığından emin olun.

## <a name="next-steps"></a>Sonraki adımlar

* Dil referansı ve öğrenme modülleri de dahil olmak üzere PowerShell hakkında daha fazla bilgi için [PowerShell Dokümanları'na](/powershell/scripting/overview)bakın.
* PowerShell cmdlet referansı için [Az.Automation'a](https://docs.microsoft.com/powershell/module/az.automation/?view=azps-3.7.0#automation
)bakın.
* Grafik çalışma kitaplarıyla başlamak için [bkz.](automation-tutorial-runbook-graphical.md)
* PowerShell İş Akışı runbook'larına başlamak için [powershell iş akışı runbook'u oluştur'a](automation-tutorial-runbook-textual.md)bakın.
* Runbook türleri ve bunların avantajları ve sınırlamaları hakkında daha fazla bilgi için [Azure Otomasyon runbook türlerine](../automation-runbook-types.md)bakın.
* PowerShell komut dosyası desteği özelliği hakkında daha fazla bilgi için [Azure Otomasyonu'nda Yerel PowerShell komut dosyası desteğine](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/)bakın.
