---
title: Azure Veri Fabrikası'nı kullanarak verileri dosya sistemine kopyalama
description: Azure Veri Fabrikası'nı kullanarak şirket içi dosya sistemine verileri nasıl kopyalayıp kopyalayarak kopyalayabildiğini öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.assetid: ce19f1ae-358e-4ffc-8a80-d802505c9c84
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 04/13/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: d298c83c0c1a0f33f28644e2e467ad5035300221
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79265941"
---
# <a name="copy-data-to-and-from-an-on-premises-file-system-by-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak verileri şirket içi dosya sistemine kopyalayın
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](data-factory-onprem-file-system-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-file-system.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [V2'deki Dosya Sistemi bağlayıcısı bölümüne](../connector-file-system.md)bakın.


Bu makalede, şirket içi bir dosya sistemine/verileri kopyalamak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Kopya etkinliğiyle birlikte veri hareketine genel bir genel bakış sunan [Veri Hareketi Etkinlikleri](data-factory-data-movement-activities.md) makalesine dayanmaktadır.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="supported-scenarios"></a>Desteklenen senaryolar
Şirket içi **dosya sistemindeki** verileri aşağıdaki veri depolarına kopyalayabilirsiniz:

[!INCLUDE [data-factory-supported-sink](../../../includes/data-factory-supported-sinks.md)]

Aşağıdaki veri depolarından verileri **şirket içi dosya sistemine**kopyalayabilirsiniz:

[!INCLUDE [data-factory-supported-sources](../../../includes/data-factory-supported-sources.md)]

> [!NOTE]
> Copy Activity, hedefe başarıyla kopyalandıktan sonra kaynak dosyayı silmez. Başarılı bir kopyadan sonra kaynak dosyayı silmeniz gerekiyorsa, dosyayı silmek ve ardışık düzendeki etkinliği kullanmak için özel bir etkinlik oluşturun.

## <a name="enabling-connectivity"></a>Bağlantı sağlama
Veri Fabrikası, **Veri Yönetimi Ağ Geçidi**aracılığıyla şirket içi dosya sistemine bağlanmayı ve şirket içi dosya sistemine bağlanmayı destekler. Dosya sistemi de dahil olmak üzere desteklenen şirket içi veri deposuna bağlanmak için Veri Fabrikası hizmetinin şirket içi ortamınıza Veri Yönetimi Ağ Geçidi'ni yüklemeniz gerekir. Veri Yönetimi Ağ Geçidi hakkında bilgi edinmek ve ağ geçidini kurma yla ilgili adım adım talimatlar için [bkz.](data-factory-move-data-between-onprem-and-cloud.md) Veri Yönetimi Ağ Geçidi dışında, şirket içi dosya sistemine ve şirket içi dosya sistemine iletişim kurmak için başka ikili dosyaların yüklenmesi gerekmez. Dosya sistemi Azure IaaS VM'de olsa bile Veri Yönetimi Ağ Geçidi'ni yüklemeniz ve kullanmanız gerekir. Ağ geçidi hakkında ayrıntılı bilgi için [Veri Yönetimi Ağ Geçidi'ne](data-factory-data-management-gateway.md)bakın.

Linux dosya paylaşımı nı kullanmak için [Samba'yı](https://www.samba.org/) Linux sunucunuza yükleyin ve Windows sunucusuna Veri Yönetimi Ağ Geçidi'ni yükleyin. Linux sunucusuna Veri Yönetimi Ağ Geçidi'nin yüklenmesi desteklenmez.

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'ler kullanarak verileri dosya sistemine/dosya sisteminden hareket ettiren bir kopyalama etkinliği içeren bir ardışık düzen oluşturabilirsiniz.

Bir ardışık yol oluşturmanın en kolay yolu **Kopyalama Sihirbazı'nı**kullanmaktır. Bkz. Öğretici: Veri kopyala sihirbazını kullanarak bir ardışık yol oluşturma konusunda hızlı bir geçiş için Kopya Sihirbazı kullanarak bir [ardışık kaynak oluşturun.](data-factory-copy-data-wizard-tutorial.md)

Bir ardışık kaynak oluşturmak için aşağıdaki araçları da kullanabilirsiniz: **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**ve **REST API**. Kopyalama etkinliği içeren bir ardışık hatlar oluşturmak için adım adım yönergeleri için [etkinlik öğreticisini](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyala'ya bakın.

Araçları veya API'leri kullanın, verileri kaynak veri deposundan bir lavabo veri deposuna aktaran bir ardışık işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirirsiniz:

1. Bir **veri fabrikası**oluşturun. Bir veri fabrikası bir veya daha fazla ardışık hat lar içerebilir.
2. Giriş ve çıktı veri depolarını veri fabrikanıza bağlamak için **bağlantılı hizmetler** oluşturun. Örneğin, bir Azure blob depolama sından şirket içi dosya sistemine veri kopyalıyorsanız, şirket içi dosya sisteminizi ve Azure depolama hesabınızı veri fabrikanıza bağlamak için iki bağlantılı hizmet oluşturursunuz. Şirket içi dosya sistemine özgü bağlantılı hizmet özellikleri için [bağlantılı hizmet özellikleri](#linked-service-properties) bölümüne bakın.
3. Kopyalama işlemi için giriş ve çıktı verilerini temsil edecek **veri kümeleri** oluşturun. Son adımda belirtilen örnekte, giriş verilerini içeren blob kapsayıcısını ve klasörünü belirtmek için bir veri kümesi oluşturursunuz. Ayrıca, dosya sisteminizdeki klasör ve dosya adını (isteğe bağlı) belirtmek için başka bir veri kümesi oluşturursunuz. Şirket içi dosya sistemine özgü veri kümesi özellikleri için [veri kümesi özellikleri](#dataset-properties) bölümüne bakın.
4. Giriş olarak veri kümesi ve çıktı olarak veri kümesi alan bir kopyalama etkinliği içeren bir **ardışık işlem oluşturma.** Daha önce bahsedilen örnekte, blobSource'u kaynak olarak, FileSystemSink'i ise kopyalama etkinliği için lavabo olarak kullanırsınız. Benzer şekilde, şirket içi dosya sisteminden Azure Blob Depolama'ya kopyalıyorsanız, kopyalama etkinliğinde FileSystemSource ve BlobSink'i kullanırsınız. Şirket içi dosya sistemine özgü kopyalama etkinlik özellikleri için [kopyalama etkinlik özellikleri](#copy-activity-properties) bölümüne bakın. Kaynak veya lavabo olarak veri deposunun nasıl kullanılacağı yla ilgili ayrıntılar için, veri deponuzun önceki bölümündeki bağlantıyı tıklatın.

Sihirbazı kullandığınızda, bu Veri Fabrikası varlıkları (bağlantılı hizmetler, veri kümeleri ve ardışık kuruluş) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçları/API'leri (.NET API hariç) kullandığınızda, Bu Veri Fabrikası varlıklarını JSON biçimini kullanarak tanımlarsınız.  Bir dosya sistemine/dosya sisteminden veri kopyalamak için kullanılan Veri Fabrikası varlıkları için JSON tanımlı örnekler için bu makalenin [JSON örnekleri](#json-examples-for-copying-data-to-and-from-file-system) bölümüne bakın.

Aşağıdaki bölümler, dosya sistemine özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar:

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri
Şirket içi dosya **sistemini, Şirket İçi Dosya Sunucusu** bağlantılı hizmetle bir Azure veri fabrikasına bağlayabilirsiniz. Aşağıdaki tablo, Şirket İçi Dosya Sunucusu bağlantılı hizmete özgü JSON öğeleri için açıklamalar sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Tür özelliğinin **OnPremisesFileServer**olarak ayarlandığından emin olun. |Evet |
| konak |Kopyalamak istediğiniz klasörün kök yolunu belirtir. Dizedeki özel karakterler için ' \ ' kaçış karakterini kullanın. Örnekler için [Örnek bağlantılı hizmet ve veri kümesi tanımlarına](#sample-linked-service-and-dataset-definitions) bakın. |Evet |
| Userıd |Sunucuya erişimi olan kullanıcının kimliğini belirtin. |Hayır (şifrelenmiş Credential seçerseniz) |
| password |Kullanıcının parolasını belirtin (userid). |Hayır (şifrelenmiş Credential seçerseniz |
| şifreli Credential |New-AzDataFactoryEncryptValue cmdlet'i çalıştırarak elde edebileceğiniz şifrelenmiş kimlik bilgilerini belirtin. |Hayır (userid ve parolayı düz metinolarak belirtmeyi seçerseniz) |
| ağ geçidiAdı |Veri Fabrikası'nın şirket içi dosya sunucusuna bağlanmak için kullanması gereken ağ geçidinin adını belirtir. |Evet |


### <a name="sample-linked-service-and-dataset-definitions"></a>Örnek bağlantılı hizmet ve veri kümesi tanımları
| Senaryo | Bağlantılı hizmet tanımında ana bilgisayar | folderDataset tanımında yol |
| --- | --- | --- |
| Veri Yönetimi Ağ Geçidi makinesinde yerel klasör: <br/><br/>Örnekler: D:\\ \* veya D:\folder\subfolder\\\* |D:\\ \\ (Veri Yönetimi Ağ Geçidi 2.0 ve sonraki sürümler için) <br/><br/> localhost (Veri Yönetimi Ağ Geçidi 2.0'dan önceki sürümler için) |. veya\\\\klasör alt klasörü (Veri Yönetimi Ağ Geçidi 2.0 ve sonraki sürümler için) \\ \\ <br/><br/>D:\\ \\ veya\\\\D: klasör alt klasörü\\\\(2.0'ın altındaki ağ geçidi sürümü için) |
| Uzaktan paylaşılan klasör: <br/><br/>Örnekler: \\ \\\\myserver\\ \* \\ \\share\\veya\\\\myserver share folder alt klasörü\\\* |\\\\\\\\myserver\\\\payı |. veya\\\\klasör alt klasörü \\ \\ |

>[!NOTE]
>UI üzerinden yazarken, JSON üzerinden yaptığınız gibi çift`\\`eğik çizgi () giriş yapmanıza gerek yok, tek bir ters eğik çizgi belirtin.

### <a name="example-using-username-and-password-in-plain-text"></a>Örnek: Kullanıcı adı ve parolayı düz metinde kullanma

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

### <a name="example-using-encryptedcredential"></a>Örnek: Şifreli credential kullanma

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
Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [bkz.](data-factory-create-datasets.md) Bir veri kümesi JSON'un yapısı, kullanılabilirliği ve ilkesi gibi bölümler tüm veri kümesi türleri için benzerdir.

typeProperties bölümü her veri kümesi türü için farklıdır. Veri deposundaki verilerin konumu ve biçimi gibi bilgiler sağlar. **FileShare** türü veri kümesi için typeProperties bölümü aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| folderPath |Klasöre alt patina belirtir. Dizedeki özel\' karakterler için kaçış karakterini kullanın. Joker karakter filtresi desteklenmez. Örnekler için [Örnek bağlantılı hizmet ve veri kümesi tanımlarına](#sample-linked-service-and-dataset-definitions) bakın.<br/><br/>Bu özelliği **partitionBy** ile birleştirerek dilim başlangıç/bitiş tarih saatlerine göre klasör yollarına sahip olabilirsiniz. |Evet |
| fileName |Tablonun klasördeki belirli bir dosyaya başvurmasını istiyorsanız **klasörDeki Dosyanın** adını belirtinPath. Bu özellik için herhangi bir değer belirtmezseniz, tablo klasördeki tüm dosyaları işaret edir.<br/><br/>Bir çıktı veri kümesi için **dosya Adı** belirtilmediğinde ve **korumaHiyerarşisi** etkinlik lavabosunda belirtilmemişse, oluşturulan dosyanın adı aşağıdaki biçimdedir: <br/><br/>`Data.<Guid>.txt`(Örnek: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt) |Hayır |
| dosyaFiltre |Tüm dosyalar yerine folderPath'teki dosyaların bir alt kümesini seçmek için kullanılacak bir filtre belirtin. <br/><br/>İzin verilen `*` değerler şunlardır: `?` (birden çok karakter) ve (tek karakter).<br/><br/>Örnek 1: "fileFilter": "*.log"<br/>Örnek 2: "fileFilter": 2014-1-?. txt"<br/><br/>FileFilter'in bir giriş FileShare veri kümesi için geçerli olduğunu unutmayın. |Hayır |
| bölümlemeBy |Zaman serisi verileri için dinamik bir folderPath/fileName belirtmek için partitionedBy'yi kullanabilirsiniz. Bir örnek, her saatlik veriler için parametrelendirilmiş klasörYol'dur. |Hayır |
| biçim | Aşağıdaki biçim türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParkeFormat**. Biçim altındaki **tür** özelliğini bu değerlerden birine ayarlayın. Daha fazla bilgi için [Metin Biçimi,](data-factory-supported-file-and-compression-formats.md#text-format) [Json Formatı,](data-factory-supported-file-and-compression-formats.md#json-format) [Avro Biçimi,](data-factory-supported-file-and-compression-formats.md#avro-format) [Ork Biçimi](data-factory-supported-file-and-compression-formats.md#orc-format)ve [Parke Biçimi](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümlerine bakın. <br><br> Dosyaları dosya tabanlı mağazalar (ikili kopya) arasında **olduğu gibi kopyalamak** istiyorsanız, hem giriş hem de çıktı veri kümesi tanımlarında biçim bölümünü atlayın. |Hayır |
| sıkıştırma | Verilerin sıkıştırma türünü ve düzeyini belirtin. Desteklenen türleri şunlardır: **GZip**, **Deflate**, **BZip2**, ve **ZipDeflate**. Desteklenen seviyeler şunlardır: **Optimal** ve **En Hızlı.** bkz. [Azure Veri Fabrikası'ndaki Dosya ve sıkıştırma biçimleri.](data-factory-supported-file-and-compression-formats.md#compression-support) |Hayır |

> [!NOTE]
> fileName ve fileFilter'i aynı anda kullanamazsınız.

### <a name="using-partitionedby-property"></a>PartitionedBy özelliğini kullanma
Önceki bölümde belirtildiği gibi, **bölümlenmişBy** özelliği, [Veri Fabrikası işlevleri ve sistem değişkenleri](data-factory-functions-variables.md)ile zaman serisi verileri için dinamik bir klasörPath ve dosya adı belirtebilirsiniz.

Zaman serisi veri kümeleri, zamanlama ve dilimler hakkında daha fazla [Scheduling and execution](data-factory-scheduling-and-execution.md)ayrıntı yı anlamak [Creating pipelines](data-factory-create-pipelines.md)için [bkz.](data-factory-create-datasets.md)

#### <a name="sample-1"></a>Örnek 1:

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```

Bu örnekte, {Slice} biçiminde Veri Fabrikası sistem değişkeni SliceStart değeri (YYYYMMDDHH) değeri ile değiştirilir. SliceStart, dilimin başlangıç saatini ifade eder. FolderPath her dilim için farklıdır. Örneğin: wikidatagateway/wikisampledataout/2014100103 veya wikidatagateway/wikisampledataout/2014100104.

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

Bu örnekte, SliceStart'ın yılı, ayı, günü ve saati, folderPath ve fileName özelliklerinin kullandığı ayrı değişkenlere ayıklanır.

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Etkinlikleri tanımlamak için kullanılabilen bölümlerin & özelliklerinin tam listesi [için, Kaynak Hatları Oluşturma](data-factory-create-pipelines.md) makalesine bakın. Ad, açıklama, giriş ve çıktı veri kümeleri ve ilkeler gibi özellikler tüm etkinlik türleri için kullanılabilir. Oysa, etkinliğin **typeProperties** bölümünde bulunan özellikler her etkinlik türüne göre değişir.

Kopyalama etkinliği için, kaynak ve lavabo türlerine bağlı olarak değişir. Verileri şirket içi dosya sisteminden taşıyorsanız, kopyalama etkinliğindeki kaynak türünü **FileSystemSource'a**ayarlarsınız. Benzer şekilde, verileri şirket içi bir dosya sistemine taşıyorsanız, kopyalama etkinliğindeki lavabo türünü **FileSystemSink**olarak ayarlarsınız. Bu bölümde FileSystemSource ve FileSystemSink tarafından desteklenen özelliklerin bir listesini sağlar.

**FileSystemSource** aşağıdaki özellikleri destekler:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| Özyinelemeli |Verilerin alt klasörlerden mi yoksa yalnızca belirtilen klasörden mi özyinelemeli olarak okunduğunu gösterir. |True, False (varsayılan) |Hayır |

**FileSystemSink** aşağıdaki özellikleri destekler:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| copyBehavior |Kaynak BlobSource veya FileSystem olduğunda kopyalama davranışını tanımlar. |**Koruma Hiyerarşisi:** Hedef klasördeki dosya hiyerarşisini korur. Diğer bir diğer olarak, kaynak dosyanın kaynak klasöre göreli yolu hedef klasöre hedef dosyanın göreli yolu ile aynıdır.<br/><br/>**FlattenHiyerarşi:** Kaynak klasördeki tüm dosyalar hedef klasörün ilk düzeyinde oluşturulur. Hedef dosyalar otomatik olarak oluşturulan bir adla oluşturulur.<br/><br/>**Birleştirme Dosyaları:** Kaynak klasördeki tüm dosyaları tek bir dosyayla birleştirir. Dosya adı/blob adı belirtilirse, birleştirilen dosya adı belirtilen addır. Aksi takdirde, otomatik olarak oluşturulan bir dosya adıdır. |Hayır |

### <a name="recursive-and-copybehavior-examples"></a>özyinelemeli ve copyBehavior örnekleri
Bu bölümde, özyinelemeli ve copyBehavior özellikleri için farklı değer birleşimleri için Kopyalama işleminin ortaya çıkan davranışı açıklanmaktadır.

| özyinelemeli değer | copyBehavior değeri | Sonuç davranış |
| --- | --- | --- |
| true |korumaHiyerarşisi |Aşağıdaki yapıya sahip bir kaynak klasör Folder1 için,<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5<br/><br/>hedef klasör Folder1 kaynak la aynı yapıile oluşturulur:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5 |
| true |flattenHiyerarşi |Aşağıdaki yapıya sahip bir kaynak klasör Folder1 için,<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5<br/><br/>hedef Folder1 aşağıdaki yapı ile oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File3 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File4 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File5 için otomatik olarak oluşturulan ad |
| true |birleştirmeDosyaları |Aşağıdaki yapıya sahip bir kaynak klasör Folder1 için,<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5<br/><br/>hedef Folder1 aşağıdaki yapı ile oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + File3 + File4 + File 5 içeriği otomatik olarak oluşturulan dosya adı ile bir dosyada birleştirilir. |
| yanlış |korumaHiyerarşisi |Aşağıdaki yapıya sahip bir kaynak klasör Folder1 için,<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5<br/><br/>hedef klasör Folder1 aşağıdaki yapı ile oluşturulur:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/><br/>Dosya3, File4 ve File5 ile alt klasör1 alınmaz. |
| yanlış |flattenHiyerarşi |Aşağıdaki yapıya sahip bir kaynak klasör Folder1 için,<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5<br/><br/>hedef klasör Folder1 aşağıdaki yapı ile oluşturulur:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2 için otomatik olarak oluşturulan ad<br/><br/>Dosya3, File4 ve File5 ile alt klasör1 alınmaz. |
| yanlış |birleştirmeDosyaları |Aşağıdaki yapıya sahip bir kaynak klasör Folder1 için,<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5<br/><br/>hedef klasör Folder1 aşağıdaki yapı ile oluşturulur:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 içeriği otomatik olarak oluşturulan dosya adı ile bir dosyada birleştirilir.<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 için otomatik oluşturulan ad<br/><br/>Dosya3, File4 ve File5 ile alt klasör1 alınmaz. |

## <a name="supported-file-and-compression-formats"></a>Desteklenen dosya ve sıkıştırma biçimleri
Ayrıntılarla ilgili [Azure Veri Fabrikası makalesinde Dosya ve sıkıştırma biçimlerine](data-factory-supported-file-and-compression-formats.md) bakın.

## <a name="json-examples-for-copying-data-to-and-from-file-system"></a>Dosya sistemine ve dosya sisteminden veri kopyalamak için JSON örnekleri
Aşağıdaki örnekler, [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)kullanarak bir ardışık hat lar oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar. Şirket içi dosya sistemine ve Azure Blob depolamasına veri kopyalamanın nasıl yapılacağını gösterirler. Ancak, Azure Veri Fabrikası'nda Kopyalama Etkinliği'ni kullanarak verileri *doğrudan* kaynaklardan herhangi birinden [Desteklenen kaynaklarda ve lavabolarda](data-factory-data-movement-activities.md#supported-data-stores-and-formats) listelenen lavabolara kopyalayabilirsiniz.

### <a name="example-copy-data-from-an-on-premises-file-system-to-azure-blob-storage"></a>Örnek: Şirket içi dosya sistemindeki verileri Azure Blob depolama alanına kopyalama
Bu örnek, şirket içi bir dosya sisteminden Azure Blob depolamasına verilerin nasıl kopyalanır olduğunu gösterir. Örnekte aşağıdaki Veri Fabrikası varlıkları vardır:

* [OnPremisesFileServer](#linked-service-properties)türünde bağlantılı bir hizmet.
* [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)türüne bağlı bir hizmet.
* [FileShare](#dataset-properties)türünden bir giriş [veri kümesi.](data-factory-create-datasets.md)
* [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)türünden bir çıktı [veri kümesi.](data-factory-create-datasets.md)
* [FileSystemSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)kullanan Copy Activity ile bir [ardışık düzen.](data-factory-create-pipelines.md)

Aşağıdaki örnek, şirket içi bir dosya sisteminden Azure Blob depolamasına her saat başı zaman serisi verilerini kopyalar. Bu örneklerde kullanılan JSON özellikleri örneklerden sonraki bölümlerde açıklanmıştır.

İlk adım olarak, Veri Yönetimi Ağ Geçidi ile şirket içi kaynaklar ve bulut arasında veri taşıma yönergelerine göre [Veri Yönetimi Ağ Geçidi'ni](data-factory-move-data-between-onprem-and-cloud.md)ayarlayın.

**Şirket İçi Dosya Sunucusu bağlantılı hizmet:**

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

Bunun yerine **userid** ve **parola** özelliklerini **kullanarak şifrelenmiş Credential** özelliğini kullanmanızı öneririz. Bu bağlantılı hizmet le ilgili ayrıntılar için [Dosya Sunucusu bağlantılı hizmete](#linked-service-properties) bakın.

**Azure Depolama bağlantılı hizmet:**

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

Veriler her saat başı yeni bir dosyadan alınır. FolderPath ve fileName özellikleri dilimin başlangıç saatine göre belirlenir.

Ayar, `"external": "true"` Veri Fabrikası'na veri kümesinin veri fabrikasının dışında olduğunu ve veri fabrikasındaki bir etkinlik tarafından üretilmediğini bildirir.

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

**Azure Blob depolama çıktı veri seti:**

Veriler her saat yeni bir blob 'a yazılır (sıklık: saat, aralık: 1). Blob için klasör yolu, işlenen dilimin başlangıç saatine göre dinamik olarak değerlendirilir. Klasör yolu, başlangıç zamanının yıl, ay, gün ve saat parçalarını kullanır.

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

**Dosya Sistemi kaynağı ve Blob lavabosu olan bir ardışık düzende kopyalama etkinliği:**

Ardışık iş, giriş ve çıktı veri kümelerini kullanacak şekilde yapılandırılan ve her saat çalışacak şekilde zamanlanan bir kopyalama etkinliği içerir. Boru hattı JSON tanımında, **kaynak** türü **FileSystemSource**olarak ayarlanır ve **lavabo** türü **BlobSink**olarak ayarlanır.

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

### <a name="example-copy-data-from-azure-sql-database-to-an-on-premises-file-system"></a>Örnek: Azure SQL Veritabanı'ndan şirket içi dosya sistemine veri kopyalama
Aşağıdaki örnek gösterir:

* [AzureSqlDatabase](data-factory-azure-sql-connector.md#linked-service-properties) türünün bağlantılı bir hizmeti.
* [OnPremisesFileServer](#linked-service-properties)türünde bağlantılı bir hizmet.
* [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties)türünden bir giriş veri kümesi.
* [FileShare](#dataset-properties)türünden bir çıktı veri kümesi.
* [SqlSource](data-factory-azure-sql-connector.md#copy-activity-properties) ve [FileSystemSink](#copy-activity-properties)kullanan bir kopyalama etkinliği olan bir ardışık düzen.

Örnek, zaman serisi verilerini bir Azure SQL tablosundan şirket içi dosya sistemine her saat başı kopyalar. Bu örneklerde kullanılan JSON özellikleri örneklerden sonraki bölümlerde açıklanmıştır.

**Azure SQL Veritabanı bağlantılı hizmet:**

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

**Şirket İçi Dosya Sunucusu bağlantılı hizmet:**

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

**Userid** ve **parola** özelliklerini kullanmak yerine **şifreli Credential** özelliğini kullanmanızı öneririz. Bu bağlantılı hizmetle ilgili ayrıntılar için [Dosya Sistemi bağlantılı hizmete](#linked-service-properties) bakın.

**Azure SQL giriş veri kümesi:**

Örnek, Azure SQL'de "MyTable" tablosu oluşturduğunuzu varsayar ve zaman serisi verileri için "zaman damgası sütunu" adlı bir sütun içerir.

Ayar, ``“external”: ”true”`` Veri Fabrikası'na veri kümesinin veri fabrikasının dışında olduğunu ve veri fabrikasındaki bir etkinlik tarafından üretilmediğini bildirir.

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

**Şirket içi dosya sistemi çıktı veri seti:**

Veriler her saat başı yeni bir dosyaya kopyalanır. Blob için folderPath ve fileName dilimin başlangıç saatine göre belirlenir.

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

**SQL kaynağı ve Dosya Sistemi lavabosu olan bir ardışık düzende kopyalama etkinliği:**

Ardışık iş, giriş ve çıktı veri kümelerini kullanacak şekilde yapılandırılan ve her saat çalışacak şekilde zamanlanan bir kopyalama etkinliği içerir. Geçiş adı JSON **tanımında, kaynak** türü **SqlSource**olarak ayarlanır ve **lavabo** türü **FileSystemSink**olarak ayarlanır. **SqlReaderQuery** özelliği için belirtilen SQL sorgusu, kopyalanacak son bir saat içinde verileri seçer.

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

Ayrıca, kopya etkinliği tanımında kaynak veri kümesinden sütunlara, lavabo veri kümesinden sütunlara kadar sütunları eşleyebilirsiniz. Ayrıntılar için Azure [Veri Fabrikası'ndaki veri kümesi sütunlarını eşleme](data-factory-map-columns.md)bilgisine bakın.

## <a name="performance-and-tuning"></a>Performans ve ayar
 Azure Veri Fabrikası'ndaki veri hareketinin (Kopyalama Etkinliği) performansını etkileyen önemli etkenler ve bunu optimize etmenin çeşitli yolları hakkında bilgi edinmek için [Kopyalama Etkinliği performansı ve atokslama kılavuzuna](data-factory-copy-activity-performance.md)bakın.
