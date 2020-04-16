---
title: Azure Veri Fabrikası fiyatlandırmalarını örneklerle anlama
description: Bu makalede, ayrıntılı örneklerle Azure Veri Fabrikası fiyatlandırma modelini açıklar ve gösterir
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 12/27/2019
ms.openlocfilehash: 9d96e3f7d127f4839592e766537cbdb07cc697dc
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81414934"
---
# <a name="understanding-data-factory-pricing-through-examples"></a>Örnekler aracılığıyla Veri Fabrikası fiyatlandırmaanlama

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Bu makalede, ayrıntılı örneklerle Azure Veri Fabrikası fiyatlandırma modelini açıklar ve gösterir.

> [!NOTE]
> Aşağıdaki örneklerde kullanılan fiyatlar varsayımsalolup, gerçek fiyatlandırmayı ima etmeamacında değildir.

## <a name="copy-data-from-aws-s3-to-azure-blob-storage-hourly"></a>AWS S3'ten Azure Blob depolamasına saatlik veri kopyalama

Bu senaryoda, AWS S3'ten Azure Blob depolama alanına saatlik zamanlamayla verileri kopyalamak istiyorsunuz.

Senaryoyu gerçekleştirmek için aşağıdaki öğelerden bir ardışık kaynak oluşturmanız gerekir:

1. Verilerin AWS S3'ten kopyalanması için giriş veri kümesine sahip bir kopyalama etkinliği.

2. Azure Depolama'daki veriler için bir çıktı veri kümesi.

3. Boru hattını her saat çalıştıracak bir zamanlama tetikleyicisi.

   ![Senaryo1](media/pricing-concepts/scenario1.png)

| **İşlemler** | **Türleri ve Birimleri** |
| --- | --- |
| Bağlantılı Hizmet Oluştur | 2 Oku/Yaz varlığı  |
| Veri Kümeleri Oluşturma | 4 Okuma/Yazma varlıkları (veri kümesi oluşturma için 2, bağlantılı hizmet başvuruları için 2) |
| Boru Hattı Oluştur | 3 Okuma/Yazma varlıkları (1'i boru hattı oluşturma, 2'si veri kümesi başvuruları için) |
| Boru Hattı alın | 1 Oku/Yaz varlığı |
| Çalıştır Boru Hattı | 2 Etkinlik çalışır (tetikleme çalışması için 1, etkinlik çalıştırmaları için 1) |
| Veri Varsayımı Kopyala: yürütme süresi = 10 dk | 10 \* 4 Azure Tümleştirme Çalışma Süresi (varsayılan DIU ayarı = 4) Veri tümleştirme birimleri hakkında daha fazla bilgi ve kopya performansını optimize etmek için [bu makaleye](copy-activity-performance.md) bakın |
| DenetimİzE VarAKSI DenetimI Denetimi Varsayımı: Yalnızca 1 çalıştırma oluştu | 2 İzleme çalıştırma kayıtları yeniden denenmiş (1 ardışık hat lar için, 1 etkinlik çalışması için) |

**Toplam Senaryo fiyatlandırması: $0.16811**

- Veri Fabrikası İşlemleri = **$0.0001**
  - Okuma/Yazma =\*10 00001 = $0.0001 [1 R/W = $0.50/50000 = 0.00001]
  - İzleme =\*2 000005 = $0.00001 [1 İzleme = $0.25/50000 = 0,000005]
- Boru Hattı &amp; Orkestrasyon Yürütme = **0,168 $**
  - Etkinlik Çalışır =\*001 2 = 0.002 [1 çalıştırın = $1/1000 = 0.001]
  - Veri Hareketi Etkinlikleri = $0.166 (Yürütme süresinin 10 dakikası için eşit olarak verilir. Azure Tümleştirme Çalışma Zamanında $0.25/saat)

## <a name="copy-data-and-transform-with-azure-databricks-hourly"></a>Azure Databricks ile verileri saatlik olarak kopyalayın ve dönüştürün

Bu senaryoda, AWS S3'ten Azure Blob depolamasına kadar olan verileri kopyalamak ve verileri Azure Databricks ile saatlik zamanlamayla dönüştürmek istiyorsunuz.

Senaryoyu gerçekleştirmek için aşağıdaki öğelerden bir ardışık kaynak oluşturmanız gerekir:

1. Verilerin AWS S3'ten kopyalanması için giriş veri kümesine ve Azure depolamadaki veriler için çıktı veri kümesine sahip bir kopyalama etkinliği.
2. Veri dönüşümü için bir Azure Databricks etkinliği.
3. Boru hattını her saat çalıştıracak bir zamanlama tetikleyicisi.

![Senaryo2](media/pricing-concepts/scenario2.png)

