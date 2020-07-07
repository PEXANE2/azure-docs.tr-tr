---
title: Eşleme veri akışı oluşturma
description: Azure Data Factory eşleme veri akışı oluşturma
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 02/12/2019
ms.openlocfilehash: cd1342ae5912a9a91f1bd8d1bdacecdb5a2bbdb0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81416522"
---
# <a name="create-azure-data-factory-data-flow"></a>Azure Data Factory Veri Akışı Oluşturma

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

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
