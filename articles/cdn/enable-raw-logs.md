---
title: HTTP ham günlüklerini Azure CDN
description: Bu makalede HTTP ham günlükleri Azure CDN açıklanmaktadır.
services: cdn
author: asudbring
manager: KumudD
ms.service: azure-cdn
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 07/22/2020
ms.author: allensu
ms.openlocfilehash: 3b36e528a013403a2ed664d3011338d92f37a3db
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87040168"
---
# <a name="azure-cdn-http-raw-logs"></a>HTTP ham günlüklerini Azure CDN
Ham Günlükler, denetim ve sorun giderme için önemli olan işlemler ve hatalar hakkında zengin bilgiler sağlar. Ham Günlükler etkinlik günlüklerinden farklıdır. Etkinlik günlükleri, Azure kaynakları üzerinde gerçekleştirilen işlemlere görünürlük sağlar. Ham Günlükler, kaynağınızın bir işlem kaydını sağlar. Ham günlük, CDN 'nin aldığı her istek hakkında zengin bilgiler sağlar. 

> [!IMPORTANT]
> HTTP ham günlükleri özelliği, Microsoft 'tan Azure CDN için kullanılabilir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="configuration---azure-portal"></a>Yapılandırma-Azure portal

Microsoft profilinden Azure CDN ham günlüklerini yapılandırmak için: 

1. Azure portal menüsünde **tüm kaynaklar**' ı seçin  >>  **\<your-CDN-profile>** .

2. **İzleme**altında **Tanılama ayarları**' nı seçin.

3. **+ Tanılama ayarı Ekle**' yi seçin.

    ![CDN tanılama ayarı](./media/cdn-raw-logs/raw-logs-01.png)

    > [!IMPORTANT]
    > Ham Günlükler yalnızca profil düzeyinde kullanılabilir, ancak toplu http durum kodu günlükleri uç nokta düzeyinde kullanılabilir.

4. **Tanılama ayarları**' nın altında, tanılama **ayarları adı**altında tanılama ayarı için bir ad girin.

5. **Günlüğü** seçin ve bekletme süresini gün olarak ayarlayın.

6. **Hedef ayrıntılarını**seçin. Hedef seçenekleri şunlardır:
    * **Log Analytics’e gönderme**
        * **Aboneliği** ve **Log Analytics çalışma alanını**seçin.
    * **Bir depolama hesabına Arşivle**
        * **Aboneliği** ve **Depolama hesabını**seçin.
    * **Bir olay hub 'ına akış**
        * **Abonelik**, **Olay Hub 'ı ad alanı**, **Olay Hub 'ı adı (isteğe bağlı)** ve **Olay Hub 'ı ilke adı**' nı seçin.

    ![CDN tanılama ayarı](./media/cdn-raw-logs/raw-logs-02.png)

7. **Kaydet**'i seçin.

## <a name="configuration---azure-powershell"></a>Yapılandırma-Azure PowerShell

