---
title: Eşleme veri akışı oluşturma
description: Azure Data Factory eşleme veri akışı oluşturma
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 02/12/2019
ms.openlocfilehash: 2eb455ba6fa40538bfa03018be47232066036c23
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2019
ms.locfileid: "74930419"
---
# <a name="create-azure-data-factory-data-flow"></a>Azure Data Factory veri akışı oluşturma



ADF 'de veri akışlarını eşleme, hiçbir kodlama gerekmeden verileri ölçeklendirmeye yönelik bir yol sağlar. Bir dizi dönüştürme oluşturarak veri akışı tasarımcısında bir veri dönüştürme işi tasarlayabilirsiniz. Herhangi bir sayıdaki kaynak dönüşümle başlayın ve ardından veri dönüştürme adımları. Daha sonra, sonuçlarınızı bir hedefe aktarmak için havuz ile veri akışınızı doldurun.

İlk olarak Azure portal yeni bir v2 Data Factory oluşturmaya başlayın. Yeni fabrikanızı oluşturduktan sonra, Data Factory Kullanıcı arabirimini başlatmak için "yazar & Izleyicisi" kutucuğuna tıklayın.

![Veri akışı seçenekleri](media/data-flow/v2portal.png "veri akışı oluşturma")

Data Factory kullanıcı arabiriminden olduktan sonra örnek veri akışlarını kullanabilirsiniz. Bu örnekler ADF Şablon Galerisi ' nden kullanılabilir. ADF 'de "şablondan işlem hattı" oluşturun ve şablon galerisinden veri akışı kategorisini seçin.

![Veri akışı seçenekleri](media/data-flow/template.png "veri akışı oluşturma")

Azure Blob depolama hesabı bilgilerinizi girmeniz istenir.

![Veri akışı seçenekleri](media/data-flow/template2.png "veri akışı oluşturma 2")

[Bu örnekler için kullanılan veriler burada bulunabilir](https://github.com/kromerm/adfdataflowdocs/tree/master/sampledata). Örnekleri yürütebilmeniz için örnek verileri indirin ve dosyaları Azure Blob depolama hesaplarınıza depolayın.

## <a name="create-new-data-flow"></a>Yeni veri akışı oluştur

Veri akışları oluşturmak için ADF Kullanıcı arabirimindeki kaynak oluştur "artı işareti" düğmesini kullanın.

![Veri akışı seçenekleri](media/data-flow/newresource.png "Yeni kaynak")

## <a name="next-steps"></a>Sonraki adımlar

Veri dönüşümünüzü [kaynak dönüşümle](data-flow-source.md)oluşturmaya başlayın.
