---
title: Azure HDInsight desteklenen düğüm yapılandırmaları
description: HDInsight küme düğümleri için minimum ve önerilen yapılandırmaları öğrenin.
keywords: vm boyutları, küme boyutları, küme yapılandırması
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 08/26/2019
ms.openlocfilehash: e75146266568001d8fee7be26898ac8bdfffb7fc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77484795"
---
# <a name="what-are-the-default-and-recommended-node-configurations-for-azure-hdinsight"></a>Azure HDInsight için varsayılan ve önerilen düğüm yapılandırmaları nelerdir?

Bu makalede, Azure HDInsight kümeleri için varsayılan ve önerilen düğüm yapılandırmaları açıklanmaktadır.

## <a name="default-and-minimum-recommended-node-configuration-and-virtual-machine-sizes-for-clusters"></a>Kümeler için varsayılan ve minimum önerilen düğüm yapılandırması ve sanal makine boyutları

Aşağıdaki tablolar varsayılan liste ve HDInsight kümeleri için önerilen sanal makine (VM) boyutları.  Bu bilgiler, HDInsight kümelerini dağıtmak için PowerShell veya Azure CLI komut dosyaları oluştururken kullanılacak VM boyutlarını anlamak için gereklidir.

Bir kümede 32'den fazla alt düğüme ihtiyacınız varsa, en az 8 çekirdek ve 14 GB RAM içeren bir baş düğümü boyutu seçin. 

Veri diskleri olan tek küme türleri, Hızlandırılmış Yazma özelliği etkinleştirilmiş Kafka ve HBase kümeleridir. HDInsight bu senaryolarda P30 ve S30 disk boyutlarını destekler.

Bu belgede kullanılan önerilen tüm minimum VM türlerinin özellikleri aşağıdaki tabloda özetlenmiştir.

| Boyut              | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum geçici depolama aktarım hızı: IOPS / Okuma MB/sn / Yazma MB/sn | Maksimum veri diski/aktarım hızı: IOPS | Max NIC ' ler / Beklenen ağ bant genişliği (Mbps) |
|-------------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D3_v2 | 4    | 14          | 200                    | 12000/187/93                                           | 16 / 16x500           | 4 / 3000                                       |
| Standard_D4_v2 | 8    | 28          | 400                    | 24000/375/187                                          | 32 / 32x500           | 8 / 6000                                       |
| Standard_D5_v2 | 16   | 56          | 800                    | 48000/750/375                                          | 64 / 64x500           | 8 / 12000                                    |
| Standard_D12_v2   | 4         | 28          | 200            | 12000/187/93                                         | 16/16x500                         | 4 / 3000                     |
| Standard_D13_v2   | 8         | 56          | 400            | 24000/375/187                                        | 32/32x500                       | 8 / 6000                     |
| Standard_D14_v2   | 16        | 112         | 800            | 48000/750/375                                        | 64 / 64x500                       | 8 / 12000          |
| Standard_A1_v2  | 1         | 2           | 10             | 1000/20/10                                           | 2/2x500               | 2 / 250                 |
| Standard_A2_v2  | 2         | 4           | 20             | 2000/40/20                                           | 4/4x500               | 2 / 500                 |
| Standard_A4_v2  | 4         | 8           | 40             | 4000/80/40                                           | 8/8x500               | 4 / 1000                     |

Her VM türünün özellikleri hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

* [Genel amaçlı sanal makine boyutları: Dv2 serisi 1-5](../virtual-machines/dv2-dsv2-series.md)
* [Bellek optimize edilmiş sanal makine boyutları: Dv2 serisi 11-15](../virtual-machines/dv2-dsv2-series-memory.md)
* [Genel amaçlı sanal makine boyutları: Av2 serisi 1-8](../virtual-machines/av2-series.md)

### <a name="all-supported-regions-except-brazil-south-and-japan-west"></a>Brezilya güney ve Japonya batı hariç tüm desteklenen bölgeler

> [!Note]
> Powershell ve diğer komut dosyalarında kullanılmak üzere SKU tanımlayıcısını `Standard_` almak için aşağıdaki tablolarda vm SKU'ların tümünün başlangıcına ekleyin. Örneğin, `D12_v2` olur `Standard_D12_v2`.

| Küme türü | Hadoop | HBase | Interactive Query | Storm | Spark | ML Sunucu | Kafka |
|---|---|---|---|---|---|---|---|
| Kafa: varsayılan VM boyutu | D12_v2 | D12_v2 | D13_v2 | A4_v2 | D12_v2, <br/>D13_v2* | D12_v2 | D3_v2 |
| Kafa: önerilen minimum VM boyutları | D5_v2 | D3_v2 | D13_v2 | A4_v2 | D12_v2, <br/>D13_v2* | D12_v2 | D3_v2 |
| İşçi: varsayılan VM boyutu | D4_v2 | D4_v2 | D14_v2 | D3_v2 | D13_v2 | D4_v2 | Broker başına 2 S30 diskile 4 D12_v2 |
| İşçi: önerilen minimum VM boyutları | D5_v2 | D3_v2 | D13_v2 | D3_v2 | D12_v2 | D4_v2 | D3_v2 |
| ZooKeeper: varsayılan VM boyutu |  | A4_v2 | A4_v2 | A4_v2 |  | A2_v2 | A4_v2 |
| ZooKeeper: minimum önerilen VM boyutları |  | A4_v2 | A4_v2 | A2_v2 |  | A2_v2 | A4_v2 |
| ML Hizmetleri: varsayılan VM boyutu |  |  |  |  |  | D4_v2 |  |
| ML Hizmetleri: önerilen minimum VM boyutu |  |  |  |  |  | D4_v2 |  |

\*= Spark Kurumsal Güvenlik Paketi (ESP) kümeleri için VM Boyutları

### <a name="brazil-south-and-japan-west-only"></a>Brezilya güney ve Japonya batı sadece

| Küme türü | Hadoop | HBase | Interactive Query | Storm | Spark | ML Services |
|---|---|---|---|---|---|---|
| Kafa: varsayılan VM boyutu | D12 | D12 | D13 | A4_v2 | D12 | D12 |
| Kafa: önerilen minimum VM boyutları | D5_v2 | D3_v2 | D13_v2 | A4_v2 | D12_v2 | D12_v2 |
| İşçi: varsayılan VM boyutu | D4 | D4 | D14 | D3 | D13 | D4 |
| İşçi: önerilen minimum VM boyutları | D5_v2 | D3_v2 | D13_v2 | D3_v2 | D12_v2 | D4_v2 |
| ZooKeeper: varsayılan VM boyutu |  | A4_v2 | A4_v2 | A4_v2 |  | A2_v2 |
| ZooKeeper: minimum önerilen VM boyutları |  | A4_v2 | A4_v2 | A4_v2 |  | A2_v2 |
| ML Hizmetleri: varsayılan VM boyutları |  |  |  |  |  | D4 |
| ML Hizmetleri: önerilen minimum VM boyutları |  |  |  |  |  | D4_v2 |

> [!NOTE]
> - Baş, Fırtına kümesi türü için *Nimbus* olarak bilinir.
> - İşçi, Fırtına kümesi türü için *DenetimAmiri* olarak bilinir.
> - İşçi, HBase küme türü için *Bölge* olarak bilinir.

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight ile kullanılabilen Apache Hadoop bileşenleri ve sürümleri nelerdir?](hdinsight-component-versioning.md)
