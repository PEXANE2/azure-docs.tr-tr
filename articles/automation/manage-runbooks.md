---
title: Azure Otomasyonu 'nda runbook 'ları yönetme
description: Bu makalede, Azure Otomasyonu 'nda runbook 'ların nasıl yönetileceği açıklanır.
services: automation
ms.subservice: process-automation
ms.date: 02/14/2019
ms.topic: conceptual
ms.openlocfilehash: ec53c4b2f80fb095f58bee9c15ac5daafb8d59ef
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79278382"
---
# <a name="manage-runbooks-in-azure-automation"></a>Azure Otomasyonu 'nda runbook 'ları yönetme

[Yeni bir tane oluşturarak](#create-a-runbook) veya var olan bir dosyayı ya da [runbook galerisinden](automation-runbook-gallery.md)bir runbook 'U [içeri aktararak](#import-a-runbook) , Azure Otomasyonu 'na bir runbook ekleyebilirsiniz. Bu makalede, runbook 'ları bir dosyadan oluşturma ve içeri aktarma hakkında bilgi sağlanır. [Azure Otomasyonu Için runbook ve modül galerilerinde](automation-runbook-gallery.md)topluluk runbook 'larına ve modüllerine erişmenin tüm ayrıntılarını alabilirsiniz.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma runbook çalışanınız hakkında az Module yükleme yönergeleri için bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Otomasyon hesabınız için, [Azure Otomasyonu 'nda Azure PowerShell modüllerini güncelleştirme](automation-update-azure-modules.md)' yi kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="create-a-runbook"></a>Runbook oluşturma

Azure portalında veya Windows PowerShell 'den birini kullanarak Azure Otomasyonu 'nda yeni bir runbook oluşturabilirsiniz. Runbook oluşturulduktan sonra, [öğrenme PowerShell Iş akışı](automation-powershell-workflow.md) ve [Azure Otomasyonu 'nda grafik yazma](automation-graphical-authoring-intro.md)bilgilerini kullanarak düzenleyebilirsiniz.

### <a name="create-a-runbook-in-the-azure-portal"></a>Azure portal runbook oluşturma

1. Azure portalında, Otomasyon hesabınızı açın.
2. Hub 'dan runbook listesini açmak için **Işlem Otomasyonu** altında **runbook 'lar** ' ı seçin.
3. **Runbook oluştur ' a**tıklayın.
4. Runbook için bir ad girin ve [türünü](automation-runbook-types.md)seçin. Runbook adı bir harfle başlamalıdır ve harfler, rakamlar, alt çizgiler ve kısa çizgiler içerebilir.
5. Runbook 'u oluşturmak için **Oluştur** ' a tıklayın ve düzenleyiciyi açın.

### <a name="create-a-runbook-with-powershell"></a>PowerShell ile runbook oluşturma

Boş bir [PowerShell Iş akışı runbook 'u](automation-runbook-types.md#powershell-workflow-runbooks)oluşturmak için [New-azautomationrunbook](https://docs.microsoft.com/powershell/module/az.automation/new-azautomationrunbook?view=azps-3.5.0) cmdlet 'ini kullanabilirsiniz. **New-AzAutomationRunbook**için tanımlanan runbook türlerinden birini belirtmek için *tür* parametresini kullanın.

Aşağıdaki örnek, yeni bir boş runbook oluşturmayı gösterir.

```azurepowershell-interactive
New-AzAutomationRunbook -AutomationAccountName MyAccount `
-Name NewRunbook -ResourceGroupName MyResourceGroup -Type PowerShell
```

## <a name="import-a-runbook"></a>Runbook 'u içeri aktarma

Bir PowerShell betiği veya PowerShell Iş akışı ( **. ps1**), içeri aktarılan bir grafik runbook ( **. Graphrunbook**) veya Python 2 betiği ( **. Kopyala**) içeri aktararak Azure Otomasyonu 'nda yeni bir runbook oluşturabilirsiniz.  İçeri aktarma sırasında oluşturulan [runbook 'un türünü](automation-runbook-types.md) belirtmeniz gerekir ve aşağıdaki noktaları dikkate alın.

* Bir iş akışı içermeyen bir **. ps1** dosyası, bir [PowerShell runbook](automation-runbook-types.md#powershell-runbooks) 'Una veya [PowerShell iş akışı runbook 'una](automation-runbook-types.md#powershell-workflow-runbooks)aktarılabilir. Bir PowerShell Iş akışı runbook 'una içeri aktarırsanız, bir iş akışına dönüştürülür. Bu durumda, yapılan değişiklikleri anlatmak için Runbook 'a açıklamalar dahildir.

* PowerShell Iş akışı içeren bir **. ps1** dosyası, yalnızca bir [PowerShell iş akışı runbook 'una](automation-runbook-types.md#powershell-workflow-runbooks)aktarılabilir. Dosya birden çok PowerShell iş akışı içeriyorsa içeri aktarma başarısız olur. Her bir iş akışını kendi dosyasına kaydetmeli ve ayrı olarak içeri aktarmanız gerekir.

* PowerShell betik altyapısı bunu tanıyamadığı için PowerShell Iş akışı içeren bir **. ps1** dosyası bir [PowerShell runbook 'una](automation-runbook-types.md#powershell-runbooks)aktarılmamalıdır.

* Bir **. graphrunbook** dosyası, yalnızca yeni bir [grafik runbook 'una](automation-runbook-types.md#graphical-runbooks)aktarılabilir. Bir **. graphrunbook** dosyasından yalnızca bir grafik runbook 'u oluşturabileceğiniz unutulmamalıdır.

### <a name="import-a-runbook-from-a-file-with-the-azure-portal"></a>Azure portal bir dosyadaki runbook 'u içeri aktarma

Bir betik dosyasını Azure Otomasyonu 'na aktarmak için aşağıdaki yordamı kullanabilirsiniz.

> [!NOTE]
> Portalı kullanarak bir PowerShell Iş akışı runbook 'unda yalnızca bir **. ps1** dosyasını içeri aktarabilirsiniz.

1. Azure portalında, Otomasyon hesabınızı açın.
2. Hub 'dan runbook listesini açmak için **Işlem Otomasyonu** altında **runbook 'lar** ' ı seçin.
3. **Runbook 'U Içeri Aktar ' a**tıklayın.
4. **Runbook dosyası** ' na tıklayın ve içeri aktarılacak dosyayı seçin.
5. **Ad** alanı etkinleştirilirse, runbook adını değiştirme seçeneğiniz vardır. Ad bir harfle başlamalıdır ve harfler, rakamlar, alt çizgiler ve kısa çizgiler içerebilir.
6. [Runbook türü](automation-runbook-types.md) otomatik olarak seçilir, ancak ilgili kısıtlamaları hesaba aldıktan sonra türü değiştirebilirsiniz.
7. **Oluştur**'a tıklayın. Yeni runbook, Otomasyon hesabı için Runbook 'lar listesinde görüntülenir.
8. Çalıştırmak için [runbook 'u yayımlamanız](#publish-a-runbook) gerekir.

> [!NOTE]
> Bir grafik runbook 'unu veya bir grafik PowerShell Iş akışı runbook 'unu içeri aktardıktan sonra, bunu başka bir türe dönüştürebilirsiniz. Ancak, bu grafik runbook 'lardan birini bir metinsel runbook 'a dönüştüremezsiniz.

### <a name="import-a-runbook-from-a-script-file-with-windows-powershell"></a>Windows PowerShell ile bir betik dosyasından bir runbook'u İçeri Aktar

Bir betik dosyasını taslak PowerShell Iş akışı runbook 'u olarak içeri aktarmak için [Import-AzAutomationRunbook](https://docs.microsoft.com/powershell/module/az.automation/import-azautomationrunbook?view=azps-3.5.0) cmdlet 'ini kullanın. Runbook zaten mevcutsa, cmdlet 'le *zorla* parametresini kullanmadığınız takdirde içeri aktarma başarısız olur.

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

## <a name="test-a-runbook"></a>Bir runbook'u test etme

Bir runbook 'u test ettiğinizde [taslak sürümü](#publish-a-runbook) yürütülür ve gerçekleştirdiği tüm işlemler tamamlanır. İş geçmişi oluşturulmaz, ancak [Çıkış](automation-runbook-output-and-messages.md#output-stream) ve [uyarı ve hata](automation-runbook-output-and-messages.md#message-streams) akışları test çıkış bölmesinde görüntülenir. [Ayrıntılı akışa](automation-runbook-output-and-messages.md#message-streams) iletiler yalnızca, *VerbosePreference* değişkeni] (Automation-runbook-Output-ve-messages. MD # Preference-Variables) **devam**olarak ayarlandıysa çıkış bölmesinde görüntülenir.

Taslak sürümü çalıştırılsa da runbook hala normal şekilde yürütülür ve ortamdaki kaynaklara karşı herhangi bir eylem gerçekleştirir. Bu nedenle, yalnızca üretim dışı kaynaklarda runbook 'ları test etmelisiniz.

Her [runbook türünü](automation-runbook-types.md) test etme yordamı aynıdır. Azure portal metin Düzenleyicisi ile grafik Düzenleyicisi arasındaki sınamada fark yoktur.

1. Runbook 'un taslak sürümünü [metin düzenleyicisinde](automation-edit-textual-runbook.md) veya [grafik düzenleyicide](automation-graphical-authoring-intro.md)açın.
1. Sınama sayfasını açmak için **Test** düğmesine tıklayın.
1. Runbook 'un parametreleri varsa, bunlar sol bölmede listelenir ve burada test için kullanılacak değerleri sağlayabilirsiniz.
1. Bir [karma runbook çalışanı](automation-hybrid-runbook-worker.md)üzerinde testi çalıştırmak Istiyorsanız, **çalışma ayarlarını** **karma çalışan** olarak değiştirin ve hedef grubun adını seçin.  Aksi takdirde, testi bulutta çalıştırmak için varsayılan **Azure** 'u saklayın.
1. Teste başlamak için **Başlat** düğmesine tıklayın.
1. Test edilirken bir [PowerShell Iş akışını](automation-runbook-types.md#powershell-workflow-runbooks) veya [grafik](automation-runbook-types.md#graphical-runbooks) runbook 'u durdurmak veya askıya almak için çıkış bölmesi altındaki düğmeleri kullanabilirsiniz. Bir runbook'u askıya aldığınızda, askıya alınmadan önce geçerli etkinliği tamamlar. Runbook askıya alındığında, durdurabilir veya yeniden başlatabilirsiniz.
1. Çıkış bölmesinde runbook 'tan çıktıyı inceleyin.

## <a name="publish-a-runbook"></a>Runbook yayımlama

Yeni bir runbook oluşturduğunuzda veya içeri aktardığınızda, çalıştırmadan önce onu yayımlamanız gerekir. Azure Otomasyonu 'ndaki her runbook 'un bir taslak sürümü ve yayımlanmış bir sürümü vardır. Yalnızca Yayımlanan sürüm çalıştırılabilir ve yalnızca Taslak sürüm düzenlenebilir. Yayımlanan sürüm Taslak sürümdeki herhangi bir değişiklikten etkilenmez. Taslak sürümü kullanılabilir hale getirilmeli hale geldiğinde, geçerli yayımlanmış sürümün üzerine bir taslak sürümü yazarak onu yayımlayın.

### <a name="publish-a-runbook-in-the-azure-portal"></a>Azure portal runbook yayımlama

1. Runbook 'u Azure portal açın.
2. **Düzenle**’ye tıklayın.
3. Doğrulama iletisine yanıt olarak **Yayımla** ' ya ve ardından **Evet** ' e tıklayın.

### <a name="publish-a-runbook-using-powershell"></a>PowerShell kullanarak runbook yayımlama

Windows PowerShell ile bir runbook yayımlamak için [Publish-AzAutomationRunbook](https://docs.microsoft.com/powershell/module/Az.Automation/Publish-AzAutomationRunbook?view=azps-3.5.0) cmdlet 'ini kullanın. Aşağıdaki örnekte, örnek bir runbook 'un nasıl yayımlanacağı gösterilmektedir.

```azurepowershell-interactive
$automationAccountName =  "AutomationAccount"
$runbookName = "Sample_TestRunbook"
$RGName = "ResourceGroup"

Publish-AzAutomationRunbook -AutomationAccountName $automationAccountName `
-Name $runbookName -ResourceGroupName $RGName
```

## <a name="next-steps"></a>Sonraki adımlar

* Runbook ve PowerShell modül galerisinden nasıl avantaj sağlayabileceğinizi öğrenmek için bkz. [Azure Otomasyonu Için runbook ve modül galerileri](automation-runbook-gallery.md).
* PowerShell ve PowerShell Iş akışı runbook 'larını metinsel düzenleyiciyle düzenleme hakkında daha fazla bilgi edinmek için bkz. [Azure Automation 'da metin runbook 'Larını düzenleme](automation-edit-textual-runbook.md).
* Grafik runbook 'u yazma hakkında daha fazla bilgi için bkz. [Azure Otomasyonu 'Nda grafik yazma](automation-graphical-authoring-intro.md).
