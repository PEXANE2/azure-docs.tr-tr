---
title: Azure Automation 'da ilk grafik runbook 'im
description: Basit bir grafik runbook uygulaması oluşturma, test etme ve yayımlama adımlarını anlatan öğretici.
keywords: runbook, runbook şablonu, runbook otomasyonu, azure runbook
services: automation
ms.subservice: process-automation
ms.date: 04/13/2018
ms.topic: conceptual
ms.openlocfilehash: 8b836ebc0adc6f0616d28b16bfb743dfc4553d1a
ms.sourcegitcommit: 512d4d56660f37d5d4c896b2e9666ddcdbaf0c35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2020
ms.locfileid: "79367424"
---
# <a name="my-first-graphical-runbook"></a>İlk grafik runbook uygulamam

> [!div class="op_single_selector"]
> * [Grafik](automation-first-runbook-graphical.md)
> * [PowerShell](automation-first-runbook-textual-powershell.md)
> * [PowerShell İş Akışı](automation-first-runbook-textual.md)
> * [Python](automation-first-runbook-textual-python2.md)
> 

Bu öğretici, Azure Automation’da bir [grafik runbook uygulaması](automation-runbook-types.md#graphical-runbooks) oluşturulmasını adım adım göstermektedir. Test ve yayımlayacağınız basit bir runbook ile başlayın. Bu işlem, runbook işinin durumunu nasıl izleyeceğinizi öğrenirken. Ardından runbook 'u gerçekten Azure kaynaklarını yönetmek üzere değiştirin, bu durumda bir Azure sanal makinesi başlatılıyor. Runbook parametreleri ve koşullu bağlantılar ekleyerek runbook 'u daha sağlam hale getirmek için öğreticiyi doldurun.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma runbook çalışanınız hakkında az Module yükleme yönergeleri için bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Otomasyon hesabınız için, [Azure Otomasyonu 'nda Azure PowerShell modüllerini güncelleştirme](automation-update-azure-modules.md)' yi kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Azure aboneliği. Henüz bir aboneliğiniz yoksa [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ya da [ücretsiz hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) için kaydolabilirsiniz.
* Runbook’u tutacak ve Azure kaynaklarında kimlik doğrulamasını yapacak bir [Automation hesabı](automation-offering-get-started.md). Bu hesabın sanal makineyi başlatma ve durdurma izni olmalıdır.
* Azure sanal makinesi. Bu makineyi durdurup başlatabileceğinizden bu, bir üretim sanal makinesi olmamalıdır.

## <a name="step-1---create-runbook"></a>1\. Adım - Runbook oluşturma

`Hello World`metin çıkışı veren basit bir runbook oluşturarak başlayın.

1. Azure portalında, Otomasyon hesabınızı açın. 

    Automation hesabı sayfası size bu hesaptaki kaynakların hızlı bir görünümünü sağlar. Birkaç varlığınız zaten olmalıdır. Bu varlıkların çoğu, otomatik olarak yeni bir Otomasyon hesabına eklenen modüllerdir. Ayrıca, aboneliğinizle ilişkili kimlik bilgisi varlığını de bilmelisiniz.
2. Runbook 'ların listesini açmak için **Işlem Otomasyonu** altında **runbook 'lar** ' ı seçin.
3. **Runbook oluştur**' a tıklayarak yeni bir runbook oluşturun.
4. Runbook’a **MyFirstRunbook-Graphical** adını verin.
5. Bu durumda, bir [grafik runbook](automation-graphical-authoring-intro.md)oluşturacağız. **Runbook türü**için **grafik** ' i seçin.<br> ![Yeni runbook](media/automation-first-runbook-graphical/create-new-runbook.png)<br>
6. Runbook’u oluşturmak için **Oluştur**’a tıklayın ve grafik düzenleyicisini açın.

## <a name="step-2---add-activities"></a>2\. adım-etkinlikleri ekleme

Düzenleyicinin sol tarafındaki Kitaplık denetimi runbook uygulamanıza eklenecek etkinlikleri seçmenizi sağlar. Runbook 'tan çıkış metnine bir `Write-Output` cmdlet 'i ekleyeceğiz.

1. Kitaplık denetiminde, arama alanına tıklayın ve `write-output`yazın. Arama sonuçları aşağıdaki görüntüde gösterilmiştir. <br> ![Microsoft.PowerShell.Utility](media/automation-first-runbook-graphical/search-powershell-cmdlet-writeoutput.png)
1. Listenin aşağısına kaydırın. **Write-output** ' a sağ tıklayın ve **tuvale Ekle**' yi seçin. Alternatif olarak, cmdlet adının yanındaki üç nokta (...) simgesine ve ardından **tuvale Ekle**' yi seçebilirsiniz.
1. Tuvalde **Write-Output** etkinliğine tıklayın. Bu eylem, etkinliği yapılandırmanızı sağlayan yapılandırma denetimi sayfasını açar.
1. **Etiket** alanı varsayılan olarak cmdlet adıdır, ancak bunu daha kolay bir şekilde değiştirebilirsiniz. `Write Hello World to output`değiştirin.
1. Cmdlet parametreleri değerlerini sağlamak için **Parametreler**’e tıklayın.

   Bazı cmdlet 'ler birden fazla parametre kümesine sahiptir ve hangisinin kullanılacağını seçmeniz gerekir. Bu durumda `Write-Output` yalnızca bir parametre kümesi vardır.

1. `InputObject` parametresini seçin. Bu, çıkış akışına göndermek üzere metin belirtmek için kullandığınız parametredir.
1. **Veri kaynağı** açılır menüsü bir parametre değerini doldurmak için kullanabileceğiniz kaynaklar sağlar. Bu menüde **PowerShell ifadesi**' ni seçin. 

   Bu tür kaynaklardan gelen çıktıyı başka bir etkinlik, Otomasyon varlığı veya PowerShell ifadesi olarak kullanabilirsiniz. Bu durumda, çıktı yalnızca `Hello World`. Bir PowerShell ifadesi kullanabilir ve dize belirtebilirsiniz.<br>

1. **İfade** alanına `Hello World` yazın ve ardından tuvale dönmek için Iki kez **Tamam** ' a tıklayın.
1. **Kaydet**’e tıklayarak runbook’u kaydedin.

## <a name="step-3---test-the-runbook"></a>3\. Adım - Runbook'u test etme

Runbook 'u üretimde kullanılabilir hale getirmek için yayımlamadan önce, düzgün çalıştığından emin olmak için test etmeniz gerekir. Bir runbook 'u test etmek taslak sürümünü çalıştırır ve çıktısını etkileşimli olarak görüntülemenizi sağlar.

1. Test bölmesini açmak için **Test bölmesi** ' ni seçin.
1. Testi başlatmak için **Başlat**’a tıklayın. Etkinleştirilen tek seçenek bu olmalıdır.
1. [Runbook işinin](automation-runbook-execution.md) oluşturulduğunu ve durumunun bölmesinde görüntülendiğini unutmayın.

   İşin durumu `Queued`olarak başlar ve işin buluttaki bir runbook worker 'ın kullanılabilir hale gelmesini beklediğini belirtir. Bir çalışan işi talep ettiği zaman durum `Starting` olarak değişir. Son olarak, runbook aslında çalışmaya başladığında durum `Running` olur.

1. Runbook işi tamamlandığında, test bölmesi çıktısını görüntüler. Bu durumda `Hello World`görürsünüz.<br> ![Hello World](media/automation-first-runbook-graphical/runbook-test-results.png)
1. Tuvale geri dönmek için Test bölmesini kapatın.

## <a name="step-4---publish-and-start-the-runbook"></a>4\. Adım - Runbook’u yayımlama ve başlatma

Oluşturduğunuz runbook hala taslak modunda. Üretimde çalıştırılabilmesi için yayımlanması gerekir. Bir runbook yayımladığınızda, Taslak sürümü mevcut Yayımlanmış sürümün üzerine yazarsınız. Örneğimizde, runbook’u henüz oluşturduğunuzdan, Yayımlanmış sürümünüz yok.

1. Runbook 'u yayımlamak için **Yayımla** ' yı seçin ve istendiğinde **Evet** ' e tıklayın.
1. Runbook 'u runbook 'Lar sayfasında görüntülemek için sola kaydırın ve **yazma durum** değerinin **yayımlandı**olarak ayarlandığını unutmayın.
1. **Myfirstrunbook-grafik**sayfasını görüntülemek için sağ tarafa geri kaydırın.

   Üstteki seçenekler runbook 'u şimdi başlatabilmenizi, gelecekteki bir başlangıç zamanı zamanlamanıza veya Runbook 'un bir HTTP çağrısıyla başlatılabilmesi için [Web kancası](automation-webhooks.md) oluşturmanıza imkan tanır.

1. **Başlat** ' ı ve ardından runbook 'u başlatmak isteyip Istemediğiniz sorulduğunda **Evet** ' i seçin.
1. Oluşturulan runbook işi için bir Iş bölmesi açılır. **İş durumu** alanının **tamamlandı**görüntülendiğini doğrulayın.
1. Görüntülenecek `Hello World` görebileceğiniz çıkış sayfasını açmak için **Çıkış** ' a tıklayın.
1. Çıkış sayfasını kapatın.
1. Runbook işine ait Akışlar bölmesini açmak için **Tüm Günlükler**’e tıklayın. Yalnızca çıkış akışında `Hello World` görmeniz gerekir. 

    Akışlar bölmesinin, runbook 'a yazıyorsa ayrıntılı ve hata akışları gibi bir runbook işi için diğer akışları gösterebileceğini unutmayın.
1. **Myfirstrunbook-grafik** sayfasına dönmek için akışlar bölmesini ve iş bölmesini kapatın.
1. Runbook 'un tüm işlerini görüntülemek için **kaynaklar**altında **işler** ' i seçin. Işler sayfasında, runbook 'larınız tarafından oluşturulan tüm işler listelenir. İşi yalnızca bir kez çalıştırdığınız için yalnızca bir iş listelendiğini görmeniz gerekir.
1. Runbook 'u başlattığınızda görüntülediğiniz aynı Iş bölmesini açmak için iş adına tıklayın. Runbook için oluşturulan herhangi bir işin ayrıntılarını görüntülemek için bu bölmeyi kullanın.

## <a name="step-5---create-variable-assets"></a>5\. Adım - Değişken varlıkları oluşturma

Runbook 'unuzu test ve yayımladınız, ancak şu ana kadar Azure kaynaklarını yönetmek için faydalı bir şey yapmadınız. Runbook 'u kimlik doğrulaması yapacak şekilde yapılandırmadan önce, abonelik KIMLIĞINI tutmak için bir değişken oluşturmanız, kimlik doğrulaması için bir etkinlik ayarlamanız ve ardından değişkene başvurmanız gerekir. Abonelik bağlamına bir başvuru eklemek, birden çok abonelikle kolayca çalışmanıza olanak sağlar.

1. Gezinti bölmesindeki **abonelikler** SEÇENEĞINDEN abonelik kimliğinizi kopyalayın.
1. Otomasyon hesapları sayfasında, **paylaşılan kaynaklar**altında **değişkenler** ' i seçin.
1. **Değişken Ekle**' yi seçin.
1. Yeni değişken sayfasında, belirtilen alanlarda aşağıdaki ayarları yapın.

    * **Ad** --`AzureSubscriptionId`girin.
    * **Değer** --abonelik kimliğinizi girin. 
    * **Tür** --dizeyi tut seçili.
    * **Şifreleme** --varsayılan değeri kullanın.
1. Değişkeni oluşturmak için **Oluştur**’a tıklayın.

## <a name="step-6---add-authentication"></a>6\. Adım-kimlik doğrulaması ekleme

Abonelik KIMLIĞINI tutmak için bir değişkene sahip olduğunuza göre, runbook 'u aboneliğiniz için farklı çalıştır kimlik bilgileriyle kimlik doğrulaması yapacak şekilde yapılandırabilirsiniz. Bunu, Azure farklı çalıştır bağlantısını bir varlık olarak ekleyerek yapın. Ayrıca, tuvale [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/Connect-AzAccount?view=azps-3.5.0) cmdlet 'Ini ve [set-azcontext](https://docs.microsoft.com/powershell/module/az.accounts/Set-AzContext?view=azps-3.5.0) cmdlet 'ini de eklemeniz gerekir.

>[!NOTE]
>PowerShell runbook 'ları için `Add-AzAccount` ve `Add-AzureRMAccount` `Connect-AzAccount`için diğer adlardır. Bu diğer adların grafik runbook 'larınız için kullanılamayacağını unutmayın. Grafik runbook 'u yalnızca `Connect-AzAccount`kullanabilir.

1. Runbook 'e gidin ve **Myfirstrunbook-grafik** sayfasında **Düzenle** ' yi seçin.
1. `Write Hello World to output` girişe daha fazla ihtiyacınız yoktur. Üç noktaya tıklayıp **Sil**' i seçmeniz yeterlidir.
1. Kitaplık denetiminde, **varlıklar**' ı ve ardından **Bağlantılar**' ı genişletin. **Tuvale Ekle ' ye**tıklayarak tuvale `AzureRunAsConnection` ekleyin.
1. Kitaplık denetiminde, arama alanına `Connect-AzAccount` yazın.
1. Tuvale `Connect-AzAccount` ekleyin.
1. Şeklin altında bir daire görünene kadar **Farklı Çalıştır Bağlantısını Al** üzerinde bekleyin. Bir bağlantı oluşturmak için daireye tıklayın ve oku `Connect-AzAccount` ' ye sürükleyin. Runbook `Get Run As Connection` başlar ve `Connect-AzAccount`çalıştırır.<br> ![Etkinlikler arasında bağlantı oluşturma](media/automation-first-runbook-graphical/runbook-link-auth-activities.png)
1. Tuvalde `Connect-AzAccount`' yi seçin. Yapılandırma denetim bölmesinde **etiket** alanına Azure 'Da **oturum aç** yazın.
1. **Parametreler**' e tıklayın ve etkinlik parametresi yapılandırma sayfası görüntülenir.
1. `Connect-AzAccount` cmdlet 'i birden fazla parametre kümesine sahiptir ve parametre değerleri sağlamadan önce bir tane seçmeniz gerekir. **Parametre kümesi** ' ne tıklayın ve ardından **serviceprincipalcertificate**' ı seçin.
1. Bu parametre kümesinin parametreleri etkinlik parametresi yapılandırma sayfasında görüntülenir. **APPLICATIONID**’ye tıklayın.<br> ![Azure hesap parametreleri ekleme](media/automation-first-runbook-graphical/Add-AzureRmAccount-params.png)
1. Parametre değeri sayfasında, aşağıdaki ayarları yapın ve ardından **Tamam**' a tıklayın.

   * **Veri kaynağı** -- **etkinlik çıkışı**' nı seçin.
   * Veri kaynağı listesi-- **Farklı Çalıştır bağlantısını al**' ı seçin.
   * **Alan yolu** --`ApplicationId`yazın. Etkinlik birden fazla özelliğe sahip bir nesne çıkardığından, alan yolu için özelliğin adını belirtmektir.
1. **CERTIFICATETHUMBPRINT**öğesine tıklayın ve parametre değeri sayfasında, aşağıdaki ayarları yapın ve ardından **Tamam**' a tıklayın.

    * **Veri kaynağı** -- **etkinlik çıkışı**' nı seçin.
    * Veri kaynağı listesi-- **Farklı Çalıştır bağlantısını al**' ı seçin.
    * **Alan yolu** --`CertificateThumbprint`yazın.
1. **ServicePrincipal**öğesine tıklayın ve parametre değeri sayfasında, **veri kaynağı** alanı için **ConstantValue** öğesini seçin; **doğru**seçeneğine tıklayın; ardından **Tamam**' a tıklayın.
1. **Tenantıd**' ye tıklayın ve parametre değeri sayfasında aşağıdaki ayarları yapın. İşiniz bittiğinde, iki kez **Tamam** ' a tıklayın.

    * **Veri kaynağı** -- **etkinlik çıkışı**' nı seçin. 
    * Veri kaynağı listesi-- **Farklı Çalıştır bağlantısını al**' ı seçin.
    * **Alan yolu** --`TenantId`yazın. 
1. Kitaplık denetiminde, arama alanına `Set-AzContext` yazın.
1. Tuvale `Set-AzContext` ekleyin.
1. Tuvalde `Set-AzContext` seçin. Yapılandırma denetim bölmesinde **etiket** alanına `Specify Subscription Id` girin.
1. **Parametreler** ' e tıklayın ve etkinlik parametresi yapılandırma sayfası görüntülenir.
1. `Set-AzContext` cmdlet 'i birden fazla parametre kümesine sahiptir ve parametre değerleri sağlamadan önce bir tane seçmeniz gerekir. **Parametre kümesi** ' ne tıklayın ve ardından **SubscriptionID**' yi seçin.
1. Bu parametre kümesinin parametreleri etkinlik parametresi yapılandırma sayfasında görüntülenir. **SubscriptionID**öğesine tıklayın.
1. Parametre değeri sayfasında, **veri kaynağı** alanı Için **değişken varlık** ' ı seçin ve kaynak listesinden **Azuyeniden gönderme scriptionıd** ' yi seçin. İşiniz bittiğinde, iki kez **Tamam** ' a tıklayın.
1. Şeklin altında bir daire görünene kadar `Login to Azure` üzerine gelin. Daireye tıklayın ve oku `Specify Subscription Id`için sürükleyin. 

Runbook'unuzda bu noktada aşağıdakine benzer: <br>![Runbook kimlik doğrulama yapılandırması](media/automation-first-runbook-graphical/runbook-auth-config.png)

## <a name="step-7---add-activity-to-start-a-virtual-machine"></a>7\. Adım - Sanal makineyi başlatmak üzere etkinlik ekleme

Artık bir sanal makineyi başlatmak için `Start-AzVM` etkinliği eklemeniz gerekir. Azure aboneliğinizdeki herhangi bir VM 'yi seçebilirsiniz ve şu anda adını [Start-AzVM](https://docs.microsoft.com/powershell/module/az.compute/start-azvm?view=azps-3.5.0) cmdlet 'ine kodırsınız.

1. Kitaplık denetiminde, arama alanına `Start-Az` yazın.
2. Tuvale `Start-AzVM` ekleyin ve `Specify Subscription Id`altına sürükleyip sürükleyin.
1. Şeklin altında bir daire görünene kadar `Specify Subscription Id` üzerine gelin. Daireye tıklayın ve oku `Start-AzVM`için sürükleyin.
1. `Start-AzVM` öğesini seçin. Etkinliğin kümelerini görüntülemek için **Parametreler** ' e ve ardından **parametre kümesi** ' ne tıklayın.
1. Parametre kümesi için **Resourcegroupnameparametersetname** ' ı seçin. **Resourcegroupname** ve **Name** alanları, bunların yanında, gerekli parametreler olduğunu göstermek için ünlem işaretleri vardır. Her iki alanın de dize değerleri beklediği unutulmamalıdır.
1. **Ad**’ı seçin. **Veri kaynağı** alanı için **PowerShell ifadesi** ' ni seçin. Bu runbook 'u başlatmak için kullandığınız VM için, çift tırnak işaretleriyle çevrelenen makine adını yazın. **Tamam** düğmesine tıklayın.
1. **ResourceGroupName**’i seçin. **Veri kaynağı** alanı için **PowerShell ifadesi** değerini kullanın ve çift tırnaklarla çevrelenen kaynak grubunun adını yazın. **Tamam** düğmesine tıklayın.
1. Runbook 'u test edebilmeniz için **Test bölmesi** ' ne tıklayın.
1. Testi başlatmak için **Başlat** ' a tıklayın. Tamamlandıktan sonra, VM 'nin başlatıldığından emin olun. 

Runbook'unuzda bu noktada aşağıdakine benzer: <br>![Runbook kimlik doğrulama yapılandırması](media/automation-first-runbook-graphical/runbook-startvm.png)

## <a name="step-8---add-additional-input-parameters"></a>8\. adım-ek giriş parametreleri ekleme

Runbook 'larınız Şu anda `Start-AzVM` cmdlet 'i için belirttiğiniz kaynak grubundaki VM 'yi başlatır. Runbook başlatıldığında hem ad hem de kaynak grubu belirtirseniz runbook daha yararlı olacaktır. Bu işlevi sağlamak için Runbook 'a giriş parametreleri ekleyelim.

1. **Myfirstrunbook-grafik** bölmesinde **Düzenle** ' ye tıklayarak grafik düzenleyiciyi açın.
1. **Giriş ve çıkış** ' ı seçin ve ardından giriş ' e **ekleyerek** runbook giriş parametresi bölmesini açın.
1. Aşağıdaki ayarları, belirtilen alanlarda yapın ve ardından **Tamam**' a tıklayın.
   * **Ad** --`VMName`belirtin.
   * **Tür** --dize ayarını saklayın.
   * **Zorunlu** --değeri **Evet**olarak değiştirin.
1. `ResourceGroupName` adlı ikinci bir zorunlu giriş parametresi oluşturun ve ardından giriş ve çıkış bölmesini kapatmak için **Tamam** ' ı tıklatın.<br> ![Runbook Giriş Parametreleri](media/automation-first-runbook-graphical/start-azurermvm-params-outputs.png)
1. `Start-AzVM` etkinliğini seçin ve ardından **Parametreler**' e tıklayın.
1. **Ad** için **veri kaynağı** alanını **runbook girişi**olarak değiştirin. Ardından **VMName**' i seçin.
1. **Resourcegroupname** için **veri kaynağı** alanını **runbook girişi** olarak değiştirin ve ardından **resourcegroupname**öğesini seçin.<br> ![start-AzVM parametreleri](media/automation-first-runbook-graphical/start-azurermvm-params-runbookinput.png)
1. Runbook'u kaydedin ve Test bölmesini açın. Şimdi testte kullanılacak olan iki girdi değişkeni için değerleri sağlayabilirsiniz.
1. Test bölmesini kapatın.
1. Runbook’un yeni sürümünü yayımlamak için **Yayımla**’ya tıklayın.
1. Daha önce başlattığınız VM 'yi durdurun.
1. Runbook'u başlatmak için **Başlat**’a tıklayın. `VMName` için değerleri yazın ve başlatacaksanız kullanılacak VM için `ResourceGroupName`.
1. Runbook tamamlandığında, VM 'nin başlatıldığından emin olun.

## <a name="step-9---create-a-conditional-link"></a>9\. Adım - Koşullu bağlantı oluşturma

Artık, runbook 'u yalnızca henüz başlatılmamışsa VM 'yi başlatmaya çalışır şekilde değiştirebilirsiniz. Bunu, sanal makinenin örnek düzeyi durumunu alan bir [Get-AzVM](https://docs.microsoft.com/powershell/module/Az.Compute/Get-AzVM?view=azps-3.5.0) cmdlet 'i ekleyerek yapın. Daha sonra, VM durumunun çalışıyor veya durdurulmuş olduğunu anlamak için PowerShell kodu kod parçacığına sahip `Get Status` adlı bir PowerShell Iş akışı kodu modülü ekleyebilirsiniz. `Get Status` modülünden koşullu bağlantı yalnızca geçerli çalışma durumu durdurulmuşsa `Start-AzVM` çalışır. Bu yordamın sonunda, runbook, sanal makine başarıyla başlatıldıysa sizi bilgilendirmek üzere bir ileti çıktısı almak için `Write-Output` cmdlet 'ini kullanır.

1. Grafik düzenleyicide **Myfirstrunbook-grafik** ' i açın.
1. `Specify Subscription Id` ve `Start-AzVM` arasındaki bağlantıyı tıklayarak kaldırın ve ardından **Sil**'e basın.
1. Kitaplık denetiminde, arama alanına `Get-Az` yazın.
1. Tuvale `Get-AzVM` ekleyin.
1. `Get-AzVM` öğesini seçin ve ardından cmdlet 'in kümelerini görüntülemek için **parametre kümesi** ' ne tıklayın. 
1. **GetVirtualMachineInResourceGroupNameParamSet** parametre kümesini seçin. **Resourcegroupname** ve **ad** alanları, bunların yanında, gerekli parametreleri belirttikleri belirten ünlem işaretleri vardır. Her iki alanın de dize değerleri beklediği unutulmamalıdır.
1. **Ad**için **veri kaynağı** altında, **runbook girişi**' ni ve ardından **VMName**' i seçin. **Tamam** düğmesine tıklayın.
1. **Resourcegroupname**için **veri kaynağı** altında, **runbook girişi**' ni ve ardından **resourcegroupname**' ı seçin. **Tamam** düğmesine tıklayın.
1. **Durum**için **veri kaynağı** altında, **sabit değer**' i seçin ve ardından **doğru**. **Tamam** düğmesine tıklayın.
1. `Specify Subscription Id` `Get-AzVM`bir bağlantı oluşturun.
1. Kitaplık denetiminde, **runbook denetimi** ' ni genişletin ve tuvale **kod** ekleyin.  
1. `Get-AzVM` `Code`bir bağlantı oluşturun.  
1. `Code` ' a tıklayın ve Yapılandırma bölmesinde, **durumu almak**için etiketi değiştirin.
1. `Code` ' yi seçin ve kod Düzenleyicisi sayfası görüntülenir.  
1. Aşağıdaki kod parçacığını düzenleyici sayfasına yapıştırın.

    ```powershell-interactive
     $StatusesJson = $ActivityOutput['Get-AzVM'].StatusesText
     $Statuses = ConvertFrom-Json $StatusesJson
     $StatusOut =""
     foreach ($Status in $Statuses){
     if($Status.Code -eq "Powerstate/running"){$StatusOut = "running"}
     elseif ($Status.Code -eq "Powerstate/deallocated") {$StatusOut = "stopped"}
     }
     $StatusOut
     ```

1. `Get Status` `Start-AzVM`bir bağlantı oluşturun.<br> ![Kod Modülü ile Runbook](media/automation-first-runbook-graphical/runbook-startvm-get-status.png)  
1. Bağlantıyı seçin ve Yapılandırma bölmesinde **koşul Uygula** ' yı **Evet**olarak değiştirin. Bağlantının, hedef etkinliğin yalnızca koşulun true olarak çözümlenirse çalışacağını belirten kesikli bir çizgi haline geldiğini unutmayın.  
1. **Koşul ifadesi**için `$ActivityOutput['Get Status'] -eq "Stopped"`yazın. `Start-AzVM` artık yalnızca VM durdurulmuşsa çalışır.
1. Kitaplık denetiminde, **Cmdlet'leri** ve ardından **Microsoft.PowerShell.Utility**’yi genişletin.
1. Tuvale iki kez `Write-Output` ekleyin.
1. İlk `Write-Output` denetimi için, **Parametreler** ' e tıklayın ve **etiket** değerini VM 'nin **başlatıldığını bildir**olarak değiştirin.
1. **InputObject**Için, **veri kaynağını** **PowerShell ifadesi**olarak değiştirin ve `$VMName successfully started.`ifadeyi yazın.
1. İkinci `Write-Output` denetiminde **Parametreler** ' e tıklayın ve **etiket** değerini **VM 'ye bildir hatasıyla**değiştirin.
1. **InputObject**Için, **veri kaynağını** **PowerShell ifadesi**olarak değiştirin ve `$VMName could not start.`ifadeyi yazın.
1. `Start-AzVM` `Notify VM Started` ve `Notify VM Start Failed`bağlantı oluşturun.
1. `Notify VM Started` bağlantısını seçin ve **koşul Uygula** ' yı true olarak değiştirin.
1. **Koşul ifadesi**için `$ActivityOutput['Start-AzVM'].IsSuccessStatusCode -eq $true`yazın. Bu `Write-Output` denetimi artık yalnızca VM başarıyla başlarsa çalışır.
1. `Notify VM Start Failed` bağlantısını seçin ve **koşul Uygula** ' yı true olarak değiştirin.
1. **Koşul ifadesi** alanı için `$ActivityOutput['Start-AzVM'].IsSuccessStatusCode -ne $true`yazın. Bu `Write-Output` denetimi artık yalnızca VM başarıyla başlatılmamışsa çalışır. Runbook 'larınızın aşağıdaki görüntü gibi görünmesi gerekir: <br> ![Write-Output ile Runbook](media/automation-first-runbook-graphical/runbook-startazurermvm-complete.png)
1. Runbook'u kaydedin ve Test bölmesini açın.
1. VM 'yi durdurulan runbook 'u başlatın ve makinenin başlaması gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* Grafik yazma hakkında daha fazla bilgi için bkz. [Azure Otomasyonu 'Nda grafik yazma](automation-graphical-authoring-intro.md).
* PowerShell runbook'ları kullanmaya başlamak için bkz. [İlk PowerShell runbook’um](automation-first-runbook-textual-powershell.md).
* PowerShell Iş akışı runbook 'larını kullanmaya başlamak için bkz. [Ilk PowerShell iş akışı runbook 'Um](automation-first-runbook-textual.md).