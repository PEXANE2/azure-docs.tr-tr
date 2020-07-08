---
title: Azure blok zinciri ve sorun giderme
description: Azure blok zinciri çalışma ekranı önizleme uygulaması sorunlarını giderme.
ms.date: 10/14/2019
ms.topic: troubleshooting
ms.reviewer: brendal
ms.openlocfilehash: 20c0f9bdd6f820a73b1ba6660de805268c0d8714
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85212862"
---
# <a name="azure-blockchain-workbench-preview-troubleshooting"></a>Azure blok zinciri ekranı önizleme sorunlarını giderme

Geliştirici hata ayıklamasına veya desteklemeye yardımcı olmak için bir PowerShell betiği kullanılabilir. Betik bir Özet oluşturur ve sorun giderme için ayrıntılı günlükleri toplar. Toplanan Günlükler şunları içerir:

* Ethereum gibi blok zinciri ağı
* Blok zinciri çalışma ekranı mikro hizmetleri
* Application Insights
* Azure Izleme (Azure Izleyici günlükleri)

Sonraki adımları tespit etmek ve sorunların temel nedenini anlamak için bu bilgileri kullanabilirsiniz.

[!INCLUDE [Preview note](./includes/preview.md)]

## <a name="troubleshooting-script"></a>Sorun giderme betiği

PowerShell sorun giderme betiği GitHub ' da kullanılabilir. GitHub’dan [zip dosyasını indirin](https://github.com/Azure-Samples/blockchain/archive/master.zip) veya örneği kopyalayın.

```
git clone https://github.com/Azure-Samples/blockchain.git
```

## <a name="run-the-script"></a>Betiği çalıştırın
[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

`collectBlockchainWorkbenchTroubleshooting.ps1`Günlükleri toplamak ve sorun giderme bilgileri klasörünü içeren BIR ZIP dosyası oluşturmak için betiği çalıştırın. Örneğin:

``` powershell
collectBlockchainWorkbenchTroubleshooting.ps1 -SubscriptionID "<subscription_id>" -ResourceGroupName "workbench-resource-group-name"
```
Betik aşağıdaki parametreleri kabul eder:

| Parametre  | Açıklama | Gerekli |
|---------|---------|----|
| SubscriptionID | Tüm kaynakları oluşturmak veya bulmak için SubscriptionID. | Evet |
| ResourceGroupName | Blok zinciri çalışma ekranının dağıtıldığı Azure Kaynak grubunun adı. | Evet |
| OutputDirectory | Çıktının oluşturulacağı yol. ZIP dosyası. Belirtilmezse, varsayılan olarak geçerli dizini alır. | Hayır |
| Geriye yönelik saat | Telemetri çekme sırasında kullanılacak saat sayısı. Varsayılan değer 24 saattir. Maksimum değer 90 saattir | Hayır |
| Omssubscriptionıd | Azure Izleyici günlüklerinin dağıtıldığı abonelik KIMLIĞI. Bu parametreyi yalnızca blok zinciri ağı için Azure Izleyici günlükleri blok zinciri çalışma ekranının kaynak grubu dışında dağıtılırsa geçirin.| Hayır |
| OmsResourceGroup |Azure Izleyici günlüklerinin dağıtıldığı kaynak grubu. Bu parametreyi yalnızca blok zinciri ağı için Azure Izleyici günlükleri blok zinciri çalışma ekranının kaynak grubu dışında dağıtılırsa geçirin.| Hayır |
| Omsçalışmaalanıadı | Log Analytics çalışma alanı adı. Bu parametreyi yalnızca blok zinciri ağı için Azure Izleyici günlüklerinin, blok zinciri çalışma ekranının kaynak grubu dışında dağıtılırsa geçirin | Hayır |

## <a name="what-is-collected"></a>Ne toplanır?

Çıkış ZIP dosyası aşağıdaki klasör yapısını içerir:

| Klasör veya dosya | Açıklama  |
|---------|---------|
| \Summary.txt | Sistemin Özeti |
| \Metrics\blockzinciri | Blok zinciri hakkında ölçümler |
| \Metrics\çalışma alanı | Çalışma ekranı hakkında ölçümler |
| \Details\blockzinciri | Blok zinciri hakkında ayrıntılı Günlükler |
| \ Ayrıntı \ çalışma alanı | Çalışma ekranı hakkında ayrıntılı Günlükler |

Özet dosyası, uygulamanın genel durumunun bir anlık görüntüsünü ve uygulamanın sistem durumunu sağlar. Özet, önerilen eylemleri, en önemli hataları vurgular ve Hizmetleri çalıştırma hakkında meta verileri sağlar.

**Ölçüm** klasörü, zaman içinde çeşitli sistem bileşenlerinin ölçümlerini içerir. Örneğin, çıkış dosyası `\Details\Workbench\apiMetrics.txt` farklı yanıt kodlarının özetini ve toplama süresi boyunca yanıt sürelerini içerir. **Ayrıntılar** klasörü, çalışma ekranı veya temeldeki blok zinciri ağı ile ilgili sorunları gidermeye yönelik ayrıntılı günlükleri içerir. Örneğin, `\Details\Workbench\Exceptions.csv` Sistemde oluşan en son özel durumların bir listesini içerir, bu, akıllı sözleşmelerle veya blok zinciriyle etkileşimlerle ilgili sorunları gidermek için kullanışlıdır. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure blok zinciri çalışma ekranı Application Insights sorun giderme kılavuzu](https://aka.ms/workbenchtroubleshooting)
