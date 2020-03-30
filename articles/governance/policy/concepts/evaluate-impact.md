---
title: Yeni bir Azure ilkesinin etkisini değerlendirme
description: Azure ortamınıza yeni bir ilke tanımı sunarken izlenmeniz gereken işlemi anlayın.
ms.date: 09/23/2019
ms.topic: conceptual
ms.openlocfilehash: 562fa2378356ddc1eac48b6ea5c160ebf655d525
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74463514"
---
# <a name="evaluate-the-impact-of-a-new-azure-policy"></a>Yeni bir Azure ilkesinin etkisini değerlendirme

Azure İlkesi, Azure kaynaklarınızı iş standartlarına göre yönetmek ve uyumluluk gereksinimlerini karşılamak için güçlü bir araçtır. İnsanlar, işlemler veya ardışık işler kaynakları oluşturduğunda veya güncelleştirdiğinde, Azure İlkesi isteği gözden geçirir. İlke tanımı efekti [Append](./effects.md#deny) veya [DeployIfNotExists](./effects.md#deployifnotexists)olduğunda, İlke isteği değiştirir veya ekler. İlke tanımı efekti [Denetim](./effects.md#audit) veya [AuditIfNotExists](./effects.md#auditifnotexists)olduğunda, İlke bir Etkinlik günlüğü girişinin oluşturulmasına neden olur. Ve ilke tanımı etkisi [Reddet](./effects.md#deny)olduğunda, İlke isteği oluşturma veya değiştirme durur.

Bu sonuçlar, ilkenin doğru tanımlandığını bildiğinizde tam olarak istenilen sonuçlardır. Ancak, yeni bir ilkenin çalışmasını değiştirmesine veya engellemesine izin vermeden önce beklendiği gibi doğrulanması önemlidir. Doğrulama, yalnızca amaçlanan kaynakların uyumsuz olarak belirlendiği ve uyumlu kaynakların sonuçlara yanlış olarak dahil edilmemesini _(yanlış pozitif_olarak bilinir) sağlamalıdır.

Yeni bir ilke tanımını doğrulamak için önerilen yaklaşım aşağıdaki adımları izleyerek yapılır:

- İlkenizi sıkı bir şekilde tanımlayın
- Mevcut kaynaklarınızı denetle
- Yeni veya güncelleştirilmiş kaynak isteklerini denetleme
- İlkenizi kaynaklara dağıtın
- Sürekli izleme

## <a name="tightly-define-your-policy"></a>İlkenizi sıkı bir şekilde tanımlayın

İş ilkesinin bir ilke tanımı olarak nasıl uygulandığını ve Azure kaynaklarının diğer Azure hizmetleriyle ilişkisini anlamak önemlidir. Bu adım gereksinimleri [tanımlayarak](../tutorials/create-custom-policy-definition.md#identify-requirements) ve [kaynak özelliklerini belirleyerek](../tutorials/create-custom-policy-definition.md#determine-resource-properties)gerçekleştirilir.
Ancak iş politikanızın dar tanımının ötesini de görmek önemlidir. Politikanız örneğin "Tüm Sanal Makineler in..."bir durum ifade ediyor mu? HDInsight veya AKS gibi VM'lerden yararlanan diğer Azure hizmetleri ne olacak? Bir ilke tanımlarken, bu ilkenin diğer hizmetler tarafından kullanılan kaynakları nasıl etkilediğini göz önünde bulundurmalıyız.

Bu nedenle, ilke tanımlarınız mümkün olduğunca sıkı bir şekilde tanımlanmalı ve uyumluluk için değerlendirmeniz gereken kaynaklara ve özelliklere odaklanmalıdır.

## <a name="audit-existing-resources"></a>Varolan kaynakları denetleme

Yeni ilke tanımınızla yeni veya güncelleştirilmiş kaynakları yönetmeyi aramadan önce, test kaynak grubu gibi varolan kaynakların sınırlı bir alt kümesini nasıl değerlendirdığını görmek en iyisidir. Etkinverme veya etkinlik günlüğü girişleri oluşturulmasını önlemek [effect](./effects.md) için ilke atamanızda_Devre Dışı bırakılan_ [zorlama modunu](./assignment-structure.md#enforcement-mode)
(DoNotEnforce) kullanın.

Bu adım, iş akışını etkilemeden varolan kaynaklara ilişkin yeni ilkenin uyumluluk sonuçlarını değerlendirme şansı verir. Uyumlu kaynakların uyumlu olmayan _(yanlış pozitif)_ olarak işaretlenilip işaretlenilmediğini ve uyumlu olmamasını beklediğiniz tüm kaynakların doğru şekilde işaretlendiğini denetleyin.
Kaynakların ilk alt kümesi beklendiği gibi doğrulandıktan sonra, değerlendirmeyi yavaş yavaş varolan tüm kaynaklara genişletin.

Varolan kaynakların bu şekilde değerlendirilmesi, yeni ilkeğin tam olarak uygulanmasından önce uyumlu olmayan kaynakları düzeltmek için de bir fırsat sağlar. İlke tanımı efekti _DeployIfNotExists_ise bu temizleme el ile veya bir [düzeltme görevi](../how-to/remediate-resources.md) aracılığıyla yapılabilir.

## <a name="audit-new-or-updated-resources"></a>Yeni veya güncelleştirilmiş kaynakları denetleme

Yeni ilke tanımınızın varolan kaynaklar üzerinde doğru şekilde raporladığını doğruladıktan sonra, kaynaklar oluşturulduğunda veya güncelleştirildiğinde ipolitikasın etkisine bakma zamanı dır. İlke tanımı etki parametresini destekliyorsa, [Denetim'i](./effects.md#audit)kullanın. Bu yapılandırma, yeni ilke tanımının varolan çalışmaları veya istekleri etkilemeden uyumlu olmayan bir kaynak için Azure Etkinliği günlüğünde bir girişi tetikleyip tetiklediğini görmek için kaynakların oluşturulmasını ve güncellenmesini izlemenize olanak tanır.

_Denetim_ etkisinin beklendiği zaman doğru şekilde tetiklendiğini görmek için hem güncelleştirmeniz hem de ilke tanımınızla eşleşen yeni kaynaklar oluşturmanız önerilir. _Denetim_ efektini tetikleyen yeni ilke tanımından etkilenmemesi gereken kaynak isteklerini gözetleyin.
Bu etkilenen kaynaklar yanlış _pozitif_ başka bir örnektir ve tam uygulamadan önce ilke tanımında sabit olmalıdır.

İlke tanımının sınamanın bu aşamasında değiştirilmesi durumunda, doğrulama işleminin varolan kaynakların denetlenmesiyle birlikte başlatılması önerilir. Yeni veya güncelleştirilmiş kaynaklar üzerinde _yanlış_ pozitif için ilke tanımında yapılan bir değişikliğin varolan kaynaklar üzerinde de etkisi olabilir.

## <a name="deploy-your-policy-to-resources"></a>İlkenizi kaynaklara dağıtın

Yeni ilke tanımınızın doğrulanmasını hem varolan kaynaklarla hem de yeni veya güncelleştirilmiş kaynak istekleriyle tamamladıktan sonra, ilkeyi uygulama işlemine başlarsınız. Yeni ilke tanımı için ilke atamasının önce kaynak grubu gibi tüm kaynakların bir alt kümesine oluşturulması önerilir. İlk dağıtımı doğruladıktan sonra, ilkenin kapsamını abonelikler ve yönetim grupları gibi daha geniş ve daha geniş düzeylere genişletin. Bu genişletme, atamakaldırılarak ve yeni ilke tanımınız tarafından karşılanması amaçlanan kaynakların tam kapsamına atanana kadar hedef kapsamlarda yeni bir atama oluşturularak elde edilir.

Kullanıma alma sırasında, yeni ilke tanımınızdan muaf olması gereken kaynaklar bulunursa, bunları aşağıdaki yollardan biriyle ele alın:

- İstenmeyen etkiyi azaltmak için ilke tanımını daha açık olması için güncelleştirme
- İlke atamasının kapsamını değiştirme (yeni bir atamayı kaldırıp oluşturarak)
- İlke ataması için kaynak grubunu dışlama listesine ekleme

Kapsamdaki herhangi bir değişiklik (düzey veya hariç tutmalar) kapsama alanında boşluk olmadığından emin olmak için güvenlik ve uyumluluk kuruluşlarınızla tam olarak doğrulanmalı ve iletilmelidir.

## <a name="monitor-your-policy-and-compliance"></a>Politikanızı ve uyumluluğunuzu izleyin

İlke tanımınızı uygulamak ve atamak son adım değildir. Yeni ilke tanımınıza uygun kaynakların [uyumluluk](../how-to/get-compliance-data.md) düzeyini sürekli olarak izleyin ve uyumlu olmayan aygıtlar tanımlandığında uygun [Azure Monitor uyarıları ve bildirimleri](../../../azure-monitor/platform/alerts-overview.md) ayarlayın. Ayrıca, ilke tanımının iş ilkesi ve uyumluluk gereksinimlerini karşıladığını doğrulamak için ilke tanımının ve ilgili atamaların zamanlanmış olarak değerlendirilmesi önerilir. Artık gerekmese ilkeler kaldırılmalıdır. Temel Azure kaynakları geliştikçe ve yeni özellikler ve özellikler ekledikçe, ilkelerin de zaman zaman güncelleştirilmesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [İlke tanımı yapısı](./definition-structure.md)hakkında bilgi edinin.
- [İlke atama yapısı](./assignment-structure.md)hakkında bilgi edinin.
- [İlkeleri programlı bir şekilde nasıl oluşturlayacağımı](../how-to/programmatically-create.md)anlayın.
- Uyumluluk verilerini nasıl [alacağınızı](../how-to/get-compliance-data.md)öğrenin.
- [Uyumlu olmayan kaynakları](../how-to/remediate-resources.md)nasıl düzelteriz öğrenin.
- [Azure yönetim gruplarıyla kaynaklarınızı düzenleyin](../../management-groups/overview.md)ile yönetim grubunun ne olduğunu gözden geçirin.