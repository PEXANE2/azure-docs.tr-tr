---
title: İlkeyi Kod iş akışları olarak tasarklama
description: Azure İlkesi tanımlarınızı kod olarak dağıtmak ve kaynakları otomatik olarak doğrulamak için iş akışları tasarlamayı öğrenin.
ms.date: 11/04/2019
ms.topic: conceptual
ms.openlocfilehash: 0914ba6510c9d2ef87d3f83417f97340d42c8bce
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74267273"
---
# <a name="design-policy-as-code-workflows"></a>İlkeyi Kod iş akışları olarak tasarklama

Bulut Yönetişim ile yolculuğunuzsırasında, Azure portalındaki her ilke tanımını el ile yönetmekten veya çeşitli SDK'lar aracılığıyla kurumsal ölçekte daha yönetilebilir ve tekrarlanabilir bir şeye geçmek istersiniz. Bulutta ölçekte sistemleri yönetmeye yönelik baskın yaklaşımlardan ikisi şunlardır:

- Kod Olarak Altyapı: Kaynak Yöneticisi şablonlarından Azure İlkesi tanımlarına ve Azure Planları'na kadar ortamlarınızı tanımlayan içeriği kaynak kodu olarak ele alma uygulaması.
- DevOps: Son kullanıcılarımıza sürekli değer sunulmasını sağlamak için insan, süreç ve ürünlerin birleşimi.

Kod olarak politika bu fikirlerin birleşimidir. Esasen, ilke tanımlarınızı kaynak denetiminde tutun ve ne zaman bir değişiklik yapıldığında, bu değişikliği test edin ve doğrulayın. Ancak, bu kod veya DevOps olarak Altyapı ile ilgili politikaların ölçüde olmamalıdır.

Doğrulama adımı, diğer sürekli tümleştirmeveya sürekli dağıtım iş akışlarının da bir bileşeni olmalıdır. Örnekler arasında uygulama ortamının veya sanal altyapının dağıtılması verilebilir. Azure İlkesi doğrulaması yapı ve dağıtım işleminin erken bir bileşeni haline getirerek uygulama ve operasyon ekipleri, değişikliklerinin çok geç olmadan ve üretimde dağıtmaya çalışmadan çok önce şikayet edilip edilemediklerini keşfeder.

## <a name="workflow-overview"></a>İş akışına genel bakış

Kod olarak İlkenin önerilen genel iş akışı aşağıdaki diyagrama benzer:

![Kod iş akışına genel bakış olarak ilke](../media/policy-as-code/policy-as-code-workflow.png)

### <a name="create-and-update-policy-definitions"></a>İlke tanımlarını oluşturma ve güncelleştirme

İlke tanımları JSON kullanılarak oluşturulur ve kaynak denetiminde depolanır. Her ilke, aynı klasörde depolanmalıdır parametreler, kurallar ve ortam parametreleri gibi kendi dosya kümesi vardır. Aşağıdaki yapı, ilke tanımlarınızı kaynak denetiminde tutmanın önerilen bir yoludur.

```text
.
|
|- policies/  ________________________ # Root folder for policies
|  |- policy1/  ______________________ # Subfolder for a policy
|     |- policy.json _________________ # Policy definition
|     |- policy.parameters.json ______ # Policy definition of parameters
|     |- policy.rules.json ___________ # Policy rule
|     |- params.dev.json _____________ # Parameters for a Dev environment
|     |- params.prd.json _____________ # Parameters for a Prod environment
|     |- params.tst.json _____________ # Parameters for a Test environment
|
|  |- policy2/  ______________________ # Subfolder for a policy
|     |- policy.json _________________ # Policy definition
|     |- policy.parameters.json ______ # Policy definition of parameters
|     |- policy.rules.json ___________ # Policy rule
|     |- params.dev.json _____________ # Parameters for a Dev environment
|     |- params.prd.json _____________ # Parameters for a Prod environment
|     |- params.tst.json _____________ # Parameters for a Test environment
|
```

Yeni bir ilke eklendiğinde veya varolan bir ilke güncellendiğinde, iş akışı Azure'daki ilke tanımını otomatik olarak güncelleştirmelidir. Yeni veya güncelleştirilmiş ilke tanımının sınant›r›lmas› daha sonraki bir ad›m da gelir.

