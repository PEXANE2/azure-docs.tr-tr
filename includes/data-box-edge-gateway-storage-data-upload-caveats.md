---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 02/21/2019
ms.author: alkohli
ms.openlocfilehash: 8c87e14071b3bb40421ab655c172df739570e295
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "67188826"
---
Aşağıdaki uyarılar, Azure 'a taşırken veriler için geçerlidir.

- Birden fazla cihazın aynı kapsayıcıya yazmamalıdır.
- Bulutta, Kopyalanmakta olan nesneyle aynı ada sahip mevcut bir Azure nesneniz (blob veya bir dosya gibi) varsa, cihaz buluttaki dosyanın üzerine yazar.
- Klasörleri paylaşma altında oluşturulan boş bir dizin hiyerarşisi (herhangi bir dosya olmadan) blob kapsayıcılarına yüklenemez.
- Dosya Gezgini ile veya komut satırı aracılığıyla sürükle ve bırak kullanarak verileri kopyalayabilirsiniz. Kopyalanan dosyaların toplam boyutu 10 GB 'den büyükse Robocopy veya rsync gibi bir toplu kopyalama programı kullanmanızı öneririz. Toplu kopyalama araçları, zaman aralıklı hatalar için kopyalama işlemini yeniden dener ve ek dayanıklılık sağlar.
- Azure depolama kapsayıcısı ile ilişkilendirilen paylaşımın, oluşturma sırasında, paylaşma için tanımlanan Blobların türüyle eşleşmeyen Blobları karşıya yüklediğinde, bu tür Bloblar güncellenmez. Örneğin, cihazda bir Blok Blobu paylaşma oluşturursunuz. Paylaşma, sayfa Blobları olan mevcut bir bulut kapsayıcısı ile ilişkilendirin. Dosyaları indirmek için bu paylaşımın yenilenmesini yapın. Bulutta sayfa Blobları olarak zaten depolanmış olan yenilenmiş dosyalardan bazılarını değiştirin. Karşıya yükleme başarısızlıklarını görürsünüz.
