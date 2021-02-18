---
title: Azure CDN için izleme, ölçümler ve ham Günlükler
description: Bu makalede, Azure CDN izleme, ölçümler ve ham günlüklerin nasıl ayarlanacağı ve kullanılacağı açıklanmaktadır.
services: cdn
author: asudbring
manager: KumudD
ms.service: azure-cdn
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 11/23/2020
ms.author: yuajia
ms.openlocfilehash: 8a4b48586c564ee5d14a0768156b0477e2935ccf
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100575456"
---
# <a name="real-time-monitoring-metrics-and-access-logs-for-azure-cdn"></a>Azure CDN için gerçek zamanlı Izleme, ölçümler ve erişim günlükleri
Microsoft 'tan Azure CDN, sorunları gidermenize, izlemenize ve hata ayıklamanıza yardımcı olmak için aşağıdaki yollarla kaynakları izleyebilirsiniz. 

* Ham Günlükler, CDN 'nin aldığı her istek hakkında zengin bilgiler sağlar. Ham Günlükler etkinlik günlüklerinden farklıdır. Etkinlik günlükleri, Azure kaynakları üzerinde gerçekleştirilen işlemlere görünürlük sağlar.
* Bayt Isabet oranı, Istek sayısı, yanıt boyutu ve toplam gecikme süresi dahil olmak üzere CDN üzerinde dört temel ölçüm görüntüleyen ölçümler. Ayrıca ölçümleri bölmek için farklı boyutlar sağlar.
* Uyarı, müşterinin temel ölçümler için uyarı ayarlamasına olanak tanır
* Müşterilerin ek ölçümler sağlamak için Azure Log Analytics kullanmasına izin veren ek ölçümler. Ayrıca, Azure Log Analytics altında birkaç başka ölçüm için de sorgu örnekleri sağlıyoruz.

> [!IMPORTANT]
> HTTP ham günlükleri özelliği, Microsoft 'tan Azure CDN için kullanılabilir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="configuration---azure-portal"></a>Yapılandırma-Azure portal

Microsoft profilinden Azure CDN ham günlüklerini yapılandırmak için: 

1. Azure portal menüsünde **tüm kaynaklar**' ı seçin  >>  **\<your-CDN-profile>** .

2. **İzleme** bölümünde **Tanılama ayarları**’nı seçin.

3. **+ Tanılama ayarı Ekle**' yi seçin.

    :::image type="content" source="./media/cdn-raw-logs/raw-logs-01.png" alt-text="CDN profili için tanılama ayarı ekleyin." border="true":::
    
    > [!IMPORTANT]
    > Ham Günlükler yalnızca profil düzeyinde kullanılabilir, ancak toplu http durum kodu günlükleri uç nokta düzeyinde kullanılabilir.

4. **Tanılama ayarları**' nın altında, tanılama **ayarları adı** altında tanılama ayarı için bir ad girin.

5. **Azurecdnaccesslog** öğesini seçin ve bekletme süresini gün olarak ayarlayın.

6. **Hedef ayrıntılarını** seçin. Hedef seçenekleri şunlardır:
    * **Log Analytics’e gönderme**
        * **Aboneliği** ve **Log Analytics çalışma alanını** seçin.
    * **Bir depolama hesabına Arşivle**
        * **Aboneliği** ve **Depolama hesabını** seçin.
    * **Olay hub’ına akış yapma**
        * **Abonelik**, **Olay Hub 'ı ad alanı**, **Olay Hub 'ı adı (isteğe bağlı)** ve **Olay Hub 'ı ilke adı**' nı seçin.

    :::image type="content" source="./media/cdn-raw-logs/raw-logs-02.png" alt-text="Günlük ayarları için hedefi yapılandırın." border="true":::

7. **Kaydet**’i seçin.

## <a name="configuration---azure-powershell"></a>Yapılandırma-Azure PowerShell

Ham Günlükler için tanılama ayarını yapılandırmak üzere [set-AzDiagnosticSetting](/powershell/module/az.monitor/set-azdiagnosticsetting) ' i kullanın.

Bekletme verileri, komutunda **-retentionındays** seçeneği tarafından tanımlanır.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

### <a name="enable-diagnostic-logs-in-a-storage-account"></a>Depolama hesabında tanılama günlüklerini etkinleştirme

1. Azure PowerShell oturum açın:

    ```azurepowershell-interactive
    Connect-AzAccount 
    ```

