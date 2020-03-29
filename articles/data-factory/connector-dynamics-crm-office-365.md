---
title: Dinamikler'de veri kopyalama (Ortak Veri Hizmeti)
description: Desteklenen lavabo veri depolarına veya desteklenen kaynak veri depolarından Dynamics CRM veya Dynamics 365'e kadar olan verileri veri fabrikasındaki bir kopyalama etkinliğini kullanarak nasıl kopyalayış edilebildiğini öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.custom: seo-lt-2019
ms.date: 11/20/2019
ms.openlocfilehash: d065439839ba5db479305ae81c61892cb5cf5e70
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74929464"
---
# <a name="copy-data-from-and-to-dynamics-365-common-data-service-or-dynamics-crm-by-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak verileri Dynamics 365 (Ortak Veri Hizmeti) veya Dynamics CRM'den kopyalama

Bu makalede, Verileri Microsoft Dynamics 365 veya Microsoft Dynamics CRM'den kopyalamak için Azure Veri Fabrikası'nda Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Kopyalama Etkinliği'ne genel bir genel bakış sunan [Kopyalama Etkinliği genel bakış](copy-activity-overview.md) makalesine dayanmaktadır.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu bağlayıcı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matrisi](copy-activity-overview.md) ile [etkinliği](copy-activity-overview.md) kopyalama
- [Arama etkinliği](control-flow-lookup-activity.md)

Dynamics 365 (Ortak Veri Hizmeti) veya Dynamics CRM'deki verileri desteklenen herhangi bir lavabo veri deposuna kopyalayabilirsiniz. Ayrıca, desteklenen herhangi bir kaynak veri deposundan dynamics 365 (Ortak Veri Hizmeti) veya Dynamics CRM'ye veri kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak veya lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Bu Dynamics bağlayıcısı, Dynamics sürüm 7.x ile 9.x arasında hem çevrimiçi hem de şirket içi için destekler. Daha spesifik olarak,

- Dynamics CRM 2015 sürüm 7.x haritaları
- Dynamics CRM 2016 ve Dynamics 365'in ilk sürümüsürüm 8.x haritaları
- Dynamics 365'in sonraki sürümüne sürüm 9.x haritaları

