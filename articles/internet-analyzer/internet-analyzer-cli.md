---
title: CLI kullanarak Internet Analyzer testi oluşturma | Microsoft Dokümanlar
description: Bu makalede, ilk Internet Analyzer testioluşturmak için nasıl öğrenin.
services: internet-analyzer
author: diego-perez-botero
ms.service: internet-analyzer
ms.topic: tutorial
ms.date: 10/16/2019
ms.author: mebeatty
ms.openlocfilehash: d474442086e2a114f26df279ab2682cd7628a5f5
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74184276"
---
# <a name="create-an-internet-analyzer-test-using-cli-preview"></a>CLI kullanarak Internet Analyzer testi oluşturma (Önizleme)

[Azure portalını](internet-analyzer-create-test-portal.md) kullanarak veya CLI'yi kullanarak Bir Internet Çözümleyici kaynağı oluşturmanın iki yolu vardır. Bu bölüm, CLI deneyimimizi kullanarak yeni bir Azure Internet Çözümleyici kaynağı oluşturmanıza yardımcı olur. 


> [!IMPORTANT]
> Bu genel önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılmamalıdır. Belirli özellikler desteklenmiyor olabilir, kısıtlı yeteneklere sahip olabilir veya tüm Azure konumlarında mevcut olmayabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

## <a name="before-you-begin"></a>Başlamadan önce

Genel önizleme genel olarak kullanılabilir; ancak, veri depolama önizleme sırasında *US West 2* ile sınırlıdır.

## <a name="object-model"></a>Nesne modeli
Internet Analyzer CLI aşağıdaki kaynak türlerini ortaya çıkarır:
* **Testler** - Bir test, iki internet uç noktasının (A ve B) zaman içinde son kullanıcı performansını karşılaştırır.
* **Profiller** - Testler Internet Analyzer profili altında oluşturulur. Profiller, ilgili testlerin gruplandırılmasını sağlar; tek bir profil bir veya daha fazla test içerebilir.
* **Önceden yapılandırılmış Uç Noktalar** - Çeşitli yapılandırmalarla (bölgeler, hızlanma teknolojileri, vb.) uç noktalar belirledik. Bu önceden yapılandırılmış uç noktalardan herhangi birini testlerinizde kullanabilirsiniz.
* **Karneler** - Karne, ölçüm sonuçlarının hızlı ve anlamlı özetlerini sağlar. [Karnenizi Yorumlamaya](internet-analyzer-scorecard.md)bakın.
* **Zaman Serisi** - Zaman serisi, bir metnin zaman içinde nasıl değiştiğini gösterir.

