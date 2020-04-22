---
title: Azure İlkesine Genel Bakış
description: Azure İlkesi, Azure ortamında ilke tanımlarınızı oluşturmak, atamak ve yönetmek için kullandığınız bir Azure hizmetidir.
ms.date: 04/21/2020
ms.topic: overview
ms.openlocfilehash: 68005a9a07e7f081a646c75566a0be4046f078ad
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81770596"
---
# <a name="what-is-azure-policy"></a>Azure İlkesi nedir?

Azure İlkesi, kuruluş standartlarının uygulanmasına ve uyumluluğun ölçekte değerlendirilmesine yardımcı olur. Uyumluluk panosu aracılığıyla, çevrenin genel durumunu değerlendirmek için, kaynak başına, ilke başına tanecikliliğe ayrıntılandırma olanağı sağlayan toplu bir görünüm sağlar. Ayrıca, varolan kaynaklar için toplu düzeltme ve yeni kaynaklar için otomatik düzeltme yoluyla kaynaklarınızı uyumluhale getirmeye de yardımcı olur.

Azure İlkesi için yaygın kullanım örnekleri, kaynak tutarlılığı, düzenleyici uyumluluk, güvenlik, maliyet ve yönetim için yönetim uygulamaiçerir. Bu yaygın kullanım örnekleriiçin ilke tanımları, kolayca başlamanıza yardımcı olmak için Azure ortamınızda yerleşik olarak zaten kullanılabilir.

## <a name="overview"></a>Genel Bakış

