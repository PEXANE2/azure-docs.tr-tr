---
title: Apache Hadoop bileşenleri ve sürümleri-Azure HDInsight
description: Azure HDInsight 'ta Apache Hadoop bileşenleri ve sürümleri hakkında bilgi edinin.
ms.service: hdinsight
ms.topic: conceptual
ms.custom: deshriva
ms.date: 02/08/2021
ms.openlocfilehash: 166c52660c9f47b8ecfce10f7e9c3b81ff067f35
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101699377"
---
# <a name="azure-hdinsight-versions"></a>Azure HDInsight sürümleri

HDInsight, ortam bileşenlerini ve HDInsight platformunu bir kümeye dağıtılan bir pakete Apache Hadoop. Daha fazla ayrıntı için bkz. [HDInsight sürümü oluşturma nasıl çalıştığını](hdinsight-overview-versioning.md)öğrenin.

## <a name="supported-hdinsight-versions"></a>Desteklenen HDInsight sürümleri

Bu tabloda, Azure portal ve PowerShell, CLı ve .NET SDK gibi diğer dağıtım yöntemlerinde bulunan HDInsight sürümleri listelenir.

| HDInsight sürümü | VM İŞLETİM SİSTEMİ | Sürüm tarihi | Destek sona erme tarihi | Emeklilik tarihi | Yüksek kullanılabilirlik |
| --- | --- | --- | --- | --- | --- |
| [HDInsight 4.0](hdinsight-40-component-versioning.md) |Ubuntu 16.0.4 LTS |Eylül 24, 2018 | | |Yes |
| [HDInsight 3,6](hdinsight-36-component-versioning.md) |Ubuntu 16.0.4 LTS |4 Nisan 2017      | * 30 Haziran 2021 |30 Haziran 2021 |Yes |

* Belirli HDInsight 3,6 küme türleri için destek zaman çerçevesini genişlettik. Bkz. [hdınsight 3,6 bileşen sürümleri](hdinsight-36-component-versioning.md).

## <a name="release-notes"></a>Sürüm notları

HDInsight 'ın en son sürümlerinde ek sürüm notları için bkz. [HDInsight sürüm notları](hdinsight-release-notes.md).

## <a name="support-options-for-hdinsight-versions"></a>HDInsight sürümleri için destek seçenekleri

HDInsight, HDInsight sürümünün Microsoft Müşteri Hizmetleri ve desteği tarafından desteklendiği bir zaman dilimi olarak tanımlanan standart destek sunmaktadır.

**Destek sona erme tarihi** , Microsoft 'un belirli HDInsight sürümü için destek sunmayacağı anlamına gelir. Artık küme oluşturma için Azure portal aracılığıyla kullanılamaz.

**Kullanımdan** kaldırma, HDInsight sürümünün mevcut kümelerinin olduğu gibi çalışmaya devam etmesi anlamına gelir. Bu sürümdeki yeni kümeler CLı ve SDK 'Ları içeren herhangi bir yöntemle oluşturulamaz. El ile ölçekleme ve otomatik ölçeklendirme gibi diğer denetim düzlemi özelliklerinin, kullanımdan kaldırma tarihinden sonra çalışmasına garanti edilmez. Kullanımdan kaldırılan sürümler için destek kullanılamaz.

## <a name="versioning-considerations"></a>Sürüm oluşturma konuları
- Bir küme bir görüntüyle dağıtıldıktan sonra, bu küme otomatik olarak yeni görüntü sürümüne yükseltilmez. Yeni kümeler oluştururken en son görüntü sürümü dağıtılır.
- Müşteriler, yeni HDInsight sürümü kullanılırken uygulamaların düzgün şekilde çalışacağını test etmelidir ve doğrular.
- HDInsight, önceki bildirimde bulunmadan varsayılan sürümü değiştirme hakkını saklı tutar. Bir sürüm bağımlılığıdır varsa, kümelerinizi oluştururken HDInsight sürümünü belirtin.
- HDInsight, HDInsight sürümünü devre dışı bırakmadan önce bir OSS bileşen sürümünü devre dışı bırakabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [HDInsight üzerinde Apache Hadoop, Spark ve daha fazlası için küme kurulumu](hdinsight-hadoop-provision-linux-clusters.md)
- [Kurumsal Güvenlik Paketi](./enterprise-security-package.md)
- [Windows BILGISAYARDAN HDInsight üzerinde Apache Hadoop çalışma](hdinsight-hadoop-windows-tools.md)