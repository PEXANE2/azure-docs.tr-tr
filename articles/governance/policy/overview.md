---
title: Azure İlkesine Genel Bakış
description: Azure İlkesi, Azure ortamında ilke tanımlarınızı oluşturmak, atamak ve yönetmek için kullandığınız bir Azure hizmetidir.
author: DCtheGeek
ms.author: dacoulte
ms.date: 12/06/2018
ms.topic: overview
ms.service: azure-policy
ms.openlocfilehash: 46d78ca9f82017e1a11642af6e5dcdc68e239c8a
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73498265"
---
# <a name="overview-of-the-azure-policy-service"></a>Azure Ilke hizmetine genel bakış

İdare, kuruluşunuzun amaçlarını etkili ve verimli bir şekılde kullanarak sağlayacağını doğrular. İş hedefleri ve BT projeleri arasında netlik oluşturarak bu ihtiyacı karşılar.

Şirketiniz asla çözülmeyecek gibi görünen önemli sayıda BT sorunlarıyla mi karşılaşıyor?
İyi bir BT yönetimine sorunların yönetilmesine ve önlenmesine yardımcı olma amacıyla girişimlerinizi planlama ve önceliklerinizi stratejik düzeyde belirleme dahildir. Bu stratejik gereksinim, Azure Ilkesinin geldiği yerdir.

Azure İlkesi, ilkelerinizi oluşturmak, atamak ve yönetmek için kullandığınız bir Azure hizmetidir. Bu ilkeler, kaynaklarınız üzerinden farklı kuralları ve eylemleri uygulatarak kaynaklarınızın kurumsal standartlarınız ve hizmet düzeyi sözleşmelerinizle uyumlu kalmasını sağlar. Azure Ilkesi, kaynakları atanan ilkelerle uyumsuz olarak değerlendirerek bu ihtiyacı karşılar. Örneğin, ortamınızda yalnızca belirli SKU boyutuna sahip sanal makinelere izin veren bir ilkeniz olabilir. Bu ilke uygulandıktan sonra, yeni ve mevcut kaynaklar uyumluluk için değerlendirilir. Doğru ilke türü ile, mevcut kaynaklar uyumluluk halinde getirilebilir. Bu belgelerde daha sonra, Azure Ilkesiyle ilke oluşturma ve uygulama hakkında daha fazla ayrıntıyı inceleyeceğiz.

> [!IMPORTANT]
> Azure İlkesi’nin uyumluluk değerlendirmesi artık fiyatlandırma katmanından bağımsız olarak tüm atamalara sağlanır. Atamalarınız uyumluluk verilerini göstermezse, lütfen aboneliğin Microsoft.PolicyInsights kaynak sağlayıcısı ile kaydolduğundan emin olun.

[!INCLUDE [service-provider-management-toolkit](../../../includes/azure-lighthouse-supported-service.md)]

## <a name="how-is-it-different-from-rbac"></a>RBAC ile farkları nelerdir?

Azure Ilkesi ve rol tabanlı erişim denetimi (RBAC) arasında bazı önemli farklılıklar vardır. RBAC farklı kapsamlardaki kullanıcı eylemlerine odaklanır. Kaynak grubu için katkıda bulunan rolüne eklenmiş olabilirsiniz ve bu kaynak grubunda değişiklik yapma olanağı sağlar. Azure Ilkesi, dağıtım sırasında ve zaten var olan kaynaklar için kaynak özelliklerine odaklanır. Azure Ilkesi, kaynakların türleri veya konumları gibi özellikleri denetler. RBAC 'den farklı olarak Azure Ilkesi, varsayılan bir izin verme ve açık reddetme sistemidir.

### <a name="rbac-permissions-in-azure-policy"></a>Azure İlkesi'ndeki RBAC İzinleri

Azure İlkesi iki Kaynak Sağlayıcısı’nda işlemler olarak bilinen bazı izinlere sahiptir:

