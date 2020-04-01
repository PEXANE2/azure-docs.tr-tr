---
title: Arka uç sağlık ve tanı günlükleri
titleSuffix: Azure Application Gateway
description: Azure Uygulama Ağ Geçidi için erişim günlüklerini ve performans günlüklerini nasıl etkinleştirmenizi ve yöneteceklerini öğrenin
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 11/22/2019
ms.author: victorh
ms.openlocfilehash: c7b38ad40977e1042032210d3a82a73ff6169adc
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80411051"
---
# <a name="back-end-health-and-diagnostic-logs-for-application-gateway"></a>Uygulama Ağ Geçidi için arka uç sağlık ve tanıgünlükleri

Azure Uygulama Ağ Geçidi kaynaklarını aşağıdaki yollarla izleyebilirsiniz:

* [Arka uç durumu](#back-end-health): Application Gateway, Arka uç havuzlarında bulunan sunucuların sistem durumunu Azure portalı ve PowerShell üzerinden izleme olanağı sağlar. Ayrıca performans tanı günlükleri aracılığıyla arka uç havuzlarının durumunu bulabilirsiniz.

* [Günlükler](#diagnostic-logging): Günlükler, performans, erişim ve diğer verilerin izleme amacıyla bir kaynaktan kaydedilmesine veya tüketilmesine olanak sağlar.

* [Ölçümler](application-gateway-metrics.md): Uygulama Ağ Geçidi, sisteminizin beklendiği gibi performans gösterdiğini doğrulamanıza yardımcı olan birkaç ölçüme sahiptir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="back-end-health"></a>Arka uç sağlık

Uygulama Ağ Geçidi, arka uç havuzlarının tek tek üyelerinin sistem durumunu portal, PowerShell ve komut satırı arabirimi (CLI) aracılığıyla izleme olanağı sağlar. Ayrıca, performans tanılama günlükleri aracılığıyla arka uç havuzlarının toplu bir sistem durumu özetini de bulabilirsiniz.

Arka uç sağlık raporu, Uygulama Ağ Geçidi sistem sondasının çıktısını arka uç örneklerine yansıtır. Sondalama başarılı olduğunda ve arka uç trafiği alabildiği zaman, sağlıklı kabul edilir. Aksi takdirde, sağlıksız olarak kabul edilir.

> [!IMPORTANT]
> Bir Uygulama Ağ Geçidi alt ağında bir ağ güvenlik grubu (NSG) varsa, gelen trafik için Uygulama Ağ Geçidi alt ağındav2 SKUs için 65503-65534 ve 65200-65535 arasında değişir. Bu bağlantı noktası aralığı Azure altyapı iletişimi için gereklidir. Bunlar Azure sertifikaları tarafından korunur (kilitlenir). Uygun sertifikalar olmadan, bu ağ geçitlerinin müşterileri de dahil olmak üzere harici varlıklar, bu uç noktalarda herhangi bir değişiklik başlatamaz.


### <a name="view-back-end-health-through-the-portal"></a>Arka uç sağlığını portal dan görüntüleme

Portalda, arka uç sağlık otomatik olarak sağlanır. Varolan bir uygulama ağ geçidinde, Arka**Uç durumunu** **Izleme'yi** > seçin.

Arka uç havuzundaki her üye bu sayfada listelenir (NIC, IP veya FQDN olsun). Arka uç havuz adı, bağlantı noktası, arka uç HTTP ayarları adı ve sistem durumu gösterilir. Sağlık durumu için geçerli değerler **Sağlıklı,** **Sağlıksız**ve **Bilinmiyor.**

> [!NOTE]
> **Bilinmeyenin**arka uç durumu görürseniz, arka uçerişiminin nsg kuralı, kullanıcı tanımlı bir rota (UDR) veya sanal ağdaki özel bir DNS tarafından engellenmediğinden emin olun.

![Arka uç sağlık][10]

### <a name="view-back-end-health-through-powershell"></a>PowerShell ile arka uç sağlığını görüntüleyin

Aşağıdaki PowerShell `Get-AzApplicationGatewayBackendHealth` kodu, cmdlet kullanarak arka uç durumunun nasıl görüntülenebildiğini gösterir:

```powershell
Get-AzApplicationGatewayBackendHealth -Name ApplicationGateway1 -ResourceGroupName Contoso
```

### <a name="view-back-end-health-through-azure-cli"></a>Azure CLI ile arka uç durumunu görüntüleme

```azurecli
az network application-gateway show-backend-health --resource-group AdatumAppGatewayRG --name AdatumAppGateway
```

### <a name="results"></a>Sonuçlar

Aşağıdaki parçacık yanıtın bir örneğini gösterir:

```json
{
"BackendAddressPool": {
    "Id": "/subscriptions/00000000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/appGatewayBackendPool"
},
"BackendHttpSettingsCollection": [
    {
    "BackendHttpSettings": {
        "Id": "/00000000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings"
    },
    "Servers": [
        {
        "Address": "hostname.westus.cloudapp.azure.com",
        "Health": "Healthy"
        },
        {
        "Address": "hostname.westus.cloudapp.azure.com",
        "Health": "Healthy"
        }
    ]
    }
]
}
```

## <a name="diagnostic-logs"></a><a name="diagnostic-logging"></a>Tanılama günlükleri

Uygulama ağ geçitlerini yönetmek ve sorun gidermek için Azure'da farklı türde günlükler kullanabilirsiniz. Bu günlüklerden bazılarına portaldan erişebilirsiniz. Tüm günlükler Azure Blob depolama alanından ayıklanabilir ve [Azure Monitor günlükleri,](../azure-monitor/insights/azure-networking-analytics.md)Excel ve Power BI gibi farklı araçlarda görüntülenebilir. Aşağıdaki listeden farklı günlük türleri hakkında daha fazla bilgi edinebilirsiniz:

* **Etkinlik günlüğü**: Azure aboneliğinize gönderilen tüm işlemleri ve durumlarını görüntülemek için [Azure etkinlik günlüklerini](../monitoring-and-diagnostics/insights-debugging-with-events.md) (eski adıyla operasyonel günlükler ve denetim günlükleri olarak bilinir) kullanabilirsiniz. Etkinlik günlüğü girişleri varsayılan olarak toplanır ve bunları Azure portalda görüntüleyebilirsiniz.
* **Erişim günlüğü**: Uygulama Ağ Geçidi erişim desenlerini görüntülemek ve önemli bilgileri analiz etmek için bu günlüğü kullanabilirsiniz. Bu, arayanın IP'sini, istenen URL'sini, yanıt gecikmesini, iade kodunu ve baytları girip çıkar. Her 60 saniyede bir erişim günlüğü toplanır. Bu günlük, Uygulama Ağ Geçidi örneği başına bir kayıt içerir. Uygulama Ağ Geçidi örneği instanceId özelliği tarafından tanımlanır.
* **Performans günlüğü**: Bu günlüğü, Uygulama Ağ Geçidi örneklerinin nasıl performans gösterdiğini görüntülemek için kullanabilirsiniz. Bu günlük, sunulan toplam istekler, baytlar, sunulan toplam istekler, başarısız istek sayısı ve sağlıklı ve sağlıksız arka uç örnek sayısı dahil olmak üzere her örnek için performans bilgilerini yakalar. Performans günlüğü her 60 saniyede bir toplanır. Performans günlüğü yalnızca v1 SKU için kullanılabilir. v2 SKU için performans verileri için [Ölçümler'i](application-gateway-metrics.md) kullanın.
* **Güvenlik Duvarı günlüğü**: Web uygulaması güvenlik duvarıyla yapılandırılan bir uygulama ağ geçidinin algılama veya önleme modu üzerinden günlüğe kaydedilen istekleri görüntülemek için bu günlüğü kullanabilirsiniz. Güvenlik duvarı günlükleri her 60 saniyede bir toplanır. 

> [!NOTE]
> Günlükler yalnızca Azure Kaynak Yöneticisi dağıtım modelinde dağıtılan kaynaklar için kullanılabilir. Klasik dağıtım modelinde kaynaklar için günlük kullanamazsınız. İki modelin daha iyi anlaşılması [için, Kaynak Yöneticisi'ni Anlama dağıtımı ve klasik dağıtım](../azure-resource-manager/management/deployment-models.md) makalesine bakın.

Günlüklerinizi depolamak için kullanabileceğiniz üç seçenek vardır:

* **Depolama hesabı**: Depolama hesaplarının en iyi kullanım amacı, günlüklerin uzun süre depolanması ve ihtiyaç duyulduğunda gözden geçirilmesi durumlarıdır.
* **Etkinlik hub'ları**: Etkinlik hub'ları, kaynaklarınız hakkında uyarı almak için diğer güvenlik bilgileri ve olay yönetimi (SIEM) araçlarıyla tümleştirme yapmak için harika bir seçenektir.
* **Azure Monitor günlükleri**: Azure Monitor günlükleri en iyi uygulamanızın genel gerçek zamanlı izlenmesi veya eğilimlere bakilmesi için kullanılır.

### <a name="enable-logging-through-powershell"></a>PowerShell'de günlüğe kaydetmeyi etkinleştirme

Etkinlik günlüğü tüm Kaynak Yöneticisi kaynakları için otomatik olarak etkinleştirilir. Bu günlükler aracılığıyla kullanılabilir verileri toplamaya başlamak için erişim ve performans günlüğe kaydetmeyi etkinleştirmeniz gerekir. Günlüğe kaydetmeyi etkinleştirmek için aşağıdaki adımları kullanın:

1. Günlük verilerinin depolandığı depolama hesabınızın kaynak kimliğini not edin. Bu değer şu biçimdedir: /subscriptions/\<abonelik kimliği\>/resourceGroups/\<kaynak grubu adı\>/providers/Microsoft.Storage/storageAccounts/\<depolama hesabı adı\>. Aboneliğinizdeki herhangi bir depolama hesabını kullanabilirsiniz. Bu bilgileri Azure portalda bulabilirsiniz.

    ![Portal: depolama hesabı için kaynak kimliği](./media/application-gateway-diagnostics/diagnostics1.png)

2. Uygulama ağ geçidinizin günlük günlüğüetkinleştirdiği kaynak kimliğine dikkat edin. Bu değer formun:\</subscriptions/\>\<subscriptionId /resourceGroups/\>kaynak grup adı /providers/Microsoft.Network/applicationGateways/\<application gateway name\>. Bu bilgileri portalda bulabilirsiniz.

    ![Portal: uygulama ağ geçidi için kaynak kimliği](./media/application-gateway-diagnostics/diagnostics2.png)

3. Tanılama günlüğüne kaydetmeyi etkinleştirmek için aşağıdaki PowerShell cmdlet'ini kullanın:

    ```powershell
    Set-AzDiagnosticSetting  -ResourceId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name> -StorageAccountId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Storage/storageAccounts/<storage account name> -Enabled $true     
    ```

> [!TIP]
>Etkinlik günlükleri ayrı bir depolama hesabı gerektirmez. Depolama alanının erişim ve performans günlüğü kaydı için kullanılması durumunda hizmet ücreti tahsil edilir.

### <a name="enable-logging-through-the-azure-portal"></a>Azure portaldan günlüğe kaydetmeyi etkinleştirme

1. Azure portalında kaynağınızı bulun ve **Tanılama ayarlarını**seçin.

   Uygulama Ağ Geçidi için üç günlük kullanılabilir:

   * Erişim günlüğü
   * Performans günlüğü
   * Güvenlik duvarı günlüğü

2. Veri toplamaya başlamak için **tanılamayı Aç'ı**seçin.

   ![Tanılamayı açma][1]

3. **Tanılama ayarları** sayfasında tanılama günlükleriyle ilgili ayarlar bulunur. Bu örnekte, Log Analytics günlükleri depolar. Tanılama günlüklerini kaydetmek için Event Hubs'ı veya depolama hesabını da kullanabilirsiniz.

   ![Yapılandırma işlemini başlatma][2]

5. Ayarlar için bir ad yazın, ayarları onaylayın ve **Kaydet'i**seçin.

### <a name="activity-log"></a>Etkinlik günlüğü

Azure varsayılan olarak etkinlik günlüğünü oluşturur. Günlükler Azure etkinlik günlükleri deposunda 90 gün boyunca saklanır. [Olayları ve etkinlik günlüğü](../monitoring-and-diagnostics/insights-debugging-with-events.md) makalesini görüntüle'yi okuyarak bu günlükler hakkında daha fazla bilgi edinin.

### <a name="access-log"></a>Erişim günlüğü

Erişim günlüğü, yalnızca önceki adımlarda açıklandığı gibi, her Uygulama Ağ Geçidi örneğinde etkinleştirdiyseniz oluşturulur. Veriler, günlüğe kaydetmeyi etkinleştirdiğinizde belirttiğiniz depolama hesabında depolanır. Uygulama Ağ Geçidi'nin her erişimi, v1 için aşağıdaki örnekte gösterildiği gibi JSON biçiminde günlüğe kaydedilir:

|Değer  |Açıklama  |
|---------|---------|
|instanceId     | İsteğe hizmet eden Uygulama Ağ Geçidi örneği.        |
|müşteriIP     | İstek için IP'den kaynaklanmaktadır.        |
|clientPort     | İstek için kaynak bağlantı noktası.       |
|httpYöntem     | İstek tarafından kullanılan HTTP yöntemi.       |
|Requesturi     | Uri alınan istek.        |
|İstekSorgusu     | **Server-Routed**: İstek gönderilen arka uç havuz örneği.</br>**X-AzureApplicationGateway-LOG-ID**: İstek için kullanılan Korelasyon Kimliği. Arka uç sunucularında trafik sorunlarını gidermek için kullanılabilir. </br>**SERVER-STATUS**: Application Gateway'in arka uçtan aldığı HTTP yanıt kodu.       |
|Useragent     | HTTP istek üstbilgisinden kullanıcı aracısı.        |
|httpDurum     | HTTP durum kodu Uygulama Ağ Geçidi'nden istemciye döndürülür.       |
|httpSürüm     | İsteğin HTTP sürümü.        |
|alınan Baytlar     | Alınan paketin boyutu, baytlar halinde.        |
|gönderen Bayt| Gönderilen paketin boyutu, baytlar halinde.|
|zaman Alınmıştır| Bir isteğin işlenmesi ve yanıtın gönderilmesi için gereken süre (milisaniye cinsinden). Bu, Uygulama Ağ Geçidi'nin bir HTTP isteğinin ilk baytını aldığı andan yanıt gönderme işleminin bittiği zamana kadar olan aralık olarak hesaplanır. Zaman Alınan alanın genellikle istek ve yanıt paketlerinin ağ üzerinden seyahat ettiği zamanı içerdiğini unutmayın. |
|sslEtkin| Arka uç havuzlarına iletişimin SSL kullanıp kullanmadığı. Geçerli değerler avesi ve kapalıdır.|
|konak| İsteğin arka uç sunucusuna gönderildiği ana bilgisayar adı. Arka uç ana bilgisayar adı geçersiz kılınıyorsa, bu ad bunu yansıtır.|
|originalHost| İsteğin istemciden Uygulama Ağ Geçidi tarafından alındığı ana bilgisayar adı.|
```json
{
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/PEERINGTEST/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/{applicationGatewayName}",
    "operationName": "ApplicationGatewayAccess",
    "time": "2017-04-26T19:27:38Z",
    "category": "ApplicationGatewayAccessLog",
    "properties": {
        "instanceId": "ApplicationGatewayRole_IN_0",
        "clientIP": "191.96.249.97",
        "clientPort": 46886,
        "httpMethod": "GET",
        "requestUri": "/phpmyadmin/scripts/setup.php",
        "requestQuery": "X-AzureApplicationGateway-CACHE-HIT=0&SERVER-ROUTED=10.4.0.4&X-AzureApplicationGateway-LOG-ID=874f1f0f-6807-41c9-b7bc-f3cfa74aa0b1&SERVER-STATUS=404",
        "userAgent": "-",
        "httpStatus": 404,
        "httpVersion": "HTTP/1.0",
        "receivedBytes": 65,
        "sentBytes": 553,
        "timeTaken": 205,
        "sslEnabled": "off",
        "host": "www.contoso.com",
        "originalHost": "www.contoso.com"
    }
}
```
Uygulama Ağ Geçidi ve WAF v2 için günlükleri biraz daha fazla bilgi gösterir:

|Değer  |Açıklama  |
|---------|---------|
|instanceId     | İsteğe hizmet eden Uygulama Ağ Geçidi örneği.        |
|müşteriIP     | İstek için IP'den kaynaklanmaktadır.        |
|clientPort     | İstek için kaynak bağlantı noktası.       |
|httpYöntem     | İstek tarafından kullanılan HTTP yöntemi.       |
|Requesturi     | Uri alınan istek.        |
|Useragent     | HTTP istek üstbilgisinden kullanıcı aracısı.        |
|httpDurum     | HTTP durum kodu Uygulama Ağ Geçidi'nden istemciye döndürülür.       |
|httpSürüm     | İsteğin HTTP sürümü.        |
|alınan Baytlar     | Alınan paketin boyutu, baytlar halinde.        |
|gönderen Bayt| Gönderilen paketin boyutu, baytlar halinde.|
|zaman Alınmıştır| Bir isteğin işlenmesi ve yanıtın gönderilmesi için gereken süre **(saniye**cinsinden). Bu, Uygulama Ağ Geçidi'nin bir HTTP isteğinin ilk baytını aldığı andan yanıt gönderme işleminin bittiği zamana kadar olan aralık olarak hesaplanır. Zaman Alınan alanın genellikle istek ve yanıt paketlerinin ağ üzerinden seyahat ettiği zamanı içerdiğini unutmayın. |
|sslEtkin| Arka uç havuzlarına iletişimin SSL kullanıp kullanmadığı. Geçerli değerler avesi ve kapalıdır.|
|sslŞifre| SSL iletişimi için kullanılan şifreleme paketi (SSL etkinse).|
|sslProtokol| SSL/TLS protokolü kullanılıyor (SSL etkinse).|
|serverRouted| Uygulama ağ geçidinin isteği yollayan arka uç sunucusu.|
|Serverstatus| Arka uç sunucusunun HTTP durum kodu.|
|serverResponseLatency| Yanıtın arka uç sunucusundan gecikmesi.|
|konak| İsteğin ana bilgisayar üstbilgisinde listelenen adres.|
```json
{
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/PEERINGTEST/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/{applicationGatewayName}",
    "operationName": "ApplicationGatewayAccess",
    "time": "2017-04-26T19:27:38Z",
    "category": "ApplicationGatewayAccessLog",
    "properties": {
        "instanceId": "appgw_1",
        "clientIP": "191.96.249.97",
        "clientPort": 46886,
        "httpMethod": "GET",
        "requestUri": "/phpmyadmin/scripts/setup.php",
        "userAgent": "-",
        "httpStatus": 404,
        "httpVersion": "HTTP/1.0",
        "receivedBytes": 65,
        "sentBytes": 553,
        "timeTaken": 205,
        "sslEnabled": "off",
        "sslCipher": "",
        "sslProtocol": "",
        "serverRouted": "104.41.114.59:80",
        "serverStatus": "200",
        "serverResponseLatency": "0.023",
        "host": "www.contoso.com",
    }
}
```

### <a name="performance-log"></a>Performans günlüğü

Performans günlüğü, yalnızca önceki adımlarda açıklandığı gibi, her Uygulama Ağ Geçidi örneğinde etkinleştirdiyseniz oluşturulur. Veriler, günlüğe kaydetmeyi etkinleştirdiğinizde belirttiğiniz depolama hesabında depolanır. Performans günlüğü verileri 1 dakikalık aralıklarla oluşturulur. Sadece v1 SKU için kullanılabilir. v2 SKU için performans verileri için [Ölçümler'i](application-gateway-metrics.md) kullanın. Aşağıdaki veriler günlüğe kaydedilir:


|Değer  |Açıklama  |
|---------|---------|
|instanceId     |  Performans verilerinin oluşturulduğu Uygulama Ağ Geçidi örneği. Birden çok örnekli uygulama ağ geçidi için, her örnek için bir satır vardır.        |
|sağlıklıHostCount     | Arka uç havuzundaki sağlıklı ana bilgisayar sayısı.        |
|unHealthyHostCount     | Arka uç havuzundaki sağlıksız ana bilgisayar sayısı.        |
|requestCount     | Sunulan istek sayısı.        |
|Gecikme | İsteklere hizmet veren örnekten arka uca gelen isteklerin ortalama gecikmesi (milisaniye cinsinden). |
|failedRequestCount| Başarısız istek sayısı.|
|aktarım hızı| Saniyebaşına bayt cinsinden ölçülen son günlükten bu yana ortalama iş miktarı.|

```json
{
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/{applicationGatewayName}",
    "operationName": "ApplicationGatewayPerformance",
    "time": "2016-04-09T00:00:00Z",
    "category": "ApplicationGatewayPerformanceLog",
    "properties":
    {
        "instanceId":"ApplicationGatewayRole_IN_1",
        "healthyHostCount":"4",
        "unHealthyHostCount":"0",
        "requestCount":"185",
        "latency":"0",
        "failedRequestCount":"0",
        "throughput":"119427"
    }
}
```

> [!NOTE]
> Gecikme süresi, HTTP isteğinin ilk baytının alındığı andan, HTTP yanıtının son baytının gönderildiği zamana kadar hesaplanır. Bu, Uygulama Ağ Geçidi işlem süresinin toplamı artı arka uçtaki ağ maliyetinin yanı sıra isteği işlemek için arka uç için gereken süredir.

### <a name="firewall-log"></a>Güvenlik duvarı günlüğü

Güvenlik duvarı günlüğü, yalnızca önceki adımlarda açıklandığı gibi, her uygulama ağ geçidi için etkinleştirdiyseniz oluşturulur. Bu günlük, web uygulaması güvenlik duvarının bir uygulama ağ geçidinde yapılandırılmasını da gerektirir. Veriler, günlüğe kaydetmeyi etkinleştirdiğinizde belirttiğiniz depolama hesabında depolanır. Aşağıdaki veriler günlüğe kaydedilir:


|Değer  |Açıklama  |
|---------|---------|
|instanceId     | Güvenlik duvarı verilerinin oluşturulduğu Uygulama Ağ Geçidi örneği. Birden çok örnekli uygulama ağ geçidi için, her örnek için bir satır vardır.         |
|clientIp     |   İstek için IP'den kaynaklanmaktadır.      |
|clientPort     |  İstek için kaynak bağlantı noktası.       |
|Requesturi     | Alınan isteğin URL'si.       |
|ruleSetType     | Kural kümesi türü. Kullanılabilir değer OWASP'dir.        |
|ruleSetVersion     | Kural kümesi sürümü kullanılır. Kullanılabilir değerler 2.2.9 ve 3.0'dır.     |
|ruleId     | Tetikleyici olayın kural kimliği.        |
|message     | Tetikleyici olay için kullanıcı dostu ileti. Ayrıntılar bölümünde daha fazla bilgi verilmiştir.        |
|action     |  İstek üzerine alınan işlem. Kullanılabilir değerler Eşlenir ve Engellenir.      |
|Site     | Günlüğün oluşturulduğu site. Kurallar genel olduğundan şu anda yalnızca Global listelenmiştir.|
|Şey     | Tetikleyici olayın ayrıntıları.        |
|ayrıntılar.message     | Kuralın açıklaması.        |
|ayrıntılar.data     | İsteğde bulunan ve kuralla eşleşen belirli veriler.         |
|ayrıntılar.file     | Kuralı içeren yapılandırma dosyası.        |
|ayrıntılar.line     | Olayı tetikleyen yapılandırma dosyasındaki satır numarası.       |
|Hostname   | Uygulama Ağ Geçidi'nin ana bilgisayar adı veya IP adresi.    |
|Transactionıd  | Aynı istek içinde gerçekleşen birden çok kural ihlalini gruplandırmaya yardımcı olan belirli bir işlem için benzersiz kimlik.   |

```json
{
  "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/{applicationGatewayName}",
  "operationName": "ApplicationGatewayFirewall",
  "time": "2017-03-20T15:52:09.1494499Z",
  "category": "ApplicationGatewayFirewallLog",
  "properties": {
    "instanceId": "ApplicationGatewayRole_IN_0",
    "clientIp": "104.210.252.3",
    "clientPort": "4835",
    "requestUri": "/?a=%3Cscript%3Ealert(%22Hello%22);%3C/script%3E",
    "ruleSetType": "OWASP",
    "ruleSetVersion": "3.0",
    "ruleId": "941320",
    "message": "Possible XSS Attack Detected - HTML Tag Handler",
    "action": "Blocked",
    "site": "Global",
    "details": {
      "message": "Warning. Pattern match \"<(a|abbr|acronym|address|applet|area|audioscope|b|base|basefront|bdo|bgsound|big|blackface|blink|blockquote|body|bq|br|button|caption|center|cite|code|col|colgroup|comment|dd|del|dfn|dir|div|dl|dt|em|embed|fieldset|fn|font|form|frame|frameset|h1|head|h ...\" at ARGS:a.",
      "data": "Matched Data: <script> found within ARGS:a: <script>alert(\\x22hello\\x22);</script>",
      "file": "rules/REQUEST-941-APPLICATION-ATTACK-XSS.conf",
      "line": "865"
    }
    "hostname": "40.90.218.100",
    "transactionId": "AYAcUqAcAcAcAcAcASAcAcAc"
  }
}

```

### <a name="view-and-analyze-the-activity-log"></a>Etkinlik günlüğünü görüntüleme ve analiz etme

Aşağıdaki yöntemlerden birini kullanarak etkinlik günlüğü verilerini görüntüleyebilir ve analiz edebilirsiniz:

* **Azure araçları**: Etkinlik günlüğü verilerini Azure PowerShell, Azure CLI, Azure REST API veya Azure portal üzerinden alabilirsiniz. Her yöntemle ilgili ayrıntılı adımlar [Kaynak Yöneticisi etkinlik işlemleri](../azure-resource-manager/management/view-activity-logs.md) makalesinde ayrıntılı bir şekilde anlatılmıştır.
* **Power BI**: [Power BI](https://powerbi.microsoft.com/pricing) hesabınız yoksa ücretsiz oluşturabilirsiniz. [Power BI şablon uygulamalarını](https://docs.microsoft.com/power-bi/service-template-apps-overview)kullanarak verilerinizi analiz edebilirsiniz.

### <a name="view-and-analyze-the-access-performance-and-firewall-logs"></a>Erişim, performans ve güvenlik duvarı günlüklerini görüntüleme ve çözümleme

[Azure Monitor günlükleri,](../azure-monitor/insights/azure-networking-analytics.md) sayaç ve olay günlüğü dosyalarını Blob depolama hesabınızdan toplayabilir. Günlüklerinizi analiz etmek için görselleştirmelere ve güçlü arama özelliklerine sahiptir.

Dilerseniz depolama hesabınıza bağlanabilir ve JSON erişim günlüklerini ve performans günlüklerini alabilirsiniz. İndirdiğiniz JSON dosyalarını CSV biçimine dönüştürebilir ve Excel, Power BI veya diğer veri görselleştirme araçlarında görüntüleyebilirsiniz.

> [!TIP]
> Visual Studio ve C# ile sabit ve değişken değerlerini değiştirme konusunda temel kavramlara hakimseniz GitHub'daki [günlük dönüştürücü araçlarını](https://github.com/Azure-Samples/networking-dotnet-log-converter) kullanabilirsiniz.
>
>

#### <a name="analyzing-access-logs-through-goaccess"></a>GoAccess üzerinden Erişim günlüklerini çözümleme

Uygulama Ağ Geçidi Erişim Günlükleri için popüler [GoAccess](https://goaccess.io/) günlük çözümleyicisini yükleyen ve çalıştıran bir Kaynak Yöneticisi şablonu yayınladık. GoAccess, Benzersiz Ziyaretçiler, İstenen Dosyalar, Ana Bilgisayarlar, İşletim Sistemleri, Tarayıcılar, HTTP Durum kodları ve daha fazlası gibi değerli HTTP trafik istatistikleri sağlar. Daha fazla bilgi için lütfen [GitHub'daki Kaynak Yöneticisi şablon klasöründeki Readme dosyasına](https://aka.ms/appgwgoaccessreadme)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Monitor günlüklerini](../azure-monitor/insights/azure-networking-analytics.md)kullanarak sayaç ve olay günlüklerini görselleştirin.
* Power BI blog [gönderisiyle Azure etkinlik günlüğünüünüzü görselleştirin.](https://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx)
* Power BI ve daha fazla blog [gönderisinde Azure etkinlik günlüklerini görüntüleyin ve analiz edin.](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/)

[1]: ./media/application-gateway-diagnostics/figure1.png
[2]: ./media/application-gateway-diagnostics/figure2.png
[3]: ./media/application-gateway-diagnostics/figure3.png
[4]: ./media/application-gateway-diagnostics/figure4.png
[5]: ./media/application-gateway-diagnostics/figure5.png
[6]: ./media/application-gateway-diagnostics/figure6.png
[7]: ./media/application-gateway-diagnostics/figure7.png
[8]: ./media/application-gateway-diagnostics/figure8.png
[9]: ./media/application-gateway-diagnostics/figure9.png
[10]: ./media/application-gateway-diagnostics/figure10.png