Azure İlkesi, azure'daki kaynakları, bu kaynakların özelliklerini iş kurallarıyla karşılaştırarak değerlendirir. [JSON formatında](./concepts/definition-structure.md)açıklanan bu iş [kuralları, ilke tanımları](#policy-definition)olarak bilinir. Yönetimi basitleştirmek için, bir [ilke girişimi](#initiative-definition) oluşturmak için çeşitli iş kuralları birlikte gruplandırılabilir (bazen _ilke Kümesi_olarak adlandırılır). İş kurallarınız oluşturulduktan sonra, ilke tanımı veya [girişimi, yönetim grupları,](../management-groups/overview.md)abonelikler, kaynak [grupları](../../azure-resource-manager/management/overview.md#resource-groups)veya tek tek kaynaklar gibi Azure'un desteklediği kaynakların herhangi bir kapsamına [atanır.](#assignments) Atama, bu atama [kapsamındaki](../../azure-resource-manager/management/overview.md#understand-scope) tüm kaynaklar için geçerlidir.
Gerekirse alt skoplar hariç tutulabilir.

Azure İlkesi, bir kaynağın uyumlu olup olmadığını belirlemek için değerlendirmenin kullandığı mantığı oluşturmak için [Bir JSON biçimi](./concepts/definition-structure.md) kullanır. Tanımlar meta verileri ve ilke kuralını içerir. Tanımlanan kural, işlevleri, parametreleri, mantıksal işleçleri, koşulları ve özellik [diğer adlarını](./concepts/definition-structure.md#aliases) tam olarak istediğiniz senaryoyla eşleştirmek için kullanabilir. İlke kuralı, atama kapsamındaki hangi kaynakların değerlendirilip değerlendirildiğini belirler.

### <a name="understand-evaluation-outcomes"></a>Değerlendirme sonuçlarını anlama

Kaynaklar, kaynak yaşam döngüsü, ilke atama yaşam döngüsü ve düzenli sürekli uyumluluk değerlendirmesi sırasında belirli zamanlarda değerlendirilir. Kaynağın değerlendirilmesine neden olan saatler veya olaylar şunlardır:

- Bir kaynak oluşturulur, güncelleştirilir veya ilke ataması içeren bir kapsamda silinir.
- Bir ilke veya girişim bir kapsama yeni atanır.
- Bir kapsama zaten atanmış bir ilke veya girişim güncelleştirilir.
- Her 24 saatte bir gerçekleşen standart uyumluluk değerlendirme döngüsü sırasında.

İlke değerlendirmesinin ne zaman ve nasıl gerçekleştiği hakkında ayrıntılı bilgi [için](./how-to/get-compliance-data.md#evaluation-triggers)bkz.

### <a name="control-the-response-to-an-evaluation"></a>Değerlendirmeye yanıtı denetleme

Uyumlu olmayan kaynakları işlemek için iş kuralları kuruluşlar arasında büyük farklılıklar gösterir. Bir kuruluşun platformun şikayet dışı bir kaynağa yanıt vermesini nasıl istediğine örnek olarak şunlar verilebilir:

- Kaynak değişikliğini reddetme
- Değişikliği kaynağa kaydetme
- Değişiklikten önce kaynağı değiştirme
- Değişiklikten sonra kaynağı değiştirme
- İlgili uyumlu kaynakları dağıtma

Azure İlkesi, bu iş yanıtlarının her birini [efekt uygulaması](./concepts/effects.md)yla mümkün kılar. [Efektler, ilke tanımının](./concepts/definition-structure.md) **ilke kuralı** bölümünde ayarlanır.

### <a name="remediate-non-compliant-resources"></a>Uyumlu olmayan kaynakları düzeltme

Bu etkiler öncelikle kaynak oluşturulduğunda veya güncelleştirildiğinde kaynağı etkilese de, Azure İlkesi, söz sözleşmeyegerek kalmadan varolan uyumlu olmayan kaynaklarla ilgili olarak da desteklenir. Varolan kaynakları uyumlu hale getirme hakkında daha fazla bilgi için, [düzeltici kaynaklara](./how-to/remediate-resources.md)bakın.

### <a name="video-overview"></a>Genel bakış videosu

Aşağıdaki Azure İlkesi genel bakış videosu Build 2018 etkinliğinde kaydedilmiştir. Slaytlar veya video indirme için, Kanal 9'daki [Azure İlkesi aracılığıyla Azure ortamınızı yönet'i](https://channel9.msdn.com/events/Build/2018/THR2030) ziyaret edin.

> [!VIDEO https://www.youtube.com/embed/dxMaYF2GB7o]

## <a name="getting-started"></a>Başlarken

### <a name="azure-policy-and-rbac"></a>Azure İlkesi ve RBAC

Azure İlkesi ve rol tabanlı erişim denetimi (RBAC) arasında birkaç önemli fark vardır. Azure İlkesi, Kaynak Yöneticisi'nde temsil edilen kaynaklardaki özellikleri ve bazı Kaynak Sağlayıcılarının özelliklerini inceleyerek durumu değerlendirir. Azure İlkesi eylemleri kısıtlamaz _(işlemler_olarak da adlandırılır). Azure İlkesi, kaynak durumunun değişikliği kimin yaptığı veya kimlerin değişiklik yapma izni olduğu yla ilgilenmeden işletme kurallarınızla uyumlu olmasını sağlar.

RBAC, farklı kapsamlarda kullanıcı [eylemlerini](../../role-based-access-control/resource-provider-operations.md) yönetmeye odaklanır. Bir eylemin denetimi gerekiyorsa, RBAC kullanılacak doğru araçtır. Bir kişinin bir eylemi gerçekleştirmek için erişimi olsa bile, sonuç uyumlu olmayan bir kaynaksa, Azure İlkesi yine de oluşturma veya güncelleştirmeyi engeller.

RBAC ve Azure İlkesi'nin birleşimi Azure'da tam kapsam denetimi sağlar.

### <a name="rbac-permissions-in-azure-policy"></a>Azure İlkesi'ndeki RBAC İzinleri

Azure İlkesi iki Kaynak Sağlayıcısı’nda işlemler olarak bilinen bazı izinlere sahiptir:

- [Microsoft.Authorization](../../role-based-access-control/resource-provider-operations.md#microsoftauthorization)
- [Microsoft.PolicyInsights](../../role-based-access-control/resource-provider-operations.md#microsoftpolicyinsights)

Birçok Yerleşik rol Azure İlkesi kaynaklarına izin verir. **Kaynak İlkesi Katılımcısı** rolü, çoğu Azure İlkesi işlemlerini içerir. **Sahibi** nin tüm hakları var. Hem **Katılımcı** hem de **Okuyucu,** _okunan_ tüm Azure İlkesi işlemlerine erişebilir. **Katılımcı** kaynak düzeltmeyi tetikleyebilir, ancak tanımlar veya atamalar _oluşturamaz._

Yerleşik rollerin hiçbirinde gerekli izinler yoksa [özel rol](../../role-based-access-control/custom-roles.md) oluşturun.

> [!NOTE]
> **DeployIfNotExists** ilkesi atamasının yönetilen kimliği, şablona dahil edilen kaynakları oluşturmak veya güncelleştirmek için yeterli izine ihtiyaç duyar. Daha fazla bilgi için [bkz.](./how-to/remediate-resources.md#configure-policy-definition)

### <a name="resources-covered-by-azure-policy"></a>Azure İlkesi kapsamındaki kaynaklar

Azure İlkesi, Azure'daki tüm kaynakları değerlendirir. [Konuk Yapılandırması,](./concepts/guest-configuration.md) [Azure Kubernetes Hizmeti](../../aks/intro-kubernetes.md)ve [Azure Key Vault](../../key-vault/key-vault-overview.md)gibi belirli kaynak sağlayıcıları için ayarları ve nesneleri yönetmek için daha derin bir tümleştirme vardır. Daha fazla bilgi edinmek için [Kaynak Sağlayıcı modlarına](./concepts/definition-structure.md)bakın.

### <a name="recommendations-for-managing-policies"></a>İlkeleri yönetme ile ilgili öneriler

Akılda tutulması gereken birkaç işaretçi ve ipucu şunlardır:

- İlke tanımınızın ortamınızdaki kaynaklar üzerindeki etkisini izlemek için reddet efekti yerine bir denetim efektiyle başlayın. Uygulamalarınızı otomatik ölçeklendirmek için zaten komut dosyalarınız varsa, reddet efekti ayarlamak zaten yürürlükte olan bu tür otomasyon görevlerini engelleyebilir.

- Tanımlar ve atamalar oluştururken kuruluş hiyerarşilerini göz önünde bulundurun. Yönetim grubu veya abonelik düzeyi gibi daha yüksek düzeylerde tanımlar oluşturmanızı öneririz. Ardından, sonraki alt düzeyde atama oluşturun. Bir yönetim grubunda bir tanım oluşturursanız, atama bu yönetim grubu içindeki bir abonelik veya kaynak grubuna indirgenebilir.

- Tek bir ilke tanımı için bile girişim tanımları oluşturmanızı ve atamanızı öneririz.
  Örneğin, politika tanımı _politikasıDefA_ var ve girişim tanımı _girişimiDefC_altında oluşturmak . Daha sonra _policyDefB_ için _policyDefA'ya_benzer hedefleri olan başka bir politika tanımı oluşturursanız, _girişimdefc_ altında ekleyebilir ve birlikte izleyebilirsiniz.

- Bir girişim ataması oluşturduktan sonra, girişime eklenen ilke tanımları da bu girişim atamalarının bir parçası haline gelir.

- Bir girişim ataması değerlendirildiğinde, girişim kapsamındaki tüm politikalar da değerlendirilir.
  Bir politikayı tek tek değerlendirmeniz gerekiyorsa, bir girişimi eklememeniz daha iyidir.

## <a name="azure-policy-objects"></a>Azure İlkesi nesneleri

### <a name="policy-definition"></a>İlke tanımı

Azure İlkesi'nde bir ilke oluşturmak ve uygulamak için önce ilke tanımını oluşturmanız gerekir. Her ilke tanımının uygulandığı koşullar vardır. Ve koşullar yerine getirildiğinde gerçekleşen tanımlanmış bir etkisi vardır.

Azure İlkesi'nde, varsayılan olarak kullanılabilen birkaç yerleşik ilke sunuyoruz. Örneğin:

- **İzin Verilen Depolama Hesabı SKU'ları** (Reddet): Dağıtılan bir depolama hesabının SKU boyutları kümesi içinde olup olmadığını belirler. Etkisi, tanımlanan SKU boyutları kümesine uymayan tüm depolama hesaplarını reddetmektir.
- **İzin Verilen Kaynak Türü** (Reddet): Dağıtabileceğiniz kaynak türlerini tanımlar. Etkisi, bu tanımlı listenin bir parçası olmayan tüm kaynakları reddetmektir.
- **İzin Verilen Konumlar** (Reddet): Yeni kaynaklar için kullanılabilir konumları kısıtlar. Sahip olduğu eylem ise coğrafi uyumluluk gereksinimlerinizi uygulamaktır.
- **İzin Verilen Sanal Makine SKU'ları** (Reddet): Dağıtabileceğiniz bir sanal makine SKU kümesi belirtir.
- **Kaynaklara etiket ekleme** (Değiştir: Dağıtım isteği tarafından belirtilmemişse, gerekli etiketi ve varsayılan değerini uygular.
- **Ek etiketi ve varsayılan değeri** (Ek): Gerekli etiketi ve değerini kaynağa zorlar.
- **İzin verilmeyen kaynak türleri** (Reddet): Kaynak türlerinin listesinin dağıtılmasını engeller.

Bu ilke tanımlarını (hem yerleşik hem de özel tanımlar) uygulamak için bunları atamanız gerekir. Bu ilkelerden herhangi birini Azure portalı, PowerShell veya Azure CLI üzerinden atayabilirsiniz.

İlke değerlendirmesi, ilke ataması veya ilke güncelleştirmeleri gibi birkaç farklı eylemle olur. Tam liste [için, İlke değerlendirme tetikleyicileri](./how-to/get-compliance-data.md#evaluation-triggers)bakın.

İlke tanımlarının yapıları hakkında daha fazla bilgi edinmek için [İlke Tanımı Yapısı](./concepts/definition-structure.md) adlı makaleye göz atın.

İlke parametreleri, oluşturmanız gereken ilke tanımlarının sayısını azaltarak ilke yönetiminizi basitleştirmeye yardımcı olur. İlke tanımı oluştururken parametreleri belirleyerek bunları daha genel hale getirebilirsiniz. Daha sonra bu ilke tanımını farklı senaryolar için kullanabilirsiniz. Bu işlemi, ilke tanımlarının atamasını yaparken farklı değerler girerek gerçekleştirebilirsiniz. Örneğin, abonelik için bir konum kümesi belirtme.

İlke tanımı oluşturulurken parametreler tanımlanır. Tanımlanan parametreye bir ad ve isteğe bağlı olarak bir değer verilir. Örneğin, _konum_ başlıklı bir ilke için parametre tanımlayabilirsiniz. Daha sonra, ilkenin atamasını yaparken _EastUS_ veya _WestUS_ gibi farklı değerler verebilirsiniz.

İlke parametreleri hakkında daha fazla bilgi için [tanım yapısı - Parametrelere](./concepts/definition-structure.md#parameters)bakın.

### <a name="initiative-definition"></a>Girişim tanımı

Girişim tanımı, tekil kapsamlı bir hedefi gerçekleştirmek üzere belirlenmiş ilke tanımlarının bir koleksiyonudur. Girişim tanımları, ilke tanımlarının yönetimini ve atanmasını basitleştirir. İlke kümelerini gruplandırıp tek bir öğe haline getirerek basitleştirirler. Örneğin, Azure Güvenlik Merkezinizdeki tüm kullanılabilir güvenlik önerilerini izlemeyi hedefleyen **Azure Güvenlik Merkezi'nde İzlemeyi Etkinleştirme** başlıklı bir girişim oluşturabilirsiniz.

> [!NOTE]
> Azure CLI ve Azure PowerShell gibi SDK, girişimlere başvurmak için **PolicySet** adlı özellikleri ve parametreleri kullanır.

Bu girişimin altında sahip olabileceğiniz ilke tanımlarından bazıları şunlardır:

- **Güvenlik Merkezi’ndeki şifrelenmemiş SQL Veritabanı’nı izleme** – Şifrelenmemiş SQL veritabanlarını ve sunucuları izlemek için.
- **Güvenlik Merkezi'ndeki işletim sistemi açıklarını izleyin** – Yapılandırılan taban çizgisini karşılamayan sunucuları izlemek için.
- **Güvenlik Merkezi’ndeki eksik Endpoint Protection’ı izleme** – Yüklü bir bitiş noktası koruma aracısı olmadan sunucuları izlemek için.

İlke parametreleri gibi, girişim parametreleri de fazlalıkları azaltarak girişim yönetiminin basitleştirilmesine yardımcı olur. Girişim parametreleri, girişim deki ilke tanımları tarafından kullanılan parametrelerdir.

Örneğin **initiativeC** adına her biri farklı bir parametre türü bekleyen **policyA** ve **policyB** ilke tanımlarına sahip olan bir girişim tanımına sahip olduğunuzu düşünelim:

| İlke | Parametrenin adı |Parametrenin türü  |Not |
|---|---|---|---|
| policyA | allowedLocations | array  |Parametre türü “array” olarak tanımlandığından bu parametre, bir değer için dizilerin bulunduğu bir liste bekler |
| policyB | allowedSingleLocation |string |Parametre türü “array” olarak tanımlandığından bu parametre, bir değer için bir sözcük bekler |

Bu senaryoda **initiativeC** için girişim parametreleri tanımlanırken üç seçeneğiniz vardır:

- Bu girişim dahilinde ilke tanımlarının parametrelerini kullanın: Bu örnekte, _allowedLocations_ ve _allowedSingleLocation_, **initiativeC** için girişim parametreleri olur.
- Bu girişim tanımındaki ilke tanımlarının parametrelerine değerler sağlayın. Bu örnekte, **policyA**'nın parametresi olan **–izin verilen Konumlar** ve **policyB**'in parametresi – **izin verilen SingleLocation**' a konumların bir listesini sağlayabilirsiniz. Bu girişimin atamasını yaparken değerleri de sağlayabilirsiniz.
- Bu girişimin atamasını yaparken kullanılabilecek bir _değer_ seçenekleri listesi sağlayın. Bu girişimi atadığınızda, girişim dahilindeki ilke tanımlarından alınan parametreler yalnızca bu sağlanan listedeki değerleri alabilir.

Bir girişim tanımında değer seçenekleri oluştururken, listenin bir parçası olmadığı için girişim ataması sırasında farklı bir değer giremiyorsunuz.

### <a name="assignments"></a>Atamalar

Atama, belirli bir kapsam içinde yer almak üzere atanmış bir ilke tanımı veya girişimidir. Bu kapsam, bir [yönetim grubundan](../management-groups/overview.md) tek bir kaynağa kadar değişebilir. Terim _kapsamı,_ tanımın atandığı tüm kaynakları, kaynak gruplarını, abonelikleri veya yönetim gruplarını ifade eder. Atamalar tüm alt kaynaklardan devralınır. Bu tasarım, kaynak grubuna uygulanan bir tanımın bu kaynak grubundaki kaynaklara da uygulandığı anlamına gelir. Ancak, bir alt kapsam atama hariç tutabilirsiniz.

Örneğin, abonelik kapsamında, ağ kaynaklarının oluşturulmasını engelleyen bir tanım atayabilirsiniz. Bu abonelikte ağ altyapısı için tasarlanmış bir kaynak grubunu hariç tutabilirsiniz. Daha sonra, ağ kaynakları oluşturma konusunda güvendiğiniz kullanıcılara bu ağ kaynak grubuna erişim izni verirsiniz.

Başka bir örnekte, yönetim grubu düzeyinde bir kaynak türü izin listesi tanımı atamak isteyebilirsiniz. Ardından, bir alt yönetim grubunda ve hatta doğrudan aboneliklere daha izin veren bir ilke (daha fazla kaynak türüne izin verme) atarsınız. Ancak, Azure İlkesi açık bir reddi sistemi olduğundan, bu örnek çalışmaz. Bunun yerine, alt yönetim grubunu veya aboneliği yönetim grubu düzeyindeki atamadan çıkarmanız gerekir. Ardından, alt yönetim grubunda veya abonelik düzeyinde daha izin verilen tanımı atayın. Herhangi bir atama, kaynağın reddedilmesiyle sonuçlanırsa, kaynağa izin vermenin tek yolu reddedilen atamayı değiştirmektir.

Portal üzerinden atamaları ayarlama hakkında daha fazla bilgi için azure [ortamınızda uyumlu olmayan kaynakları tanımlamak için bir ilke ataması oluştur'a](assign-policy-portal.md)bakın. [PowerShell](assign-policy-powershell.md) ve [Azure CLI](assign-policy-azurecli.md) adımları da mevcuttur.

## <a name="maximum-count-of-azure-policy-objects"></a>Azure İlkesi nesnelerinin maksimum sayısı

[!INCLUDE [policy-limits](../../../includes/azure-policy-limits.md)]

## <a name="next-steps"></a>Sonraki adımlar

Artık Azure İlkesi ve bazı önemli kavramlar ile ilgili bir genel bakışa sahipsiniz, önerdiğimiz diğer adımları aşağıda bulabilirsiniz:

- [İlke tanımı yapısını gözden geçirin.](./concepts/definition-structure.md)
- [Portalı kullanarak bir ilke tanımı atayın.](./assign-policy-portal.md)
