---
title: "Öğretici: SYNAPSE bilgi merkezi 'Ni kullanmaya başlayın"
description: Bu öğreticide, SYNAPSE bilgi merkezini kullanmayı öğreneceksiniz.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: workspace
ms.topic: tutorial
ms.date: 11/16/2020
ms.openlocfilehash: 59364a0a2c1d1e411f41dc35dee812c8e920b72d
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "98209211"
---
# <a name="explore-the-synapse-knowledge-center"></a>SYNAPSE bilgi merkezini araştırma

Bu öğreticide, SYNAPSE Studio bilgi merkezini kullanmayı öğreneceksiniz.

## <a name="getting-to-the-knowledge-center"></a>Bilgi merkezine alma

SYNAPSE Studio 'da bilgi merkezini bulmanın iki yolu vardır:

  1. Ana Hub 'da, sayfanın sağ üst kısmındaki **öğrenme**' ye tıklayın.
  2. Üstteki menü çubuğunda, **?** ' e tıklayın. daha sonra **Bilgi Merkezi**.

Herhangi bir yöntemi seçin ve **bilgi merkezini** açın.

## <a name="overview"></a>Genel Bakış

**Bilgi Merkezi** , üç şey yapmanıza olanak sağlar:
* **Örnekleri hemen kullanın**. SYNAPSE 'in nasıl çalıştığına ilişkin hızlı bir örnek istiyorsanız bu seçeneği belirleyin.
* **Galeriye gözatamazsınız**. Bu seçenek, örnek veri kümelerini bağlamanızı ve SQL betikleri, Not defterleri ve işlem hatları biçiminde örnek kod eklemenizi sağlar.
* **Tur SYNAPSE Studio**. Bu seçenek sizi SYNAPSE Studio 'nun temel bölümlerinin kısa bir turuna götürür. Daha önce SYNAPSE Studio 'Yu kullanmadıysanız bu kullanışlıdır.

## <a name="exploring-blob-storage-with-serverless-sql-pool"></a>Sunucusuz SQL havuzuyla blob depolamayı keşfetme

1. **Bilgi merkezine** gidin, **örnekleri hemen kullan**' a tıklayın.
1. **SQL Ile sorgu verileri**' ni seçin.
1. **Örnek kullan**' a tıklayın.
1. Yeni bir örnek SQL betiği açılır.
1. İlk sorguya kaydırın (28-32 satırları) ve sorgu metnini seçin.
1. Çalıştır'a tıklayın. Yalnızca seçtiğiniz kodu çalıştırır.

## <a name="loading-more-nyc-taxi-data"></a>Daha fazla NYC TAXI verisi yükleniyor
1. **Bilgi merkezine** gidin, **Galeriye** git ' e tıklayın.
1. Üstteki **SQL betikleri** sekmesini seçin.
1. **New York Taxicab veri kümesi** veri alım örneğini Yükle ' yi seçin, **devam**' a tıklayın.
1. **SQL havuzu** altında, **var olan bir havuzu seçin** ' i seçin ve **SQLPOOL1**' ı seçin ve daha önce oluşturduğunuz **SQLPOOL1** veritabanını seçin.
1. **Betiği aç**' a tıklayın.
1. Yeni bir örnek SQL betiği açılır.
1. **Çalıştır** 'a tıklayın
1. Bu, NYC TAXI verileri için birkaç tablo oluşturur ve T-SQL COPY komutunu kullanarak bunları yükler. Önceki hızlı başlangıç adımlarında bu tabloları oluşturduysanız, mevcut olmayan tablolar için oluşturmak ve kopyalamak üzere yalnızca kodu seçin ve yürütün.

    > [!NOTE] 
    > Adanmış bir SQL Havuzu (eski adıyla SQL DW) içeren SQL betiği için örnek Galeri 'yi kullanırken, yalnızca var olan ayrılmış bir SQL havuzunu (eski adıyla SQL DW) kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Synapse Analytics ile çalışmaya başlama](get-started.md)
* [Çalışma alanı oluşturma](quickstart-create-workspace.md)
* [Sunucusuz SQL havuzu kullanma](quickstart-sql-on-demand.md)
