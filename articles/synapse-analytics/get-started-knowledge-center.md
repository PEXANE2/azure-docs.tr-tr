---
title: "Öğretici: SYNAPSE bilgi merkezi 'Ni kullanmaya başlayın"
description: Bu öğreticide, SYNAPSE bilgi merkezini kullanmayı öğreneceksiniz.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.topic: tutorial
ms.date: 09/15/2020
ms.openlocfilehash: c8aaef9c98e1fe44c73d4bd464bb1e52122e7a57
ms.sourcegitcommit: 7374b41bb1469f2e3ef119ffaf735f03f5fad484
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2020
ms.locfileid: "90709692"
---
# <a name="explore-the-synapse-knowledge-center"></a>SYNAPSE bilgi merkezini araştırma

Bu öğreticide, SYNAPSE Studio bilgi merkezini kullanmayı öğreneceksiniz.

## <a name="getting-to-the-knowledge-center"></a>Bilgi merkezine alma

SYNAPSE Studio 'da bilgi merkezini bulmanın iki yolu vardır:

  1. Giriş merkezinde, yararlı bağlantılar altında, **Bilgi Merkezi**adlı ilk bağlantıya tıklayın.
  2. Üstteki menü çubuğunda, **?** ' e tıklayın. daha sonra  **Bilgi Merkezi**.

Herhangi bir yöntemi seçin ve **bilgi merkezini**açın.

## <a name="overview"></a>Genel Bakış

**Bilgi Merkezi** , üç şey yapmanıza olanak sağlar:
* **Örnekleri hemen kullanın**. Bu seçenek, analiz işlemini mümkün olduğunca hızlı şekilde görmek için en iyi duruma getirilmiştir. SYNAPSE 'in nasıl çalıştığına ilişkin hızlı bir örnek istiyorsanız bu seçeneği belirleyin.
* **Tarayıcı kullanılabilir örneği**. Bu seçenek, örnek veri kümelerini bağlamanızı ve SQL betikleri, Not defterleri ve işlem hatları biçiminde örnek kod eklemenizi sağlar.
* **Tur SYNAPSE Studio**. Bu seçenek sizi SYNAPSE Studio 'nun temel bölümlerinin kısa bir turuna götürür. Daha önce SYNAPSE Studio 'Yu kullanmadıysanız bu kullanışlıdır.

## <a name="exploring-blob-storage-with-sql-on-demand"></a>İsteğe bağlı SQL ile blob depolamayı keşfetme

1. **Bilgi Bankası merkezi**, **örnekleri hemen kullan** ' a tıklayın.
1. **SQL Ile Sorgu verisi** seçme 
1. **Örnekleri hemen kullan** ' a tıklayın
1. Yeni bir SQL betiği oluşturacak.
1. İlk sorguya kaydırın (28-32 satırları) ve sorgu metnini seçin
1. Çalıştır'a tıklayın. Seçtiğiniz metni çalıştıracaktır.

## <a name="loading-the-more-nyc-taxi-data"></a>Diğer NYC TAXI verileri yükleniyor

1. **Bilgi Bankası merkezi**, **kullanılabilir örneklere gözatıp** ' ye tıklayın 
1. Üstteki **SQL betikleri** sekmesini seçin
1. **New York Taxicab veri kümesini yükle** ' yi seçin
1. **Girişler**altında, **var olan bir havuzu seçin** öğesini seçin ve **SQLDB1** seçin
1. **Betiği aç** ' a tıklayın
1. Yeni bir SQL betiği görüntülenir.
1. **Çalıştır** 'a tıklayın
1. Bu, NYC TAXI verileri için birkaç tablo oluşturur ve T-SQL COPY komutunu kullanarak bunları yükler.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [SQL havuzu kullanarak çözümleme](get-started-analyze-sql-pool.md)
