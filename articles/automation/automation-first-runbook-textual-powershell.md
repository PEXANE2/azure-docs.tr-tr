---
title: Azure Automation 'da ilk PowerShell runbook 'um
description: Basit bir PowerShell runbook oluşturma, test etme ve yayımlama adımlarını anlatan öğretici.
keywords: azure powershell, powershell betik öğreticisi, powershell otomasyonu
services: automation
ms.subservice: process-automation
ms.date: 11/27/2018
ms.topic: conceptual
ms.openlocfilehash: b9808ddc3b61b0055642c5a0f2a82b0dc7553b33
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79278915"
---
# <a name="my-first-powershell-runbook"></a>İlk PowerShell runbook’um

> [!div class="op_single_selector"]
> * [Grafik](automation-first-runbook-graphical.md)
> * [PowerShell](automation-first-runbook-textual-powershell.md)
> * [PowerShell İş Akışı](automation-first-runbook-textual.md)
> * [Python](automation-first-runbook-textual-python2.md)

Bu öğretici, Azure Automation’da bir [PowerShell runbook](automation-runbook-types.md#powershell-runbooks) oluşturulmasını adım adım göstermektedir. Test ve yayımlayacağınız basit bir runbook ile başlayın. Bu işlem, runbook işinin durumunu nasıl izleyeceğinizi öğrenirken. Ardından runbook 'u gerçekten Azure kaynaklarını yönetmek üzere değiştirin, bu durumda bir Azure sanal makinesi başlatılıyor. Runbook parametrelerini ekleyerek runbook 'u daha sağlam hale getirmek için öğreticiyi doldurun.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma runbook çalışanınız hakkında az Module yükleme yönergeleri için bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Otomasyon hesabınız için, [Azure Otomasyonu 'nda Azure PowerShell modüllerini güncelleştirme](automation-update-azure-modules.md)' yi kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Azure aboneliği. Henüz bir aboneliğiniz yoksa [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ya da [ücretsiz hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) için kaydolabilirsiniz.
* Runbook’u tutacak ve Azure kaynaklarında kimlik doğrulamasını yapacak bir [Automation hesabı](automation-quickstart-create-account.md). Bu hesabın sanal makineyi başlatma ve durdurma izni olmalıdır.
* Azure sanal makinesi. Bu makineyi durdurup başlatabileceğinizden bu, bir üretim sanal makinesi olmamalıdır.
* Gerekirse, [Azure modüllerini içeri aktarın](shared-resources/modules.md) veya kullandığınız cmdlet 'lere göre [modülleri güncelleştirin](automation-update-azure-modules.md) .

## <a name="differences-from-powershell-workflow-runbooks"></a>PowerShell Iş akışı runbook 'larının farkları

PowerShell runbook 'ları, PowerShell Iş akışı runbook 'ları ile aynı yaşam döngüsü, yetenekler ve yönetime sahiptir. Ancak bazı farklar ve sınırlamalar vardır.

* PowerShell runbook 'ları, derleme adımı kullanmayan PowerShell Iş akışı runbook 'ları ile karşılaştırıldığında hızlı çalışır.
* PowerShell çalışma kitapları, PowerShell Iş akışı runbook 'larında kullanılan denetim noktalarını desteklemez. Kontrol noktalarını kullanarak PowerShell Iş akışı runbook 'ları herhangi bir noktadan işlemi sürdürür. PowerShell runbook 'u yalnızca baştan başlayarak işlemi sürdürür.
* PowerShell runbook 'ları komutları yalnızca hizmet temelli olarak yürütebilir. PowerShell Iş akışı runbook 'ları hem seri hem de paralel yürütmeyi destekler.
* Bir PowerShell runbook 'u için, bir betikteki her şey tek bir çalışma alanında çalışır. PowerShell Iş akışı runbook 'unda, etkinlik, komut veya betik bloğu kendi çalışma alanına sahip olabilir. 
* PowerShell runbook 'ları, PowerShell Iş akışı runbook 'lardan bazı [sözdizimsel farklılıklara](https://technet.microsoft.com/magazine/dn151046.aspx) sahiptir.

## <a name="step-1---create-runbook"></a>1\. Adım - Runbook oluşturma

Çıktı olarak **Merhaba Dünya** metnini veren basit bir runbook oluşturun.

1. Azure portalında, Otomasyon hesabınızı açın.
2. Runbook 'ların listesini açmak için **Işlem Otomasyonu** altında **runbook 'lar** ' ı seçin.
3. **Runbook oluştur**' a tıklayarak yeni bir runbook oluşturun.
4. Runbook’a **MyFirstRunbook-PowerShell** adını verin.
5. Bu durumda, bir [PowerShell runbook 'u](automation-runbook-types.md#powershell-runbooks)oluşturacağız. **Runbook türü**için **PowerShell** ' i seçin.
6. Runbook’u oluşturmak için **Oluştur**’a tıklayın ve metin düzenleyicisini açın.

## <a name="step-2---add-code-to-the-runbook"></a>2\. Adım - Runbook'a kod ekleme

Kodu doğrudan runbook’a yazabilir veya Kitaplık denetiminde cmdlet’leri, runbook’ları ve varlıkları seçebilir ve ilgili parametrelerle bunların runbook’a eklenmesini sağlayabilirsiniz. Bu öğreticide doğrudan runbook 'a kod yazacağım.

1. Runbook 'larınız Şu anda boş. Betiğin gövdesinde `Write-Output "Hello World"` yazın.

   ![Hello World](media/automation-first-runbook-textual-powershell/automation-helloworld.png)

2. **Kaydet**’e tıklayarak runbook’u kaydedin.

## <a name="step-3---test-the-runbook"></a> 3. adım-runbook 'u test etme

Runbook 'u üretimde kullanılabilir hale getirmek için yayımlamadan önce, düzgün çalıştığından emin olmak için test etmeniz gerekir. Bir runbook 'u test etmek taslak sürümünü çalıştırır ve çıktısını etkileşimli olarak görüntülemenizi sağlar.

1. Test bölmesini açmak için **Test bölmesi**’ne tıklayın.
2. Testi başlatmak için **Başlat**’a tıklayın. Etkinleştirilen tek seçenek bu olmalıdır.
3. [Runbook işinin](automation-runbook-execution.md) oluşturulduğunu ve durumunun bölmesinde görüntülendiğini unutmayın.

   İşin durumu **kuyruğa alındı**olarak başlar ve işin buluttaki bir runbook worker 'ın kullanılabilir hale gelmesini beklediğini belirtir. Bir çalışan işi talep ettiği zaman, durum **başlayacak** şekilde değişir. Son olarak, runbook aslında çalışmaya başladığında durum **çalışıyor** olur.

4. Runbook işi tamamlandığında, test bölmesi çıktısını görüntüler. Bu durumda, **Merhaba Dünya** ifadesini görürsünüz.

   ![Test Bölmesi Çıktısı](media/automation-first-runbook-textual-powershell/automation-testpane-output.png)

5. Tuvale geri dönmek için Test bölmesini kapatın.

## <a name="step-4---publish-and-start-the-runbook"></a>4\. Adım - Runbook’u yayımlama ve başlatma

Oluşturduğunuz runbook hala taslak modunda. Üretimde çalıştırılabilmesi için yayımlanması gerekir. Bir runbook yayımladığınızda, Taslak sürümü mevcut Yayımlanmış sürümün üzerine yazarsınız. Örneğimizde, runbook’u henüz oluşturduğunuzdan, Yayımlanmış sürümünüz yok.

1. Runbook’u yayımlamak için **Yayımla**’ya tıklayın ve sorulduğunda **Evet**’e tıklayın.
1. Runbook 'u runbook 'Lar sayfasında görüntülemek için sola kaydırın ve **yazma durum** değerinin **yayımlandı**olarak ayarlandığını unutmayın.
1. **MyFirstRunbook-PowerShell**’e ait bölmeyi görüntülemek üzere geri sağa kaydırın.
   
   Üstteki seçenekler runbook 'u şimdi başlatabilmenizi, gelecekteki bir başlangıç zamanı zamanlamanıza veya Runbook 'un bir HTTP çağrısıyla başlatılabilmesi için [Web kancası](automation-webhooks.md) oluşturmanıza imkan tanır.
1. **Başlat** ' ı ve ardından runbook 'u başlatmak isteyip Istemediğiniz sorulduğunda **Evet** ' i seçin. 
1. Oluşturulan runbook işi için bir Iş bölmesi açılır. Bu bölmeyi kapatabilir olsanız da işin ilerlemesini izleyebilmeniz için hemen açık bırakın. İş durumu **Iş özetinde**gösterilir ve olası durumlar runbook 'u test etmek için açıklanmıştır.

   ![İş Özeti](media/automation-first-runbook-textual-powershell/job-pane-status-blade-jobsummary.png)

1. Runbook durumu **tamamlandı**olarak görüntülendiğinde çıkış sayfasını açmak için **Çıkış** ' a tıklayın, burada **Merhaba Dünya** görebilirsiniz.

   ![İş Çıktısı](media/automation-first-runbook-textual-powershell/job-pane-status-blade-outputtile.png)

1. Çıkış sayfasını kapatın.
1. Runbook işine ait Akışlar bölmesini açmak için **Tüm Günlükler**’e tıklayın. Yalnızca çıkış akışında **Merhaba Dünya** görmeniz gerekir.

    Akışlar bölmesinin, runbook 'a yazıyorsa ayrıntılı ve hata akışları gibi bir runbook işi için diğer akışları gösterebileceğini unutmayın.

   ![Tüm Günlükler](media/automation-first-runbook-textual-powershell/job-pane-status-blade-alllogstile.png)

1. **Myfirstrunbook-PowerShell** sayfasına dönmek için akışlar bölmesini ve iş bölmesini kapatın.
1. **Ayrıntılar**altında, bu runbook 'un işler sayfasını açmak için **işler** ' e tıklayın. Bu sayfada runbook 'larınız tarafından oluşturulan tüm işler listelenir. İşi yalnızca bir kez çalıştırdığınız için, yalnızca bir iş listelendiğini görmeniz gerekir.

   ![İş Listesi](media/automation-first-runbook-textual-powershell/runbook-control-job-tile.png)

1. Runbook 'u başlattığınızda görüntülediğiniz aynı Iş bölmesini açmak için iş adına tıklayın. Runbook için oluşturulan herhangi bir işin ayrıntılarını görüntülemek için bu bölmeyi kullanın.

## <a name="step-5---add-authentication-to-manage-azure-resources"></a>5\. Adım- Azure kaynaklarını yönetmek için kimlik doğrulaması ekleme

Runbook uygulamanızı test ettiniz ve yayımladınız, ancak şu ana kadar faydalı bir şey yapmadı. Bu runbook’un Azure kaynaklarını yönetmesini istiyorsunuz. Bunu yapmak için, runbook 'un Otomasyon hesabınızı oluştururken otomatik olarak oluşturulan farklı çalıştır hesabını kullanarak kimlik doğrulaması yapabilmesi gerekir.

Aşağıdaki örnekte gösterildiği gibi, farklı çalıştır bağlantısı [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-3.5.0) cmdlet 'i ile yapılır. Kaynakları birden çok abonelik genelinde yönetiyorsanız, [Get-azcontext](https://docs.microsoft.com/powershell/module/Az.Accounts/Get-AzContext?view=azps-3.5.0)Ile birlikte *azcontext* parametresini kullanmanız gerekir.

> [!NOTE]
> PowerShell runbook 'ları için, **Add-azaccount** ve **Add-AzureRMAccount** , **Connect-azaccount**için diğer adlardır. Bu cmdlet 'leri kullanabilir veya Otomasyon hesabınızdaki [modüllerinizi](automation-update-azure-modules.md) en son sürümlere güncelleştirebilirsiniz. Yeni bir Otomasyon hesabı oluşturmuş olsanız bile modüllerinizi güncelleştirmeniz gerekebilir.

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
1. **Myfirstrunbook-PowerShell** sayfasında **Düzenle** ' ye tıklayarak metin düzenleyicisini açın.
1. **Yazma çıkış** satırına daha fazla ihtiyacınız yoktur. Hemen devam edip silmeniz yeterlidir.
1. Otomasyon farklı çalıştır hesabınızla kimlik doğrulamasını işleyen aşağıdaki kodu yazın veya kopyalayıp yapıştırın.

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

1. Runbook 'u test edebilmeniz için **Test bölmesi** ' ne tıklayın.
1. Testi başlatmak için **Başlat**’a tıklayın. Tamamlandıktan sonra, hesabınızdaki temel bilgileri görüntüleyen aşağıdakine benzer bir çıktı görmeniz gerekir. Bu çıktı, farklı çalıştır hesabının geçerli olduğunu onaylar.

   ![Kimlik doğrulaması](media/automation-first-runbook-textual-powershell/runbook-auth-output.png)

## <a name="step-6---add-code-to-start-a-virtual-machine"></a>6\. Adım - Sanal makineyi başlatmak için kod ekleme

Runbook 'larınızın Azure aboneliğinizde kimlik doğrulaması yapdığına göre, kaynakları yönetebilirsiniz. Bir sanal makineyi başlatmak için bir komut ekleyelim. Azure aboneliğinizdeki herhangi bir sanal makineyi seçebilir ve bu adı yalnızca runbook 'ta o ada gönderebilirsiniz.

1. Runbook betiğe, sanal makineyi başlatmak için [Start-AzVM](https://docs.microsoft.com/powershell/module/Az.Compute/Start-AzVM?view=azps-3.5.0) cmdlet 'ini ekleyin. Aşağıda gösterildiği gibi, cmdlet, **VMName** adlı ve **resourcegroupname**adlı bir kaynak grubuyla bir sanal makine başlatır.

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

1. Runbook 'u kaydedin ve test **bölmesi** ' ne tıklayarak test edebilirsiniz.
1. Testi başlatmak için **Başlat** ' a tıklayın. İşlem tamamlandıktan sonra, sanal makinenin başlatıldığından emin olun.

## <a name="step-7---add-an-input-parameter"></a>7\. adım-giriş parametresi ekleme

Runbook 'larınız Şu anda runbook 'a kodlandığı sanal makineyi başlatır. Runbook başlatıldığında sanal makineyi belirtirseniz runbook daha yararlı olacaktır. Bu işlevi sağlamak için Runbook 'a giriş parametreleri ekleyelim.

1. Metinsel düzenleyicide, *VMName* ve *resourcegroupname*parametrelerinin değişkenlerini kullanmak Için **Start-azvm** cmdlet 'ini değiştirin. 

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

1. Runbook'u kaydedin ve Test bölmesini açın. Şimdi testte kullanılacak olan iki girdi değişkeni için değerleri sağlayabilirsiniz.
1. Test bölmesini kapatın.
1. Runbook’un yeni sürümünü yayımlamak için **Yayımla**’ya tıklayın.
1. Daha önce başlattığınız sanal makineyi durdurun.
1. Runbook'u başlatmak için **Başlat**’a tıklayın. 
1. Başlayacağız sanal makineye yönelik **VMName** ve **resourcegroupname** değerlerini yazın ve ardından **Tamam**' a tıklayın.<br><br> ![Parametre Geçirme](media/automation-first-runbook-textual-powershell/automation-pass-params.png)<br>
1. Runbook tamamlandığında, sanal makinenin başlatıldığından emin olun.

## <a name="next-steps"></a>Sonraki adımlar

* Dil başvurusu ve öğrenme modülleri dahil olmak üzere PowerShell hakkında daha fazla bilgi için bkz. [PowerShell belgeleri](/powershell/scripting/overview).
* Grafik runbook 'ları kullanmaya başlamak için bkz. [ilk grafik runbook 'Um](automation-first-runbook-graphical.md).
* PowerShell İş Akışı runbook'larını kullanmaya başlamak için bkz. [İlk PowerShell İş Akışı runbook uygulamam](automation-first-runbook-textual.md).
* Runbook türleri ve bunların avantajları ve sınırlamaları hakkında daha fazla bilgi edinmek için bkz. [Azure Otomasyonu runbook türleri](automation-runbook-types.md).
* PowerShell betiği desteği özelliği hakkında daha fazla bilgi için bkz. [Azure Otomasyonu 'Nda yerel PowerShell betiği desteği](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/).
