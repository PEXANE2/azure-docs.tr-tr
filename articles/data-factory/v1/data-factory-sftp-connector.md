---
title: Azure Veri Fabrikası'nı kullanarak Verileri SFTP sunucusundan taşıma
description: Azure Veri Fabrikası'nı kullanarak şirket içi veya bulut SFTP sunucusundan verileri nasıl taşıyabilirsiniz hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 02/12/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 3f78934fb11dd4f9e34bf27d565d471d47f250b4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79265811"
---
# <a name="move-data-from-an-sftp-server-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak verileri bir SFTP sunucusundan taşıma
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](data-factory-sftp-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-sftp.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [V2'deki SFTPconnector'a](../connector-sftp.md)bakın.

Bu makalede, verileri şirket içi/bulut SFTP sunucusundan desteklenen bir lavabo veri deposuna taşımak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı özetlenmiştir. Bu makalede, kopya etkinliği ile veri hareketinin genel bir genel bakış ve kaynak /lavabo olarak desteklenen veri depolarının listesini sunan [veri hareketi etkinlikleri](data-factory-data-movement-activities.md) makalesi üzerine bir temel oluşturur.

Veri fabrikası şu anda yalnızca bir SFTP sunucusundan diğer veri depolarına veri taşımayı destekler, ancak verileri diğer veri depolarından Bir SFTP sunucusuna taşımak için değildir. Hem şirket içi hem de bulut SFTP sunucularını destekler.

> [!NOTE]
> Copy Activity, hedefe başarıyla kopyalandıktan sonra kaynak dosyayı silmez. Başarılı bir kopyadan sonra kaynak dosyayı silmeniz gerekiyorsa, dosyayı silmek ve ardışık düzendeki etkinliği kullanmak için özel bir etkinlik oluşturun.

## <a name="supported-scenarios-and-authentication-types"></a>Desteklenen senaryolar ve kimlik doğrulama türleri
Bu SFTP bağlayıcısını hem bulut **SFTP sunucularından hem de şirket içi SFTP sunucularından**veri kopyalamak için kullanabilirsiniz. **Temel** ve **SshPublicKey** kimlik doğrulama türleri SFTP sunucusuna bağlanırken desteklenir.

Şirket içi bir SFTP sunucusundan veri kopyalarken, şirket içi ortama/Azure VM'ye bir Veri Yönetimi Ağ Geçidi yüklemeniz gerekir. Ağ geçidiyle ilgili ayrıntılar için [Veri Yönetimi Ağ Geçidi'ne](data-factory-data-management-gateway.md) bakın. Ağ geçidini kurma ve kullanma yla ilgili adım adım talimatlar için [şirket içi konumlar ve bulut](data-factory-move-data-between-onprem-and-cloud.md) makalesi arasında hareketli veri bakın.

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'ler kullanarak verileri bir SFTP kaynağından hareket ettiren bir kopyalama etkinliği içeren bir ardışık hatlar oluşturabilirsiniz.

- Bir ardışık yol oluşturmanın en kolay yolu **Kopyalama Sihirbazı'nı**kullanmaktır. Bkz. Öğretici: Veri kopyala sihirbazını kullanarak bir ardışık yol oluşturma konusunda hızlı bir geçiş için Kopya Sihirbazı kullanarak bir [ardışık kaynak oluşturun.](data-factory-copy-data-wizard-tutorial.md)

