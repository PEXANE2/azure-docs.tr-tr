---
title: Eşleme veri akışı oluşturma
description: Azure Veri Fabrikası eşleme veri akışı oluşturma
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 02/12/2019
ms.openlocfilehash: cd1342ae5912a9a91f1bd8d1bdacecdb5a2bbdb0
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81416522"
---
# <a name="create-azure-data-factory-data-flow"></a>Azure Data Factory Veri Akışı Oluşturma

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

ADF'deki Veri Akışlarını eşleme, herhangi bir kodlama gerektirmeden verileri ölçekte dönüştürmenin bir yolunu sağlar. Bir dizi dönüşüm oluşturarak veri akışı tasarımcısında bir veri dönüştürme işi tasarlayabilirsiniz. Veri dönüştürme adımlarını izleyen herhangi bir sayıda kaynak dönüşümüyle başlayın. Ardından, sonuçlarınızı bir hedefe çıkarmak için veri akışınızı lavaboile tamamlayın.

Azure portalından yeni bir V2 Veri Fabrikası oluşturarak başlayın. Yeni fabrikanızı oluşturduktan sonra, Veri Fabrikası Kullanıcı Çağrı'sını başlatmak için "Yazar & Monitörü" döşemesine tıklayın.

![Veri Akışı seçenekleri](media/data-flow/v2portal.png "veri akışı oluşturmak")

Veri Fabrikası UI'ye ulaştıktan sonra örnek Veri Akışları'nı kullanabilirsiniz. Örnekler ADF Şablon Galerisi'nden edinilebilir. ADF'de "Şablondan Ardışık Kaynak" oluşturun ve şablon galerisinden Veri Akışı kategorisini seçin.

![Veri Akışı seçenekleri](media/data-flow/template.png "veri akışı oluşturmak")

Azure Blob Depolama hesap bilgilerinizi girmeniz istenir.

![Veri Akışı seçenekleri](media/data-flow/template2.png "veri akışı oluşturmak 2")

[Bu örnekler için kullanılan verileri burada bulabilirsiniz.](https://github.com/kromerm/adfdataflowdocs/tree/master/sampledata) Örnek verileri indirin ve örnekleri yürütebilmeniz için dosyaları Azure Blob depolama hesaplarınızda saklayın.

## <a name="create-new-data-flow"></a>Yeni veri akışı oluşturma

Veri Akışları oluşturmak için ADF UI'deki Kaynak Oluştur "artı işareti" düğmesini kullanın.

![Veri Akışı seçenekleri](media/data-flow/newresource.png "Yeni Kaynak")

## <a name="next-steps"></a>Sonraki adımlar

[Kaynak dönüşümü](data-flow-source.md)yle veri dönüşümünüzü oluşturmaya başlayın.
