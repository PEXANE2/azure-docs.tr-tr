---
title: Azure Veri Paylaşımı terminolojisi
description: Azure Veri Paylaşımı'nda kullanılan kaynakları tanımlamak için kullanılan yaygın terimler hakkında bilgi edinin (veri sağlayıcısı, veri tüketicisi, veri paylaşımı, paylaşım aboneliği, anlık görüntü, davet, alıcı.)
ms.service: data-share
author: joannapea
ms.author: joanpo
ms.topic: conceptual
ms.date: 07/10/2019
ms.openlocfilehash: 33532380d8f98df44029eeea998130d1da5fdafd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73468546"
---
# <a name="azure-data-share-concepts"></a>Azure Veri Paylaşımı Kavramları 

Azure Veri Paylaşımı, veri paylaşımıyla ilgili bazı yeni terminolojiler sunar. Bu makalede, hizmet boyunca kullanılan görebilirsiniz bazı sık kullanılan terimler açıklanmaktadır. 

## <a name="data-provider"></a>Veri sağlayıcısı

Veri sağlayıcısı, verileri tüketicilerle paylaşan kuruluştur. Genellikle veri sağlayıcısı, verilerin sahibi veya küratörü olabilir. Veri sağlayıcıları çeşitli türdeki verileri paylaşmak ister. Bir veri sağlayıcısının paylaşmak isteyebileceği bazı veri örnekleri, satış noktası veya zaman serisi verileri gibi ham verileri içerir. Bir veri sağlayıcısı, önceden işlenmiş, önceden seçilmiş ve zaten analitik ve öngörüler içeren verileri paylaşmak isteyebilir. 

## <a name="data-consumer"></a>Veri tüketicisi 

Veri tüketicisi, bir veri sağlayıcısından veri alan kuruluştur. Veri tüketicisi, öngörüler elde etmek için paylaşılan verilere kendi verileriyle katılmak istiyor olabilir. Bazı durumlarda, veri tüketicisi zaten işlenmiş verileri alıyor olabilir. 

## <a name="data-share"></a>Veri Paylaşımı

Veri paylaşımı, tek bir varlık olarak paylaşılan bir veri kümeleri grubudur. Veri kümeleri, Azure Veri Paylaşımı tarafından desteklenen bir dizi Azure veri kaynağından olabilir. Şu anda Azure Veri Paylaşımı, Azure Blob Depolama ve Azure Veri Gölü Deposu'nü destekler. 

## <a name="share-subscription"></a>Aboneliği Paylaş 

Bir veri tüketicisi bir veri sağlayıcısından gelen veri paylaşımı davetini kabul ettiğinde Paylaşım Aboneliği oluşturulur. Veri sağlayıcıları, Azure Veri Paylaşımı hesaplarında **Sent Shares'e** göz gezinerek ve **Abonelikleri Paylaş'ı**seçerek etkin paylaşım aboneliklerini görüntüleyebilir.

Veri tüketicisi, **Alınan Paylaşımlar'a** göz atarak ve aldıkları hisselerin durumunu görüntüleyerek etkin bir hisse aboneliğiolup olmadığını denetleyebilir. 

## <a name="snapshot"></a>Anlık Görüntü

Bir anlık görüntü, veri paylaşımı davetini kabul ettiklerinde veri tüketicisi tarafından oluşturulabilir. Bir daveti kabul ettiklerinde, kendileriyle paylaşılan verilerin tam anlık görüntüsünü tetikleyebilirler. Anlık görüntü, veri tüketicisinin anlık fotoğrafı oluşturduğu noktadaki verilerin bir kopyasıdır. 

Tam ve artımlı olmak gibi iki tür anlık görüntü vardır. Tam anlık görüntü, veri paylaşımındaki tüm verileri içerir. Artımlı anlık görüntü, son anlık görüntü tetiklendirildi beri güncelleştirilen/eklenen tüm verileri içerir. 

## <a name="snapshot-settings-in-azure-data-share"></a>Azure Veri Paylaşımı'nda anlık görüntü ayarları
 
Veri sağlayıcısı, veri paylaşımı için anlık görüntü ayarını etkinleştirebilir. Bu ayar, veri tüketicilerinin oluştukları sırada artımlı güncelleştirmeler almalarını sağlar. Veri sağlayıcısı, veri tüketicilerinin paylaşılan verileriçin güncelleştirmeler almasını istiyorsa, bu ayar etkinleştirilmelidir. 

Bir veri sağlayıcısı bu ayarı etkinleştirilerse, bir yineleme aralığı seçilebilir. Yineleme aralığı saatlik veya günlük olabilir. 

Veri tüketicisi, yeni bir anlık görüntü oluşturduklardan bu yana değişen verileri içeren artımlı güncelleştirmeleri almak için bu anlık görüntü zamanlamasını kabul etme seçeneğine sahiptir. 

## <a name="invitation"></a>Davet

Bir veri sağlayıcısı, birden çok alıcıyı veri paylaşımına davet edebilir. Bunu veri paylaşımına alıcı ekleyerek yapabilirler. Bir veri paylaşımı oluşturulduktan sonra davetiyeler de eklenebilir. 

Bir veri sağlayıcısı, kabul edilmediyse, bir daveti gönderildikten sonra silebilir. Veri sağlayıcısı bir daveti silerve henüz kabul edilmemişse, veri tüketicisi daveti kabul edemeyecektir. 

Davetiyeler günde beş defaya kadar gücenebilir. 

## <a name="recipient"></a>Alıcı

Alıcı, veri paylaşımı daveti alan kişidir. Genellikle, bir veri sağlayıcısı oluşturdukları veri paylaşımına alıcılar ekler. Daveti alan alıcı daveti kabul ettikten sonra veri tüketicisi olur.  

## <a name="next-steps"></a>Sonraki adımlar

Veri paylaşmaya nasıl başlayacağınızı öğrenmek [için, veri öğreticinizi paylaşmaya devam edin.](share-your-data.md)
