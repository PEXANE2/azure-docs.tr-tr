---
title: Resim etiketi en iyi uygulamalar
description: Azure konteyner kayıt defterine görüntüleri iterken ve görüntüler çekerken Docker konteyner görüntülerini etiketlemek ve sürüm yapmak için en iyi uygulamalar
author: stevelasker
ms.topic: article
ms.date: 07/10/2019
ms.author: stevelas
ms.openlocfilehash: b483317960409fe1fbea181706f12375606fe659
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75445746"
---
# <a name="recommendations-for-tagging-and-versioning-container-images"></a>Kapsayıcı görüntülerini etiketleme ve sürüm leme önerileri

Kapsayıcı görüntülerini bir kapsayıcı kayıt defterine dağıtırken ve sonra dağıtırken, görüntü etiketleme ve sürüm leme için bir stratejiye ihtiyacınız vardır. Bu makalede, iki yaklaşım ve her konteyner yaşam döngüsü sırasında uygun nerede anlatılmaktadır:

* Kararlı etiketler - Örneğin, *mycontainerimage:1.0*gibi önemli veya küçük bir sürümü belirtmek için yeniden kullandığınız **etiketler.**
* **Benzersiz etiketler** - *Mycontainerimage:abc123*gibi bir kayıt defterine itinher görüntü için farklı bir etiket .

## <a name="stable-tags"></a>Kararlı etiketler

**Öneri**: Konteyner yapılarınızın **temel görüntülerini** korumak için kararlı etiketler kullanın. Bu etiketler güncelleştirmeleri almaya devam ettiği ve üretim ortamlarında tutarsızlıklara neden olabileceğinden, kararlı etiketlerle dağıtımlardan kaçının.

*Kararlı etiketler,* bir geliştiricinin veya bir yapı sisteminin güncelleştirmeleri almaya devam eden belirli bir etiketi çekmeye devam edebileceği anlamına gelir. Ahır, içindekilerin dondurulduğu anlamına gelmez. Bunun yerine, kararlı görüntü nün bu sürümün amacı için kararlı olması gerektiğini ima eder. "Kararlı" kalmak için, güvenlik düzeltme eki veya çerçeve güncelleştirmeleri uygulamak için hizmet olabilir.

### <a name="example"></a>Örnek

Bir çerçeve ekibi sürüm 1.0'ı gemiyle karşılar. Küçük güncellemeler de dahil olmak üzere güncellemeleri göndereceğini biliyorlar. Belirli bir büyük ve küçük sürüm için kararlı etiketleri desteklemek için, kararlı etiketleri iki kümesi vardır.

* `:1`– ana sürüm için kararlı bir etiket. `1`"en yeni" veya "en son" 1.* sürümünü temsil eder.
* `:1.0`- sürüm 1.0 için kararlı bir etiket, bir geliştiricinin 1.0 güncelleştirmelerine bağlanmasına ve yayımlandığında 1.1'e yuvarlanmamasını sağlar.

Takım ayrıca, `:latest` geçerli ana sürüm ne olursa olsun, en son kararlı etiketi işaret etiketi ni kullanır.

Temel görüntü güncelleştirmeleri veya çerçevenin herhangi bir hizmet sürümü olduğunda, kararlı etiketleri olan görüntüler, bu sürümün en güncel kararlı sürümünü temsil eden en yeni özete güncelleştirilir.

Bu durumda, hem büyük hem de küçük etiketlersürekli olarak servis ediliyor. Temel görüntü senaryosundan, bu görüntü sahibinin servisli görüntüler sağlamasına olanak tanır.

### <a name="delete-untagged-manifests"></a>Etiketlenmemiş bildirimleri silme

Kararlı etiketi olan bir görüntü güncelleştirilirse, önceden etiketlenmiş görüntü etiketlenmemiş olur ve bu da yetim bir görüntüyle sonuçlanır. Önceki resmin manifestosu ve benzersiz katman verileri kayıt defterinde kalır. Kayıt defteri boyutunuzu korumak için, kararlı görüntü güncelleştirmelerinden kaynaklanan etiketlenmemiş bildirimleri düzenli aralıklarla silebilirsiniz. Örneğin, etiketlenmemiş bildirimleri [otomatik olarak temizleme,](container-registry-auto-purge.md) belirli bir süreden daha eski direnebilir veya etiketlenmemiş bildirimler için bir [bekletme ilkesi](container-registry-retention-policy.md) ayarlayın.

## <a name="unique-tags"></a>Benzersiz etiketler

