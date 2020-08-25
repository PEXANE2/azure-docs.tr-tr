---
title: Verileri ve SFTP sunucusundan Kopyala
description: Azure Data Factory kullanarak verileri ve SFTP sunucusundan kopyalamayı öğrenin.
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
ms.date: 08/25/2020
ms.openlocfilehash: a03a141a4140ca4ac000a8e2afb8dd8f45d40662
ms.sourcegitcommit: d39f2cd3e0b917b351046112ef1b8dc240a47a4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88816632"
---
# <a name="copy-data-from-and-to-the-sftp-server-by-using-azure-data-factory"></a>Azure Data Factory kullanarak SFTP sunucusundan verileri kopyalama

> [!div class="op_single_selector" title1="Kullanmakta olduğunuz Data Factory hizmetinin sürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-sftp-connector.md)
> * [Güncel sürüm](connector-sftp.md)
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, verileri ve güvenli FTP (SFTP) sunucusundan verilerin nasıl kopyalanacağı özetlenmektedir. Azure Data Factory hakkında bilgi edinmek için [tanıtım makalesini](introduction.md)okuyun.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

SFTP Bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/havuz matrisi](copy-activity-overview.md) ile [kopyalama etkinliği](copy-activity-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)
- [GetMetadata etkinliği](control-flow-get-metadata-activity.md)
- [Etkinliği sil](delete-activity.md)

Özellikle, SFTP Bağlayıcısı şunları destekler:

- *Temel* veya *sshpublickey* kimlik doğrulamasını kullanarak dosyaları ve SFTP sunucusuna kopyalama.
- Dosyaları olarak kopyalama veya [Desteklenen dosya biçimleri ve sıkıştırma codec bileşenleri](supported-file-formats-and-compression-codecs.md)ile dosyaları ayrıştırma veya oluşturma.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [data-factory-v2-integration-runtime-requirements](../../includes/data-factory-v2-integration-runtime-requirements.md)]

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, SFTP 'ye özgü Data Factory varlıkları tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlı hizmet özellikleri

