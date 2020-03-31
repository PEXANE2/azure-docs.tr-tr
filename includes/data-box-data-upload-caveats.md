---
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: include
ms.date: 11/05/2019
ms.author: alkohli
ms.openlocfilehash: 16647b6a13e64073ab570d36a8a380d0e36bd855
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73800178"
---
- Dosyaları doğrudan önceden oluşturulmuş paylaşımların hiçbirine kopyalamayın. Paylaşımın altında bir klasör oluşturmanız ve ardından dosyaları bu klasöre kopyalamanız gerekir.
- *StorageAccount_BlockBlob* ve *StorageAccount_PageBlob* altındaki klasör bir kapsayıcıdır. Örneğin, kapsayıcılar *StorageAccount_BlockBlob/kapsayıcı* ve *StorageAccount_PageBlob/kapsayıcı*olarak oluşturulur.
- *doğrudan StorageAccount_AzureFiles* altında oluşturulan her klasör bir Azure Dosya Paylaşımı'na dönüştürülr.
- Bulutta kopyalanan nesneyle aynı ada sahip varolan bir Azure nesneniz (örneğin, bir blob veya dosya gibi) varsa, Veri Kutusu dosyanın üzerine bulutta yazar.
- *StorageAccount_BlockBlob* ve *StorageAccount_PageBlob* paylaşımlara yazılan her dosya sırasıyla blok blob ve sayfa blob olarak yüklenir.
- Azure blob depolama dizinleri desteklemez. *StorageAccount_BlockBlob* klasörün altında bir klasör oluşturursanız, blob adında sanal klasörler oluşturulur. Azure Dosyaları için gerçek dizin yapısı korunur.
- *StorageAccount_BlockBlob* ve StorageAccount_PageBlob klasörleri altında oluşturulan boş dizin hiyerarşisi (herhangi *bir* dosya olmadan) yüklenmiyor.
- Azure'a veri yüklerken herhangi bir hata varsa, hedef depolama hesabında bir hata günlüğü oluşturulur. Yükleme tamamlandığında bu hata günlüğüne giden yol kullanılabilir ve düzeltici eylemde kalmak için günlüğü gözden geçirebilirsiniz. Yüklenen verileri doğrulamadan verileri kaynaktan silmeyin.
- Dosya meta verileri ve NTFS izinleri, veriler Azure Dosyaları'na yüklendiğinde korunmaz. Örneğin, veriler kopyalandığında dosyaların *Son değiştirilen özniteliği* tutulmaz.