### <a name="create-and-update-initiative-definitions"></a>Girişim tanımlarını oluşturma ve güncelleştirme

Aynı şekilde, girişimlerin kendi JSON dosyası ve aynı klasörde depolanmalıdır ilgili dosyaları var. Girişim tanımı, ilke tanımının zaten var olmasını gerektirir, bu nedenle ilkenin kaynağı kaynak denetiminde güncelleştirilip Azure'da güncelleştirilmeden oluşturulamaz veya güncelleştirilemez. Aşağıdaki yapı, girişim tanımlarınızı kaynak denetiminde tutmanın önerilen bir yoludur:

```text
.
|
|- initiatives/ ______________________ # Root folder for initiatives
|  |- init1/ _________________________ # Subfolder for an initiative
|     |- policyset.json ______________ # Initiative definition
|     |- policyset.definitions.json __ # Initiative list of policies
|     |- policyset.parameters.json ___ # Initiative definition of parameters
|     |- params.dev.json _____________ # Parameters for a Dev environment
|     |- params.prd.json _____________ # Parameters for a Prod environment
|     |- params.tst.json _____________ # Parameters for a Test environment
|
|  |- init2/ _________________________ # Subfolder for an initiative
|     |- policyset.json ______________ # Initiative definition
|     |- policyset.definitions.json __ # Initiative list of policies
|     |- policyset.parameters.json ___ # Initiative definition of parameters
|     |- params.dev.json _____________ # Parameters for a Dev environment
|     |- params.prd.json _____________ # Parameters for a Prod environment
|     |- params.tst.json _____________ # Parameters for a Test environment
|
```

İlke tanımları gibi, varolan bir girişimi eklerken veya güncellerken, iş akışı Azure'daki girişim tanımını otomatik olarak güncelleştirmelidir. Yeni veya güncelleştirilmiş girişim tanımının sınant›r›lmas› daha sonraki bir ad›m da gelir.

### <a name="test-and-validate-the-updated-definition"></a>Güncelleştirilmiş tanımı test edin ve doğrulayın

Otomasyon, yeni oluşturulan veya güncellenen ilke veya girişim tanımlarınızı aldıktan ve Azure'da nesneye güncelleştirme yaptıktan sonra, yapılan değişiklikleri test etme zamanı gelmiştir. Ya politika ya da bir parçası olduğu girişim(ler) daha sonra üretimden uzak çevredeki kaynaklara atanmalıdır. Bu ortam genellikle _Dev_Dev'dir.

