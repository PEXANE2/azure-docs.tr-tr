---
title: Azure Automation 'da ilk PowerShell Iş akışı runbook 'um
description: PowerShell İş Akışı kullanarak basit bir runbook oluşturma, test etme ve yayımlama adımlarını anlatan öğretici.
keywords: powershell iş akışı, powershell iş akışı örnekleri, iş akışı powershell
services: automation
ms.subservice: process-automation
ms.date: 09/24/2018
ms.topic: conceptual
ms.openlocfilehash: 16b6a0cf3e43b172667f55b1ac95e8a278769f9d
ms.sourcegitcommit: e4c33439642cf05682af7f28db1dbdb5cf273cc6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2020
ms.locfileid: "78246395"
---
# <a name="my-first-powershell-workflow-runbook"></a>İlk PowerShell İş Akışı runbook uygulamam

> [!div class="op_single_selector"]
> * [Grafik](automation-first-runbook-graphical.md)
> * [PowerShell](automation-first-runbook-textual-powershell.md)
> * [PowerShell İş Akışı](automation-first-runbook-textual.md)
> * [Python](automation-first-runbook-textual-python2.md)

Bu öğretici, Azure Automation’da bir [PowerShell İş Akışı runbook](automation-runbook-types.md#powershell-workflow-runbooks) oluşturulmasını adım adım göstermektedir. Runbook işinin durumunun nasıl izleneceğini öğrenirken test ve yayımladığınız basit bir runbook 'u kullanmaya başlayın. Ardından, bir Azure sanal makinesini başlatarak, runbook 'u Azure kaynaklarını yönetmek için değiştirin. Son olarak, runbook parametreleri ekleyerek runbook 'u daha sağlam hale getirin.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma runbook çalışanınız hakkında az Module yükleme yönergeleri için bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Otomasyon hesabınız için, [Azure Otomasyonu 'nda Azure PowerShell modüllerini güncelleştirme](automation-update-azure-modules.md)' yi kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Azure aboneliği. Henüz bir aboneliğiniz yoksa [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ya da [ücretsiz hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) için kaydolabilirsiniz.
* Runbook’u tutacak ve Azure kaynaklarında kimlik doğrulamasını yapacak bir [Automation hesabı](automation-offering-get-started.md). Bu hesabın sanal makineyi başlatma ve durdurma izni olmalıdır.
* Azure sanal makinesi. Bu makineyi durdurup başlatabileceğinizden bu, bir üretim sanal makinesi olmamalıdır.

## <a name="step-1---create-new-runbook"></a>1\. Adım - Yeni runbook oluşturma

Çıktı olarak **Merhaba Dünya** metnini veren basit bir runbook oluşturun.

1. Azure portalında, Otomasyon hesabınızı açın.

   Automation hesabı sayfası size bu hesaptaki kaynakların hızlı bir görünümünü sağlar. Birkaç varlığınız zaten olmalıdır. Bu varlıkların çoğu, otomatik olarak yeni bir Otomasyon hesabına eklenen modüllerdir. Ayrıca, aboneliğinizle ilişkili kimlik bilgisi varlığını de bilmelisiniz.
 
1. Runbook 'ların listesini açmak için **Işlem Otomasyonu** altında **runbook 'lar** ' ı seçin.
1. **Runbook oluştur**' a tıklayarak yeni bir runbook oluşturun.
1. Runbook’a **MyFirstRunbook-Workflow** adını verin.
1. Bu durumda, bir [PowerShell Iş akışı runbook 'u](automation-runbook-types.md#powershell-workflow-runbooks)oluşturacağız. Bu nedenle **runbook türü**Için **PowerShell iş akışını** seçin.
1. Runbook’u oluşturmak için **Oluştur**’a tıklayın ve metin düzenleyicisini açın.

## <a name="step-2---add-code-to-the-runbook"></a>2\. Adım - Runbook'a kod ekleme

Kodu doğrudan runbook 'a yazabilir ya da kitaplık denetiminden cmdlet 'leri, runbook 'ları ve varlıkları seçebilir ve ilgili parametrelerle runbook 'lara ekleyebilirsiniz. Bu öğreticide doğrudan runbook 'a kod yazın.

1. Runbook 'larınız Şu anda yalnızca gerekli **Iş akışı** anahtar sözcüğü, runbook 'un adı ve tüm iş akışını gösteren küme ayraçları ile boştur.

   ```powershell-interactive
   Workflow MyFirstRunbook-Workflow
   {
   }
   ```

1. Küme ayraçları arasında `Write-Output "Hello World"` yazın.

   ```powershell-interactive
   Workflow MyFirstRunbook-Workflow
   {
   Write-Output "Hello World"
   }
   ```

1. **Kaydet**’e tıklayarak runbook’u kaydedin.

## <a name="step-3---test-the-runbook"></a>3\. Adım - Runbook'u test etme

Runbook 'u üretimde kullanılabilir hale getirmek için yayımlamadan önce, düzgün çalıştığından emin olmak için test etmeniz gerekir. Bir runbook 'u test etmek taslak sürümünü çalıştırır ve çıktısını etkileşimli olarak görüntülemenizi sağlar.

1. Test bölmesini açmak için **Test bölmesi** ' ni seçin.
1. Sınamayı başlatmak için **Başlat** ' a tıklayın ve yalnızca etkin seçeneği test edin.
1. [Runbook işinin](automation-runbook-execution.md) oluşturulduğunu ve durumunun bölmesinde görüntülendiğini unutmayın.

   İşin durumu **kuyruğa alındı**olarak başlar ve işin buluttaki bir runbook worker 'ın kullanılabilir hale gelmesini beklediğini belirtir. Bir çalışan işi talep ettiği zaman, durum **başlayacak** şekilde değişir. Son olarak, runbook aslında çalışmaya başladığında durum **çalışıyor** olur.

1. Runbook işi tamamlandığında, test bölmesi çıktısını görüntüler. Bu durumda, **Merhaba Dünya** ifadesini görürsünüz.

   ![Hello World](media/automation-first-runbook-textual/test-output-hello-world.png)

1. Tuvale geri dönmek için Test bölmesini kapatın.

## <a name="step-4---publish-and-start-the-runbook"></a>4\. Adım - Runbook’u yayımlama ve başlatma

Oluşturduğunuz runbook hala taslak modunda. Üretim ortamında çalıştırmadan önce onu yayımlamanız gerekir. Bir runbook yayımladığınızda, Taslak sürümü mevcut Yayımlanmış sürümün üzerine yazarsınız. Örneğimizde, runbook’u henüz oluşturduğunuzdan, Yayımlanmış sürümünüz yok.

1. Runbook’u yayımlamak için **Yayımla**’ya tıklayın ve sorulduğunda **Evet**’e tıklayın.
1. Runbook 'u runbook **'lar** sayfasında görüntülemek için sola kaydırın ve **yazma durumu** alanının **yayımlandı**olarak ayarlandığını unutmayın.
1. **Myfirstrunbook-Workflow**için sayfayı görüntülemek üzere sağa doğru kaydırın.

   Üstteki seçenekler runbook 'u şimdi başlatabilmenizi, gelecekteki bir başlangıç zamanı zamanlamanıza veya Runbook 'un bir HTTP çağrısıyla başlatılabilmesi için [Web kancası](automation-webhooks.md) oluşturmanıza imkan tanır.

1. **Başlat** ' ı ve ardından runbook 'u başlatmak isteyip Istemediğiniz sorulduğunda **Evet** ' i seçin.

   ![Runbook’u başlatma](media/automation-first-runbook-textual/automation-runbook-controls-start.png)

1. Oluşturulan runbook işi için bir Iş bölmesi açılır. Bu durumda, işin ilerlemesini izlemek için bölmeyi açık bırakın.

1. İş durumunun **Iş özetinde**gösterildiğini unutmayın. Bu durum, runbook 'u test ederken gördüğünüz durumlardan eşleşir.

   ![İş Özeti](media/automation-first-runbook-textual/job-pane-status-blade-jobsummary.png)

1. Runbook durumu olarak **Tamamlandı** gösterilince **Çıktı**’ya tıklayın. **Merhaba Dünya** Iletinizi görebileceğiniz çıkış sayfası açılır.

   ![İş Özeti](media/automation-first-runbook-textual/job-pane-status-blade-outputtile.png)

1. Çıkış sayfasını kapatın.

1. Runbook işine ait Akışlar bölmesini açmak için **Tüm Günlükler**’e tıklayın. Yalnızca çıkış akışında **Merhaba Dünya** görmeniz gerekir. Akışlar bölmesinin, runbook 'a yazıyorsa ayrıntılı ve hata akışları gibi bir runbook işi için diğer akışları gösterebileceğini unutmayın.

   ![İş Özeti](media/automation-first-runbook-textual/job-pane-status-blade-alllogstile.png)

1. **Myfirstrunbook** sayfasına dönmek için akışlar bölmesini ve iş bölmesini kapatın.
1. Bu runbook 'un Işler sayfasını açmak için **kaynaklar** altında **işler** ' e tıklayın. Bu sayfada runbook 'larınız tarafından oluşturulan tüm işler listelenir. İşi yalnızca bir kez çalıştırdığınız için yalnızca bir iş listelendiğini görmeniz gerekir.

   ![İşler](media/automation-first-runbook-textual/runbook-control-job-tile.png)

1. Runbook 'u başlattığınızda görüntülediğiniz aynı Iş bölmesini açmak için iş adına tıklayın. Runbook için oluşturulan herhangi bir işin ayrıntılarını görüntülemek için bu bölmeyi kullanın.

## <a name="step-5---add-authentication-to-manage-azure-resources"></a>5\. Adım- Azure kaynaklarını yönetmek için kimlik doğrulaması ekleme

Runbook uygulamanızı test ettiniz ve yayımladınız, ancak şu ana kadar faydalı bir şey yapmadı. Bu runbook’un Azure kaynaklarını yönetmesini istiyorsunuz. Bu, aboneliğin kimlik bilgilerini kullanarak kimlik doğrulaması yapamadığı takdirde bunu yapamıyor. Kimlik doğrulaması **Connect-AzAccount** cmdlet 'ini kullanır.

>[!NOTE]
>PowerShell runbook 'ları için, **Add-azaccount** ve **Add-AzureRMAccount** , **Connect-azaccount**için diğer adlardır. Bu cmdlet 'leri kullanabilir veya Otomasyon hesabınızdaki [modüllerinizi](automation-update-azure-modules.md) en son sürümlere güncelleştirebilirsiniz. Yeni bir Otomasyon hesabı oluşturmuş olsanız bile modüllerinizi güncelleştirmeniz gerekebilir.

1. **Myfirstrunbook-Workflow** sayfasına gidin ve **Düzenle**' ye tıklayarak metin düzenleyicisini açın.
2. `Write-Output` satırını silin.
3. İmleci ayraçlar arasında boş bir satıra getirin.
4. Otomasyon farklı çalıştır hesabınızla kimlik doğrulamasını işleyen aşağıdaki kodu yazın veya kopyalayıp yapıştırın.

   ```powershell-interactive
   # Ensures you do not inherit an AzContext in your runbook
   Disable-AzContextAutosave –Scope Process

   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzAccount -ServicePrincipal -Tenant $Conn.TenantID `
   -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

   $AzureContext = Select-AzSubscription -SubscriptionId $Conn.SubscriptionID
   ```

1. Runbook 'u test edebilmeniz için **Test bölmesi** ' ne tıklayın.
1. Testi başlatmak için **Başlat**’a tıklayın. Tamamlandıktan sonra, hesabınızdaki temel bilgileri görüntüleyen aşağıdakine benzer bir çıktı görmeniz gerekir. Bu eylem, kimlik bilgisinin geçerli olduğunu onaylar.

   ![Kimlik doğrulaması](media/automation-first-runbook-textual/runbook-auth-output.png)

## <a name="step-6---add-code-to-start-a-virtual-machine"></a>6\. Adım - Sanal makineyi başlatmak için kod ekleme

Runbook 'larınızın Azure aboneliğine kimlik doğrulaması yapdığına göre, kaynakları yönetebilirsiniz. Bir sanal makineyi başlatmak için bir komut ekleyelim. Azure aboneliğinizdeki herhangi bir VM 'yi seçebilirsiniz ve şu anda runbook 'ta bu adı kodlıyoruz. Kaynakları birden çok abonelik genelinde yönetiyorsanız, [Get-azcontext](/powershell/module/az.accounts/get-azcontext) cmdlet 'Ini kullanarak *azcontext* parametresini kullanmanız gerekir.

1. Aşağıda gösterildiği gibi [Start-AzVM](https://docs.microsoft.com/powershell/module/Az.Compute/Start-AzVM?view=azps-3.5.0
) cmdlet 'ine bir çağrı girerek başlatılacak sanal makinenin adını ve kaynak grubu adını belirtin. 

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

1. Runbook 'u kaydedin ve test **bölmesi** ' ne tıklayarak test edebilirsiniz.
1. Testi başlatmak için **Başlat**’a tıklayın. Tamamlandıktan sonra VM 'nin başlatılmış olduğunu denetleyin.

## <a name="step-7---add-an-input-parameter-to-the-runbook"></a>7\. Adım - Runbook'a girdi parametresi ekleme

Runbook 'larınız Şu anda runbook 'ta sabit kodlanmış sanal makineyi başlatır. Runbook başlatıldığında VM 'yi belirtebileceğiniz zaman daha yararlı olacaktır. Bu işlevi sağlamak için Runbook 'a giriş parametreleri ekleyelim.

1. Runbook 'a *VMName* ve *resourcegroupname* değerlerini ekleyin ve aşağıda gösterildiği gibi **Start-azvm** cmdlet 'i ile ilişkili değişkenleri kullanın.

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
5. Başlattığınız VM 'yi durdurun.
6. Runbook'u başlatmak için **Başlat**’a tıklayın. 
7. Başlayacaksanız kullanılacak VM için **VMName** ve **resourcegroupname** değerlerini yazın.

   ![Runbook’u Başlatma](media/automation-first-runbook-textual/automation-pass-params.png)

8. Runbook tamamlandığında, VM 'nin başlatıldığını doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

* Dil başvurusu ve öğrenme modülleri de dahil olmak üzere PowerShell hakkında daha fazla bilgi için [PowerShell belgelerine](https://docs.microsoft.com/powershell/scripting/overview)bakın.
* Grafik runbook 'ları kullanmaya başlamak için bkz. [ilk grafik runbook 'Um](automation-first-runbook-graphical.md).
* PowerShell runbook'ları kullanmaya başlamak için bkz. [İlk PowerShell runbook’um](automation-first-runbook-textual-powershell.md).
* Runbook türleri ve bunların avantajları ve sınırlamaları hakkında daha fazla bilgi edinmek için bkz. [Azure Otomasyonu runbook türleri](automation-runbook-types.md).
* PowerShell betiği destek özellikleri hakkında daha fazla bilgi için bkz. [Azure Otomasyonu 'Nda yerel PowerShell betiği desteği](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/).
