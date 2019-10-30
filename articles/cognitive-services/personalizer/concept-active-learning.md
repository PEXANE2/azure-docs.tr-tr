---
title: Etkin ve etkin olmayan olaylar-kişiselleştirici
titleSuffix: Azure Cognitive Services
description: ''
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: diberry
ms.openlocfilehash: 321f12fef44cae43caf53d78b2908e68f9edd0a8
ms.sourcegitcommit: 38251963cf3b8c9373929e071b50fd9049942b37
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73043899"
---
# <a name="active-and-inactive-events"></a>Etkin ve etkin olmayan olaylar

Uygulamanız, derecelendirme API 'sini çağırdığında, uygulamanın Rewarterctionıd alanında hangi eylemde gösterilmesi gerektiğini alırsınız.  Bu andan itibaren, kişiselleştirici aynı EventID ile bir Reward çağrısı bekliyordu. Daha sonraki derecelendirme çağrılarında kullanılacak modeli eğitebilmek için ödül puanı kullanılacaktır. EventID için bir ödül çağrısı alınmıyorsa, bir en da daha sonra uygulanacak bir ödül olur. Varsayılan yeniden ödüller Azure portalında oluşturulur.

Bazı durumlarda, uygulamanın, sonucun ne zaman kullanılacağını veya kullanıcıya görüntülendiğini bilse bile, uygulamanın sırasıyla derecelendirme çağrısı yapması gerekebilir. Bu durum, örneğin, yükseltilen içeriğin sayfa işleme bir pazarlama kampanyası ile üzerine yazılmasından kaynaklanıyor olabilir. Derece çağrısının sonucu hiç kullanılmadıysa ve Kullanıcı onu görmüyorsa, her türlü, sıfır veya başka türlü herhangi bir şey ile eğitebilmek hatalı olur.
Genellikle bu durum oluşur:

* Kullanıcının görebiliyorsa veya görmeyebilir bir kullanıcı ARABIRIMI önceden oluşturuluyor olabilir. 
* Uygulamanız, daha az gerçek zamanlı bağlamla derecelendirme çağrılarının yapıldığı ve bunların çıktıları uygulama tarafından kullanılabilecek tahmine dayalı kişiselleştirmeye başlayabilir. 

Bu gibi durumlarda, kişiselleştirici kullanmanın doğru yolu, olayın _devre dışı_olmasını ısteyen bir sıra çağırarak. Kişiselleştiriciye bu olay için bir ödül beklenmez ve varsayılan olarak bir ödül uygulanmaz. İş mantığınızdaki daha sonra uygulama, sıralama çağrısından bilgileri kullanıyorsa, tüm yapmanız gereken olayı _etkinleştirir_ . Olayın etkin olduğu andan itibaren, yeniden oluşturma API 'sine açık bir çağrı yapılcaksa, kişiselleştirici olay için bir tepki bekler veya varsayılan bir ödül uygular.

## <a name="get-inactive-events"></a>Etkin olmayan olayları al

Bir olayın eğitimini devre dışı bırakmak için `learningEnabled = False` ile ara sıra çağırın.

Etkinlik kimliği için bir ödül gönderirseniz veya bu EventID için `activate` API 'sini çağırdığınızda etkin olmayan bir olay için öğrenme işlemi açıkça etkinleştirilir.

## <a name="learning-settings"></a>Öğrenme ayarları

Öğrenme ayarları, model eğitiminin belirli *hiper parametrelerini* belirler. Farklı öğrenme ayarları üzerinde eğitilen aynı verilerin iki modeli farklı olacak.

### <a name="import-and-export-learning-policies"></a>Öğrenme ilkelerini içeri ve dışarı aktarma

Azure portal öğrenme ilkesi dosyalarını içeri ve dışarı aktarabilirsiniz. Bu, mevcut ilkeleri kaydetmenize, test etmenize, bunları değiştirmenize ve bunları kaynak kodu denetiinizde daha sonra başvuru ve denetim için yapıtlar olarak arşivlemenize olanak tanır.

### <a name="learning-policy-settings"></a>Öğrenme ilkesi ayarları

**Öğrenme ilkesindeki** ayarların değiştirilmesi amaçlanmamaktadır. Yalnızca kişiselleştirmeden nasıl etkilendikleri hakkında bilgi alırken ayarları değiştirin. Ayarları bu bilgi olmadan değiştirmek, geçersiz kılma için kişiselleştirici modeller dahil olmak üzere yan etkilere neden olur.

### <a name="comparing-effectiveness-of-learning-policies"></a>Öğrenme ilkelerinin verimliliğini karşılaştırma

Farklı öğrenme Ilkelerinin, [çevrimdışı değerlendirmeler](concepts-offline-evaluation.md)gerçekleştirerek, kişiselleştirici günlüklerinde geçmiş verilere karşı nasıl gerçekleştirileceğini karşılaştırabilirsiniz.

Geçerli öğrenme ilkesiyle karşılaştırmak için [kendi öğrenme Ilkelerinizi karşıya yükleyin](how-to-offline-evaluation.md) .

### <a name="discovery-of-optimized-learning-policies"></a>İyileştirilmiş öğrenme ilkelerinin keşfi

Kişiselleştirici, [çevrimdışı bir değerlendirme](how-to-offline-evaluation.md)yaparken daha iyileştirilmiş bir öğrenme ilkesi oluşturabilir. Çevrimdışı bir değerlendirmede daha iyi bir performans elde etmek için gösterilen daha iyileştirilmiş bir öğrenme ilkesi, kişiselleştirmede çevrimiçi olarak kullanıldığında daha iyi sonuçlar verir.

İyileştirilmiş bir öğrenme ilkesi oluşturulduktan sonra, geçerli ilkenin hemen yerini alacak şekilde doğrudan kişiselleştirmeye uygulayabilir ya da daha fazla değerlendirme için kaydedebilir ve gelecekte atma, kaydetme veya uygulama hakkında daha sonra karar verebilirsiniz.
