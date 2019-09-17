---
title: Azure Data Factory kullanarak Dynamics CRM veya Dynamics 365 'den (Common Data Service) veri kopyalama | Microsoft Docs
description: Veri Fabrikası ardışık düzeninde bir kopyalama etkinliği kullanarak Microsoft Dynamics CRM 'den veya Microsoft Dynamics 365 ' den (Common Data Service) desteklenen havuz veri depolarına veya desteklenen kaynak veri depolarından, Dynamics CRM veya Dynamics 365 ' e veri kopyalamayı öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 07/01/2019
ms.author: jingwang
ms.openlocfilehash: 18fdb14430eee97ff2780d963abf3e5ceafe1126
ms.sourcegitcommit: a819209a7c293078ff5377dee266fa76fd20902c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2019
ms.locfileid: "71009390"
---
# <a name="copy-data-from-and-to-dynamics-365-common-data-service-or-dynamics-crm-by-using-azure-data-factory"></a>Ve Azure Data Factory kullanarak Dynamics 365 (Common Data Service) veya Dynamics CRM 'den veri kopyalama

Bu makalede, Microsoft Dynamics 365 veya Microsoft Dynamics CRM 'den verileri kopyalamak için Azure Data Factory kopyalama etkinliğinin nasıl kullanılacağı özetlenmektedir. Yapılar [kopyalama etkinliğine genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Bu bağlayıcı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/havuz matrisi](copy-activity-overview.md) ile [kopyalama etkinliği](copy-activity-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)