| **İşlemler** | **Türleri ve Birimleri** |
| --- | --- |
| Bağlantılı Hizmet Oluştur | 3 Oku/Yaz varlığı  |
| Veri Kümeleri Oluşturma | 4 Okuma/Yazma varlıkları (veri kümesi oluşturma için 2, bağlantılı hizmet başvuruları için 2) |
| Boru Hattı Oluştur | 3 Okuma/Yazma varlıkları (1'i boru hattı oluşturma, 2'si veri kümesi başvuruları için) |
| Boru Hattı alın | 1 Oku/Yaz varlığı |
| Çalıştır Boru Hattı | 3 Etkinlik çalışır (tetikleme çalışması için 1, etkinlik çalıştırmaları için 2) |
| Veri Varsayımı Kopyala: yürütme süresi = 10 dk | 10 \* 4 Azure Tümleştirme Çalışma Süresi (varsayılan DIU ayarı = 4) Veri tümleştirme birimleri hakkında daha fazla bilgi ve kopya performansını optimize etmek için [bu makaleye](copy-activity-performance.md) bakın |
| DenetimİzE VarAKSI DenetimI Denetimi Varsayımı: Yalnızca 1 çalıştırma oluştu | 3 İzleme çalıştırma kayıtları yeniden denenmiş (1 boru hattı çalıştırımı için, 2 etkinlik çalışması için) |
| Databricks etkinliği Varsayım yürüt: yürütme süresi = 10 dk | 10 dk Dış Boru Hattı Faaliyet Yürütme |

**Toplam Senaryo fiyatlandırması: $0.16916**

- Veri Fabrikası İşlemleri = **$0.00012**
  - Okuma/Yazma =\*11 00001 = $0.00011 [1 R/W = $0.50/50000 = 0.00001]
  - İzleme =\*3 000005 = $0.00001 [1 İzleme = $0.25/50000 = 0,000005]