İlgili Dynamics sürümleri/ürünleri için desteklenen kimlik doğrulama türleri ve yapılandırmaları ile ilgili aşağıdaki tabloya bakın. (IFD, Internet'e dönük dağıtımın kısaltmasIdır.)

| Dynamics sürümleri | Kimlik doğrulaması türleri | Bağlantılı hizmet örnekleri |
|:--- |:--- |:--- |
| Common Data Service <br> Dynamics 365 çevrimiçi <br> Dynamics CRM Online | AAD hizmet sorumlusu <br> Ofis 365 | [Dinamikleri online + AAD hizmet müdürü veya Office365 auth](#dynamics-365-and-dynamics-crm-online) |
| IFD ile Dynamics 365 şirket içi <br> DYNAMICS CRM 2016 IFD ile şirket içi <br> DYNAMICS CRM 2015 IFD ile şirket içi | IFD | [IFD + IFD auth ile dinamikler şirket içi](#dynamics-365-and-dynamics-crm-on-premises-with-ifd) |

Özellikle Dynamics 365 için aşağıdaki uygulama türleri desteklenir:

- Dynamics 365 for Sales
- Dynamics 365 for Customer Service
- Dynamics 365 for Field Service
- Dynamics 365 for Project Service Automation
- Dynamics 365 for Marketing

Finans ve Operasyon, Yetenek vb. gibi diğer uygulama türleri bu bağlayıcı tarafından desteklenmez.

Bu Dynamics konektörü [Dynamics XRM takım lama](https://docs.microsoft.com/dynamics365/customer-engagement/developer/build-windows-client-applications-xrm-tools)nın üzerine inşa edilmiştir.

>[!TIP]
>**Dynamics 365 Finans ve Operasyon**verilerini kopyalamak için Dynamics [AX konektörünü](connector-dynamics-ax.md)kullanabilirsiniz.

## <a name="get-started"></a>Kullanmaya başlayın

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Dynamics'e özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Dynamics bağlantılı hizmet için aşağıdaki özellikler desteklenir.

### <a name="dynamics-365-and-dynamics-crm-online"></a>Dynamics 365 ve Dynamics CRM Online

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği **Dynamics,** **DynamicsCrm**veya **CommonDataServiceForApps**olarak ayarlanmalıdır. | Evet |
| Deploymenttype | Dynamics örneğinin dağıtım türü. Dynamics online için **"Online"** olmalıdır. | Evet |
| serviceUri | Dynamics örneğinizin hizmet URL'si, `https://adfdynamics.crm.dynamics.com`örn. | Evet |
| authenticationType | Dynamics sunucusuna bağlanmak için kimlik doğrulama türü. İzin verilen değerler şunlardır: **AADServicePrincipal** veya **"Office365"**. | Evet |
| hizmetPrincipalId | Azure Etkin Dizin uygulamasının istemci kimliğini belirtin. | Kimlik doğrulaması kullanırken `AADServicePrincipal` evet |
| hizmetPrincipalCredentialType | Hizmet asıl kimlik doğrulaması için kullanılacak kimlik bilgisi türünü belirtin. İzin verilen değerler şunlardır: **ServicePrincipalKey** veya **ServicePrincipalCert**. | Kimlik doğrulaması kullanırken `AADServicePrincipal` evet |
| hizmetPrincipalCredential | Hizmet temel kimlik belgesini belirtin. <br>Kimlik `ServicePrincipalKey` bilgisi türü olarak kullanırken, `servicePrincipalCredential` bir dize (ADF bağlantılı hizmet dağıtımı sırasında şifreler) veya AKV bir gizli bir başvuru olabilir. <br>Kimlik `ServicePrincipalCert` bilgisi olarak kullanırken, `servicePrincipalCredential` AKV'deki bir sertifikaya başvuru olmalıdır. | Kimlik doğrulaması kullanırken `AADServicePrincipal` evet | 
| kullanıcı adı | Dynamics'e bağlanmak için kullanıcı adını belirtin. | Kimlik doğrulaması kullanırken `Office365` evet |
| password | Kullanıcı adı için belirlediğiniz kullanıcı hesabının parolasını belirtin. Bu alanı, Veri Fabrikası'nda güvenli bir şekilde depolamak için SecureString olarak işaretleyin veya [Azure Key Vault'ta depolanan bir gizliye başvurun.](store-credentials-in-key-vault.md) | Kimlik doğrulaması kullanırken `Office365` evet |
| connectVia | Veri deposuna bağlanmak için kullanılacak [tümleştirme çalışma süresi.](concepts-integration-runtime.md) Belirtilmemişse, varsayılan Azure Tümleştirme Çalışma Süresini kullanır. | Kaynak için hayır, kaynak bağlantılı hizmetin tümleştirme çalışma süresi yoksa lavabo için Evet |

>[!NOTE]
>Dynamics bağlayıcısı, Dynamics CRM/365 Çevrimiçi örneğini tanımlamak için isteğe bağlı "organizationName" özelliğini kullanır. Çalışmaya devam ederken, örneğin keşif için daha iyi performans elde etmek için yerine yeni "serviceUri" özelliğini belirtmeniz önerilir.

**Örnek: AAD hizmet sorumlusu + anahtar kimlik doğrulaması kullanarak dinamikler çevrimiçi**

```json
{  
    "name": "DynamicsLinkedService",  
    "properties": {  
        "type": "Dynamics",  
        "typeProperties": {  
            "deploymentType": "Online",  
            "serviceUri": "https://adfdynamics.crm.dynamics.com",  
            "authenticationType": "AADServicePrincipal",  
            "servicePrincipalId": "<service principal id>",  
            "servicePrincipalCredentialType": "ServicePrincipalKey",  
            "servicePrincipalCredential": "<service principal key>"
        },  
        "connectVia": {  
            "referenceName": "<name of Integration Runtime>",  
            "type": "IntegrationRuntimeReference"  
        }  
    }  
}  
```
**Örnek: AAD hizmet sorumlusu + sertifika kimlik doğrulaması kullanarak dinamikler çevrimiçi**

```json
{ 
    "name": "DynamicsLinkedService", 
    "properties": { 
        "type": "Dynamics", 
        "typeProperties": { 
            "deploymentType": "Online", 
            "serviceUri": "https://adfdynamics.crm.dynamics.com", 
            "authenticationType": "AADServicePrincipal", 
            "servicePrincipalId": "<service principal id>", 
            "servicePrincipalCredentialType": "ServicePrincipalCert", 
            "servicePrincipalCredential": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<AKV reference>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<certificate name in AKV>" 
            } 
        }, 
        "connectVia": { 
            "referenceName": "<name of Integration Runtime>", 
            "type": "IntegrationRuntimeReference" 
        } 
    } 
} 
```

**Örnek: Office365 kimlik doğrulamasını kullanarak dinamikler çevrimiçi**

```json
{
    "name": "DynamicsLinkedService",
    "properties": {
        "type": "Dynamics",
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

### <a name="dynamics-365-and-dynamics-crm-on-premises-with-ifd"></a>DYNAMICS 365 ve Dynamics CRM IFD ile şirket içi

*Dynamics çevrimiçi ile karşılaştırılabilen ek özellikler "hostName" ve "port" olarak adlandırılır.*

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği **Dynamics,** **DynamicsCrm**veya **CommonDataServiceForApps**olarak ayarlanmalıdır. | Evet |
| Deploymenttype | Dynamics örneğinin dağıtım türü. IFD ile dynamics için **"OnPremisesWithIfd"** olmalıdır.| Evet |
| Hostname | Şirket içi Dynamics sunucusunun ana bilgisayar adı. | Evet |
| port | Şirket içi Dynamics sunucusunun bağlantı noktası. | Hayır, varsayılan 443 |
| organizationName | Dynamics örneğinin kuruluş adı. | Evet |
| authenticationType | Dynamics sunucusuna bağlanmak için kimlik doğrulama türü. IFD ile birlikte Dynamics için **"Ifd"** belirtin. | Evet |
| kullanıcı adı | Dynamics'e bağlanmak için kullanıcı adını belirtin. | Evet |
| password | Kullanıcı adı için belirlediğiniz kullanıcı hesabının parolasını belirtin. Bu alanı Güvenli Bir ADF'de güvenli bir şekilde depolamak veya parolayı Azure Key Vault'ta depolamak ve veri kopyalama gerçekleştirirken kopyalama etkinliğinin oradan çekilmesine izin vermek için SecureString olarak işaretlemeyi seçebilirsiniz - [Key Vault'taki Mağaza kimlik numaralarından](store-credentials-in-key-vault.md)daha fazla bilgi edinin. | Evet |
| connectVia | Veri deposuna bağlanmak için kullanılacak [tümleştirme çalışma süresi.](concepts-integration-runtime.md) Belirtilmemişse, varsayılan Azure Tümleştirme Çalışma Süresini kullanır. | Kaynak için hayır, lavabo için Evet |

**Örnek: IFD kimlik doğrulaması kullanarak IFD ile dinamikler**

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

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [Datasets](concepts-datasets-linked-services.md) makalesine bakın. Bu bölümde Dynamics veri kümesi tarafından desteklenen özelliklerin bir listesi sağlar.

Verileri Dynamics'ten ve Dynamics'e kopyalamak için aşağıdaki özellikler desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin tür özelliği **DynamicsEntity**, **DynamicsCrmEntity**veya **CommonDataServiceForAppsEntity**olarak ayarlanmalıdır. |Evet |
| Varlıkadı | Alabilecek varlığın mantıksal adı. | Kaynak için hayır (etkinlik kaynağında "sorgu" belirtilirse), lavabo için Evet |

**Örnek:**

```json
{
    "name": "DynamicsDataset",
    "properties": {
        "type": "DynamicsEntity",
        "schema": [],
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

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi [için, Pipelines](concepts-pipelines-activities.md) makalesine bakın. Bu bölümde, Dynamics kaynağı ve lavabo türleri tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="dynamics-as-a-source-type"></a>Kaynak türü olarak dinamikler

Dynamics'teki verileri kopyalamak için, kopyalama etkinliği **kaynak** bölümünde aşağıdaki özellikler desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinlik kaynağının türü özelliği **DynamicsSource,** **DynamicsCrmSource**veya **CommonDataServiceForAppsSource**olarak ayarlanmalıdır. | Evet |
| sorgu | FetchXML, Dynamics'te (çevrimiçi ve şirket içinde) kullanılan özel bir sorgu dilidir. Aşağıdaki örneğe bakın. Daha fazla bilgi için bkz: [FetchXML ile sorgu oluştur.](https://msdn.microsoft.com/library/gg328332.aspx) | Hayır (veri kümesinde "entityName" belirtilirse) |

>[!NOTE]
>FetchXML sorgusunda yapılandırdığınız sütun projeksiyonu bunu içermese bile PK sütunu her zaman kopyalanır.

> [!IMPORTANT]
>- Dynamics'teki verileri kopyaladiğinizde, Dynamics'ten lavaboya açık sütun eşleme isteğe bağlıdır, ancak deterministik bir kopya sonucu sağlamak için yüksek oranda yeniden komut verilir.
>- UI yazarken şema aktarırken, ADF kaynak sütun listesini başlatmak için Dynamics sorgusu sonucunun üst satırlarını örnekleyerek şemayı çıkartır ve bu durumda üst satırlarda değer olmayan sütunlar atlanır. Açık eşleme yoksa, aynı davranış kopyalama yürütmeleri için de geçerlidir. Kopya çalışma süresi sırasında onurlandırılacak eşleme içine daha fazla sütun gözden geçirebilir ve ekleyebilirsiniz.

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

### <a name="dynamics-as-a-sink-type"></a>Lavabo türü olarak dinamikler

Verileri Dynamics'e kopyalamak için, kopyalama etkinliği **lavabo** bölümünde aşağıdaki özellikler desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği lavabo türü özelliği **DynamicsSink**, **DynamicsCrmSink**veya **CommonDataServiceForAppsSink**olarak ayarlanmalıdır. | Evet |
| yazmaDavranışı | İşlemin yazma davranışı.<br/>İzin verilen değer **"Upsert"** dir. | Evet |
| alternatifKeyName | "Upsert" gerçekleştirmek için varlığınızda tanımlanan alternatif anahtar adını belirtin. | Hayır |
| yazmaBatchSize | Her toplu iş partisinde Dynamics'e yazılan verilerin satır sayısı. | Hayır (varsayılan değer 10'dur) |
| yoksNullValues | Yazma işlemi sırasında giriş verilerinden (anahtar alanlar hariç) geçersiz değerleri yoksayıp yoksaymayacağını gösterir.<br/>İzin verilen değerler **doğru** ve **yanlıştır.**<br>- **True**: Bir yükseltme/güncelleştirme işlemi yaptığınızda hedef nesnedeki verileri değişmeden bırakın. Bir ekleme işlemi yaparken tanımlı bir varsayılan değer ekleyin.<br/>- **False**: Bir yükseltme/güncelleştirme işlemi yaptığınızda hedef nesnedeki verileri NULL olarak güncelleştirin. Bir ekleme işlemi yaparken NULL değeri ekleyin. | Hayır (varsayılan yanlıştır) |

>[!NOTE]
>Dinamiği lavabosu için lavabonun varsayılan değeri "**writeBatchSize**" ve kopyalama etkinliği "**[parallelCopys](copy-activity-performance.md#parallel-copy)**" 10'dur. Bu nedenle, 100 kayıt aynı anda Dynamics'e gönderilir.

Dynamics 365 çevrimiçi [için, kuruluş başına 2 eşzamanlı toplu arama](https://msdn.microsoft.com/library/jj863631.aspx#Run-time%20limitations)sınırı vardır. Bu sınır aşılırsa, ilk istek yürütülmeden önce bir "Sunucu Meşgul" hatası atılır. "writeBatchSize"yı daha az veya 10'a eşit tutmak, eşzamanlı çağrıların bu kadar daraltılmasından kaçınır.

"**writeBatchSize**" ve "**parallelCopy**" in optimal kombinasyonu, sütun sayısı, satır boyutu, eklenti sayısı/iş akışları/iş akışı etkinlikleri gibi varlığınızın şemasına bağlıdır. 10 writeBatchSize * 10 parallelCopy varsayılan ayarı en iyi performans olmayabilir rağmen en Dynamics varlıkları için çalışacak Dynamics hizmetine göre tavsiyedir. Kopyalama etkinliği ayarlarınızdaki kombinasyonu ayarlayarak performansı ayarlayabilirsiniz.

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

## <a name="data-type-mapping-for-dynamics"></a>Dinamikler için veri türü eşleme

Dynamics'teki verileri kopyaladiğinizde, Dynamics veri türlerinden Veri Fabrikası geçici veri türlerine aşağıdaki eşlemeler kullanılır. Kopyalama etkinliğinin kaynak şemasını ve veri türünü lavaboyla nasıl eşlenebildiğini öğrenmek için Bkz. [Şema ve veri türü eşlemeleri.](copy-activity-schema-and-type-mapping.md)

Aşağıdaki eşleme tablosunu kullanarak kaynak Dinamikleri veri türünü temel alarak bir veri kümesi yapısında ilgili Veri Fabrikası veri türünü yapılandırın.

| Dinamikler veri türü | Veri Fabrikası geçici veri türü | Kaynak olarak desteklenir | Lavabo olarak desteklenir |
|:--- |:--- |:--- |:--- |
| ÖznitelikTypeCode.BigInt | Uzun | ✓ | ✓ |
| ÖznitelikTypeCode.Boolean | Boole | ✓ | ✓ |
| ÖznitelikType.Customer | Guid | ✓ | |
| ÖznitelikType.DateTime | Tarih saat | ✓ | ✓ |
| ÖznitelikType.Ondalık | Ondalık | ✓ | ✓ |
| ÖznitelikType.Double | Çift | ✓ | ✓ |
| ÖznitelikType.EntityName | Dize | ✓ | ✓ |
| ÖznitelikType.Integer | Int32 | ✓ | ✓ |
| ÖznitelikType.Lookup | Guid | ✓ | ✓ (tek hedef ilişkili) |
| ÖznitelikType.ManagedProperty | Boole | ✓ | |
| ÖznitelikType.Memo | Dize | ✓ | ✓ |
| ÖznitelikType.Money | Ondalık | ✓ | ✓ |
| ÖznitelikType.Owner | Guid | ✓ | |
| ÖznitelikType.Picklist | Int32 | ✓ | ✓ |
| ÖznitelikType.Uniqueidentifier | Guid | ✓ | ✓ |
| ÖznitelikType.String | Dize | ✓ | ✓ |
| ÖznitelikType.State | Int32 | ✓ | ✓ |
| ÖznitelikType.Status | Int32 | ✓ | ✓ |

> [!NOTE]
> Dinamikveri türleri AttributeType.CalendarRules, AttributeType.MultiSelectPicklist ve AttributeType.PartyList desteklenmez.

## <a name="lookup-activity-properties"></a>Arama etkinlik özellikleri

Özellikler hakkında daha fazla bilgi edinmek için [Arama etkinliğini](control-flow-lookup-activity.md)kontrol edin.

## <a name="next-steps"></a>Sonraki adımlar
Veri Fabrikası'ndaki kopyalama etkinliği tarafından kaynak ve lavabo olarak desteklenen veri depolarının listesi [için](copy-activity-overview.md#supported-data-stores-and-formats)bkz.
