---
title: Azure HPC önbellek kullanım modelleri
description: Farklı önbellek kullanım modellerini ve bunların arasından salt okunurdur veya okuma/yazma önbelleği ayarlamak ve diğer önbelleğe alma ayarlarını denetlemek için nasıl tercih edileceğini açıklar
author: ekpgh
ms.service: hpc-cache
ms.topic: how-to
ms.date: 03/08/2021
ms.author: v-erkel
ms.openlocfilehash: 856f2c15d2bd0b39212e8962a92b1df50cada29e
ms.sourcegitcommit: 66ce33826d77416dc2e4ba5447eeb387705a6ae5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2021
ms.locfileid: "103472890"
---
# <a name="understand-cache-usage-models"></a>Önbellek kullanım modellerini anlama

Önbellek kullanım modelleri, Azure HPC önbelleğinizi iş akışınızı hızlandırmak için dosyaları nasıl depolayabileceğinizi özelleştirmenize olanak tanır.

## <a name="basic-file-caching-concepts"></a>Temel dosya önbelleğe alma kavramları

Dosya önbelleğe alma, Azure HPC önbelleğinin istemci isteklerini nasıl hızlandıran. Bu temel uygulamaları kullanır:

* **Okuma önbelleği** -Azure HPC Cache, istemcilerin talep edilen depolama sisteminden aldığı dosyaların bir kopyasını tutar. İstemci bir dahaki sefer aynı dosyayı istediğinde HPC önbelleği, arka uç depolama sisteminden yeniden getirmek yerine sürümü önbellekte sağlayabilir.

* **Yazma önbelleği** -isteğe bağlı olarak, Azure HPC Cache, istemci makinelerden gönderilen değiştirilmiş dosyaların bir kopyasını depolayabilirler. Birden çok istemci, kısa bir süre içinde aynı dosyada değişiklik yapsa, önbellek her değişikliği arka uç depolama sistemine tek tek yazmak yerine önbellekteki tüm değişiklikleri toplayabilir.

  Değişiklik olmadan belirli bir süre geçtikten sonra önbellek, dosyayı uzun süreli depolama sistemine taşımıştır.

  Yazma önbelleği devre dışıysa, önbellek değiştirilen dosyayı depolamaz ve hemen arka uç depolama sistemine yazar.

* **Geri yazma gecikmesi** -yazma önbelleği açık olan bir önbellek için geri yazma gecikmesi, dosyanın arka uç depolama sistemine taşınmadan önce önbelleğin ek dosya değişiklikleri için bekleyeceği süre miktarıdır.

* **Arka uç doğrulaması** -arka uç doğrulama ayarı, önbelleğin arka uç depolama sistemindeki uzak sürümle bir dosyanın yerel kopyasını ne sıklıkta karşılaştırdığını belirler. Arka uç kopyası önbelleğe alınmış kopyadan daha yeniyse, önbellek uzak kopyayı getirir ve gelecekteki istekler için depolar.

  Arka uç doğrulama ayarı, önbelleğin, dosyalarını uzak depolamadaki kaynak dosyalarla *otomatik olarak* karşılaştırdığı zamanı gösterir. Ancak, bir readdirplus isteği içeren bir dizin işlemi gerçekleştirerek Azure HPC önbelleğini dosyaları karşılaştırmaya zorlayabilirsiniz. Readdirplus, dizin meta verileri döndüren standart bir NFS API 'sidir (genişletilmiş okuma olarak da bilinir). Bu, önbelleğin dosyaları karşılaştırmasına ve güncelleştirmesine neden olur.

Azure HPC önbelleğinde yerleşik kullanım modellerinin bu ayarlar için farklı değerleri vardır ve bu sayede durumunuz için en iyi birleşimi seçebilmenizi sağlayabilirsiniz.

## <a name="choose-the-right-usage-model-for-your-workflow"></a>İş akışınız için doğru kullanım modelini seçin

Kullandığınız her NFS bağlı depolama hedefi için bir kullanım modeli seçmeniz gerekir. Azure Blob depolama hedefleri, özelleştirilebilecek yerleşik bir kullanım modeline sahiptir.

HPC önbelleği kullanım modelleri, eski verileri alma riskiyle hızlı yanıt dengelemeye olanak tanır. Dosya okuma hızını iyileştirmek isterseniz, önbellekteki dosyaların arka uç dosyalarına karşı kontrol edilip edilmeyeceğini önemsemeyebilirsiniz. Diğer taraftan, her zaman uzak depolama ile dosyalarınızın güncel olduğundan emin olmak istiyorsanız, sık denetleyen bir model seçin.

Birkaç seçenek vardır:

* **Ağır, seyrek erişimli yazmaları okuma** -statik veya nadiren değiştirilen dosyalara okuma erişimini hızlandırmak istiyorsanız bu seçeneği kullanın.

  Bu seçenek, istemci okumalarını önbelleğe alır ancak önbelleğe alma işlemleri yapmaz. Yazma işlemlerini hemen arka uç depolamaya geçirir.
  
  Önbellekte depolanan dosyalar, NFS depolama birimindeki dosyalarla otomatik olarak karşılaştırılmaz. (Bunları el ile karşılaştırmayı öğrenmek için yukarıdaki arka uç doğrulamasının açıklamasını okuyun.)

  Bir dosyanın doğrudan depolama sisteminde önbellekte yazılmadan değiştirilebilmesi için bir risk varsa bu seçeneği kullanmayın. Bu durumda, dosyanın önbelleğe alınmış sürümü arka uç dosyasıyla eşitlenmemiş olur.

