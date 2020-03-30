---
title: Etkinlik performans optimizasyonu özelliklerini kopyalama
description: Azure Veri Fabrikası'nda kopyalama etkinliği performansını optimize ekolte sizleştirmenize yardımcı olan temel özellikler hakkında bilgi edinin.
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
ms.date: 03/09/2020
ms.openlocfilehash: d37b4648c0a37f16fe5c9d8794bd78417c5780ea
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80257895"
---
# <a name="copy-activity-performance-optimization-features"></a>Etkinlik performans optimizasyonu özelliklerini kopyalama

Bu makalede, Azure Veri Fabrikası'nda yararlanabileceğiniz kopyalama etkinliği optimizasyonu özellikleri özetlenmektedir.

## <a name="data-integration-units"></a>Veri Tümleştirme Birimleri

Veri Tümleştirme Birimi, Azure Veri Fabrikası'ndaki tek bir birimin gücünü (CPU, bellek ve ağ kaynağı ayırmanın birleşimi) temsil eden bir ölçüdür. Veri Tümleştirme Birimi yalnızca [Azure tümleştirme çalışma zamanı](concepts-integration-runtime.md#azure-integration-runtime)için geçerlidir, ancak kendi kendine [barındırılan tümleştirme çalışma zamanı](concepts-integration-runtime.md#self-hosted-integration-runtime)için geçerli değildir.

İzin verilen DIUs'ların bir kopyalama etkinliğini güçlendirmesi 2 ile **256 arasındadır.** UI'de belirtilmemişse veya "Otomatik" seçeneğini seçerseniz, Veri Fabrikası kaynak lavabo çiftinize ve veri deseninize göre en iyi DIU ayarını dinamik olarak uygular. Aşağıdaki tablo, desteklenen DIU aralıklarını ve farklı kopyalama senaryolarında varsayılan davranışı listeler:

