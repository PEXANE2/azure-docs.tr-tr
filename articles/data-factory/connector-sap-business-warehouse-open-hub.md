---
title: Sap Business Warehouse'dan Open Hub üzerinden veri kopyalama
description: Bir Azure Veri Fabrikası ardışık hattında bir kopyalama etkinliği kullanarak desteklenen lavabo veri depolarına Open Hub üzerinden SAP İş Ambarı'ndan (BW) verileri nasıl kopyalayabilirsiniz öğrenin.
services: data-factory
documentationcenter: ''
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 03/24/2020
ms.openlocfilehash: ad7d171cb115729e174090c1c80915abbde5999f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80238739"
---
# <a name="copy-data-from-sap-business-warehouse-via-open-hub-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak Open Hub üzerinden SAP İş Ambarı'ndan veri kopyalama

Bu makalede, Açık Hub üzerinden bir SAP İş Ambarı'ndan (BW) veri kopyalamak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Kopyalama etkinliğine genel bir genel bakış sunan [kopyalama etkinliğine genel bakış](copy-activity-overview.md) makalesi üzerine inşa edin.

>[!TIP]
>Sap veri tümleştirme senaryosunda ADF'nin genel desteğini öğrenmek için, ayrıntılı giriş, karşılaştırma ve kılavuzlu [Azure Veri Fabrikası teknik incelemesini kullanarak SAP veri tümleştirmesine](https://github.com/Azure/Azure-DataFactory/blob/master/whitepaper/SAP%20Data%20Integration%20using%20Azure%20Data%20Factory.pdf) bakın.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Open Hub konektörü üzerinden bu SAP İş Ambarı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matrisi](copy-activity-overview.md) ile [etkinliği](copy-activity-overview.md) kopyalama
- [Arama etkinliği](control-flow-lookup-activity.md)

SAP İş Ambarı'ndaki verileri Open Hub üzerinden desteklenen herhangi bir lavabo veri deposuna kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak/lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Özellikle, bu SAP İş Ambarı Açık Hub bağlayıcısı destekler:

