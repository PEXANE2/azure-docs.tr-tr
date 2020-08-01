---
title: Azure depolama izleme veri başvurusu | Microsoft Docs
description: Azure depolama 'daki verileri izlemek için günlük ve ölçüm başvurusu.
author: normesta
services: azure-monitor
ms.service: azure-monitor
ms.topic: reference
ms.date: 05/01/2020
ms.author: normesta
ms.subservice: logs
ms.custom: monitoring
ms.openlocfilehash: 28a127b4debeacd2562867008bc594897558d50d
ms.sourcegitcommit: cee72954f4467096b01ba287d30074751bcb7ff4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2020
ms.locfileid: "87446848"
---
# <a name="azure-storage-monitoring-data-reference"></a>Azure depolama izleme veri başvurusu

Azure depolama için izleme verilerini toplama ve çözümleme hakkında ayrıntılı bilgi için bkz. [Azure Storage 'ı izleme](monitor-storage.md) .

## <a name="metrics"></a>Ölçümler

Aşağıdaki tablolarda, Azure depolama için toplanan platform ölçümleri listelenmektedir. 

### <a name="capacity-metrics"></a>Kapasite ölçümleri

Kapasite ölçümleri değerleri her saat Azure Izleyici 'ye gönderilir. Değerler günlük olarak yenilenir. Zaman dilimi, ölçüm değerlerinin sunulduğu zaman aralığını tanımlar. Tüm kapasite ölçümleri için desteklenen zaman dilimi bir saattir (PT1H).

Azure depolama, Azure Izleyici 'de aşağıdaki kapasite ölçümlerini sağlar.

#### <a name="account-level"></a>Hesap düzeyi

Bu tabloda [hesap düzeyi ölçümleri](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported#microsoftstoragestorageaccounts)gösterilmektedir.

| Ölçüm | Açıklama |
| ------------------- | ----------------- |
| UsedCapacity | Depolama hesabı tarafından kullanılan depolama miktarı. Standart depolama hesapları için blob, tablo, dosya ve kuyruk tarafından kullanılan kapasitenin toplamıdır. Premium depolama ve Blob Depolama hesapları için BlobCapacity ile aynıdır. <br/><br/> Birim: bayt <br/> Toplama türü: Ortalama <br/> Değer örneği: 1024 |

#### <a name="blob-storage"></a>Blob depolama

Bu tabloda [BLOB depolama ölçümleri](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported#microsoftstoragestorageaccountsblobservices)gösterilmektedir.

| Ölçüm | Açıklama |
| ------------------- | ----------------- |
| BlobCapacity | Depolama hesabında kullanılan BLOB depolama alanı toplamı. <br/><br/> Birim: bayt <br/> Toplama türü: Ortalama <br/> Değer örneği: 1024 <br/> Boyutlar: **Blobtype**ve **blobtier** ([tanım](#metrics-dimensions)) |
| BlobCount    | Depolama hesabında depolanan BLOB nesnelerinin sayısı. <br/><br/> Birim: sayım <br/> Toplama türü: Ortalama <br/> Değer örneği: 1024 <br/> Boyutlar: **Blobtype**ve **blobtier** ([tanım](#metrics-dimensions)) |
| BlobProvisionedSize | Depolama hesabında sağlanan depolama miktarı. Bu ölçüm yalnızca Premium Depolama hesapları için geçerlidir. <br/><br/> Birim: bayt <br/> Toplama türü: Ortalama |
| ContainerCount    | Depolama hesabındaki kapsayıcıların sayısı. <br/><br/> Birim: sayım <br/> Toplama türü: Ortalama <br/> Değer örneği: 1024 |
| Dizin kapasitesi     | ADLS 2. hiyerarşik dizin tarafından kullanılan depolama miktarı <br/><br/> Birim: bayt <br/> Toplama türü: Ortalama <br/> Değer örneği: 1024 |

#### <a name="table-storage"></a>Table Storage

Bu tabloda [Tablo depolama ölçümleri](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported#microsoftstoragestorageaccountstableservices)gösterilmektedir.

| Ölçüm | Açıklama |
| ------------------- | ----------------- |
| Tablokapasitesi | Depolama hesabı tarafından kullanılan tablo depolaması miktarı. <br/><br/> Birim: bayt <br/> Toplama türü: Ortalama <br/> Değer örneği: 1024 |
| Tablosayısı   | Depolama hesabındaki tablo sayısı. <br/><br/> Birim: sayım <br/> Toplama türü: Ortalama <br/> Değer örneği: 1024 |
| TableEntityCount | Depolama hesabındaki tablo varlıklarının sayısı. <br/><br/> Birim: sayım <br/> Toplama türü: Ortalama <br/> Değer örneği: 1024 |

#### <a name="queue-storage"></a>Kuyruk depolama

Bu tabloda [kuyruk depolama ölçümleri](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported#microsoftstoragestorageaccountsfileservices)gösterilmektedir.

| Ölçüm | Açıklama |
| ------------------- | ----------------- |
| QueueCapacity | Depolama hesabı tarafından kullanılan kuyruk depolama miktarı. <br/><br/> Birim: bayt <br/> Toplama türü: Ortalama <br/> Değer örneği: 1024 |
| QueueCount   | Depolama hesabındaki sıraların sayısı. <br/><br/> Birim: sayım <br/> Toplama türü: Ortalama <br/> Değer örneği: 1024 |
| QueueMessageCount | Depolama hesabının Kuyruk hizmeti sıra iletilerinin yaklaşık sayısı. <br/><br/>Birim: sayım <br/> Toplama türü: Ortalama <br/> Değer örneği: 1024 |

#### <a name="file-storage"></a>Dosya depolama

Bu tabloda [dosya depolama ölçümleri](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported#microsoftstoragestorageaccountsqueueservices)gösterilmektedir.

| Ölçüm | Açıklama |
| ------------------- | ----------------- |
| Dosya kapasitesi | Depolama hesabı tarafından kullanılan dosya depolama alanı miktarı. <br/><br/> Birim: bayt <br/> Toplama türü: Ortalama <br/> Değer örneği: 1024 |
| FileCount   | Depolama hesabındaki dosyaların sayısı. <br/><br/> Birim: sayım <br/> Toplama türü: Ortalama <br/> Değer örneği: 1024 |
| Dosya ShareCount | Depolama hesabındaki dosya paylaşımlarının sayısı. <br/><br/> Birim: sayım <br/> Toplama türü: Ortalama <br/> Değer örneği: 1024 |
| FileShareProvisionedIOPS | Bir dosya paylaşımında sağlanan ıOPS sayısı. Bu ölçüm yalnızca Premium dosya depolaması için geçerlidir. <br/><br/> Birim: bayt <br/> Toplama türü: Ortalama |

### <a name="transaction-metrics"></a>İşlem ölçümleri

İşlem ölçümleri, Azure depolama 'daki bir depolama hesabına yönelik her istek için Azure Izleyici 'ye dağıtılır. Depolama hesabınızda hiçbir etkinlik olmaması durumunda, dönem içinde işlem ölçümlerinde hiç veri olmayacaktır. Tüm işlem ölçümleri hem hesap hem de hizmet düzeyinde (BLOB depolama, tablo depolama, Azure dosyaları ve kuyruk depolama) kullanılabilir. Zaman dilimi, ölçüm değerlerinin sunulduğu zaman aralığını tanımlar. Tüm işlem ölçümleri için desteklenen saat graıns PT1H ve PT1M.

Azure depolama, Azure Izleyici 'de aşağıdaki işlem ölçümlerini sağlar.

| Ölçüm | Açıklama |
| ------------------- | ----------------- |
| İşlemler | Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız istekleri ve hata üreten istekleri içerir. <br/><br/> Birim: sayım <br/> Toplama türü: Toplam <br/> Geçerli Boyutlar: ResponseType, GeoType, ApiName ve Authentication ([tanım](#metrics-dimensions))<br/> Değer örneği: 1024 |
| Giriş | Giriş verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir. <br/><br/> Birim: bayt <br/> Toplama türü: Toplam <br/> Geçerli Boyutlar: GeoType, ApiName ve Authentication ([tanım](#metrics-dimensions)) <br/> Değer örneği: 1024 |
| Çıkış | Çıkış verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya çıkan ve Azure içinde çıkan verileri içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz. <br/><br/> Birim: bayt <br/> Toplama türü: Toplam <br/> Geçerli Boyutlar: GeoType, ApiName ve Authentication ([tanım](#metrics-dimensions)) <br/> Değer örneği: 1024 |
| Başarılı Sunucugecikmesi | Azure Depolama tarafından gerçekleştirilen başarılı bir isteği işlemek için kullanılan ortalama süre. Bu değer, Başarı E2E Gecikme Süresi’nde belirtilen ağ gecikme süresini içermez. <br/><br/> Birim: milisaniye <br/> Toplama türü: Ortalama <br/> Geçerli Boyutlar: GeoType, ApiName ve Authentication ([tanım](#metrics-dimensions)) <br/> Değer örneği: 1024 |
| SuccessE2ELatency | Bir depolama hizmetine yapılan başarılı isteklerin veya belirtilen API işleminin ortalama uçtan uca gecikme süresi. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir. <br/><br/> Birim: milisaniye <br/> Toplama türü: Ortalama <br/> Geçerli Boyutlar: GeoType, ApiName ve Authentication ([tanım](#metrics-dimensions)) <br/> Değer örneği: 1024 |
| Kullanılabilirlik | Depolama hizmeti için kullanılabilirlik yüzdesi veya belirtilen API işlemi. Kullanılabilirlik, toplam faturalandırılabilir istek değerinin beklenmeyen hata üreten istekler dahil olmak üzere ilgili istek sayısına bölünmesiyle hesaplanır. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır. <br/><br/> Birim: yüzde <br/> Toplama türü: Ortalama <br/> Geçerli Boyutlar: GeoType, ApiName ve Authentication ([tanım](#metrics-dimensions)) <br/> Değer örneği: 99,99 |

<a id="metrics-dimensions"></a>

## <a name="metrics-dimensions"></a>Ölçüm boyutları

Azure depolama, Azure Izleyici 'de ölçümler için aşağıdaki boyutları destekler.

| Boyut adı | Açıklama |
| ------------------- | ----------------- |
| **BlobType** | Yalnızca blob ölçümleri için blob türü. Desteklenen değerler **Blockblob**, **pageblob**ve **Azure Data Lake Storage**. Ekleme Blobları **Blockblob**'a dahildir. |
| **BlobTier** | Azure depolama, blob nesne verilerini en düşük maliyetli biçimde depolamanıza olanak sağlayan farklı erişim katmanları sunar. Bkz. [Azure Storage blob katmanında](../blobs/storage-blob-storage-tiers.md)daha fazla bilgi. Desteklenen değerler şunlardır: <br/> <li>**Sık**erişimli: etkin katman</li> <li>**Cool**: Cool Tier</li> <li>**Arşiv**: Arşiv katmanı</li> <li>**Premium**: Blok Blobu için Premium katman</li> <li>**P4/P6/P10/P15/P20/P30/P40/P50/P60**: Premium Sayfa Blobu için katman türleri</li> <li>**Standart**: standart sayfa blobu için katman türü</li> <li>**Katmanlanmadan**: genel amaçlı v1 depolama hesabı için katman türü</li> |
| **GeoType** | Birincil veya Ikincil kümeden işlem. Kullanılabilir değerler **PRIMARY** ve **Secondary**' i içerir. İkincil kiracının nesnelerini okurken Okuma Erişimli Coğrafi olarak yedekli depolama (RA-GRS) için geçerlidir. |
| **ResponseType** | İşlem yanıt türü. Kullanılabilir değerler şunlardır: <br/><br/> <li>**ServerOtherError**: Açıklananlar hariç diğer tüm sunucu tarafı hatalar </li> <li>**ServerBusyError**: HTTP 503 durum kodu döndüren, kimliği doğrulanmış istek. </li> <li>**ServerTimeoutError**: HTTP 500 durum kodu döndüren, zaman aşımına uğramış ve kimliği doğrulanmış istek. Zaman aşımı bir sunucu hatası nedeniyle gerçekleşti. </li> <li>**AuthorizationError**: Yetkisiz veri erişimi veya yetkilendirme hatası nedeniyle başarısız olmuş bir kimliği doğrulanmış istek. </li> <li>**NetworkError**: Ağ hataları nedeniyle başarısız olmuş bir kimliği doğrulanmış istek. Çoğunlukla bir istemci, zaman aşımı süre sonundan önce bağlantıyı erkenden kapattığında gerçekleşir. </li><li>**ClientAccountBandwidthThrottlingError**: istek, [depolama hesabı ölçeklenebilirlik sınırlarını](scalability-targets-standard-account.md)aşan bant genişliği üzerinde kısıtlandı.</li><li>**Clientaccountrequestazaltıngerror**: istek, [depolama hesabı ölçeklenebilirlik sınırlarını](scalability-targets-standard-account.md)aşmamak için istek hızında kısıtlanıyor.<li>**Clientkısıtlar Lingerror**: diğer istemci tarafı azaltma hatası. ClientAccountBandwidthThrottlingError ve Clientaccountrequestkısıtlar Lingerror hariç tutulur.</li> <li>**ClientTimeoutError**: HTTP 500 durum kodu döndüren, zaman aşımına uğramış ve kimliği doğrulanmış istek. İstemcinin ağ zaman aşımı veya istek zaman aşımı depolama hizmetinin beklediğinden düşük bir değere ayarlanmışsa beklenen bir zaman aşımıdır. Aksi takdirde, bir ServerTimeoutError olarak bildirilir. </li> <li>**ClientOtherError**: Açıklananlar hariç diğer tüm istemci tarafı hatalar. </li> <li>**Success**: Başarılı istek</li> <li> Başarılı **azaltma**: bir SMB istemcisi ilk denemede azaltılırsa, ancak yeniden denemeler yapıldıktan sonra başarılı olan istek.</li> |
| **ApiName** | İşlemin adı. Örnek: <br/> <li>**CreateContainer**</li> <li>**DeleteBlob**</li> <li>**GetBlob**</li> Tüm işlem adları için bkz. [belge](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages). |
| **Kimlik Doğrulaması** | İşlemlerde kullanılan kimlik doğrulaması türü. Kullanılabilir değerler şunlardır: <br/> <li>**Accountkey**: işlem, depolama hesabı anahtarıyla doğrulanır.</li> <li>**SAS**: işlem, paylaşılan erişim imzaları ile doğrulanır.</li> <li>**OAuth**: Işlem, OAuth erişim belirteçleriyle doğrulanır.</li> <li>**Anonim**: işlem anonim olarak istendi. Bu, ön kontrol isteklerini içermez.</li> <li>**Anonymousbir ön**kontrol: işlem, ön kontrol isteği.</li> |

Boyutları destekleyen ölçümler için, karşılık gelen ölçüm değerlerini görmek için boyut değerini belirtmeniz gerekir. Örneğin, başarılı yanıtlar için **işlem** değerine bakarsanız, **responseType** boyutunu **başarılı**olarak filtrelemeniz gerekir. Ya da Blok Blobu için **Blobcount** değerine bakarsanız, **Blobtype** boyutunu **blockblob**ile filtrelemeniz gerekir.

## <a name="resource-logs-preview"></a>Kaynak günlükleri (Önizleme)

> [!NOTE]
> Azure Izleyici 'de Azure depolama günlükleri genel önizlemede bulunur ve tüm genel bulut bölgelerinde önizleme testi için kullanılabilir. Önizlemeye kaydolmak için [Bu sayfaya](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRxW65f1VQyNCuBHMIMBV8qlUM0E0MFdPRFpOVTRYVklDSE1WUTcyTVAwOC4u)bakın.  Bu önizleme, genel amaçlı v1 ve genel amaçlı v2 depolama hesaplarında blob 'lar (Azure Data Lake Storage 2.), dosyalar, kuyruklar, tablolar, Premium Depolama hesapları için Günlükler sunar. Klasik depolama hesapları desteklenmez.

Aşağıdaki tabloda Azure Izleyici günlüklerinde veya Azure Storage 'da toplandıklarında Azure depolama kaynak günlüklerinin özellikleri listelenmektedir. Özellikler işlemi, hizmeti ve işlemi gerçekleştirmek için kullanılan yetkilendirme türünü anlatmaktadır.

### <a name="fields-that-describe-the-operation"></a>İşlemi tanımlayan alanlar

```json
{
    "time": "2019-02-28T19:10:21.2123117Z",
    "resourceId": "/subscriptions/12345678-2222-3333-4444-555555555555/resourceGroups/mytestrp/providers/Microsoft.Storage/storageAccounts/testaccount1/blobServices/default",
    "category": "StorageWrite",
    "operationName": "PutBlob",
    "operationVersion": "2017-04-17",
    "schemaVersion": "1.0",
    "statusCode": 201,
    "statusText": "Success",
    "durationMs": 5,
    "callerIpAddress": "192.168.0.1:11111",
    "correlationId": "ad881411-201e-004e-1c99-cfd67d000000",
    "location": "uswestcentral",
    "uri": "http://mystorageaccount.blob.core.windows.net/cont1/blobname?timeout=10"
}
```

| Özellik | Açıklama |
|:--- |:---|
|**ışınızda** | İsteğin depolama tarafından alındığı evrensel zaman eşgüdümlü (UTC) saati. Örneğin: `2018/11/08 21:09:36.6900118`.|
|**RESOURCEID** | Depolama hesabının kaynak KIMLIĞI. Örnek: `/subscriptions/208841be-a4v3-4234-9450-08b90c09f4/resourceGroups/`<br>`myresourcegroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount/storageAccounts/blobServices/default`|
|**alan** | İstenen işlemin kategorisi. Örneğin: `StorageRead` , `StorageWrite` , veya `StorageDelete` .|
|**operationName** | Gerçekleştirilen REST işleminin türü. <br> İşlemlerin tüm listesi için bkz. [depolama Analizi günlüğe kaydedilmiş işlemler ve durum iletileri konusu](https://docs.microsoft.com/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages). |
|**operationVersion** | İstek yapıldığında belirtilen depolama hizmeti sürümü. Bu, **x-MS-Version** üst bilgisinin değerine eşdeğerdir. Örneğin: `2017-04-17`.|
|**schemaVersion** | Günlüğün şema sürümü. Örneğin: `1.0`.|
|**statusCode** | İsteğin HTTP durum kodu. İstek kesintiye uğrarsa, bu değer olarak ayarlanabilir `Unknown` . <br> Örnek: `206` |
|**statusText** | İstenen işlemin durumu.  Durum iletilerinin tamamı listesi için bkz. [depolama Analizi günlüğe kaydedilmiş işlemler ve durum iletileri konusu](https://docs.microsoft.com/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages). Sürüm 2017-04-17 ve sonrasında durum iletisi `ClientOtherError` kullanılmaz. Bunun yerine, bu alan bir hata kodu içerir. Örnek: `SASSuccess`  |
|**Ort** | İstenen işlemi gerçekleştirmek için milisaniye olarak ifade edilen toplam süre. Bu, gelen isteği okuma ve yanıtı istek sahibine gönderme süresini içerir. Örneğin: `12`.|
|**callerIpAddress** | İstek sahibinin IP adresi (bağlantı noktası numarası dahil). Örneğin: `192.100.0.102:4362`. |
|**ID** | Kaynaklar arasında günlükleri ilişkilendirmek için kullanılan KIMLIK. Örneğin: `b99ba45e-a01e-0042-4ea6-772bbb000000`. |
|**konumuna** | Depolama hesabının konumu. Örneğin: `North Europe`. |
|**Protocol**|İşlemde kullanılan protokol. Örneğin: `HTTP` ,, `HTTPS` `SMB` , veya`NFS`|
| **kullanılmamışsa** | İstenen Tekdüzen Kaynak tanımlayıcısı. Örneğin: `http://myaccountname.blob.core.windows.net/cont1/blobname?timeout=10`. |

### <a name="fields-that-describe-how-the-operation-was-authenticated"></a>İşlemin nasıl doğrulandığını tanımlayan alanlar

```json
{
    "identity": {
        "authorization": [
            {
                "action": "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read",
                "principals": [
                    {
                        "id": "fde5ba15-4355-4223-b811-cccccccccccc",
                        "type": "User"
                    }
                ],
                "roleAssignmentId": "ecf75cb8-491c-4a25-ad6e-aaaaaaaaaaaa",
                "roleDefinitionId": "b7e6dc6d-f1e8-4753-8033-ffffffffffff"
            }
        ],
        "requester": {
            "appId": "691458b9-1327-4635-9f55-bbbbbbbbbbbb",
            "audience": "https://storage.azure.com/",
            "objectId": "fde5ba15-4355-4223-b811-cccccccccccc",
            "tenantId": "72f988bf-86f1-41af-91ab-dddddddddddd",
            "tokenIssuer": "https://sts.windows.net/72f988bf-86f1-41af-91ab-eeeeeeeeeeee/"
           },
        "type": "OAuth"
    },
}

```

| Özellik | Açıklama |
|:--- |:---|
|**kimlik/tür** | İsteği yapmak için kullanılan kimlik doğrulaması türü. Örneğin: `OAuth` ,, `SAS Key` `Account Key` , veya`Anonymous` |
|**Identity/tokenHash**|Bu alan yalnızca iç kullanım için ayrılmıştır. |
|**Yetkilendirme/eylem** | İsteğe atanan eylem. Örnek: `Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read` |
|**Authorization/Roleatamakimliği** | Rol atama KIMLIĞI. Örneğin: `4e2521b7-13be-4363-aeda-111111111111`.|
|**Yetkilendirme/Roledefinitionıd** | Rol tanımı KIMLIĞI. Örneğin: `ba92f5b4-2d11-453d-a403-111111111111"`.|
|**sorumlular/ID** | Güvenlik sorumlusunun KIMLIĞI. Örneğin: `a4711f3a-254f-4cfb-8a2d-111111111111`.|
|**sorumlular/tür** | Güvenlik sorumlusu türü. Örneğin: `ServicePrincipal`. |
|**istek sahibi/AppID** | İstek sahibi olarak kullanılan Open Authorization (OAuth) uygulama KIMLIĞI. <br> Örneğin: `d3f7d5fe-e64a-4e4e-871d-333333333333`.|
|**istek sahibi/izleyici** | İsteğin OAuth hedef kitlesi. Örneğin: `https://storage.azure.com`. |
|**istek sahibi/ObjectID** | İstek sahibinin OAuth nesne KIMLIĞI. Kerberos kimlik doğrulaması durumunda Kerberos kimliği doğrulanmış kullanıcının nesne tanımlayıcısını temsil eder. Örneğin: `0e0bf547-55e5-465c-91b7-2873712b249c`. |
|**istek sahibi/Tenantıd** | Kimliğin OAuth kiracı KIMLIĞI. Örneğin: `72f988bf-86f1-41af-91ab-222222222222`.|
|**istek sahibi/Tokenıssuer** | OAuth belirteci veren. Örneğin: `https://sts.windows.net/72f988bf-86f1-41af-91ab-222222222222/`.|
|**istek sahibi/UPN** | İstek sahibinin kullanıcı asıl adı (UPN). Örneğin: `someone@contoso.com`. |
|**istek sahibi/Kullanıcı adı** | Bu alan yalnızca iç kullanım için ayrılmıştır.|

### <a name="fields-that-describe-the-service"></a>Hizmeti tanımlayan alanlar

```json
{
    "properties": {
        "accountName": "testaccount1",
        "requestUrl": "https://testaccount1.blob.core.windows.net:443/upload?restype=container&comp=list&prefix=&delimiter=%2F&marker=&maxresults=30&include=metadata&_=1551405598426",
        "userAgentHeader": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/64.0.3282.140 Safari/537.36 Edge/17.17134",
        "referrerHeader": "blob:https://ms.portal.azure.com/6f50025f-3b88-488d-b29e-3c592a31ddc9",
        "clientRequestId": "",
        "etag": "",
        "serverLatencyMs": 63,
        "serviceType": "blob",
        "operationCount": 0,
        "requestHeaderSize": 2658,
        "requestBodySize": 0,
        "responseHeaderSize": 295,
        "responseBodySize": 2018,
        "contentLengthHeader": 0,
        "requestMd5": "",
        "serverMd5": "",
        "lastModifiedTime": "",
        "conditionsUsed": "",
        "smbTreeConnectID" : "0x3",
        "smbPersistentHandleID" : "0x6003f",
        "smbVolatileHandleID" : "0xFFFFFFFF00000065",
        "smbMessageID" : "0x3b165",
        "smbCreditsConsumed" : "0x3",
        "smbCommandDetail" : "0x2000 bytes at offset 0xf2000",
        "smbFileId" : " 0x9223442405598953",
        "smbSessionID" : "0x8530280128000049",
        "smbCommandMajor" : "0x6",
        "smbCommandMinor" : "DirectoryCloseAndDelete"
    }

}
```

| Özellik | Açıklama |
|:--- |:---|
|**Adı** | Depolama hesabının adı. Örneğin: `mystorageaccount`.  |
|**requestUrl** | İstenen URL. Örneğin: `http://mystorageaccount.blob.core.windows.net/cont1/blobname?timeout=10`.|
|**userAgentHeader** | **Kullanıcı Aracısı üst bilgi** değeri, tırnak içinde. Örneğin: `WA-Storage/6.2.0 (.NET CLR 4.0.30319.42000; Win32NT 6.2.9200.0)`.|
|**Referyeniden üstbilgi** | **Başvuran** üst bilgi değeri. Örneğin: `http://contoso.com/about.html`.|
|**Clientrequestıd 'ye sahip** | İsteğin **x-MS-Client-Request-id** üstbilgi değeri. Örneğin: `360b66a6-ad4f-4c4a-84a4-0ad7cb44f7a6`. |
|**özelliği** | Döndürülen nesnenin, tırnak içinde ETag tanımlayıcısı. Örneğin: `0x8D101F7E4B662C4`.  |
|**Sunucu Redms** | İstenen işlemi gerçekleştirmek için milisaniye olarak ifade edilen toplam süre. Bu değer ağ gecikmesini (gelen isteği okumak ve yanıtı istek sahibine göndermek için geçen süre) içermez. Örneğin: `22`. |
|**Türü** | Bu istekle ilişkili hizmet. Örneğin: `blob` ,, `table` `files` veya `queue` . |
|**operationCount** | İstekte bulunan günlüğe kaydedilen her işlemin sayısı. Bu sayı bir diziniyle başlar `0` . Bazı istekler, blob kopyalama isteği gibi birden fazla işlem gerektirir. Çoğu istek yalnızca bir işlem gerçekleştirir. Örneğin: `1`. |
|**requestHeaderSize** | İstek üstbilgisinin boyutu bayt cinsinden ifade edilir. Örneğin: `578`. <br>Bir istek başarısız olursa, bu değer boş olabilir. |
|**requestBodySize** | Depolama hizmeti tarafından okunan, bayt olarak ifade edilen istek paketlerinin boyutu. <br> Örneğin: `0`. <br>Bir istek başarısız olursa, bu değer boş olabilir.  |
|**responseHeaderSize** | Yanıt üstbilgisinin boyutu bayt cinsinden ifade edilir. Örneğin: `216`. <br>Bir istek başarısız olursa, bu değer boş olabilir.  |
|**responseBodySize** | Depolama hizmeti tarafından yazılan Yanıt paketlerinin bayt cinsinden boyutu. Bir istek başarısız olursa, bu değer boş olabilir. Örneğin: `216`.  |
|**requestMd5** | İstekteki **Content-MD5** üst bilgisi veya **x-MS-Content-MD5** üst bilgisi değeri. Bu alanda belirtilen MD5 karma değeri istekteki içeriği temsil eder. Örneğin: `788815fd0198be0d275ad329cafd1830`. <br>Bu alan boş olabilir.  |
|**serverMd5** | Depolama hizmeti tarafından hesaplanan MD5 karmasının değeri. Örneğin: `3228b3cf1069a5489b298446321f8521`. <br>Bu alan boş olabilir.  |
|**Zamanı** | Döndürülen nesnenin son değiştirilme zamanı (LMT).  Örneğin: `Tuesday, 09-Aug-11 21:13:26 GMT`. <br>Birden çok nesne döndüresağlayan işlemler için bu alan boştur. |
|**conditionsUsed** | Bir koşulu temsil eden, anahtar-değer çiftleri için noktalı virgülle ayrılmış bir liste. Koşullar aşağıdakilerden herhangi biri olabilir: <li> If-Modified-Since <li> If-değiştirilmemiş-bu yana <li> IF-Match <li> If-None-Match  <br> Örneğin: `If-Modified-Since=Friday, 05-Aug-11 19:11:54 GMT`. |
|**contentLengthHeader** | Depolama hizmetine gönderilen isteğin Içerik uzunluğu üstbilgisinin değeri. İstek başarılı olduysa, bu değer requestBodySize değerine eşittir. Bir istek başarısız olursa, bu değer requestBodySize eşit olmayabilir veya boş olabilir. |
|**tlsVersion** | İstek bağlantısında kullanılan TLS sürümü. Örneğin: `TLS 1.2`. |
|**Smbtreeconnectıd** | Sunucu Ileti bloğu ( **SMB) ağaç** bağlantı zamanında oluşturulmuştur. Örnek: `0x3` |
|**smbPersistentHandleID** | Ağ yeniden bağlandığında SMB2 oluşturma isteğinden kalıcı tanıtıcı KIMLIĞI.  [MS-SMB2](https://docs.microsoft.com/openspecs/windows_protocols/ms-smb2/f1d9b40d-e335-45fc-9d0b-199a31ede4c3) 2.2.14.1 olarak SMB2_FILEID olarak başvurulur **. Kalıcı**. Örnek: `0x6003f` |
|**Smbvolatilehandleıd** | Ağ yeniden bağlandığında geri dönüştürülecek bir SMB2 CREATE isteğinden geçici tanıtıcı KIMLIĞI.  [MS-SMB2](https://docs.microsoft.com/openspecs/windows_protocols/ms-smb2/f1d9b40d-e335-45fc-9d0b-199a31ede4c3) 2.2.14.1 olarak SMB2_FILEID olarak başvurulur **. Geçici**. Örnek: `0xFFFFFFFF00000065` |
|**Smbmessageıd** | Bağlantı göreli **MessageID**. Örnek: `0x3b165` |
|**Smbalacaklı Tstüketilen** | 64K birimi olarak istek tarafından tüketilen giriş veya çıkış. Örnek: `0x3` |
|**smbCommandDetail** | Genel istek türü yerine bu özel istek hakkında daha fazla bilgi. Örnek: `0x2000 bytes at offset 0xf2000` |
|**Smbfileıd** | Dosya veya dizinle ilişkili **FileId** .  Kabaca bir NTFS FileId 'ye benzer. Örnek: `0x9223442405598953` |
|**Smbsessionıd** | Oturum Kurulum zamanında SMB2 **SessionID** oluşturuldu. Örnek: `0x8530280128000049` |
|**Smbcommandana uint32** | **SMB2_HEADER. Command**içindeki değer. Şu anda bu sayı 0 ile 18 arasında bir sayıdır. Örnek: `0x6` |
|**smbCommandMinor** | Doğru yerde, **Smbcommandana**'nin alt sınıfı. Örnek: `DirectoryCloseAndDelete` |

## <a name="see-also"></a>Ayrıca bkz.

- Bkz. Azure Storage izleme açıklaması için bkz. [Azure Storage](monitor-storage.md) 'ı izleme.
- Azure kaynaklarını izleme hakkında ayrıntılı bilgi için bkz. Azure [izleyici ile Azure kaynaklarını izleme](../../azure-monitor/insights/monitor-azure-resource.md) .