* **%15 ' ten fazla yazma** -Bu seçenek hem okuma hem de yazma performansını hızlandırır. Bu seçeneği kullanırken, tüm istemcilerin arka uç depolamayı doğrudan bağlamak yerine Azure HPC Cache aracılığıyla dosyalara erişmesi gerekir. Önbelleğe alınan dosyalar arka uçta depolanmayan son değişikliklere sahip olur.

  Bu kullanım modelinde, önbellekteki dosyalar yalnızca her sekiz saatte bir arka uç depolamada bulunan dosyalara karşı denetlenir. Dosyanın önbelleğe alınan sürümünün daha güncel olduğu varsayılır. Önbellekte değiştirilen bir dosya, önbellekte 20 dakika geçtikten sonra arka uç depolama sistemine yazılır<!-- an hour --> ek değişiklik yok.

* **İstemciler, önbelleği ATLAYARAK NFS hedefine yazar** -iş akışlarınızdan herhangi bir istemci, önce önbelleğe yazmadan veya veri tutarlılığını iyileştirmek istiyorsanız bu seçeneği belirleyin. İstemcilerin istediği dosyalar önbelleğe alınır (okur), ancak istemciden (yazma) bu dosyalardaki değişiklikler önbelleğe alınmaz. Bunlar doğrudan arka uç depolama sistemine geçirilir.

  Bu kullanım modeliyle, önbellekteki dosyalar güncelleştirmeler için arka uç sürümlerine göre sıklıkla denetlenir. Bu doğrulama, verilerin tutarlılığın korunmasında önbelleğin dışında değiştirilmesine izin verir.

* **%15 ' ten fazla yazma, her 30 saniyede** bir ve 15 ' ten fazla yazma için yedekleme sunucusu denetleniyor **, her 60 saniyede bir yedekleme sunucusu denetleniyor** ; bu seçenekler, hem okuma hem de yazma işlemlerini hızlandırmak istediğiniz iş akışları için tasarlanmıştır, ancak başka bir kullanıcının doğrudan arka uç depolama sistemine yazacağı bir şansınız vardır. Örneğin, birden çok istemci kümesi farklı konumlardan aynı dosyalar üzerinde çalışıyorsa, bu kullanım modelleri kaynaktaki eski içerik için düşük toleranslı olan hızlı dosya erişimi gereksinimini dengelemek açısından anlamlı hale gelebilir.

* **%15 ' ten fazla yazma, her 30 saniyede bir sunucuya geri yazma** -Bu seçenek, birden fazla istemcinin aynı dosyaları etkin bir şekilde değiştiren senaryoya veya bazı istemcilerde önbelleği bağlamak yerine doğrudan uç depolamaya erişebildiğine yönelik olarak tasarlanmıştır.

  Sık kullanılan arka uç yazma işlemleri önbellek performansını etkiler. bu nedenle, dosya çakışmasıyla ilgili düşük bir risk varsa, örneğin farklı istemcilerin aynı dosya kümesinin farklı alanlarında çalıştığını biliyorsanız, **%15 ' ten fazla yazma** kullanım modelini kullanmayı düşünmelisiniz.

* **Yoğun okuma, yedekleme sunucusunu 3 saatte bir denetleme** -Bu seçenek istemci tarafında hızlı okumaları önceliklendirir, ancak Ayrıca, **yoğun okuma, seyrek kullanılan yazma** kullanım modelinden farklı olarak önbelleğe alınan dosyaları arka uç depolama sisteminden de yeniler.

Bu tablo, kullanım modeli farklarını özetler:

| Kullanım modeli                   | Önbelleğe alma modu | Arka uç doğrulaması | En fazla geri yazma gecikmesi |
|-------------------------------|--------------|-----------------------|--------------------------|
| Yoğun, seyrek okunan yazma işlemleri | Okuma         | Asla                 | Yok                     |
| %15 yazma boyutundan büyük       | Okuma/yazma   | 8 saat               | 20 dakika               |
| İstemcileri önbelleği atlar      | Okuma         | 30 saniye            | Yok                     |
| %15 yazma boyutundan büyük, sık kullanılan arka uç denetimi (30 saniye) | Okuma/yazma | 30 saniye | 20 dakika |
| %15 yazma daha fazla, sık kullanılan arka uç denetimi (60 saniye) | Okuma/yazma | 60 saniye | 20 dakika |
| %15 yazma boyutundan büyük, sık geri yazma | Okuma/yazma | 30 saniye | 30 saniye |
| Yoğun okuma, her 3 saatte bir yedekleme sunucusu denetleniyor | Okuma | 3 saat | Yok |

Azure HPC Cache iş akışınız için en iyi kullanım modeliyle ilgili sorularınız varsa, Azure temsilcinizle konuşun veya yardım için bir destek isteği açın.

## <a name="next-steps"></a>Sonraki adımlar

* Azure HPC önbelleğinize [depolama hedefleri ekleme](hpc-cache-add-storage.md)
