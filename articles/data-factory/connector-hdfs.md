---
title: Azure Veri Fabrikası'nı kullanarak HDFS'den veri kopyalama
description: Azure Veri Fabrikası ardışık bir ardışık ardışık ardışık ardışık bir kopyalama etkinliği kullanarak desteklenen lavabo veri depolarına bulut veya şirket içi HDFS kaynağından verileri nasıl kopyalaylaydestekleyeceğinizi öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 12/10/2019
ms.author: jingwang
ms.openlocfilehash: 2cd76afa9412e89c57cfb6c357eb164ce5d3d1c4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75830437"
---
# <a name="copy-data-from-hdfs-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak HDFS'den veri kopyalama
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-hdfs-connector.md)
> * [Geçerli sürüm](connector-hdfs.md)

Bu makalede, HDFS sunucusundan verilerin nasıl kopyalanış edilebisolduğu açıklanmıştır. Azure Veri Fabrikası hakkında bilgi edinmek için [giriş makalesini](introduction.md)okuyun.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu HDFS bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matrisi](copy-activity-overview.md) ile [etkinliği](copy-activity-overview.md) kopyalama
- [Arama etkinliği](control-flow-lookup-activity.md)

Özellikle, bu HDFS bağlayıcıdestekler:

- **Windows** (Kerberos) veya **Anonim** kimlik doğrulaması kullanarak dosyaları kopyalama.
- **Webhdfs** protokolü veya **yerleşik DistCp** desteği kullanarak dosyaları kopyalama.
- Dosyaları olduğu gibi kopyalama veya desteklenen dosya biçimleri [ve sıkıştırma codec'leri](supported-file-formats-and-compression-codecs.md)ile dosyaları ayrıştırma/oluşturma.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [data-factory-v2-integration-runtime-requirements](../../includes/data-factory-v2-integration-runtime-requirements.md)]

> [!NOTE]
> Tümleştirme Runtime **TÜM** [ad düğümü sunucusu]:[ad düğümü bağlantı noktası] ve [veri düğümü sunucuları]:[veri düğümü bağlantı noktası] Hadoop kümesinin erişebilirsiniz emin olun. Varsayılan [ad düğümü bağlantı noktası] 50070 ve varsayılan [veri düğümü bağlantı noktası] 50075'tir.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, HDFS'ye özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında ayrıntılar sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler HDFS bağlantılı hizmet için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği ayarlanmalıdır: **Hdfs**. | Evet |
| url |HDFS'nin URL'si |Evet |
| authenticationType | İzin verilen değerler şunlardır: **Anonim**veya **Windows**. <br><br> HDFS bağlayıcısı için **Kerberos kimlik doğrulamasını** kullanmak için, şirket içi ortamınızı buna göre ayarlamak için [bu bölüme](#use-kerberos-authentication-for-hdfs-connector) bakın. |Evet |
| userName |Windows kimlik doğrulaması için kullanıcı adı. Kerberos kimlik doğrulaması `<username>@<domain>.com`için . |Evet (Windows Kimlik Doğrulama için) |
| password |Windows kimlik doğrulama için parola. Bu alanı, Veri Fabrikası'nda güvenli bir şekilde depolamak için SecureString olarak işaretleyin veya [Azure Key Vault'ta depolanan bir gizliye başvurun.](store-credentials-in-key-vault.md) |Evet (Windows Kimlik Doğrulama için) |
| connectVia | Veri deposuna bağlanmak için kullanılacak [Tümleştirme Çalışma Süresi.](concepts-integration-runtime.md) [Önkoşullar](#prerequisites) bölümünden daha fazla bilgi edinin. Belirtilmemişse, varsayılan Azure Tümleştirme Çalışma Süresini kullanır. |Hayır |

**Örnek: Anonim kimlik doğrulamasını kullanarak**

```json
{
    "name": "HDFSLinkedService",
    "properties": {
        "type": "Hdfs",
        "typeProperties": {
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "authenticationType": "Anonymous",
            "userName": "hadoop"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Örnek: Windows kimlik doğrulamasını kullanma**

```json
{
    "name": "HDFSLinkedService",
    "properties": {
        "type": "Hdfs",
        "typeProperties": {
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "authenticationType": "Windows",
            "userName": "<username>@<domain>.com (for Kerberos auth)",
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

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [Datasets](concepts-datasets-linked-services.md) makalesine bakın. 

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Aşağıdaki özellikler, biçim tabanlı veri `location` kümesinde ayarlar altında HDFS için desteklenir:

| Özellik   | Açıklama                                                  | Gerekli |
| ---------- | ------------------------------------------------------------ | -------- |
| type       | Veri kümesinin `location` altındaki tür özelliği **HdfsLocation**olarak ayarlanmalıdır. | Evet      |
| folderPath | Klasöre giden yol. Klasöre filtre açmak için joker karakter kullanmak istiyorsanız, bu ayarı atlayın ve etkinlik kaynağı ayarlarını belirtin. | Hayır       |
| fileName   | Verilen folderPath altında dosya adı. Dosyaları filtrelemek için joker karakter kullanmak istiyorsanız, bu ayarı atlayın ve etkinlik kaynağı ayarlarını belirtin. | Hayır       |

**Örnek:**

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<HDFS linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, auto retrieved during authoring > ],
        "typeProperties": {
            "location": {
                "type": "HdfsLocation",
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

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi [için, Pipelines](concepts-pipelines-activities.md) makalesine bakın. Bu bölümde, HDFS kaynağı tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="hdfs-as-source"></a>Kaynak olarak HDFS

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Aşağıdaki özellikler, biçim tabanlı kopyalama `storeSettings` kaynağındaki ayarlar altında HDFS için desteklenir:

| Özellik                 | Açıklama                                                  | Gerekli                                      |
| ------------------------ | ------------------------------------------------------------ | --------------------------------------------- |
| type                     | Altındaki `storeSettings` tür özelliği **HdfsReadSettings**olarak ayarlanmalıdır. | Evet                                           |
| Özyinelemeli                | Verilerin alt klasörlerden mi yoksa yalnızca belirtilen klasörden mi özyinelemeli olarak okunduğunu gösterir. Özyineleme gerçek olarak ayarlandığında ve lavabo dosya tabanlı bir depo olduğunda, boş bir klasörün veya alt klasörün kopyalanmadığını veya lavaboda oluşturulmadığını unutmayın. İzin verilen değerler **doğru** (varsayılan) ve **yanlıştır.** | Hayır                                            |
| joker KarakterFolderPath       | Kaynak klasörleri filtrelemek için joker karakteriçeren klasör yolu. <br>İzin verilen joker `*` karakterler şunlardır: (sıfır `?` veya daha fazla karakterle eşleşir) ve (sıfır veya tek karakterle eşleşir); gerçek `^` klasör adınızı joker veya bu kaçış char içinde varsa kaçmak için kullanın. <br>Klasör ve [dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek görün. | Hayır                                            |
| jokerKarakterFileName         | Kaynak dosyaları filtrelemek için verilen folderPath/jokerKarakterIn altında joker karakter içeren dosya adı. <br>İzin verilen joker `*` karakterler şunlardır: (sıfır `?` veya daha fazla karakterle eşleşir) ve (sıfır veya tek karakterle eşleşir); gerçek `^` klasör adınızı joker veya bu kaçış char içinde varsa kaçmak için kullanın.  Klasör ve [dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek görün. | Veri `fileName` kümesinde belirtilmemişse evet |
| modifiyeDatetimeBaşlat    | Dosyalar özniteliğe göre filtre: Son Değiştirildi. Son değiştirilen zamanları ile `modifiedDatetimeStart` `modifiedDatetimeEnd`arasındaki zaman aralığında ysa, dosyalar seçilecektir. Bu süre UTC saat dilimine "2018-12-01T05:00:00Z" biçiminde uygulanır. <br> Özellikler NULL olabilir, bu da veri kümesine dosya özniteliği filtresi uygulanamayacağı anlamına gelir.  Datetime değeri ne zaman, `modifiedDatetimeStart` ancak `modifiedDatetimeEnd` NULL olduğunda, son değiştirilen özniteliği datetime değerinden büyük veya eşit olan dosyaların seçileceği anlamına gelir.  Datetime değeri ne zaman, `modifiedDatetimeEnd` ancak `modifiedDatetimeStart` NULL olduğunda, son değiştirilen özniteliği datetime değerinden daha az olan dosyaların seçileceği anlamına gelir. | Hayır                                            |
| modifiedDatetimeEnd      | Yukarıdakiyle aynı.                                               | Hayır                                            |
| distcpAyarlar | HDFS DistCp kullanırken özellik grubu. | Hayır |
| resourceManagerEndpoint | İplik Kaynak Yöneticisi bitiş noktası | DistCp kullanıyorsanız Evet |
| tempScriptPath | Geçici DistCp komut komut komut dosyası depolamak için kullanılan bir klasör yolu. Komut dosyası dosyası dosyası Veri Fabrikası tarafından oluşturulur ve Kopya işi bittikten sonra kaldırılır. | DistCp kullanıyorsanız Evet |
| distcpOptions | DistCp komutuna sağlanan ek seçenekler. | Hayır |
| maxConcurrentConnections | Depolama deposuna aynı anda bağlanacak bağlantıların sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlamak istediğinizde belirtin. | Hayır                                            |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromHDFS",
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
                    "type": "HdfsReadSettings",
                    "recursive": true,
                    "distcpSettings": {
                        "resourceManagerEndpoint": "resourcemanagerendpoint:8088",
                        "tempScriptPath": "/usr/hadoop/tempscript",
                        "distcpOptions": "-m 100"
                    }
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="folder-and-file-filter-examples"></a>Klasör ve dosya filtresi örnekleri

Bu bölümde, klasör yolunun ve dosya adının joker karakter filtreleriyle ortaya çıkan davranışı açıklanmaktadır.

| folderPath | fileName             | Özyinelemeli | Kaynak klasör yapısı ve filtre sonucu **(kalın** renkteki dosyalar alınır) |
| :--------- | :------------------- | :-------- | :----------------------------------------------------------- |
| `Folder*`  | (boş, varsayılan kullanın) | yanlış     | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Dosya1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Dosya2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya6.csv |
| `Folder*`  | (boş, varsayılan kullanın) | true      | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Dosya1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Dosya2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Dosya3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Dosya4.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Dosya5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya6.csv |
| `Folder*`  | `*.csv`              | yanlış     | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Dosya1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya6.csv |
| `Folder*`  | `*.csv`              | true      | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Dosya1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Dosya3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Dosya5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya6.csv |

## <a name="use-distcp-to-copy-data-from-hdfs"></a>HDFS'den verileri kopyalamak için DistCp'yi kullanın

[DistCp,](https://hadoop.apache.org/docs/current3/hadoop-distcp/DistCp.html) Hadoop kümesinde dağıtılmış kopya yapmak için bir Hadoop yerel komut satırı aracıdır. Bir Distcp komutu çalıştırıldığında, önce kopyalanacak tüm dosyaları listeleyecek, Hadoop kümesine birkaç Harita işi oluşturacak ve her Harita işi kaynaktan batmaya ikili kopya yapar.

Dosyaları Azure Blob'a [(aşamalı kopya](copy-activity-performance.md)dahil) veya Azure Veri Gölü Deposu'na kopyalamak için DistCp'yi kullanarak Etkinlik kopyalama desteği, bu durumda kendi kendine barındırılan Tümleştirme Çalışma Süresi'nde çalışmak yerine kümenizin gücünden tam olarak yararlanabiliyor. Özellikle kümeniz çok güçlüyse, daha iyi kopyalama işlenecek şekilde olacaktır. Azure Veri Fabrikası'ndaki yapılandırmanıza bağlı olarak, Kopyalama etkinliği otomatik olarak bir discp komutu oluşturmak, Hadoop kümenize göndermek ve kopyalama durumunu izlemek.

### <a name="prerequisites"></a>Ön koşullar

HDFS'den Azure Blob'a (aşamalı kopya dahil) veya Azure Veri Gölü Deposu'na kadar olan dosyaları kopyalamak için DistCp'yi kullanmak için Hadoop kümenizin aşağıdaki gereksinimleri karşıladığından emin olun:

1. MapReduce ve İplik hizmetleri etkinleştirildi.
2. İplik versiyonu 2.5 veya üzeridir.
3. HDFS sunucusu hedef veri deponuzla entegre edilmiştir - Azure Blob veya Azure Veri Gölü Deposu:

    - Azure Blob FileSystem, Hadoop 2.7'den bu yana yerel olarak desteklenir. Sadece Hadoop env config kavanoz yolu belirtmeniz gerekir.
    - Azure Veri Gölü Deposu Dosya Sistemi Hadoop 3.0.0-alpha1'den başlayarak paketlenir. Hadoop kümeniz bu sürümden daha düşükse, ADLS ile ilgili kavanoz paketlerini (azure-datalake-store.jar) [buradan](https://hadoop.apache.org/releases.html)kümeye el ile almanız ve Hadoop env config'de kavanoz yolunu belirtmeniz gerekir.

4. HDFS'de geçici bir klasör hazırlayın. Bu geçici klasör, DistCp kabuk komut dosyası depolamak için kullanılır, bu nedenle KB düzeyinde yer kaplar.
5. HDFS Bağlantılı Hizmet'te sağlanan kullanıcı hesabının a) İplik'te başvuruda bulunma iznine sahip olduğundan emin olun; b) alt klasör oluşturma iznine sahip, temp klasörün altında dosyaları okuma/yazma.

### <a name="configurations"></a>Yapılandırmalar

Kaynak bölümü [olarak HDFS'deki](#hdfs-as-source) DSCp ile ilgili yapılandırmalara ve örneklere bakın.

## <a name="use-kerberos-authentication-for-hdfs-connector"></a>HDFS konektörü için Kerberos kimlik doğrulamasını kullanma

HDFS bağlayıcısında Kerberos Kimlik Doğrulaması'nı kullanmak için şirket içi ortamı ayarlamak için iki seçenek vardır. Durumunuza daha uygun olanı seçebilirsiniz.
* Seçenek 1: [Kerberos bölgesinde Kendi kendine barındırılan Entegrasyon Runtime makinesine katılın](#kerberos-join-realm)
* Seçenek 2: [Windows etki alanı ile Kerberos diyarı arasında karşılıklı güveni etkinleştirme](#kerberos-mutual-trust)

### <a name="option-1-join-self-hosted-integration-runtime-machine-in-kerberos-realm"></a><a name="kerberos-join-realm"></a>Seçenek 1: Kerberos bölgesinde Kendi kendine barındırılan Entegrasyon Runtime makinesine katılın

#### <a name="requirements"></a>Gereksinimler

* Kendi kendine barındırılan Tümleştirme Runtime makinesinin Kerberos alanına katılması gerekir ve herhangi bir Windows etki alanına katılamaz.

#### <a name="how-to-configure"></a>Yapılandırma

**Kendi kendine barındırılan Entegrasyon Runtime makinesinde:**

1.  Kerberos KDC sunucu ve bölge yapılandırmak için **Ksetup** yardımcı programı çalıştırın.

    Kerberos alemi Windows etki alanından farklı olduğundan, makine bir çalışma grubunun üyesi olarak yapılandırılmalıdır. Bu, Kerberos aleminin ayarlanması ve aşağıdaki gibi bir KDC sunucusu eklenmesiyle elde edilebilir. *gerektiğinde kendi* bölgenizle REALM.COM değiştirin.

            C:> Ksetup /setdomain REALM.COM
            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>

    Bu 2 komutu çalıştırdıktan sonra makineyi **yeniden başlatın.**

2.  Yapılandırmayı **Ksetup** komutuyla doğrulayın. Çıktı gibi olmalıdır:

            C:> Ksetup
            default realm = REALM.COM (external)
            REALM.com:
                kdc = <your_kdc_server_address>

**Azure Veri Fabrikası'nda:**

* HDFS veri kaynağına bağlanmak için **Windows kimlik doğrulamasını** kullanarak HDFS bağlayıcısını Kerberos ana adınız ve parolanızla birlikte yapılandırın. Yapılandırma ayrıntılarıyla ilgili [HDFS Bağlantılı Hizmet özellikleri](#linked-service-properties) bölümünü kontrol edin.

### <a name="option-2-enable-mutual-trust-between-windows-domain-and-kerberos-realm"></a><a name="kerberos-mutual-trust"></a>Seçenek 2: Windows etki alanı ile Kerberos diyarı arasında karşılıklı güveni etkinleştirme

#### <a name="requirements"></a>Gereksinimler

*   Kendi kendine barındırılan Tümleştirme Runtime makinesi bir Windows etki alanına katılmalı.
*   Etki alanı denetleyicisinin ayarlarını güncelleştirmek için izne ihtiyacınız vardır.

#### <a name="how-to-configure"></a>Yapılandırma

> [!NOTE]
> Aşağıdaki öğreticide REALM.COM ve AD.COM gerektiğinde kendi bölgeniz ve etki alanı denetleyiciniz ile değiştirin.

**KDC sunucusunda:**

1. KDC'nin aşağıdaki yapılandırma şablonuna atıfta bulunarak Windows Etki Alanı'na güveneizin vermek için **Krb5.conf** dosyasındaki KDC yapılandırmasını düzenleme. Varsayılan olarak, yapılandırma **/etc/krb5.conf**adresinde bulunur.

           [logging]
            default = FILE:/var/log/krb5libs.log
            kdc = FILE:/var/log/krb5kdc.log
            admin_server = FILE:/var/log/kadmind.log
            
           [libdefaults]
            default_realm = REALM.COM
            dns_lookup_realm = false
            dns_lookup_kdc = false
            ticket_lifetime = 24h
            renew_lifetime = 7d
            forwardable = true
            
           [realms]
            REALM.COM = {
             kdc = node.REALM.COM
             admin_server = node.REALM.COM
            }
           AD.COM = {
            kdc = windc.ad.com
            admin_server = windc.ad.com
           }
            
           [domain_realm]
            .REALM.COM = REALM.COM
            REALM.COM = REALM.COM
            .ad.com = AD.COM
            ad.com = AD.COM
            
           [capaths]
            AD.COM = {
             REALM.COM = .
            }

   Yapılandırmadan sonra KDC hizmetini **yeniden başlatın.**

2. KDC sunucusunda **krbtgt/REALM.COM\@** AD.COM adlı bir müdür hazırlayın:

           Kadmin> addprinc krbtgt/REALM.COM@AD.COM

3. **hadoop.security.auth_to_local** HDFS hizmet yapılandırma `RULE:[1:$1@$0](.*\@AD.COM)s/\@.*//`dosyasında, ekleyin.

**Etki alanı denetleyicisi:**

1.  Bir bölge girişi eklemek için aşağıdaki **Ksetup** komutlarını çalıştırın:

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

2.  Windows Etki Alanı'ndan Kerberos Diyarı'na güven kurun. [şifre] AD.COM ana **krbtgt/REALM.COM\@** için şifredir.

            C:> netdom trust REALM.COM /Domain: AD.COM /add /realm /passwordt:[password]

3.  Kerberos'ta kullanılan şifreleme algoritması'nı seçin.

    1. Varsayılan veya Etkin Etki Alanı İlkesi > Grup İlkesi Nesneleri > Etki Alanı > Grup İlkesi > Yönetimi'ne gidin ve Düzenleme.

    2. Grup **İlkesi Yönetimi Düzenleyicisi** açılır penceresinde, Windows Ayarları > Güvenlik Ayarları > Yerel İlkeler > Güvenlik Seçenekleri > Bilgisayar Yapılandırma > İlkeleri'ne gidin ve **Ağ güvenliğini yapılandırma: Kerberos için izin verilen Şifreleme türlerini yapılandırın.**

    3. KDC'ye bağlanırken kullanmak istediğiniz şifreleme algoritmasını seçin. Genellikle, sadece tüm seçenekleri seçebilirsiniz.

        ![Kerberos için Config Şifreleme Türleri](media/connector-hdfs/config-encryption-types-for-kerberos.png)

    4. Belirli REALM'de kullanılacak şifreleme algoritmasını belirtmek için **Ksetup** komutunu kullanın.

                C:> ksetup /SetEncTypeAttr REALM.COM DES-CBC-CRC DES-CBC-MD5 RC4-HMAC-MD5 AES128-CTS-HMAC-SHA1-96 AES256-CTS-HMAC-SHA1-96

4.  Windows Etki Alanı'nda Kerberos müdürünü kullanmak için etki alanı hesabı ile Kerberos müdürü arasındaki eşlemi oluşturun.

    1. **Etkin Dizin Kullanıcıları ve Bilgisayarları**> Yönetim araçlarını başlatın.

    2. **Gelişmiş Özellikleri** **Görüntüle'yi** > tıklatarak gelişmiş özellikleri yapılandırın.

    3. Eşlemeler oluşturmak istediğiniz hesabı bulun ve **Ad Eşlemeleri'ni** görüntülemek için sağ tıklatın > **Kerberos Adları** sekmesini tıklatın.

    4. Diyardan bir müdür ekle.

        ![Harita Güvenlik Kimliği](media/connector-hdfs/map-security-identity.png)

**Kendi kendine barındırılan Entegrasyon Runtime makinesinde:**

* Bir bölge girişi eklemek için aşağıdaki **Ksetup** komutlarını çalıştırın.

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

**Azure Veri Fabrikası'nda:**

* HDFS veri kaynağına bağlanmak için **Windows kimlik doğrulamasını** kullanarak HDFS bağlayıcısını Etki Alanı Hesabınız veya Kerberos Sorumlusuile birlikte yapılandırın. Yapılandırma ayrıntılarıyla ilgili [HDFS Bağlantılı Hizmet özellikleri](#linked-service-properties) bölümünü kontrol edin.

## <a name="lookup-activity-properties"></a>Arama etkinlik özellikleri

Özellikler hakkında daha fazla bilgi edinmek için [Arama etkinliğini](control-flow-lookup-activity.md)kontrol edin.

## <a name="legacy-models"></a>Eski modeller

>[!NOTE]
>Aşağıdaki modeller hala geriye dönük uyumluluk için olduğu gibi desteklenir. İleriye dönük yukarıdaki bölümlerde belirtilen yeni modeli kullanmanız önerilir ve ADF yazarlı UI yeni modeli oluşturmaya geçti.

### <a name="legacy-dataset-model"></a>Eski dataset modeli

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin tür özelliği şu şekilde ayarlanmalıdır: **FileShare** |Evet |
| folderPath | Klasöre giden yol. Joker karakter filtresi desteklenir, joker `*` karakterlere izin verilir: (sıfır veya daha fazla karakterle eşleşir) ve `?` (sıfır veya tek karakterle eşleşir); gerçek `^` dosya adınızı joker veya bu kaçış char içinde varsa kaçmak için kullanın. <br/><br/>Örnekler: rootfolder/subfolder/, Klasör [ve dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek görün. |Evet |
| fileName |  Belirtilen "folderPath" altındaki dosya(lar) için **ad veya joker karakter filtresi.** Bu özellik için bir değer belirtmezseniz, veri kümesi klasördeki tüm dosyaları işaret edir. <br/><br/>Filtre için izin verilen `*` joker karakterler şunlardır: (sıfır veya daha fazla karakterle eşleşir) ve `?` (sıfır veya tek karakterle eşleşir).<br/>- Örnek 1:`"fileName": "*.csv"`<br/>- Örnek 2:`"fileName": "???20180427.txt"`<br/>Gerçek `^` klasör adınızı joker veya bu kaçış char içinde varsa kaçmak için kullanın. |Hayır |
| modifiyeDatetimeBaşlat | Dosyalar özniteliğe göre filtre: Son Değiştirildi. Son değiştirilen zamanları ile `modifiedDatetimeStart` `modifiedDatetimeEnd`arasındaki zaman aralığında ysa, dosyalar seçilecektir. Bu süre UTC saat dilimine "2018-12-01T05:00:00Z" biçiminde uygulanır. <br/><br/> Büyük miktarda dosya filtresi yapmak istediğinizde bu ayarı etkinleştirerek veri hareketinin genel performansının etkileneceğini unutmayın. <br/><br/> Özellikler, veri kümesine dosya özniteliği filtresi uygulanmayacak anlamına gelen NULL olabilir.  Datetime değeri ne zaman, `modifiedDatetimeStart` ancak `modifiedDatetimeEnd` NULL olduğunda, son değiştirilen özniteliği datetime değerinden büyük veya eşit olan dosyaların seçileceği anlamına gelir.  Datetime değeri ne zaman, `modifiedDatetimeEnd` ancak `modifiedDatetimeStart` NULL olduğunda, son değiştirilen özniteliği datetime değerinden daha az olan dosyaların seçileceği anlamına gelir.| Hayır |
| modifiedDatetimeEnd | Dosyalar özniteliğe göre filtre: Son Değiştirildi. Son değiştirilen zamanları ile `modifiedDatetimeStart` `modifiedDatetimeEnd`arasındaki zaman aralığında ysa, dosyalar seçilecektir. Bu süre UTC saat dilimine "2018-12-01T05:00:00Z" biçiminde uygulanır. <br/><br/> Büyük miktarda dosya filtresi yapmak istediğinizde bu ayarı etkinleştirerek veri hareketinin genel performansının etkileneceğini unutmayın. <br/><br/> Özellikler, veri kümesine dosya özniteliği filtresi uygulanmayacak anlamına gelen NULL olabilir.  Datetime değeri ne zaman, `modifiedDatetimeStart` ancak `modifiedDatetimeEnd` NULL olduğunda, son değiştirilen özniteliği datetime değerinden büyük veya eşit olan dosyaların seçileceği anlamına gelir.  Datetime değeri ne zaman, `modifiedDatetimeEnd` ancak `modifiedDatetimeStart` NULL olduğunda, son değiştirilen özniteliği datetime değerinden daha az olan dosyaların seçileceği anlamına gelir.| Hayır |
| biçim | Dosyaları dosya tabanlı mağazalar (ikili kopya) arasında **olduğu gibi kopyalamak** istiyorsanız, hem giriş hem de çıktı veri kümesi tanımlarında biçim bölümünü atlayın.<br/><br/>Dosyaları belirli bir biçimde ayrıştmak istiyorsanız, aşağıdaki dosya biçimi türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParkquetFormat**. Biçim altındaki **tür** özelliğini bu değerlerden birine ayarlayın. Daha fazla bilgi için [Metin Biçimi,](supported-file-formats-and-compression-codecs-legacy.md#text-format) [Json Formatı,](supported-file-formats-and-compression-codecs-legacy.md#json-format) [Avro Biçimi,](supported-file-formats-and-compression-codecs-legacy.md#avro-format) [Ork Biçimi](supported-file-formats-and-compression-codecs-legacy.md#orc-format)ve [Parke Biçimi](supported-file-formats-and-compression-codecs-legacy.md#parquet-format) bölümlerine bakın. |Hayır (yalnızca ikili kopyalama senaryosu için) |
| sıkıştırma | Verilerin sıkıştırma türünü ve düzeyini belirtin. Daha fazla bilgi için [desteklenen dosya biçimleri ve sıkıştırma codec'lerine](supported-file-formats-and-compression-codecs-legacy.md#compression-support)bakın.<br/>Desteklenen türleri şunlardır: **GZip**, **Deflate**, **BZip2**, ve **ZipDeflate**.<br/>Desteklenen seviyeler şunlardır: **Optimal** ve **En Hızlı.** |Hayır |

>[!TIP]
>Tüm dosyaları bir klasörün altında kopyalamak için **yalnızca folderPath'i** belirtin.<br>Belirli bir ada sahip tek bir dosyayı kopyalamak için **folderPath** klasörünü klasör bölümü ve dosya adı ile **dosya adı** ile belirtin.<br>Bir klasörün altındaki dosyaların alt kümesini kopyalamak için **folderPath** klasörünü klasör bölümüyle ve joker karakter filtresiile **fileName'yi** belirtin.

**Örnek:**

```json
{
    "name": "HDFSDataset",
    "properties": {
        "type": "FileShare",
        "linkedServiceName":{
            "referenceName": "<HDFS linked service name>",
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
| type | Kopyalama etkinlik kaynağının türü özelliği ayarlanmalıdır: **HdfsSource** |Evet |
| Özyinelemeli | Verilerin alt klasörlerden mi yoksa yalnızca belirtilen klasörden mi özyinelemeli olarak okunduğunu gösterir. Özyineleme doğru ayarlandığında ve lavabo dosya tabanlı depolandığında, boş klasör/alt klasör lavaboda kopyalanmaz/oluşturulmaz.<br/>İzin verilen değerler şunlardır: **true** (varsayılan), **yanlış** | Hayır |
| distcpAyarlar | HDFS DistCp kullanırken özellik grubu. | Hayır |
| resourceManagerEndpoint | İplik Kaynak Yöneticisi bitiş noktası | DistCp kullanıyorsanız Evet |
| tempScriptPath | Geçici DistCp komut komut komut dosyası depolamak için kullanılan bir klasör yolu. Komut dosyası dosyası dosyası Veri Fabrikası tarafından oluşturulur ve Kopya işi bittikten sonra kaldırılır. | DistCp kullanıyorsanız Evet |
| distcpOptions | DistCp komutuna sağlanan ek seçenekler. | Hayır |
| maxConcurrentConnections | Depolama deposuna aynı anda bağlanacak bağlantıların sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlamak istediğinizde belirtin. | Hayır |

**Örnek: DistCp kullanarak kopyalama etkinliğinde HDFS kaynağı**

```json
"source": {
    "type": "HdfsSource",
    "distcpSettings": {
        "resourceManagerEndpoint": "resourcemanagerendpoint:8088",
        "tempScriptPath": "/usr/hadoop/tempscript",
        "distcpOptions": "-m 100"
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar
Azure Veri Fabrikası'ndaki kopyalama etkinliği tarafından kaynak ve lavabo olarak desteklenen veri depolarının listesi için [desteklenen veri depolarına](copy-activity-overview.md#supported-data-stores-and-formats)bakın.