| Senaryoyu kopyalama | Desteklenen DIU aralığı | Hizmet tarafından belirlenen Varsayılan DIUs'lar |
|:--- |:--- |---- |
| Dosya mağazaları arasında |- **Dosyadan veya tek dosyadan kopyala**: 2-4 <br>- **Kopya ve birden fazla dosya :** 2-256 dosyaların sayısı ve boyutuna bağlı olarak <br><br>Örneğin, 4 büyük dosyaiçeren bir klasördeki verileri kopyalar ve hiyerarşiyi korumayı seçerseniz, etkin diu'nun maksimumu 16'dır; dosyayı birleştirmeyi seçtiğinizde, etkin DIU maksimum 4'dür. |Dosyaların sayısına ve boyutuna bağlı olarak 4 ile 32 arasında |
| Dosya deposundan dosya olmayan depoya |- **Tek dosyadan kopya**: 2-4 <br/>- **Birden fazla dosyadan kopyala**: Dosyaların sayısına ve boyutuna bağlı olarak 2-256 <br/><br/>Örneğin, 4 büyük dosyaiçeren bir klasördeki verileri kopyalarsanız, etkin DIU'nun maksimumu 16'dır. |- **Azure SQL Veritabanına veya Azure Cosmos DB'ye kopyalayın**: lavabo katmanına (DTUs/RUs) ve kaynak dosya desenine bağlı olarak 4 ile 16 arasında<br>- PolyBase veya COPY deyimini kullanarak **Azure Synapse Analytics'e kopyalayın:** 2<br>- Diğer senaryo: 4 |
| Dosya olmayan depodan dosya deposuna |- **Bölüm seçeneği etkin veri depolarından kopyalayın** [(Oracle](connector-oracle.md#oracle-as-source)/[Netezza](connector-netezza.md#netezza-as-source)/[Teradata](connector-teradata.md#teradata-as-source)dahil): Bir klasöre yazarken 2-256 ve tek bir dosyaya yazarken 2-4. Kaynak başına not veri bölümü en fazla 4 DIUs kullanabilirsiniz.<br>- **Diğer senaryolar**: 2-4 |- **REST veya HTTP'den kopyala**: 1<br/>- UNLOAD kullanarak **Amazon Redshift kopyalayın:** 2<br>- **Diğer senaryo**: 4 |
| Dosya olmayan mağazalar arasında |- **Bölüm seçeneği etkin veri depolarından kopyalayın** [(Oracle](connector-oracle.md#oracle-as-source)/[Netezza](connector-netezza.md#netezza-as-source)/[Teradata](connector-teradata.md#teradata-as-source)dahil): Bir klasöre yazarken 2-256 ve tek bir dosyaya yazarken 2-4. Kaynak başına not veri bölümü en fazla 4 DIUs kullanabilirsiniz.<br/>- **Diğer senaryolar**: 2-4 |- **REST veya HTTP'den kopyala**: 1<br>- **Diğer senaryo**: 4 |

Kopyalama etkinliği izleme görünümünde veya etkinlik çıktısında her kopya çalışması için kullanılan DIUs'ları görebilirsiniz. Daha fazla bilgi için [bkz.](copy-activity-monitoring.md) Bu varsayılan geçersiz kılmak için, `dataIntegrationUnits` aşağıdaki gibi özellik için bir değer belirtin. Kopya işleminin çalışma zamanında kullandığı *gerçek DIUs sayısı,* veri deseninize bağlı olarak yapılandırılan değere eşit veya daha azdır.

**Kullanılmış DIUs \* kopyalama süresi \* birim fiyat/DIU-saat #** tahsil edilecektir. [Burada](https://azure.microsoft.com/pricing/details/data-factory/data-pipeline/)güncel fiyatlara bakın. Abonelik türü başına yerel para birimi ve ayrı iskonto uygulanabilir.

**Örnek:**

```json
"activities":[
    {
        "name": "Sample copy activity",
        "type": "Copy",
        "inputs": [...],
        "outputs": [...],
        "typeProperties": {
            "source": {
                "type": "BlobSource",
            },
            "sink": {
                "type": "AzureDataLakeStoreSink"
            },
            "dataIntegrationUnits": 128
        }
    }
]
```

## <a name="self-hosted-integration-runtime-scalability"></a>Kendi kendine barındırılan tümleştirme çalışma zamanı ölçeklenebilirliği

Daha yüksek iş elde etmek istiyorsanız, Kendi kendine barındırılan IR'yi ölçeklendirebilir veya ölçeklendirebilirsiniz:

- Cpu ve Kullanılabilir Bellek Self barındırılan IR düğümü tam olarak kullanılmır, ancak aynı işlerin yürütülmesi sınırı alıyorsa, düğümünde çalışabilecek çağdaşilerin sayısını artırarak ölçeklendirmeniz gerekir.  Talimatlar için [buraya](create-self-hosted-integration-runtime.md#scale-up) bakın.
- Diğer taraftan, CPU Kendi kendine barındırılan IR düğümünde yüksekse veya kullanılabilir bellek düşükse, yükü birden çok düğüm arasında ölçeklendirmeye yardımcı olacak yeni bir düğüm ekleyebilirsiniz.  Talimatlar için [buraya](create-self-hosted-integration-runtime.md#high-availability-and-scalability) bakın.

Aşağıdaki senaryolarda, tek kopya etkinliği yürütme birden çok Self barındırılan IR düğümleri kaldıraç olabilir:

- Dosyaların sayısına ve boyutuna bağlı olarak dosya tabanlı mağazalardaki verileri kopyalayın.
- Veri bölümlerisayısına bağlı olarak, bölüm seçeneği etkin veri deposundan [(Oracle,](connector-oracle.md#oracle-as-source) [Netezza,](connector-netezza.md#netezza-as-source) [Teradata,](connector-teradata.md#teradata-as-source) [SAP HANA,](connector-sap-hana.md#sap-hana-as-source) [SAP Table](connector-sap-table.md#sap-table-as-source)ve SAP [Open Hub](connector-sap-business-warehouse-open-hub.md#sap-bw-open-hub-as-source)dahil) verileri kopyalayın.

## <a name="parallel-copy"></a>Paralel kopya

Kopyalama etkinliğinin kullanmasını`parallelCopies` istediğiniz paralelliği belirtmek için kopya etkinliğine paralel kopya (özellik) ayarlayabilirsiniz. Bu özelliği, kaynağınızdan okunan veya paralel olarak lavabo veri depolarınıza yazan kopyalama etkinliği içindeki maksimum iş parçacığı sayısı olarak düşünebilirsiniz.

Paralel kopya [Veri Tümleştirme Birimleri](#data-integration-units) veya [Kendi kendine barındırılan IR düğümleri](#self-hosted-integration-runtime-scalability)için ortogonal. Tüm DIUs veya Kendi kendine barındırılan IR düğümleri arasında sayılır.

Her kopyalama etkinliği için, varsayılan olarak Azure Veri Fabrikası, kaynak lavabo çiftinize ve veri deseninize göre en iyi paralel kopyalama ayarını dinamik olarak uygulayın. 

> [!TIP]
> Paralel kopyanın varsayılan davranışı genellikle kaynak lavabo çiftinize, veri deseninize ve CPU sayısına veya Kendi kendine barındırılan IR'nin CPU/bellek/düğüm sayısına göre ADF tarafından otomatik olarak belirlenen en iyi iş verisini sağlar. Paralel kopyanın ne zaman ayarlandığına ilişkin [Sorun Giderme etkinliği performansına](copy-activity-performance-troubleshooting.md) bakın.

Aşağıdaki tabloda paralel kopyalama davranışı listelenir:

| Senaryoyu kopyalama | Paralel kopyalama davranışı |
| --- | --- |
| Dosya mağazaları arasında | `parallelCopies`**dosya düzeyinde**paralelliği belirler. Her dosyanın içindeki yığınlar otomatik ve saydam olarak altında gerçekleşir. Verileri paralel olarak yüklemek için belirli bir veri deposu türü için en uygun yığın boyutunu kullanmak üzere tasarlanmıştır. <br/><br/>Çalışma zamanında kullandığı paralel kopya kopyalama etkinliğinin gerçek sayısı, sahip olduğunuz dosya sayısından fazla değildir. Kopyalama davranışı dosya bağına **birleştirmeDosya** ise, kopyalama etkinliği dosya düzeyinde paralellikten yararlanamaz. |
| Dosya deposundan dosya olmayan depoya | - Verileri Azure SQL Veritabanı'na veya Azure Cosmos DB'ye kopyalarken, varsayılan paralel kopya da lavabo katmanına (DTR/RUs sayısı) bağlıdır.<br>- Verileri Azure Tablosu'na kopyalarken varsayılan paralel kopya 4'tür. |
| Dosya olmayan depodan dosya deposuna | - Bölüm seçeneği etkin veri deposundan [(Oracle,](connector-oracle.md#oracle-as-source) [Netezza,](connector-netezza.md#netezza-as-source) [Teradata,](connector-teradata.md#teradata-as-source) [SAP HANA, SAP](connector-sap-hana.md#sap-hana-as-source) [Table](connector-sap-table.md#sap-table-as-source)ve SAP [Open Hub](connector-sap-business-warehouse-open-hub.md#sap-bw-open-hub-as-source)dahil) veri kopyalarken varsayılan paralel kopya 4'dür. Çalışma zamanında kullandığı paralel kopya kopyalama etkinliğinin gerçek sayısı, sahip olduğunuz veri bölümleri sayısından fazla değildir. Kendi kendine barındırılan Tümleştirme Çalışma Süresi'ni kullandığınızda ve Azure Blob/ADLS Gen2'ye kopyaladığızaman, maksimum etkili paralel kopyanın IR düğümü başına 4 veya 5 olduğunu unutmayın.<br>- Diğer senaryolar için paralel kopya etkili olmaz. Paralellik belirtilmiş olsa bile uygulanmaz. |
| Dosya olmayan mağazalar arasında | - Verileri Azure SQL Veritabanı'na veya Azure Cosmos DB'ye kopyalarken, varsayılan paralel kopya da lavabo katmanına (DTR/RUs sayısı) bağlıdır.<br/>- Bölüm seçeneği etkin veri deposundan [(Oracle,](connector-oracle.md#oracle-as-source) [Netezza,](connector-netezza.md#netezza-as-source) [Teradata,](connector-teradata.md#teradata-as-source) [SAP HANA, SAP](connector-sap-hana.md#sap-hana-as-source) [Table](connector-sap-table.md#sap-table-as-source)ve SAP [Open Hub](connector-sap-business-warehouse-open-hub.md#sap-bw-open-hub-as-source)dahil) veri kopyalarken varsayılan paralel kopya 4'dür.<br>- Verileri Azure Tablosu'na kopyalarken varsayılan paralel kopya 4'tür. |

Veri depolarınızı barındıran makinelerdeki yükü denetlemek veya kopyalama performansını ayarlamak için varsayılan değeri geçersiz kılabilir ve özellik için bir değer belirtebilirsiniz. `parallelCopies` Değer, 1'den büyük veya eşit bir karşıcı olmalıdır. Çalışma zamanında, en iyi performans için kopyalama etkinliği, ayarladığınız değerden daha az veya eşit bir değer kullanır.

`parallelCopies` Özellik için bir değer belirttiğinizde, kaynağınızdaki yük artışını alın ve veri depolarını da hesaba katın. Ayrıca, kopyalama etkinliği tarafından yetkilendirilmişse, kendi kendine barındırılan tümleştirme çalışma süresine yük artışını da göz önünde bulundurun. Bu yük artışı, özellikle aynı veri deposuna karşı çalışan aynı etkinliklerin birden çok aktivitesi veya eşzamanlı çalıştırmaları varsa gerçekleşir. Veri deposunun veya kendi kendine barındırılan tümleştirme çalışma süresinin yükten `parallelCopies` bunalmış olduğunu fark ederseniz, yükü hafifletmek için değeri azaltın.

**Örnek:**

```json
"activities":[
    {
        "name": "Sample copy activity",
        "type": "Copy",
        "inputs": [...],
        "outputs": [...],
        "typeProperties": {
            "source": {
                "type": "BlobSource",
            },
            "sink": {
                "type": "AzureDataLakeStoreSink"
            },
            "parallelCopies": 32
        }
    }
]
```

## <a name="staged-copy"></a>Aşamalı kopya

Verileri bir kaynak veri deposundan lavabo veri deposuna kopyaladiğinizde, Blob depolama alanını geçici bir hazırlama deposu olarak kullanmayı seçebilirsiniz. Evreleme özellikle aşağıdaki durumlarda yararlıdır:

- **Çeşitli veri depolarından gelen verileri PolyBase üzerinden SQL Veri Ambarı'na almak istiyorsunuz.** SQL Veri Ambarı, büyük miktarda veriyi SQL Veri Ambarı'na yüklemek için yüksek iş kaynağı mekanizması olarak PolyBase kullanır. Kaynak veriler Blob depolama veya Azure Veri Gölü Deposu'nda olmalıdır ve ek ölçütleri karşılamalıdır. Blob depolama veya Azure Veri Gölü Deposu dışındaki bir veri deposundan veri yüklediğinizde, geçici evreleme Blob depolama yoluyla veri kopyalamayı etkinleştirebilirsiniz. Bu durumda, Azure Veri Fabrikası, PolyBase gereksinimlerini karşıladığından emin olmak için gerekli veri dönüşümlerini gerçekleştirir. Daha sonra verileri SQL Veri Ambarı'na verimli bir şekilde yüklemek için PolyBase'i kullanır. Daha fazla bilgi [için, verileri Azure SQL Veri Ambarı'na yüklemek için PolyBase'i kullanın'a](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-sql-data-warehouse)bakın.
- **Bazen yavaş bir ağ bağlantısı üzerinden karma veri hareketi gerçekleştirmek (diğer bir şekilde şirket içi veri deposundan bulut veri deposuna kopyalamak) zaman alır.** Performansı artırmak için, verileri buluttaki hazırlama veri deposuna taşımanın daha az zaman alması için verileri şirket içinde sıkıştırmak için aşamalı kopyayı kullanabilirsiniz. Ardından, hedef veri deposuna yüklemeden önce hazırlama deposundaki verileri sıkıştırabilirsiniz.
- **Kurumsal BT ilkeleri nedeniyle güvenlik duvarınızda 80 ve bağlantı noktası 443 dışındaki bağlantı noktalarını açmak istemezsiniz.** Örneğin, şirket içi veri deposundan Azure SQL Veritabanı lavabosuna veya Azure SQL Veri Ambarı lavabosuna verileri kopyalarken, hem Windows güvenlik duvarınız hem de şirket güvenlik duvarınız için 1433 bağlantı noktasında giden TCP iletişimini etkinleştirmeniz gerekir. Bu senaryoda, sahnelenen kopya, verileri 443 bağlantı noktasındahttp veya HTTPS üzerinden blob depolama evreleme örneğine ilk kopyalamak için kendi barındırılan tümleştirme çalışma süresinden yararlanabilir. Daha sonra, Blob depolama evrelemesinden verileri SQL Veritabanı'na veya SQL Veri Ambarı'na yükleyebilir. Bu akışta, bağlantı noktası 1433'e etkinleştirmeniz gerekmez.

### <a name="how-staged-copy-works"></a>Aşamalı kopya nasıl çalışır?

Hazırlama özelliğini etkinleştirdiğinizde, önce veriler kaynak veri deposundan evreleme Blob depolamasına kopyalanır (kendi verinizi getirin). Ardından, veriler hazırlama veri deposundan lavabo veri deposuna kopyalanır. Azure Veri Fabrikası sizin için iki aşamalı akışı otomatik olarak yönetir. Azure Veri Fabrikası, veri hareketi tamamlandıktan sonra hazırlama depolamasından gelen geçici verileri de temizler.

![Aşamalı kopya](media/copy-activity-performance/staged-copy.png)

Bir evreleme deposu kullanarak veri hareketini etkinleştirdiğinizde, verileri kaynak veri deposundan geçici veya evreleme veri deposuna taşımadan önce verilerin sıkıştırılıp sıkıştırılmamasını isteyip istemediğiniz ve verileri geçici veya evrelemeden taşımadan önce desıkıştırılmanızı isteyip istemediğinizbelirtebilirsiniz veri deposuna veri deposu.

Şu anda, farklı Self barındırılan IRs üzerinden bağlı iki veri deposu arasındaki verileri kopyalayamaz, ne ile ne de aşamalı kopya olmadan. Bu tür bir senaryo için, kaynaktan evrelemesonra da evrelemeden lavaboya kopyalamak için iki açık zincirlenmiş kopyalama etkinliği yapılandırabilirsiniz.

### <a name="configuration"></a>Yapılandırma

Veri oluşturma etkinliğini, hedef veri deposuna yüklemeden önce verilerin Blob depolama alanında düzenlenmesini isteyip istemediğinizbelirtilmek üzere kopyalama etkinliğindeki **etkinleştirme Hazırlama** ayarını yapılandırın. **enableStaging'i** `TRUE`ayarladığınızda, aşağıdaki tabloda listelenen ek özellikleri belirtin. Ayrıca, evreleme için bir Azure Depolama veya Depolama paylaşılan erişim imzasına bağlı bir hizmet oluşturmanız gerekir.

| Özellik | Açıklama | Varsayılan değer | Gerekli |
| --- | --- | --- | --- |
| enableStaging |Geçici bir hazırlama deposu aracılığıyla verileri kopyalamak isteyip istemediğinizi belirtin. |False |Hayır |
| linkedServiceName |Geçici bir hazırlama deposu olarak kullandığınız Depolama örneğine atıfta bulunan [AzureStorage](connector-azure-blob-storage.md#linked-service-properties) bağlantılı bir hizmetin adını belirtin. <br/><br/> PolyBase üzerinden SQL Veri Ambarı'na veri yüklemek için paylaşılan bir erişim imzasıyla Depolama'yı kullanamazsınız. Diğer tüm senaryolarda kullanabilirsiniz. |Yok |Evet, **enableStaging** TRUE olarak ayarlandığında |
| yol |Aşamalı verileri içermek istediğiniz Blob depolama yolunu belirtin. Bir yol sağlamazsanız, hizmet geçici verileri depolamak için bir kapsayıcı oluşturur. <br/><br/> Bir yol belirtin, yalnızca paylaşılan bir erişim imzasıyla Depolama'yı kullanıyorsanız veya geçici verilerin belirli bir konumda olmasını istiyorsanız. |Yok |Hayır |
| etkinleştirsıkıştırma |Verilerin hedefe kopyalanıp kopyalanmayacağını belirtir. Bu ayar aktarılan veri hacmini azaltır. |False |Hayır |

>[!NOTE]
> Sıkıştırma etkinleştirilmiş aşamalı kopya kullanıyorsanız, blob bağlantılı hizmet evreleme için servis sorumlusu veya MSI kimlik doğrulaması desteklenmez.

Aşağıda, önceki tabloda açıklanan özelliklere sahip bir kopyalama etkinliğinin örnek tanımı verilmiştir:

```json
"activities":[
    {
        "name": "Sample copy activity",
        "type": "Copy",
        "inputs": [...],
        "outputs": [...],
        "typeProperties": {
            "source": {
                "type": "SqlSource",
            },
            "sink": {
                "type": "SqlSink"
            },
            "enableStaging": true,
            "stagingSettings": {
                "linkedServiceName": {
                    "referenceName": "MyStagingBlob",
                    "type": "LinkedServiceReference"
                },
                "path": "stagingcontainer/path",
                "enableCompression": true
            }
        }
    }
]
```

### <a name="staged-copy-billing-impact"></a>Aşamalı kopyalama faturalandırma etkisi

İki adıma göre ücretlendirilirsiniz: kopyalama süresi ve kopya türü.

* Bir bulut veri deposundan başka bir bulut veri deposuna veri kopyalama sırasında evreleme kullandığınızda, her iki aşamada da Azure tümleştirme çalışma süresi yle güçlendirilmiştir, [adım 1 ve adım 2] x [bulut kopyalama birim fiyatı] için kopya süresi nin toplamı ücretlendirilirsiniz.
* Bir şirket içi veri deposundan bulut veri deposuna veri kopyalama, kendi kendine barındırılan tümleştirme çalışma süresi tarafından yetkilendirilmiş bir aşama olan karma kopya sırasında evreleme kullandığınızda, [karma kopyalama süresi] x [karma kopyalama birim fiyatı] + [bulut kopyalama süresi] için ücretlendirilirsiniz x [bulut kopyalama birim fiyatı].

## <a name="next-steps"></a>Sonraki adımlar
Diğer kopyalama etkinliği makalelerini görün:

- [Kopyalama etkinliğine genel bakış](copy-activity-overview.md)
- [Etkinlik performansı ve ölçeklenebilirlik kılavuzunu kopyalama](copy-activity-performance.md)
- [Sorun giderme kopyalama etkinliği performansı](copy-activity-performance-troubleshooting.md)
- [Verilerinizi veri gölünüzden veya veri ambarınızdan Azure'a geçirmek için Azure Veri Fabrikası'nı kullanın](data-migration-guidance-overview.md)
- [Verileri Amazon S3'ten Azure Depolama'ya geçirme](data-migration-guidance-s3-azure-storage.md)