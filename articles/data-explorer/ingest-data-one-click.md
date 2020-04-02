---
title: Azure Veri Gezgini'ne veri almak için tek tıklamayla yararlanma kullanma
description: Tek tıklamayla Azure Veri Gezgini'ne veri alma (yükleme) genel bakışı.
author: orspod
ms.author: orspodek
ms.reviewer: tzgitlin
ms.service: data-explorer
ms.topic: overview
ms.date: 03/29/2020
ms.openlocfilehash: 5bde63427ce76f14832551864bbf2c3d8e015fd6
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80521571"
---
# <a name="what-is-one-click-ingestion"></a>Tek tıklamayla yutma nedir? 

Tek tıklatma işlemi, Azure Veri Gezgini'ndeki bir veri kaynağına dayalı olarak verileri hızla yutmanızı ve otomatik olarak tablo ve eşleme yapıları önermenizi sağlar. 

Azure Veri Gezgini Web UI'sini kullanarak, depolama (blob dosyası), yerel bir dosya veya bir kapsayıcıdan (en fazla 10.000 blob) veri yutabilirsiniz. Sürekli yutma için bir kapsayıcı üzerinde bir olay ızgarası da tanımlayabilirsiniz. Veriler JSON, CSV ve [diğer biçimlerde](#file-formats)varolan veya yeni bir tabloya yutulabilir. Tek bir tıklatma, veri kaynağına dayalı yeni bir tablo ve tablo eşlemesi için bir yapı önerebilir ve varolan tablo ve tablo eşlemesinin tablo yapısını ayarlamak için sezgisel bir platform sağlayabilir. Tek bir tıklama yla veriler yalnızca birkaç dakika içinde tabloya girer.

Tek tıklatma, verileri ilk kez sindirirken veya verilerinizin şeması size yabancı olduğunda özellikle yararlıdır.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.
* [Azure Veri Gezgini kümesi ve veritabanı](create-cluster-database-portal.md)oluşturun.
* Azure Veri [Gezgini Web UI'de](https://dataexplorer.azure.com/) oturum açın ve [kümenize bağlantı ekleyin.](/azure/data-explorer/web-query-data#add-clusters)

## <a name="file-formats"></a>Dosya biçimleri

Tek tıklatma, kaynak verilerden aşağıdaki biçimlerden herhangi birinde yeni bir tablo nun sindirilmesine destek olur:
* JSON
* CSV
* Tsv
* SCSV
* SOHSV
* TSVE
* Psv

## <a name="one-click-ingestion-wizard"></a>Tek tıklamayla yutma sihirbazı

Tek tıklatma sihirbazı, tek tıklatma işlemi boyunca size yol gösteriyor. 

> [!Note]
> Bu bölümde genel olarak sihirbaz açıklanır. Seçtiğiniz seçenekler, yeni veya varolan bir tabloyu sindirip yutmadığınıza bağlıdır. Daha fazla bilgi için bkz.
    > * [Yeni bir tabloya](one-click-ingestion-new-table.md) girme
    > * Varolan bir [tabloya](one-click-ingestion-existing-table.md) alma 
    
1. Sihirbaza erişmek için Azure Veri Gezgini web UI'nin sol menüsündeki *veritabanıveya* *tablo* satırına sağ tıklayın ve **yeni verileri (önizleme) yut'u**seçin.

    ![Web Web Web Arama Sürümü'nde tek tıklatma oluşturma yı seçme](media/ingest-data-one-click/one-click-ingestion-in-webui.png)   

1. Sihirbaz aşağıdaki seçenekler de size rehberlik eder:
       * Varolan bir [tabloya](one-click-ingestion-existing-table.md) alma
       * [Yeni bir tabloya](one-click-ingestion-new-table.md) girme
       * Veri yutma: * Blob depolama * Yerel bir dosya * Bir kapsayıcı
       * Örnek boyutunu 1 ile 10.000 satır arasında girin (yalnızca kapsayıcıdan)
       
1. Veri kaynağını başarıyla seçtiğinizde, verilerin önizlemesi görüntülenir. 
    Bir kapsayıcıdan veri yutamıyorsanız, verileri yalnızca belirli öneeklere veya dosya uzantılarına sahip dosyaların yutulabilmesi için filtreleyebilirsiniz. Örneğin, yalnızca *Avrupa*sözcüğüyle başlayan dosya adlarına sahip dosyaları veya yalnızca uzantı .json olan dosyaları yutmak *isteyebilirsiniz.* 

1. **Şema'yı Edit'e**tıklayın. Belirli bir tabloya veri sindiriyorsanız, kaynak sütunları hedef sütunlara eşleyebilir ve sütun adlarını dahil edip etmemeye karar verebilirsiniz.

1. Veri alma işlemini başlatın.

> [!Note]
> Veri kaynağınız bir kapsayıcıysa, Azure Veri Gezgini'nin veri toplama toplama (toplu oluşturma) ilkesinin yutma işlemini optimize etmek için tasarladığını unutmayın. Varsayılan olarak, ilke 5 dakika veya 500 MB veri olarak yapılandırılır, böylece gecikme karşılaşabilirsiniz. Toplama seçenekleri için [toplu laştırma ilkesine](/azure/kusto/concepts/batchingpolicy) bakın. Diğer kaynaklardan veri alırken, yutma hemen etkili olacaktır.

## <a name="next-steps"></a>Sonraki adımlar

* [Varolan bir tabloya](one-click-ingestion-existing-table.md) veya [yeni bir tabloya](one-click-ingestion-new-table.md) veri almak için tek tıklatma kullanıp kullanmamadığınıza karar verin
* [Azure Veri Gezgini Web UI'deki verileri sorgula](/azure/data-explorer/web-query-data)
* [Kusto Query Language'i kullanarak Azure Veri Gezgini için sorgu yazma](/azure/data-explorer/write-queries)