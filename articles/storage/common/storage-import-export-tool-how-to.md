---
title: Azure Içeri/dışarı aktarma aracı 'nı kullanma | Microsoft Docs
description: İçeri aktarma/dışarı aktarma aracını kullanarak bir içeri aktarma işi için sabit sürücüler hazırlama, bir içeri aktarma işini onarma veya bir dışarı aktarma işini onarma hakkında bilgi edinin.
author: twooley
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/15/2017
ms.author: twooley
ms.subservice: common
ms.openlocfilehash: 35236a95e691d6f7d0444a038e62116a684965a7
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "75978457"
---
# <a name="using-the-azure-importexport-tool"></a>Azure Içeri/dışarı aktarma aracı 'nı kullanma

Azure içeri/dışarı aktarma aracı (WAImportExport.exe), Azure Içeri/dışarı aktarma hizmeti için iş oluşturmak ve yönetmek üzere kullanılır. böylece Azure Blob depolama alanına veya dışına büyük miktarlarda veri aktarabilirsiniz.

Bu belge, Azure Içeri/dışarı aktarma aracı 'nın en son sürümüne yöneliktir. Klasik dağıtım modelini kullanma hakkında daha fazla bilgi için bkz. [Azure içeri/dışarı aktarma aracı v1 kullanma](storage-import-export-tool-how-to-v1.md).

Aşağıdaki makalelerde nasıl yapılacağı gösterilmektedir:  

- Azure Içeri/dışarı aktarma aracı 'nı yükleyip kurun.
- Sürücülerinizdeki verileri Azure Blob depolama alanına aktardığınız bir iş için sabit sürücülerinizi hazırlayın.
- Günlük dosyalarını Kopyala ile bir işin durumunu gözden geçirin.
- Bir içeri aktarma işini onarın.
- Bir dışarı aktarma işini onarın.
- Azure Içeri/dışarı aktarma aracı sorunlarını giderin.

## <a name="next-steps"></a>Sonraki adımlar

* [Waımportexport aracını ayarlama](storage-import-export-tool-setup.md)
