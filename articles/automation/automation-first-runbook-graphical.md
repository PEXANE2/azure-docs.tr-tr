---
title: Azure Otomasyon'daki ilk grafik çalışma kitabım
description: Basit bir grafik runbook uygulaması oluşturma, test etme ve yayımlama adımlarını anlatan öğretici.
keywords: runbook, runbook şablonu, runbook otomasyonu, azure runbook
services: automation
ms.subservice: process-automation
ms.date: 04/13/2018
ms.topic: conceptual
ms.openlocfilehash: 6bd360b2075c337e3ed3d69d84368d16571a9335
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79536063"
---
# <a name="my-first-graphical-runbook"></a>İlk grafik runbook uygulamam

> [!div class="op_single_selector"]
> * [Grafik](automation-first-runbook-graphical.md)
> * [Powershell](automation-first-runbook-textual-powershell.md)
> * [PowerShell İş Akışı](automation-first-runbook-textual.md)
> * [Python](automation-first-runbook-textual-python2.md)
> 

Bu öğretici, Azure Automation’da bir [grafik runbook uygulaması](automation-runbook-types.md#graphical-runbooks) oluşturulmasını adım adım göstermektedir. Runbook işinin durumunu izlemeyi öğrenirken test edip yayımlayabileceğiniz basit bir runbook ile başlayın. Bu durumda, azure sanal makinesini başlatacak şekilde runbook'u değiştirin. Runbook parametreleri ve koşullu bağlantılar ekleyerek runbook daha sağlam hale getirmek için öğretici tamamlayın.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma Runbook Worker'ınızdaki Az modül yükleme yönergeleri için Azure [PowerShell Modül'üne](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)bakın. Otomasyon hesabınız için, Azure Otomasyonu'nda Azure [PowerShell modüllerini nasıl güncelleştirebileceğinizi](automation-update-azure-modules.md)kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Azure aboneliği. Henüz bir hesabınız yoksa, [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) veya ücretsiz bir [hesaba](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)kaydolabilirsiniz.
* Runbook’u tutacak ve Azure kaynaklarında kimlik doğrulamasını yapacak bir [Automation hesabı](automation-offering-get-started.md). Bu hesabın sanal makineyi başlatma ve durdurma izni olmalıdır.
* Azure sanal makinesi. Bu makineyi durup çalıştırdığınıza göre, bu bir üretim VM'si olmamalı.

## <a name="step-1---create-runbook"></a>1. Adım - Runbook oluşturma

Metni `Hello World`çıktılayan basit bir runbook oluşturarak başlayın.

1. Azure portalında, Otomasyon hesabınızı açın. 

    Automation hesabı sayfası size bu hesaptaki kaynakların hızlı bir görünümünü sağlar. Birkaç varlığınız zaten olmalıdır. Bu varlıkların çoğu otomatik olarak yeni bir Otomasyon hesabına dahil modüllerdir. Aboneliğinizle ilişkili Kimlik Bilgisi kıymetine de sahip olmalısınız.
2. Runbook listesini açmak için **Proses Otomasyonu** altındaki **Runbook'ları** seçin.
3. **Runbook Oluştur'u**seçerek yeni bir runbook oluşturun.
4. Runbook’a **MyFirstRunbook-Graphical** adını verin.
5. Bu durumda, bir [grafik runbook](automation-graphical-authoring-intro.md)oluşturmak için gidiyoruz. **Runbook türü**için **Grafik'i** seçin.<br> ![Yeni runbook](media/automation-first-runbook-graphical/create-new-runbook.png)<br>
6. Runbook’u oluşturmak için **Oluştur**’a tıklayın ve grafik düzenleyicisini açın.

## <a name="step-2---add-activities"></a>Adım 2 - Etkinlik ekleme

Düzenleyicinin sol tarafındaki Kitaplık denetimi runbook uygulamanıza eklenecek etkinlikleri seçmenizi sağlar. Runbook'tan çıkış `Write-Output` metnine bir cmdlet ekleyebilirsin.

1. Kitaplık denetiminde, arama alanında tıklatın `write-output`ve yazın. Arama sonuçları aşağıdaki resimde gösterilir. <br> ![Microsoft.PowerShell.Utility](media/automation-first-runbook-graphical/search-powershell-cmdlet-writeoutput.png)
1. Listenin aşağısına kaydırın. **Yazma-Çıktı'yı** sağ tıklatın ve **tuvale ekle'yi**seçin. Alternatif olarak, cmdlet adının yanındaki elipsleri (...) tıklatıp **tuvale ekle'yi**seçebilirsiniz.
1. Tuvalde **Write-Output** etkinliğine tıklayın. Bu eylem, etkinliği yapılandırmanızı sağlayan Yapılandırma denetim sayfasını açar.
1. **Etiket** alanı varsayılan olarak cmdlet adına değişir, ancak daha kolay bir şeyle değiştirebilirsiniz. `Write Hello World to output`Değiştirin.
1. Cmdlet parametreleri değerlerini sağlamak için **Parametreler**’e tıklayın. 

   Bazı cmdlets birden çok parametre kümeleri var ve hangisini kullanmak için seçmeniz gerekir. Bu durumda, `Write-Output` yalnızca bir parametre kümesi vardır.

1. Parametreyi `InputObject` seçin. Bu, çıktı akışına gönderilecek metni belirtmek için kullandığınız parametredir.
1. **Veri kaynağı** açılır menüsü, parametre değerini doldurmak için kullanabileceğiniz kaynaklar sağlar. Bu menüde **PowerShell ifadesini**seçin. 

   Başka bir etkinlik, otomasyon varlığı veya PowerShell ifadesi gibi kaynaklardan gelen çıktıyı kullanabilirsiniz. Bu durumda, çıktı sadece `Hello World`. Bir PowerShell ifadesi kullanabilir ve dize belirtebilirsiniz.<br>

1. **İfade** alanında, tuvale dönmek için Iki kez Tamam yazın `Hello World` ve sonra **Tamam'ı** tıklatın.
1. **Kaydet**’e tıklayarak runbook’u kaydedin.

## <a name="step-3---test-the-runbook"></a>3. Adım - Runbook'u test etme

Üretimde kullanılabilir hale getirmek için runbook'u yayımlamadan önce, düzgün çalıştığından emin olmak için kitabı sınamalısınız. Bir runbook'u test etmek Taslak sürümünü çalıştırIr ve çıktısını etkileşimli olarak görüntülemenize olanak tanır.

1. Test bölmesini açmak için **Test bölmesini** seçin.
1. Testi başlatmak için **Başlat**’a tıklayın. Etkinleştirilen tek seçenek bu olmalıdır.
1. [Runbook işinin](automation-runbook-execution.md) oluşturulduğunu ve durumunun bölmede görüntülendiğini unutmayın.

   İş durumu , `Queued`işin buluttaki bir runbook çalışanının kullanılabilir olmasını beklediğini belirten olarak başlar. Durum, bir `Starting` işçinin işi talep etmesiyle değişir. Son olarak, `Running` durum runbook gerçekten çalıştırmaya başladığında olur.

1. Runbook işi tamamlandığında, Test bölmesi çıktısını görüntüler. Bu durumda, görüyorsunuz `Hello World`.

    ![Hello World](media/automation-first-runbook-graphical/runbook-test-results.png)
1. Tuvale geri dönmek için Test bölmesini kapatın.

## <a name="step-4---publish-and-start-the-runbook"></a>4. Adım - Runbook’u yayımlama ve başlatma

Oluşturduğunuz runbook hala Taslak modundadır. Üretimde çalıştırılabilmesi için yayımlanması gerekir. Bir runbook yayımladığınızda, Taslak sürümü mevcut Yayımlanmış sürümün üzerine yazarsınız. Örneğimizde, runbook’u henüz oluşturduğunuzdan, Yayımlanmış sürümünüz yok.

1. Runbook'u yayımlamak için **Yayımla'yı** ve istendiğinde **Evet'i** seçin.
1. Runbook sayfasındarunbook'u görüntülemek için sola kaydırın ve **Yazarlık Durumu** değerinin **Yayımlanmış**olarak ayarlandığına dikkat edin.
1. **MyFirstRunbook-Graphical**için sayfayı görüntülemek için sağa geri kaydırın.

   Üstteki seçenekler, runbook'u şimdi başlatmanızı, gelecekteki bir başlangıç saatini zamanlamanızı veya runbook'un http araması yoluyla başlatılabilmesi için bir [webhook](automation-webhooks.md) oluşturmanıza olanak sağlar.

1. **Runbook'u** başlatmak istendiğinde Başlat ve ardından **Evet'i** seçin.
1. Oluşturulan runbook işi için bir İş bölmesi açılır. **İş durumu** alanının **Tamamlandı'yı**gösterdiğini doğrulayın.
1. Görüntülenen görmek `Hello World` için Çıktı sayfasını açmak için **Çıktı'yı** tıklatın.
1. Çıktı sayfasını kapatın.
1. Runbook işine ait Akışlar bölmesini açmak için **Tüm Günlükler**’e tıklayın. Yalnızca çıkış `Hello World` akışında görmeniz gerekir. 

    Runbook onlara yazıyorsa, Akışlar bölmesinin verbose ve hata akışları gibi bir runbook işi için diğer akışları gösterebileceğini unutmayın.
1. **MyFirstRunbook-Graphical** sayfasına dönmek için Akışlar bölmesini ve İş bölmesini kapatın.
1. Runbook'un tüm işlerini görüntülemek **için, Kaynaklar**altında **İşler'i** seçin. İşler sayfası, runbook'unuzun oluşturduğu tüm işleri listeler. İşi yalnızca bir kez çalıştırdığınızdan, yalnızca bir iş listelenmiş olarak görmeniz gerekir.
1. Runbook'u başlattığınızda görüntülediğiniz aynı İş bölmesini açmak için iş adını tıklatın. Runbook için oluşturulan herhangi bir işin ayrıntılarını görüntülemek için bu bölmeyi kullanın.

## <a name="step-5---create-variable-assets"></a>5. Adım - Değişken varlıkları oluşturma

Runbook'unuzu test ettiniz ve yayımladınız, ancak şimdiye kadar Azure kaynaklarını yönetmek için yararlı bir şey yapmadı. Runbook'u kimlik doğrulaması için yapılandırmadan önce, abonelik kimliğini tutmak, kimlik doğrulaması yapmak için bir etkinlik ayarlamak ve sonra değişkene başvurmak için bir değişken oluşturmanız gerekir. Abonelik bağlamına bir başvuru da dahil olmak üzere, birden çok abonelikle kolayca çalışmanızı sağlar.

1. Abonelik kimliğinizi Gezinti bölmesindeki **Abonelikler** seçeneğinden kopyalayın.
1. Otomasyon Hesapları sayfasında, **Paylaşılan Kaynaklar**altında **Değişkenler'i** seçin.
1. **Değişken ekle'yi**seçin.
1. Yeni değişken sayfasında, sağlanan alanlarda aşağıdaki ayarları yapın.

    * **Ad** -- `AzureSubscriptionId`girin .
    * **Değer** -- abonelik kimliğinizi girin. 
    * **Yazın** -- dizeyi seçili tutun.
    * **Şifreleme** -- varsayılan değeri kullanın.
1. Değişkeni oluşturmak için **Oluştur**’a tıklayın.

## <a name="step-6---add-authentication"></a>Adım 6 - Kimlik doğrulama ekleme

Artık abonelik kimliğini tutmak için bir değişkeniniz olduğuna göre, runbook'u aboneliğiniz için Run As kimlik bilgileriyle kimlik doğrulaması yapacak şekilde yapılandırabilirsiniz. Bunu, Varlık Olarak Azure Çalıştır bağlantısını ekleyerek yapın. [Ayrıca connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/Connect-AzAccount?view=azps-3.5.0) cmdlet ve [Set-AzContext](https://docs.microsoft.com/powershell/module/az.accounts/Set-AzContext?view=azps-3.5.0) cmdlet'i tuvale eklemeniz gerekir.

>[!NOTE]
>PowerShell runbook'ları için `Add-AzAccount` ve `Add-AzureRMAccount` `Connect-AzAccount`diğer adlar . Bu diğer adların grafik çalışma kitaplarınız için kullanılamadığını unutmayın. Grafik çalışma kitabı yalnızca `Connect-AzAccount`kendisini kullanabilir.

1. Runbook'unuza gidin ve **MyFirstRunbook-Graphical** sayfasında **Edit'i** seçin.
1. Artık girişe `Write Hello World to output` ihtiyacın yok. Elipsleri tıklatın ve **Sil'i**seçin.
1. Kitaplık denetiminde, **VARLıKLAR'ı,** ardından **Bağlantıları**genişletin. Tuvale Ekle'yi seçerek `AzureRunAsConnection` **tuvale**ekleyin.
1. Yeniden `AzureRunAsConnection` adlandırmak `Get Run As Connection`için.
1. Kitaplık denetiminde, `Connect-AzAccount` arama alanına yazın.
1. Tuvale ekle. `Connect-AzAccount`
1. Şeklin `Get Run As Connection` alt kısmında bir daire belirene kadar yukarı doğru yuvarlanın. Daireyi tıklatın ve bir `Connect-AzAccount` bağlantı oluşturmak için oku sürükleyin. Runbook ile `Get Run As Connection` başlar ve `Connect-AzAccount`sonra çalışır.<br> ![Etkinlikler arasında bağlantı oluşturma](media/automation-first-runbook-graphical/runbook-link-auth-activities.png)
1. Tuval üzerinde, `Connect-AzAccount`seçin. Yapılandırma denetim bölmesinde, **Etiket** alanına **Azure'a Giriş** yazın.
1. **Parametreler'i**tıklatın ve Etkinlik Parametre Yapılandırması sayfası görüntülenir.
1. Cmdlet birden `Connect-AzAccount` çok parametre kümesine sahiptir ve parametre değerleri sağlamadan önce bir tane seçmeniz gerekir. **Parametre Kümesi'ni** tıklatın ve ardından **ServicePrincipalCertificateWithSubscriptionId'i**seçin.
1. Bu parametre kümesinin parametreleri Etkinlik Parametresi Yapılandırma sayfasında görüntülenir. **APPLICATIONID**’ye tıklayın.<br> ![Azure hesap parametreleri ekleme](media/automation-first-runbook-graphical/Add-AzureRmAccount-params.png)
1. Parametre Değeri sayfasında aşağıdaki ayarları yapın ve ardından **Tamam'ı**tıklatın.

   * **Veri kaynağı** -- **Etkinlik çıktısını**seçin.
   * Veri kaynak listesi -- **Otomasyon Bağlantısı Al'ı**seçin.
   * **Alan yolu** `ApplicationId`-- yazın. Etkinlik birden çok özelliği olan bir nesne çıktıları, alan yolu için özelliğin adını belirtirsiniz.

1. **CERTIFICATETHUMBPRINT'i**ve Parametre Değeri sayfasında aşağıdaki ayarları yapın ve ardından **Tamam'ı**tıklatın.

    * **Veri kaynağı** -- **Etkinlik çıktısını**seçin.
    * Veri kaynak listesi -- **Otomasyon Bağlantısı Al'ı**seçin.
    * **Alan yolu** `CertificateThumbprint`-- yazın.
1. **SERVICEPRINCIPAL'ı**tıklatın ve Parametre Değeri sayfasında **Veri kaynak** alanı için **ConstantValue'ı** seçin; **True**seçeneğini tıklatın ; ve sonra **Tamam'ı**tıklatın.
1. **TENANTID'yi**tıklatın ve Parametre Değeri sayfasında aşağıdaki ayarları yapın. Bittiğinde, Iki kez **Tamam'ı** tıklatın.

    * **Veri kaynağı** -- **Etkinlik çıktısını**seçin. 
    * Veri kaynak listesi -- **Otomasyon Bağlantısı Al'ı**seçin.
    * **Alan yolu** `TenantId`-- yazın. 
1. Kitaplık denetiminde, `Set-AzContext` arama alanına yazın.
1. Tuvale ekle. `Set-AzContext`
1. Tuvalüzerinde seçin. `Set-AzContext` Yapılandırma denetim bölmesine, `Specify Subscription Id` **Etiket** alanına girin.
1. **Parametreler'i** tıklatın ve Etkinlik Parametre Yapılandırmasayfası görüntülenir.
1. Cmdlet birden `Set-AzContext` çok parametre kümesine sahiptir ve parametre değerleri sağlamadan önce bir tane seçmeniz gerekir. **Parametre Kümesi'ni** tıklatın ve ardından **SubscriptionId'i**seçin.
1. Bu parametre kümesinin parametreleri Etkinlik Parametresi Yapılandırma sayfasında görüntülenir. **SubscriptionID'yi**tıklatın.
1. Parametre Değeri sayfasında, **Veri kaynak** alanı için **Değişken Varlık'ı** seçin ve kaynak listeden **AzureSubscriptionId'i** seçin. Bittiğinde, Iki kez **Tamam'ı** tıklatın.
1. Şeklin `Login to Azure` alt kısmında bir daire belirene kadar yukarı doğru yuvarlanın. Daireyi tıklatın ve oku `Specify Subscription Id`' ya sürükleyin. Runbook bu noktada aşağıdaki gibi görünmelidir.

    ![Runbook kimlik doğrulama yapılandırması](media/automation-first-runbook-graphical/runbook-auth-config.png)

## <a name="step-7---add-activity-to-start-a-virtual-machine"></a>7. Adım - Sanal makineyi başlatmak üzere etkinlik ekleme

Şimdi sanal bir `Start-AzVM` makine başlatmak için bir etkinlik eklemeniz gerekir. Azure aboneliğinizde herhangi bir VM seçebilirsiniz ve şimdilik adını [Start-AzVM](https://docs.microsoft.com/powershell/module/az.compute/start-azvm?view=azps-3.5.0) cmdlet'ine sabit lazim.

1. Kitaplık denetiminde, `Start-Az` arama alanına yazın.
2. Tuvale ekleyin `Start-AzVM` ve sonra tıklayın ve `Specify Subscription Id`altına sürükleyin.
1. Şeklin `Specify Subscription Id` alt kısmında bir daire belirene kadar yukarı doğru yuvarlanın. Daireyi tıklatın ve oku `Start-AzVM`' ya sürükleyin.
1. `Start-AzVM` öğesini seçin. Etkinlik kümelerini görüntülemek için **Parametreler'i** ve ardından **Parametre Kümesi'ni** tıklatın.
1. Parametre kümesi için **ResourceGroupNameParameterSetName'i** seçin. Kaynak **Grubu Adı** ve **Adı** alanları, gerekli parametreler olduğunu belirtmek için yanlarında ünlem işaretleri içerir. Her iki alanın da dize değerleri beklediğini unutmayın.
1. **Ad**’ı seçin. **Veri kaynak** alanı için **PowerShell ifadesini** seçin. Bu runbook'u başlatmak için kullandığınız VM için, çift tırnak la çevrili makine adını yazın. **Tamam**'a tıklayın.
1. **ResourceGroupName**’i seçin. **Veri kaynak** alanı için **PowerShell** değerini kullanın ve çift tırnak la çevrili kaynak grubunun adını yazın. **Tamam**'a tıklayın.
1. Çalışma kitabını test etmek için **Test bölmesini** tıklatın.
1. Teste başlamak için **Başlat'ı** tıklatın. Tamamlandıktan sonra, VM'nin başladığından emin olun. Runbook bu noktada aşağıdaki gibi görünmelidir.

    ![Runbook kimlik doğrulama yapılandırması](media/automation-first-runbook-graphical/runbook-startvm.png)

## <a name="step-8---add-additional-input-parameters"></a>Adım 8 - Ek giriş parametreleri ekleme

Runbook'unuz şu anda cmdlet için `Start-AzVM` belirlediğiniz kaynak grubunda VM'yi başlatır. Runbook başlatıldığında hem ad hem de kaynak grubu belirtirseniz runbook daha kullanışlı olacaktır. Bu işlevselliği sağlamak için runbook'a giriş parametreleri ekleyelim.

1. **MyFirstRunbook-Graphical** bölmesine **tıklayarak** grafik düzenleyicisini açın.
1. **Giriş ve çıktıyı** seçin ve runbook Giriş Parametre bölmesini açmak için **giriş ekleyin.**
1. Sağlanan alanlarda aşağıdaki ayarları yapın ve sonra **Tamam'ı**tıklatın.
   * **Ad** -- `VMName`belirtin.
   * **Yazın** -- dize ayarını koruyun.
   * **Zorunlu** -- değeri **Evet**olarak değiştirin.
1. İkinci bir zorunlu giriş parametresi adlı `ResourceGroupName` oluşturun ve sonra Giriş ve Çıktı bölmesini kapatmak için **Tamam'ı** tıklatın.<br> ![Runbook Giriş Parametreleri](media/automation-first-runbook-graphical/start-azurermvm-params-outputs.png)
1. `Start-AzVM` Etkinliği seçin ve ardından **Parametreler'i**tıklatın.
1. **Runbook** girişi için Ad için **Veri kaynak** alanını **değiştirin.** Sonra **VMName**seçin.
1. **ResourceGroupName** için **Veri kaynak** alanını **Runbook girişine** değiştirin ve ardından **ResourceGroupName'i**seçin.<br> ![Başlangıç-AzVM Parametreleri](media/automation-first-runbook-graphical/start-azurermvm-params-runbookinput.png)
1. Runbook'u kaydedin ve Test bölmesini açın. Şimdi testte kullanılacak olan iki girdi değişkeni için değerleri sağlayabilirsiniz.
1. Test bölmesini kapatın.
1. Runbook’un yeni sürümünü yayımlamak için **Yayımla**’ya tıklayın.
1. Daha önce başlattığınız VM'yi durdurun.
1. Runbook'u başlatmak için **Başlat**’a tıklayın. Başlatacağınız VM için `VMName` ve `ResourceGroupName` için değerleri yazın.
1. Runbook tamamlandığında, VM'nin başlatıldığından emin olun.

## <a name="step-9---create-a-conditional-link"></a>9. Adım - Koşullu bağlantı oluşturma

Artık runbook'u yalnızca zaten başlatılmazsa VM'yi başlatmaya çalışacak şekilde değiştirebilirsiniz. Bunu, VM'nin örnek düzeyi durumunu alan bir [Get-AzVM](https://docs.microsoft.com/powershell/module/Az.Compute/Get-AzVM?view=azps-3.5.0) cmdlet ekleyerek yapın. Ardından, VM durumunun çalışıp `Get Status` çalışmadığınızı veya durdurulup durdurulmada olmadığını belirlemek için PowerShell kodu parçacıklarıyla birlikte çağrılan bir PowerShell İş Akışı kodu modülü ekleyebilirsiniz. `Get Status` Modülden koşullu bir bağlantı `Start-AzVM` yalnızca geçerli çalışan durum durdurulursa çalışır. Bu yordamın sonunda, runbook'unuzun `Write-Output` cmdlet'i kullanması, VM'nin başarıyla başlatıldığında sizi bilgilendirmek için bir ileti yayımlamak için kullanır.

1. Grafik düzenleyicisinde **MyFirstRunbook-Graphical'ı** açın.
1. Arasındaki `Specify Subscription Id` bağlantıyı kaldırın `Start-AzVM` ve üzerine tıklayarak ve sonra **Sil**tuşuna basarak.
1. Kitaplık denetiminde, `Get-Az` arama alanına yazın.
1. Tuvale ekle. `Get-AzVM`
1. Cmdlet kümelerini görüntülemek için Parametre Kümesi'ni seçin `Get-AzVM` ve ardından **Parametre Kümesi'ni** tıklatın. 
1. **GetVirtualMachineInResourceGroupNameParamSet** parametre kümesini seçin. **Kaynak Grubu Adı** ve **Ad** alanlarının yanında, gerekli parametreleri belirttiklerini belirten ünlem işaretleri vardır. Her iki alanın da dize değerleri beklediğini unutmayın.
1. **Ad**için **Veri kaynağı** altında, **Runbook girişi**seçin, sonra **VMName**. **Tamam**'a tıklayın.
1. **ResourceGroupName**için **Veri kaynağı** altında **Runbook girişi**seçin, sonra **ResourceGroupName**. **Tamam**'a tıklayın.
1. **Durum**için **Veri kaynağı** altında, **Sabit değeri**seçin, sonra **True**. **Tamam**'a tıklayın.
1. 'den `Specify Subscription Id` ' `Get-AzVM`e bir bağlantı oluşturun
1. Kitaplık denetiminde **Runbook Denetimi'ni** genişletin ve tuvale **Kod** ekleyin.  
1. 'den `Get-AzVM` ' `Code`e bir bağlantı oluşturun  
1. Yapılandırma `Code` bölmesinde etiketi tıklatın ve Durumu **Al'ı**değiştirin.
1. Seçin `Code` ve Kod Düzenleyicisi sayfası görüntülenir.  
1. Aşağıdaki kod parçacıklarını editör sayfasına yapıştırın.

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

1. 'den `Get Status` ' `Start-AzVM`e bir bağlantı oluşturun

    ![Kod Modülü ile Runbook](media/automation-first-runbook-graphical/runbook-startvm-get-status.png)  
1. Bağlantıyı seçin ve Yapılandırma bölmesinde Uygula **koşulunu** **Evet**olarak değiştirin. Bağlantının kesik çizgili bir çizgi ye dönüştüğünü ve hedef etkinliğin yalnızca koşul doğru giderici olduğunda çalıştığını belirteceğini unutmayın.  
1. **Koşul ifadesi**için `$ActivityOutput['Get Status'] -eq "Stopped"`, yazın. `Start-AzVM`şimdi yalnızca VM durdurulursa çalışır.
1. Kitaplık denetiminde, **Cmdlet'leri** ve ardından **Microsoft.PowerShell.Utility**’yi genişletin.
1. Tuvale iki kez ekleyin. `Write-Output`
1. İlk `Write-Output` denetim için **Parametreler'i** tıklatın ve **Başlatılan VM'yi Bildirmek**için **Etiket** değerini değiştirin.
1. **InputObject**için, **PowerShell ifadesi** **için Veri kaynağı** `$VMName successfully started.`değiştirin ve ifade yazın.
1. İkinci `Write-Output` denetimde Parametreler'i **tıklatın** ve **VM Start Failed'ü bildirmek**için **Etiket** değerini değiştirin.
1. **InputObject**için, **PowerShell ifadesi** **için Veri kaynağı** `$VMName could not start.`değiştirin ve ifade yazın.
1. ve. `Start-AzVM` için `Notify VM Started` bağlantılar oluşturun `Notify VM Start Failed`
1. Bağlantıseçin `Notify VM Started` ve **uygula koşulunu** doğru olarak değiştirin.
1. Durum **ifadesi**için `$ActivityOutput['Start-AzVM'].IsSuccessStatusCode -eq $true`, yazın. Bu `Write-Output` denetim artık yalnızca VM başarıyla başlarsa çalışır.
1. Bağlantıseçin `Notify VM Start Failed` ve **uygula koşulunu** doğru olarak değiştirin.
1. Durum **ifadesi** alanı için `$ActivityOutput['Start-AzVM'].IsSuccessStatusCode -ne $true`. Bu `Write-Output` denetim şimdi yalnızca VM başarıyla başlatılmazsa çalışır. Runbook'unuz aşağıdaki resim gibi görünmelidir.

    ![Write-Output ile Runbook](media/automation-first-runbook-graphical/runbook-startazurermvm-complete.png)
1. Runbook'u kaydedin ve Test bölmesini açın.
1. VM durdurularak runbook'u çalıştırın ve makinenin başlaması gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* Grafik yazma hakkında daha fazla bilgi edinmek için [Azure Otomasyonu'nda Grafik yazma'ya](automation-graphical-authoring-intro.md)bakın.
* PowerShell runbook'ları ile başlamak için [ilk PowerShell runbook'uma](automation-first-runbook-textual-powershell.md)bakın.
* PowerShell İş Akışı runbook'larına başlamak için [ilk PowerShell iş akışı çalışma kitabıma](automation-first-runbook-textual.md)bakın.