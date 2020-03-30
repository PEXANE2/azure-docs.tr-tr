---
title: Kopyalama Etkinliği'ni kullanarak verileri taşıma
description: "Veri Fabrikası ardışık hatlarındaki veri hareketi hakkında bilgi edinin: bulut depoları arasında ve şirket içi mağaza ile bulut mağazası arasında veri geçişi. Kopyalama Etkinliği'ni kullanın."
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.assetid: 67543a20-b7d5-4d19-8b5e-af4c1fd7bc75
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 12/05/2017
ms.author: jingwang
robots: noindex
ms.openlocfilehash: fbaa8c3544b35978786404619879f59ab91a6979
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79281892"
---
# <a name="move-data-by-using-copy-activity"></a>Kopyalama Etkinliği'ni kullanarak verileri taşıma
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](data-factory-data-movement-activities.md)
> * [Sürüm 2 (geçerli sürüm)](../copy-activity-overview.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [V2'deki Etkinliği Kopyala'ya](../copy-activity-overview.md)bakın.

## <a name="overview"></a>Genel Bakış
Azure Veri Fabrikası'nda, şirket içi ve bulut veri depoları arasındaki verileri kopyalamak için Etkinliği Kopyala'yı kullanabilirsiniz. Veriler kopyalandıktan sonra daha da dönüştürülebilir ve analiz edilebilir. İş zekası (BI) ve uygulama tüketimi için dönüşüm ve analiz sonuçlarını yayımlamak için Kopyalama Etkinliği'ni de kullanabilirsiniz.

![Kopyalama Etkinliğinin Rolü](media/data-factory-data-movement-activities/copy-activity.png)

Kopyalama Etkinliği, güvenli, güvenilir, ölçeklenebilir ve [genel olarak kullanılabilir](#global)bir hizmet tarafından desteklenmektedir. Bu makalede, Veri Fabrikası ve Kopyalama Etkinliği'nde veri hareketi hakkında ayrıntılı bilgi verilmektedir.

İlk olarak, iki bulut veri deposu ile şirket içi veri deposu ile bulut veri deposu arasında veri aktarışlarının nasıl oluştuğunu görelim.

> [!NOTE]
> Genel olarak etkinlikler hakkında bilgi edinmek için [bkz.](data-factory-create-pipelines.md)
>
>

### <a name="copy-data-between-two-cloud-data-stores"></a>İki bulut veri deposu arasında veri kopyalama
Hem kaynak hem de lavabo veri depoları bulutta olduğunda, Kopyalama Etkinliği kaynaktan lavaboya verileri kopyalamak için aşağıdaki aşamalardan geçer. Kopyalama Etkinliğine yetki veren hizmet:

1. Kaynak veri deposundaki verileri okur.
2. Serileştirme/deserialization, sıkıştırma/dekompresyon, sütun eşleme ve tür dönüştürme gerçekleştirir. Bu işlemleri giriş veri kümesi, çıktı veri kümesi ve Kopyalama Etkinliği yapılandırmalarına göre yapar.
3. Hedef veri deposuna veri yazar.

Hizmet, veri hareketini gerçekleştirmek için otomatik olarak en uygun bölgeyi seçer. Bu bölge genellikle lavabo veri deposuna en yakın bölgedir.

![Bulut-bulut kopyalama](./media/data-factory-data-movement-activities/cloud-to-cloud.png)

### <a name="copy-data-between-an-on-premises-data-store-and-a-cloud-data-store"></a>Şirket içi veri deposu ile bulut veri deposu arasında veri kopyalama
Verileri şirket içi veri deposu ile bulut veri deposu arasında güvenli bir şekilde taşımak için, şirket içi makinenize Veri Yönetimi Ağ Geçidi yükleyin. Veri Yönetimi Ağ Geçidi, karma veri hareketi ve işleme sağlayan bir aracıdır. Bunu veri deposunun kendisiyle aynı makineye veya veri deposuna erişimi olan ayrı bir makineye yükleyebilirsiniz.

Bu senaryoda, Veri Yönetimi Ağ Geçidi serileştirme/deserialization, sıkıştırma/dekompresyon, sütun eşleme ve tür dönüştürme gerçekleştirir. Veriler Azure Veri Fabrikası hizmetinden akmaz. Bunun yerine, Veri Yönetimi Ağ Geçidi verileri doğrudan hedef depoya yazar.

![Şirket içi buluta kopyalama](./media/data-factory-data-movement-activities/onprem-to-cloud.png)

Giriş ve geçiş için [verileri şirket içi ve bulut veri depoları arasında taşıyın.](data-factory-move-data-between-onprem-and-cloud.md) Bu aracı hakkında ayrıntılı bilgi için [Veri Yönetimi Ağ Geçidi'ne](data-factory-data-management-gateway.md) bakın.

Veri Yönetimi Ağ Geçidi'ni kullanarak verileri Azure IaaS sanal makinelerde (VM) barındırılan desteklenen veri depolarından/bunlara taşıyabilirsiniz. Bu durumda, Veri Yönetimi Ağ Geçidi'ni veri deposunun kendisiyle aynı VM'ye veya veri deposuna erişimi olan ayrı bir VM'ye yükleyebilirsiniz.

## <a name="supported-data-stores-and-formats"></a>Desteklenen veri depoları ve biçimleri
Data Factory’deki Kopyalama Etkinliği bir kaynak veri deposundan havuz veri deposuna verileri kopyalar. Data Factory aşağıdaki veri depolarını destekler. Herhangi bir kaynaktan gelen veriler herhangi bir havuza yazılabilir. Bir depoya veya depodan veri kopyalama hakkında bilgi edinmek için veri deposuna tıklayın.

> [!NOTE] 
> Kopyalama Etkinliğinin desteklemediği bir veri deposuna/veri deposundan veri taşımanız gerekirse Data Factory’de **özel bir etkinliği** kendi veri kopyalama/taşıma mantığınızla kullanın. Özel bir etkinlik oluşturma ve kullanma hakkında ayrıntılı bilgi için bkz. [Azure Data Factory işlem hattında özel etkinlikler kullanma](data-factory-use-custom-activities.md).

[!INCLUDE [data-factory-supported-data-stores](../../../includes/data-factory-supported-data-stores.md)]

> [!NOTE]
> * taşıyan veri depoları şirket içi veya Azure IaaS üzerinde olabilir ve bir şirket içi/Azure IaaS makinesine [Veri Yönetimi Ağ Geçidi](data-factory-data-management-gateway.md) yüklemenizi gerektirir.

### <a name="supported-file-formats"></a>Desteklenen dosya biçimleri
Dosyaları iki dosya tabanlı veri deposu arasında **olduğu gibi kopyalamak** için Kopyalama Etkinliği'ni kullanabilirsiniz, hem giriş hem de çıktı veri kümesi tanımlarında [biçim bölümünü](data-factory-create-datasets.md) atlayabilirsiniz. Veriler herhangi bir serileştirme/deserialization olmadan verimli bir şekilde kopyalanır.

Copy Activity ayrıca belirli biçimlerde dosyalara okur ve yazar: **Metin, JSON, Avro, ORC ve Parke**ve sıkıştırma codec **GZip, Deflate, BZip2 ve ZipDeflate** desteklenir. Ayrıntılarla [desteklenen dosya ve sıkıştırma biçimlerine](data-factory-supported-file-and-compression-formats.md) bakın.

Örneğin, aşağıdaki kopyalama etkinliklerini yapabilirsiniz:

* Şirket içi SQL Server'da verileri kopyalayın ve ORC formatında Azure Data Lake Store'a yazın.
* Dosyaları şirket içi Dosya Sisteminden metin (CSV) biçiminde kopyalayın ve Avro biçiminde Azure Blob'a yazın.
* Şirket içi Dosya Sistemi'nden sıkıştırılamış dosyaları kopyalayın ve sıkıştırmayı dekomprese edin ve ardından Azure Veri Gölü Deposu'na inin.
* Azure Blob'dan GZip sıkıştırılmış metin (CSV) biçimindeki verileri kopyalayın ve Azure SQL Veritabanı'na yazın.

## <a name="globally-available-data-movement"></a><a name="global"></a>Küresel olarak kullanılabilir veri hareketi
Azure Veri Fabrikası yalnızca Batı ABD, Doğu ABD ve Kuzey Avrupa bölgelerinde kullanılabilir. Ancak, Kopyalama Etkinliği'ne yetki veren hizmet, aşağıdaki bölgelerde ve coğrafyalarda genel olarak kullanılabilir. Küresel olarak kullanılabilir topoloji, genellikle bölgeler arası atlamaları önleyen verimli veri hareketi sağlar. Bir bölgedeki Veri Fabrikası ve Veri Hareketi kullanılabilirliği için [bölgeye göre hizmetlere](https://azure.microsoft.com/regions/#services) bakın.

### <a name="copy-data-between-cloud-data-stores"></a>Bulut veri depoları arasında veri kopyalama
Hem kaynak hem de lavabo veri depoları bulutta olduğunda, Veri Fabrikası verileri taşımak için aynı coğrafyadaki lavaboya en yakın bölgede bir hizmet dağıtımı kullanır. Eşleme için aşağıdaki tabloya bakın:

| Hedef veri depolarının coğrafyası | Hedef veri deposunun bölgesi | Veri hareketi için kullanılan bölge |
|:--- |:--- |:--- |
| Amerika Birleşik Devletleri | Doğu ABD | Doğu ABD |
| &nbsp; | Doğu ABD 2 | Doğu ABD 2 |
| &nbsp; | Orta ABD | Orta ABD |
| &nbsp; | Orta Kuzey ABD | Orta Kuzey ABD |
| &nbsp; | Orta Güney ABD | Orta Güney ABD |
| &nbsp; | Orta Batı ABD | Orta Batı ABD |
| &nbsp; | Batı ABD | Batı ABD |
| &nbsp; | Batı ABD 2 | Batı ABD 2 |
| Kanada | Doğu Kanada | Orta Kanada |
| &nbsp; | Orta Kanada | Orta Kanada |
| Brezilya | Güney Brezilya | Güney Brezilya |
| Avrupa | Kuzey Avrupa | Kuzey Avrupa |
| &nbsp; | Batı Avrupa | Batı Avrupa |
| Birleşik Krallık | Batı Birleşik Krallık | Güney Birleşik Krallık |
| &nbsp; | Güney Birleşik Krallık | Güney Birleşik Krallık |
| Asya Pasifik | Güneydoğu Asya | Güneydoğu Asya |
| &nbsp; | Doğu Asya | Güneydoğu Asya |
| Avustralya | Doğu Avustralya | Doğu Avustralya |
| &nbsp; | Güneydoğu Avustralya | Güneydoğu Avustralya |
| Hindistan | Orta Hindistan | Orta Hindistan |
| &nbsp; | Batı Hindistan | Orta Hindistan |
| &nbsp; | Güney Hindistan | Orta Hindistan |
| Japonya | Doğu Japonya | Doğu Japonya |
| &nbsp; | Batı Japonya | Doğu Japonya |
| Güney Kore | Güney Kore - Orta | Güney Kore - Orta |
| &nbsp; | Güney Kore - Güney | Güney Kore - Orta |

Alternatif olarak, Kopyalama Etkinliği `executionLocation` `typeProperties`altında özellik belirterek kopyayı gerçekleştirmek için kullanılacak Veri Fabrikası hizmetinin bölgesini açıkça belirtebilirsiniz. Bu özellik için desteklenen değerler, veri hareketi sütunu için kullanılan yukarıda **Bölge'de** listelenir. Verilerinizin kopya sırasında o bölgeden kablo üzerinden geçtiğine dikkat edin. Örneğin, Kore'deki Azure mağazaları arasında kopyalamak için, Japonya bölgesinde rota belirlemeyi belirtebilirsiniz `"executionLocation": "Japan East"` [(bkz. örnek JSON](#by-using-json-scripts) referans olarak).

> [!NOTE]
> Hedef veri deposunun bölgesi önceki listede yoksa veya algılanamazsa, belirtilmediği sürece `executionLocation` alternatif bir bölgeden geçmek yerine varsayılan olarak Kopyalama Etkinliği başarısız olur. Desteklenen bölge listesi zaman içinde genişletilecektir.
>

### <a name="copy-data-between-an-on-premises-data-store-and-a-cloud-data-store"></a>Şirket içi veri deposu ile bulut veri deposu arasında veri kopyalama
Veriler şirket içi (veya Azure sanal makineler/IaaS) ve bulut depoları arasında kopyalanırken, [Veri Yönetimi Ağ Geçidi](data-factory-data-management-gateway.md) şirket içi bir makinede veya sanal makinede veri hareketi gerçekleştirir. [Aşamalı kopyalama](data-factory-copy-activity-performance.md#staged-copy) özelliğini kullanmadığınız sürece veriler buluttaki hizmetten akmaz. Bu durumda, veriler, lavabo veri deposuna yazılmadan önce hazırlama Azure Blob depolama alanında akar.

## <a name="create-a-pipeline-with-copy-activity"></a>Kopyalama Etkinliği ile bir ardışık hatlar oluşturma
Kopyalama Etkinliği ile bir ardışık kaynak oluşturabilirsiniz:

### <a name="by-using-the-copy-wizard"></a>Kopyalama Sihirbazını kullanarak
Veri Fabrikası Kopyalama Sihirbazı, Kopyalama Etkinliği ile bir ardışık hatlar oluşturmanıza yardımcı olur. Bu ardışık işlem, desteklenen kaynaklardan gelen verileri bağlantılı hizmetler, veri kümeleri ve ardışık hatlar için JSON tanımları *yazmadan* hedeflere kopyalamanıza olanak tanır. Sihirbaz hakkında ayrıntılar için [Veri Fabrikası Kopyalama Sihirbazı'na](data-factory-copy-wizard.md) bakın.  

### <a name="by-using-json-scripts"></a>JSON komut dosyalarını kullanarak
Visual Studio'daki Veri Fabrikası Düzenleyicisi'ni veya Bir ardışık hatlar için JSON tanımı oluşturmak için Azure PowerShell'i kullanabilirsiniz (Kopyalama Etkinliği'ni kullanarak). Ardından, veri fabrikasında ardışık hatlar oluşturmak için dağıtabilirsiniz. Bkz. Öğretici: Adım adım yönergeleri olan bir öğretici için [Azure Veri Fabrikası ardışık alanında Kopyalama Etkinliği'ni kullanın.](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)    

JSON özellikleri (ad, açıklama, giriş ve çıktı tabloları ve ilkeler gibi) tüm etkinlik türleri için kullanılabilir. Etkinlik `typeProperties` bölümünde bulunan özellikler her etkinlik türüne göre değişir.

Kopyalama Etkinliği için `typeProperties` bölüm, kaynak ve lavabo türlerine bağlı olarak değişir. [Desteklenen kaynaklar ve lavabolar](#supported-data-stores-and-formats) bölümünde, Kopyalama Etkinliği'nin bu veri deposu için desteklediği tür özellikleri hakkında bilgi edinmek için bir kaynak/lavaboyu tıklatın.

Burada örnek bir JSON tanımı:

```json
{
  "name": "ADFTutorialPipeline",
  "properties": {
    "description": "Copy data from Azure blob to Azure SQL table",
    "activities": [
      {
        "name": "CopyFromBlobToSQL",
        "type": "Copy",
        "inputs": [
          {
            "name": "InputBlobTable"
          }
        ],
        "outputs": [
          {
            "name": "OutputSQLTable"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "SqlSink"
          },
          "executionLocation": "Japan East"          
        },
        "Policy": {
          "concurrency": 1,
          "executionPriorityOrder": "NewestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
    ],
    "start": "2016-07-12T00:00:00Z",
    "end": "2016-07-13T00:00:00Z"
  }
}
```
Çıktı veri kümesinde tanımlanan zamanlama, etkinliğin ne zaman çalıştığını belirler (örneğin: **günlük**, **gün**olarak sıklık ve aralık **1**olarak). Etkinlik, verileri bir giriş veri kümesinden **(kaynak)** çıktı veri kümesine **(lavabo)** kopyalar.

Etkinliği Kopyalamak için birden fazla giriş veri kümesi belirtebilirsiniz. Bunlar, etkinlik çalıştırılmadan önce bağımlılıkları doğrulamak için kullanılır. Ancak, yalnızca ilk veri kümesindeki veriler hedef veri kümesine kopyalanır. Daha fazla bilgi için [Zamanlama ve yürütme](data-factory-scheduling-and-execution.md)ye bakın.  

## <a name="performance-and-tuning"></a>Performans ve ayar
Azure Veri Fabrikası'ndaki veri hareketinin (Kopyalama Etkinliği) performansını etkileyen önemli faktörleri açıklayan [Kopyalama Etkinliği performansı ve aakma kılavuzuna](data-factory-copy-activity-performance.md)bakın. Ayrıca, dahili sınama sırasında gözlenen performansı listeler ve Kopyalama Etkinliği performansını optimize etmek için çeşitli yolları tartışır.

## <a name="fault-tolerance"></a>Hataya dayanıklılık
Varsayılan olarak, kopyalama etkinliği kaynak ve lavabo arasında uyumsuz verilerle karşılaştığında veri kopyalamayı durdurur ve başarısızlığı döndürecektir; uyumlu satırları atlayacak ve günlüğe kaydetmek için açıkça yapılandırabilirsiniz ve yalnızca kopyanın başarılı olması için bu uyumlu verileri kopyalayabilirsiniz. Daha fazla ayrıntı da [Kopyalama Etkinliği hata toleransına](data-factory-copy-activity-fault-tolerance.md) bakın.

## <a name="security-considerations"></a>Güvenlik konuları
Azure Veri Fabrikası'ndaki veri hareketi hizmetlerinin verilerinizi güvence altına almak için kullandığı güvenlik altyapısını açıklayan [Güvenlik hususlarına](data-factory-data-movement-security-considerations.md)bakın.

## <a name="scheduling-and-sequential-copy"></a>Zamanlama ve sıralı kopya
Veri [Fabrikası'nda](data-factory-scheduling-and-execution.md) zamanlama ve yürütmenin nasıl çalıştığı hakkında ayrıntılı bilgi için zamanlama ve yürütmeye bakın. Birden çok kopyalama işlemi sıralı/sıralı bir şekilde birbiri ardına çalıştırmak mümkündür. [Sırayla Kopyala](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) bölümüne bakın.

## <a name="type-conversions"></a>Tür dönüştürmeleri
Farklı veri depolarında farklı yerel tür sistemleri vardır. Copy Activity, aşağıdaki iki adımlı yaklaşımla kaynak türlerinden lavabo türlerine otomatik tür dönüşümleri gerçekleştirir:

1. Yerel kaynak türlerinden bir .NET türüne dönüştürün.
2. .NET türünden yerel lavabo türüne dönüştürün.

Yerel bir tür sisteminden veri deposu için bir .NET türüne eşleme ilgili veri deposu makalesindedir. (Desteklenen veri depoları tablosundaki belirli bağlantıyı tıklatın). Tablolarınızı oluştururken uygun türleri belirlemek için bu eşlemeleri kullanabilirsiniz, böylece Kopyalama Etkinliği doğru dönüşümleri gerçekleştirir.

## <a name="next-steps"></a>Sonraki adımlar
* Kopyalama Etkinliği hakkında daha fazla bilgi edinmek için Azure [Blob depolamadan Azure SQL Veritabanı'na veri kopyalama 'ya](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)bakın.
* Verileri şirket içi veri deposundan bulut veri deposuna taşıma hakkında bilgi edinmek için [bkz.](data-factory-move-data-between-onprem-and-cloud.md)