Ham Günlükler için tanılama ayarını yapılandırmak üzere [set-AzDiagnosticSetting](https://docs.microsoft.com/powershell/module/az.monitor/set-azdiagnosticsetting?view=latest) ' i kullanın.

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

| Özellik              | Açıklama                                                                                                                                                                                          |
|-----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| TrackingReference     | Ön kapıya yönelik olarak sunulan ve istemciye X-Azure-ref üst bilgisi olarak gönderilen bir isteği tanımlayan benzersiz başvuru dizesi. Belirli bir istek için erişim günlüklerindeki ayrıntıları aramak için gereklidir. |
| HttpMethod            | İstek tarafından kullanılan HTTP yöntemi.                                                                                                                                                                     |
| HttpVersion           | İstek veya bağlantı türü.                                                                                                                                                                   |
| RequestUri            | Alınan isteğin URI 'SI.                                                                                                                                                                         |
| RequestBytes          | İstek üst bilgileri ve istek gövdesi dahil olmak üzere HTTP isteği iletisinin bayt cinsinden boyutu.                                                                                                   |
| ResponseBytes         | Yanıt olarak arka uç sunucusu tarafından gönderilen bayt.                                                                                                                                                    |
| Kullanıcı             | İstemcinin kullandığı tarayıcı türü.                                                                                                                                                               |
| ClientIp              | İsteği yapan istemcinin IP adresi.                                                                                                                                                  |
| TimeTaken             | İşlemin gerçekleştiği sürenin milisaniye cinsinden uzunluğu.                                                                                                                                            |
| SecurityProtocol      | İstek tarafından kullanılan TLS/SSL protokol sürümü veya şifreleme yoksa null.                                                                                                                           |
| Uç Noktası              | CDN uç noktası Konağı, üst CDN profili altında yapılandırıldı.                                                                                                                                   |
| Arka uç konak adı     | İsteklerin gönderildiği arka uç konağın veya başlangıcının adı.                                                                                                                                |
| Kaynak Shield 'a gönderildi </br> (kullanım dışı) * **aşağıdaki kullanımdan kaldırıldığına bakın.** | True ise, isteğin kenar pop 'u yerine kaynak kalkan önbelleğinden yanıtlandığını gösterir. Kaynak kalkan, önbellek isabet oranını artırmak için kullanılan bir üst önbelleğidir.                                       |
| ıreceived Fromclient | Doğru ise, isteğin istemciden geldiği anlamına gelir. Yanlış ise, istek kenarda (alt POP) bir isabetsizlik olur ve kaynak kalkanı (üst POP) ile yanıt verilir. 
| HttpStatusCode        | Proxy 'den döndürülen HTTP durum kodu.                                                                                                                                                        |
| HttpStatusDetails     | İstek üzerine sonuçtaki durum. Bu dize değerinin anlamı, bir durum başvuru tablosunda bulunabilir.                                                                                              |
| Cağımız                   | Kullanıcı isteğine yanıt veren kenar açılır. Pop 'Ların kısaltmaları ilgili Metros Havaalanı kodlarıdır.                                                                                   |
| Önbellek durumu          | Nesnenin önbellekten döndürülüp döndürülmeyeceğini veya kaynaktan geldiğini belirtir.                                                                                                             |
> [!NOTE]
> Günlükler, bir sorgu çalıştırılarak Log Analytics profiliniz altında görüntülenebilir. Örnek bir sorgu AzureDiagnostics gibi görünür WHERE kategorisi = = "AzureCdnAccessLog"


### <a name="sent-to-origin-shield-deprecation"></a>Kaynak kalkan kullanım dışı bırakılmasıyla gönderildi
Ham günlük özelliği **ısenttooriginshield** kullanım dışıdır ve yeni bir alan tarafından alınmış bir **fromclient**tarafından değiştirildi. Zaten kullanım dışı olan alanı kullanıyorsanız, yeni alanı kullanın. 

Ham günlüklerde hem CDN Edge (alt POP) hem de kaynak kalkanı tarafından oluşturulan Günlükler bulunur. Kaynak kalkan, dünya genelinde stratejik olarak bulunan üst düğümlere başvurur. Bu düğümler, kaynak sunucularıyla iletişim kurar ve kaynak yükünü kaynak olarak azaltır. 

Kaynak Shield 'a giden her istek için 2 günlük girişi vardır:

* Kenar düğümleri için bir tane
* Kaynak kalkan için bir tane. 

Uç düğümlerinden ve kaynak kalanlardan çıkış ya da yanıtları ayırt etmek için, uygun verileri almak üzere ıreceived Fromclient alanını kullanabilirsiniz. 

Değer false ise, isteğin kaynak Shield 'dan kenar düğümlerine yanıt verdiği anlamına gelir. Bu yaklaşım, ham günlükleri faturalandırma verileriyle karşılaştırmak için geçerlidir. Ücret, çıkış durumundan kenar düğümlerine çıkış için tahakkuk etmemektedir. Ücretler, uç düğümlerinden istemcilere çıkış için ücretlendirilir. 

**Log Analytics kaynak kalkanı üzerinde oluşturulan günlüklerin hariç tutulmasını sağlamak için sorgu örneği kusto.**

```kusto
AzureDiagnostics 
| where OperationName == "Microsoft.Cdn/Profiles/AccessLog/Write" and Category == "AzureCdnAccessLog"  
| where isReceivedFromClient == true

```

> [!IMPORTANT]
> HTTP ham günlükleri özelliği, **25 şubat 2020**' den sonra oluşturulan veya güncellenen tüm profiller için otomatik olarak kullanılabilir. Daha önce oluşturulan CDN profilleri için, bir tane, günlüğü ayarladıktan sonra CDN uç noktasını güncelleştirmelidir. Örneğin, bir tane, CDN uç noktaları altında coğrafi filtrelemeye gidebilir ve iş yüküyle ilgili olmayan herhangi bir ülkeyi/bölgeyi engelleyebilir ve Kaydet ' i ziyaret edebilir. 

## <a name="next-steps"></a>Sonraki Adımlar
Bu makalede, Microsoft CDN hizmeti için HTTP RAW günlüklerini etkinleştirdiniz.

Azure CDN ve bu makalede bahsedilen diğer Azure hizmetleri hakkında daha fazla bilgi için bkz.:

* [Çözümle](cdn-log-analysis.md) Kullanım desenlerini Azure CDN.

* [Azure izleyici](https://docs.microsoft.com/azure/azure-monitor/overview)hakkında daha fazla bilgi edinin.

* [Azure izleyici 'de Log Analytics](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal)yapılandırın.