2. Bir depolama hesabında tanılama günlüklerini etkinleştirmek için, bu komutları girin. Değişkenleri değerlerinizle değiştirin:

    ```azurepowershell-interactive
    ## Variables for the commands ##
    $rsg = <your-resource-group-name>
    $cdnprofile = <your-cdn-profile-name>
    $cdnendpoint = <your-cdn-endpoint-name>
    $storageacct = <your-storage-account-name>
    $diagname = <your-diagnostic-setting-name>
    $days = '30'

    $cdn = Get-AzCdnEndpoint -ResourceGroupName $rsg -ProfileName $cdnprofile -EndpointName $cdnendpoint

    $storage = Get-AzStorageAccount -ResourceGroupName $rsg -Name $storageacct

    Set-AzDiagnosticSetting -Name $diagname -ResourceId $cdn.id -StorageAccountId $storage.id -Enabled $true -Category AzureCdnAccessLog -RetentionEnabled 1 -RetentionInDays $days
    ```

### <a name="enable-diagnostics-logs-for-log-analytics-workspace"></a>Log Analytics çalışma alanı için tanılama günlüklerini etkinleştirme

1. Azure PowerShell oturum açın:

    ```azurepowershell-interactive
    Connect-AzAccount 
    ```
2. Bir Log Analytics çalışma alanı için tanılama günlüklerini etkinleştirmek üzere, bu komutları girin. Değişkenleri değerlerinizle değiştirin:

    ```azurepowershell-interactive
    ## Variables for the commands ##
    $rsg = <your-resource-group-name>
    $cdnprofile = <your-cdn-profile-name>
    $cdnendpoint = <your-cdn-endpoint-name>
    $workspacename = <your-log-analytics-workspace-name>
    $diagname = <your-diagnostic-setting-name>
    $days = '30'

    $cdn = Get-AzCdnEndpoint -ResourceGroupName $rsg -ProfileName $cdnprofile -EndpointName $cdnendpoint

    $workspace = Get-AzOperationalInsightsWorkspace -ResourceGroupName $rsg -Name $workspacename

    Set-AzDiagnosticSetting -Name $diagname -ResourceId $cdn.id -WorkspaceId $workspace.ResourceId -Enabled $true -Category AzureCdnAccessLog -RetentionEnabled 1 -RetentionInDays $days
    ```
### <a name="enable-diagnostics-logs-for-event-hub-namespace"></a>Olay Hub 'ı ad alanı için tanılama günlüklerini etkinleştirme

1. Azure PowerShell oturum açın:

    ```azurepowershell-interactive
    Connect-AzAccount 
    ```
2. Bir olay hub 'ı ad alanı için tanılama günlüklerini etkinleştirmek üzere, bu komutları girin. Değişkenleri değerlerinizle değiştirin:

    ```azurepowershell-interactive
    ## Variables for the commands ##
    $rsg = <your-resource-group-name>
    $cdnprofile = <your-cdn-profile-name>
    $cdnendpoint = <your-cdn-endpoint-name>
    $evthubnamespace = <your-event-hub-namespace-name>
    $diagname = <your-diagnostic-setting-name>

    $cdn = Get-AzCdnEndpoint -ResourceGroupName $rsg -ProfileName $cdnprofile -EndpointName $cdnendpoint

    $eventhub = Get-AzEventHubNamespace -ResourceGroupName $rsg -Name $eventhubname

    Set-AzDiagnosticSetting -Name $diagname -ResourceId $cdn.id -EventHubName $eventhub.id -Enabled $true -Category AzureCdnAccessLog -RetentionEnabled 1 -RetentionInDays $days
    ```

## <a name="raw-logs-properties"></a>Ham Günlükler özellikleri

Microsoft Service Azure CDN Şu anda ham Günlükler sağlıyor. Ham Günlükler her bir girdiyle aşağıdaki şemaya sahip API istekleri sağlar: 

