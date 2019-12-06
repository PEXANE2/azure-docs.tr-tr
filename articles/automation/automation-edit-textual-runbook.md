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
ms.openlocfilehash: 5a7eec0a7650f9c8e04a8d1062d32b6feb7d1d99
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74850865"
---
# <a name="editing-textual-runbooks-in-azure-automation"></a>Azure Automation 'da metin runbook 'larını Düzenle

Azure Otomasyonu 'ndaki metinsel düzenleyici, [PowerShell runbook](automation-runbook-types.md#powershell-runbooks) 'Larını ve [PowerShell iş akışı runbook 'larını](automation-runbook-types.md#powershell-workflow-runbooks)düzenlemek için kullanılabilir. Bu, IntelliSense gibi diğer kod düzenleyicilerinin tipik özelliklerine sahiptir ve Runbook 'larda ortak kaynaklara erişirken size yardımcı olacak ek özel özelliklerle renk kodlaması sağlar. Bu makalede, bu düzenleyiciyle farklı işlevler gerçekleştirmeye yönelik ayrıntılı adımlar sağlanmaktadır.

Metinsel düzenleyici, bir runbook 'a cmdlet 'ler, varlıklar ve alt runbook 'lar için kod ekleme özelliğini içerir. Kodu kendiniz yazmak yerine, kullanılabilir kaynaklar listesinden seçim yapabilir ve uygun kodun runbook 'a eklenmesini sağlayabilirsiniz.

Azure Otomasyonu 'ndaki her runbook 'un taslak ve yayımlanan iki sürümü vardır. Runbook 'un taslak sürümünü düzenleyin ve ardından yürütülebilmesi için yayımlayın. Yayımlanan sürüm düzenlenemez. Daha fazla bilgi için bkz. [runbook yayımlama](manage-runbooks.md#publish-a-runbook).

[Grafik runbook](automation-runbook-types.md#graphical-runbooks)'larla çalışmak için bkz. [Azure Otomasyonu 'nda grafik yazma](automation-graphical-authoring-intro.md).

## <a name="to-edit-a-runbook-with-the-azure-portal"></a>Azure portal bir runbook 'u düzenlemek için

Metin düzenleyicisinde düzenleme için bir runbook 'u açmak üzere aşağıdaki yordamı kullanın.

1. Azure portal Otomasyon hesabınızı seçin.
2. **Işlem Otomasyonu**altında runbook 'ların listesini açmak Için **runbook 'lar** ' ı seçin.
3. Düzenlemek istediğiniz runbook 'u seçin ve ardından **Düzenle** düğmesine tıklayın.
4. Runbook 'u düzenleyin.
5. Düzenlemeleriniz tamamlandığında **Kaydet** 'e tıklayın.
6. Runbook'un en son taslak sürümünün yayımlanmasını istiyorsanız **Yayımla** 'ya tıklayın.

### <a name="to-insert-a-cmdlet-into-a-runbook"></a>Bir runbook 'a bir cmdlet eklemek için

1. Metinsel düzenleyicinin tuvalinde, cmdlet 'ini yerleştirmek istediğiniz yere imleci konumlandırın.
2. Kitaplık denetimindeki **cmdlet 'ler** düğümünü genişletin.
3. Kullanmak istediğiniz cmdlet 'i içeren modülü genişletin.
4. Eklemek için cmdlet 'e sağ tıklayın ve **tuvale Ekle**' yi seçin. Cmdlet 'in birden fazla parametre kümesi varsa, varsayılan küme eklenir. Ayrıca cmdlet 'i genişleterek farklı bir parametre kümesi seçebilirsiniz.
5. Cmdlet kodu, tüm parametre listesiyle birlikte eklenir.
6. Tüm gerekli parametreler için > ayraçları < çevrelenmiş veri türü yerine uygun bir değer sağlayın. İhtiyacınız olmayan parametreleri kaldırın.

### <a name="to-insert-code-for-a-child-runbook-into-a-runbook"></a>Bir runbook 'a bir alt runbook kodu eklemek için

1. Metinsel düzenleyicinin tuvalinde, [alt runbook](automation-child-runbooks.md)için kodu yerleştirmek istediğiniz yere imleci konumlandırın.
2. Kitaplık denetimindeki **runbook 'lar** düğümünü genişletin.
3. Eklemek için Runbook 'a sağ tıklayın ve **tuvale Ekle**' yi seçin.
4. Alt runbook kodu herhangi bir runbook parametresi için herhangi bir yer tutucu ile birlikte eklenir.
5. Yer tutucuları her parametre için uygun değerlerle değiştirin.

### <a name="to-insert-an-asset-into-a-runbook"></a>Bir runbook 'a varlık eklemek için

1. Metinsel düzenleyicinin tuvalinde, alt runbook için kodu yerleştirmek istediğiniz yere imleci konumlandırın.
2. Kitaplık denetimindeki **varlıklar** düğümünü genişletin.
3. İstediğiniz varlık türünün düğümünü genişletin.
4. Eklemek için varlığa sağ tıklayın ve **tuvale Ekle**' yi seçin. [Değişken varlıklar](automation-variables.md)Için **"değişkeni al" öğesini tuvale Ekle** ' yi seçin ya da değişkeni almak veya ayarlamak Istediğinize bağlı olarak **"set variable" öğesini tuvale ekleyin** .
5. Varlığın kodu runbook 'a eklenir.

## <a name="to-edit-an-azure-automation-runbook-using-windows-powershell"></a>Windows PowerShell kullanarak bir Azure Otomasyonu runbook 'unu düzenlemek için

Windows PowerShell ile bir runbook 'u düzenlemek için, seçtiğiniz düzenleyiciyi kullanın ve bir `.ps1` dosyasına kaydedin. Runbook 'un içeriğini almak için [Export-AzureRmAutomationRunbook](/powershell/module/AzureRM.Automation/Export-AzureRmAutomationRunbook) cmdlet 'ini kullanabilir ve sonra var olan taslak runbook 'unu değiştirilmiş bir şekilde değiştirmek Için [-azurermautomationrunbook](/powershell/module/AzureRM.Automation/import-azurermautomationrunbook) cmdlet 'ini kullanabilirsiniz.

### <a name="to-retrieve-the-contents-of-a-runbook-using-windows-powershell"></a>Windows PowerShell Kullanarak bir Runbook'un İçeriğini Alma

Aşağıdaki örnek komutlarda bir runbook için betiğin nasıl alınacağı ve bir betik dosyasına kaydedileceği gösterilmektedir. Bu örnekte Taslak sürümü alınmaktadır. Runbook 'un yayımlanan sürümünü almak mümkün olsa da bu sürüm değiştirilemez.

```powershell-interactive
$resourceGroupName = "MyResourceGroup"
$automationAccountName = "MyAutomatonAccount"
$runbookName = "Hello-World"
$scriptFolder = "c:\runbooks"

Export-AzureRmAutomationRunbook -Name $runbookName -AutomationAccountName $automationAccountName -ResourceGroupName $resourceGroupName -OutputFolder $scriptFolder -Slot Draft
```

### <a name="to-change-the-contents-of-a-runbook-using-windows-powershell"></a>Windows PowerShell Kullanarak bir Runbook'un İçeriğini Değiştirme

Aşağıdaki örnek komutlar bir runbook'un var olan içeriğinin bir betik dosyasının içeriğiyle nasıl değiştirileceğini göstermektedir. Bu, [Windows PowerShell ile bir betik dosyasından bir runbook 'u içeri aktarmak için ile](manage-runbooks.md#import-a-runbook)aynı örnek yordamdır.

```powershell-interactive
$resourceGroupName = "MyResourceGroup"
$automationAccountName = "MyAutomatonAccount"
$runbookName = "Hello-World"
$scriptFolder = "c:\runbooks"

Import-AzureRmAutomationRunbook -Path "$scriptfolder\Hello-World.ps1" -Name $runbookName -Type PowerShell -AutomationAccountName $automationAccountName -ResourceGroupName $resourceGroupName -Force
Publish-AzureRmAutomationRunbook -Name $runbookName -AutomationAccountName $automationAccountName -ResourceGroupName $resourceGroupName
```

## <a name="related-articles"></a>İlgili makaleler

* [Azure Otomasyonu 'nda runbook 'ları yönetme](manage-runbooks.md)
* [PowerShell iş akışını öğrenme](automation-powershell-workflow.md)
* [Azure Otomasyonu 'nda grafik yazma](automation-graphical-authoring-intro.md)
* [Sertifikalar](automation-certificates.md)
* [Bağlantılar](automation-connections.md)
* [Kimlik Bilgileri](automation-credentials.md)
* [Zamanlamalar](automation-schedules.md)
* [Değişkenler](automation-variables.md)

