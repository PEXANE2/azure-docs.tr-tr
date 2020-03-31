---
title: Azure Monitör çalışma kitapları veri kaynakları | Microsoft dokümanlar
description: Birden çok veri kaynağından oluşturulmuş önceden oluşturulmuş ve özel parametrelendirilmiş Azure Monitor Çalışma Kitapları yla karmaşık raporlamayı basitleştirin
services: azure-monitor
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: mbullwin
ms.openlocfilehash: d57910ae31d4db9be17b3dc46b5920a925ab4fcf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79248586"
---
# <a name="azure-monitor-workbooks-data-sources"></a>Azure Monitör çalışma kitapları veri kaynakları

Çalışma kitapları çok sayıda veri kaynağıyla uyumludur. Bu makale, şu anda Azure Monitor çalışma kitapları için kullanılabilen veri kaynaklarında size yol açacaktır.

## <a name="logs"></a>Günlükler

Çalışma kitapları aşağıdaki kaynaklardan günlükleri sorgulamaya izin verir:

* Azure Monitör Günlükleri (Uygulama Öngörüleri Kaynakları ve Günlük Analizi Çalışma Alanları.)
* Kaynak merkezli veriler (Etkinlik günlükleri)

Çalışma kitabı yazarları, metin, grafikler veya ızgaralar olarak görselleştirilebilen bir sonuç kümesi seçmek için temel kaynak verilerini dönüştüren KQL sorgularını kullanabilir.

![Çalışma kitaplarının ekran görüntüsü günlükleri rapor arabirimi](./media/workbooks-overview/logs.png)

Çalışma kitabı yazarları, gerçekten birleşik zengin raporlama deneyimi oluşturarak birden çok kaynak arasında kolayca sorgulayabilir.

## <a name="metrics"></a>Ölçümler

Azure kaynakları, çalışma kitapları aracılığıyla erişilebilen [ölçümler](data-platform-metrics.md) yontabilir. Ölçümlere, hedef kaynakları, istenen ölçümleri ve bunların toplarını belirtmenize olanak tanıyan özel bir denetim aracılığıyla çalışma kitaplarında erişilebilir. Bu veriler daha sonra grafiklerde veya ızgaralarda çizilebilir.

![Çalışma kitabı ölçüm lerinin cpu kullanımı grafiklerinin ekran görüntüsü](./media/workbooks-overview/metrics-graph.png)

![Çalışma kitabı ölçümleri arabiriminin ekran görüntüsü](./media/workbooks-overview/metrics.png)

## <a name="azure-resource-graph"></a>Azure Kaynak Grafiği 

Çalışma kitapları, Azure Kaynak Grafiği 'ni (ARG) kullanarak kaynakları ve meta verilerini sorgulamayı destekler. Bu işlev, öncelikle raporlar için özel sorgu kapsamları oluşturmak için kullanılır. Kaynak kapsamı, ARG'nin desteklediği bir KQL alt kümesi aracılığıyla ifade edilir ve bu genellikle yaygın kullanım durumları için yeterlidir.

Sorgu denetiminin bu veri kaynağını kullanmasını sağlamak için, Azure Kaynak Grafiği'ni seçmek ve hedeflemek için abonelikleri seçmek için Sorgu türü açılır dosyasını kullanın. İlginç bir kaynak alt kümesi ni seçen ARG KQL alt kümesini eklemek için Sorgu denetimini kullanın.


![Azure Kaynak Grafiği KQL sorgusunun ekran görüntüsü](./media/workbooks-overview/azure-resource-graph.png)

## <a name="alerts-preview"></a>Uyarılar (önizleme)

Çalışma kitapları, kullanıcıların kaynaklarıyla ilgili etkin uyarıları görselleştirmelerine olanak sağlar. Bu özellik, bildirim verilerini (uyarı) ve tanılama bilgilerini (ölçümler, günlükler) tek bir rapor da bir araya getiren raporların oluşturulmasına olanak tanır. Bu bilgiler, bu veri kaynakları arasında öngörüleri birleştiren zengin raporlar oluşturmak için de biraraya getirilebilir.

Sorgu denetiminin bu veri kaynağını kullanmasını sağlamak için, Uyarıları seçmek ve hedeflemek için abonelikleri, kaynak gruplarını veya kaynakları seçmek için Sorgu türü açılır dosyasını kullanın. Analitik gereksinimleriniz için ilginç bir uyarı alt kümesini seçmek için uyarı filtresi açılır düşüşlerini kullanın.

![Uyarılar sorgusunun ekran görüntüsü](./media/workbooks-overview/alerts.png)

## <a name="workload-health-preview"></a>İş yükü durumu (önizleme)

Azure Monitor, Windows veya Linux konuk işletim sistemlerinin kullanılabilirliğini ve performansını proaktif olarak izleyen işlevsellik lere sahiptir. Azure Monitor, önemli bileşenleri ve ilişkilerini, bu bileşenlerin sistem durumunu nasıl ölçeceklerine ilişkin ölçütleri ve sağlıksız bir durum algılandığında hangi bileşenlerin sizi uyarmasını sağlar. Çalışma kitapları, kullanıcıların zengin etkileşimli raporlar oluşturmak için bu bilgileri kullanmasına olanak sağlar.

Sorgu denetimi yapmak için bu veri kaynağını kullanın, İş Yükü Durumu'nu seçmek ve hedeflemek için abonelik, kaynak grubu veya VM kaynaklarını seçmek için **Sorgu türü** açılır dosyasını kullanın. Analitik gereksinimleriniz için sağlık olaylarının ilginç bir alt kümesini seçmek için sistem durumu filtresi açılır düşüşlerini kullanın.

![Uyarılar sorgusunun ekran görüntüsü](./media/workbooks-overview/workload-health.png)

## <a name="azure-resource-health"></a>Azure kaynak durumu 

Çalışma kitapları, zengin, etkileşimli sistem durumu raporları oluşturmak için Azure kaynak sağlığının elde etmesini ve diğer veri kaynaklarıyla birleştirilmesini destekleme

Sorgu denetiminin bu veri kaynağını kullanmasını sağlamak için, Azure sistem durumunu seçmek ve hedeflenene kaynakları seçmek için **Sorgu türü** açılır dosyasını kullanın. Analitik gereksinimleriniz için ilginç bir kaynak sorunları alt kümesini seçmek için sistem durumu filtresi açılır düşüşlerini kullanın.

![Uyarılar sorgusunun ekran görüntüsü](./media/workbooks-overview/resource-health.png)

## <a name="azure-data-explorer-preview"></a>Azure Veri Gezgini (önizleme)

Çalışma kitapları artık güçlü [Kusto](https://docs.microsoft.com/azure/kusto/query/index) sorgu diliyle [Azure Veri Gezgini](https://docs.microsoft.com/azure/data-explorer/) kümelerinden sorgulama desteğine sahiptir.   

![Kusto sorgu penceresinin ekran görüntüsü](./media/workbooks-overview/data-explorer.png)

## <a name="next-steps"></a>Sonraki adımlar

* Çalışma kitapları hakkında daha fazla bilgi [edinmeye başlayın](workbooks-visualizations.md) birçok zengin görselleştirme seçeneği.
* Çalışma kitabı kaynaklarınıza erişimi [kontrol](workbooks-access-control.md) edin ve paylaşın.
