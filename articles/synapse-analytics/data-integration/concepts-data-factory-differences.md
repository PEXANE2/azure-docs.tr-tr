---
title: Azure Data Factory ile arasındaki farklar
description: Azure SYNAPSE Analytics 'in veri tümleştirme yetilerinin Azure Data Factory birbirinden farklı olduğunu öğrenin
services: synapse-analytics
author: kromerm
ms.service: synapse-analytics
ms.topic: conceptual
ms.date: 12/10/2020
ms.author: makromer
ms.reviewer: jrasnick
ms.openlocfilehash: 8818d4db489cef8203ae515c18c61e215d577033
ms.sourcegitcommit: ea17e3a6219f0f01330cf7610e54f033a394b459
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97387624"
---
# <a name="data-integration-in-azure-synapse-analytics-versus-azure-data-factory"></a>Azure SYNAPSE Analytics 'te veri tümleştirmesi ve Azure Data Factory karşılaştırması

Azure SYNAPSE Analytics 'te, SYNAPSE işlem hatları ve veri akışları gibi veri tümleştirme özellikleri Azure Data Factory birine göre belirlenir. Daha fazla bilgi için bkz. [Azure Data Factory nedir?](../../data-factory/introduction.md).


## <a name="available-features-in-adf--azure-synapse-analytics"></a>ADF & Azure SYNAPSE Analytics 'teki kullanılabilir özellikler

Özelliklerin kullanılabilirliği için aşağıdaki tabloyu denetleyin:

| Kategori                 | Özellik    |  Azure Data Factory  | Azure Synapse Analytics |
| ------------------------ | ---------- | :------------------: | :---------------------: |
| **Integration Runtime**  | SSIS ve SSIS Integration Runtime kullanma | ✓ | ✗ |
|                          | Bölgeler arası Integration Runtime (veri akışları) için destek | ✓ | ✗ |
|                          | Integration Runtime paylaşma | ✓<br><small>*Farklı veri fabrikaları arasında paylaşılabilir* | ✗ |
|                          | Yaşam süresi | ✓ | ✗ |
| **İşlem hatları etkinlikleri** | SSIS paketi etkinliği | ✓ | ✗ |
|                          | Power Query etkinlik desteği | ✓ | ✓ |
| **Şablon Galerisi ve bilgi merkezi** | Çözüm Şablonları | ✓<br><small>*Azure Data Factory Şablonu Galerisi* | ✓<br><small>*SYNAPSE çalışma alanı bilgi merkezi* |
| **GIT deposu tümleştirmesi** | GIT tümleştirmesi | ✓ | ✓ |
| **İzleme**           | Veri akışı için Spark Işlerinin izlenmesi | ✗ | ✓<br><small>*SYNAPSE Spark havuzlarından yararlanın* |
|                          | Azure Izleyici ile tümleştirme | ✓ | ✗ |

> [!Note]
> **Yaşam süresi** , Spark kümesinin bir veri akışı yürütmeden sonra bir süre için *normal kalmasını* sağlayan bir Azure Integration Runtime ayarıdır.
>


## <a name="next-steps"></a>Sonraki adımlar

[Azure Data Lake Storage Gen2 hesabına veri](data-integration-data-lake.md)almayı öğrenerek SYNAPSE çalışma alanınızdaki veri tümleştirmesini kullanmaya başlayın.
