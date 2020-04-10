---
title: PowerShell ile Azure Akış Analizi işlerini izleyin ve yönetin
description: Bu makalede, Azure Akış Analizi işlerini izlemek ve yönetmek için Azure PowerShell ve cmdlets nasıl kullanılacağı açıklanmaktadır.
author: jseb225
ms.author: jeanb
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/28/2017
ms.openlocfilehash: 295141dfd9b84428e2ee69354ab0c249fa46d1b6
ms.sourcegitcommit: 25490467e43cbc3139a0df60125687e2b1c73c09
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80998879"
---
# <a name="monitor-and-manage-stream-analytics-jobs-with-azure-powershell-cmdlets"></a>Azure PowerShell cmdlets ile Stream Analytics işlerini izleme ve yönetme
Temel Stream Analytics görevlerini yürüten Azure PowerShell cmdlets ve powershell komut dosyası yla Stream Analytics kaynaklarını nasıl izleyeceğinizi ve yönettiğinizi öğrenin.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites-for-running-azure-powershell-cmdlets-for-stream-analytics"></a>Stream Analytics için Azure PowerShell cmdlets çalıştırmak için ön koşullar
* Aboneliğinizde bir Azure Kaynak Grubu oluşturun. Aşağıda örnek bir Azure PowerShell komut dosyası verem. Azure PowerShell bilgileri için Azure [PowerShell'i Yükle ve yapılandırma](/powershell/azure/overview)(  

Azure PowerShell 0.9.8:  

```powershell
# Log in to your Azure account
Add-AzureAccount
# Select the Azure subscription you want to use to create the resource group if you have more han one subscription on your account.
Select-AzureSubscription -SubscriptionName <subscription name>
# If Stream Analytics has not been registered to the subscription, remove remark symbol below (#)to run the Register-AzureProvider cmdlet to register the provider namespace.
#Register-AzureProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'
# Create an Azure resource group
New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>
```

Azure PowerShell 1.0:

```powershell
# Log in to your Azure account
Connect-AzAccount
# Select the Azure subscription you want to use to create the resource group.
Get-AzSubscription -SubscriptionName "your sub" | Select-AzSubscription
# If Stream Analytics has not been registered to the subscription, remove remark symbol below (#)to run the Register-AzureProvider cmdlet to register the provider namespace.
#Register-AzResourceProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'
# Create an Azure resource group
New-AzResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>
```


> [!NOTE]
> Programlı olarak oluşturulan Akış Analizi işleri varsayılan olarak izlemeyi etkinleştirmez.  İşi Takip Eden Sayfa'ya gidip Etkinleştir düğmesini tıklatarak Azure Portalı'nda izlemeyi el ile etkinleştirebilir veya Azure Akış [Analizi - Stream Analytics İşlerini Programlı olarak izleyerek](stream-analytics-monitor-jobs.md)bunu programlı olarak yapabilirsiniz.
> 
> 

## <a name="azure-powershell-cmdlets-for-stream-analytics"></a>Akış Analizi için Azure PowerShell cmdlets
Aşağıdaki Azure PowerShell cmdlets'leri, Azure Akış Analizi işlerini izlemek ve yönetmek için kullanılabilir. Azure PowerShell'in farklı sürümleri olduğunu unutmayın. 
**Listelenen örneklerde ilk komut Azure PowerShell 0.9.8 için, ikinci komut Azure PowerShell 1.0 içindir.** Azure PowerShell 1.0 komutları her zaman komutta "Az" olacaktır.

### <a name="get-azurestreamanalyticsjob--get-azstreamanalyticsjob"></a>AzureStreamAnalyticsJob | Get-AzStreamAnalyticsJob
Azure aboneliğinde veya belirtilen kaynak grubunda tanımlanan tüm Akış Analizi işlerini listeler veya kaynak grubu içindeki belirli bir iş hakkında iş bilgileri alır.

**Örnek 1**

Azure PowerShell 0.9.8:  

```powershell
Get-AzureStreamAnalyticsJob
```

Azure PowerShell 1.0:  

```powershell
Get-AzStreamAnalyticsJob
```

Bu PowerShell komutu, Azure aboneliğindeki tüm Akış Analizi işleri hakkında bilgi verir.

**Örnek 2**

Azure PowerShell 0.9.8:  

```powershell
Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 
```

Azure PowerShell 1.0:  

```powershell
Get-AzStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 
```

Bu PowerShell komutu, StreamAnalytics-Default-Central-US kaynak grubundaki tüm Stream Analytics işleri hakkında bilgi verir.

**Örnek 3**

Azure PowerShell 0.9.8:  

```powershell
Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob
```

Azure PowerShell 1.0:  

```powershell
Get-AzStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob
```

Bu PowerShell komutu, StreamAnalytics-Default-Central-US kaynak grubunda Akış Analizi işi StreamingJob hakkında bilgi verir.

### <a name="get-azurestreamanalyticsinput--get-azstreamanalyticsinput"></a>AzureStreamAnalyticsInput | Get-AzStreamAnalyticsInput
Belirli bir Akış Analizi işinde tanımlanan tüm girişleri listeler veya belirli bir giriş hakkında bilgi alır.

**Örnek 1**

Azure PowerShell 0.9.8:  

```powershell
Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob
```

Azure PowerShell 1.0:  

```powershell
Get-AzStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob
```

Bu PowerShell komutu, Akışİş işinde tanımlanan tüm girişler hakkında bilgi verir.

**Örnek 2**

Azure PowerShell 0.9.8:  

```powershell
Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob -Name EntryStream
```

Azure PowerShell 1.0:  

```powershell
Get-AzStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob -Name EntryStream
```

Bu PowerShell komutu, Akışİş işinde tanımlanan EntryStream adlı giriş le ilgili bilgileri döndürür.

### <a name="get-azurestreamanalyticsoutput--get-azstreamanalyticsoutput"></a>AzureStreamAnalyticsOutput | Get-AzStreamAnalyticsOutput
Belirli bir Akış Analizi işinde tanımlanan tüm çıktıları listeler veya belirli bir çıktı hakkında bilgi alır.

**Örnek 1**

Azure PowerShell 0.9.8:  

```powershell
Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob
```

Azure PowerShell 1.0:  

```powershell
Get-AzStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob
```

Bu PowerShell komutu, Akışİş işinde tanımlanan çıktılar hakkında bilgi verir.

**Örnek 2**

Azure PowerShell 0.9.8:  

```powershell
Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob -Name Output
```

Azure PowerShell 1.0:  

```powershell
Get-AzStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob -Name Output
```

Bu PowerShell komutu, Akışİş işinde tanımlanan Çıktı adlı çıktı hakkında bilgi verir.

### <a name="get-azurestreamanalyticsquota--get-azstreamanalyticsquota"></a>AzureStreamAnalyticsKota | Get-AzStreamAnalyticsKota
Belirli bir bölgedeki akış birimlerinin kotası hakkında bilgi alır.

**Örnek 1**

Azure PowerShell 0.9.8:  

```powershell
Get-AzureStreamAnalyticsQuota -Location "Central US" 
```

Azure PowerShell 1.0:  

```powershell
Get-AzStreamAnalyticsQuota -Location "Central US" 
```

Bu PowerShell komutu, Orta ABD bölgesindeki akış birimlerinin kotası ve kullanımı hakkında bilgi verir.

### <a name="get-azurestreamanalyticstransformation--get-azstreamanalyticstransformation"></a>AzureStreamAnalyticsDönüşümü | Get-AzStreamAnalyticsTransformation
Bir Akış Analizi işinde tanımlanan belirli bir dönüşüm hakkında bilgi alır.

**Örnek 1**

Azure PowerShell 0.9.8:  

```powershell
Get-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob -Name StreamingJob
```

Azure PowerShell 1.0:  

```powershell
Get-AzStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob -Name StreamingJob
```

Bu PowerShell komutu, Akışİş'te Akışİş adı verilen dönüşüm hakkında bilgi verir.

### <a name="new-azurestreamanalyticsinput--new-azstreamanalyticsinput"></a>Yeni AzureStreamAnalyticsInput | Yeni-AzStreamAnalyticsInput
Bir Akış Analizi işi içinde yeni bir girdi oluşturur veya varolan bir belirtilen girişi güncelleştirir.

Girişin adı .json dosyasında veya komut satırında belirtilebilir. Her ikisi de belirtilirse, komut satırındaki ad dosyadakiyle aynı olmalıdır.

Zaten var olan bir giriş belirtir seniz ve -Kuvvet parametresini belirtmezseniz, cmdlet varolan girdinin değiştirilip değiştirilmeyeceğini sorar.

-Force parametresini belirtip varolan bir giriş adı belirtirseniz, giriş onay olmadan değiştirilir.

JSON dosya yapısı ve içeriği hakkında ayrıntılı bilgi [için, Akış Analizi Yönetimi REST API Başvuru Kitaplığı'nın][stream.analytics.rest.api.reference] [Giriş Oluştur (Azure Akışı Analizi)][msdn-rest-api-create-stream-analytics-input] bölümüne bakın.

**Örnek 1**

Azure PowerShell 0.9.8:  

```powershell
New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob -File "C:\Input.json" 
```

Azure PowerShell 1.0:  

```powershell
New-AzStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob -File "C:\Input.json" 
```

Bu PowerShell komutu, Input.json dosyasından yeni bir giriş oluşturur. Giriş tanımı dosyasında belirtilen ada sahip varolan bir giriş zaten tanımlanmışsa, cmdlet bunu değiştirip değiştirmeme mi soracaktır.

**Örnek 2**

Azure PowerShell 0.9.8:  

```powershell
New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob -File "C:\Input.json" -Name EntryStream
```

Azure PowerShell 1.0:  

```powershell
New-AzStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob -File "C:\Input.json" -Name EntryStream
```

Bu PowerShell komutu EntryStream adlı işte yeni bir giriş oluşturur. Bu ada sahip varolan bir giriş zaten tanımlanmışsa, cmdlet değiştirip değiştirmememi sorar.

**Örnek 3**

Azure PowerShell 0.9.8:  

```powershell
New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob -File "C:\Input.json" -Name EntryStream -Force
```

Azure PowerShell 1.0:  

```powershell
New-AzStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob -File "C:\Input.json" -Name EntryStream -Force
```

Bu PowerShell komutu, entrystream adı verilen varolan giriş kaynağının tanımının yerine dosyadaki tanımla birlikte gelir.

### <a name="new-azurestreamanalyticsjob--new-azstreamanalyticsjob"></a>Yeni AzureStreamAnalyticsJob | Yeni-AzStreamAnalyticsJob
Microsoft Azure'da yeni bir Akış Analizi işi oluşturur veya varolan belirtilen işin tanımını güncelleştirir.

İşin adı .json dosyasında veya komut satırında belirtilebilir. Her ikisi de belirtilirse, komut satırındaki ad dosyadakiyle aynı olmalıdır.

Zaten var olan bir iş adı belirtir ve -Kuvvet parametresini belirtmezseniz, cmdlet varolan işin değiştirilip değiştirilmeyeceğini sorar.

-Kuvvet parametresini belirtir ve varolan bir iş adı belirtirseniz, iş tanımı onay olmadan değiştirilir.

JSON dosya yapısı ve içeriği hakkında ayrıntılı bilgi için, [Stream Analytics Management REST API Başvuru Kitaplığı'nın][stream.analytics.rest.api.reference] [Akış Analizi İşi Oluştur][msdn-rest-api-create-stream-analytics-job] bölümüne bakın.

**Örnek 1**

Azure PowerShell 0.9.8:  

```powershell
New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -File "C:\JobDefinition.json" 
```

Azure PowerShell 1.0:  

```powershell
New-AzStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -File "C:\JobDefinition.json" 
```

Bu PowerShell komutu JobDefinition.json'daki tanımdan yeni bir iş oluşturur. İş tanımı dosyasında belirtilen ada sahip varolan bir iş zaten tanımlanmışsa, cmdlet değiştirip değiştirmeme mi soracaktır.

**Örnek 2**

Azure PowerShell 0.9.8:  

```powershell
New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -File "C:\JobDefinition.json" -Name StreamingJob -Force
```

Azure PowerShell 1.0:  

```powershell
New-AzStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -File "C:\JobDefinition.json" -Name StreamingJob -Force
```

Bu PowerShell komutu, StreamingJob için iş tanımının yerini alır.

### <a name="new-azurestreamanalyticsoutput--new-azstreamanalyticsoutput"></a>Yeni AzureStreamAnalyticsOutput | Yeni-AzStreamAnalyticsOutput
Bir Akış Analizi işi içinde yeni bir çıktı oluşturur veya varolan bir çıktıyı güncelleştirir.  

Çıktının adı .json dosyasında veya komut satırında belirtilebilir. Her ikisi de belirtilirse, komut satırındaki ad dosyadakiyle aynı olmalıdır.

Zaten var olan bir çıktı belirtir seniz ve -Kuvvet parametresini belirtmezseniz, cmdlet varolan çıktıyı değiştirip değiştirmemenizi sorar.

-Kuvvet parametresini belirtir ve varolan bir çıktı adı belirtirseniz, çıktı onay olmadan değiştirilir.

JSON dosya yapısı ve içeriği hakkında ayrıntılı bilgi [için, Akış Analizi Yönetimi REST API Başvuru Kitaplığı'nın][stream.analytics.rest.api.reference] [Çıktı Oluştur (Azure Akışı Analizi)][msdn-rest-api-create-stream-analytics-output] bölümüne bakın.

**Örnek 1**

Azure PowerShell 0.9.8:  

```powershell
New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -File "C:\Output.json" -JobName StreamingJob -Name output
```

Azure PowerShell 1.0:  

```powershell
New-AzStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -File "C:\Output.json" -JobName StreamingJob -Name output
```

Bu PowerShell komutu, Akışİş işinde "çıktı" adı verilen yeni bir çıktı oluşturur. Bu ada sahip varolan bir çıktı zaten tanımlanmışsa, cmdlet değiştirip değiştirmememi sorar.

**Örnek 2**

Azure PowerShell 0.9.8:  

```powershell
New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -File "C:\Output.json" -JobName StreamingJob -Name output -Force
```

Azure PowerShell 1.0:  

```powershell
New-AzStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -File "C:\Output.json" -JobName StreamingJob -Name output -Force
```

Bu PowerShell komutu, Akışİş'teki "çıktı" tanımının yerini alır.

### <a name="new-azurestreamanalyticstransformation--new-azstreamanalyticstransformation"></a>Yeni AzureStreamAnalyticsTransformation | Yeni-AzStreamAnalyticsDönüşüm
Bir Akış Analizi işinde yeni bir dönüşüm oluşturur veya varolan dönüşümü güncelleştirir.

Dönüştürmenin adı .json dosyasında veya komut satırında belirtilebilir. Her ikisi de belirtilirse, komut satırındaki ad dosyadakiyle aynı olmalıdır.

Zaten var olan bir dönüştürme belirtir seniz ve -Kuvvet parametresini belirtmezseniz, cmdlet varolan dönüşümün yerine gelip geçmemesini sorar.

-Kuvvet parametresini belirtir ve varolan bir dönüşüm adı belirtirseniz, dönüşüm onay olmadan değiştirilir.

JSON dosya yapısı ve içeriği hakkında ayrıntılı bilgi [için, Akış Analizi Yönetimi REST API Başvuru Kitaplığı'nın][stream.analytics.rest.api.reference] [Dönüşüm Oluştur (Azure Akışı Analizi)][msdn-rest-api-create-stream-analytics-transformation] bölümüne bakın.

**Örnek 1**

Azure PowerShell 0.9.8:  

```powershell
New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -File "C:\Transformation.json" -JobName StreamingJob -Name StreamingJobTransform
```

Azure PowerShell 1.0:  

```powershell
New-AzStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -File "C:\Transformation.json" -JobName StreamingJob -Name StreamingJobTransform
```

Bu PowerShell komutu, StreamingJob işinde StreamingJobTransform adında yeni bir dönüşüm oluşturur. Varolan bir dönüşüm zaten bu adla tanımlanmışsa, cmdlet bu ada değiştirip değiştirmeme mi soracaktır.

**Örnek 2**

Azure PowerShell 0.9.8:  

```powershell
New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -File "C:\Transformation.json" -JobName StreamingJob -Name StreamingJobTransform -Force
```

Azure PowerShell 1.0:  

```powershell
New-AzStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -File "C:\Transformation.json" -JobName StreamingJob -Name StreamingJobTransform -Force
```

 Bu PowerShell komutu, StreamingJob iş akışında StreamingJobTransform tanımının yerini alır.

### <a name="remove-azurestreamanalyticsinput--remove-azstreamanalyticsinput"></a>Remove-AzureStreamAnalyticsInput | Remove-AzStreamAnalyticsInput
Microsoft Azure'daki bir Akış Analizi işinden belirli bir girişi eş senkronize olarak siler.  
-Kuvvet parametresini belirtirseniz, giriş onay olmadan silinir.

**Örnek 1**

Azure PowerShell 0.9.8:  

```powershell
Remove-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob -Name EventStream
```

Azure PowerShell 1.0:  

```powershell
Remove-AzStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob -Name EventStream
```

Bu PowerShell komutu, Akışİş iş giriş EventStream kaldırır.  

### <a name="remove-azurestreamanalyticsjob--remove-azstreamanalyticsjob"></a>Kaldır-AzureStreamAnalyticsJob | Kaldır-AzStreamAnalyticsJob
Microsoft Azure'daki belirli bir Akış Analizi işini eş senkronize olarak siler.  
-Kuvvet parametresini belirtirseniz, iş onay olmadan silinir.

**Örnek 1**

Azure PowerShell 0.9.8:  

```powershell
Remove-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob 
```

Azure PowerShell 1.0:  

```powershell
Remove-AzStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob 
```

Bu PowerShell komutu Akışİş işini kaldırır.  

### <a name="remove-azurestreamanalyticsoutput--remove-azstreamanalyticsoutput"></a>Remove-AzureStreamAnalyticsOutput | Remove-AzStreamAnalyticsOutput
Microsoft Azure'daki bir Akış Analizi işinden belirli bir çıktıyı eşzamanlı olarak siler.  
-Kuvvet parametresini belirtirseniz, çıktı onay olmadan silinir.

**Örnek 1**

Azure PowerShell 0.9.8:  

```powershell
Remove-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob -Name Output
```

Azure PowerShell 1.0:  

```powershell
Remove-AzStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob -Name Output
```

Bu PowerShell komutu, Akışİş iş çıktısını kaldırır.  

### <a name="start-azurestreamanalyticsjob--start-azstreamanalyticsjob"></a>Başlangıç-AzureStreamAnalyticsJob | Başlangıç-AzStreamAnalyticsJob
Microsoft Azure'da bir Akış Analizi işini eş senkronize olarak dağıtır ve başlatır.

**Örnek 1**

Azure PowerShell 0.9.8:  

```powershell
Start-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z
```

Azure PowerShell 1.0:  

```powershell
Start-AzStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z
```

Bu PowerShell komutu, 12 Aralık 2012, 12:12:12 UTC olarak ayarlanmış özel bir çıktı başlangıç saati ile iş StreamingJob'u başlatır.

### <a name="stop-azurestreamanalyticsjob--stop-azstreamanalyticsjob"></a>Stop-AzureStreamAnalyticsJob | Stop-AzStreamAnalyticsJob
Asynchronously Microsoft Azure'da çalışan bir Akış Analizi iş durdurur ve kullanılan kaynakları ayırır. İş tanımı ve meta veriler, hem Azure portalı hem de yönetim API'leri aracılığıyla aboneliğinizde kullanılabilir kalır, ancak iş düzenlenebilir ve yeniden başlatılabilir. Durdurulan eyaletteki bir iş için ücret lendirilmezsiniz.

**Örnek 1**

Azure PowerShell 0.9.8:  

```powershell
Stop-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob 
```

Azure PowerShell 1.0:  

```powershell
Stop-AzStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob 
```

Bu PowerShell komutu Akışİş işini durdurur.  

### <a name="test-azurestreamanalyticsinput--test-azstreamanalyticsinput"></a>Test-AzureStreamAnalyticsInput | Test-AzStreamAnalyticsInput
Stream Analytics'in belirli bir girişe bağlanma yeteneğini sınar.

**Örnek 1**

Azure PowerShell 0.9.8:  

```powershell
Test-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob -Name EntryStream
```

Azure PowerShell 1.0:  

```powershell
Test-AzStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob -Name EntryStream
```

Bu PowerShell komutu, StreamingJob'daki Giriş Girişi Akışının bağlantı durumunu sınar.  

### <a name="test-azurestreamanalyticsoutput--test-azstreamanalyticsoutput"></a>Test-AzureStreamAnalyticsOutput | Test-AzStreamAnalyticsOutput
Stream Analytics'in belirli bir çıktıya bağlanma yeteneğini sınar.

**Örnek 1**

Azure PowerShell 0.9.8:  

```powershell
Test-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob -Name Output
```

Azure PowerShell 1.0:  

```powershell
Test-AzStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob -Name Output
```

Bu PowerShell komutu, StreamingJob'daki çıktı çıktısının bağlantı durumunu sınar.  

## <a name="get-support"></a>Destek alın
Daha fazla yardım için [Azure Akışı Analizi forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)deneyin. 

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Akış Analizine Giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

[msdn-switch-azuremode]: https://msdn.microsoft.com/library/dn722470.aspx
[powershell-install]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[msdn-rest-api-create-stream-analytics-job]: https://msdn.microsoft.com/library/dn834994.aspx
[msdn-rest-api-create-stream-analytics-input]: https://msdn.microsoft.com/library/dn835010.aspx
[msdn-rest-api-create-stream-analytics-output]: https://msdn.microsoft.com/library/dn835015.aspx
[msdn-rest-api-create-stream-analytics-transformation]: https://msdn.microsoft.com/library/dn835007.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: https://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: https://go.microsoft.com/fwlink/?LinkId=517301

