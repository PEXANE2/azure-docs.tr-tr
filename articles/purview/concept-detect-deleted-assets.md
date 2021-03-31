---
title: Taramalar silinen varlıkları nasıl algılar?
description: Bu makalede, bir Azure purview hesabının taramalar sırasında silinen varlıkları nasıl algıladığı açıklanmaktadır.
author: yaronyg
ms.author: yarong
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: conceptual
ms.date: 10/16/2020
ms.openlocfilehash: 9b1515ef00355c831e161c01227678197792cc20
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "96555560"
---
# <a name="how-scans-detect-deleted-assets"></a>Taramalar silinen varlıkları nasıl algılar?

Bu makalede, Azure purview 'ın silinen varlıkları algılamak için tarama sonuçlarını nasıl kullandığı açıklanır.

## <a name="background-info"></a>Arka plan bilgileri

Bir Azure purview kataloğu, bir veri deposunun durumunu yalnızca taradığında algılar. Kataloğun bir dosya, tablo veya kapsayıcının silinip silinmediğini bilmesini sağlamak için, son tarama çıkışını geçerli tarama çıktısına göre karşılaştırır. Örneğin, Azure Data Lake Storage 2. bir hesabı son kez taradığınızı varsayalım, *Klasör1* adlı bir klasör dahil edilmiştir. Aynı hesap yeniden tarandıktan sonra, *Klasör1* eksik olur. Bu nedenle, Katalog klasörün silindiğini varsayar.

## <a name="detecting-deleted-files"></a>Silinen dosyalar algılanıyor

Eksik dosyaları tespit etme mantığı aynı kullanıcı tarafından ve farklı kullanıcılara göre birden fazla tarama için de geçerlidir. Örneğin, bir kullanıcının A, B ve C klasörlerinde bir Data Lake Storage 2. veri deposunda tek seferlik bir tarama çalıştığını varsayalım. Daha sonra, aynı hesaptaki farklı bir kullanıcı aynı veri deposunun C, D ve E klasörlerinde farklı bir kerelik tarama çalıştırır. C klasörü iki kez tarandığı için, Katalog bunu olası silme işlemleri için denetler. Ancak A, B, D ve E klasörleri yalnızca bir kez tarandı ve Katalog, Silinen varlıklar için bunları denetlemez.

Silinen dosyaları kataloğunuzda tutmak için düzenli taramalar çalıştırmak önemlidir. Tarama aralığı, başka bir tarama çalıştırılıncaya kadar, Katalog silinen varlıkları algılayamayacağı için önemlidir. Bu nedenle, belirli bir depoda her ay bir kez taramalar çalıştırırsanız, bir sonraki taramayı daha sonra çalıştırana kadar Katalog bu depodaki silinen veri varlıklarını algılayamaz.

Data Lake Storage 2. gibi büyük veri depolarını Numaralandırdığınızda, bilgileri kaçırmak için birden çok yol (numaralandırma hataları ve bırakılan olaylar dahil) vardır. Belirli bir tarama, bir dosyanın oluşturulduğunu veya silindiğini kaçırabilir. Bu nedenle, Katalog belirli bir dosyanın silindiği takdirde katalogdan silinmez. Bu strateji, taranmış veri deposunda mevcut olmayan bir dosya katalogda hala mevcut olduğunda hatalar olabileceği anlamına gelir. Bazı durumlarda, bir veri deposunun belirli silinen varlıkları yakalamasını sağlamak için iki veya üç kez taranması gerekebilir.

## <a name="next-steps"></a>Sonraki adımlar

Azure purview kataloglarıyla çalışmaya başlamak için bkz. [hızlı başlangıç: purview hesabı oluşturma](create-catalog-portal.md).
