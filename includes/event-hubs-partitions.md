---
title: include dosyası
description: include dosyası
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.topic: include
ms.date: 05/22/2019
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: dc7c86ff1df48f9ce96769098f7aab76d33c8822
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "68481594"
---
Event Hubs her bir tüketicinin ileti akışında yalnızca belirli bir alt küme ya da bölümü okuduğu bölünmüş bir tüketici modeli aracılığıyla ileti akışı sağlar. Bu model, olay işleme için yatay ölçek sağlar ve kuyruklar ile konularda kullanılamayan diğer akış odaklı özellikleri sunar.

Bölüm bir olay hub'ında tutulan olayların sıralı dizisidir. Yeni olaylar geldikçe dizinin sonuna eklenir. Bölüm bir "yürütme günlüğü" olarak düşünülebilir.

![Event Hubs](./media/event-hubs-partitions/partition.png)

Olay Hub'ları, olay merkezindeki tüm bölümlerde uygulanan yapılandırılmış bir bekletme süresi için verileri saklar. Olayların süresi saat bazında dolar; bunları açıkça silemezsiniz. Bölümler birbirinden bağımsız olup kendi veri dizisini içerdiğinden genellikle farklı hızlarda büyürler.

![Event Hubs](./media/event-hubs-partitions/multiple-partitions.png)

Bölüm sayısı, oluşturma sırasında belirtilir ve 2 ile 32 arasında olmalıdır. Bölüm sayısı değiştirilemez; bu nedenle, bölüm sayısını ayarlarken uzun vadeli ölçeği dikkate almanız gerekir. Bölümler, tüketen uygulamalarda gerekli aşağı akış paralelliğiyle ilişkili bir veri düzenleme mekanizmasıdır. Bir olay hub'ındaki bölüm sayısı, sahip olmayı beklediğiniz eşzamanlı okuyucu sayısıyla doğrudan ilgilidir. Event Hubs ekibine başvurarak bölüm sayısını 32’nin üzerine çıkarabilirsiniz.

Oluşturma sırasında mümkün olan en yüksek değer olan 32 olarak ayarlamak isteyebilirsiniz. Birden fazla bölüme sahip olmanın, gönderenleri yalnızca kalan 31 bölümden gereksiz bırakarak 32 bölümden tek bir bölüme gönderecek şekilde yapılandırmadığınız sürece, sırayı tutmadan birden fazla bölüme gönderilen olaylara neden olacağını unutmayın. Eski durumda, tüm 32 bölüm boyunca olayları okumak zorunda kalacak. İkinci durumda, Olay İşlemci Ana Bilgisayar'da yapmak zorunda olduğunuz ekstra yapılandırma dışında belirgin bir ek maliyet yoktur.

Bölümler tanımlanabilir ve doğrudan gönderilebilir olsa da, doğrudan bir bölüme gönderme önerilmez. Bunun yerine, [Olay yayıncıları](../articles/event-hubs/event-hubs-features.md#event-publishers) bölümünde tanıtılan daha yüksek düzey yapıları kullanabilirsiniz. 

Bölümler, olayın gövdesini, kullanıcı tanımlı özellik çantasını ve bölümdeki ofset'i ve akış dizisindeki numarası gibi meta verileri içeren bir dizi olay verisiyle doldurulur.

En iyi ölçek elde etmek için 1:1 iş bölümü ve bölümleri dengelemenizi öneririz. Tek bir bölüm, garantili giriş ve çıkış a kadar bir iş birimi vardır. Bir bölüm üzerinde daha yüksek iş elde etmek mümkün olsa da, performans garanti edilmez. Bu nedenle, bir olay merkezindeki bölüm sayısının, iş birimi sayısından daha büyük veya eşit olmasını şiddetle tavsiye ediyoruz.

İhtiyacınız olan toplam iş tükenen toplam iş bölümü göz önüne alındığında, gereksinim duyduğunuz iş birimi sayısını ve en az bölüm sayısını biliyorsunuz, ancak kaç bölüme sahip olmanız gerekir? Gelecekteki iş akışı gereksinimleriniz kadar ulaşmak istediğiniz aşağı akış paralelliğine göre bölüm sayısını seçin. Olay Hub'ındaki bölüm sayısı için ücret alınmaz.

Bölümleri ve kullanılabilirlikleri ile güvenilirlikleri arasındaki dengeleme hakkında daha fazla bilgi için, bkz: [Event Hubs programlama kılavuzu](../articles/event-hubs/event-hubs-programming-guide.md#partition-key) ve [Event Hubs’ta kullanılabilirlik ve tutarlılık](../articles/event-hubs/event-hubs-availability-and-consistency.md) makalesi.