- SAP Business Warehouse **sürümü 7.01 veya üzeri (2015 yılından sonra yayımlanan son SAP Destek Paketi Yığını'nda)**.
- Altında DSO, InfoCube, MultiProvider, DataSource, vb olabilir Open Hub Hedef yerel tablo üzerinden veri kopyalama
- Temel kimlik doğrulamasını kullanarak verileri kopyalama.
- Application Server'a bağlanma.

## <a name="sap-bw-open-hub-integration"></a>SAP BW Açık Hub Tümleştirmesi 

[SAP BW Open Hub Hizmeti,](https://wiki.scn.sap.com/wiki/display/BI/Overview+of+Open+Hub+Service) SAP BW'den veri ayıklamanın etkili bir yoludur. Aşağıdaki diyagram, müşterilerin SAP sisteminde sahip olduğu tipik akışlardan birini gösterir ve bu durumda sap ECC -> PSA -> DSO -> Küpü'nden veri akışı sağlar.

SAP BW Açık Hub Hedefi (OHD), SAP verilerinin aktarıldığı hedefi tanımlar. SAP Veri Aktarım Süreci (DTP) tarafından desteklenen tüm nesneler, DSO, InfoCube, DataSource gibi açık hub veri kaynakları olarak kullanılabilir. Aktarılan verilerin depolandığı Hub Hedef türünü açın veritabanı tabloları (yerel veya uzak) ve düz dosyalar olabilir. Bu SAP BW Open Hub konektörü, BW'deki OHD yerel tablosundan veri kopyalamayı destekler. Diğer türleri kullanıyorsanız, diğer bağlayıcıları kullanarak doğrudan veritabanına veya dosya sistemine bağlanabilirsiniz.

![SAP BW Açık Hub](./media/connector-sap-business-warehouse-open-hub/sap-bw-open-hub.png)

## <a name="delta-extraction-flow"></a>Delta çıkarma akışı

ADF SAP BW Open Hub Bağlayıcısı `excludeLastRequest` `baseRequestId` iki isteğe bağlı özellik sunar: ve Open Hub'dan delta yükünü işlemek için kullanılabilir. 

- **excludeLastRequestId**: Son isteğin kayıtlarının hariç tutup tutmaması. Varsayılan değer doğrudur. 
- **baseRequestId**: Delta yükleme isteğinin kimliği. Ayarlandıktan sonra, yalnızca requestId ile bu özelliğin değerinden daha büyük olan veriler alınır. 

Genel olarak, SAP Bilgi Sağlayıcıları'ndan Azure Veri Fabrikası'na (ADF) çıkarma işlemi 2 adımdan oluşur: 

1. **SAP BW Veri Aktarım Süreci (DTP)** Bu adım, verileri SAP BW InfoProvider'dan sap BW Açık Hub tablosuna kopyalar 

1. **ADF veri kopyası** Bu adımda, Aç Hub tablosu ADF Bağlayıcısı tarafından okunur 

![Delta çıkarma akışı](media/connector-sap-business-warehouse-open-hub/delta-extraction-flow.png)

İlk adımda, bir DTP yürütülür. Her yürütme yeni bir SAP istek kimliği oluşturur. İstek kimliği Açık Hub tablosunda depolanır ve daha sonra Delta'yı tanımlamak için ADF bağlayıcısı tarafından kullanılır. İki adım eş senkronize çalışır: DTP SAP tarafından tetiklenir ve ADF veri kopyası ADF aracılığıyla tetiklenir. 

Varsayılan olarak, ADF Açık Hub tablosundan en son deltayı okumuyor ("son isteği hariç tutma" seçeneği doğrudur). Bu nedenle, ADF'deki veriler Açık Hub tablosundaki verilerle %100 güncel değildir (son delta eksiktir). Buna karşılık, bu yordam, asynchronous ekstraksiyon nedeniyle hiçbir satır kaybolmak sağlar. DTP hala aynı tabloya yazarken ADF Açık Hub tablosunu okurken bile iyi çalışır. 

Genellikle en üst kopyalanan istek kimliğini adf tarafından son çalıştırmada bir hazırlama veri deposunda (yukarıdaki diyagramdaki Azure Blob gibi) depolarsınız. Bu nedenle, aynı istek sonraki çalıştırmada ADF tarafından ikinci kez okunmaz. Bu arada, verilerin Açık Hub tablosundan otomatik olarak silinmediğini unutmayın.

Uygun delta işleme için, aynı Açık Hub tablosunda farklı DTP'lerden istek dislerinin olmasına izin verilmez. Bu nedenle, her Açık Hub Hedefi (OHD) için birden fazla DTP oluşturmamalısınız. Aynı InfoProvider'dan Tam ve Delta çıkarma gereksinimi duyduğunuzda, aynı InfoProvider için iki OHD oluşturmanız gerekir. 

## <a name="prerequisites"></a>Ön koşullar

Bu SAP İş Ambarı Açık Hub konektörünü kullanmak için şunları yapmanız gerekir:

- Sürüm 3.13 veya üzeri olan Kendi kendine barındırılan Bir Tümleştirme Çalışma Süresi ayarlayın. Ayrıntılar için [Kendi barındırılan Tümleştirme Çalışma Zamanı](create-self-hosted-integration-runtime.md) makalesine bakın.

- SAP'nin web sitesinden **64 bit [SAP .NET Bağlayıcı 3.0'ı](https://support.sap.com/en/product/connectors/msnet.html) ** indirin ve kendi barındırılan IR makinesine yükleyin. Yükleme yaparken, isteğe bağlı kurulum adımları penceresinde, aşağıdaki resimde gösterildiği gibi **GAC'ye Derlemeleri Yükle** seçeneğini seçtiğinizden emin olun. 

    ![SAP .NET Konektörü Kurutun](./media/connector-sap-business-warehouse-open-hub/install-sap-dotnet-connector.png)

- Veri Fabrikası BW konektöründe kullanılan SAP kullanıcısının aşağıdaki izinlere sahip olması gerekir: 

    - RFC ve SAP BW için yetkilendirme. 
    - Yetkilendirme Nesnesi "S_SDSAUTH" "Yürütme" Etkinliği için izinler.

- "Teknik Anahtar" seçeneği işaretli **Veritabanı Tablosu** olarak SAP Open Hub Hedef türünü oluşturun.  Ayrıca, gerekli olmasa da Tablodan Veri Silme'yi işaretsiz olarak bırakmaları da önerilir. Seçtiğiniz kaynak nesneden (küp gibi) veri elde etmek için DTP'den (doğrudan yürütme veya varolan işlem zincirine tümleştirme) yararlanın.

## <a name="getting-started"></a>Başlarken

> [!TIP]
>
> SAP BW Open Hub bağlayıcısı kullanma nın bir bölümü [için, Azure Veri Fabrikası'nı kullanarak SAP İş Ambarı'ndan (BW) gelen Yük verilerini](load-sap-bw-data.md)görün.

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, SAP İş Ambarı Açık Hub bağlayıcısına özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Sap Business Warehouse Open Hub bağlantılı hizmet için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği şu şekilde ayarlanmalıdır: **SapOpenHub** | Evet |
| sunucu | SAP BW örneğinin bulunduğu sunucunun adı. | Evet |
| systemNumber | SAP BW sisteminin sistem numarası.<br/>İzin verilen değer: dize olarak temsil edilen iki basamaklı ondalık sayı. | Evet |
| clientId | SAP W sistemindeki istemci kimliği.<br/>İzin verilen değer: dize olarak temsil edilen üç basamaklı ondalık sayı. | Evet |
| language | SAP sisteminin kullandığı dil. | Hayır (varsayılan **EN**değer EN'dir)|
| userName | SAP sunucusuna erişimi olan kullanıcının adı. | Evet |
| password | Kullanıcının parolası. Bu alanı, Veri Fabrikası'nda güvenli bir şekilde depolamak için SecureString olarak işaretleyin veya [Azure Key Vault'ta depolanan bir gizliye başvurun.](store-credentials-in-key-vault.md) | Evet |
| connectVia | Veri deposuna bağlanmak için kullanılacak [Tümleştirme Çalışma Süresi.](concepts-integration-runtime.md) [Önkoşullarda](#prerequisites)belirtildiği gibi Kendi kendine barındırılan Tümleştirme Çalışma Süresi gereklidir. |Evet |

**Örnek:**

```json
{
    "name": "SapBwOpenHubLinkedService",
    "properties": {
        "type": "SapOpenHub",
        "typeProperties": {
            "server": "<server name>",
            "systemNumber": "<system number>",
            "clientId": "<client id>",
            "userName": "<SAP user>",
            "password": {
                "type": "SecureString",
                "value": "<Password for SAP user>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [Datasets](concepts-datasets-linked-services.md) makalesine bakın. Bu bölümde SAP BW Open Hub veri kümesi tarafından desteklenen özelliklerin bir listesi sağlar.

Verileri SAP BW Open Hub'dan kopyalamak için, veri kümesinin tür özelliğini **SapOpenHubTable**olarak ayarlayın. Aşağıdaki özellikler desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği **SapOpenHubTable**olarak ayarlanmalıdır.  | Evet |
| openHubDestinationName | Verileri kopyalamak için Açık Hub Hedefi'nin adı. | Evet |

Yeni modeli `excludeLastRequest` ileriye `baseRequestId` dönük olarak etkinlik kaynağında kullanmanız önerilirken, ayarlarve veri kümesinde hala olduğu gibi desteklenir.

**Örnek:**

```json
{
    "name": "SAPBWOpenHubDataset",
    "properties": {
        "type": "SapOpenHubTable",
        "typeProperties": {
            "openHubDestinationName": "<open hub destination name>"
        },
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<SAP BW Open Hub linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi [için, Pipelines](concepts-pipelines-activities.md) makalesine bakın. Bu bölümde SAP BW Open Hub kaynağı tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="sap-bw-open-hub-as-source"></a>KAYNAK olarak SAP BW Open Hub

SAP BW Open Hub'daki verileri kopyalamak için, kopyalama etkinliği **kaynak** bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinlik kaynağının **türü** özelliği **SapOpenHubSource**olarak ayarlanmalıdır. | Evet |
| dışlamaLastRequest | Son isteğin kayıtlarının hariç tutup tutmayacağı. | Hayır (varsayılan **değer doğrudur)** |
| baseRequestId | Delta yükleme isteğinin kimliği. Ayarlandıktan sonra, yalnızca requestId ile bu özelliğin değerinden **daha büyük** olan veriler alınır.  | Hayır |

>[!TIP]
>Açık Hub tablonuz yalnızca tek istek kimliği yle oluşturulan verileri içeriyorsa, örneğin, tablodaki varolan verileri her zaman tam olarak yükleyip üzerine yazarsınız veya DTP'yi yalnızca bir kez test için çalıştırın, kopyalamak için "dışlamaSonİstek" seçeneğinin işaretlerini geri yüklemeyi unutmayın veri dışarı.

Veri yüklemesini hızlandırmak için, [`parallelCopies`](copy-activity-performance.md#parallel-copy) SAP BW Open Hub'dan verileri paralel olarak yüklemek için kopyalama etkinliğini ayarlayabilirsiniz. Örneğin, dörde `parallelCopies` ayarlarsanız, Veri Fabrikası aynı anda dört RFC çağrısı yürütür ve her RFC çağrısı, DTP istek kimliği ve paket kimliği yle bölümlenen SAP BW Açık Hub tablonuzdan verilerin bir kısmını alır. Bu, benzersiz DTP istek kimliği + paket kimliği nin değerinden büyük olduğunda `parallelCopies`geçerlidir. Verileri dosya tabanlı veri deposuna kopyalarken, bir klasöre birden çok dosya olarak yazmak (yalnızca klasör adını belirtin) komutu da verilir, bu durumda performans tek bir dosyaya yazmaktan daha iyidir.

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromSAPBWOpenHub",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SAP BW Open Hub input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "SapOpenHubSource",
                "excludeLastRequest": true
            },
            "sink": {
                "type": "<sink type>"
            },
            "parallelCopies": 4
        }
    }
]
```

## <a name="data-type-mapping-for-sap-bw-open-hub"></a>SAP BW Open Hub için veri türü eşleme

SAP BW Open Hub'dan veri kopyalanırken, SAP BW veri türlerinden Azure Veri Fabrikası geçici veri türlerine aşağıdaki eşlemeler kullanılır. Kopya etkinliği kaynak şemasını ve veri türünü lavaboyla nasıl eşler hakkında bilgi edinmek için Şema ve [veri türü eşlemelerine](copy-activity-schema-and-type-mapping.md) bakın.

| SAP ABAP Tipi | Veri fabrikası geçici veri türü |
|:--- |:--- |
| C (Dize) | Dize |
| I (sonda) | Int32 |
| F (Şamandıra) | Çift |
| D (Tarih) | Dize |
| T (Zaman) | Dize |
| P (BCD Paketlenmiş, Para Birimi, Ondalık, Qty) | Ondalık |
| N (Numc) | Dize |
| X (İkili ve Ham) | Dize |

## <a name="lookup-activity-properties"></a>Arama etkinlik özellikleri

Özellikler hakkında daha fazla bilgi edinmek için [Arama etkinliğini](control-flow-lookup-activity.md)kontrol edin.

## <a name="troubleshooting-tips"></a>Sorun giderme ipuçları

**Belirtileri:** HANA'da SAP BW çalıştırıyorsanız ve yalnızca veri alt kümesinin ADF kopyalama etkinliği (1 milyon satır) kullanılarak kopyalandığı gözlemliyorsanız, olası neden DTP'nizde "SAP HANA Yürütme" seçeneğini etkinleştirmenizdir ve bu durumda ADF yalnızca ilk veri kümesini alabilir.

**Çözünürlük:** DTP'de "SAP HANA Yürütme" seçeneğini devre dışı, verileri yeniden işleyin ve ardından kopyalama etkinliğini yeniden yürütmeyi deneyin.

## <a name="next-steps"></a>Sonraki adımlar
Azure Veri Fabrikası'ndaki kopyalama etkinliği tarafından kaynak ve lavabo olarak desteklenen veri depolarının listesi için [desteklenen veri depolarına](copy-activity-overview.md#supported-data-stores-and-formats)bakın.