**Öneri**: Özellikle birden çok düğümüzerinde ölçeklendirilebilen bir ortamda **dağıtımlar**için benzersiz etiketler kullanın. Bileşenlerin tutarlı bir sürümünün kasıtlı olarak dağıtımını isteyebilirsiniz. Kapsayıcınız yeniden başlatılırsa veya bir orkestratör daha fazla örneği ölçeklendirirse, ana bilgisayarlarınız yanlışlıkla diğer düğümlerle tutarsız yeni bir sürüm çekmez.

Benzersiz etiketleme, bir kayıt defterine itilen her görüntünün benzersiz bir etiketi olduğu anlamına gelir. Etiketler yeniden kullanılmaz. Benzersiz etiketler oluşturmak için izleyebileceğiniz çeşitli desenler vardır:

* **Tarih-zaman damgası** - Bu yaklaşım oldukça yaygındır, çünkü görüntünün ne zaman üretedildiğini açıkça anlayabilirsiniz. Ama, nasıl geri yapı sistemi ile ilişkilendirmek için? Aynı anda tamamlanan yapıyı bulmak zorunda mısın? Hangi saat dilimindesin? Tüm yapı sistemleriniz UTC'ye ayarlandı mı?
* **Git commit** – Bu yaklaşım, temel görüntü güncelleştirmelerini desteklemeye başlayana kadar çalışır. Bir temel görüntü güncelleştirmesi gerçekleşirse, yapı sisteminiz önceki yapıyla aynı Git commit ile başlatılır. Ancak, temel görüntü yeni içeriğe sahiptir. Genel olarak, Git commit *yarı*kararlı bir etiket sağlar.
* **Manifest özeti** - Bir konteyner kayıt defterine itilen her konteyner görüntüsü, benzersiz bir SHA-256 karma veya özet tarafından tanımlanan bir manifesto ile ilişkilidir. Benzersiz olsa da, özet uzun, okunması zor ve yapı ortamınızla ilgisizdir.
* **Yapı Kimliği** - Bu seçenek, büyük olasılıkla artımlı olduğundan en iyi seçenek olabilir ve tüm yapıları ve günlükleri bulmak için belirli yapıyla ilişkilendirmenize olanak tanır. Ancak, apaçık bir özet gibi, bir insan için okumak zor olabilir.

  Kuruluşunuzda birden çok yapı sistemi varsa, etiketi yapı sistemi adı ile `<build-system>-<build-id>`öne etmek bu seçeneğin bir varyasyonudur: . Örneğin, yapıları API ekibinin Jenkins yapı sisteminden ve web ekibinin Azure Ardışık Hatları yapı sisteminden ayırt edebilirsiniz.

### <a name="lock-deployed-image-tags"></a>Dağıtılan resim etiketlerini kilitleme

En iyi uygulama olarak, bağlı olduğu herhangi bir resim `write-enabled` etiketini, `false`özniteliğini ' ye ayarlayarak [kilitlemenizi](container-registry-image-lock.md) öneririz. Bu uygulama, bir resmi yanlışlıkla kayıt defterinden kaldırmanızı ve büyük olasılıkla dağıtımlarınızı kesintiye uğratmanızı önler. Kilitleme adımını sürüm ardınıza ekleyebilirsiniz.

Dağıtılan bir görüntüyü kilitlemek, kayıt defterinizi korumak için Azure Konteyner Kayıt Defteri özelliklerini kullanarak kayıt defterinizden dağıtılmamış diğer görüntüleri kaldırmanıza olanak tanır. Örneğin, etiketlenmemiş bildirimleri veya belirli bir süreden daha eski olmayan görüntülerin kilidini açan [otomatik temizleme](container-registry-auto-purge.md) veya etiketlenmemiş bildirimler için bir [bekletme ilkesi](container-registry-retention-policy.md) ayarlayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu makaledeki kavramların daha ayrıntılı bir tartışma için, blog yazısı [Docker Etiketleme bakın: etiketleme ve docker görüntüleri sürüm için en iyi uygulamalar](https://stevelasker.blog/2018/03/01/docker-tagging-best-practices-for-tagging-and-versioning-docker-images/).

Azure kapsayıcı kayıt defterinizin performansını ve uygun maliyetli kullanımını en üst düzeye çıkarmak [için Azure Kapsayıcı Kayıt Defteri için en iyi uygulamalara](container-registry-best-practices.md)bakın.

<!-- IMAGES -->


<!-- LINKS - Internal -->

