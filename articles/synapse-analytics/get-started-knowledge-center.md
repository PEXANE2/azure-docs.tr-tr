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
ms.date: 04/04/2021
ms.openlocfilehash: 92d1534b5dd0233ce88b81d605c6502b67a121df
ms.sourcegitcommit: b4fbb7a6a0aa93656e8dd29979786069eca567dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2021
ms.locfileid: "107307903"
---
# <a name="explore-the-synapse-knowledge-center"></a>SYNAPSE bilgi merkezini araştırma

Bu öğreticide, SYNAPSE Studio bilgi merkezini kullanmayı öğreneceksiniz.

## <a name="introduction-to-the-knowledge-center"></a>Bilgi merkezine giriş

SYNAPSE Studio 'da bilgi merkezini bulmanın iki yolu vardır:

  1. Ana Hub 'da, sayfanın sağ üst kısmındaki **öğrenme**' ye tıklayın.
  2. Üstteki menü çubuğunda, **?** ' e tıklayın. daha sonra **Bilgi Merkezi**.

Herhangi bir yöntemi seçin ve **bilgi merkezini** açın.

Görünür olduktan sonra, **Bilgi Merkezi** 'nin üç şey yapmanıza izin verdiğini görürsünüz:
* **Örnekleri hemen kullanın**. SYNAPSE 'in nasıl çalıştığına ilişkin hızlı bir örnek istiyorsanız bu seçeneği belirleyin.
* **Galeriye gözatamazsınız**. Bu seçenek, örnek veri kümelerini bağlamanızı ve SQL betikleri, Not defterleri ve işlem hatları biçiminde örnek kod eklemenizi sağlar.
* **Tur SYNAPSE Studio**. Bu seçenek sizi SYNAPSE Studio 'nun temel bölümlerinin kısa bir turuna götürür. Daha önce SYNAPSE Studio 'Yu kullanmadıysanız bu kullanışlıdır.

## <a name="exploring-use-samples-immediately"></a>Keşfetme: örnekleri hemen kullanın

Bu bölümde üç öğe vardır:
* Spark ile örnek verileri araştırma
* SQL ile verileri sorgulama
* SQL ile dış tablo oluşturma

1. **Bilgi merkezinde**, **örnekleri hemen kullan**' a tıklayın.
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

> [!div class="nextstepaction"]
> [Yönetici ekleme](get-started-add-admin.md)

