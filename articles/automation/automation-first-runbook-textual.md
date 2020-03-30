---
title: Azure Otomasyonundaki ilk PowerShell İş Akışı çalışma kitabım
description: PowerShell İş Akışı kullanarak basit bir runbook oluşturma, test etme ve yayımlama adımlarını anlatan öğretici.
keywords: powershell iş akışı, powershell iş akışı örnekleri, iş akışı powershell
services: automation
ms.subservice: process-automation
ms.date: 09/24/2018
ms.topic: conceptual
ms.openlocfilehash: a5a1cad3179063f75a5d9a19567624180b5793a1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79367270"
---
# <a name="my-first-powershell-workflow-runbook"></a>İlk PowerShell İş Akışı runbook uygulamam

> [!div class="op_single_selector"]
> * [Grafik](automation-first-runbook-graphical.md)
> * [Powershell](automation-first-runbook-textual-powershell.md)
> * [PowerShell İş Akışı](automation-first-runbook-textual.md)
> * [Python](automation-first-runbook-textual-python2.md)

Bu öğretici, Azure Automation’da bir [PowerShell İş Akışı runbook](automation-runbook-types.md#powershell-workflow-runbooks) oluşturulmasını adım adım göstermektedir. Runbook işinin durumunu izlemeyi öğrenirken test ettiğiniz ve yayımladığınız basit bir runbook ile başlayın. Ardından, bir Azure sanal makinesi başlatarak gösterildiği gibi Azure kaynaklarını gerçekten yönetmek için runbook'u değiştirin. Son olarak, runbook parametreleri ekleyerek runbook'u daha sağlam hale getirin.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma Runbook Worker'ınızdaki Az modül yükleme yönergeleri için Azure [PowerShell Modül'üne](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)bakın. Otomasyon hesabınız için, Azure Otomasyonu'nda Azure [PowerShell modüllerini nasıl güncelleştirebileceğinizi](automation-update-azure-modules.md)kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Azure aboneliği. Henüz bir hesabınız yoksa, [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) veya ücretsiz bir [hesaba](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)kaydolabilirsiniz.
* Runbook’u tutacak ve Azure kaynaklarında kimlik doğrulamasını yapacak bir [Automation hesabı](automation-offering-get-started.md). Bu hesabın sanal makineyi başlatma ve durdurma izni olmalıdır.
* Azure sanal makinesi. Bu makineyi durup çalıştırdığınıza göre, bu bir üretim VM'si olmamalı.

## <a name="step-1---create-new-runbook"></a>1. Adım - Yeni runbook oluşturma

Metni `Hello World`çıktılayan basit bir runbook oluşturarak başlayın.

1. Azure portalında, Otomasyon hesabınızı açın.

   Automation hesabı sayfası size bu hesaptaki kaynakların hızlı bir görünümünü sağlar. Birkaç varlığınız zaten olmalıdır. Bu varlıkların çoğu otomatik olarak yeni bir Otomasyon hesabına dahil modüllerdir. Aboneliğinizle ilişkili Kimlik Bilgisi kıymetine de sahip olmalısınız.
 
1. Runbook listesini açmak için **Proses Otomasyonu** altındaki **Runbook'ları** seçin.
1. **Runbook Oluştur'u**seçerek yeni bir runbook oluşturun.
1. Runbook’a **MyFirstRunbook-Workflow** adını verin.
1. Bu durumda, bir [PowerShell İş Akışı runbook](automation-runbook-types.md#powershell-workflow-runbooks)oluşturmak için gidiyoruz. Runbook türü için **PowerShell İş** **Akışı'nı**seçin.
1. Runbook’u oluşturmak için **Oluştur**’a tıklayın ve metin düzenleyicisini açın.

## <a name="step-2---add-code-to-the-runbook"></a>2. Adım - Runbook'a kod ekleme

Kodu doğrudan runbook'a yazabilir veya Kitaplık denetiminden cmdlets, runbook ve varlıkları seçip ilgili parametrelerle birlikte runbook'a ekleyebilirsiniz. Bu öğretici için, kodu doğrudan runbook'a yazarsınız.

1. Runbook'unuz şu anda `Workflow` yalnızca gerekli anahtar kelime, runbook'un adı ve tüm iş akışını kaplayan ayraçlarla boş.

   ```powershell-interactive
   Workflow MyFirstRunbook-Workflow
   {
   }
   ```

1. Ayraçlar arasında yazın. `Write-Output "Hello World"`

   ```powershell-interactive
   Workflow MyFirstRunbook-Workflow
   {
   Write-Output "Hello World"
   }
   ```

1. **Kaydet**’e tıklayarak runbook’u kaydedin.

## <a name="step-3---test-the-runbook"></a>3. Adım - Runbook'u test etme

Üretimde kullanılabilir hale getirmek için runbook'u yayımlamadan önce, düzgün çalıştığından emin olmak için kitabı sınamalısınız. Bir runbook'u test etmek Taslak sürümünü çalıştırIr ve çıktısını etkileşimli olarak görüntülemenize olanak tanır.

1. Test bölmesini açmak için **Test bölmesini** seçin.
1. Tek etkin seçeneği test etmekle testi başlatmak için **Başlat'ı** tıklatın.
1. [Runbook işinin](automation-runbook-execution.md) oluşturulduğunu ve durumunun bölmede görüntülendiğini unutmayın.

   İş durumu , `Queued`işin buluttaki bir runbook çalışanının kullanılabilir olmasını beklediğini belirten olarak başlar. Durum, bir `Starting` işçinin işi talep etmesiyle değişir. Son olarak, `Running` durum runbook gerçekten çalıştırmaya başladığında olur.

1. Runbook işi tamamlandığında, Test bölmesi çıktısını görüntüler. Bu durumda, görüyorsunuz `Hello World`.

   ![Hello World](media/automation-first-runbook-textual/test-output-hello-world.png)

1. Tuvale geri dönmek için Test bölmesini kapatın.

## <a name="step-4---publish-and-start-the-runbook"></a>4. Adım - Runbook’u yayımlama ve başlatma

Oluşturduğunuz runbook hala Taslak modundadır. Üretimde çalıştıramadan önce yayımlamalısınız. Bir runbook yayımladığınızda, Taslak sürümü mevcut Yayımlanmış sürümün üzerine yazarsınız. Örneğimizde, runbook’u henüz oluşturduğunuzdan, Yayımlanmış sürümünüz yok.

1. Runbook’u yayımlamak için **Yayımla**’ya tıklayın ve sorulduğunda **Evet**’e tıklayın.
1. **Runbook** sayfasındaki runbook'u görüntülemek için sola kaydırın ve **Yazar Durumu** alanının **Yayımlanmış**olarak ayarlandığına dikkat edin.
1. **MyFirstRunbook-Workflow**için sayfayı görüntülemek için sağa geri kaydırın.

   Üstteki seçenekler, runbook'u şimdi başlatmanızı, gelecekteki bir başlangıç saatini zamanlamanızı veya runbook'un http araması yoluyla başlatılabilmesi için bir [webhook](automation-webhooks.md) oluşturmanıza olanak sağlar.

1. **Runbook'u** başlatmak istendiğinde Başlat ve ardından **Evet'i** seçin.

   ![Runbook’u başlatma](media/automation-first-runbook-textual/automation-runbook-controls-start.png)

1. Oluşturulan runbook işi için bir İş bölmesi açılır. Bu durumda, işin ilerlemesini izleyebilmeniz için bölmeyi açık bırakın.

1. İş durumunun **İş Özeti'nde**gösterildiğini unutmayın. Bu durum, runbook'u sınarken gördüğünüz durumla eşleşir.

   ![İş Özeti](media/automation-first-runbook-textual/job-pane-status-blade-jobsummary.png)

1. Runbook durumu gösterdiğinde `Completed` **Çıktı'yı**tıklatın. İletinizi `Hello World` görebileceğiniz Çıktı sayfası açılır.

   ![İş Özeti](media/automation-first-runbook-textual/job-pane-status-blade-outputtile.png)

1. Çıktı sayfasını kapatın.

1. Runbook işine ait Akışlar bölmesini açmak için **Tüm Günlükler**’e tıklayın. Yalnızca çıkış `Hello World` akışında görmeniz gerekir. Runbook onlara yazıyorsa, Akışlar bölmesinin bir runbook işi için ayrıntılı ve hata akışları gibi diğer akışları gösterebileceğini unutmayın.

   ![İş Özeti](media/automation-first-runbook-textual/job-pane-status-blade-alllogstile.png)

1. **MyFirstRunbook** sayfasına dönmek için Akışlar bölmesini ve İş bölmesini kapatın.
1. Bu runbook'un İşler sayfasını açmak için **Kaynaklar** altında **İşler'i** tıklatın. Bu sayfa, runbook tarafından oluşturulan tüm işleri listeler. İşi yalnızca bir kez çalıştırdığınızdan, yalnızca bir iş listelenmiş olarak görmeniz gerekir.

   ![İşler](media/automation-first-runbook-textual/runbook-control-job-tile.png)

1. Runbook'u başlattığınızda görüntülediğiniz aynı İş bölmesini açmak için iş adını tıklatın. Runbook için oluşturulan herhangi bir işin ayrıntılarını görüntülemek için bu bölmeyi kullanın.

## <a name="step-5---add-authentication-to-manage-azure-resources"></a>5. Adım- Azure kaynaklarını yönetmek için kimlik doğrulaması ekleme

Runbook uygulamanızı test ettiniz ve yayımladınız, ancak şu ana kadar faydalı bir şey yapmadı. Bu runbook’un Azure kaynaklarını yönetmesini istiyorsunuz. Abonelik kimlik bilgilerini kullanarak kimlik doğrulaması yapmadıkça bunu yapamaz. Kimlik doğrulama `Connect-AzAccount` cmdlet kullanır.

>[!NOTE]
>PowerShell runbook'ları için `Add-AzAccount` ve `Add-AzureRMAccount` `Connect-AzAccount`diğer adlar . Bu cmdlets'i kullanabilir veya Otomasyon hesabınızdaki [modüllerinizi](automation-update-azure-modules.md) en son sürümlere güncelleyebilirsiniz. Yeni bir Otomasyon hesabı oluşturmuş olsanız bile modüllerinizi güncellemeniz gerekebilir.

1. **MyFirstRunbook-Workflow** sayfasına gidin ve **Düzenleme'yi**tıklatarak metin düzenleyicisini açın.
2. `Write-Output` Satırı silin.
3. İmleci ayraçlar arasında boş bir satıra getirin.
4. Otomasyon Çalıştır hesabınızla kimlik doğrulamasını işleyen aşağıdaki kodu yazın veya kopyalayın ve yapıştırın.

   ```powershell-interactive
   # Ensures you do not inherit an AzContext in your runbook
   Disable-AzContextAutosave –Scope Process

   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzAccount -ServicePrincipal -Tenant $Conn.TenantID `
   -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

   $AzureContext = Select-AzSubscription -SubscriptionId $Conn.SubscriptionID
   ```

1. Çalışma kitabını test etmek için **Test bölmesini** tıklatın.
1. Testi başlatmak için **Başlat**’a tıklayın. Tamamlandıktan sonra, hesabınızdan temel bilgileri görüntüleyen aşağıdakilere benzer çıktıgörmeniz gerekir. Bu eylem, kimlik bilgisinin geçerli olduğunu doğrular.

   ![Kimlik doğrulaması](media/automation-first-runbook-textual/runbook-auth-output.png)

## <a name="step-6---add-code-to-start-a-virtual-machine"></a>6. Adım - Sanal makineyi başlatmak için kod ekleme

Runbook'unuzun Azure aboneliğine kimlik tonuyla geldiğini zedeleyen kaynakları yönetebilirsiniz. Sanal bir makine başlatmak için bir komut ekleyelim. Azure aboneliğinizde herhangi bir VM seçebilirsiniz ve şimdilik runbook'ta bu adı sabit şifrelersiniz. Kaynakları birden çok abonelik te yönetiyorsanız, `AzContext` [Get-AzContext](/powershell/module/az.accounts/get-azcontext) cmdlet ile parametreyi kullanmanız gerekir.

1. Aşağıda gösterildiği gibi [Start-AzVM](https://docs.microsoft.com/powershell/module/Az.Compute/Start-AzVM?view=azps-3.5.0
) cmdlet'e bir çağrı girerek başlamak için VM'nin adını ve kaynak grup adını sağlayın. 

   ```powershell-interactive
   workflow MyFirstRunbook-Workflow
   {
   # Ensures that you do not inherit an AzContext in your runbook
   Disable-AzContextAutosave –Scope Process

   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

   $AzureContext = Get-AzSubscription -SubscriptionId $Conn.SubscriptionID

   Start-AzVM -Name 'VMName' -ResourceGroupName 'ResourceGroupName' -AzContext $AzureContext
   }
   ```

1. Runbook'u kaydedin ve test etmek için **Test bölmesini** tıklatın.
1. Testi başlatmak için **Başlat**’a tıklayın. Tamamlandıktan sonra, VM'nin başlatıldığından kontrol edin.

## <a name="step-7---add-an-input-parameter-to-the-runbook"></a>7. Adım - Runbook'a girdi parametresi ekleme

Runbook'unuzun çalışma defterinde kodladığınız VM'yi başlatması gerekir. Runbook başlatıldığında VM'yi belirtebilirseniz daha yararlı olacaktır. Bu işlevselliği sağlamak için runbook'a giriş parametreleri ekleyelim.

1. Runbook'a `VMName` parametreler `ResourceGroupName` ve değişkenler için değişkenler ekleyin `Start-AzVM` ve aşağıda gösterildiği gibi cmdlet ile değişkenleri kullanın.

   ```powershell-interactive
   workflow MyFirstRunbook-Workflow
   {
    Param(
     [string]$VMName,
     [string]$ResourceGroupName
    )
   # Ensures you do not inherit an AzContext in your runbook
   Disable-AzContextAutosave –Scope Process

   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
   Start-AzVM -Name $VMName -ResourceGroupName $ResourceGroupName
   }
   ```

2. Runbook'u kaydedin ve Test bölmesini açın. Artık testteki iki giriş değişkeni için değerler sağlayabilirsiniz.
3. Test bölmesini kapatın.
4. Runbook’un yeni sürümünü yayımlamak için **Yayımla**’ya tıklayın.
5. Başlattığınız VM'yi durdurun.
6. Runbook'u başlatmak için **Başlat**’a tıklayın. 
7. Başlatacağınız VM için **VMNAME** ve **RESOURCEGROUPNAME** değerlerini yazın.

   ![Runbook’u Başlatma](media/automation-first-runbook-textual/automation-pass-params.png)

8. Runbook tamamlandığında, VM'nin başladığını doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

* Dil referansı ve öğrenme modülleri de dahil olmak üzere PowerShell hakkında daha fazla bilgi için [PowerShell Dokümanları'na](https://docs.microsoft.com/powershell/scripting/overview)bakın.
* Grafik çalışma kitaplarıyla başlamak için [ilk grafik çalışma kitabıma](automation-first-runbook-graphical.md)bakın.
* PowerShell runbook'ları ile başlamak için [ilk PowerShell runbook'uma](automation-first-runbook-textual-powershell.md)bakın.
* Runbook türleri ve bunların avantajları ve sınırlamaları hakkında daha fazla bilgi edinmek için [Azure Otomasyon runbook türlerine](automation-runbook-types.md)bakın.
* PowerShell komut dosyası destek özellikleri hakkında daha fazla bilgi için [Azure Otomasyonu'nda Yerel PowerShell komut dosyası desteğine](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/)bakın.
