---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 02/21/2019
ms.author: alkohli
ms.openlocfilehash: 8c87e14071b3bb40421ab655c172df739570e295
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188826"
---
Aşağıdaki uyarılar, Azure'a taşınırken veriler için geçerlidir.

- Birden fazla aygıtın aynı kapsayıcıya yazmamasını öneririz.
- Bulutta kopyalanan nesneyle aynı ada sahip varolan bir Azure nesneniz (örneğin, bir blob veya dosya gibi) varsa, aygıt dosyanın üzerine bulutta yazar.
- Paylaşım klasörleri altında oluşturulan boş bir dizin hiyerarşisi (herhangi bir dosya olmadan) blob kapsayıcılarına yüklenmez.
- Dosya Gezgini ile sürükle ve bırak kullanarak veya komut satırı üzerinden verileri kopyalayabilirsiniz. Kopyalanan dosyaların toplam boyutu 10 GB'dan büyükse, Robocopy veya rsync gibi toplu kopyalama programı kullanmanızı öneririz. Toplu kopyalama araçları, kopyalama işlemini aralıklı hatalar için yeniden dener ve ek esneklik sağlar.
- Azure depolama kapsayıcısıyla ilişkili paylaşım, oluşturma sırasında paylaşım için tanımlanan blob'ların türüyle eşleşmeyan blob'lar yüklerse, bu tür lekeler güncelleştirilmez. Örneğin, aygıtta bir blok blob paylaşımı oluşturursunuz. Paylaşımı sayfa lekeleri olan varolan bir bulut kapsayıcısıyla ilişkilendirin. Dosyaları indirmek için bu paylaşımı yenileyin. Bulutta sayfa lekeleri olarak zaten depolanmış olan yenilenmiş dosyaların bazılarını değiştirin. Yükleme hataları göreceksiniz.