- Bir ardışık kaynak oluşturmak için aşağıdaki araçları da kullanabilirsiniz: **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**ve **REST API**. Kopyalama etkinliği içeren bir ardışık hatlar oluşturmak için adım adım yönergeleri için [etkinlik öğreticisini](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyala'ya bakın. JSON örneklerinin SFTP sunucusundan Azure Blob Depolama'ya veri kopyalaması için Bkz. [JSON Örneği: Verileri SFTP sunucusundan](#json-example-copy-data-from-sftp-server-to-azure-blob) bu makalenin Azure blob bölümüne kopyalayın.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri
Aşağıdaki tablo, FTP bağlantılı hizmete özgü JSON öğeleri için açıklama sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type | Tür özelliği `Sftp`' ye ayarlanmalıdır. |Evet |
| konak | SFTP sunucusunun adı veya IP adresi. |Evet |
| port |SFTP sunucusunun dinlediği bağlantı noktası. Varsayılan değer: 21 |Hayır |
| authenticationType |Kimlik doğrulama türünü belirtin. İzin verilen değerler: **Temel**, **SshPublicKey**. <br><br> Temel [kimlik doğrulamayı kullanma](#using-basic-authentication) ve [sb ortak anahtar kimlik doğrulama](#using-ssh-public-key-authentication) bölümlerini sırasıyla daha fazla özellik ve JSON örneklerine göre kullanma bölümüne bakın. |Evet |
| skipHostKeyValidation | Ana bilgisayar anahtar doğrulaması atlayıp atlamayacağıbelirtin. | Hayır. Varsayılan değer: false |
| hostKeyFingerprint | Ana bilgisayar anahtarının parmak izini belirtin. | Evet eğer `skipHostKeyValidation` yanlış ayarlanırsa.  |
| ağ geçidiAdı |Şirket içi bir SFTP sunucusuna bağlanmak için Veri Yönetimi Ağ Geçidi'nin adı. | Şirket içi bir SFTP sunucusundan veri kopyalıyorsanız evet. |
| şifreli Credential | SFTP sunucusuna erişmek için şifrelenmiş kimlik bilgisi. Kopya sihirbazında veya ClickOnce açılır pencereiletişim kutusunda temel kimlik doğrulaması (kullanıcı adı + parola) veya SshPublicKey kimlik doğrulaması (kullanıcı adı + özel anahtar yolu veya içerik) belirttiğinde otomatik olarak oluşturulur. | Hayır. Yalnızca şirket içi bir SFTP sunucusundan veri kopyalarken uygulayın. |

### <a name="using-basic-authentication"></a>Temel kimlik doğrulamayı kullanma

Temel kimlik doğrulamasını `authenticationType` kullanmak `Basic`için, "olarak ayarlayın ve son bölümde tanıtılan SFTP bağlayıcısı jenerik özelliklerinin yanı sıra aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| kullanıcı adı | SFTP sunucusuna erişimi olan kullanıcı. |Evet |
| password | Kullanıcı nın şifresi (kullanıcı adı). | Evet |

#### <a name="example-basic-authentication"></a>Örnek: Temel kimlik doğrulama
```json
{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver",
            "port": 22,
            "authenticationType": "Basic",
            "username": "xxx",
            "password": "xxx",
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "gatewayName": "mygateway"
        }
    }
}
```

#### <a name="example-basic-authentication-with-encrypted-credential"></a>Örnek: Şifreli kimlik bilgisi içeren temel kimlik doğrulaması

```JSON
{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver",
            "port": 22,
            "authenticationType": "Basic",
            "username": "xxx",
            "encryptedCredential": "xxxxxxxxxxxxxxxxx",
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "gatewayName": "mygateway"
        }
      }
}
```

### <a name="using-ssh-public-key-authentication"></a>SSH ortak anahtar kimlik doğrulamasını kullanma

SSH ortak anahtar kimlik doğrulamasını `SshPublicKey`kullanmak için, son bölümde tanıtılan SFTP bağlayıcısı jenerik özelliklerinin yanı sıra aşağıdaki özellikleri ayarlayın `authenticationType` ve belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| kullanıcı adı |SFTP sunucusuna erişimi olan kullanıcı |Evet |
| privateKeyPath | Ağ geçidinin erişebileceği özel anahtar dosyasına mutlak yol belirtin. | Belirtin `privateKeyPath` ya `privateKeyContent`da . <br><br> Yalnızca şirket içi bir SFTP sunucusundan veri kopyalarken uygulayın. |
| privateKeyContent | Özel anahtar içeriğinin serileştirilmiş dizesi. Kopyalama Sihirbazı özel anahtar dosyasını okuyabilir ve özel anahtar içeriğini otomatik olarak ayıklayabilir. Başka bir araç/SDK kullanıyorsanız, bunun yerine privateKeyPath özelliğini kullanın. | Belirtin `privateKeyPath` ya `privateKeyContent`da . |
| Parola | Anahtar dosyası bir geçiş tümceciği tarafından korunuyorsa, özel anahtarın şifresini çözmek için geçiş tümceciği/parolasını belirtin. | Özel anahtar dosyası bir geçiş tümceciği tarafından korunuyorsa evet. |

> [!NOTE]
> SFTP konektörü RSA/DSA OpenSSH anahtarını destekler. Anahtar dosya içeriğinizin "-----BEGIN [RSA/DSA] PRIVATE KEY-----" ile başladığından emin olun. Özel anahtar dosyası ppk formatında bir dosyaysa, lütfen .ppk'den OpenSSH biçimine dönüştürmek için Macun aracını kullanın.

#### <a name="example-sshpublickey-authentication-using-private-key-filepath"></a>Örnek: Özel anahtar dosyasıPath kullanarak SshPublicKey kimlik doğrulaması

```json
{
    "name": "SftpLinkedServiceWithPrivateKeyPath",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver",
            "port": 22,
            "authenticationType": "SshPublicKey",
            "username": "xxx",
            "privateKeyPath": "D:\\privatekey_openssh",
            "passPhrase": "xxx",
            "skipHostKeyValidation": true,
            "gatewayName": "mygateway"
        }
    }
}
```

#### <a name="example-sshpublickey-authentication-using-private-key-content"></a>Örnek: Özel anahtar içeriğini kullanarak SshPublicKey kimlik doğrulaması

```json
{
    "name": "SftpLinkedServiceWithPrivateKeyContent",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver.westus.cloudapp.azure.com",
            "port": 22,
            "authenticationType": "SshPublicKey",
            "username": "xxx",
            "privateKeyContent": "<base64 string of the private key content>",
            "passPhrase": "xxx",
            "skipHostKeyValidation": true
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Veri kümelerini tanımlamak için kullanılabilen bölümlerin & özelliklerin tam listesi için [veri kümelerini oluşturma](data-factory-create-datasets.md) makalesine bakın. Bir veri kümesi JSON'un yapısı, kullanılabilirliği ve ilkesi gibi bölümler tüm veri kümesi türleri için benzerdir.

**typeProperties** bölümü her veri kümesi türü için farklıdır. Veri kümesi türüne özgü bilgiler sağlar. **FileShare** veri kümesinin bir veri kümesi için typeProperties bölümü aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| folderPath |Klasöre alt yol. Dizedeki özel karakterler için ' \ ' kaçış karakterini kullanın. Örnekler için Örnek bağlantılı hizmet ve veri kümesi tanımlarına bakın.<br/><br/>Bu özelliği **partitionBy** ile birleştirerek dilim başlangıç/bitiş tarih saatlerine göre klasör yollarına sahip olabilirsiniz. |Evet |
| fileName |Tablonun klasördeki belirli bir dosyaya başvurmasını istiyorsanız **klasörDeki Dosyanın** adını belirtinPath. Bu özellik için herhangi bir değer belirtmezseniz, tablo klasördeki tüm dosyaları işaret edir.<br/><br/>Bir çıktı veri kümesi için dosya Adı belirtilmediğinde, oluşturulan dosyanın adı aşağıdaki biçimde olacaktır: <br/><br/>`Data.<Guid>.txt`(Örnek: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Hayır |
| dosyaFiltre |Tüm dosyalar yerine folderPath'teki dosyaların bir alt kümesini seçmek için kullanılacak bir filtre belirtin.<br/><br/>İzin verilen `*` değerler şunlardır: `?` (birden çok karakter) ve (tek karakter).<br/><br/>Örnekler 1:`"fileFilter": "*.log"`<br/>Örnek 2:`"fileFilter": 2014-1-?.txt"`<br/><br/> fileFilter bir giriş FileShare veri kümesi için geçerlidir. Bu özellik HDFS ile desteklenmez. |Hayır |
| bölümlemeBy |partitionedBy dinamik bir folderPath, zaman serisi verileri için dosya adı belirtmek için kullanılabilir. Örneğin, klasörPath veri her saat için parametreli. |Hayır |
| biçim | Aşağıdaki biçim türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParkeFormat**. Biçim altındaki **tür** özelliğini bu değerlerden birine ayarlayın. Daha fazla bilgi için [Metin Biçimi,](data-factory-supported-file-and-compression-formats.md#text-format) [Json Formatı,](data-factory-supported-file-and-compression-formats.md#json-format) [Avro Biçimi,](data-factory-supported-file-and-compression-formats.md#avro-format) [Ork Biçimi](data-factory-supported-file-and-compression-formats.md#orc-format)ve [Parke Biçimi](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümlerine bakın. <br><br> Dosyaları dosya tabanlı mağazalar (ikili kopya) arasında **olduğu gibi kopyalamak** istiyorsanız, hem giriş hem de çıktı veri kümesi tanımlarında biçim bölümünü atlayın. |Hayır |
| sıkıştırma | Verilerin sıkıştırma türünü ve düzeyini belirtin. Desteklenen türleri şunlardır: **GZip**, **Deflate**, **BZip2**, ve **ZipDeflate**. Desteklenen seviyeler şunlardır: **Optimal** ve **En Hızlı.** Daha fazla bilgi için [Azure Veri Fabrikası'ndaki Dosya ve sıkıştırma biçimlerine](data-factory-supported-file-and-compression-formats.md#compression-support)bakın. |Hayır |
| useBinaryTransfer |İkili aktarım modunu kullanıp kullanmayacağını belirtin. İkili mod ve yanlış ASCII için geçerlidir. Varsayılan değer: True. Bu özellik yalnızca ilişkili bağlantılı hizmet türü türünde olduğunda kullanılabilir: FtpServer. |Hayır |

> [!NOTE]
> dosya adı ve fileFilter aynı anda kullanılamaz.

### <a name="using-partionedby-property"></a>PartionedBy özelliğini kullanma
Önceki bölümde belirtildiği gibi, dinamik bir folderPath, bölümlenmişBy ile zaman serisi verileri için dosya adı belirtebilirsiniz. Bunu Veri Fabrikası makroları ve belirli bir veri dilimi için mantıksal zaman dilimini gösteren SliceEnd sistem değişkeni SliceStart ile yapabilirsiniz.

Zaman serisi veri kümeleri, zamanlama ve dilimler hakkında bilgi edinmek için Bkz. [Veri Kümeleri Oluşturma,](data-factory-create-datasets.md) [Yürütme & Zamanlama](data-factory-scheduling-and-execution.md)ve [Ardışık Hatlar](data-factory-create-pipelines.md) makaleleri oluşturma.

#### <a name="sample-1"></a>Örnek 1:

```json
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```
Bu örnekte {Slice} veri fabrikası sistem değişkeni SliceStart değeri ile değiştirilir formatta (YYYYMMDDHH) belirtilir. SliceStart, dilimin başlangıç saatini ifade eder. FolderPath her dilim için farklıdır. Örnek: wikidatagateway/wikisampledataout/2014100103 veya wikidatagateway/wikisampledataout/2014100104.

#### <a name="sample-2"></a>Örnek 2:

```json
"folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
"fileName": "{Hour}.csv",
"partitionedBy":
[
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
    { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
],
```
Bu örnekte, SliceStart'ın yılı, ayı, günü ve saati folderPath ve fileName özellikleri tarafından kullanılan ayrı değişkenlere ayıklanır.

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Etkinlikleri tanımlamak için kullanılabilen bölümlerin & özelliklerinin tam listesi [için, Kaynak Hatları Oluşturma](data-factory-create-pipelines.md) makalesine bakın. Ad, açıklama, giriş ve çıktı tabloları ve ilkeler gibi özellikler tüm etkinlik türleri için kullanılabilir.

Oysa, etkinliğin typeProperties bölümünde bulunan özellikler her etkinlik türüne göre değişir. Kopyalama etkinliği için tür özellikleri kaynak ve lavabo türlerine bağlı olarak değişir.

[!INCLUDE [data-factory-file-system-source](../../../includes/data-factory-file-system-source.md)]

## <a name="supported-file-and-compression-formats"></a>Desteklenen dosya ve sıkıştırma biçimleri
Ayrıntılarla ilgili [Azure Veri Fabrikası makalesinde Dosya ve sıkıştırma biçimlerine](data-factory-supported-file-and-compression-formats.md) bakın.

## <a name="json-example-copy-data-from-sftp-server-to-azure-blob"></a>JSON Örneği: SFTP sunucusundan Azure blob'una veri kopyalama
Aşağıdaki örnek, [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)kullanarak bir ardışık hat lar oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar. SFTP kaynağından Azure Blob Depolama'ya verilerin nasıl kopyalanır olduğunu gösterirler. Ancak veriler, Azure Veri Fabrikası'ndaki Kopyalama Etkinliği kullanılarak [doğrudan](data-factory-data-movement-activities.md#supported-data-stores-and-formats) herhangi bir **kaynaktan** burada belirtilen lavabolara kopyalanabilir.

> [!IMPORTANT]
> Bu örnek JSON parçacıkları sağlar. Veri fabrikası oluşturmak için adım adım yönergeler içermez. Adım adım yönergeler için [şirket içi konumlar ve bulut](data-factory-move-data-between-onprem-and-cloud.md) makalesi arasında hareketli veri bakın.

Örnekte aşağıdaki veri fabrikası varlıkları vardır:

* [Sftp](#linked-service-properties)türüne bağlı bir hizmet.
* [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)türüne bağlı bir hizmet.
* [FileShare](#dataset-properties)türünden bir giriş [veri kümesi.](data-factory-create-datasets.md)
* [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)türünden bir çıktı [veri kümesi.](data-factory-create-datasets.md)
* [FileSystemSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)kullanan Copy Activity ile bir [ardışık düzen.](data-factory-create-pipelines.md)

Örnek, verileri bir SFTP sunucusundan her saat başı bir Azure blob'una kopyalar. Bu örneklerde kullanılan JSON özellikleri, örnekleri izleyen bölümlerde açıklanmıştır.

**SFTP bağlantılı hizmet**

Bu örnek, kullanıcı adı ve parolaile birlikte düz metindeki temel kimlik doğrulamasını kullanır. Aşağıdaki yollardan birini de kullanabilirsiniz:

* Şifreli kimlik bilgileriyle temel kimlik doğrulaması
* SSH genel anahtar kimlik doğrulaması

Kullanabileceğiniz farklı kimlik doğrulama türleri için [FTP bağlantılı hizmet](#linked-service-properties) bölümüne bakın.

```JSON

{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver",
            "port": 22,
            "authenticationType": "Basic",
            "username": "myuser",
            "password": "mypassword",
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "gatewayName": "mygateway"
        }
    }
}
```
**Azure Depolama bağlantılı hizmet**

```JSON
{
  "name": "AzureStorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
**SFTP giriş veri seti**

Bu veri kümesi SFTP `mysharedfolder` klasörü `test.csv`ve dosyası anlamına gelir. Ardışık iş, dosyayı hedefe kopyalar.

"Dış"ı ayarlamak: "true" veri kümesinin veri fabrikasının dışında olduğunu ve veri fabrikasındaki bir etkinlik tarafından üretilmediğini Veri Fabrikası hizmetine bildirir.

```JSON
{
  "name": "SFTPFileInput",
  "properties": {
    "type": "FileShare",
    "linkedServiceName": "SftpLinkedService",
    "typeProperties": {
      "folderPath": "mysharedfolder",
      "fileName": "test.csv"
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

**Azure Blob çıktı veri kümesi**

Veriler her saat yeni bir blob 'a yazılır (sıklık: saat, aralık: 1). Blob için klasör yolu, işlenen dilimin başlangıç saatine göre dinamik olarak değerlendirilir. Klasör yolu, başlangıç zamanının yıl, ay, gün ve saat bölümlerini kullanır.

```JSON
{
    "name": "AzureBlobOutput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/sftp/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            },
            "partitionedBy": [
                {
                    "name": "Year",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "yyyy"
                    }
                },
                {
                    "name": "Month",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "MM"
                    }
                },
                {
                    "name": "Day",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "dd"
                    }
                },
                {
                    "name": "Hour",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "HH"
                    }
                }
            ]
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

**Kopyalama etkinliği olan boru hattı**

Ardışık iş, giriş ve çıktı veri kümelerini kullanacak şekilde yapılandırılan ve her saat çalışacak şekilde zamanlanan bir Kopyalama Etkinliği içerir. Boru hattı JSON tanımında, **kaynak** türü **FileSystemSource** olarak ayarlanır ve **lavabo** türü **BlobSink**olarak ayarlanır.

```JSON
{
    "name": "pipeline",
    "properties": {
        "activities": [{
            "name": "SFTPToBlobCopy",
            "inputs": [{
                "name": "SFTPFileInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "NewestFirst",
                "retry": 1,
                "timeout": "00:05:00"
            }
        }],
        "start": "2017-02-20T18:00:00Z",
        "end": "2017-02-20T19:00:00Z"
    }
}
```

## <a name="performance-and-tuning"></a>Performans ve Tuning
Azure Veri Fabrikası'ndaki veri hareketinin performansını etkileyen önemli faktörler (Kopyalama Etkinliği) ve bunu optimize etmenin çeşitli yolları hakkında bilgi edinmek için [Etkinlik performansını & Tuning Kılavuzu'na](data-factory-copy-activity-performance.md) bakın.

## <a name="next-steps"></a>Sonraki Adımlar
Aşağıdaki makalelere bakın:

* Kopyalama Etkinliği ile bir ardışık hatlar oluşturmak için adım adım yönergeleri için [Etkinlik öğreticikopyalayın.](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
