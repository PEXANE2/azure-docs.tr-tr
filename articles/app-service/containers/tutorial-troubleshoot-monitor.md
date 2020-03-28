---
title: 'Öğretici: Azure Monitör ile Sorun Giderme'
description: Azure Monitor ve Log Analytics'in Uygulama Hizmeti web uygulamanızı izlemenize nasıl yardımcı olduğunu öğrenin. Azure Monitor, ortamlarınızı izlemek için kapsamlı bir çözüm sunarak kullanılabilirliği en üst düzeye çıkarır.
author: msangapu-msft
ms.author: msangapu
ms.topic: tutorial
ms.date: 2/28/2020
ms.openlocfilehash: d543a9364311b2cf5f0258fbf9185d27bb1bfb2f
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78399521"
---
# <a name="tutorial-troubleshoot-an-app-service-app-with-azure-monitor"></a>Öğretici: Azure Monitor ile Bir Uygulama Hizmeti uygulamasını sorun giderme

> [!NOTE]
> Uygulama Hizmeti ile Azure Monitör [tümleştirmesi önizlemede.](https://aka.ms/appsvcblog-azmon)
>

[Linux’ta App Service](app-service-linux-intro.md) Linux işletim sistemini kullanan yüksek oranda ölçeklenebilir, otomatik olarak düzeltme eki uygulayan bir web barındırma hizmeti sağlar. [Azure Monitor,](https://docs.microsoft.com/azure/azure-monitor/overview) bulut ve şirket içi ortamlarınızdan telemetri toplamak, analiz etmek ve hareket etmek için kapsamlı bir çözüm sunarak uygulamalarınızın ve hizmetlerinizin kullanılabilirliğini ve performansını en üst düzeye çıkarır.

Bu öğretici, [Azure Monitor'u](https://docs.microsoft.com/azure/azure-monitor/overview)kullanarak bir uygulamayı nasıl gidereceklerini gösterir. Örnek uygulama, belleği tüketmeye ve HTTP 500 hatalarına neden olacak kodlar içerir, böylece Azure Monitor'u kullanarak sorunu tanılayabilir ve düzeltebilirsiniz.

İşi nizi bitirdiğinde, [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview)ile entegre edilmiş Linux'ta App Service'de çalışan bir örnek uygulamanız olacaktır.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Azure Monitor ile bir web uygulamasını yapılandırma
> * Konsol günlüklerini Log Analytics'e gönderme
> * Web uygulaması hatalarını tanımlamak ve sorun gidermek için Günlük sorgularını kullanma

Bu öğreticideki adımları MacOS, Linux ve Windows üzerinde izleyebilirsiniz.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için şunları yapmanız gerekir:

- [Azure aboneliği](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
- [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli)
- [Git](https://git-scm.com/)

## <a name="create-azure-resources"></a>Azure kaynakları oluşturma

İlk olarak, bu öğretici ile kullanmak için örnek bir uygulama kurmak için yerel olarak çeşitli komutlar çalıştırın. Komutlar örnek bir uygulamayı klonlar, Azure kaynakları oluşturur, dağıtım kullanıcısı oluşturur ve uygulamayı Azure'a dağıtır. Dağıtım kullanıcısının oluşturulmasının bir parçası olarak sağlanan parola için istenirsiniz. 

```bash
git clone https://github.com/Azure-Samples/App-Service-Troubleshoot-Azure-Monitor
az group create --name myResourceGroup --location "South Central US"
az webapp deployment user set --user-name <username> --password <password>
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku B1 --is-linux
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app-name> --runtime "PHP|7.3" --deployment-local-git
git remote add azure <url_from_previous_step>
git push azure master
```

## <a name="configure-azure-monitor-preview"></a>Azure Monitörünü Yapılandırma (önizleme)

### <a name="create-a-log-analytics-workspace"></a>Günlük Analizi Çalışma Alanı Oluşturma

Örnek uygulamayı Azure Uygulama Hizmeti'ne dağıttığınız için, sorunlar ortaya çıktığında uygulamayı sorun giderecek izleme özelliğini yapılandıracaksınız. Azure Monitor günlük verilerini bir Log Analytics çalışma alanında depolar. Çalışma alanı, veri ve yapılandırma bilgilerini içeren bir kapsayıcıdır.

Bu adımda, Azure Monitörünü uygulamanızla yapılandırmak için bir Log Analytics çalışma alanı oluşturursunuz.

```bash
az monitor log-analytics workspace create --resource-group myResourceGroup --workspace-name myMonitorWorkspace
```

> [!NOTE]
> [Azure Monitor Log Analytics için veri alımı ve veri saklama için ödeme yapabilirsiniz.](https://azure.microsoft.com/pricing/details/monitor/)
>

### <a name="create-a-diagnostic-setting"></a>Tanılama ayarı oluşturma

Tanılama ayarları, günlük sorgularını kullanarak diğer izleme verileriyle analiz için azure monitör günlüklerinde belirli Azure hizmetlerinin ölçümlerini toplamak için kullanılabilir. Bu öğretici için, web sunucusunu ve standart çıktı/hata günlüklerini etkinleştirin. Günlük türlerinin ve açıklamaların tam listesi için [desteklenen günlük türlerine](https://docs.microsoft.com/azure/app-service/troubleshoot-diagnostic-logs#supported-log-types) bakın.

AppServiceConsoleLogs (standart çıktı/hata) ve AppServiceHTTPLogs (web sunucusu günlükleri) için tanılama ayarları oluşturmak için aşağıdaki komutları çalıştırın. _ \<Uygulama adı>_ ve _ \<çalışma alanı adı>_ değerlerinizle değiştirin. 

> [!NOTE]
> İlk iki komut `resourceID` ve `workspaceID`, `az monitor diagnostic-settings create` komutkullanılacak değişkenlerdir. Bu komut hakkında daha fazla bilgi için [Azure CLI kullanarak tanı lama ayarları oluşturma'ya](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings#create-diagnostic-settings-using-azure-cli) bakın.
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

## <a name="troubleshoot-the-app"></a>Uygulamayı sorun giderme

`http://<app-name>.azurewebsites.net` adresine gidin.

Örnek uygulama, ImageConverter, 'den `JPG` . `PNG` Bu öğreticinin koduna kasıtlı olarak bir hata yerleştirildi. Yeterli resim seçerseniz, uygulama görüntü dönüştürme sırasında bir HTTP 500 hatası oluşturur. Bu senaryonun geliştirme aşamasında dikkate alınmadığını düşünün. Hatayı gidermek için Azure Monitor'u kullanırsınız.

### <a name="verify-the-app-is-works"></a>Uygulamanın işe yaradığını doğrulayın

Resimleri dönüştürmek için `Tools` tıklatın `Convert to PNG`ve seçin.

!['Araçlar'ı tıklatın ve 'PNG'ye dönüştür' seçeneğini belirleyin](./media/tutorial-azure-monitor/sample-monitor-app-tools-menu.png)

İlk iki resmi seçin `convert`ve . Bu başarıyla dönüştürür.

![İlk iki görüntüyü seçin](./media/tutorial-azure-monitor/sample-monitor-app-convert-two-images.png)

### <a name="break-the-app"></a>Uygulamayı kır

İki görüntüyü başarıyla dönüştürerek uygulamayı doğruladığınıza göre, ilk beş görüntüyü dönüştürmeye çalışacağız.

![İlk beş görüntüyü dönüştürme](./media/tutorial-azure-monitor/sample-monitor-app-convert-five-images.png)

Bu eylem başarısız `HTTP 500` olur ve geliştirme sırasında sınanmadı bir hata üretir.

![Dönüştürme, HTTP 500 hatasına neden olur](./media/tutorial-azure-monitor/sample-monitor-app-http-500.png)

## <a name="use-log-query-to-view-azure-monitor-logs"></a>Azure Monitor günlüklerini görüntülemek için günlük sorgusunun kullanılması

Log Analytics çalışma alanında hangi günlüklerin kullanılabilmesini görelim. 

Azure portalındaki çalışma alanınıza erişmek için bu [Günlük Analizi çalışma alanı bağlantısını](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.OperationalInsights%2Fworkspaces) tıklatın.

Azure portalında, Günlük Analizi çalışma alanınızı seçin.

### <a name="log-queries"></a>Günlük sorguları

Günlük sorguları, Azure Monitör Günlükleri'nde toplanan verilerin değerinden tam olarak yararlanmanıza yardımcı olur. Hem AppServiceHTTPLogs hem de AppServiceConsoleLogs'daki günlükleri tanımlamak için günlük sorgularını kullanırsınız. Günlük sorguları hakkında daha fazla bilgi için [günlük sorgusuna genel bakışa](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview) bakın.

### <a name="view-appservicehttplogs-with-log-query"></a>Günlük sorgusuyla AppServiceHTTPLogs'u görüntüleyin

Uygulamaya erişmiş olduğumuza göre, http istekleriyle ilişkili verileri `AppServiceHTTPLogs`görüntüleyelim.

1. Sol `Logs` daki gezinmeden tıklayın.

![Günlük Anlitics Worksace Günlükleri](./media/tutorial-azure-monitor/log-analytics-workspace-logs.png)

2. Arama `appservice` ve çift `AppServiceHTTPLogs`tıklayın.

![Günlük analizi Çalışma Alanı Tabloları](./media/tutorial-azure-monitor/log-analytics-workspace-app-service-tables.png)

3. `Run` öğesine tıklayın.

![Günlük Analitik Çalışma Alanı Uygulama Hizmeti HTTP Günlükleri](./media/tutorial-azure-monitor/log-analytics-workspace-app-service-http-logs.png)

Sorgu, `AppServiceHTTPLogs` son 24 saat içinde tüm istekleri döndürür. Sütun `ScStatus` HTTP durumunu içerir. Hataları tanılamak `HTTP 500` için, `ScStatus` 500 ile sınırlayın ve sorguyu aşağıda gösterildiği gibi çalıştırın:

```kusto
AppServiceHTTPLogs
| where ScStatus == 500
```

### <a name="view-appserviceconsolelogs-with-log-query"></a>Günlük sorgusuyla AppServiceConsoleLogs'u görüntüleyin

ŞIMDI HTTP 500'leri doğruladıysanız, uygulamadan alınan standart çıktılara/hatalara bir göz atalım. Bu günlükler 'AppServiceConsoleLogs'da bulunur.

(1) `+` Yeni bir sorgu oluşturmak için tıklatın. 

(2) Tabloya `AppServiceConsoleLogs` çift tıklayın `Run`ve . 

Beş görüntüyü dönüştürme, sunucu hatalarıyla sonuçladığından, uygulamanın hatalara `ResultDescription` filtre uygulayarak da hata yazıp yazmadığınızı aşağıda göstererek görebilirsiniz:

```kusto
AppServiceConsoleLogs |
where ResultDescription  contains "error" 
```

`ResultDescription` Sütunda aşağıdaki hatayı görürsünüz:

```
PHP Fatal error:  Allowed memory size of 134217728 bytes exhausted 
(tried to allocate 16384 bytes) in /home/site/wwwroot/process.php on line 20, 
referer: http://<app-name>.azurewebsites.net/
```

### <a name="join-appservicehttplogs-and-appserviceconsolelogs"></a>AppServiceHTTPLogs ve AppServiceConsoleLogs katılın

Hem HTTP 500'leri hem de standart hataları tanımladığınıza göre, bu iletiler arasında bir ilişki olup olmadığını doğrulamanız gerekir. Sonra, zaman damgası dayalı tablolar birlikte `TimeGenerated`katılmak.

> [!NOTE]
> Sizin için aşağıdakileri yapan bir sorgu hazırlanmıştır:
>
> - Filtreler 500 hata için HTTPLogs
> - Sorgular konsol günlükleri
> - Üzerinde tablolara katılır`TimeGenerated`
>

Aşağıdaki sorguyu çalıştırın:

```kusto
let myHttp = AppServiceHTTPLogs | where  ScStatus == 500 | project TimeGen=substring(TimeGenerated, 0, 19), CsUriStem, ScStatus;  

let myConsole = AppServiceConsoleLogs | project TimeGen=substring(TimeGenerated, 0, 19), ResultDescription;

myHttp | join myConsole on TimeGen | project TimeGen, CsUriStem, ScStatus, ResultDescription;
```

`ResultDescription` Sütunda, web sunucusu hatalarıyla aynı anda aşağıdaki hatayı görürsünüz:

```
PHP Fatal error:  Allowed memory size of 134217728 bytes exhausted 
(tried to allocate 16384 bytes) in /home/site/wwwroot/process.php on line 20, 
referer: http://<app-name>.azurewebsites.net/
```

İleti, belleğin `process.php`20. Şimdi uygulama nın HTTP 500 hatası sırasında bir hata ürettiğini doğruladınız. Sorunu tanımlamak için koda bir göz atalım.

## <a name="identify-the-error"></a>Hatayı belirleme

Yerel dizinde, açın `process.php` ve satır 20 bakmak. 

```php
imagepng($imgArray[$x], $filename);
```

İlk bağımsız `$imgArray[$x]`değişken, dönüşüm gerektiren tüm YBM'leri (bellek tespül eden) bir değişkendir. Ancak, `imagepng` yalnızca görüntünün dönüştürülmesi gerekir ve tüm görüntüler değil. Ön yükleme görüntüleri gerekli değildir ve http 500s yol açan bellek yorgunluğuna neden olabilir. Sorunu çözüp çözmedigini görmek için görüntüleri isteğe bağlı yüklemek için kodu güncelleştirelim. Ardından, bellek sorununu gidermek için kodu geliştireceksiniz.

## <a name="fix-the-app"></a>Uygulamayı düzeltme

### <a name="update-locally-and-redeploy-the-code"></a>Kodu yerel makinede güncelleştirme ve yeniden dağıtma

Bellek yorgunluğunu işlemek `process.php` için aşağıdaki değişiklikleri yaparsınız:

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

### <a name="browse-to-the-azure-app"></a>Azure uygulamasına göz atın

`http://<app-name>.azurewebsites.net` adresine gidin. 

Görüntüleri dönüştürmek artık HTTP 500 hatalarını oluşturmamalıdır.

![Azure App Service’te çalışan PHP uygulaması](./media/tutorial-azure-monitor/sample-monitor-app-working.png)

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

Tanılama ayarını aşağıdaki komutla silin:

```bash
az monitor diagnostic-settings delete --resource $resourceID -n myMonitorLogs
```
Öğrendikleriniz:

> [!div class="checklist"]
> * Azure Monitor ile bir web uygulamasını yapılandırıldı
> * Günlükler Log Analytics'e gönderildi
> * Web uygulaması hatalarını tanımlamak ve sorun gidermek için kullanılan günlük sorguları

## <a name="next-steps"></a><a name="nextsteps"></a>Sonraki adımlar
* [Azure Monitor ile günlükleri sorgula](../../azure-monitor/log-query/log-query-overview.md)
* [Visual Studio'da Azure Uygulama Hizmetinde Sorun Giderme](../troubleshoot-dotnet-visual-studio.md)
* [HDInsight'ta uygulama Günlüklerini analiz edin](https://gallery.technet.microsoft.com/scriptcenter/Analyses-Windows-Azure-web-0b27d413)
