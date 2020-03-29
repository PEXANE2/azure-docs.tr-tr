---
title: Azure Otomasyonu'nda runbook'ları yönetme
description: Bu makalede, Azure Otomasyonu'nda runbook'ların nasıl yönetilenbir şekilde yönetilen.
services: automation
ms.subservice: process-automation
ms.date: 02/14/2019
ms.topic: conceptual
ms.openlocfilehash: ad8c05b3347ed4741d574a5e6bcc1d928db08411
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79366845"
---
# <a name="manage-runbooks-in-azure-automation"></a>Azure Otomasyonu'nda runbook'ları yönetme

Yeni bir runbook [oluşturarak](#create-a-runbook) veya bir dosyadan veya [Runbook Galerisi'nden](automation-runbook-gallery.md)varolan bir kitabı içe [aktararak](#import-a-runbook) Azure Otomasyonu'na bir runbook ekleyebilirsiniz. Bu makalede, bir dosyadan runbook oluşturma ve alma hakkında bilgi sağlar. [Azure Otomasyonu için Runbook ve modül galerilerinde](automation-runbook-gallery.md)topluluk runbook'larına ve modüllerine erişmenin tüm ayrıntılarını alabilirsiniz.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma Runbook Worker'ınızdaki Az modül yükleme yönergeleri için Azure [PowerShell Modül'üne](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)bakın. Otomasyon hesabınız için, Azure Otomasyonu'nda Azure [PowerShell modüllerini nasıl güncelleştirebileceğinizi](automation-update-azure-modules.md)kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="create-a-runbook"></a>Runbook oluşturma

Azure portallarından birini veya Windows PowerShell'i kullanarak Azure Otomasyonu'nda yeni bir runbook oluşturabilirsiniz. Runbook oluşturulduktan sonra, [Öğrenme PowerShell İş Akışı](automation-powershell-workflow.md) ve Azure [Otomasyonu'nda Grafik sel](automation-graphical-authoring-intro.md)yazma bilgileri kullanarak kitabı yeniden ayarlayabilirsiniz.

### <a name="create-a-runbook-in-the-azure-portal"></a>Azure portalında runbook oluşturma

1. Azure portalında, Otomasyon hesabınızı açın.
2. Merkezden, runbook listesini açmak için **Proses Otomasyonu** altındaki **Runbook'ları** seçin.
3. **Runbook Oluştur'u**tıklatın.
4. Runbook için bir ad girin ve [Türünü](automation-runbook-types.md)seçin. Runbook adı bir harfle başlamalı ve harfler, sayılar, alt çizgi ve tireler içerebilir.
5. Runbook'u oluşturmak ve düzenleyiciyi açmak için **Oluştur'u** tıklatın.

### <a name="create-a-runbook-with-powershell"></a>PowerShell ile runbook oluşturma

Boş bir [PowerShell İş Akışı runbook](automation-runbook-types.md#powershell-workflow-runbooks)oluşturmak için [Yeni-AzAutomationRunbook](https://docs.microsoft.com/powershell/module/az.automation/new-azautomationrunbook?view=azps-3.5.0) cmdlet kullanabilirsiniz. ' `Type` için tanımlanan runbook türlerinden birini `New-AzAutomationRunbook`belirtmek için parametreyi kullanın

Aşağıdaki örnekte, yeni bir boş runbook nasıl oluşturulacak gösterilmektedir.

```azurepowershell-interactive
New-AzAutomationRunbook -AutomationAccountName MyAccount `
-Name NewRunbook -ResourceGroupName MyResourceGroup -Type PowerShell
```

## <a name="import-a-runbook"></a>Runbook alma

Bir PowerShell komut dosyası veya PowerShell İş Akışı **(.ps1),** dışa aktarılan grafik runbook **(.graphrunbook)** veya Python 2 komut dosyası (**.py)** içe aktararak Azure Otomasyonu'nda yeni bir runbook oluşturabilirsiniz.  Alma sırasında oluşturulan [runbook türünü](automation-runbook-types.md) aşağıdaki hususları dikkate alarak belirtmeniz gerekir.

* İş akışı içermeyen bir **.ps1** dosyası [PowerShell runbook](automation-runbook-types.md#powershell-runbooks) veya [PowerShell Workflow runbook'a](automation-runbook-types.md#powershell-workflow-runbooks)aktarılabilir. Bir PowerShell İş Akışı runbook içine içe aktarSanız, bir iş akışına dönüştürülür. Bu durumda, yapılan değişiklikleri açıklamak için açıklamalar runbook'a dahil edilir.

* PowerShell İş Akışı içeren bir **.ps1** dosyası yalnızca [PowerShell İş Akışı runbook'una](automation-runbook-types.md#powershell-workflow-runbooks)aktarılabilir. Dosya birden çok PowerShell iş akışı içeriyorsa, içe aktarma başarısız olur. Her iş akışını kendi dosyasına kaydetmeniz ve her biri ayrı ayrı içe aktarmanız gerekir.

* PowerShell İş Akışı içeren bir **.ps1** dosyası, PowerShell komut dosyası altyapısı tanıyamadığı için [PowerShell runbook'a](automation-runbook-types.md#powershell-runbooks)alınmamalıdır.

* Bir **.graphrunbook** dosyası yalnızca yeni bir [grafik runbook'a](automation-runbook-types.md#graphical-runbooks)aktarılabilir. Yalnızca **.graphrunbook** dosyasından grafik çalışma kitabı oluşturabileceğinizi unutmayın.

### <a name="import-a-runbook-from-a-file-with-the-azure-portal"></a>Azure portalı olan bir dosyadan runbook alma

Bir komut dosyası dosyasını Azure Otomasyonuna almak için aşağıdaki yordamı kullanabilirsiniz.

> [!NOTE]
> Portalı kullanarak bir **.ps1** dosyasını powershell iş akışı runbook'una yalnızca içe aktarabilirsiniz.

1. Azure portalında, Otomasyon hesabınızı açın.
2. Merkezden, runbook listesini açmak için **Proses Otomasyonu** altındaki **Runbook'ları** seçin.
3. **Runbook'u içe aktar'ı**tıklatın.
4. **Runbook dosyasını** tıklatın ve içe aktarılabilmek için dosyayı seçin.
5. **Ad** alanı etkinse, runbook adını değiştirme seçeneğiniz olur. Ad bir harfle başlamalı ve harfler, sayılar, alt çizerler ve tireler içerebilir.
6. [Runbook türü](automation-runbook-types.md) otomatik olarak seçilir, ancak geçerli kısıtlamaları dikkate aldıktan sonra türü değiştirebilirsiniz.
7. **Oluştur'u**tıklatın. Yeni runbook, Otomasyon hesabının runbook'ları listesinde görünür.
8. Çalıştıramadan önce [runbook'u yayımlamanız](#publish-a-runbook) gerekir.

> [!NOTE]
> Bir grafik çalışma kitabı veya grafiksel PowerShell İş Akışı çalışma kitabı aldıktan sonra, bunu başka bir türe dönüştürebilirsiniz. Ancak, bu grafik çalışma kitaplarından birini metin çalışması kitabına dönüştüremezsiniz.

### <a name="import-a-runbook-from-a-script-file-with-windows-powershell"></a>Windows PowerShell ile komut dosyası dosyasından runbook alma

Taslak PowerShell İş Akışı runbook olarak bir komut dosyası dosyası almak için [İthalat-AzAutomationRunbook](https://docs.microsoft.com/powershell/module/az.automation/import-azautomationrunbook?view=azps-3.5.0) cmdlet kullanın. Runbook zaten varsa, cmdlet ile `Force` parametre kullanmadığınız sürece içe aktarma başarısız olur.

Aşağıdaki örnek, komut dosyası dosyasının runbook'a nasıl aktarılsüreceğini gösterir.

```azurepowershell-interactive
$automationAccountName =  "AutomationAccount"
$runbookName = "Sample_TestRunbook"
$scriptPath = "C:\Runbooks\Sample_TestRunbook.ps1"
$RGName = "ResourceGroup"

Import-AzAutomationRunbook -Name $runbookName -Path $scriptPath `
-ResourceGroupName $RGName -AutomationAccountName $automationAccountName `
-Type PowerShellWorkflow
```

## <a name="test-a-runbook"></a>Bir runbook'u test etme

Bir runbook sınadığınızda, [Taslak sürümü](#publish-a-runbook) yürütülür ve gerçekleştirdiği tüm eylemler tamamlanır. İş geçmişi oluşturulmaz, ancak [Çıktı](automation-runbook-output-and-messages.md#output-stream) ve [Uyarı ve Hata](automation-runbook-output-and-messages.md#message-streams) akışları Test çıktı bölmesinde görüntülenir. [Verbose akışına](automation-runbook-output-and-messages.md#message-streams) iletiler yalnızca `VerbosePreference` değişken](otomasyon-runbook-output-and-messages.md#preference-variables) Devam olarak ayarlanmışsa Çıktı bölmesinde görüntülenir.

Taslak sürüm çalıştırılsa bile, runbook yine de normal olarak yürütülür ve ortamdaki kaynaklara karşı herhangi bir eylem gerçekleştirir. Bu nedenle, çalışma kitaplarını yalnızca üretim dışı kaynaklarda sınamalısınız.

Her [runbook türünü](automation-runbook-types.md) sınamak için yordam aynıdır. Azure portalındaki metin düzenleyicisi ile grafik düzenleyicisi arasında test etme arasında hiçbir fark yoktur.

1. Metin [düzenleyicisinde](automation-edit-textual-runbook.md) veya [grafik düzenleyicisinde](automation-graphical-authoring-intro.md)runbook'un Taslak sürümünü açın.
1. Test sayfasını açmak için **Test** düğmesini tıklatın.
1. Runbook parametreleri varsa, bunlar test için kullanılacak değerleri sağlayabilir sol bölmesinde listelenir.
1. Testi [Karma Runbook Worker'da](automation-hybrid-runbook-worker.md)çalıştırmak istiyorsanız, **Çalışma Ayarlarını** **Karma İşçi** olarak değiştirin ve hedef grubun adını seçin.  Aksi takdirde, testi bulutta çalıştırmak için varsayılan **Azure'u** saklayın.
1. Teste başlamak için **Başlat** düğmesini tıklatın.
1. Test edilirken [PowerShell İş Akışını](automation-runbook-types.md#powershell-workflow-runbooks) veya [grafik çalışma](automation-runbook-types.md#graphical-runbooks) kitabını durdurmak veya askıya almak için Çıktı bölmesinin altındaki düğmeleri kullanabilirsiniz. Bir runbook'u askıya aldığınızda, askıya alınmadan önce geçerli etkinliği tamamlar. Runbook askıya alındığında, durdurabilir veya yeniden başlatabilirsiniz.
1. Çıktı bölmesinde runbook'tan çıktıyı inceleyin.

## <a name="publish-a-runbook"></a>Runbook yayımlama

Yeni bir runbook oluşturduğunuzda veya içe aktardığınızda, çalıştıramadan önce kitabı yayımlamanız gerekir. Azure Otomasyonu'ndaki her runbook'un taslak sürümü ve Yayımlanmış sürümü vardır. Yalnızca Yayımlanan sürüm çalıştırılabilir ve yalnızca Taslak sürüm düzenlenebilir. Yayımlanan sürüm Taslak sürümdeki herhangi bir değişiklikten etkilenmez. Taslak sürümü kullanıma sunulduğunda, geçerli Yayımlanmış sürümün üzerine Taslak sürümüyle birlikte yayınlanır.

### <a name="publish-a-runbook-in-the-azure-portal"></a>Azure portalında bir runbook yayımlama

1. Azure portalında runbook'u açın.
2. **Düzenle**’ye tıklayın.
3. Doğrulama iletisine yanıt olarak **Yayımla'yı** ve ardından **Evet'i** tıklatın.

### <a name="publish-a-runbook-using-powershell"></a>PowerShell'i kullanarak runbook yayımlama

Windows PowerShell içeren bir runbook yayınlamak için [Publish-AzAutomationRunbook](https://docs.microsoft.com/powershell/module/Az.Automation/Publish-AzAutomationRunbook?view=azps-3.5.0) cmdlet'i kullanın. Aşağıdaki örnek, örnek bir runbook'un nasıl yayımlandırılabildiğini gösterir.

```azurepowershell-interactive
$automationAccountName =  "AutomationAccount"
$runbookName = "Sample_TestRunbook"
$RGName = "ResourceGroup"

Publish-AzAutomationRunbook -AutomationAccountName $automationAccountName `
-Name $runbookName -ResourceGroupName $RGName
```

## <a name="next-steps"></a>Sonraki adımlar

* Runbook ve PowerShell Modül Galerisi'nden nasıl yararlanabileceğinizi öğrenmek [için Azure Otomasyonu için Runbook ve modül galerilerine](automation-runbook-gallery.md)bakın.
* Metin düzenleyicisi olan PowerShell ve PowerShell İş Akışı çalışma kitaplarını düzenleme hakkında daha fazla bilgi edinmek için [Azure Otomasyonu'nda metin çalışma kitaplarını düzenleme'ye](automation-edit-textual-runbook.md)bakın.
* Grafik çalışma kitabı yazma hakkında daha fazla bilgi edinmek için [Azure Otomasyonu'nda Grafik selyazma kitabına](automation-graphical-authoring-intro.md)bakın.
