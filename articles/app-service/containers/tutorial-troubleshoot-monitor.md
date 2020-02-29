---
title: 'Öğretici: Azure Izleyici ile sorun giderme'
description: Azure Izleyici ve Log Analytics App Service Web uygulamanızı izlemenize nasıl yardımcı olduğunu öğrenin. Azure Izleyici, ortamlarınızı izlemeye yönelik kapsamlı bir çözüm sunarak kullanılabilirliği en üst düzeye çıkarır.
author: msangapu-msft
ms.author: msangapu
ms.topic: tutorial
ms.date: 1/29/2019
ms.openlocfilehash: c0df972d893e82d7b49d5b3ed2a2bbf7067548c2
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78201818"
---
# <a name="tutorial-troubleshoot-an-app-service-app-with-azure-monitor"></a>Öğretici: Azure Izleyici ile App Service uygulamasında sorun giderme

[Linux’ta App Service](app-service-linux-intro.md) Linux işletim sistemini kullanan yüksek oranda ölçeklenebilir, otomatik olarak düzeltme eki uygulayan bir web barındırma hizmeti sağlar. [Azure izleyici](https://docs.microsoft.com/azure/azure-monitor/overview) , bulut ve şirket içi ortamlarınızdaki telemetri toplama, çözümleme ve üzerinde işlem yapmaya yönelik kapsamlı bir çözüm sunarak uygulamalarınızın ve hizmetlerinizin kullanılabilirliğini ve performansını en üst düzeye çıkarır.

Bu öğreticide, [Azure izleyici](https://docs.microsoft.com/azure/azure-monitor/overview)kullanarak bir uygulamayla ilgili sorunları giderme gösterilmektedir. Örnek uygulama, belleği tüketmeye yönelik kodu içerir ve HTTP 500 hatalarına neden olur, böylece Azure Izleyici 'yi kullanarak sorunu tanılayabilir ve giderebilirsiniz.

İşiniz bittiğinde, [Azure izleyici](https://docs.microsoft.com/azure/azure-monitor/overview)Ile tümleştirilmiş Linux üzerinde App Service üzerinde çalışan bir örnek uygulamanız olacaktır.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Azure Izleyici ile bir Web uygulaması yapılandırma
> * Konsol günlüklerini Log Analytics gönder
> * Web uygulaması hatalarını tanımlamak ve sorunlarını gidermek için günlük sorgularını kullanma

Bu öğreticideki adımları MacOS, Linux ve Windows üzerinde izleyebilirsiniz.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlayabilmeniz için şunlar gerekir:

- [Azure aboneliği](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
- [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli)
- [Git](https://git-scm.com/)

## <a name="create-azure-resources"></a>Azure kaynakları oluşturma

İlk olarak, bu öğreticiyle kullanılacak örnek bir uygulama ayarlamak için birkaç komutu yerel olarak çalıştırırsınız. Komutlar örnek bir uygulamayı kopyalar, Azure kaynakları oluşturur, bir dağıtım kullanıcısı oluşturur ve uygulamayı Azure 'a dağıtır. Dağıtım kullanıcısı oluşturmanın bir parçası olarak sağlanan parola istenir. 

```bash
git clone https://github.com/Azure-Samples/App-Service-Troubleshoot-Azure-Monitor
az group create --name myResourceGroup --location "South Central US"
az webapp deployment user set --user-name <username> --password <password>
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku B1 --is-linux
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app-name> --runtime "PHP|7.3" --deployment-local-git
git remote add azure <url_from_previous_step>
git push azure master
```

## <a name="configure-azure-monitor"></a>Azure Izleyicisini yapılandırma

### <a name="create-a-log-analytics-workspace"></a>Log Analytics çalışma alanı oluşturma

Örnek uygulamayı Azure App Service dağıttığınıza göre, sorun ortaya çıktığında uygulamanın sorunlarını gidermek için izleme özelliğini yapılandıracaksınız. Azure Izleyici, günlük verilerini bir Log Analytics çalışma alanında depolar. Çalışma alanı, veri ve yapılandırma bilgilerini içeren bir kapsayıcıdır.

Bu adımda, Azure Izleyici 'yi uygulamanızla birlikte yapılandırmak için bir Log Analytics çalışma alanı oluşturursunuz.

```bash
az monitor log-analytics workspace create --resource-group myResourceGroup --workspace-name myMonitorWorkspace
```

> [!NOTE]
> [Azure Izleyici Log Analytics için veri alımı ve veri saklama için ödeme yaparsınız.](https://azure.microsoft.com/pricing/details/monitor/)
>

### <a name="create-a-diagnostic-setting"></a>Tanılama ayarı oluştur

Tanılama ayarları, belirli Azure hizmetleri için Azure Izleyici günlüklerine yönelik ölçümleri, günlük sorguları kullanılarak diğer izleme verileriyle analiz edilmek üzere toplamak için kullanılabilir. Bu öğreticide, Web sunucusunu ve standart çıkış/hata günlüklerini etkinleştirirsiniz. Günlük türlerinin ve açıklamalarının tüm listesi için bkz. [desteklenen günlük türleri](https://docs.microsoft.com/azure/app-service/troubleshoot-diagnostic-logs#supported-log-types) .

AppServiceConsoleLogs (Standart çıkış/hata) ve AppServiceHTTPLogs (Web sunucusu günlükleri) için Tanılama ayarları oluşturmak üzere aşağıdaki komutları çalıştırın. _\<app-name >_ ve _\<çalışma alanı-adı >_ değerini değerlerinizle değiştirin. 

> [!NOTE]
> `resourceID` ve `workspaceID`ilk iki komut, `az monitor diagnostic-settings create` komutunda kullanılacak değişkenlerdir. Bu komutla ilgili daha fazla bilgi için bkz. [Azure CLI kullanarak tanılama ayarları oluşturma](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings#create-diagnostic-settings-using-azure-cli) .
>

```bash

resourceID=$(az webapp show -g myResourceGroup -n <app-name> --query id --output tsv)

workspaceID=$(az monitor log-analytics workspace show -g myResourceGroup --workspace-name <workspace-name> --query id --output tsv)

az monitor diagnostic-settings create --resource $resourceID \
 --workspace $workspaceID \
 -n myMonitorLogs \
 --logs '[{"category": "AppServiceConsoleLogs", "enabled": true},
  {"category": "AppServiceHTTPLogs", "enabled": true}]'

```

## <a name="troubleshoot-the-app"></a>Uygulamanın sorunlarını giderme

`http://<app-name>.azurewebsites.net` adresine gidin.

Örnek uygulama olan ImageConverter, `JPG` eklenen görüntüleri `PNG`dönüştürür. Bu öğreticinin koduna kasıtlı olarak bir hata yerleştirildi. Yeterli görüntü seçerseniz, uygulama görüntü dönüştürme sırasında bir HTTP 500 hatası oluşturur. Bu senaryonun geliştirme aşamasında dikkate alınmadığını düşünün. Hatayı gidermek için Azure Izleyici 'yi kullanacaksınız.

### <a name="verify-the-app-is-works"></a>Uygulamanın çalıştığını doğrulama

Görüntüleri dönüştürmek için `Tools` ' ye tıklayın ve `Convert to PNG`' ı seçin.

![' Araçlar 'ı tıklatın ve ' PNG 'ye Dönüştür ' seçeneğini belirleyin](./media/tutorial-azure-monitor/sample-monitor-app-tools-menu.png)

İlk iki görüntüyü seçin ve `convert`' ye tıklayın. Bu, başarıyla dönüştürülecek.

![İlk iki görüntüyü seçin](./media/tutorial-azure-monitor/sample-monitor-app-convert-two-images.png)

### <a name="break-the-app"></a>Uygulamayı kes

İki görüntüyü başarıyla dönüştürerek uygulamayı doğruladığınıza göre, ilk beş görüntüyü dönüştürmeyi deneyeceğiz.

![İlk beş görüntüyü Dönüştür](./media/tutorial-azure-monitor/sample-monitor-app-convert-five-images.png)

Bu eylem başarısız olur ve geliştirme sırasında sınanmamıştır `HTTP 500` bir hata üretir.

![Convert, HTTP 500 hatasına neden olur](./media/tutorial-azure-monitor/sample-monitor-app-http-500.png)

## <a name="use-log-query-to-view-azure-monitor-logs"></a>Azure Izleyici günlüklerini görüntülemek için günlük sorgusu kullanma

Log Analytics çalışma alanında hangi günlüklerin kullanılabildiğini görelim. 

Azure portal çalışma alanınıza erişmek için bu [Log Analytics çalışma alanı bağlantısına](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.OperationalInsights%2Fworkspaces) tıklayın.

Azure portal, Log Analytics çalışma alanınızı seçin.

### <a name="log-queries"></a>Günlük sorguları

Günlük sorguları, Azure Izleyici günlüklerinde toplanan verilerin değerini tamamen kullanmanıza yardımcı olur. Günlük sorgularını hem AppServiceHTTPLogs hem de AppServiceConsoleLogs içindeki günlükleri tanımlamak için kullanırsınız. Günlük sorguları hakkında daha fazla bilgi için [günlük sorgusuna genel bakış](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview) bölümüne bakın.

### <a name="view-appservicehttplogs-with-log-query"></a>AppServiceHTTPLogs 'u günlük sorgusuyla görüntüleme

Uygulamaya eriştiğimiz artık, `AppServiceHTTPLogs`bulunan HTTP istekleriyle ilişkili verileri görüntüleyelim.

1. Sol taraftaki gezinmede `Logs` ' a tıklayın.

![Anlytika çalışma günlüklerini günlüğe kaydet](./media/tutorial-azure-monitor/log-analytics-workspace-logs.png)

2. `appservice` arayın ve `AppServiceHTTPLogs`çift tıklayın.

![Log Analytics çalışma alanı tabloları](./media/tutorial-azure-monitor/log-analytics-workspace-app-service-tables.png)

3. `Run` öğesine tıklayın.

![Log Analytics çalışma alanı App Service HTTP günlükleri](./media/tutorial-azure-monitor/log-analytics-workspace-app-service-http-logs.png)

`AppServiceHTTPLogs` sorgusu, son 24 saat içindeki tüm istekleri döndürür. `ScStatus` sütunu HTTP durumunu içerir. `HTTP 500` hatalarını tanılamak için `ScStatus` 500 ile sınırlayın ve sorguyu aşağıda gösterildiği gibi çalıştırın:

```kusto
AppServiceHTTPLogs
| where ScStatus == 500
```

### <a name="view-appserviceconsolelogs-with-log-query"></a>AppServiceConsoleLogs 'u günlük sorgusuyla görüntüleme

Artık HTTP 500s ' i onayladığınıza göre uygulamadan standart çıkış/hatalara göz atalım. Bu Günlükler ' AppServiceConsoleLogs ' içinde bulunur.

(1) yeni bir sorgu oluşturmak için `+` ' a tıklayın. 

(2) `AppServiceConsoleLogs` tabloya çift tıklayın ve `Run`' a tıklayın. 

Beş Görüntüyü dönüştürürken sunucu hatalarına neden olduğundan, aşağıda gösterildiği gibi, hata için `ResultDescription` filtreleyerek uygulamanın da hata yazıyor olduğunu görebilirsiniz:

```kusto
AppServiceConsoleLogs |
where ResultDescription  contains "error" 
```

`ResultDescription` sütununda aşağıdaki hatayı görürsünüz:

```
PHP Fatal error:  Allowed memory size of 134217728 bytes exhausted 
(tried to allocate 16384 bytes) in /home/site/wwwroot/process.php on line 20, 
referer: http://<app-name>.azurewebsites.net/
```

### <a name="join-appservicehttplogs-and-appserviceconsolelogs"></a>AppServiceHTTPLogs ve AppServiceConsoleLogs 'a katılarak

HTTP 500s ve standart hatalarını tanımladığınıza göre, bu iletiler arasında bir bağıntı olup olmadığını onaylamanız gerekir. Daha sonra, `TimeGenerated`zaman damgasına göre tabloları birlikte birleştirebilirsiniz.

> [!NOTE]
> Şunları yapmak için bir sorgu hazırlanmıştır:
>
> - 500 hata için HTTPLogs filtrelerini filtreler
> - Konsol günlüklerini sorgular
> - `TimeGenerated` tabloları birleştirir
>

Şu sorguyu çalıştırın:

```kusto
let myHttp = AppServiceHTTPLogs | where  ScStatus == 500 | project TimeGen=substring(TimeGenerated, 0, 19), CsUriStem, ScStatus;  

let myConsole = AppServiceConsoleLogs | project TimeGen=substring(TimeGenerated, 0, 19), ResultDescription;

myHttp | join myConsole on TimeGen | project TimeGen, CsUriStem, ScStatus, ResultDescription;
```

`ResultDescription` sütununda, Web sunucusu hatalarıyla aynı anda aşağıdaki hatayı görürsünüz:

```
PHP Fatal error:  Allowed memory size of 134217728 bytes exhausted 
(tried to allocate 16384 bytes) in /home/site/wwwroot/process.php on line 20, 
referer: http://<app-name>.azurewebsites.net/
```

İleti, belleğin `process.php`20. satırında tükendi. Artık HTTP 500 hatası sırasında uygulamanın bir hata üretmediğini onayladık. Sorunu belirlemek için koda göz atalım.

## <a name="identify-the-error"></a>Hatayı tanımla

Yerel dizinde `process.php` açın ve 20. satıra bakın. 

```php
imagepng($imgArray[$x], $filename);
```

`$imgArray[$x]`ilk bağımsız değişken, dönüştürmeye ihtiyaç duyan tüm JPGs (bellekteki) tutan bir değişkendir. Ancak, `imagepng` yalnızca görüntünün dönüştürülmesini gerektirir ve tüm görüntüleri değil. Önceden yükleme görüntüleri gerekli değildir ve bellek tükenmesi ile, önde gelen HTTP 500s ' e yol açabilir. Sorunu çözüp çözmediğine bakmak için, daha sonra bu görüntüyü isteğe bağlı olarak yükleyecek şekilde kodu güncelleştirelim. Daha sonra, bellek sorununu gidermek için kodu iyileştirecaksınız.

## <a name="fix-the-app"></a>Uygulama düzeltme

### <a name="update-locally-and-redeploy-the-code"></a>Kodu yerel makinede güncelleştirme ve yeniden dağıtma

Bellek tükenmesi 'ni işlemek için `process.php` aşağıdaki değişiklikleri yaparsınız:

```php
<?php

//Retrieve query parameters
$maxImages = $_GET['images'];
$imgNames  = explode(",",$_GET['imgNames']);

//Load JPEGs into an array (in memory)
for ($x=0; $x<$maxImages; $x++){
    $filename = './images/converted_' . substr($imgNames[$x],0,-4) . '.png';
    imagepng(imagecreatefromjpeg("./images/" . $imgNames[$x]), $filename);
}
```

Değişikliklerinizi Git’e işleyin ve ardından kod değişikliklerini Azure’a gönderin.

```bash
git commit -am "Load images on-demand in process.php"
git push azure master
```

### <a name="browse-to-the-azure-app"></a>Azure uygulamasına gidin

`http://<app-name>.azurewebsites.net` adresine gidin. 

Görüntülerin dönüştürülmesi, HTTP 500 hatalarını daha uzun bir şekilde üretmemelidir.

![Azure App Service’te çalışan PHP uygulaması](./media/tutorial-azure-monitor/sample-monitor-app-working.png)

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

Aşağıdaki komutla tanılama ayarını silin:

```bash
az monitor diagnostic-settings delete --resource $resourceID -n myMonitorLogs
```
Öğrendikleriniz:

> [!div class="checklist"]
> * Azure Izleyici ile bir Web uygulaması yapılandırıldı
> * Günlükler Log Analytics gönderildi
> * Web uygulaması hatalarını tanımlamak ve sorunlarını gidermek için kullanılan günlük sorguları

## <a name="nextsteps"></a> Sonraki adımlar
* [Azure Izleyici ile günlük sorgulama](../../azure-monitor/log-query/log-query-overview.md)
* [Visual Studio 'da Azure App Service sorunlarını giderme](../troubleshoot-dotnet-visual-studio.md)
* [HDInsight 'ta uygulama günlüklerini çözümleme](https://gallery.technet.microsoft.com/scriptcenter/Analyses-Windows-Azure-web-0b27d413)
