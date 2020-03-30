---
title: Azure SignalR Hizmeti için Tanılama Günlükleri
description: Azure Sinyal Hizmeti için tanılama günlüklerini nasıl ayarlayın ve kendi kendine sorun gidermek için nasıl kullanabileceğinizi öğrenin.
author: wanlwanl
ms.service: signalr
ms.topic: conceptual
ms.date: 12/17/2019
ms.author: wanl
ms.openlocfilehash: 72f57ba4bbbbde07f6d26edc88c158f301ebe2f2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79536743"
---
# <a name="diagnostic-logs-for-azure-signalr-service"></a>Azure SignalR Hizmeti için Tanılama Günlükleri

Bu öğretici, Azure Sinyal Rıtahizmeti için tanı günlüklerinin neler olduğunu ve tanılama günlüklerinin nasıl ayarlandığını ve tanılama günlükleriyle nasıl sorun giderilenleri kapsar.

## <a name="prerequisites"></a>Ön koşullar
Tanılama günlüklerini etkinleştirmek için günlük verilerinizi depolayacak bir yere ihtiyacınız vardır. Bu öğretici, Azure Depolama ve Günlük Analizi'ni kullanır.

* [Azure depolama](../azure-monitor/platform/resource-logs-collect-storage.md) - İlke denetimi, statik çözümleme veya yedekleme için tanılama günlüklerini korur.
* [Log Analytics](../azure-monitor/platform/resource-logs-collect-workspace.md) - Bir Azure kaynağı tarafından oluşturulan ham günlüklerin analizine olanak tanıyan esnek bir günlük arama ve analiz aracı.

## <a name="set-up-diagnostic-logs-for-an-azure-signalr-service"></a>Azure SinyalR Hizmeti için tanılama günlükleri ayarlama

Azure SignalR Hizmeti için tanılama günlüklerini görüntüleyebilirsiniz. Bu günlükler, Azure Sinyal RHizmeti örneğinize daha zengin bağlantı görünümü sağlar. Tanılama günlükleri her bağlantının ayrıntılı bilgisini sağlar. Örneğin, bağlantının temel bilgileri (kullanıcı kimliği, bağlantı kimliği ve aktarım türü vb.) ve olay bilgileri (bağlantı, bağlantı kesme ve iptal etme vb.) Tanılama günlükleri sorun tanımlama, bağlantı izleme ve analiz için kullanılabilir.

### <a name="enable-diagnostic-logs"></a>Tanılama günlüklerini etkinleştirme

Tanılama günlükleri varsayılan olarak devre dışı bırakılır. Tanılama günlüklerini etkinleştirmek için aşağıdaki adımları izleyin:

1. Azure [portalında](https://portal.azure.com), **İzleme**altında **Tanılama ayarlarını**tıklatın.

    ![Tanılama ayarlarına bölme gezintisi](./media/signalr-tutorial-diagnostic-logs/diagnostic-settings-menu-item.png)

1. Ardından **tanı ayarını ekle'yi**tıklatın.

    ![Tanılama günlükleri ekleme](./media/signalr-tutorial-diagnostic-logs/add-diagnostic-setting.png)

1. İstediğinize arşiv hedefini ayarlayın. Şu anda **Arşiv'i bir depolama hesabına** destekliyoruz ve **Log Analytics'e gönder.**

1. Arşivlemek istediğiniz günlükleri seçin.

    ![Tanılama ayarları bölmesi](./media/signalr-tutorial-diagnostic-logs/diagnostics-settings-pane.png)


1. Yeni tanılama ayarlarını kaydedin.

Yeni ayarlar yaklaşık 10 dakika içinde etkili olur. Bundan sonra, günlükler yapılandırılan arşiv hedefinde, **Tanılama günlükleri** bölmesinde görünür.

Tanılamayı yapılandırma hakkında daha fazla bilgi için [Azure tanı günlüklerine genel bakış](../azure-monitor/platform/platform-logs-overview.md)alameti dir.

### <a name="diagnostic-logs-categories"></a>Tanılama günlükleri kategorileri

Azure SignalR Hizmeti tanı günlüklerini tek bir kategoride yakalar:

* **Tüm Günlükler**: Azure SignalR Hizmetine bağlanan bağlantıları izleyin. Günlükler bağlantı/bağlantı kesme, kimlik doğrulama ve azaltma hakkında bilgi sağlar. Daha fazla bilgi için sonraki bölüme bakın.

### <a name="archive-to-a-storage-account"></a>Bir depolama hesabına arşivle

Günlükler, **Tanılama günlükleri** bölmesinde yapılandırılan depolama hesabında depolanır. Tanı günlüklerini depolamak için otomatik olarak adlandırılmış `insights-logs-alllogs` bir kapsayıcı oluşturulur. Kapsayıcının içinde, günlükler dosyada `resourceId=/SUBSCRIPTIONS/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/RESOURCEGROUPS/XXXX/PROVIDERS/MICROSOFT.SIGNALRSERVICE/SIGNALR/XXX/y=YYYY/m=MM/d=DD/h=HH/m=00/PT1H.json`saklanır. Temel olarak, yol `resource ID` ile `Date Time`birleştirilir ve . Günlük dosyaları `hour`. Bu nedenle, dakika `m=00`her zaman .

Tüm günlükler JavaScript Nesne Gösterimi (JSON) biçiminde depolanır. Her giriş, aşağıdaki bölümlerde açıklanan biçimi kullanan dize alanları vardır.

Arşiv günlüğü JSON dizeleri aşağıdaki tablolarda listelenen öğeleri içerir:

**Biçimlendir**

Adı | Açıklama
------- | -------
time | Günlük olay zamanı
düzey | Günlük olay düzeyi
resourceId | Azure SignalR Hizmetinizin kaynak kimliği
location | Azure SinyalR Hizmetinizin Konumu
category | Günlük olayının kategorisi
operationName | Olayın işlem adı
callerIpAddress | Sunucunuzun/istemcinizin IP adresi
properties | Bu günlük olayıyla ilgili ayrıntılı özellikler. Daha fazla ayrıntı için aşağıdaki özellikler tablosuna bakın

**Özellikler Tablosu**

Adı | Açıklama
------- | -------
type | Günlük olayıtürü. Şu anda Azure SignalR Hizmetine bağlantı hakkında bilgi sağlıyoruz. Yalnızca `ConnectivityLogs` tür kullanılabilir
 koleksiyonu | Günlük olayının toplanması. İzin verilen `Connection`değerler `Authorization` şunlardır: , ve`Throttling`
Connectionıd | Bağlantının kimliği
Transporttype | Bağlantının aktarım türü. İzin verilen `Websockets` \| `ServerSentEvents` \| değerler şunlardır:`LongPolling`
Connectiontype | Bağlantı türü. İzin verilen `Server` \| `Client`değerler şunlardır: . `Server`: sunucu tarafından bağlantı; `Client`: istemci tarafından bağlantı
userId | Kullanıcının kimliği
message | Günlük olayının ayrıntılı iletisi

Aşağıdaki kod bir arşiv günlüğü JSON dize örneğidir:

```json
{
    "properties": {
        "message": "Entered Serverless mode.",
        "type": "ConnectivityLogs",
        "collection": "Connection",
        "connectionId": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
        "userId": "User",
        "transportType": "WebSockets",
        "connectionType": "Client"
    },
    "operationName": "ServerlessModeEntered",
    "category": "AllLogs",
    "level": "Informational",
    "callerIpAddress": "xxx.xxx.xxx.xxx",
    "time": "2019-01-01T00:00:00Z",
    "resourceId": "/SUBSCRIPTIONS/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/RESOURCEGROUPS/XXXX/PROVIDERS/MICROSOFT.SIGNALRSERVICE/SIGNALR/XXX",
    "location": "xxxx"
}
```

### <a name="archive-logs-schema-for-log-analytics"></a>Log Analytics için arşiv günlükleri şema

Tanılama günlüklerini görüntülemek için aşağıdaki adımları izleyin:

1. Hedef `Logs` Log Analytics'e tıklayın.

    ![Günlük Analytics menü öğesi](./media/signalr-tutorial-diagnostic-logs/log-analytics-menu-item.png)

2. Tanılama günlüklerini sorgulamak için zaman aralığını girin `SignalRServiceDiagnosticLogs` ve seçin. Gelişmiş sorgu için Azure [Monitörü'nde Günlük Analizi'ne başlayın'a](../azure-monitor/log-query/get-started-portal.md) bakın

    ![Log Analytics'te sorgu günlüğü](./media/signalr-tutorial-diagnostic-logs/query-log-in-log-analytics.png)

Arşiv günlüğü sütunları aşağıdaki tabloda listelenen öğeleri içerir:

Adı | Açıklama
------- | ------- 
TimeGenerated | Günlük olay zamanı
Koleksiyon | Günlük olayının toplanması. İzin verilen `Connection`değerler `Authorization` şunlardır: , ve`Throttling`
ThrottledRequests | Olayın işlem adı
Konum | Azure SinyalR Hizmetinizin Konumu
Düzey | Günlük olay düzeyi
ArayanIpAddress | Sunucunuzun/istemcinizin IP adresi
İleti | Günlük olayının ayrıntılı iletisi
UserId | Kullanıcının kimliği
Connectionıd | Bağlantının kimliği
Connectiontype | Bağlantı türü. İzin verilen `Server` \| `Client`değerler şunlardır: . `Server`: sunucu tarafından bağlantı; `Client`: istemci tarafından bağlantı
Transporttype | Bağlantının aktarım türü. İzin verilen `Websockets` \| `ServerSentEvents` \| değerler şunlardır:`LongPolling`

### <a name="troubleshooting-with-diagnostic-logs"></a>Tanılama günlükleriyle sorun giderme

Azure SignalR Hizmeti için sorun gidermek için, sunucu/istemci yan günlüklerinin hataları yakalamasını etkinleştirebilirsiniz. Şu anda, Azure SignalR Hizmeti tanılama günlüklerini ortaya çıkarır, hizmet tarafı için günlükleri de etkinleştirebilirsiniz.

Bağlantı beklenmeyen büyüme veya düşme durumuyla karşılaştığında, sorun gidermek için tanılama günlüklerinden yararlanabilirsiniz.

Tipik sorunlar genellikle bağlantıların beklenmeyen miktar değişiklikleri, bağlantılar bağlantı sınırlarına ulaşmak ve yetkilendirme hatası ile ilgilidir. Sorun giderme yle ilgili sonraki bölümlere bakın.

#### <a name="unexpected-connection-number-changes"></a>Beklenmeyen bağlantı numarası değişiklikleri

##### <a name="unexpected-connection-dropping"></a>Beklenmeyen bağlantı düşüyor

Beklenmeyen bağlantılar düşerse, öncelikle hizmet, sunucu ve istemci taraflarında günlükleri etkinleştirin.

Bağlantı kesilirse, tanılama günlükleri bu bağlantı kesme olayını `ConnectionAborted` `ConnectionEnded` kaydeder, görürsünüz veya `operationName`.

Arasındaki `ConnectionAborted` fark `ConnectionEnded` ve `ConnectionEnded` istemci veya sunucu tarafı tarafından tetiklenen beklenen bir bağlantı kesme olmasıdır. `ConnectionAborted` Genellikle beklenmeyen bir bağlantı bırakma olayı iken ve iptal nedeni `message`sağlanacaktır.

İptal nedenleri aşağıdaki tabloda listelenmiştir:

Neden | Açıklama
------- | ------- 
Bağlantı sayısı sınıra ulaşır | Bağlantı sayısı geçerli fiyat seviyenizin sınırına ulaşır. Servis birimini ölçeklendirmeyi göz önünde bulundurun
Uygulama sunucusu bağlantıyı kapattı | Uygulama sunucusu kürtajı tetikler. Bu beklenen bir kürtaj olarak kabul edilebilir
Bağlantı ping zaman | Genellikle ağ sorunu neden olur. Uygulama sunucunuzun internetten kullanılabilirliğini kontrol etmeyi düşünün
Hizmet yeniden yükleme, yeniden bağlanma | Azure SignalR Hizmeti yeniden yükleniyor. Azure SignalR otomatik yeniden bağlanmayı destekler, Azure SinyalR Hizmetine yeniden bağlanana veya el ile yeniden bağlanana kadar bekleyebilirsiniz
Dahili sunucu geçici hatası | Azure SignalR Hizmeti'nde geçici hata oluşur, otomatik olarak kurtarılmalıdır
Sunucu bağlantısı düştü | Sunucu bağlantısı bilinmeyen bir hatayla düşer, önce servis/sunucu/istemci yan günlüğüyle kendi kendine sorun gidermeyi düşünün. Temel sorunları (örn. Ağ sorunu, uygulama sunucusu yan sorunu vb.) hariç tutmayı deneyin. Sorun çözülmezse, daha fazla yardım için bizimle iletişime geçin. Daha fazla bilgi için yardım [al](#get-help) bölümüne bakın. 

##### <a name="unexpected-connection-growing"></a>Beklenmeyen bağlantı büyüyor

Beklenmeyen bağlantı nın büyümeyle ilgili sorun giderme de yapmanız gereken ilk şey, ek bağlantıları filtrelemektir. Test istemcisi bağlantınıza benzersiz test kullanıcı kimliği ekleyebilirsiniz. Daha sonra tanılama günlükleri ile doğrulayın, birden fazla istemci bağlantıları aynı test kullanıcı kimliği veya IP görürseniz, o zaman istemci tarafı oluşturmak ve beklenti daha fazla bağlantı kurmak muhtemeldir. Müşteri tarafını kontrol et.

#### <a name="authorization-failure"></a>Yetkilendirme hatası

İstemci istekleri için 401 Yetkisiz iade alırsanız, tanılama günlüklerinizi kontrol edin. Karşılaşırsanız, `Failed to validate audience. Expected Audiences: <valid audience>. Actual Audiences: <actual audience>`bu, erişim belirtecinizdeki tüm izleyicilerin geçersiz olduğu anlamına gelir. Günlükte önerilen geçerli hedef kitleleri kullanmaya çalışın.


#### <a name="throttling"></a>Azaltma

Azure SinyalR Hizmeti'ne SignalR istemci bağlantıları kuramayacağınızı fark ederseniz, tanılama günlüklerinizi kontrol edin. Tanılama `Connection count reaches limit` günlüğünde karşılaşırsanız, bağlantı sayısı sınırına ulaşan SignalR Hizmeti'ne çok fazla bağlantı kurarsınız. SignalR Hizmetinizi yükseltmeyi düşünün. Tanılama `Message count reaches limit` günlüğünde karşılaşırsanız, bu ücretsiz katman kullandığınız ve ileti kotasını kullandığınız anlamına gelir. Daha fazla ileti göndermek istiyorsanız, ek iletiler göndermek için SignalR Hizmetinizi standart katmana değiştirmeyi düşünün. Daha fazla bilgi için Azure [SignalR Hizmet Fiyatlandırması'na](https://azure.microsoft.com/pricing/details/signalr-service/)bakın.

### <a name="get-help"></a>Yardım alın

Önce sorun gidermenizi öneririz. Sorunların çoğu uygulama sunucusu veya ağ sorunları ndan kaynaklanır. Temel nedeni bulmak için tanılama günlüğü ve [temel sorun giderme kılavuzuyla](https://github.com/Azure/azure-signalr/blob/dev/docs/tsg.md) [sorun giderme kılavuzunu](#troubleshooting-with-diagnostic-logs) izleyin.
Sorun hala çözülemiyorsa, GitHub'da bir sorun açmayı veya Azure Portalı'nda bilet oluşturmayı düşünün.
Sağlamak:
1. Sorun oluştuğunda yaklaşık 30 dakika zaman aralığı
2. Azure SignalR Hizmetinin kaynak kimliği
3. Sorun ayrıntıları, mümkün olduğunca spesifik: Örneğin, appserver mesaj göndermez, istemci bağlantısı düşer ve benzeri
4. Sunucu/istemci tarafından toplanan günlükler ve yararlı olabilecek diğer materyaller
5. [İsteğe bağlı] Repro kodu

> [!NOTE]
> Sorunu GitHub'da açarsanız, hassas bilgilerinizi (örneğin, kaynak kimliği, sunucu/istemci günlükleri) gizli tutun, yalnızca Microsoft kuruluşundaki üyelere özel olarak gönderin.