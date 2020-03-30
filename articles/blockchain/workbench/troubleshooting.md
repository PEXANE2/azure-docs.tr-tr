---
title: Azure Blockchain Workbench sorun giderme
description: Azure Blockchain Workbench Preview uygulamasının sorun giderme nasıl giderilir?
ms.date: 10/14/2019
ms.topic: article
ms.reviewer: brendal
ms.openlocfilehash: ef4bce4dfba77aafa9b86c6877c153534b54636e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74324293"
---
# <a name="azure-blockchain-workbench-preview-troubleshooting"></a>Azure Blockchain Çalışma Tezgahı Önizleme sorun giderme

Bir PowerShell komut dosyası geliştirici hata ayıklama veya destek ile yardımcı olmak için kullanılabilir. Komut dosyası bir özet oluşturur ve sorun giderme için ayrıntılı günlükleri toplar. Toplanan günlükler şunlardır:

* Ethereum gibi Blockchain ağı
* Blockchain Çalışma Tezgahı mikrohizmetleri
* Application Insights
* Azure İzleme (Azure Monitor günlükleri)

Bilgileri sonraki adımları belirlemek ve sorunların temel nedenini belirlemek için kullanabilirsiniz.

[!INCLUDE [Preview note](./includes/preview.md)]

## <a name="troubleshooting-script"></a>Sorun giderme komut dosyası

PowerShell sorun giderme komut dosyası GitHub'da kullanılabilir. GitHub’dan [zip dosyasını indirin](https://github.com/Azure-Samples/blockchain/archive/master.zip) veya örneği kopyalayın.

```
git clone https://github.com/Azure-Samples/blockchain.git
```

## <a name="run-the-script"></a>Betiği çalıştırın
[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

Günlükleri `collectBlockchainWorkbenchTroubleshooting.ps1` toplamak ve sorun giderme bilgileri klasörünü içeren bir ZIP dosyası oluşturmak için komut dosyasını çalıştırın. Örnek:

``` powershell
collectBlockchainWorkbenchTroubleshooting.ps1 -SubscriptionID "<subscription_id>" -ResourceGroupName "workbench-resource-group-name"
```
Komut dosyası aşağıdaki parametreleri kabul eder:

| Parametre  | Açıklama | Gerekli |
|---------|---------|----|
| Abonelik Kimliği | Tüm kaynakları oluşturmak veya bulmak için SubscriptionID. | Evet |
| ResourceGroupName | Blockchain Workbench'in dağıtıldığı Azure Kaynak Grubu'nun adı. | Evet |
| Outputdirectory | Çıkış oluşturmak için yol . ZIP dosyası. Belirtilmemişse, varsayılan olarak geçerli dizini alır. | Hayır |
| LookbackHours | Telemetri çekerken kullanılacak saat sayısı. Varsayılan değer 24 saattir. Maksimum değer 90 saattir | Hayır |
| OmsSubscriptionId | Azure Monitor günlüğünün dağıtıldığı abonelik kimliği. Bu parametreyi yalnızca Blockchain Ağı için Azure Monitor günlükleri Blockchain Workbench'in kaynak grubunun dışına dağıtılırsa geçirin.| Hayır |
| OmsResourceGroup |Azure Monitor günlüğünün dağıtıldığı kaynak grubu. Bu parametreyi yalnızca Blockchain Ağı için Azure Monitor günlükleri Blockchain Workbench'in kaynak grubunun dışına dağıtılırsa geçirin.| Hayır |
| OmsWorkspaceName | Log Analytics çalışma alanı adı. Bu parametreyi yalnızca Blockchain Workbench'in kaynak grubunun dışında blockchain ağı için Azure Monitor günlükleri dağıtılırsa geçirin | Hayır |

## <a name="what-is-collected"></a>Ne toplanır?

Çıktı ZIP dosyası aşağıdaki klasör yapısını içerir:

| Klasör veya Dosya | Açıklama  |
|---------|---------|
| \Özet.txt | Sistemin özeti |
| \Ölçüler\blockchain | Blockchain ile ilgili ölçümler |
| \Ölçüler\Çalışma Tezgahı | Çalışma tezgahı ile ilgili ölçümler |
| \Ayrıntılar\Blockchain | Blockchain hakkında detaylı günlükler |
| \Ayrıntılar\Çalışma Tezgahı | Çalışma tezgahı hakkında ayrıntılı günlükler |

Özet dosyası, uygulamanın genel durumunun ve uygulamanın durumunun anlık görüntüsünü verir. Özet önerilen eylemler sağlar, en iyi hataları vurgular ve hizmetleri çalıştırma yla ilgili meta veriler sağlar.

**Ölçümler** klasörü zaman içinde çeşitli sistem bileşenlerinin ölçümlerini içerir. Örneğin, çıktı dosyası, `\Details\Workbench\apiMetrics.txt` toplama dönemi boyunca farklı yanıt kodlarının ve yanıt sürelerinin bir özetini içerir. **Ayrıntılar** klasörü, Workbench veya altta yatan blockchain ağıyla ilgili belirli sorunları gidermek için ayrıntılı günlükler içerir. Örneğin, `\Details\Workbench\Exceptions.csv` sistemde oluşan en son özel durumların bir listesini içerir ve bu liste, akıllı sözleşmelerdeki hataları veya blockchain ile etkileşimleri gidermek için yararlıdır. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Blockchain Workbench Application Insights sorun giderme kılavuzu](https://aka.ms/workbenchtroubleshooting)