- Boru Hattı &amp; Orkestrasyon Yürütme = **0,16904 $**
  - Etkinlik Çalışır =\*001 3 = 0.003 [1 çalıştırın = $1/1000 = 0.001]
  - Veri Hareketi Etkinlikleri = $0.166 (Yürütme süresinin 10 dakikası için eşit olarak verilir. Azure Tümleştirme Çalışma Zamanında $0.25/saat)
  - Dış Boru Hattı Etkinliği = $0.000041 (10 dakikalık yürütme süresi için eşit olarak eşitolarak sürülür. Azure Tümleştirme Çalışma Süresi'nde $0.00025/saat)

## <a name="copy-data-and-transform-with-dynamic-parameters-hourly"></a>Verileri kopyalayın ve dinamik parametrelerle saatlik dönüştürme

Bu senaryoda, AWS S3'ten Azure Blob depolama alanına kadar olan verileri kopyalamak ve Azure Veri Tuğlaları (komut dosyasındaki dinamik parametrelerle) saatlik zamanlamayla dönüştürmek istiyorsunuz.

Senaryoyu gerçekleştirmek için aşağıdaki öğelerden bir ardışık kaynak oluşturmanız gerekir:

1. Verilerin Azure depolamadaki veriler için bir çıktı veri kümesi olan AWS S3'ten kopyalanması için giriş veri kümesine sahip bir kopyalama etkinliği.
2. Parametreleri dönüşüm komut dosyasına dinamik olarak geçirmek için bir Arama etkinliği.
3. Veri dönüşümü için bir Azure Databricks etkinliği.
4. Boru hattını her saat çalıştıracak bir zamanlama tetikleyicisi.

![Senaryo3](media/pricing-concepts/scenario3.png)

| **İşlemler** | **Türleri ve Birimleri** |
| --- | --- |
| Bağlantılı Hizmet Oluştur | 3 Oku/Yaz varlığı  |
| Veri Kümeleri Oluşturma | 4 Okuma/Yazma varlıkları (veri kümesi oluşturma için 2, bağlantılı hizmet başvuruları için 2) |
| Boru Hattı Oluştur | 3 Okuma/Yazma varlıkları (1'i boru hattı oluşturma, 2'si veri kümesi başvuruları için) |
| Boru Hattı alın | 1 Oku/Yaz varlığı |
| Çalıştır Boru Hattı | 4 Etkinlik çalışır (tetikleme çalışması için 1, etkinlik çalıştırmaları için 3) |
| Veri Varsayımı Kopyala: yürütme süresi = 10 dk | 10 \* 4 Azure Tümleştirme Çalışma Süresi (varsayılan DIU ayarı = 4) Veri tümleştirme birimleri hakkında daha fazla bilgi ve kopya performansını optimize etmek için [bu makaleye](copy-activity-performance.md) bakın |
| DenetimİzE VarAKSI DenetimI Denetimi Varsayımı: Yalnızca 1 çalıştırma oluştu | 4 İzleme çalıştırma kayıtları yeniden denenmiş (1 boru hattı çalıştırımı için, 3 etkinlik çalışması için) |
| Yürütme Arama etkinliği Varsayım: yürütme süresi = 1 dk | 1 dk Boru Hattı Etkinliği yürütme |
| Databricks etkinliği Varsayım yürüt: yürütme süresi = 10 dk | 10 dk Dış Boru Hattı Etkinliği yürütme |

**Toplam Senaryo fiyatlandırması: $0.17020**

- Veri Fabrikası İşlemleri = **$0.00013**
  - Okuma/Yazma =\*11 00001 = $0.00011 [1 R/W = $0.50/50000 = 0.00001]
  - İzleme =\*4 000005 = $0.00002 [1 İzleme = $0.25/50000 = 0,000005]
- Boru Hattı &amp; Orkestrasyon Yürütme = **0,17007 $**
  - Etkinlik Çalışır =\*001 4 = 0.004 [1 çalıştırın = $1/1000 = 0.001]
  - Veri Hareketi Etkinlikleri = $0.166 (Yürütme süresinin 10 dakikası için eşit olarak verilir. Azure Tümleştirme Çalışma Zamanında $0.25/saat)
  - Boru Hattı Etkinliği = $0.00003 (Yürütme süresinin 1 dakikası için eşit olarak eşittir. Azure Tümleştirme Çalışma Zamanında $0.002/saat)
  - Dış Boru Hattı Etkinliği = $0.000041 (10 dakikalık yürütme süresi için eşit olarak eşitolarak sürülür. Azure Tümleştirme Çalışma Süresi'nde $0.00025/saat)

## <a name="using-mapping-data-flow-debug-for-a-normal-workday"></a>Normal bir iş günü için veri akışı hata ayıklama eşleme kullanma

Veri Mühendisi olarak, haritalama veri akışlarını her gün tasarlamaktan, oluşturmakve test etmek sizin sorumluluğunuzdadır. Sabah ADF UI'ye giriş yapın ve Veri Akışları için Hata Ayıklama modunu etkinleştirin. Hata Ayıklama oturumları için varsayılan TTL 60 dakikadır. Hata Ayıklama oturumunuzun süresinin dolmaması için gün boyunca 8 saat çalışırsınız. Bu nedenle, gün için ücret olacaktır:

**8 (saat) x 8 (işlem için optimize edilmiş çekirdekler) x 0,193 $ = 12,35 TL**

## <a name="transform-data-in-blob-store-with-mapping-data-flows"></a>Veri akışlarını eşlemeyle blob deposunda verileri dönüştürme

Bu senaryoda, Blob Store'daki verileri Saatlik zamanlamayla ADF eşleme veri akışlarında görsel olarak dönüştürmek istiyorsunuz.

Senaryoyu gerçekleştirmek için aşağıdaki öğelerden bir ardışık kaynak oluşturmanız gerekir:

1. Dönüşüm mantığıile veri akışı etkinliği.

2. Azure Depolama'daki veriler için giriş veri kümesi.

3. Azure Depolama'daki veriler için bir çıktı veri kümesi.

4. Boru hattını her saat çalıştıracak bir zamanlama tetikleyicisi.

| **İşlemler** | **Türleri ve Birimleri** |
| --- | --- |
| Bağlantılı Hizmet Oluştur | 2 Oku/Yaz varlığı  |
| Veri Kümeleri Oluşturma | 4 Okuma/Yazma varlıkları (veri kümesi oluşturma için 2, bağlantılı hizmet başvuruları için 2) |
| Boru Hattı Oluştur | 3 Okuma/Yazma varlıkları (1'i boru hattı oluşturma, 2'si veri kümesi başvuruları için) |
| Boru Hattı alın | 1 Oku/Yaz varlığı |
| Çalıştır Boru Hattı | 2 Etkinlik çalışır (tetikleme çalışması için 1, etkinlik çalıştırmaları için 1) |
| Veri Akışı Varsayımları: yürütme süresi = 10 dk + 10 dk TTL | 10 \* TTL ile Genel Hesaplama 10 16 çekirdek |
| DenetimİzE VarAKSI DenetimI Denetimi Varsayımı: Yalnızca 1 çalıştırma oluştu | 2 İzleme çalıştırma kayıtları yeniden denenmiş (1 ardışık hat lar için, 1 etkinlik çalışması için) |

**Toplam Senaryo fiyatlandırması: $1.4631**

- Veri Fabrikası İşlemleri = **$0.0001**
  - Okuma/Yazma =\*10 00001 = $0.0001 [1 R/W = $0.50/50000 = 0.00001]
  - İzleme =\*2 000005 = $0.00001 [1 İzleme = $0.25/50000 = 0,000005]
- Boru Hattı &amp; Orkestrasyon Yürütme = **1.463 $**
  - Etkinlik Çalışır =\*001 2 = 0.002 [1 çalıştırın = $1/1000 = 0.001]
  - Veri Akışı Etkinlikleri = $1.461 20 dakika (10 dakika yürütme süresi + 10 dakika TTL) için eşit olarak verilir. 16 çekirdekli genel işlemle Azure Tümleştirme Çalışma Süresi'nde $0.274/saat

## <a name="next-steps"></a>Sonraki adımlar

Azure Veri Fabrikası'nın fiyatlandırmasını anladığınıziçin, işe başlayın!

- [Azure Data Factory UI kullanarak veri fabrikası oluşturma](quickstart-create-data-factory-portal.md)

- [Azure Data Factory'ye giriş](introduction.md)

- [Azure Veri Fabrikası'nda görsel yazma](author-visually.md)