Dynamics 365 (Common Data Service) veya Dynamics CRM 'den, desteklenen herhangi bir havuz veri deposuna veri kopyalayabilirsiniz. Ayrıca, desteklenen herhangi bir kaynak veri deposundan verileri Dynamics 365 (Common Data Service) veya Dynamics CRM 'ye kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak veya havuz olarak desteklenen veri depolarının listesi için [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Bu Dynamics Connector, hem çevrimiçi hem de şirket içi Dynamics sürüm 7. x-9. x ' i destekler. Daha özel olarak,

- Sürüm 7. x, Dynamics CRM 2015 ile eşlenir
- Sürüm 8. x, Dynamics CRM 2016 ve önceki Dynamics 365 sürümüne eşlenir
- Sürüm 9. x, Dynamics 365 'in sonraki sürümüyle eşlenir

Karşılık gelen Dynamics sürümleri/ürünleri için desteklenen kimlik doğrulama türleri ve yapılandırmalarında aşağıdaki tabloya bakın. (IFD, internet 'e yönelik dağıtım için kısadır.)

| Dynamics sürümleri | Kimlik doğrulama türleri | Bağlı hizmet örnekleri |
|:--- |:--- |:--- |
| Dynamics 365 çevrimiçi <br> Dynamics CRM Online | Office365 | [Dynamics Online + Office365 auth](#dynamics-365-and-dynamics-crm-online) |
| Şirket içinde ıFD ile Dynamics 365 <br> Azure 'da ıFD ile Dynamics CRM 2016 <br> Azure 'da ıFD ile Dynamics CRM 2015 | UYGULAMASINI | [IFD ve ıFD kimlik doğrulaması ile şirket içi Dynamics](#dynamics-365-and-dynamics-crm-on-premises-with-ifd) |

Dynamics 365 özel olarak, aşağıdaki uygulama türleri desteklenir:

- Sales için Dynamics 365
- Müşteri Hizmetleri için Dynamics 365
- Alan hizmeti için Dynamics 365
- Project Service Automation için Dynamics 365
- Pazarlama için Dynamics 365

Finans ve Işlemler, Taödünvs. gibi diğer uygulama türleri bu bağlayıcı tarafından desteklenmez.

Bu Dynamics Connector, [Dynamics XRM araçları](https://docs.microsoft.com/dynamics365/customer-engagement/developer/build-windows-client-applications-xrm-tools)'nın üzerine kurulmuştur.

>[!TIP]
>**Dynamics 365 finans ve işlemlerinden**veri kopyalamak IÇIN [Dynamics AX bağlayıcısını](connector-dynamics-ax.md)kullanabilirsiniz.

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, Dynamics 'e özgü Data Factory varlıkları tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgiler sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Dynamics bağlantılı hizmeti için aşağıdaki özellikler desteklenir.

### <a name="dynamics-365-and-dynamics-crm-online"></a>Dynamics 365 ve Dynamics CRM Online

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği **Dynamics**olarak ayarlanmalıdır. | Evet |
| deploymentType | Dynamics örneğinin dağıtım türü. Dynamics Online için **"çevrimiçi"** olması gerekir. | Evet |
| serviceUri | Dynamics örneğinizin hizmet URL 'SI, ör `https://adfdynamics.crm.dynamics.com`. | Evet |
| authenticationType | Bir Dynamics sunucusuna bağlanmak için kimlik doğrulaması türü. Dynamics Online için **"Office365"** belirtin. | Evet |
| username | Dynamics 'e bağlanmak için Kullanıcı adını belirtin. | Evet |
| password | Kullanıcı adı için belirttiğiniz kullanıcı hesabının parolasını belirtin. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Evet |
| connectVia | [Integration runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. | Kaynak için Hayır, kaynak bağlı hizmetin bir tümleştirme çalışma zamanı yoksa, havuz için Evet |

>[!NOTE]
>Dynamics Connector, Dynamics CRM/365 çevrimiçi örneğinizi tanımlamak için isteğe bağlı "Organizasyonadi" özelliğini kullanmak için kullanılır. Çalışmaya devam ederken, örnek bulma için daha iyi performans kazanmak üzere yeni "serviceUri" özelliğini belirtmeniz önerilir.

**Örnek: Office365 kimlik doğrulaması kullanarak Dynamics Online**

```json
{
    "name": "DynamicsLinkedService",
    "properties": {
        "type": "Dynamics",
        "description": "Dynamics online linked service using Office365 authentication",
        "typeProperties": {
            "deploymentType": "Online",
            "serviceUri": "https://adfdynamics.crm.dynamics.com",
            "authenticationType": "Office365",
            "username": "test@contoso.onmicrosoft.com",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="dynamics-365-and-dynamics-crm-on-premises-with-ifd"></a>Dynamics 365 ve şirket içi Dynamics CRM 'yi ıFD ile

*Dynamics Online ile karşılaştırılan ek özellikler şunlardır. "ana bilgisayar adı" ve "bağlantı noktası".*

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği **Dynamics**olarak ayarlanmalıdır. | Evet |
| deploymentType | Dynamics örneğinin dağıtım türü. Bu, ıFD ile Dynamics şirket içi için **"OnPremisesWithIfd"** olmalıdır.| Evet |
| hostName | Şirket içi Dynamics sunucusunun ana bilgisayar adı. | Evet |
| port | Şirket içi Dynamics sunucusunun bağlantı noktası. | Hayır, varsayılan değer 443 ' dir |
| organizationName | Dynamics örneğinin kuruluş adı. | Evet |
| authenticationType | Dynamics sunucusuna bağlanmak için kimlik doğrulaması türü. IFD ile Dynamics şirket içi için **"IFD"** belirtin. | Evet |
| username | Dynamics 'e bağlanmak için Kullanıcı adını belirtin. | Evet |
| password | Kullanıcı adı için belirttiğiniz kullanıcı hesabının parolasını belirtin. Bu alan ADF içinde güvenli bir şekilde depolayın veya Azure anahtar Kasası'nda parolayı depolamak için bir SecureString olarak işaretlemek seçin ve veri kopyalama gerçekleştirirken buradan çekme - daha fazla bilgi için kopyalama etkinliği izin [Key Vault'nda kimlik bilgileri Store](store-credentials-in-key-vault.md). | Evet |
| connectVia | [Integration runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. | Kaynak, havuz için Evet Hayır |

**Örnek: IFD kimlik doğrulaması kullanarak ıFD ile şirket içi Dynamics**

```json
{
    "name": "DynamicsLinkedService",
    "properties": {
        "type": "Dynamics",
        "description": "Dynamics on-premises with IFD linked service using IFD authentication",
        "typeProperties": {
            "deploymentType": "OnPremisesWithIFD",
            "hostName": "contosodynamicsserver.contoso.com",
            "port": 443,
            "organizationName": "admsDynamicsTest",
            "authenticationType": "Ifd",
            "username": "test@contoso.onmicrosoft.com",
            "password": {
                "type": "SecureString",
                "value": "<password>"
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

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölüm, Dynamics veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Ve Dynamics 'ten verileri kopyalamak için, veri kümesinin Type özelliğini **Dynamicsentity**olarak ayarlayın. Aşağıdaki özellikler desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin Type özelliği **Dynamicsentity**olarak ayarlanmalıdır. |Evet |
| entityName | Alınacak varlığın mantıksal adı. | Kaynak için Hayır (etkinlik kaynağında "sorgu" belirtilmişse), havuz için Evet |

> [!IMPORTANT]
>- Dynamics 'ten veri kopyaladığınızda, "yapı" bölümü isteğe bağlıdır, ancak bu durum, belirleyici bir kopya sonucu sağlamak için Dynamics veri kümesinde yüksek oranda yeniden belirlenir. Bu, üzerine kopyalamak istediğiniz Dynamics verileri için sütun adını ve veri türünü tanımlar. Daha fazla bilgi edinmek için bkz. Dynamics için veri [kümesi yapısı](concepts-datasets-linked-services.md#dataset-structure-or-schema) ve [veri türü eşleme](#data-type-mapping-for-dynamics).
>- Yazar Kullanıcı arabiriminde şemayı içeri aktarırken, ADF, yapı oluşumunu başlatmak için Dynamics sorgu sonucundan en üstteki satırları örnekleyerek şemayı çıkarmaz, bu durumda hiçbir değer içermeyen sütunlar atlanır. Açık yapı tanımı yoksa, aynı davranış yürütmeleri kopyalama için de geçerlidir. İsterseniz Dynamics veri kümesi şemasına/yapısına daha fazla sütun ekleyebilir ve bunları kopyalama çalışma zamanı sırasında bu şekilde kabul edebilirsiniz.
>- Dynamics 'e veri kopyaladığınızda, Dynamics veri kümesinde "yapı" bölümü isteğe bağlıdır. İçine kopyalanacak sütunlar, kaynak veri şeması tarafından belirlenir. Kaynağınız, üst bilgisi olmayan bir CSV dosyası ise, giriş veri kümesinde, sütun adı ve veri türü ile birlikte "Structure" öğesini belirtin. CSV dosyasındaki alanlarla tek tek sırayla eşlenir.

**Örnek:**

```json
{
    "name": "DynamicsDataset",
    "properties": {
        "type": "DynamicsEntity",
        "structure": [
            {
                "name": "accountid",
                "type": "Guid"
            },
            {
                "name": "name",
                "type": "String"
            },
            {
                "name": "marketingonly",
                "type": "Boolean"
            },
            {
                "name": "modifiedon",
                "type": "Datetime"
            }
        ],
        "typeProperties": {
            "entityName": "account"
        },
        "linkedServiceName": {
            "referenceName": "<Dynamics linked service name>",
            "type": "linkedservicereference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölüm, Dynamics kaynak ve havuz türleri tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="dynamics-as-a-source-type"></a>Kaynak türü olarak Dynamics

Dynamics 'ten veri kopyalamak için kopyalama etkinliğindeki kaynak türünü **Dynamicssource**olarak ayarlayın. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağının Type özelliği **Dynamicssource**olarak ayarlanmalıdır. | Evet |
| query | FetchXML, Dynamics 'te (çevrimiçi ve şirket içi) kullanılan özel bir sorgu dilidir. Aşağıdaki örneğe bakın. Daha fazla bilgi için bkz. [FetchXML Ile derleme sorguları](https://msdn.microsoft.com/library/gg328332.aspx). | Hayır (veri kümesindeki "entityName" belirtilmişse) |

>[!NOTE]
>FetchXML sorgusunda yapılandırdığınız sütun projeksiyonu içermediği halde, PK sütunu her zaman gönderilir.

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromDynamics",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Dynamics input dataset>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "DynamicsSource",
                "query": "<FetchXML Query>"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="sample-fetchxml-query"></a>Örnek FetchXML sorgusu

```xml
<fetch>
  <entity name="account">
    <attribute name="accountid" />
    <attribute name="name" />
    <attribute name="marketingonly" />
    <attribute name="modifiedon" />
    <order attribute="modifiedon" descending="false" />
    <filter type="and">
      <condition attribute ="modifiedon" operator="between">
        <value>2017-03-10 18:40:00z</value>
        <value>2017-03-12 20:40:00z</value>
      </condition>
    </filter>
  </entity>
</fetch>
```

### <a name="dynamics-as-a-sink-type"></a>Havuz türü olarak Dynamics

Verileri Dynamics 'e kopyalamak için kopyalama etkinliğindeki havuz türünü **Dynamicssink**olarak ayarlayın. Aşağıdaki özellikler, etkinlik **havuzunu** Kopyala bölümünde desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği havuzunun Type özelliği **Dynamicssink**olarak ayarlanmalıdır. | Evet |
| writeBehavior | İşlemin yazma davranışı.<br/>İzin verilen değer **"upsert"** . | Evet |
| writeBatchSize | Her toplu işte Dynamics 'e yazılan verilerin satır sayısı. | Hayır (varsayılan değer 10 ' dur) |
| ıgnorenullvalues | Bir yazma işlemi sırasında giriş verilerinden (anahtar alanları hariç) null değerlerin yoksayılıp yoksayılmayacağını gösterir.<br/>İzin verilen değerler **true** ve **false**şeklindedir.<br>- **Doğru**: Bir upsert/Update işlemi gerçekleştirdiğinizde verileri hedef nesnede değiştirmeden bırakın. Ekleme işlemi yaparken tanımlanmış bir varsayılan değer ekleyin.<br/>- **Yanlış**: Bir upsert/Update işlemi gerçekleştirdiğinizde hedef nesnesindeki verileri NULL olarak güncelleştirin. Ekleme işlemi yaparken NULL değer ekleyin. | Hayır (varsayılan değer false) |

>[!NOTE]
>"**Writebatchsize**" havuzunun varsayılan değeri ve Dynamics havuzu için kopyalama etkinliğinin " **[parallelcopy](copy-activity-performance.md#parallel-copy)** " değeri 10 ' dur. Bu nedenle, 100 kayıt aynı anda Dynamics 'e gönderilir.

Dynamics 365 Online için, [kuruluş başına 2 eşzamanlı toplu iş çağrısı](https://msdn.microsoft.com/library/jj863631.aspx#Run-time%20limitations)sınırı vardır. Bu sınır aşılırsa, ilk istek yürütülmeden önce bir "sunucu meşgul" hatası oluşturulur. "WriteBatchSize" öğesinin daha az veya 10 ' a eşit tutulması, eşzamanlı çağrıların bu şekilde azaltılmasını önler.

"**Writebatchsize**" ve "**parallelkopyaların**" en iyi birleşimi, varlığınızın şemasına bağlıdır, örneğin sütun sayısı, satır boyutu, eklentiler/iş akışları/iş akışı etkinliklerinin sayısı, bu çağrılara bağlanır, vb. 10 writeBatchSize * 10 Parallelkopyaların varsayılan ayarı, Dynamics Service 'e göre öneri, ancak çoğu Dynamics varlık için de en iyi performansa sahip olmayabilir. Kopyalama etkinliği ayarlarınızda birleşimi ayarlayarak performansı ayarlayabilirsiniz.

**Örnek:**

```json
"activities":[
    {
        "name": "CopyToDynamics",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Dynamics output dataset>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "DynamicsSink",
                "writeBehavior": "Upsert",
                "writeBatchSize": 10,
                "ignoreNullValues": true
            }
        }
    }
]
```

## <a name="data-type-mapping-for-dynamics"></a>Dynamics için veri türü eşlemesi

Verileri Dynamics 'ten kopyaladığınızda, Dynamics veri türlerinden aşağıdaki eşlemeler, geçici veri türleri Data Factory için kullanılır. Kopyalama etkinliğinin kaynak şemayı ve veri türünü havuza nasıl eşlediğini öğrenmek için bkz. [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md).

Aşağıdaki eşleme tablosunu kullanarak kaynak Dynamics veri türüne göre bir veri kümesi yapısında karşılık gelen Data Factory veri türünü yapılandırın.

| Dynamics veri türü | Veri Fabrikası geçici veri türü | Kaynak olarak desteklenir | Havuz olarak destekleniyor |
|:--- |:--- |:--- |:--- |
| AttributeTypeCode. BigInt | Uzun | ✓ | ✓ |
| AttributeTypeCode. Boolean | Boole değeri | ✓ | ✓ |
| AttributeType. müşterisi | Guid | ✓ | |
| AttributeType. DateTime | Datetime | ✓ | ✓ |
| AttributeType. Decimal | Decimal | ✓ | ✓ |
| AttributeType. Double | Double | ✓ | ✓ |
| AttributeType. EntityName | Dize | ✓ | ✓ |
| AttributeType. Integer | Int32 | ✓ | ✓ |
| AttributeType. Lookup | Guid | ✓ | ✓ (tek hedefle ilişkili) |
| AttributeType. ManagedProperty | Boole değeri | ✓ | |
| AttributeType. memo | Dize | ✓ | ✓ |
| AttributeType. para | Decimal | ✓ | ✓ |
| AttributeType. Owner | Guid | ✓ | |
| AttributeType. seçim listesi | Int32 | ✓ | ✓ |
| AttributeType. uniqueidentifier | Guid | ✓ | ✓ |
| AttributeType. String | Dize | ✓ | ✓ |
| AttributeType. State | Int32 | ✓ | ✓ |
| AttributeType. Status | Int32 | ✓ | ✓ |

> [!NOTE]
> AttributeType. CalendarRules, AttributeType. Multiselectseçim listesi ve AttributeType. PartyList Dynamics veri türleri desteklenmez.

## <a name="lookup-activity-properties"></a>Arama etkinliği özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [arama etkinliğini](control-flow-lookup-activity.md)denetleyin.

## <a name="next-steps"></a>Sonraki adımlar
Veri fabrikasında kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