| Özellik  | Açıklama |
| ------------- | ------------- |
| BackendHostname | İstek bir arka uca iletilirse Bu alan, arka ucun ana bilgisayar adını temsil eder. İstek yeniden yönlendirilirse veya bir bölgesel önbelleğe iletilirse (yönlendirme kuralı için önbelleğe alma etkinleştirildiğinde) Bu alan boş olur. |
| CacheStatus | Önbelleğe alma senaryolarında bu alan, POP 'ta önbelleğin isabet/isabetsizlik sını tanımlar |
| ClientIp | İsteği yapan istemcinin IP adresi. İstekte bir X-Iletilmiş-for üstbilgisi varsa, Istemci IP 'si aynı üzerinden çekilir. |
| Istemci bağlantı noktası | İsteği yapan istemcinin IP bağlantı noktası. |
| HttpMethod | İstek tarafından kullanılan HTTP yöntemi. |
| HttpStatusCode | Proxy 'den döndürülen HTTP durum kodu. |
| HttpStatusDetails | İstek üzerine sonuçtaki durum. Bu dize değerinin anlamı, bir durum başvuru tablosunda bulunabilir. |
| HttpVersion | İstek veya bağlantı türü. |
| POP | İsteğin bulunduğu kenarın kısa adı. |
| RequestBytes | İstek üst bilgileri ve istek gövdesi dahil olmak üzere HTTP isteği iletisinin bayt cinsinden boyutu. |
| RequestUri | Alınan isteğin URI 'SI. |
| ResponseBytes | Yanıt olarak arka uç sunucusu tarafından gönderilen bayt.  |
| RoutingRuleName | İsteğin eşleştiği yönlendirme kuralının adı. |
| RulesEngineMatchNames | İsteğin eşleştiği kuralların adları. |
| SecurityProtocol | İstek tarafından kullanılan TLS/SSL protokol sürümü veya şifreleme yoksa null. |
| SentToOriginShield </br> (kullanım dışı) * **aşağıdaki bölümde kullanımdan kaldırılması için notlara bakın.**| True ise, isteğin kenar pop 'u yerine kaynak kalkan önbelleğinden yanıtlandığını gösterir. Kaynak kalkan, önbellek isabet oranını artırmak için kullanılan bir üst önbelleğidir. |
| ıreceived Fromclient | Doğru ise, isteğin istemciden geldiği anlamına gelir. Yanlış ise, istek kenarda (alt POP) bir isabetsizlik olur ve kaynak kalkanı (üst POP) ile yanıt verilir. |
| TimeTaken | İsteğin ilk baytının, saniye cinsinden son bayta geçen bayt olan sürenin uzunluğu. |
| TrackingReference | Ön kapıya yönelik olarak sunulan ve istemciye X-Azure-ref üst bilgisi olarak gönderilen bir isteği tanımlayan benzersiz başvuru dizesi. Belirli bir istek için erişim günlüklerindeki ayrıntıları aramak için gereklidir. |
| Kullanıcı | İstemcinin kullandığı tarayıcı türü. |
| ErrorInfo | Bu alan, sorun giderme alanını daraltmak için belirli hata türünü içerir. </br> Olası değerler şunlardır: </br> **NOERROR**: herhangi bir hata bulunamadığını gösterir. </br> **Sertifikahatası**: genel SSL sertifikası hatası.</br> **Certificatenamecheckfailed**: SSL sertifikasındaki konak adı geçersiz veya eşleşmiyor. </br> **Clientconnected**: istemci ağ bağlantısı nedeniyle hata isteme. </br> **Unbelirtilmediclienterror**: genel istemci hatası. </br> **Invalidrequest**: geçersiz istek. Hatalı biçimlendirilmiş üst bilgi, gövde ve URL nedeniyle meydana gelebilir. </br> **Dnsfailure**: DNS hatası. </br> **Dnsnamenotresolved**: sunucu adı veya adresi çözümlenemedi. </br> **Originconnectiondurdurulan**: kaynak bağlantısı aniden durduruldu. </br> **Originconnectionerror**: genel kaynak bağlantı hatası. </br> **Originconnectionreddedildi**: kaynağa olan bağlantı oluşturulamadı. </br> **Originerror**: genel kaynak hatası. </br> **Originınvalidresponse**: Origin geçersiz veya tanınmayan bir yanıt döndürdü. </br> **Origintimeout**: kaynak isteği için zaman aşımı süresi doldu. </br> **Responseheadertoobıg**: kaynak bir yanıt üst bilgisi için çok büyük döndürdü. </br> **Kısıttedıp**: kısıtlanmış IP nedeniyle istek engellendi. </br> **Sslhandshakeerror**: SSL birlikte sallama hatası nedeniyle Origin ile bağlantı kurulamıyor. </br> **UnspecifiedError**: tablodaki hatalara uymayan bir hata oluştu. |
| TimeToFirstByte | Microsoft CDN 'nin istemciye ilk baytın gönderilmesi için isteği aldığı sürenin milisaniye cinsinden süresi. Saat yalnızca Microsoft tarafından ölçülür. İstemci tarafı veriler ölçülmez. |
> [!NOTE]
> Günlükler, bir sorgu çalıştırılarak Log Analytics profiliniz altında görüntülenebilir. Örnek bir sorgu şöyle görünür:
    ```
    AzureDiagnostics | where Category == "AzureCdnAccessLog"
    ```

