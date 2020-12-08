---
title: dosya dahil etme
description: dosya dahil etme
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 08/28/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 741ace82a5ec0b44215e4a2df6c64f5b92e0c275
ms.sourcegitcommit: 8b4b4e060c109a97d58e8f8df6f5d759f1ef12cf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2020
ms.locfileid: "96842723"
---
Azure dosyaları, kendi senaryolarınızın performans ve fiyat gereksinimlerine yönelik olarak paylaşımlarınızı uyarlamanızı sağlamak için dört farklı depolama, Premium, işlem için iyileştirilmiş, sık erişimli ve seyrek katman katmanları sunar:

- **Premium**: Premium dosya paylaşımları katı hal sürücüleri (SSD 'ler) tarafından DESTEKLENIR ve GÇ yoğun iş yükleri IÇIN çoğu GÇ işlemi için tek basamaklı milisaniye içinde tutarlı yüksek performans ve düşük gecikme süresi sağlar. Premium dosya paylaşımları veritabanları, Web sitesi barındırma ve geliştirme ortamları gibi çok çeşitli iş yükleri için uygundur. Premium dosya paylaşımları, sunucu Ileti bloğu (SMB) ve ağ dosya sistemi (NFS) protokolleriyle birlikte kullanılabilir.
- **İşlem için iyileştirilmiş**: işlem için iyileştirilmiş dosya paylaşımları Premium dosya paylaşımları tarafından sunulan gecikmeye gerek olmayan işlem ağır iş yüklerini etkinleştirir. İşlem için iyileştirilmiş dosya paylaşımları, sabit disk sürücüleri (HDD 'Ler) tarafından desteklenen standart depolama donanımında sunulur. İşlem için iyileştirilmiş, "standart" olarak adlandırılmıştı, ancak bu, katmanın kendisi yerine depolama medyası türü anlamına gelir (Standart depolama donanımında olduklarından, sık ve seyrek da "standart" katmanlarıdır).
- **Sık** erişimli: sık erişimli dosya paylaşımları, takım paylaşımları ve Azure dosya eşitleme gibi genel amaçlı dosya paylaşım senaryolarında en iyi duruma getirilmiş depolama sunar. Etkin dosya paylaşımları, HDD 'Ler tarafından desteklenen standart depolama donanımında sunulur.
- Seyrek **erişimli: seyrek** erişimli dosya paylaşımları, çevrimiçi arşiv depolama senaryoları için iyileştirilmiş uygun maliyetli depolama sunar. Azure Dosya Eşitleme, daha düşük dalgalanma iş yükleri için de uygun olabilir. Seyrek Erişimli dosya paylaşımları, HDD 'Ler tarafından desteklenen standart depolama donanımında sunulur.

Premium dosya paylaşımları, **FileStorage depolama hesabı** türüne dağıtılır ve yalnızca sağlanan faturalandırma modelinde kullanılabilir. Premium dosya paylaşımları için sağlanan faturalandırma modeli hakkında daha fazla bilgi için bkz. [Premium dosya paylaşımları için sağlamayı anlama](../articles/storage/files/storage-files-planning.md#understanding-provisioning-for-premium-file-shares). İşlem için iyileştirilmiş, sık erişimli ve seyrek erişimli dosya paylaşımları dahil olmak üzere standart dosya paylaşımları **genel amaçlı sürüm 2 (GPv2) depolama hesabı** türüne dağıtılır ve kullandıkça öde faturalandırma üzerinden kullanılabilir. Sık erişimli ve seyrek erişimli dosya paylaşımları, tüm Azure Kamu ve Azure Kamu bölgelerinde kullanılabilir. İşlem için iyileştirilmiş dosya paylaşımları, Azure Çin ve Azure Almanya bölgeleri dahil olmak üzere tüm Azure bölgelerinde kullanılabilir.

İş yükünüz için bir depolama katmanı seçerken, performans ve kullanım gereksinimlerinizi göz önünde bulundurun. İş yükünüz tek basamaklı gecikme gerektiriyorsa veya şirket içi SSD depolama medyası kullanıyorsanız, Premium katman büyük olasılıkla en uygun seçenektir. Düşük gecikme süresi çok önemli değilse, örneğin, Azure 'dan şirket içine takılan veya şirket içinde Azure Dosya Eşitleme aracılığıyla önbelleğe alınmış takım paylaşımları ile standart depolama, maliyet açısından daha iyi bir uyum sağlayabilir.

İşlem için iyileştirilmiş, sık erişimli ve seyrek erişimli katmanlar, tam olarak aynı standart depolama donanımında depolanır. Bu üç katman için önemli fark, daha soğuk katmanlarda daha düşük olan ve daha soğuk katmanlarda olan işlem fiyatlarına ait olan, bekleyen veri depolama fiyatlarıdır. Diğer bir deyişle:

- Adın gösterdiği gibi işlem için iyileştirilmiş, yüksek işlem iş yüklerinin fiyatını iyileştirir. İşlem için iyileştirilmiş en yüksek veri, Rest depolama fiyatına, ancak en düşük işlem fiyatlarına sahiptir.
- Sık kullanılan etkin iş yükleri için çok sayıda işlem içermeyen ve daha düşük bir veri bekleyen depolama fiyatına sahip olan ve işlem iyileştirilmesine kıyasla biraz daha yüksek işlem fiyatları vardır.
- Seyrek Erişimli, çok fazla etkinliği olmayan iş yüklerinin fiyatını en iyi duruma getirir ve en düşük veri, bekleyen depolama fiyatı, ancak en yüksek işlem fiyatlarıdır.

İş yükünüz ve etkinlik düzeyiniz standart dosya paylaşımınız için en uygun maliyetli katmanı saptacaktır. [Azure dosyaları fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/storage/files/)istediğiniz bölge, artıklık düzeyi ve para birimi için tam fiyatları görüntüleyebilirsiniz.

Depolama hesabında bir dosya paylaşma oluşturduktan sonra, farklı depolama hesabı türlerine özel katmanlara taşıyamazsınız. Örneğin, bir işlem için iyileştirilmiş dosya payını Premium katmana taşımak için, bir FileStorage depolama hesabında yeni bir dosya paylaşma oluşturmanız ve verileri özgün paylaşımınızdan FileStorage hesabındaki yeni bir dosya paylaşımıyla kopyalamanız gerekir. Azure dosya paylaşımları arasında veri kopyalamak için AzCopy kullanmanız önerilir, ancak `robocopy` Windows veya macOS ve Linux gibi araçları da kullanabilirsiniz `rsync` . 

GPv2 depolama hesapları içinde dağıtılan dosya paylaşımları, yeni bir depolama hesabı oluşturmadan ve veri geçirmeden Standart katmanlar (işlem için iyileştirilmiş, sık erişimli ve seyrek erişimli) arasında taşınabilir, ancak katmanınızı değiştirdiğinizde işlem maliyetlerine tabi olursunuz. Bir eklentiyi bir Hotter katmanından daha soğuk bir katmana taşıdığınızda, paylaşımdaki her dosya için daha soğuk katmanın yazma işlemi ücreti uygulanır. Bir dosya paylaşımının daha soğuk bir katmandan Hotter katmanına taşınması, paylaşılan katmanın paylaşımdaki her dosya için okuma işlemi ücretine neden olur.