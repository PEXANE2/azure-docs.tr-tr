---
title: Azure Ilkesi ve Azure şemaları ile güvenlik uyumluluğu
description: Azure Ilkesi ve Azure Ilkeleri ile güvenliği zorunlu hale getirerek, bu, ASD RONIZLE ve gerekli 8 ' e ait olan Avustralya kamu kurumları için Azure şemaları
author: galey801
ms.service: azure-australia
ms.topic: conceptual
ms.date: 07/22/2019
ms.author: grgale
ms.openlocfilehash: 0eb3e669f622a9c09c903530cbbab19206ce227f
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68571750"
---
# <a name="security-compliance-with-azure-policy-and-azure-blueprints"></a>Azure Ilkesi ve Azure şemaları ile güvenlik uyumluluğu

BT ortamınızda idare zorlama, şirket içi, bulut Yerel veya karma bir ortam olmak üzere tüm organizasyonlar için mevcut olup olmadığı. Microsoft Azure ortamınızın tasarım, düzenleme ve güvenlik gereksinimleriyle uyumlu olduğundan emin olmak için sağlam bir teknik idare çerçevesi olması gerekir.

Avustralya kamu kurumları için, riski değerlendirmek için önemli denetimleri [Avustralya Cyber Güvenlik Merkezi (ACSC) bilgi güvenliği el ile](https://acsc.gov.au/infosec/ism/index.htm) (ISM) göz önünde bulundurmaları gerekir. ISM dahilinde ayrıntılandırılan denetimlerin çoğu, teknik idare uygulamasının etkili bir şekilde yönetilmesini ve zorlanmasını gerektirir. Ortamlarınızdaki yapılandırmayı değerlendirmek ve zorlamak için uygun araçlara sahip olmanız önemlidir.

Microsoft Azure, bu zorluk, Azure Ilkesi ve Azure şemaları konusunda yardımcı olmak için iki adet ücretsiz hizmet sağlar.

## <a name="azure-policy"></a>Azure İlkesi

Azure Ilkesi, bir kuruluşun BT yönetimi 'nin teknik öğelerinin uygulamasına izin vermez. Azure Ilkesi, sürekli büyüyen bir yerleşik ilke kitaplığı içerir. Her Ilke hedeflenen Azure kaynaklarında kuralları ve etkileri zorlar.

Bir ilke kaynaklara atandıktan sonra, bu ilkeye karşı genel uyumluluk değerlendirilebilmelidir ve gerekirse düzeltilebilir.

Yerleşik Azure Ilkeleri kitaplığı, kuruluşların ACSC ıSM içinde bulunan denetim türlerini hızla zorlayabilmenizi sağlar. Denetim örnekleri şunları içerir:

* Eksik sistem güncelleştirmeleri için sanal makineleri izleme
* Multi-Factor Authentication için yükseltilmiş izinlere sahip hesapları denetleme
* Şifrelenmemiş SQL veritabanlarını tanımlama
* Özel Azure rol tabanlı erişim denetimi (RBAC) kullanımını izleme
* Kaynakların oluşturulabildiğiniz Azure bölgelerini kısıtlama

İdare veya mevzuat denetimleri yerleşik bir Azure Ilke tanımıyla karşılanmazsa, özel bir tanım oluşturulabilir ve atanabilir. Tüm Azure Ilke tanımları JSON 'da tanımlanır ve standart [tanım yapısını](https://docs.microsoft.com/azure/governance/policy/concepts/definition-structure)izler. Mevcut Azure Ilke tanımları da yinelenebilir ve özel bir Ilke tanımının temelini oluşturmak için kullanılabilir.

Özellikle karmaşık ortamlarda veya sıkı düzenleme gereksinimlerine sahip ortamlarda, kaynaklara ayrı Azure Ilkeleri atama, yöneticileriniz için büyük ek yük oluşturabilir. Bu güçlüklere yardımcı olmak için bir Azure Ilkeleri kümesi bir Azure Ilke girişimi oluşturmak üzere birlikte gruplandırılabilir. İlke girişimleri, bir grup olarak birlikte uygulandığında, belirli bir güvenlik veya uyumluluk hedefinin temelini oluşturan ilgili Azure ilkelerini birleştirmek için kullanılır. Microsoft, belirli yasal gereksinimleri karşılamak için tasarlanan tanımlar dahil olmak üzere yerleşik Azure Ilke girişim tanımları ekliyor:

![Mevzuat uyumluluk Ilkesi girişimleri](media/regulatory-initiatives.png)

Tüm Azure Ilkeleri ve girişimleri atama kapsamına atanır. Bu kapsam, Azure aboneliği, Azure yönetim grubu veya Azure Kaynak grubu düzeylerinde tanımlanmıştır. Gerekli Azure Ilkeleri veya Ilke girişimleri atandıktan sonra, bir organizasyon, yeni oluşturulan tüm Azure kaynaklarında yapılandırma gereksinimlerini zorunlu kılabilir.

Yeni bir Azure Ilkesi veya girişimi atanması, mevcut Azure kaynaklarını etkilemez. Azure Ilkesi,; Bununla birlikte, mevcut Azure kaynaklarının uyumluluğunu görüntülemek için bir organizasyon etkinleştirin. Uyumsuz olarak tanımlanmış olan kaynaklar, kuruluşun kararına göre düzeltilebilir

### <a name="azure-policy-and-initiatives-in-action"></a>Azure Ilkesi ve girişimleri eylemde

Kullanılabilir yerleşik Azure Ilkesi ve girişim tanımları, Azure portal Ilke bölümündeki tanım düğümünün altında bulunabilir:

![Yerleşik Azure Ilke tanımları](media/policy-definitions.png)

Yerleşik tanımlar kitaplığını kullanarak, bir organizasyon gereksinimini karşılayan Ilkeleri hızlı bir şekilde arayabilir, ilke tanımını inceleyebilir ve Ilkeyi uygun kaynaklara atayabilirsiniz. Örneğin, ıSM tüm ayrıcalıklı kullanıcılar için çok faktörlü kimlik doğrulaması (MFA) ve tüm kullanıcıların önemli veri depolarına erişimi olması gerekir. Azure Ilkesinde, Azure Ilke tanımları arasında "MFA" araması yapabilirsiniz:

![Azure MFA Ilkeleri](media/mfa-policies.png)

Uygun bir ilke tanımlandıktan sonra, ilkeyi istenen kapsama atarsınız. Gereksinimlerinizi karşılayan bir yerleşik ilke yoksa, mevcut ilkeyi çoğaltabilir ve istediğiniz değişiklikleri yapabilirsiniz:

![Yinelenen mevcut Azure Ilkesi](media/duplicate-policy.png)

Microsoft ayrıca özel Azure Ilkeleri oluşturmanız için [GitHub](https://github.com/Azure/azure-policy) 'Da Azure ilke örnekleri koleksiyonu ' hızlı başlangıç ' olarak sunulur. Bu Ilke örnekleri, doğrudan Azure portal içindeki Azure Ilke düzenleyicisine kopyalanabilir.

Azure Ilke girişimlerini oluştururken, hem yerleşik hem de özel olan kullanılabilir ilke tanımlarının listesini, gerekli tanımları ekleyerek sıralayabilirsiniz.

Örneğin, Windows sanal makinelerle ilgili tüm ilkeler için kullanılabilir Azure Ilke tanımları listesinde arama yapabilirsiniz. Ardından, önerilen sanal makine sağlamlaştırma uygulamalarını zorlamak için tasarlanan bir girişim için bu tanımları kullanabilirsiniz:

![Azure Ilkelerinin listesi](media/initiative-definitions.png)

Atama kapsamına bir Azure Ilkesi veya Ilke girişimi atarken, Azure Yönetim Grupları veya Azure kaynak gruplarını dışlayarak Azure kaynaklarını Ilkelerin etkileriyle hariç tutmak mümkündür.

### <a name="real-time-enforcement-and-compliance-assessment"></a>Gerçek zamanlı zorlama ve uyumluluk değerlendirmesi

Aşağıdaki koşullar karşılandığında, kapsamdaki Azure kaynakları için Azure Ilkesi uyumluluk taramaları alınır:

* Bir Azure Ilkesi veya Azure Ilke girişimi atandığında
* Mevcut bir Azure Ilkesi veya girişim kapsamı değiştirildiğinde
* API aracılığıyla saat başına en fazla 10 tarama
* Her 24 saatte bir-varsayılan davranış

Kaynak üzerinde bir değişiklik yapıldıktan sonra tek bir Azure kaynağı için ilke uyumluluk taraması 15 dakika sürer.

Kaynakların Azure Ilkesi uyumluluğuna genel bakış, Ilke uyumluluk panosu aracılığıyla Azure portal içinde gözden geçirilebilir:

![Azure Ilkesi Uyumluluk puanı](media/simple-compliance.png)

Genel kaynak uyumluluğu yüzdesi şekil, tüm kapsamta dağıtılan kaynakların tüm atanan Azure Ilkelerinize karşı uyumluluğunun toplamıdır. Bu, uyumlu olmayan bir ortamdaki kaynakları tanımlamanızı ve bu kaynakları en iyi şekilde düzeltmeye yönelik planı devlemenizi sağlar.

Ilke uyumluluk panosu her kaynak için değişiklik geçmişini de içerir. Bir kaynak, atanan ilkeyle uyumlu değil olarak tanımlanmışsa ve otomatik düzeltme etkin değilse, değişikliği kimin yaptığını, nelerin değiştirildiğini ve bu kaynakta yapılan değişikliklerin ne zaman yapıldığını görüntüleyebilirsiniz.

## <a name="azure-blueprints"></a>Azure Blueprints

Azure şemaları, Azure Ilke özelliğini ile birleştirerek genişletir:

* Azure RBAC
* Azure Kaynak grupları
* [Azure Resource Manager şablonları](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates)

Planlar, Azure kaynaklarını Kaynak Yöneticisi şablonlarından dağıtan ortam tasarımlarının oluşturulmasına, RBAC yapılandırmasına ve Azure Ilkesi atayarak yapılandırmayı zorlamaya ve denetlemeye izin verir. Planlar düzenlenebilir ve yeniden dağıtılabilir bir ortam şablonu oluşturur. Blueprint oluşturulduktan sonra bir Azure aboneliğine atanabilir. Atandıktan sonra, Blueprint içinde tanımlanan tüm Azure kaynakları oluşturulur ve Azure Ilkeleri uygulanır. Azure Blueprint tanımlanan kaynakların dağıtımı ve yapılandırması, Azure portal Azure Blueprint konsolundan izlenebilir.

Düzenlenmiş Azure şemaları Azure portal yeniden yayımlanmalıdır. Bir Blueprint her yeniden yayımlandığında, BLUEPRINT sürüm numarası artırılır. Sürüm numarası, bir Blueprint 'in bir Azure aboneliğine hangi belirli sürümünün dağıtıldığını belirlemenizi sağlar. İsterseniz, şu anda atanmış olan Blueprint sürümü en son sürüme güncelleştirilebilen olabilir.

Bir Azure Blueprint kullanılarak dağıtılan kaynaklar, dağıtım sırasında [Azure kaynak kilitleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources) ile yapılandırılabilir. Kaynak kilitleri, kaynakların yanlışlıkla değiştirilmesini veya silinmesini engeller.

Microsoft, bir dizi sektör ve mevzuata gereksinimi için Azure Blueprint şablonları geliştirmektedir. Kullanılabilir Azure Blueprint tanımlarının geçerli kitaplığı, hizmet güven portalındaki Azure portal veya [Azure Güvenlik ve uyumluluk şeması](https://servicetrust.microsoft.com/ViewPage/BlueprintOverview/) sayfasında görüntülenebilir.

### <a name="azure-blueprint-artifacts"></a>Azure Blueprint yapıtlar

Bir Azure Blueprint oluşturmak için boş bir Blueprint şablonuyla başlayabilir ya da bir başlangıç noktası olarak mevcut örnek bir taslaklara birini kullanabilirsiniz. Dağıtımın bir parçası olarak yapılandırılacak, Blueprint öğesine yapıt ekleyebilirsiniz:

![Azure Blueprint yapıtlar](media/blueprint-artifacts.png)

Bu yapıtlar, ortamınız için gereken yapılandırmayı zorunlu kılmak üzere Azure Kaynak grubu ve kaynakları ile ilişkili Azure Ilkesi ve Ilke girişimlerini içerebilir; Örneğin, ıSM denetimleri gibi düzenleme gereksinimleriniz Sistem sağlamlaştırma için.

Bu yapıtların her biri parametrelerle de yapılandırılabilir. Bu değerler, Blueprint bir Azure aboneliğine atandığında ve dağıtıldığında sağlanır. Parametreler, temel Blueprint 'i düzenlemeye gerek kalmadan, kaynakları farklı ortamlara dağıtmak için tek bir Blueprint 'in oluşturulmasını ve kullanılmasını sağlar.

Microsoft, Blueprint 'in bir CI/CD işlem hattı aracılığıyla korunabilir ve dağıtılabilmesi amacıyla Azure şemaları oluşturup yönetmek için Azure PowerShell ve CLı cmdlet 'lerini geliştirmektedir.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Ilkesi ve Azure şemaları ile idare ve güvenliğin nasıl zorlanacağını anlatılmıştı. Artık yüksek düzeyde kullanıma sunuldığınıza göre, her bir hizmeti daha ayrıntılı olarak kullanmayı öğrenin:

* [Azure Ilkesine genel bakış](https://docs.microsoft.com/azure/governance/policy/overview)
* [Azure şemaları genel bakış](https://azure.microsoft.com/services/blueprints/)
* [Azure İlkesi Örnekleri](https://docs.microsoft.com/azure/governance/policy/samples/index)
* [Azure Ilke örnekleri deposu](https://github.com/Azure/azure-policy)
* [Azure Ilke tanımı yapısı](https://docs.microsoft.com/azure/governance/policy/concepts/definition-structure)
* [Azure Ilke etkileri](https://docs.microsoft.com/azure/governance/policy/concepts/effects)