### <a name="sent-to-origin-shield-deprecation"></a>Kaynak kalkan kullanım dışı bırakılmasıyla gönderildi
Ham günlük özelliği **ısenttooriginshield** kullanım dışıdır ve yeni bir alan tarafından alınmış bir **fromclient** tarafından değiştirildi. Zaten kullanım dışı olan alanı kullanıyorsanız, yeni alanı kullanın. 

Ham günlüklerde hem CDN Edge (alt POP) hem de kaynak kalkanı tarafından oluşturulan Günlükler bulunur. Kaynak kalkan, dünya genelinde stratejik olarak bulunan üst düğümlere başvurur. Bu düğümler, kaynak sunucularıyla iletişim kurar ve kaynak yükünü kaynak olarak azaltır. 

Kaynak Shield 'a giden her istek için 2 günlük girişi vardır:

* Kenar düğümleri için bir tane
* Kaynak kalkan için bir tane. 

Uç düğümlerinden ve kaynak kalanlardan çıkış ya da yanıtları ayırt etmek için, uygun verileri almak üzere **ıreceived Fromclient** alanını kullanabilirsiniz. 

Değer false ise, isteğin kaynak Shield 'dan kenar düğümlerine yanıt verdiği anlamına gelir. Bu yaklaşım, ham günlükleri faturalandırma verileriyle karşılaştırmak için geçerlidir. Ücret, çıkış durumundan kenar düğümlerine çıkış için tahakkuk etmemektedir. Ücretler, uç düğümlerinden istemcilere çıkış için ücretlendirilir. 

**Log Analytics kaynak kalkanı üzerinde oluşturulan günlüklerin hariç tutulmasını sağlamak için sorgu örneği kusto.**

```kusto
AzureDiagnostics 
| where OperationName == "Microsoft.Cdn/Profiles/AccessLog/Write" and Category == "AzureCdnAccessLog"  
| where isReceivedFromClient == true

```

> [!IMPORTANT]
> HTTP ham günlükleri özelliği, **25 şubat 2020**' den sonra oluşturulan veya güncellenen tüm profiller için otomatik olarak kullanılabilir. Daha önce oluşturulan CDN profilleri için, bir tane, günlüğü ayarladıktan sonra CDN uç noktasını güncelleştirmelidir. Örneğin, bir tane, CDN uç noktaları altında coğrafi filtrelemeye gidebilir ve iş yüküyle ilgili olmayan herhangi bir ülkeyi/bölgeyi engelleyebilir ve Kaydet ' i ziyaret edebilir.


## <a name="metrics"></a>Ölçümler
Microsoft 'tan Azure CDN, Azure Izleyici ile tümleşiktir ve sorunları izlemeye, gidermenize ve hata ayıklamanıza yardımcı olmak için dört CDN ölçümü yayımlar. 

Ölçümler, grafiklerde görüntülenir ve PowerShell, CLı ve API aracılığıyla erişilebilir. CDN ölçümleri ücretsizdir.

Microsoft ölçülerden Azure CDN ve ölçümlerini 60 saniyelik aralıklarla gönderir. Ölçümlerin portalda görünmesi 3 dakika kadar sürebilir. 

Daha fazla bilgi için bkz. [Azure izleyici ölçümleri](../azure-monitor/essentials/data-platform-metrics.md).

**Microsoft 'tan Azure CDN tarafından desteklenen ölçümler**

| Ölçümler  | Description | Boyutlar |
| ------------- | ------------- | ------------- |
| Bayt Isabet oranı * | Toplam çıkış için hesaplanan CDN önbelleğinden çıkış yüzdesi. | Uç Nokta |
| Istek sayısı | CDN tarafından sunulan istemci isteklerinin sayısı. | Uç Nokta </br> İstemci ülkesi. </br> İstemci bölgesi. </br> HTTP durumu. </br> HTTP durum grubu. |
| Yanıt boyutu | CDN kenarından istemcilere yanıt olarak gönderilen bayt sayısı. |Uç Nokta </br> İstemci ülkesi. </br> İstemci bölgesi. </br> HTTP durumu. </br> HTTP durum grubu. |
| TotalLatency | **Son Yanıt BAYTı CDN 'den istemciye gönderilene kadar** CDN tarafından alınan istemci isteğinin toplam süresi. |Uç Nokta </br> İstemci ülkesi. </br> İstemci bölgesi. </br> HTTP durumu. </br> HTTP durum grubu. |

***Bayt Isabet sayısı = (uçtan çıkış çıkış kaynağından çıkış)/kenardan çıkış**

Bayt isabet oranı hesaplamasında dışlanan senaryolar:

