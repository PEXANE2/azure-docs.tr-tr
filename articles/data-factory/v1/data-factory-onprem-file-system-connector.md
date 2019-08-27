---
title: Azure Data Factory kullanarak bir dosya sistemine/veritabanından veri kopyalama | Microsoft Docs
description: Azure Data Factory kullanarak şirket içi bir dosya sistemine veri kopyalamayı öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: ce19f1ae-358e-4ffc-8a80-d802505c9c84
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/13/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 92274f63db78d53bdd0fa3fd440977422be3b4a1
ms.sourcegitcommit: 94ee81a728f1d55d71827ea356ed9847943f7397
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2019
ms.locfileid: "70036276"
---
# <a name="copy-data-to-and-from-an-on-premises-file-system-by-using-azure-data-factory"></a>Azure Data Factory kullanarak şirket içi dosya sistemine veri kopyalama
> [!div class="op_single_selector" title1="Kullandığınız Data Factory hizmeti sürümünü seçin:"]
> * [Sürüm 1](data-factory-onprem-file-system-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-file-system.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız, bkz. [v2 'de dosya sistemi Bağlayıcısı](../connector-file-system.md).


Bu makalede, şirket içi bir dosya sistemine/veritabanından veri kopyalamak için Azure Data Factory kopyalama etkinliğinin nasıl kullanılacağı açıklanmaktadır. Kopyalama etkinliğiyle veri hareketine genel bir bakış sunan [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makalesinde oluşturulur.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="supported-scenarios"></a>Desteklenen senaryolar
Şirket **içi bir dosya sisteminden** aşağıdaki veri depolarına veri kopyalayabilirsiniz:

[!INCLUDE [data-factory-supported-sink](../../../includes/data-factory-supported-sinks.md)]

Aşağıdaki veri depolarından verileri **Şirket içi dosya sistemine**kopyalayabilirsiniz:

[!INCLUDE [data-factory-supported-sources](../../../includes/data-factory-supported-sources.md)]

> [!NOTE]
> Kopyalama etkinliği, kaynak dosyayı başarıyla hedefe kopyalandıktan sonra silmez. Başarılı bir kopyadan sonra kaynak dosyayı silmeniz gerekiyorsa, dosyayı silmek ve işlem hattındaki etkinliği kullanmak için özel bir etkinlik oluşturun.

## <a name="enabling-connectivity"></a>Bağlantı etkinleştiriliyor
Data Factory, **veri yönetimi ağ geçidi**aracılığıyla şirket içi bir dosya sistemine bağlanmayı destekler. Dosya sistemi dahil desteklenen herhangi bir şirket içi veri deposuna bağlanmak için Data Factory hizmetinin şirket içi ortamınıza Veri Yönetimi ağ geçidini yüklemelisiniz. Veri Yönetimi ağ geçidi hakkında bilgi edinmek ve ağ geçidini ayarlamaya yönelik adım adım yönergeler için bkz. [veri yönetimi ağ geçidi ile şirket içi kaynaklar ve bulut arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md). Veri Yönetimi ağ geçidi dışında, şirket içi bir dosya sisteminden ve bu bilgisayardan iletişim kurmak için başka hiçbir ikili dosyanın yüklenmesi gerekmez. Dosya sistemi Azure IaaS VM 'sinde olsa bile Veri Yönetimi ağ geçidini yüklemeli ve kullanmanız gerekir. Ağ Geçidi hakkında ayrıntılı bilgi için bkz. [veri yönetimi Gateway](data-factory-data-management-gateway.md).

Linux dosya paylaşımının kullanılması için, Linux sunucunuza [Samba](https://www.samba.org/) 'yi yükledikten sonra bir Windows sunucusuna veri yönetimi ağ geçidi yüklersiniz. Linux sunucusuna Veri Yönetimi ağ geçidi yükleme desteklenmez.

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API 'Ler kullanarak bir dosya sistemine/veritabanından veri taşıyan kopyalama etkinliğiyle bir işlem hattı oluşturabilirsiniz.

İşlem hattı oluşturmanın en kolay yolu **Kopyalama Sihirbazı**' nı kullanmaktır. Öğreticiye bakın [: Veri kopyalama Sihirbazı 'nı kullanarak işlem](data-factory-copy-data-wizard-tutorial.md) hattı oluşturma hakkında hızlı bir yol için kopyalama Sihirbazı 'nı kullanarak bir işlem hattı oluşturun.

İşlem hattı oluşturmak için aşağıdaki araçları da kullanabilirsiniz: **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**ve **REST API**. Bkz: [kopyalama etkinliği Öğreticisi](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyalama etkinliği ile işlem hattı oluşturmak adım adım yönergeler için.

Araçları veya API 'Leri kullanıp kullanmayacağınızı bir kaynak veri deposundan havuz veri deposuna veri taşınan bir işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirirsiniz:

1. Bir **Veri Fabrikası**oluşturun. Bir veri fabrikası bir veya daha fazla işlem hattı içerebilir.
2. Giriş ve çıkış veri depolarını veri fabrikanıza bağlamak için **bağlı hizmetler** oluşturun. Örneğin, bir Azure Blob depolamadan şirket içi dosya sistemine veri kopyalıyorsanız, şirket içi dosya sisteminizi ve Azure depolama hesabınızı veri fabrikasına bağlamak için iki bağlı hizmet oluşturursunuz. Şirket içi bir dosya sistemine özgü bağlantılı hizmet özellikleri için bkz. [bağlı hizmet özellikleri](#linked-service-properties) bölümü.
3. Kopyalama işlemi için girdi ve çıktı verilerini temsil edecek **veri kümeleri** oluşturun. Son adımda bahsedilen örnekte, blob kapsayıcısını ve girdi verilerini içeren klasörü belirtmek için bir veri kümesi oluşturursunuz. Dosya sisteminizde klasör ve dosya adı (isteğe bağlı) belirtmek için başka bir veri kümesi oluşturursunuz. Şirket içi dosya sistemine özgü veri kümesi özellikleri için bkz. [veri kümesi özellikleri](#dataset-properties) bölümü.
4. Bir veri kümesini girdi olarak ve bir veri kümesini çıkış olarak alan kopyalama etkinliği ile bir işlem **hattı** oluşturun. Daha önce bahsedilen örnekte, BlobSource 'u kaynak olarak, kopyalama etkinliği için bir havuz olarak bir havuz olarak kullanırsınız. Benzer şekilde, şirket içi dosya sisteminden Azure Blob depolama alanına kopyalama yapıyorsanız, kopyalama etkinliğinde FileSystemSource ve BlobSink kullanın. Şirket içi dosya sistemine özgü kopyalama etkinliği özellikleri için bkz. [kopyalama etkinliği özellikleri](#copy-activity-properties) bölümü. Bir veri deposunu kaynak veya havuz olarak kullanma hakkında ayrıntılı bilgi için, veri deponuzdaki önceki bölümde yer alan bağlantıya tıklayın.

Sihirbazı kullandığınızda, bu Data Factory varlıkların JSON tanımları (bağlı hizmetler, veri kümeleri ve işlem hattı) sizin için otomatik olarak oluşturulur. Araçlar/API 'Leri (.NET API hariç) kullandığınızda, bu Data Factory varlıkları JSON biçimini kullanarak tanımlarsınız.  Bir dosya sistemine/dosyasından veri kopyalamak için kullanılan Data Factory varlıkları için JSON tanımları içeren örnekler için, bu makalenin [JSON örnekleri](#json-examples-for-copying-data-to-and-from-file-system) bölümüne bakın.

Aşağıdaki bölümler, dosya sistemine özgü Data Factory varlıkları tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar:

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri
Şirket içi **dosya sunucusu** bağlı hizmeti Ile bir Azure Data Factory 'ye şirket içi dosya sistemi bağlayabilirsiniz. Aşağıdaki tabloda, şirket Içi dosya sunucusu bağlantılı hizmetine özgü JSON öğelerine yönelik açıklamalar sağlanmaktadır.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Type özelliğinin **OnPremisesFileServer**olarak ayarlandığından emin olun. |Evet |
| host |Kopyalamak istediğiniz klasörün kök yolunu belirtir. Dizedeki özel karakterler için ' \ ' kaçış karakterini kullanın. Bkz: [örnek bağlantılı hizmet ve veri kümesi tanımları](#sample-linked-service-and-dataset-definitions) örnekler. |Evet |
| userid |Sunucu erişimi olan kullanıcının kimliği belirtin. |Hayır (encryptedCredential öğesini seçerseniz) |
| password |(Kullanıcı kimliği) kullanıcının parolasını belirtin. |Hayır (encryptedCredential seçeneğini belirlerseniz |
| encryptedCredential |New-AzDataFactoryEncryptValue cmdlet 'ini çalıştırarak alabileceğiniz şifrelenmiş kimlik bilgilerini belirtin. |Hayır (Kullanıcı kimliği ve parolayı düz metin olarak belirtmeyi seçerseniz) |
| gatewayName |Data Factory şirket içi dosya sunucusuna bağlanmak için kullanması gereken ağ geçidinin adını belirtir. |Evet |


### <a name="sample-linked-service-and-dataset-definitions"></a>Bağlı hizmet ve veri kümesi tanımları örneği
| Senaryo | Bağlı hizmet tanımında ana bilgisayar | veri kümesi tanımında folderPath |
| --- | --- | --- |
| Veri Yönetimi ağ geçidi makinesindeki yerel klasör: <br/><br/>Örnekler: D:\\ \* veya d:\folder\alt klasörü\\\* |D:\\ \\ (veri yönetimi Gateway 2,0 ve üzeri sürümler için) <br/><br/> localhost (Veri Yönetimi Gateway 'den önceki sürümlerde 2,0) |. \\ veyaklasör\\alt klasörü (veri yönetimi Gateway 2,0 ve üzeri sürümler için)\\ \\ <br/><br/>D:\\ \\veyad\\: Folderaltklasörü\\(2,0 altındaki Ağ Geçidi sürümü için)\\\\ |
| Paylaşılan uzak klasör: <br/><br/>Örnekler: \\ \\sunucumShare\\*veya\\sunucum Share klasöralt\\klasörü \\\\\\ \\\\\\* |\\\\\\\\sunucum\\paylaşma\\ |. \\ veyaklasör\\alt klasörü \\ \\ |

>[!NOTE]
>Kullanıcı Arabirimi yazılırken çift ters eğik çizgi giriş gerekmez (`\\`) gibi JSON erişmek için tek bir ters eğik çizgi belirtin.

### <a name="example-using-username-and-password-in-plain-text"></a>Örnek: Düz metinde Kullanıcı adı ve parola kullanma

```JSON
{
  "Name": "OnPremisesFileServerLinkedService",
  "properties": {
    "type": "OnPremisesFileServer",
    "typeProperties": {
      "host": "\\\\Contosogame-Asia",
      "userid": "Admin",
      "password": "123456",
      "gatewayName": "mygateway"
    }
  }
}
```

### <a name="example-using-encryptedcredential"></a>Örnek: Encryptedcredential kullanma

```JSON
{
  "Name": " OnPremisesFileServerLinkedService ",
  "properties": {
    "type": "OnPremisesFileServer",
    "typeProperties": {
      "host": "D:\\",
      "encryptedCredential": "WFuIGlzIGRpc3Rpbmd1aXNoZWQsIG5vdCBvbmx5IGJ5xxxxxxxxxxxxxxxxx",
      "gatewayName": "mygateway"
    }
  }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Veri kümelerini tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. [veri kümeleri oluşturma](data-factory-create-datasets.md). Bir veri kümesinin yapısı, kullanılabilirliği ve İlkesi gibi bölümler, tüm veri kümesi türleri için benzerdir.

TypeProperties bölümü her bir veri kümesi türü için farklıdır. Veri deposundaki verilerin konumu ve biçimi gibi bilgiler sağlar. **FileShare** türündeki veri kümesinin typeproperties bölümü aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| folderPath |Klasörün alt yol belirtir. Dizedeki özel karakterler için '\' kaçış karakterini kullanın. Joker karakter filtresi desteklenmez. Bkz: [örnek bağlantılı hizmet ve veri kümesi tanımları](#sample-linked-service-and-dataset-definitions) örnekler.<br/><br/>Bu özelliği, dilim başlangıç/bitiş tarihi-saati temelinde klasör yolları sağlamak için **Partitionby** ile birleştirebilirsiniz. |Evet |
| fileName |Tablonun klasördeki belirli bir dosyaya başvurmasını istiyorsanız, **FolderPath** içindeki dosyanın adını belirtin. Bu özellik için herhangi bir değer belirtmezseniz tablo, klasördeki tüm dosyaları gösterir.<br/><br/>Bir çıkış veri kümesi için **dosya adı** belirtilmediğinde ve etkinlik havuzunda **preservehierarchy** belirtilmemişse, oluşturulan dosyanın adı şu biçimdedir: <br/><br/>`Data.<Guid>.txt`Örneğinde Data. 0a405f8a-93ff-4c6f-B3BE-f69616f1df7a. txt) |Hayır |
| fileFilter |Tüm dosyalar yerine folderPath içindeki dosyaların bir alt kümesini seçmek için kullanılacak bir filtre belirtin. <br/><br/>İzin verilen değerler: `*` (birden çok karakter) `?` ve (tek karakter).<br/><br/>Örnek 1: "fileFilter": "*. log"<br/>Örnek 2: "fileFilter": 2014-1-?. txt<br/><br/>FileFilter 'nin bir giriş FileShare veri kümesi için geçerli olduğunu unutmayın. |Hayır |
| partitionedBy |Zaman serisi verileri için dinamik bir folderPath/fileName belirtmek üzere partitionedBy ' i kullanabilirsiniz. Her saat veri için folderPath parametreli bir örnektir. |Hayır |
| format | Aşağıdaki biçim türleri desteklenir: **TextFormat**, **jsonformat**, **avroformat**, **orcformat**, **parquetformat**. Ayarlama **türü** özelliği şu değerlerden biri olarak biçimine altında. Daha fazla bilgi için [metin biçimi](data-factory-supported-file-and-compression-formats.md#text-format), [Json biçimine](data-factory-supported-file-and-compression-formats.md#json-format), [Avro biçimi](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc biçimi](data-factory-supported-file-and-compression-formats.md#orc-format), ve [Parquetbiçimi](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümler. <br><br> İsterseniz **olarak dosya kopyalama-olan** dosya tabanlı depoları arasında (ikili kopya), her iki girdi ve çıktı veri kümesi tanımları biçimi bölümünde atlayın. |Hayır |
| compression | Veri sıkıştırma düzeyi ve türünü belirtin. Desteklenen türler şunlardır: **Gzip**, **söndür**, **bzip2**ve **zipsöndür**. Desteklenen düzeyler şunlardır: **En iyi** ve **en hızlı**. [Azure Data Factory dosya ve sıkıştırma biçimlerine](data-factory-supported-file-and-compression-formats.md#compression-support)bakın. |Hayır |

> [!NOTE]
> Dosya adı ve fileFilter 'yi aynı anda kullanamazsınız.

### <a name="using-partitionedby-property"></a>PartitionedBy özelliğini kullanma
Önceki bölümde belirtildiği gibi, **Partitionedby** özelliği, [Data Factory işlevleri ve sistem değişkenleri](data-factory-functions-variables.md)ile zaman serisi verileri için dinamik bir FolderPath ve filename belirtebilirsiniz.

Zaman serisi veri kümeleri, zamanlama ve dilimleri hakkında daha fazla ayrıntıyı anlamak için bkz. [veri kümeleri](data-factory-create-datasets.md), [zamanlama ve yürütme](data-factory-scheduling-and-execution.md)ve işlem [hatları](data-factory-create-pipelines.md)oluşturma.

#### <a name="sample-1"></a>Örnek 1:

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```

Bu örnekte, {Slice}, (YYYYMMDDHH) biçimindeki Data Factory sistem değişkeninin değeri ile değiştirilmiştir. Dilimcstart, dilimin başlangıç zamanına başvurur. FolderPath her bir dilim için farklıdır. Örneğin: wikidatagateway/wisvahili amptadataout/2014100103 veya wikidatagateway/wisvahili amptadataout/2014100104.

#### <a name="sample-2"></a>Örnek 2:

```JSON
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

Bu örnekte, monthestart 'ın Year, month, Day ve Time, folderPath ve fileName özelliklerinin kullandığı ayrı değişkenlere ayıklanır.

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Etkinlikleri tanımlamaya yönelik bölüm & özelliklerinin tam listesi için, işlem [hatları oluşturma](data-factory-create-pipelines.md) makalesine bakın. Ad, açıklama, giriş ve çıkış veri kümeleri ve ilkeler gibi özellikler, tüm etkinlik türleri için kullanılabilir. Ancak, etkinliğin **typeproperties** bölümünde kullanılabilen özellikler her etkinlik türüyle farklılık gösterir.

Kopyalama etkinliği için, kaynak ve havuz türlerine göre farklılık gösterir. Şirket içi bir dosya sisteminden veri taşıyorsanız, kopyalama etkinliğinde kaynak türünü **Filesystemsource**olarak ayarlarsınız. Benzer şekilde, verileri şirket içi bir dosya sistemine taşıyorsanız, kopyalama etkinliğindeki havuz türünü **Filesystemmsink**olarak ayarlarsınız. Bu bölüm, FileSystemSource ve Filesystemmsink tarafından desteklenen özelliklerin bir listesini sağlar.

**Filesystemsource** aşağıdaki özellikleri destekler:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| recursive |Belirtilen klasörün alt klasörleri ya da yalnızca veri yinelemeli olarak okunur olup olmadığını belirtir. |True, false (varsayılan) |Hayır |

**Filesystemmsink** aşağıdaki özellikleri destekler:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| copyBehavior |Kaynak BlobSource veya FileSystem olduğunda kopyalama davranışını tanımlar. |**PreserveHierarchy:** Hedef klasördeki dosya hiyerarşisini korur. Diğer bir deyişle, kaynak dosyanın kaynak klasöre göreli yolu hedef klasöre hedef dosyanın göreli yoluyla aynıdır.<br/><br/>**DÜZEDEN hiyerarşisi:** Kaynak klasördeki tüm dosyalar hedef klasörün ilk düzeyinde oluşturulur. Hedef dosyalar, otomatik olarak oluşturulan bir adla oluşturulur.<br/><br/>**MergeFiles** Kaynak klasördeki tüm dosyaları tek bir dosya ile birleştirir. Dosya adı/blob adı belirtilmişse, birleştirilmiş dosya adı belirtilen addır. Aksi takdirde, otomatik olarak oluşturulan bir dosya adıdır. |Hayır |

### <a name="recursive-and-copybehavior-examples"></a>özyinelemeli ve copyBehavior örnekleri
Bu bölümde, özyinelemeli ve copyBehavior özelliklerine yönelik farklı değer birleşimleri için kopyalama işleminin ortaya çıkan davranışı açıklanmaktadır.

| özyinelemeli değer | copyBehavior değeri | Sonuç davranışı |
| --- | --- | --- |
| true |preserveHierarchy |Aşağıdaki yapıyla Klasör1 kaynak klasörü için<br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>hedef klasör Klasör1, kaynak ile aynı yapıyla oluşturulur:<br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 |
| true |flattenHierarchy |Aşağıdaki yapıyla Klasör1 kaynak klasörü için<br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>hedef Klasör1 aşağıdaki yapıyla oluşturulur: <br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de otomatik olarak oluşturulan adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya2 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya3 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File4 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File5 için otomatik olarak oluşturulan ad |
| true |mergeFiles |Aşağıdaki yapıyla Klasör1 kaynak klasörü için<br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>hedef Klasör1 aşağıdaki yapıyla oluşturulur: <br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;FILE1 + dosya2 + File3 + File4 + dosya 5 içerikleri, otomatik olarak oluşturulan dosya adına sahip tek bir dosyada birleştirilir. |
| false |preserveHierarchy |Aşağıdaki yapıyla Klasör1 kaynak klasörü için<br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Klasör1 hedef klasörü aşağıdaki yapıyla oluşturulur:<br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/><br/>Subfolder1 dosya3 File4 ve File5 ile toplanmış değil. |
| false |flattenHierarchy |Aşağıdaki yapıyla Klasör1 kaynak klasörü için<br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Klasör1 hedef klasörü aşağıdaki yapıyla oluşturulur:<br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de otomatik olarak oluşturulan adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya2 için otomatik olarak oluşturulan ad<br/><br/>Subfolder1 dosya3 File4 ve File5 ile toplanmış değil. |
| false |mergeFiles |Aşağıdaki yapıyla Klasör1 kaynak klasörü için<br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Klasör1 hedef klasörü aşağıdaki yapıyla oluşturulur:<br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;FILE1 + dosya2 içerikleri, otomatik olarak oluşturulan dosya adı ile tek bir dosyada birleştirilir.<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1 için otomatik olarak oluşturulan ad<br/><br/>Subfolder1 dosya3 File4 ve File5 ile toplanmış değil. |

## <a name="supported-file-and-compression-formats"></a>Desteklenen dosya ve sıkıştırma biçimleri
Ayrıntılar hakkında [Azure Data Factory makalesinde dosya ve sıkıştırma biçimlerine](data-factory-supported-file-and-compression-formats.md) bakın.

## <a name="json-examples-for-copying-data-to-and-from-file-system"></a>Dosya sistemine veya dosya sistemine veri kopyalamaya yönelik JSON örnekleri
Aşağıdaki örnekler, [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)kullanarak bir işlem hattı oluşturmak için kullanabileceğiniz örnek JSON tanımlarını sağlar. Şirket içi dosya sistemine ve Azure Blob depolama 'ya veri kopyalamayı gösterir. Ancak, Azure Data Factory ' deki kopyalama etkinliğini kullanarak, kaynakları, [desteklenen kaynaklarda ve havuzlar](data-factory-data-movement-activities.md#supported-data-stores-and-formats) bölümünde listelenen herhangi bir havuza *doğrudan* kopyalayabilirsiniz.

### <a name="example-copy-data-from-an-on-premises-file-system-to-azure-blob-storage"></a>Örnek: Şirket içi dosya sisteminden Azure Blob depolama alanına veri kopyalama
Bu örnek, şirket içi bir dosya sisteminden Azure Blob depolamaya nasıl veri kopyalanacağını gösterir. Örnek aşağıdaki Data Factory varlıklara sahiptir:

* [OnPremisesFileServer](#linked-service-properties)türünde bağlı bir hizmet.
* [Azurestorage](data-factory-azure-blob-connector.md#linked-service-properties)türünde bağlı bir hizmet.
* [FileShare](#dataset-properties)türünde bir giriş [veri kümesi](data-factory-create-datasets.md) .
* [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)türünde bir çıkış [veri kümesi](data-factory-create-datasets.md) .
* [Filesystemsource](#copy-activity-properties) ve [Blobsink](data-factory-azure-blob-connector.md#copy-activity-properties)kullanan kopyalama etkinliğine sahip bir işlem [hattı](data-factory-create-pipelines.md) .

Aşağıdaki örnek, saat serisi verilerini şirket içi bir dosya sisteminden her saat Azure Blob depolama alanına kopyalar. Bu örneklerde kullanılan JSON özellikleri, örneklerden sonraki bölümlerde açıklanmıştır.

İlk adım olarak, [veri yönetimi ağ geçidi ile şirket içi kaynaklar ve bulut arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md)bölümündeki yönergelere göre veri yönetimi ağ geçidini ayarlayın.

**Şirket Içi dosya sunucusu bağlı hizmeti:**

```JSON
{
  "Name": "OnPremisesFileServerLinkedService",
  "properties": {
    "type": "OnPremisesFileServer",
    "typeProperties": {
      "host": "\\\\Contosogame-Asia.<region>.corp.<company>.com",
      "userid": "Admin",
      "password": "123456",
      "gatewayName": "mygateway"
    }
  }
}
```

**Kullanıcı kimliği** ve **parola** özellikleri yerine **encryptedcredential** özelliğinin kullanılması önerilir. Bu bağlı hizmet hakkındaki ayrıntılar için bkz. [dosya sunucusu bağlı hizmeti](#linked-service-properties) .

**Azure depolama bağlı hizmeti:**

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```

**Şirket içi dosya sistemi giriş veri kümesi:**

Veriler her saat yeni bir dosyadan alınır. FolderPath ve fileName özellikleri, dilimin başlangıç saatine göre belirlenir.

Data Factory, veri kümesinin veri fabrikasında dış olduğunu ve veri fabrikasındaki bir etkinlik tarafından üretilmediğini bildirir.`"external": "true"`

```JSON
{
  "name": "OnpremisesFileSystemInput",
  "properties": {
    "type": " FileShare",
    "linkedServiceName": " OnPremisesFileServerLinkedService ",
    "typeProperties": {
      "folderPath": "mysharedfolder/yearno={Year}/monthno={Month}/dayno={Day}",
      "fileName": "{Hour}.csv",
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
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      "externalData": {
        "retryInterval": "00:01:00",
        "retryTimeout": "00:10:00",
        "maximumRetry": 3
      }
    }
  }
}
```

**Azure Blob depolama çıkış veri kümesi:**

Veriler her saat yeni bir bloba yazılır (sıklık: saat, Aralık: 1). Blob 'un klasör yolu, işlenmekte olan dilimin başlangıç zamanına göre dinamik olarak değerlendirilir. Klasör yolu başlangıç zamanının yıl, ay, gün ve saat kısımlarını kullanır.

```JSON
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": "\t",
        "rowDelimiter": "\n"
      }
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

**Dosya sistemi kaynağına ve BLOB havuzuna sahip bir işlem hattındaki kopyalama etkinliği:**

İşlem hattı, giriş ve çıkış veri kümelerini kullanmak üzere yapılandırılmış bir kopyalama etkinliği içerir ve her saat çalışacak şekilde zamanlanır. Ardışık düzen JSON tanımında **kaynak** türü **filesystemsource**olarak ayarlanır ve **Havuz** türü **blobsink**olarak ayarlanır.

```JSON
{
  "name":"SamplePipeline",
  "properties":{
    "start":"2015-06-01T18:00:00",
    "end":"2015-06-01T19:00:00",
    "description":"Pipeline for copy activity",
    "activities":[
      {
        "name": "OnpremisesFileSystemtoBlob",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "OnpremisesFileSystemInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
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
          "executionPriorityOrder": "OldestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
    ]
  }
}
```

### <a name="example-copy-data-from-azure-sql-database-to-an-on-premises-file-system"></a>Örnek: Azure SQL veritabanından şirket içi dosya sistemine veri kopyalama
Aşağıdaki örnek şunu gösterir:

* [Azuressqldatabase](data-factory-azure-sql-connector.md#linked-service-properties) türünde bağlı bir hizmet.
* [OnPremisesFileServer](#linked-service-properties)türünde bağlı bir hizmet.
* [Azuressqltable](data-factory-azure-sql-connector.md#dataset-properties)türünde bir giriş veri kümesi.
* [FileShare](#dataset-properties)türünde bir çıkış veri kümesi.
* [SQLSource](data-factory-azure-sql-connector.md##copy-activity-properties) ve [filesystemmsink](#copy-activity-properties)kullanan kopyalama etkinliğine sahip bir işlem hattı.

Örnek, bir Azure SQL tablosundan saat serisi verilerini her saat için bir şirket içi dosya sistemine kopyalar. Bu örneklerde kullanılan JSON özellikleri, örneklerden sonraki bölümlerde açıklanmıştır.

**Azure SQL veritabanı bağlı hizmeti:**

```JSON
{
  "name": "AzureSqlLinkedService",
  "properties": {
    "type": "AzureSqlDatabase",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```

**Şirket Içi dosya sunucusu bağlı hizmeti:**

```JSON
{
  "Name": "OnPremisesFileServerLinkedService",
  "properties": {
    "type": "OnPremisesFileServer",
    "typeProperties": {
      "host": "\\\\Contosogame-Asia.<region>.corp.<company>.com",
      "userid": "Admin",
      "password": "123456",
      "gatewayName": "mygateway"
    }
  }
}
```

**UserID** ve **Password** özelliklerini kullanmak yerine **encryptedcredential** özelliğinin kullanılması önerilir. Bu bağlı hizmet hakkındaki ayrıntılar için bkz. [dosya sistemi bağlı hizmeti](#linked-service-properties) .

**Azure SQL giriş veri kümesi:**

Örnek, Azure SQL 'de bir "MyTable" tablosu oluşturduğunuzu ve zaman serisi verileri için "timestampcolumn" adlı bir sütun içerdiğini varsayar.

Data Factory, veri kümesinin veri fabrikasında dış olduğunu ve veri fabrikasındaki bir etkinlik tarafından üretilmediğini bildirir.``“external”: ”true”``

```JSON
{
  "name": "AzureSqlInput",
  "properties": {
    "type": "AzureSqlTable",
    "linkedServiceName": "AzureSqlLinkedService",
    "typeProperties": {
      "tableName": "MyTable"
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      "externalData": {
        "retryInterval": "00:01:00",
        "retryTimeout": "00:10:00",
        "maximumRetry": 3
      }
    }
  }
}
```

**Şirket içi dosya sistemi çıkış veri kümesi:**

Veriler her saat yeni bir dosyaya kopyalanır. Blobun için folderPath ve fileName, dilimin başlangıç saatine göre belirlenir.

```JSON
{
  "name": "OnpremisesFileSystemOutput",
  "properties": {
    "type": "FileShare",
    "linkedServiceName": " OnPremisesFileServerLinkedService ",
    "typeProperties": {
      "folderPath": "mysharedfolder/yearno={Year}/monthno={Month}/dayno={Day}",
      "fileName": "{Hour}.csv",
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
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      "externalData": {
        "retryInterval": "00:01:00",
        "retryTimeout": "00:10:00",
        "maximumRetry": 3
      }
    }
  }
}
```

**SQL kaynak ve dosya sistemi havuzu ile bir işlem hattındaki kopyalama etkinliği:**

İşlem hattı, giriş ve çıkış veri kümelerini kullanmak üzere yapılandırılmış bir kopyalama etkinliği içerir ve her saat çalışacak şekilde zamanlanır. İşlem hattı JSON tanımında **kaynak** türü **SQLSource**olarak ayarlanır ve **Havuz** türü **filesystemmsink**olarak ayarlanır. **Sqlreaderquery** özelliği IÇIN belirtilen SQL sorgusu, kopyalamanın Son saatteki verilerini seçer.

```JSON
{
  "name":"SamplePipeline",
  "properties":{
    "start":"2015-06-01T18:00:00",
    "end":"2015-06-01T20:00:00",
    "description":"pipeline for copy activity",
    "activities":[
      {
        "name": "AzureSQLtoOnPremisesFile",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureSQLInput"
          }
        ],
        "outputs": [
          {
            "name": "OnpremisesFileSystemOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "SqlSource",
            "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd}\\'', WindowStart, WindowEnd)"
          },
          "sink": {
            "type": "FileSystemSink"
          }
        },
       "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 3,
          "timeout": "01:00:00"
        }
      }
    ]
  }
}
```

Ayrıca, kaynak veri kümesindeki sütunları, kopyalama etkinliği tanımındaki havuz veri kümesinden sütunlara eşleyebilirsiniz. Ayrıntılar için bkz. [Azure Data Factory veri kümesi sütunlarını eşleme](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Performans ve ayar
 Azure Data Factory ve en iyileştirmek için çeşitli yollarla veri taşıma (kopyalama etkinliği) performansını etkileyen anahtar faktörleri hakkında bilgi edinmek için bkz. [kopyalama etkinliği performansı ve ayarlama Kılavuzu](data-factory-copy-activity-performance.md).