Atama, kaynak oluşturma ve güncelleştirmelerin engellenmemesi, ancak güncelleştirilmiş ilke tanımına uygunluk için varolan kaynakların yine de denetlenmesi için _devre dışı bırakma_ [modunu](./assignment-structure.md#enforcement-mode) kullanmalıdır. enforcementMode'da bile, atama kapsamının bir kaynak grubu veya ilkeleri doğrulamak için özel olarak kullanılan bir abonelik olması önerilir.

> [!NOTE]
> Zorlama modu yararlı olsa da, çeşitli koşullar altında bir ilke tanımını kapsamlı bir şekilde sınamak için bir yedek değildir. İlke tanımı ile `PUT` test `PATCH` edilmelidir ve REST API aramaları, uyumlu ve uyumlu olmayan kaynaklar ve kaynaktan eksik bir özellik gibi kenar durumları.

Atama dağıtıldıktan sonra, yeni atama için [uyumluluk verilerini almak](../how-to/get-compliance-data.md) için İlke SDK'sını kullanın. İlkeleri ve atamaları sınamak için kullanılan ortam hem uyumlu hem de uyumlu olmayan kaynaklara sahip olmalıdır. Kod için iyi bir birim testi gibi, kaynakların beklendiği gibi olduğunu ve yanlış pozitif veya yanlış negatif olmadığını da test etmek istersiniz. Yalnızca beklediğiniz için test ve doğrulama, ilke beklenmeyen ve tanımlanamayan etkisi olabilir. Daha fazla bilgi için [bkz.](./evaluate-impact.md)

### <a name="enable-remediation-tasks"></a>Düzeltme görevlerini etkinleştirme

Atamanın doğrulanması beklentileri karşılıyorsa, bir sonraki adım düzeltmeyi doğrulamaktır.
[DeployIfNotExists](./effects.md#deployifnotexists) veya [değiştirimi](./effects.md#modify) kullanan ilkeler bir düzeltme görevine dönüştürülebilir ve uyumlu olmayan bir durumdan kaynakları doğrulayabilir.

Bunu yapmanın ilk adımı, ilke atamasına ilke tanımında tanımlanan rol atamasını vermektir. Bu rol atama, ilke ataması yönetilen kimlik kaynağı uyumlu hale getirmek için gerekli değişiklikleri yapmak için yeterli haklar verir.

İlke ataması uygun haklara sahip olduktan sonra, uyumlu olmadığı bilinen bir dizi kaynağa karşı düzeltme görevi tetiklemek için İlke SDK'sını kullanın. Devam etmeden önce bu düzeltilen görevlere karşı üç test tamamlanmalıdır:

- Düzeltme görevinin başarıyla tamamlandığını doğrulama
- İlke uyumluluğu sonuçlarının beklendiği gibi güncelleştirdiğini görmek için ilke değerlendirmesini çalıştırın
- Özelliklerinin değiştiğini doğrulamak için kaynaklara karşı bir ortam birimi testi çalıştırma

Hem güncelleştirilmiş ilke değerlendirme sonuçlarının hem de ortamın test edilmesi, düzeltme görevlerinin bekleneni değiştirdiği ve ilke tanımının uyumluluk değişikliğini beklendiği gibi gördüğü ne doğrulmaktadır.

### <a name="update-to-enforced-assignments"></a>Zorlanan atamalara güncelleştirme

Tüm doğrulama kapıları tamamlandıktan sonra, _etkin_ **enforcementMode** kullanmak için atama güncelleştirin. Bu değişiklik başlangıçta üretimden uzak aynı ortamda yapılmalıdır. Bu ortam beklendiği gibi çalıştığı doğrulandıktan sonra, ilke üretim kaynaklarına dağıtılana kadar değişikliğin bir sonraki ortamı ve benzeri nidurum içerecek şekilde kapsamı nın kapsamı gerekir.

## <a name="process-integrated-evaluations"></a>Süreç entegre değerlendirmeleri

Kod olarak İlken'in genel iş akışı, politika ve girişimleri ölçekte bir ortama geliştirmek ve dağıtmak içindir. Ancak, ilke değerlendirmesi, Azure'da uygulamaları dağıtma veya altyapı oluşturmak için Kaynak Yöneticisi şablonları çalıştırma gibi kaynakları dağıtan veya oluşturan tüm iş akışları için dağıtım işleminin bir parçası olmalıdır.

Bu gibi durumlarda, uygulama veya altyapı dağıtımı bir test aboneliğine veya kaynak grubuna yapıldıktan sonra, varolan tüm ilke ve girişimlerin doğrulanmasını denetleyen kapsam için ilke değerlendirmesi yapılmalıdır. Bu tür bir ortamda **enforcementMode** _devre dışı_ bırakılmış olarak yapılandırılabilir, ancak bir uygulama veya altyapı dağıtımının ilke tanımlarını erken ihlal edip etmediklerini önceden bilmek yararlıdır. Bu nedenle, bu ilke değerlendirmesi bu iş akışlarında bir adım olmalı ve uyumlu olmayan kaynaklar oluşturan dağıtımları başarısız lığa uğratmalıdır.

## <a name="review"></a>Gözden geçirme

Bu makalede, Kod olarak İlke için genel iş akışı ve ayrıca ilke değerlendirmesinin diğer dağıtım iş akışlarının bir parçası olması gerektiği yer yer almaktadır. Bu iş akışı, komut dosyası adımlarını ve tetikleyicilere dayalı otomasyonu destekleyen her ortamda kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [İlke tanımı yapısı](./definition-structure.md)hakkında bilgi edinin.
- [İlke atama yapısı](./assignment-structure.md)hakkında bilgi edinin.
- [İlkeleri programlı bir şekilde nasıl oluşturlayacağımı](../how-to/programmatically-create.md)anlayın.
- Uyumluluk verilerini nasıl [alacağınızı](../how-to/get-compliance-data.md)öğrenin.
- [Uyumlu olmayan kaynakları](../how-to/remediate-resources.md)nasıl düzelteriz öğrenin.
- [Azure yönetim gruplarıyla kaynaklarınızı düzenleyin](../../management-groups/overview.md)ile yönetim grubunun ne olduğunu gözden geçirin.