* Kural altyapısı veya sorgu dizesi önbellek davranışı aracılığıyla herhangi bir önbellek kesin olarak yapılandırırsınız.
* Cache-Control yönergesini, mağaza veya özel önbellek olmadan açıkça yapılandırırsınız.

### <a name="metrics-configuration"></a>Ölçüm yapılandırması

1. Azure portal menüsünde **tüm kaynaklar**' ı seçin  >>  **\<your-CDN-profile>** .

2. **İzleme** altında **ölçümler**' i seçin:

    :::image type="content" source="./media/cdn-raw-logs/raw-logs-03.png" alt-text="CDN profili ölçümleri." border="true":::

3. **Ölçüm Ekle**' yi seçin, eklenecek ölçümü seçin:

    :::image type="content" source="./media/cdn-raw-logs/raw-logs-04.png" alt-text="CDN profili için ölçüm ekleyin ve seçin." border="true":::

4. Filtre eklemek için **Filtre Ekle** ' yi seçin:
    
    :::image type="content" source="./media/cdn-raw-logs/raw-logs-05.png" alt-text="Ölçüme filtre uygulayın." border="true":::

5. Farklı boyutlara göre eğilimi görmek için bölmeyi **Uygula** ' yı seçin:

    :::image type="content" source="./media/cdn-raw-logs/raw-logs-06.png" alt-text="Ölçüme bölme uygulayın." border="true":::

6. Yeni **grafik ' i seçerek** yeni bir grafik ekleyin:

    :::image type="content" source="./media/cdn-raw-logs/raw-logs-07.png" alt-text="Ölçüm görünümüne yeni grafik ekleyin." border="true":::

### <a name="alerts"></a>Uyarılar

**İzleme** uyarıları ' nı seçerek Microsoft CDN 'de uyarılar ayarlayabilirsiniz  >>  .

Ölçümler bölümünde listelenen ölçümler için **Yeni uyarı kuralı** seçin:

:::image type="content" source="./media/cdn-raw-logs/raw-logs-08.png" alt-text="CDN uç noktası için uyarıları yapılandırın." border="true":::

Uyarı, Azure Izleyici 'ye göre ücretlendirilir. Uyarılar hakkında daha fazla bilgi için bkz. [Azure izleyici uyarıları](../azure-monitor/alerts/alerts-overview.md).

### <a name="additional-metrics"></a>Ek ölçümler
Ek bir maliyet için Azure Log Analytics ve ham günlükleri kullanarak ek ölçümler etkinleştirebilirsiniz.

1. Tanılama 'yı Log Analytics 'e ham günlük göndermek için etkinleştirme bölümündeki adımları izleyin.

2. Oluşturduğunuz Log Analytics çalışma alanını seçin:

    :::image type="content" source="./media/cdn-raw-logs/raw-logs-09.png" alt-text="Log Analytics çalışma alanını seçin" border="true":::   

3. Log Analytics çalışma alanında **genel** altında **Günlükler** ' i seçin.  Sonra **kullanmaya** başlayın ' ı seçin:

    :::image type="content" source="./media/cdn-raw-logs/raw-logs-10.png" alt-text="Log Analytics kaynak çalışma alanı." border="true":::   
 
4. **CDN profillerini** seçin.  Özel bir sorgu girmek için ÇALıŞTıRıLACAK örnek bir sorgu seçin veya örnek ekranı kapatın:

    :::image type="content" source="./media/cdn-raw-logs/raw-logs-11.png" alt-text="Örnek sorgu ekranı." border="true":::   

    :::image type="content" source="./media/cdn-raw-logs/raw-logs-12.png" alt-text="Sorgu yürütme." border="true":::   

4. Verileri grafiğe göre görüntülemek için **grafik**' i seçin.  Grafiği Azure panosuna sabitlemek için **panoya sabitle** ' yi seçin:

    :::image type="content" source="./media/cdn-raw-logs/raw-logs-13.png" alt-text="Grafiği panoya sabitle." border="true"::: 

## <a name="next-steps"></a>Sonraki Adımlar
Bu makalede, Microsoft CDN hizmeti için HTTP RAW günlüklerini etkinleştirdiniz.

Azure CDN ve bu makalede bahsedilen diğer Azure hizmetleri hakkında daha fazla bilgi için bkz.:

* [Çözümle](cdn-log-analysis.md) Kullanım desenlerini Azure CDN.

* [Azure izleyici](../azure-monitor/overview.md)hakkında daha fazla bilgi edinin.

* [Azure izleyici 'de Log Analytics](../azure-monitor/logs/log-analytics-tutorial.md)yapılandırın.