SFTP bağlantılı hizmeti için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | Type özelliği *SFTP*olarak ayarlanmalıdır. |Yes |
| konak | SFTP sunucusunun adı veya IP adresi. |Yes |
| port | SFTP sunucusunun dinlediği bağlantı noktası.<br/>İzin verilen değer bir tamsayıdır ve varsayılan değer *22*' dir. |Hayır |
| skipHostKeyValidation | Konak anahtarı doğrulamanın atlanıp atlanmayacağını belirtin.<br/>İzin verilen değerler *true* ve *false* (varsayılan) şeklindedir.  | Hayır |
| hostKeyFingerprint | Ana bilgisayar anahtarının parmak izini belirtin. | Evet, "skipHostKeyValidation" false olarak ayarlanır.  |
| authenticationType | Kimlik doğrulama türünü belirtin.<br/>İzin verilen değerler *temel* ve *sshpublickey*. Daha fazla özellik için, [temel kimlik doğrulaması kullanma](#use-basic-authentication) bölümüne bakın. JSON örnekleri için [SSH ortak anahtar kimlik doğrulamasını kullanma](#use-ssh-public-key-authentication) bölümüne bakın. |Yes |
| connectVia | Veri deposuna bağlanmak için kullanılacak [tümleştirme çalışma zamanı](concepts-integration-runtime.md) . Daha fazla bilgi edinmek için [Önkoşullar](#prerequisites) bölümüne bakın. Tümleştirme çalışma zamanı belirtilmemişse, hizmet varsayılan Azure Integration Runtime kullanır. |Hayır |

### <a name="use-basic-authentication"></a>Temel kimlik doğrulaması kullan

Temel kimlik doğrulaması kullanmak için *AuthenticationType* özelliğini *temel*olarak ayarlayın ve ÖNCEKI bölümde tanıtılan SFTP Bağlayıcısı genel özelliklerine ek olarak aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| userName | SFTP sunucusuna erişimi olan kullanıcı. |Yes |
| password | Kullanıcının parolası (Kullanıcı adı). Veri fabrikanıza güvenli bir şekilde depolamak için bu alanı SecureString olarak işaretleyin veya bir [Azure Anahtar Kasası 'nda depolanan bir gizli dizi başvurusu](store-credentials-in-key-vault.md)yapın. | Yes |

**Örnek:**

```json
{
    "name": "SftpLinkedService",
    "type": "linkedservices",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<sftp server>",
            "port": 22,
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "authenticationType": "Basic",
            "userName": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of integration runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="use-ssh-public-key-authentication"></a>SSH ortak anahtar kimlik doğrulaması kullan

SSH ortak anahtar kimlik doğrulamasını kullanmak için, "authenticationType" özelliğini **Sshpublickey**olarak ayarlayın ve en son bölümde tanıtılan SFTP Bağlayıcısı genel 'in yanı sıra aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| userName | SFTP sunucusuna erişimi olan kullanıcı. |Yes |
| privateKeyPath | Tümleştirme çalışma zamanının erişebileceği özel anahtar dosyasının mutlak yolunu belirtin. Bu, yalnızca kendi kendine barındırılan tümleştirme çalışma zamanı türü "connectVia" içinde belirtildiğinde geçerlidir. | Ya da `privateKeyPath` belirtin `privateKeyContent` .  |
| privateKeyContent | Base64 kodlamalı SSH özel anahtar içeriği. SSH özel anahtarı OpenSSH biçiminde olmalıdır. Veri fabrikanıza güvenli bir şekilde depolamak için bu alanı SecureString olarak işaretleyin veya bir [Azure Anahtar Kasası 'nda depolanan bir gizli dizi başvurusu](store-credentials-in-key-vault.md)yapın. | Ya da `privateKeyPath` belirtin `privateKeyContent` . |
| Deyimi | Anahtar dosyası veya anahtar içeriği bir pass ifadesi tarafından korunuyorsa, özel anahtarın şifresini çözmek için geçiş tümceciğini veya parolayı belirtin. Veri fabrikanıza güvenli bir şekilde depolamak için bu alanı SecureString olarak işaretleyin veya bir [Azure Anahtar Kasası 'nda depolanan bir gizli dizi başvurusu](store-credentials-in-key-vault.md)yapın. | Evet, özel anahtar dosyası veya anahtar içeriği bir pass ifadesi tarafından korunuyorsa. |

> [!NOTE]
> SFTP Bağlayıcısı bir RSA/DSA OpenSSH anahtarını destekler. Anahtar dosyası içeriğinizin "-----BEGIN [RSA/DSA] özel anahtar-----" ile başlayıp başlamadığına emin olun. Özel anahtar dosyası bir PPK biçimli dosya ise, PPK 'den OpenSSH biçimine dönüştürmek için PuTTY aracını kullanın. 

**Örnek 1: özel anahtar filePath kullanarak SshPublicKey kimlik doğrulaması**

```json
{
    "name": "SftpLinkedService",
    "type": "Linkedservices",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<sftp server>",
            "port": 22,
            "skipHostKeyValidation": true,
            "authenticationType": "SshPublicKey",
            "userName": "xxx",
            "privateKeyPath": "D:\\privatekey_openssh",
            "passPhrase": {
                "type": "SecureString",
                "value": "<pass phrase>"
            }
        },
        "connectVia": {
            "referenceName": "<name of integration runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Örnek 2: özel anahtar içeriği kullanarak SshPublicKey kimlik doğrulaması**

```json
{
    "name": "SftpLinkedService",
    "type": "Linkedservices",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<sftp server>",
            "port": 22,
            "skipHostKeyValidation": true,
            "authenticationType": "SshPublicKey",
            "userName": "<username>",
            "privateKeyContent": {
                "type": "SecureString",
                "value": "<base64 string of the private key content>"
            },
            "passPhrase": {
                "type": "SecureString",
                "value": "<pass phrase>"
            }
        },
        "connectVia": {
            "referenceName": "<name of integration runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. [veri kümeleri](concepts-datasets-linked-services.md) makalesi. 

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Aşağıdaki özellikler, `location` Biçim tabanlı veri kümesindeki ayarlar altında SFTP için desteklenir:

| Özellik   | Açıklama                                                  | Gerekli |
| ---------- | ------------------------------------------------------------ | -------- |
| tür       | DataSet içinde *Type* özelliği `location` *sftplocation*olarak ayarlanmalıdır. | Yes      |
| folderPath | Klasörün yolu. Klasörü filtrelemek için bir joker karakter kullanmak istiyorsanız, bu ayarı atlayın ve etkinlik kaynağı ayarları ' nda yolunu belirtin. | Hayır       |
| fileName   | Belirtilen folderPath altındaki dosya adı. Dosyaları filtrelemek için bir joker karakter kullanmak istiyorsanız, bu ayarı atlayın ve etkinlik kaynağı ayarlarında dosya adını belirtin. | Hayır       |

**Örnek:**

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<SFTP linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, auto retrieved during authoring > ],
        "typeProperties": {
            "location": {
                "type": "SftpLocation",
                "folderPath": "root/folder/subfolder"
            },
            "columnDelimiter": ",",
            "quoteChar": "\"",
            "firstRowAsHeader": true,
            "compressionCodec": "gzip"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. işlem [hatları](concepts-pipelines-activities.md) makalesi. Bu bölüm, SFTP kaynağı tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="sftp-as-source"></a>Kaynak olarak SFTP

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Aşağıdaki özellikler, `storeSettings` Biçim tabanlı kopyalama kaynağındaki ayarlar altında SFTP için desteklenir:

| Özellik                 | Açıklama                                                  | Gerekli                                      |
| ------------------------ | ------------------------------------------------------------ | --------------------------------------------- |
| tür                     | Altındaki *Type* özelliği `storeSettings` *Sftpreadsettings*olarak ayarlanmalıdır. | Yes                                           |
| ***Kopyalanacak dosyaları bulun*** |  |  |
| SEÇENEK 1: statik yol<br> | Veri kümesinde belirtilen klasör/dosya yolundan kopyalama. Tüm dosyaları bir klasörden kopyalamak istiyorsanız, ayrıca olarak öğesini belirtin `wildcardFileName` `*` . |  |
| Seçenek 2: joker karakter<br>-Yavaya Cardfolderpath | Kaynak klasörlerin filtreleneceği joker karakter içeren klasör yolu. <br>İzin verilen joker karakterler `*` (sıfır veya daha fazla karakterle eşleşir) ve `?` (sıfır veya tek karakterle eşleşir); `^` gerçek klasör adınızın bir joker karakter veya içinde bu kaçış karakteri varsa kaçış için kullanın. <br>Daha fazla örnek için bkz. [klasör ve dosya filtresi örnekleri](#folder-and-file-filter-examples). | Hayır                                            |
| Seçenek 2: joker karakter<br>-Yavaya Cardfilename | Kaynak dosyaları filtrelemek için belirtilen folderPath/Yavao Cardfolderpath altındaki joker karakterlerle dosya adı. <br>İzin verilen joker karakterler `*` (sıfır veya daha fazla karakterle eşleşir) ve `?` (sıfır veya tek karakterle eşleşir); `^` gerçek klasör adınızın joker karakter veya içinde bu kaçış karakteri varsa kaçış için kullanın.  Daha fazla örnek için bkz. [klasör ve dosya filtresi örnekleri](#folder-and-file-filter-examples). | Yes |
| Seçenek 3: dosya listesi<br>-fileListPath | Belirtilen dosya kümesinin kopyalanacağını gösterir. Kopyalamak istediğiniz dosyaların listesini içeren bir metin dosyası üzerine gelin (veri kümesinde yapılandırılan yolun göreli yolu ile her satıra bir dosya).<br/>Bu seçeneği kullandığınızda, veri kümesinde dosya adı belirtmeyin. Daha fazla örnek için bkz. [dosya listesi örnekleri](#file-list-examples). |Hayır |
| ***Ek ayarlar*** |  | |
| öz | Verilerin alt klasörlerden veya yalnızca belirtilen klasörden özyinelemeli olarak okunup okunmadığını gösterir. Özyinelemeli değeri true olarak ayarlandığında ve havuz dosya tabanlı bir deposa, havuzda boş bir klasör veya alt klasör kopyalanmaz veya oluşturulmaz. <br>İzin verilen değerler *true* (varsayılan) ve *false*şeklindedir.<br>Bu özellik, yapılandırdığınızda uygulanmaz `fileListPath` . |Hayır |
| deleteFilesAfterCompletion | Hedef depoya başarıyla taşıdıktan sonra, ikili dosyaların kaynak depodan silinip silinmeyeceğini gösterir. Dosya silme dosya başına yapılır, bu nedenle kopyalama etkinliği başarısız olduğunda, bazı dosyaların hedefe zaten kopyalanmış ve kaynaktan silindiği görürsünüz, diğerleri ise kaynak deposunda hala kalır. <br/>Bu özellik yalnızca, veri kaynağı depolamadaki blob, ADLS 1., ADLS 2., S3, Google bulut depolama, dosya, Azure dosyası, SFTP veya FTP olan ikili kopyalama senaryosunda geçerlidir. Varsayılan değer: false. |Hayır |
| modifiedDatetimeStart    | Dosyalar, *son değiştirilen*özniteliğe göre filtrelenir. <br>Son değiştirilme zamanı, ile arasında ise dosyalar seçilir `modifiedDatetimeStart` `modifiedDatetimeEnd` . Saat, UTC saat dilimine *2018-12-01T05:00:00Z*biçiminde uygulanır. <br> Özellikler NULL olabilir, bu da veri kümesine hiçbir dosya özniteliği filtresinin uygulanmadığı anlamına gelir.  Ne zaman `modifiedDatetimeStart` bir tarih saat değeri olduğunda `modifiedDatetimeEnd` , ancak null ise, son değiştirilen özniteliği DateTime değerinden büyük veya buna eşit olan dosyaların seçildiği anlamına gelir.  Ne zaman `modifiedDatetimeEnd` bir tarih saat değeri olduğunda `modifiedDatetimeStart` , ancak null ise, son değiştirilen özniteliği tarih saat değerinden küçük olan dosyaların seçildiği anlamına gelir.<br/>Bu özellik, yapılandırdığınızda uygulanmaz `fileListPath` . | Hayır                                            |
| modifiedDatetimeEnd      | Yukarıdaki gibi.                                               | Hayır                                            |
| maxConcurrentConnections | Depolama deposuna eşzamanlı olarak bağlanabilecek bağlantı sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlandırmak istediğinizde bir değer belirtin. | Hayır                                            |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromSFTP",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Delimited text input dataset name>",
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
                "type": "DelimitedTextSource",
                "formatSettings":{
                    "type": "DelimitedTextReadSettings",
                    "skipLineCount": 10
                },
                "storeSettings":{
                    "type": "SftpReadSettings",
                    "recursive": true,
                    "wildcardFolderPath": "myfolder*A",
                    "wildcardFileName": "*.csv"
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="sftp-as-a-sink"></a>Havuz olarak SFTP

[!INCLUDE [data-factory-v2-file-sink-formats](../../includes/data-factory-v2-file-sink-formats.md)]

Aşağıdaki özellikler, `storeSettings` Biçim tabanlı bir kopya havuzunda ayarlar altında SFTP için desteklenir:

| Özellik                 | Açıklama                                                  | Gerekli |
| ------------------------ | ------------------------------------------------------------ | -------- |
| tür                     | Altındaki *Type* özelliği `storeSettings` *sftpwritesettings*olarak ayarlanmalıdır. | Yes      |
| copyBehavior             | Kaynak dosya tabanlı bir veri deposundan dosyalar olduğunda kopyalama davranışını tanımlar.<br/><br/>İzin verilen değerler şunlardır:<br/><b>-Preservehierarchy (varsayılan)</b>: Hedef klasördeki dosya hiyerarşisini korur. Kaynak dosyanın kaynak klasöre göreli yolu, hedef dosyanın göreli yoluyla hedef klasöre aynıdır.<br/><b>-DÜZEDEN hiyerarşi</b>: kaynak klasördeki tüm dosyalar hedef klasörün ilk düzeyindedir. Hedef dosyalar otomatik olarak oluşturulan adlara sahiptir. <br/><b>-Mergefiles</b>: kaynak klasördeki tüm dosyaları tek bir dosya ile birleştirir. Dosya adı belirtilmişse, birleştirilmiş dosya adı belirtilen addır. Aksi takdirde, otomatik olarak oluşturulan bir dosya adıdır. | Hayır       |
| maxConcurrentConnections | Depolama deposuna eşzamanlı olarak bağlanabilecek bağlantı sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlandırmak istediğinizde bir değer belirtin. | Hayır       |
| useTempFileRename | Geçici dosyalara yüklenip yüklenmeyeceğini ve yeniden adlandırıp, hedef klasöre veya dosya konumuna doğrudan yazmayı belirtin. Varsayılan olarak, Azure Data Factory ilk olarak geçici dosyalara yazar ve ardından karşıya yükleme tamamlandığında onları yeniden adlandırır. Bu sıra, (1) aynı dosyaya yazma işleminin diğer işlemleriniz varsa ve (2) aktarım sırasında dosyanın orijinal sürümünün mevcut olduğundan emin olmak için, bozuk bir dosyaya neden olabilecek çakışmaların önlenmesine yardımcı olur. SFTP sunucunuz bir yeniden adlandırma işlemini desteklemiyorsa, bu seçeneği devre dışı bırakın ve hedef dosyaya eşzamanlı bir yazma işlemi olmadığından emin olun. Daha fazla bilgi için, bu tablonun sonundaki sorun giderme ipucu 'na bakın. | Hayır. Varsayılan değer *true*'dur. |
| operationTimeout | Her yazma isteğinin SFTP sunucusuna zaman aşımına uğramadan önce bekleme süresi. Varsayılan değer 60 dakikadır (01:00:00).|Hayır |

>[!TIP]
>"UserErrorSftpPathNotFound", "Usererrorsftppermissionreddedildi" veya "SftpOperationFail" hatasını alırsanız, SFTP 'ye veri yazarken ve kullandığınız SFTP kullanıcısı uygun *izinlere sahip olduğunda* , SFTP sunucunuzun dosya yeniden adlandırma işleminin çalışıp çalışmadığını denetleyin. Değilse, **geçici dosyayı karşıya yükle** () seçeneğini devre dışı bırakın `useTempFileRename` ve yeniden deneyin. Bu özellik hakkında daha fazla bilgi edinmek için önceki tabloya bakın. Kopyalama etkinliği için şirket içinde barındırılan bir tümleştirme çalışma zamanı kullanıyorsanız, 4,6 veya sonraki bir sürümü kullandığınızdan emin olun.

**Örnek:**

```json
"activities":[
    {
        "name": "CopyToSFTP",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
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
                "type": "<source type>"
            },
            "sink": {
                "type": "BinarySink",
                "storeSettings":{
                    "type": "SftpWriteSettings",
                    "copyBehavior": "PreserveHierarchy"
                }
            }
        }
    }
]
```

### <a name="folder-and-file-filter-examples"></a>Klasör ve dosya filtresi örnekleri

Bu bölümde, klasör yolları ve dosya adlarıyla joker karakter filtreleri kullanmanın sonucu olan davranış açıklanmaktadır.

| folderPath | fileName | öz | Kaynak klasör yapısı ve filtre sonucu ( **kalın** olan dosyalar alınır)|
|:--- |:--- |:--- |:--- |
| `Folder*` | (boş, Varsayılanı kullan) | yanlış | Klasör a<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;** ÜzerindeFile2.js**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ÜzerindeFile4.js<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | (boş, Varsayılanı kullan) | true | Klasör a<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;** ÜzerindeFile2.js**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;** ÜzerindeFile4.js**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | `*.csv` | yanlış | Klasör a<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp; ÜzerindeFile2.js<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ÜzerindeFile4.js<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | `*.csv` | true | Klasör a<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp; ÜzerindeFile2.js<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ÜzerindeFile4.js<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |

### <a name="file-list-examples"></a>Dosya listesi örnekleri

Bu tabloda, kopyalama etkinliği kaynağında bir dosya listesi yolu kullanmanın sonucu olan davranış açıklanmaktadır. Aşağıdaki kaynak klasör yapısına sahip olduğunuzu ve kalın türdeki dosyaları kopyalamak istediğinizi varsayar:

| Örnek kaynak yapısı                                      | FileListToCopy.txt içerik                             | Azure Data Factory Yapılandırması                                            |
| ------------------------------------------------------------ | --------------------------------------------------------- | ------------------------------------------------------------ |
| kök<br/>&nbsp;&nbsp;&nbsp;&nbsp;Klasör a<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ÜzerindeFile2.js<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ÜzerindeFile4.js<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Veriyi<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;FileListToCopy.txt | File1.csv<br>Subfolder1/File3.csv<br>Subfolder1/File5.csv | **Veri kümesinde:**<br>-Klasör yolu: `root/FolderA`<br><br>**Kopyalama etkinliği kaynağı:**<br>-Dosya listesi yolu: `root/Metadata/FileListToCopy.txt` <br><br>Dosya listesi yolu, kopyalamak istediğiniz dosyaların listesini içeren aynı veri deposundaki bir metin dosyasına işaret eder (veri kümesinde yapılandırılan yolun göreli yolu ile her satıra bir dosya). |

## <a name="lookup-activity-properties"></a>Arama etkinliği özellikleri

Arama etkinliği özellikleri hakkında daha fazla bilgi için [Azure Data Factory arama etkinliği](control-flow-lookup-activity.md)bölümüne bakın.

## <a name="getmetadata-activity-properties"></a>GetMetadata etkinlik özellikleri

GetMetadata etkinlik özellikleri hakkında bilgi için bkz. [Azure Data Factory GetMetadata etkinliği](control-flow-get-metadata-activity.md). 

## <a name="delete-activity-properties"></a>Etkinlik özelliklerini Sil

Etkinlik özelliklerini silme hakkında daha fazla bilgi için bkz. [Azure Data Factory etkinliği silme](delete-activity.md).

## <a name="legacy-models"></a>Eski modeller

>[!NOTE]
>Geriye dönük uyumluluk için olduğu gibi aşağıdaki modeller de desteklenir. Azure Data Factory yazma Kullanıcı arabirimi yeni modeli oluşturmak üzere değiştiğinden, daha önce tartışılan yeni modeli kullanmanızı öneririz.

### <a name="legacy-dataset-model"></a>Eski veri kümesi modeli

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | Veri kümesinin *Type* özelliği *FileShare*olarak ayarlanmalıdır. |Yes |
| folderPath | Klasörün yolu. Joker karakter filtresi desteklenir. İzin verilen joker karakterler `*` (sıfır veya daha fazla karakterle eşleşir) ve `?` (sıfır veya tek karakterle eşleşir); `^` gerçek dosya adınızın bir joker karakter veya içinde bu kaçış karakteri varsa kaçış için kullanın. <br/><br/>Örnekler: RootFolder/alt klasör/, bkz. [klasör ve dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek. |Yes |
| fileName |  Belirtilen "folderPath" altındaki dosyalar için **ad veya joker karakter filtresi** . Bu özellik için bir değer belirtmezseniz, veri kümesi klasördeki tüm dosyaları işaret eder. <br/><br/>Filtre için, izin verilen joker karakterler `*` (sıfır veya daha fazla karakterle eşleşir) ve `?` (sıfır veya tek bir karakterle eşleşir).<br/>-Örnek 1: `"fileName": "*.csv"`<br/>-Örnek 2: `"fileName": "???20180427.txt"`<br/>`^`Gerçek klasör adınızın joker karakter veya içinde bu kaçış karakteri varsa kaçış için kullanın. |Hayır |
| modifiedDatetimeStart | Dosyalar, *son değiştirilen*özniteliğe göre filtrelenir. Son değiştirilme zamanı, ile arasında ise dosyalar seçilir `modifiedDatetimeStart` `modifiedDatetimeEnd` . Saat, UTC saat dilimine *2018-12-01T05:00:00Z*biçiminde uygulanır. <br/><br/> Veri hareketinin genel performansı, çok sayıda dosyadan dosya filtresi yapmak istediğinizde bu ayar etkinleştirilerek etkilenecektir. <br/><br/> Özellikler NULL olabilir, bu da veri kümesine hiçbir dosya özniteliği filtresinin uygulanmadığı anlamına gelir.  Ne zaman `modifiedDatetimeStart` bir tarih saat değeri olduğunda `modifiedDatetimeEnd` , ancak null ise, son değiştirilen özniteliği DateTime değerinden büyük veya buna eşit olan dosyaların seçildiği anlamına gelir.  Ne zaman `modifiedDatetimeEnd` bir tarih saat değeri olduğunda `modifiedDatetimeStart` , ancak null ise, son değiştirilen özniteliği tarih saat değerinden küçük olan dosyaların seçildiği anlamına gelir.| Hayır |
| modifiedDatetimeEnd | Dosyalar, *son değiştirilen*özniteliğe göre filtrelenir. Son değiştirilme zamanı, ile arasında ise dosyalar seçilir `modifiedDatetimeStart` `modifiedDatetimeEnd` . Saat, UTC saat dilimine *2018-12-01T05:00:00Z*biçiminde uygulanır. <br/><br/> Veri hareketinin genel performansı, çok sayıda dosyadan dosya filtresi yapmak istediğinizde bu ayar etkinleştirilerek etkilenecektir. <br/><br/> Özellikler NULL olabilir, bu da veri kümesine hiçbir dosya özniteliği filtresinin uygulanmadığı anlamına gelir.  Ne zaman `modifiedDatetimeStart` bir tarih saat değeri olduğunda `modifiedDatetimeEnd` , ancak null ise, son değiştirilen özniteliği DateTime değerinden büyük veya buna eşit olan dosyaların seçildiği anlamına gelir.  Ne zaman `modifiedDatetimeEnd` bir tarih saat değeri olduğunda `modifiedDatetimeStart` , ancak null ise, son değiştirilen özniteliği tarih saat değerinden küçük olan dosyaların seçildiği anlamına gelir.| Hayır |
| biçim | Dosyaları dosya tabanlı depolarla (ikili kopya) olduğu gibi kopyalamak istiyorsanız hem giriş hem de çıkış veri kümesi tanımlarının biçim bölümünü atlayın.<br/><br/>Dosyaları belirli bir biçimde ayrıştırmak isterseniz, aşağıdaki dosya biçimi türleri desteklenir: *TextFormat*, *jsonformat*, *avroformat*, *Orcformat*ve *parquetformat*. Biçim ' in altındaki *Type* özelliğini bu değerlerden birine ayarlayın. Daha fazla bilgi için bkz. [metin biçimi](supported-file-formats-and-compression-codecs-legacy.md#text-format), [JSON biçimi](supported-file-formats-and-compression-codecs-legacy.md#json-format), [avro Format](supported-file-formats-and-compression-codecs-legacy.md#avro-format), [orc biçimi](supported-file-formats-and-compression-codecs-legacy.md#orc-format)ve [Parquet biçim](supported-file-formats-and-compression-codecs-legacy.md#parquet-format) bölümleri. |Hayır (yalnızca ikili kopya senaryosu için) |
| sıkıştırma | Verilerin türünü ve sıkıştırma düzeyini belirtin. Daha fazla bilgi için bkz. [Desteklenen dosya biçimleri ve sıkıştırma codec bileşenleri](supported-file-formats-and-compression-codecs-legacy.md#compression-support).<br/>Desteklenen türler *gzip*, *söndür*, *bzip2*ve *zipsöndür*.<br/>Desteklenen düzeyler *en iyi* ve *en hızlardır*. |Hayır |

>[!TIP]
>Bir klasörün altındaki tüm dosyaları kopyalamak için, yalnızca *FolderPath* ' i belirtin.<br>Belirtilen bir ada sahip tek bir dosyayı kopyalamak için, klasör bölüm ve dosya adı ile dosya *adını Içeren* *FolderPath* öğesini belirtin.<br>Bir klasörün altına bir dosya alt kümesini kopyalamak için, klasör bölüm ve *dosya adı* ' nı joker karakter filtresi Ile birlikte *FolderPath* belirtin.

>[!NOTE]
>Dosya filtresi için *FileFilter* özelliğini kullanıyorsanız, hala olduğu gibi desteklenir, ancak şimdi *dosya adına* eklenen yeni filtre özelliğini kullanmanızı öneririz.

**Örnek:**

```json
{
    "name": "SFTPDataset",
    "type": "Datasets",
    "properties": {
        "type": "FileShare",
        "linkedServiceName":{
            "referenceName": "<SFTP linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "folder/subfolder/",
            "fileName": "*",
            "modifiedDatetimeStart": "2018-12-01T05:00:00Z",
            "modifiedDatetimeEnd": "2018-12-01T06:00:00Z",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

### <a name="legacy-copy-activity-source-model"></a>Eski kopyalama etkinliği kaynak modeli

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | Kopyalama etkinliği kaynağının *Type* özelliği *filesystemsource* olarak ayarlanmalıdır |Yes |
| öz | Verilerin alt klasörlerden veya yalnızca belirtilen klasörden özyinelemeli olarak okunup okunmadığını gösterir. Özyinelemeli değeri *true* olarak ayarlandığında ve havuz dosya tabanlı bir deposa, boş klasörler ve alt klasörler havuzda kopyalanmaz veya oluşturulmaz.<br/>İzin verilen değerler *doğru* (varsayılan) ve *yanlış* | Hayır |
| maxConcurrentConnections | Aynı anda bir depolama deposuna bağlanabilecek bağlantı sayısı. Yalnızca veri deposuyla eş zamanlı bağlantıları sınırlandırmak istediğinizde bir sayı belirtin. | Hayır |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromSFTP",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SFTP input dataset name>",
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
                "type": "FileSystemSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory içindeki kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
