---
title: Azure veri paylaşımında desteklenen veri depoları
description: Azure veri paylaşımının kullanımı için desteklenen veri depoları hakkında bilgi edinin.
ms.service: data-share
author: joannapea
ms.author: joanpo
ms.topic: conceptual
ms.date: 10/30/2019
ms.openlocfilehash: 56103ed89d2e7813fd60bc50ecca7271f5421a4a
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75438681"
---
# <a name="supported-data-stores-in-azure-data-share"></a>Azure veri paylaşımında desteklenen veri depoları

Azure veri paylaşımı, ve farklı veri depolarından paylaşma özelliği de dahil olmak üzere açık ve esnek veri paylaşımı sağlar. Veri sağlayıcıları verileri bir veri deposu türünden paylaşabilir ve veri tüketicileri verileri hangi veri deposuna alacağını seçebilirler. 

Bu makalede, Azure veri paylaşımında desteklenen zengin Azure veri depoları kümesi hakkında bilgi edineceksiniz. Ayrıca, veri sağlayıcılarının ve veri tüketicilerinin yararlanılabilir olabilecek veri depolarının birleşimleri hakkında bilgi bulabilirsiniz. 

## <a name="what-data-stores-are-supported-in-azure-data-share"></a>Azure veri paylaşımında hangi veri depoları desteklenir? 

Aşağıdaki tabloda, Azure veri paylaşımında desteklenen veri kaynaklarının ayrıntıları verilmiştir. 

| Veri deposu | Anlık görüntü tabanlı paylaşım | Yerinde paylaşım 
|:--- |:--- |:--- |:--- |:--- |:--- |
| Azure Blob depolama |✓ | |
| Azure Data Lake Storage Gen1 |✓ | |
| Azure Data Lake Storage Gen2 |✓ ||
| Azure SQL Veritabanı |Genel önizlemeye sunuldu | |
| Azure SYNAPSE Analytics (eski adıyla Azure SQL DW) |Genel önizlemeye sunuldu | |
| Azure Veri Gezgini | |[Sınırlı Önizleme](https://aka.ms/azuredatasharepreviewsignup) |

## <a name="data-store-support-matrix"></a>Veri deposu destek matrisi

Azure veri paylaşımında, ' deki verileri kabul etmek için bir veri deposunda karar verirken veri tüketicileri esnekliği sunulmaktadır. Örneğin, Azure SQL veritabanı 'ndan paylaşılmakta olan veriler Azure Data Lake Store Gen2, Azure SQL veritabanı veya Azure SYNAPSE Analytics ' e alınabilir. Müşteriler, alınan bir veri paylaşımının yapılandırılmasında hangi biçimin veri alacağını seçebilirler. 

Aşağıdaki tabloda veri tüketicilerinin veri payını kabul edip yapılandırırken kullandığı farklı birleşimler ve seçimler ayrıntılı olarak verilmiştir. Veri kümesi eşlemelerini yapılandırma hakkında daha fazla bilgi için bkz. [veri kümesi eşlemelerini yapılandırma](how-to-configure-mapping.md).

|  | Azure Blob Depolama | Azure SQL Data Lake Gen1 | Azure SQL Data Lake Gen2 | Azure SQL Veritabanı | Azure Synapse Analytics 
|:--- |:--- |:--- |:--- |:--- |:--- |
| Azure Blob depolama |✓ ||✓|
| Azure Data Lake Storage Gen1 |✓ | |✓|
| Azure Data Lake Storage Gen2 |✓ | |✓|
| Azure SQL Veritabanı |✓ | |✓|✓|✓|
| Azure Synapse Analytics |✓ | |✓|✓|✓|

## <a name="next-steps"></a>Sonraki adımlar

Verileri paylaşmaya nasıl başlayacağınızı öğrenmek için [verilerinizi paylaşma](share-your-data.md) öğreticiye geçin.