- [Microsoft.Authorization](../../role-based-access-control/resource-provider-operations.md#microsoftauthorization)
- [Microsoft.PolicyInsights](../../role-based-access-control/resource-provider-operations.md#microsoftpolicyinsights)

Birçok Yerleşik rol Azure İlkesi kaynaklarına izin verir. **Kaynak Ilkesi katkıda bulunan (Önizleme)** rolü çoğu Azure ilke işlemini içerir. **Sahibinin** tam hakları vardır. **Katkıda bulunan** ve **Reader** tüm Azure ilkesi Işlemlerini kullanabilir, ancak **katkıda bulunan** düzeltme de tetiklenebilir.

Yerleşik rollerin hiçbirinde gerekli izinler yoksa [özel rol](../../role-based-access-control/custom-roles.md) oluşturun.

## <a name="policy-definition"></a>İlke tanımı

Azure İlkesi'nde bir ilke oluşturmak ve uygulamak için önce ilke tanımını oluşturmanız gerekir. Her ilke tanımında, üzerinde zorlanan koşullar vardır. Ve koşullar karşılanıyorsa, tanımlanmış bir etkiye sahiptir.

Azure Ilkesinde, varsayılan olarak kullanılabilen birkaç yerleşik ilke sunuyoruz. Örneğin:

- **Izin verilen depolama hesabı SKU 'ları**: dağıtılan bir depolama HESABıNıN bir SKU boyutları kümesi içinde olup olmadığını belirler. Etkisi, tanımlı SKU boyutları kümesine bağlı olmayan tüm depolama hesaplarını reddetmektir.
- **Izin verilen kaynak türü**: dağıtabileceğiniz kaynak türlerini tanımlar. Etkisi, bu tanımlı listenin parçası olmayan tüm kaynakları reddetmektir.
- **Izin verilen konumlar**: yeni kaynaklar için kullanılabilir konumları kısıtlar. Sahip olduğu eylem ise coğrafi uyumluluk gereksinimlerinizi uygulamaktır.
- **Izin verilen sanal makine SKU 'ları**: dağıtabileceğiniz bir sanal makine SKU 'su kümesi belirtir.
- **Kaynaklara bir etiket ekleyin**: dağıtım isteği tarafından belirtilmemişse gerekli bir etiketi ve varsayılan değerini uygular.
- **Etiketi ve değerini zorla**: gerekli bir etiketi ve değerini bir kaynağa zorlar.
- **İzin verilmeyen kaynak türleri**: bir kaynak türleri listesinin dağıtılmasını engeller.

Bu ilke tanımlarını uygulamak için (hem yerleşik hem de özel tanımlar), bunları atamanız gerekir. Bu ilkelerden herhangi birini Azure portalı, PowerShell veya Azure CLI üzerinden atayabilirsiniz.

İlke değerlendirmesi, ilke ataması veya ilke güncelleştirmeleri gibi çeşitli farklı eylemlerle gerçekleştirilir. Tüm liste için bkz. [ilke değerlendirme Tetikleyicileri](./how-to/get-compliance-data.md#evaluation-triggers).

İlke tanımlarının yapıları hakkında daha fazla bilgi edinmek için [İlke Tanımı Yapısı](./concepts/definition-structure.md) adlı makaleye göz atın.

## <a name="policy-assignment"></a>İlke ataması

İlke ataması, belirli bir kapsamda gerçekleşmesi için atanmış bir ilke tanımıdır. Bu kapsamın dahilinde [yönetim gruplarından](../management-groups/overview.md) kaynak gruplarına kadar birçok grup bulunabilir. *Kapsam*, ilke tanımının atandığı tüm kaynak gruplarını, abonelikleri veya yönetim gruplarını ifade eder. İlke atamaları, tüm alt kaynaklar tarafından devralınır. Bu tasarım, kaynak grubuna uygulanan bir ilkenin bu kaynak grubundaki kaynaklara da uygulanacağını gösterir. Ancak, dilerseniz bir alt kapsamı ilke atamasından dışlayabilirsiniz.

Örneğin abonelik kapsamında, ağ kaynaklarının oluşturulmasını önleyen bir ilke atayabilirsiniz. Ağ altyapısı için tasarlanan bir kaynak grubunu bu abonelikte dışlayabilirsiniz. Daha sonra, ağ kaynakları oluşturma ile güvendiğiniz kullanıcılara bu ağ kaynak grubuna erişim izni verirsiniz.

Başka bir örnekte, yönetim grubu düzeyinde liste ilkesine izin ver kaynak türü atamak isteyebilirsiniz. Daha sonra bir alt yönetim grubunda veya doğrudan aboneliklerde daha esnek bir ilke (daha fazla kaynak türüne izin veren) atayın. Ancak ilke, açık bir reddetme sistemi olduğundan bu örnek çalışmaz. Bunun yerine, alt yönetim grubunu veya aboneliğini yönetim grubu düzeyinde ilke atamasının dışında bırakmanız gerekir. Daha sonra alt yönetim grubunda veya abonelik düzeyinde daha esnek ilkeyi atayın. Herhangi bir ilke bir kaynağın reddedilmesiyle sonuçlanırsa, kaynağa izin vermek için tek yol reddetme ilkesini değiştirmektir.

Portaldan ilke tanımlarını ve atamalarını ayarlama hakkında daha fazla bilgi için bkz. [Azure ortamınızdaki uyumlu olmayan kaynakları tanımlamak için bir ilke ataması oluşturma](assign-policy-portal.md). [PowerShell](assign-policy-powershell.md) ve [Azure CLI](assign-policy-azurecli.md) adımları da mevcuttur.

## <a name="policy-parameters"></a>İlke parametreleri

İlke parametreleri, oluşturmanız gereken ilke tanımlarının sayısını azaltarak ilke yönetiminizi basitleştirmeye yardımcı olur. İlke tanımı oluştururken parametreleri belirleyerek bunları daha genel hale getirebilirsiniz. Daha sonra bu ilke tanımını farklı senaryolar için kullanabilirsiniz. Bu işlemi, ilke tanımlarının atamasını yaparken farklı değerler girerek gerçekleştirebilirsiniz. Örneğin, abonelik için bir konum kümesi belirtme.

Bir ilke tanımı oluşturulurken parametreler tanımlanmıştır. Bir parametre tanımlandığında, bir ad verilir ve isteğe bağlı olarak bir değer verilir. Örneğin, *konum* başlıklı bir ilke için parametre tanımlayabilirsiniz. Daha sonra, ilkenin atamasını yaparken *EastUS* veya *WestUS* gibi farklı değerler verebilirsiniz.

İlke parametreleri hakkında daha fazla bilgi için bkz. [tanım yapısı-parametreler](./concepts/definition-structure.md#parameters).

## <a name="initiative-definition"></a>Girişim tanımı

Girişim tanımı, tekil kapsamlı bir hedefi gerçekleştirmek üzere belirlenmiş ilke tanımlarının bir koleksiyonudur. Girişim tanımları, ilke tanımlarının yönetimini ve atanmasını basitleştirir. İlke kümelerini gruplandırıp tek bir öğe haline getirerek basitleştirirler. Örneğin, Azure Güvenlik Merkezinizdeki tüm kullanılabilir güvenlik önerilerini izlemeyi hedefleyen **Azure Güvenlik Merkezi'nde İzlemeyi Etkinleştirme** başlıklı bir girişim oluşturabilirsiniz.

Bu girişimin altında sahip olabileceğiniz ilke tanımlarından bazıları şunlardır:

- **Güvenlik Merkezi’ndeki şifrelenmemiş SQL Veritabanı’nı izleme** – Şifrelenmemiş SQL veritabanlarını ve sunucuları izlemek için.
- **Güvenlik Merkezi 'nde işletim sistemi güvenlik açıklarını** izleme – yapılandırılmış temeli karşılamayan sunucuları izleme.
- **Güvenlik Merkezi’ndeki eksik Endpoint Protection’ı izleme** – Yüklü bir bitiş noktası koruma aracısı olmadan sunucuları izlemek için.

## <a name="initiative-assignment"></a>Girişim ataması

İlke ataması gibi, girişim ataması da belirli bir kapsama atanmış olan girişim tanımıdır. Girişim atamaları, her kapsam için birkaç girişim tanımları yapma ihtiyacını azaltır. Bu kapsamın dahilinde de yönetim gruplarından kaynak gruplarına kadar birçok grup bulunabilir.

Her girişim farklı kapsamlara atanabilir. Bir girişim hem **Subscriptiona** hem de **subscriptionb**öğesine atanabilir.

## <a name="initiative-parameters"></a>Girişim parametreleri

İlke parametreleri gibi, girişim parametreleri de fazlalıkları azaltarak girişim yönetiminin basitleştirilmesine yardımcı olur. Girişim parametreleri, girişim içindeki ilke tanımları tarafından kullanılan parametrelerdir.

Örneğin **initiativeC** adına her biri farklı bir parametre türü bekleyen **policyA** ve **policyB** ilke tanımlarına sahip olan bir girişim tanımına sahip olduğunuzu düşünelim:

| İlke | Parametrenin adı |Parametrenin türü  |Not |
|---|---|---|---|
| policyA | allowedLocations | array  |Parametre türü “array” olarak tanımlandığından bu parametre, bir değer için dizilerin bulunduğu bir liste bekler |
| policyB | allowedSingleLocation |string |Parametre türü “array” olarak tanımlandığından bu parametre, bir değer için bir sözcük bekler |

Bu senaryoda **initiativeC** için girişim parametreleri tanımlanırken üç seçeneğiniz vardır:

- Bu girişim dahilinde ilke tanımlarının parametrelerini kullanın: Bu örnekte, *allowedLocations* ve *allowedSingleLocation*, **initiativeC** için girişim parametreleri olur.
- Bu girişim tanımındaki ilke tanımlarının parametrelerine değerler sağlayın. Bu örnekte, **Policya 'ın parametresi – allowedLocations** ve **policyb 'Nin parametresi – allowedsinglelocation**için konumların bir listesini sağlayabilirsiniz. Bu girişimin atamasını yaparken değerleri de sağlayabilirsiniz.
- Bu girişimin atamasını yaparken kullanılabilecek bir *değer* seçenekleri listesi sağlayın. Bu girişimi atadığınızda, girişim dahilindeki ilke tanımlarından alınan parametreler yalnızca bu sağlanan listedeki değerleri alabilir.

Bir girişim tanımında değer seçenekleri oluştururken, girişim ataması sırasında, listenin bir parçası olmadığından farklı bir değer girmeyebilir.

## <a name="maximum-count-of-azure-policy-objects"></a>Maksimum Azure Ilke nesnesi sayısı

[!INCLUDE [policy-limits](../../../includes/azure-policy-limits.md)]

## <a name="recommendations-for-managing-policies"></a>İlkeleri yönetme ile ilgili öneriler

Göz önünde bulundurmanız gereken birkaç işaretçi ve ipucu aşağıda verilmiştir:

- Ortamınızdaki kaynaklarda ilke tanımınızın etkisini izlemek için bir reddetme etkisi yerine bir denetim etkisi ile başlayın. Uygulamalarınızı otomatik olarak otomatik ölçeklendirme için kullanabileceğiniz betikleriniz varsa, bir reddetme efektinin ayarlanması, bu tür otomasyon görevlerinin zaten var olduğunu gösterebilir.

- Tanımlar ve atamalar oluştururken kuruluş hiyerarşilerini göz önünde bulundurun. Yönetim grubu veya abonelik düzeyi gibi daha yüksek düzeylerde tanımlar oluşturmanızı öneririz. Ardından, bir sonraki alt düzeyde atama oluşturun. Bir yönetim grubunda bir tanım oluşturursanız, atama söz konusu yönetim grubu içindeki bir abonelik veya kaynak grubu için kapsamı değiştirilebilir.

- Tek bir ilke tanımı için bile girişim tanımlarının oluşturulmasını ve atanmasını öneririz.
Örneğin, ilke tanımı *policyDefA* ve bunu girişim tanımı *initiativeDefC*altında oluşturabilirsiniz. *Policydefb* için daha sonra *policyDefA*benzeri hedefleri olan bir Ilke tanımı oluşturursanız, *initiativeDefC* altına ekleyebilir ve bunları birlikte izleyebilirsiniz.

- Bir girişim ataması oluşturduktan sonra, girişim öğesine eklenen ilke tanımları da bu girişim atamalarının bir parçası haline gelir.

- Bir girişim ataması değerlendirildiğinde, girişim içindeki tüm ilkeler de değerlendirilir. Bir ilkeyi ayrı ayrı değerlendirmeniz gerekiyorsa, bunu bir girişimde dahil etmek daha iyidir.

## <a name="video-overview"></a>Videoya genel bakış

Aşağıdaki Azure İlkesi genel bakış videosu Build 2018 etkinliğinde kaydedilmiştir. Slaytlar veya video indirme için, Kanal 9 ' da [Azure ilkesi aracılığıyla Azure ortamınızı](https://channel9.msdn.com/events/Build/2018/THR2030) yönetme makalesini ziyaret edin.

> [!VIDEO https://www.youtube.com/embed/dxMaYF2GB7o]

## <a name="next-steps"></a>Sonraki adımlar

Artık Azure İlkesi ve bazı önemli kavramlar ile ilgili bir genel bakışa sahipsiniz, önerdiğimiz diğer adımları aşağıda bulabilirsiniz:

- [Portalı kullanarak bir ilke tanımı atayın](assign-policy-portal.md).
- [Azure CLI kullanarak bir ilke tanımı atayın](assign-policy-azurecli.md).
- [PowerShell kullanarak bir ilke tanımı atayın](assign-policy-powershell.md).
- [Kaynakları Azure Yönetim gruplarıyla düzenleme](..//management-groups/overview.md)ile yönetim grubunun ne olduğunu inceleyin.
- Azure ortamınızı Kanal 9 ' da [Azure ilkesi aracılığıyla](https://channel9.msdn.com/events/Build/2018/THR2030) yönetme görünümü.
