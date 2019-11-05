---
title: Azure Data Factory 'de wrangling veri akışları | Microsoft Docs
description: Azure Data Factory 'de denetimi veri akışlarına genel bakış
author: djpmsft
ms.author: daperlov
ms.reviewer: gamal
ms.service: data-factory
ms.topic: conceptual
ms.date: 11/01/2019
ms.openlocfilehash: 05119adfa5b254297fd656854a027ede05c5ef26
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73518586"
---
# <a name="what-are-wrangling-data-flows"></a>Denetimi veri akışları nelerdir?

Kuruluşların her gün büyümeye devam eden karmaşık verilerin doğru analizi için veri hazırlama ve denetimi yapması gerekir. Kuruluşların verileri çeşitli iş süreçlerinde kullanabilmesi ve değerin süresini azaltabilmesi için veri hazırlama gereklidir.

Azure Data Factory ' deki wrangling veri akışları, bulut ölçeğinde kod içermeyen veri hazırlığı yapmanıza olanak sağlar. Wrangling veri akışları [Power Query çevrimiçi](https://docs.microsoft.com/power-query/) olarak tümleştirilir ve Data Factory kullanıcıları Için Power Query M işlevlerini kullanılabilir hale getirir.

Wrangling veri akışı, Power Query çevrimiçi karma düzenleyici tarafından oluşturulan M 'yi bulut ölçek yürütmesi için Spark koduna çevirir.

Wrangling veri akışları, özellikle veri mühendisleri veya ' vatandaşlık veri tümleştiricileri ' için faydalıdır.

## <a name="use-cases"></a>Uygulama alanları

### <a name="fast-interactive-data-exploration-and-preparation"></a>Hızlı etkileşimli veri araştırması ve hazırlığı

Birden çok veri mühendisi ve vatandaşlık veri tümleştiricileri, bulut ölçeğinde veri kümelerini etkileşimli bir şekilde araştırabilir ve hazırlayabilir. Verilerin yükseminde, veri kümelerindeki verilerin çeşitliliğini, çeşitli ve hızına sahip kullanıcılar, veri kümelerini keşfetmeye ve hazırlamaya yönelik etkili bir yönteme ihtiyaç duyar. Örneğin, ' 2017 ' den beri yeni müşteriler için tüm müşteri demoonları bilgilerine sahip bir veri kümesi oluşturmanız gerekebilir. Bilinen bir hedefle eşlememeniz yok. Gölü üzerinde yayımlamadan önce bir gereksinimi karşılamak için araştırırken, wrangling ve veri kümeleri önceden alınıyor. Wrangling veri akışları genellikle daha az resmi analiz senaryosu için kullanılır. Önceden yazılmış veri kümeleri, dönüşümler ve makine öğrenimi işlemleri için aşağı akış yapmak üzere kullanılabilir.

### <a name="code-free-agile-data-preparation"></a>Kod ücretsiz çevik veri hazırlama

Vatandaşlık veri tümleştiricileri, verileri arayan ve hazırlama zamanının %60 ' inden fazlasını harcıyordu. İşlem üretkenliğini artırmak için bunu bir kod ücretsiz olarak yapmak arıyorlar. Vatandaşlık veri tümleştiricilerinin, ölçeklenebilir bir şekilde Power Query çevrimiçi gibi bilinen araçları kullanarak verileri zenginleştirmesine ve yayımlamasına olanak tanımak, büyük ölçüde üretkenliğini artırır. Azure Data Factory 'de wrangling veri akışı, vatandaşlık veri tümleştiricilerinin hataları hızla düzeltmesine, verileri standartlaştırmasına ve iş kararlarını desteklemek için yüksek kaliteli veriler oluşturmasına izin vermek üzere tanıdık Power Query çevrimiçi karma düzenleyici 'yi sağlar.

### <a name="data-validation"></a>Veri doğrulama

Herhangi bir şeyi kaldırmak ve hızlı analizler için bir şekle uymak üzere verilerinizi kod ücretsiz olarak görsel olarak tarayın.

## <a name="next-steps"></a>Sonraki adımlar

[Bir denetimi veri akışı oluşturmayı](wrangling-data-flow-tutorial.md)öğrenin.