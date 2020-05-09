---
title: Azure Automation 'da metin runbook 'larını Düzenle
description: Bu makalede, metinsel düzenleyiciyi kullanarak Azure Otomasyonu 'nda PowerShell ve PowerShell Iş akışı runbook 'ları ile çalışmaya yönelik farklı yordamlar sunulmaktadır.
services: automation
ms.service: automation
ms.subservice: process-automation
author: mgoedtel
ms.author: magoedte
ms.date: 08/01/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: aef6a58d18c0d9c1b18915786dd6e3359c31eda0
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82855783"
---
# <a name="editing-textual-runbooks-in-azure-automation"></a>Azure Automation 'da metin runbook 'larını Düzenle

Azure Otomasyonu 'ndaki metinsel düzenleyici, [PowerShell runbook](automation-runbook-types.md#powershell-runbooks) 'Larını ve [PowerShell iş akışı runbook 'larını](automation-runbook-types.md#powershell-workflow-runbooks)düzenlemek için kullanılabilir. Bu düzenleyici, IntelliSense gibi diğer kod düzenleyicilerinin tipik özelliklerine sahiptir. Runbook 'larda ortak kaynaklara erişirken size yardımcı olacak ek özel özelliklerle birlikte renk kodlaması de vardır. 

Metinsel düzenleyici, bir runbook 'a cmdlet 'ler, varlıklar ve alt runbook 'lar için kod ekleme özelliğini içerir. Kodu kendiniz yazmak yerine, kullanılabilir kaynaklar listesinden seçim yapabilirsiniz ve düzenleyici ilgili kodu runbook 'a ekler.

Azure Otomasyonu 'ndaki her runbook 'un taslak ve yayımlanan iki sürümü vardır. Runbook 'un taslak sürümünü düzenleyin ve ardından yürütülebilmesi için yayımlayın. Yayımlanan sürüm düzenlenemez. Daha fazla bilgi için bkz. [runbook yayımlama](manage-runbooks.md#publish-a-runbook).

Bu makalede, bu düzenleyiciyle farklı işlevler gerçekleştirmeye yönelik ayrıntılı adımlar sağlanmaktadır. Bunlar [grafik runbook 'lar](automation-runbook-types.md#graphical-runbooks)için geçerli değildir. Bu runbook 'larla çalışmak için bkz. [Azure Otomasyonu 'Nda grafik yazma](automation-graphical-authoring-intro.md).

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma runbook çalışanınız hakkında az Module yükleme yönergeleri için bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Otomasyon hesabınız için, [Azure Otomasyonu 'nda Azure PowerShell modüllerini güncelleştirme](automation-update-azure-modules.md)' yi kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="editing-a-runbook-with-the-azure-portal"></a>Azure portal ile runbook 'u düzenlemeyle

Metin düzenleyicisinde düzenleme için bir runbook 'u açmak üzere aşağıdaki yordamı kullanın.

1. Azure portal Otomasyon hesabınızı seçin.
2. **Işlem Otomasyonu**altında runbook 'ların listesini açmak Için **runbook 'lar** ' ı seçin.
3. Düzenlenecek runbook 'u seçin ve ardından **Düzenle**' ye tıklayın.
4. Runbook 'u düzenleyin.
5. Düzenlemeleriniz tamamlandığında **Kaydet** 'e tıklayın.
6. Runbook 'un en son taslak sürümünü yayımlamak istiyorsanız **Yayımla** ' ya tıklayın.

### <a name="insert-a-cmdlet-into-a-runbook"></a>Runbook 'a bir cmdlet ekleme

1. Metinsel düzenleyicinin tuvalinde, cmdlet 'ini yerleştirmek istediğiniz yere imleci konumlandırın.
2. Kitaplık denetimindeki **cmdlet 'ler** düğümünü genişletin.
3. Kullanılacak cmdlet 'i içeren modülü genişletin.
4. Eklemek için cmdlet adına sağ tıklayın ve **tuvale Ekle**' yi seçin. Cmdlet 'in birden fazla parametre kümesi varsa, düzenleyici varsayılan kümesini ekler. Ayrıca cmdlet 'i genişleterek farklı bir parametre kümesi seçebilirsiniz.
5. Cmdlet kodunun tüm parametre listesiyle birlikte eklendiğini unutmayın.
6. Gerekli parametreler için açılı ayraç (<>) içine alınmış değerin yerine uygun bir değer sağlayın. İhtiyacınız olmayan parametreleri kaldırın.

### <a name="insert-code-for-a-child-runbook-into-a-runbook"></a>Runbook 'a bir alt runbook için kod ekleme

1. Metinsel düzenleyicinin tuvalinde, [alt runbook](automation-child-runbooks.md)için kodu yerleştirmek istediğiniz yere imleci konumlandırın.
2. Kitaplık denetimindeki **runbook 'lar** düğümünü genişletin.
3. Eklemek için Runbook 'a sağ tıklayın ve **tuvale Ekle**' yi seçin.
4. Alt runbook kodu herhangi bir runbook parametresi için herhangi bir yer tutucu ile birlikte eklenir.
5. Yer tutucuları her parametre için uygun değerlerle değiştirin.

### <a name="insert-an-asset-into-a-runbook"></a>Runbook 'a varlık ekleme

1. Metinsel düzenleyicinin tuval denetiminde, alt runbook için kodu yerleştirmek istediğiniz yere imleci konumlandırın.
2. Kitaplık denetimindeki **varlıklar** düğümünü genişletin.
3. İstenen varlık türü için düğümü genişletin.
4. Eklemek için varlık adına sağ tıklayın ve **tuvale Ekle**' yi seçin. Değişken [varlıklar](automation-variables.md)için, değişkeni almak veya ayarlamak istediğinize bağlı olarak " **değişkeni al" seçeneğini tuvale ekleyin** veya **"set variable" öğesini tuvale ekleyin**.
5. Varlık kodunun runbook 'a eklendiğini unutmayın.

## <a name="editing-an-azure-automation-runbook-using-windows-powershell"></a>Windows PowerShell kullanarak bir Azure Otomasyonu runbook 'unu düzenlemeyle

Windows PowerShell ile bir runbook 'u düzenlemek için, seçtiğiniz düzenleyiciyi kullanın ve Runbook 'u bir **. ps1** dosyasına kaydedin. Runbook 'un içeriğini almak için [Export-AzAutomationRunbook](/powershell/module/Az.Automation/Export-AzAutomationRunbook) cmdlet 'ini kullanabilirsiniz. Var olan taslak runbook 'unu değiştirilmiş bir ile değiştirmek için [Import-AzAutomationRunbook](/powershell/module/Az.Automation/import-azautomationrunbook) cmdlet 'ini kullanabilirsiniz.

### <a name="retrieve-the-contents-of-a-runbook-using-windows-powershell"></a>Windows PowerShell kullanarak bir runbook 'un içeriğini alma

Aşağıdaki örnek komutlarda bir runbook için betiğin nasıl alınacağı ve bir betik dosyasına kaydedileceği gösterilmektedir. Bu örnekte Taslak sürümü alınmaktadır. Runbook 'un yayımlanan sürümünü almak mümkün olsa da bu sürüm değiştirilemez.

```powershell-interactive
$resourceGroupName = "MyResourceGroup"
$automationAccountName = "MyAutomatonAccount"
$runbookName = "Hello-World"
$scriptFolder = "c:\runbooks"

Export-AzAutomationRunbook -Name $runbookName -AutomationAccountName $automationAccountName -ResourceGroupName $resourceGroupName -OutputFolder $scriptFolder -Slot Draft
```

### <a name="change-the-contents-of-a-runbook-using-windows-powershell"></a>Windows PowerShell kullanarak bir runbook 'un içeriğini değiştirme

Aşağıdaki örnek komutlarda bir runbook 'un mevcut içeriğinin bir betik dosyasının içeriğiyle nasıl değiştirileceği gösterilmektedir. 

```powershell-interactive
$resourceGroupName = "MyResourceGroup"
$automationAccountName = "MyAutomatonAccount"
$runbookName = "Hello-World"
$scriptFolder = "c:\runbooks"

Import-AzAutomationRunbook -Path "$scriptfolder\Hello-World.ps1" -Name $runbookName -Type PowerShell -AutomationAccountName $automationAccountName -ResourceGroupName $resourceGroupName -Force
Publish-AzAutomationRunbook -Name $runbookName -AutomationAccountName $automationAccountName -ResourceGroupName $resourceGroupName
```

## <a name="related-articles"></a>İlgili makaleler:

* [Azure Otomasyonu 'nda runbook 'ları yönetme](manage-runbooks.md)
* [PowerShell iş akışını öğrenme](automation-powershell-workflow.md)
* [Azure Otomasyonu 'nda grafik yazma](automation-graphical-authoring-intro.md)
* [Sertifikalar](automation-certificates.md)
* [Bağlantılar](automation-connections.md)
* [Kimlik Bilgileri](automation-credentials.md)
* [Zamanlamalar](automation-schedules.md)
* [Değişkenler](automation-variables.md)
* [PowerShell cmdlet başvurusu](https://docs.microsoft.com/powershell/module/az.automation/?view=azps-3.7.0#automation)
