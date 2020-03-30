---
title: Azure FarmBeats Mimarisi
description: Azure FarmBeats mimarisini açıklar
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: d64c2175072d9979cfda2ea5f75beb34d3ad0d6b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75482380"
---
# <a name="integration-patterns"></a>Tümleştirme desenleri

Azure FarmBeats, Azure Marketi'nde kullanılabilen bir işletmeden işletmeye bir tekliftir. FarmBeats, veri kümelerini eriterek Yapay Zeka (AI) veya Machine Learning (ML) modellerini oluşturarak, sağlayıcılar arasında tarım veri kümelerinin toplanmasını ve işlem uygulanabilir öngörüler oluşturulmasını sağlar.

![Proje Çiftlik Beats](./media/architecture-for-farmbeats/farmbeats-architecture-1.png)

Aşağıdaki bölümlerde Azure FarmBeats için tümleştirme deseni açıklanmaktadır.

## <a name="why-integrate-with-azure-farmbeats"></a>Neden Azure FarmBeats ile entegre?

Bu bölüm, veri sistemlerini (sensörler, drone'lar, hava istasyonları gibi) Azure FarmBeats'e entegre etmek isteyen iş ortaklarına odaklanmaktadır.

Azure FarmBeats, tarım işletmelerinin farklı tarihsel ve gerçek zamanlı tarımsal veri kümelerini tek bir platforma eklemelerine olanak tanıyan genişletilebilir bir tekliftir. Azure FarmBeats, bir tarım işletmesinin verilerini bir çiftlik bağlamında normalleştirmesine, bağlamsallaştırmasına ve toplamasına yardımcı olur.

Azure FarmBeats ile veri ortağı olarak, sistemlerinizi daha geniş bir benimsemeye açabilir ve veri teklifleriile daha fazla müşteriye ulaşabilirsiniz. Azure FarmBeats, aygıtlarınızdan sistematik olarak ve standart bir şemaya veri almanıza yardımcı olan Datahub adı verilen genişletilebilir bir API katmanı sağlar.

Veriler müşterilerinizin Azure FarmBeats örneğinde kullanılabilir olduğunda, müşterileriniz verilerinizin üzerine daha zengin analizler ve araçlar oluşturabilir.

## <a name="next-steps"></a>Sonraki adımlar

Sensör veri tümleştirmesi hakkında daha fazla bilgi için sensör [veri tümleştirmesi](sensor-partner-integration-in-azure-farmbeats.md) ve görüntü ortağı tümleştirmesi için [görüntü ortağı tümleştirmesi](imagery-partner-integration-in-azure-farmbeats.md)için bkz.