## <a name="profile-and-test-creation"></a>Profil ve Test Oluşturma
1. [Azure Internet Analyzer SSS'den](internet-analyzer-faq.md)gelen yönergeleri izleyerek Internet Analyzer önizleme erişimi **alın.**
2. [Azure CLI'yi yükleyin.](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
3. CLI `login` oturumunu başlatmak için komutu çalıştırın:
    ```azurecli-interactive
    az login
    ```

    CLI varsayılan tarayıcınızı açabilirse, bunu yapar ve bir Azure oturum açma sayfası yükler.
    Aksi takdirde, terminalinizde görüntülenen yetkilendirme kodunu girin https://aka.ms/devicelogin ve bir tarayıcı sayfası açın.

4. Tarayıcıda hesabınızın kimlik bilgileriyle oturum açın.

5. Internet Analyzer genel önizlemesine erişim izni verilmiş Olan Abonelik Kimliğinizi seçin.

    Oturum açtıktan sonra Azure hesabınızla ilişkili aboneliklerin listesini görürsünüz. Abonelik bilgileri, `isDefault: true` oturum açtıktan sonra şu anda etkinleştirilen aboneliktir. Başka bir abonelik seçmek için, abonelik kimliğine geçmek için [az hesap kümesi](https://docs.microsoft.com/cli/azure/account#az-account-set) komutunu kullanın. Abonelik seçimi hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest)

    [Azure CLI ile oturum açma](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest) bölümünde ayrıntılı bir şekilde ele alınan etkileşimsiz olarak oturum açma yöntemleri mevcuttur.

6. **[İsteğe bağlı]** Yeni bir Azure Kaynak Grubu oluşturun:
    ```azurecli-interactive
    az group create --location eastus --name "MyInternetAnalyzerResourceGroup"
    ```

7. Azure CLI Internet Analyzer Uzantısını yükleyin:
     ```azurecli-interactive
    az extension add --name internet-analyzer
    ```

8. Yeni bir Internet Analyzer profili oluşturun:
    ```azurecli-interactive
    az internet-analyzer profile create --location eastus --resource-group "MyInternetAnalyzerResourceGroup" --name "MyInternetAnalyzerProfile" --enabled-state Enabled
    ```

9. Yeni oluşturulan profilde bulunan önceden yapılandırılmış tüm uç noktaları listele:
    ```azurecli-interactive
    az internet-analyzer preconfigured-endpoint list --resource-group "MyInternetAnalyzerResourceGroup" --profile-name "MyInternetAnalyzerProfile"
    ```

10. Yeni oluşturulan InternetAnalyzer profili altında yeni bir test oluşturun:
    ```azurecli-interactive
    az internet-analyzer test create --resource-group "MyInternetAnalyzerResourceGroup" --profile-name "MyInternetAnalyzerProfile" --endpoint-a-name "contoso" --endpoint-a-endpoint "www.contoso.com/some/path/to/trans.gif" --endpoint-b-name "microsoft" --endpoint-b-endpoint "www.microsoft.com/another/path/to/trans.gif" --name "MyFirstInternetAnalyzerTest" --enabled-state Enabled
    ```

    Yukarıdaki komut, her `www.contoso.com` iki `www.microsoft.com` sinin de özel yollar altında tek pikselli görüntüyü[(trans.gif)](https://fpc.msedge.net/apc/trans.gif)barındırdığını varsayar. Bir nesne yolu açıkça belirtilmemişse, Internet `/apc/trans.gif` Analyzer varsayılan olarak nesne yolu olarak kullanır ve bu da önceden yapılandırılmış uç noktaların tek piksellik görüntüyü barındırdığı yerdir. Ayrıca şema (https/http) belirtilmesi gerekmez unutmayın; Internet Analyzer yalnızca HTTPS uç noktalarını destekler, bu nedenle HTTPS kabul edilir.

11. Yeni test Internet Analyzer profili altında görünmelidir:
    ```azurecli-interactive
    az internet-analyzer test list --resource-group "MyInternetAnalyzerResourceGroup" --profile-name "MyInternetAnalyzerProfile"
    ```

    Örnek çıktı:
    ````
    [
        {
            "description": null,
            "enabledState": "Enabled",
            "endpointA": {
            "endpoint": "www.contoso.com/some/path/to/1k.jpg",
            "name": "contoso"
            },
            "endpointB": {
            "endpoint": "www.microsoft.com/another/path/to/1k.jpg",
            "name": "microsoft"
            },
            "id": "/subscriptions/faa9ddd0-9137-4659-99b7-cdc55a953342/resourcegroups/MyInternetAnalyzerResourceGroup/providers/Microsoft.Network/networkexperimentprofiles/MyInternetAnalyzerProfile/experiments/MyFirstInternetAnalyzerTest",
            "location": null,
            "name": "MyFirstInternetAnalyzerTest",
            "resourceGroup": "MyInternetAnalyzerResourceGroup",
            "resourceState": "Enabled",
            "scriptFileUri": "https://fpc.msedge.net/client/v2/d8c6fc64238d464c882cee4a310898b2/ab.min.js",
            "status": "Created",
            "tags": null,
            "type": "Microsoft.Network/networkexperimentprofiles/experiments"
        }
    ]
    ````

12. Ölçüm oluşturmaya başlamak için, testin **komut dosyasıFileUri** tarafından işaret edilen JavaScript dosyasının Web uygulamanıza gömülmüş olması gerekir. Embed [Internet Analyzer Client](internet-analyzer-embed-client.md) sayfasında özel talimatlar bulunabilir.

13. "Durum" değerini izleyerek testin ilerlemesini izleyebilirsiniz:
    ```azurecli-interactive
    az internet-analyzer test show --resource-group "MyInternetAnalyzerResourceGroup" --profile-name "MyInternetAnalyzerProfile" --name "MyFirstInternetAnalyzerTest"
    ```

14. Testin toplanan sonuçlarını, bunun için zaman serileri veya karneler oluşturarak inceleyebilirsiniz:
    ```azurecli-interactive
    az internet-analyzer show-scorecard --resource-group "MyInternetAnalyzerResourceGroup" --profile-name "MyInternetAnalyzerProfile" --name "MyFirstInternetAnalyzerTest" --aggregation-interval "Daily" --end-date-time-utc "2019-10-24T00:00:00"
    ```

    ```azurecli-interactive
    az internet-analyzer show-timeseries --resource-group "MyInternetAnalyzerResourceGroup" --profile-name "MyInternetAnalyzerProfile" --name "MyFirstInternetAnalyzerTest" --aggregation-interval "Hourly" --start-date-time-utc "2019-10-23T00:00:00" --end-date-time-utc "2019-10-24T00:00:00" --timeseries-type MeasurementCounts
    ```


## <a name="next-steps"></a>Sonraki adımlar

* Desteklenen komutların ve kullanım örneklerinin tam listesi için [Internet Analyzer CLI başvurusuna](https://docs.microsoft.com/cli/azure/ext/internet-analyzer/internet-analyzer?view=azure-cli-latest) göz atın.
* Internet [Analyzer SSS](internet-analyzer-faq.md)okuyun.
* [Internet Analyzer İstemci'sini](internet-analyzer-embed-client.md) katıştırma ve özel bir [bitiş noktası](internet-analyzer-custom-endpoint.md)oluşturma hakkında daha fazla bilgi edinin. 
