---
title: Azure İlkesine Genel Bakış
description: Azure İlkesi, Azure ortamında ilke tanımlarınızı oluşturmak, atamak ve yönetmek için kullandığınız bir Azure hizmetidir.
ms.date: 11/25/2019
ms.topic: overview
ms.custom: fasttrack-edit
ms.openlocfilehash: e886f37a8d7f1395b5c831e81e600ecc6e2dd20f
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79241531"
---
# <a name="what-is-azure-policy"></a>Azure İlkesi nedir?

Yönetim, kuruluşunuzun BT'yi etkin ve verimli bir şekilde kullanarak hedeflerine ulaşabileceğini doğrular. İş hedefleri ve BT projeleri arasında netlik oluşturarak bu ihtiyacı karşılar.

Şirketiniz asla çözülmeyecek gibi görünen önemli sayıda BT sorunlarıyla mi karşılaşıyor? İyi bir BT yönetimine sorunların yönetilmesine ve önlenmesine yardımcı olma amacıyla girişimlerinizi planlama ve önceliklerinizi stratejik düzeyde belirleme dahildir. Bu stratejik ihtiyaç Azure İlkesi'nin devreye girdiği yerdir.

Azure İlkesi, ilkelerinizi oluşturmak, atamak ve yönetmek için kullandığınız bir Azure hizmetidir. Bu ilkeler, kaynaklarınız üzerinden farklı kuralları ve eylemleri uygulatarak kaynaklarınızın kurumsal standartlarınız ve hizmet düzeyi sözleşmelerinizle uyumlu kalmasını sağlar. Azure İlkesi, kaynaklarınızın atanan ilkelerle uyumlu olup olmadığını değerlendirerek bu ihtiyacı karşılar. Azure İlkesi tarafından depolanan tüm veriler istirahatte şifrelenir.

Örneğin, ortamınızda yalnızca belirli SKU boyutuna sahip sanal makinelere izin veren bir ilkeniz olabilir. Bu ilke uygulamaya alındığında yeni ve mevcut kaynaklarda uyumluluk değerlendirmesi yapılır. Doğru ilke türleri ile mevcut kaynakları uyumlu hale getirebilirsiniz. Bu dokümantasyonun ilerleyen saatlerinde, Azure İlkesi ile ilkelerin nasıl oluşturulacağı ve uygulanacağı hakkında daha fazla ayrıntının üzerinden geçeceğiz.

> [!IMPORTANT]
> Azure İlkesi’nin uyumluluk değerlendirmesi artık fiyatlandırma katmanından bağımsız olarak tüm atamalara sağlanır. Atamalarınız uyumluluk verilerini göstermezse, lütfen aboneliğin Microsoft.PolicyInsights kaynak sağlayıcısı ile kaydolduğundan emin olun.

[!INCLUDE [azure-lighthouse-supported-service](../../../includes/azure-lighthouse-supported-service.md)]

## <a name="how-is-it-different-from-rbac"></a>RBAC ile farkları nelerdir?

Azure İlkesi ve rol tabanlı erişim denetimi (RBAC) arasında birkaç önemli fark vardır. RBAC farklı kapsamlardaki kullanıcı eylemlerine odaklanır. Kaynak grubu için katılımcı rolüne eklenebilir ve bu kaynak grubunda değişiklik yapabilirsiniz. Azure İlkesi, dağıtım sırasında ve zaten varolan kaynaklar için kaynak özelliklerine odaklanır. Azure İlkesi kaynakların türü veya konumu gibi özellikleri denetler. RBAC'ın aksine, Azure İlkesi varsayılan izin veren ve açıkça reddeden bir sistemdir.

### <a name="rbac-permissions-in-azure-policy"></a>Azure İlkesi'ndeki RBAC İzinleri

Azure İlkesi iki Kaynak Sağlayıcısı’nda işlemler olarak bilinen bazı izinlere sahiptir:

- [Microsoft.Authorization](../../role-based-access-control/resource-provider-operations.md#microsoftauthorization)
- [Microsoft.PolicyInsights](../../role-based-access-control/resource-provider-operations.md#microsoftpolicyinsights)

Birçok Yerleşik rol Azure İlkesi kaynaklarına izin verir. **Kaynak İlkesi Katılımcısı** rolü, çoğu Azure İlkesi işlemlerini içerir. **Sahibi** nin tüm hakları var. Hem **Katılımcı** hem de **Okuyucu** tüm okunan Azure İlkesi işlemlerini kullanabilir, ancak **Katkıda Bulunan** kişi düzeltmeyi de tetikleyebilir.

Yerleşik rollerin hiçbirinde gerekli izinler yoksa [özel rol](../../role-based-access-control/custom-roles.md) oluşturun.

## <a name="policy-definition"></a>İlke tanımı

Azure İlkesi'nde bir ilke oluşturmak ve uygulamak için önce ilke tanımını oluşturmanız gerekir. Her ilke tanımının uygulandığı koşullar vardır. Ve koşullar yerine getirildiğinde gerçekleşen tanımlanmış bir etkisi vardır.

Azure İlkesi'nde, varsayılan olarak kullanılabilen birkaç yerleşik ilke sunuyoruz. Örnek:

- **İzin Verilen Depolama Hesabı SKU'ları**: Dağıtılan bir depolama hesabının SKU boyutları kümesi içinde olup olmadığını belirler. Etkisi, tanımlanan SKU boyutları kümesine uymayan tüm depolama hesaplarını reddetmektir.
- **İzin Verilen Kaynak Türü**: Dağıtabileceğiniz kaynak türlerini tanımlar. Etkisi, bu tanımlı listenin bir parçası olmayan tüm kaynakları reddetmektir.
- **İzin Verilen Konumlar**: Yeni kaynaklar için kullanılabilir konumları kısıtlar. Sahip olduğu eylem ise coğrafi uyumluluk gereksinimlerinizi uygulamaktır.
- **İzin Verilen Sanal Makine SKU'ları**: Dağıtabileceğiniz bir sanal makine SKU'ları kümesini belirtir.
- **Kaynaklara etiket ekleme**: Dağıtım isteğinde belirtilmemişse, gerekli etiketi ve varsayılan değerini uygular.
- **Etiketi ve değerini zorla**: Gerekli etiketi ve değerini kaynağa zorlar.
- **İzin verilmeyen kaynak türleri**: Kaynak türlerinin listesinin dağıtılmasını engeller.

Bu ilke tanımlarını (hem yerleşik hem de özel tanımlar) uygulamak için bunları atamanız gerekir. Bu ilkelerden herhangi birini Azure portalı, PowerShell veya Azure CLI üzerinden atayabilirsiniz.

İlke değerlendirmesi, ilke ataması veya ilke güncelleştirmeleri gibi birkaç farklı eylemle olur. Tam liste [için, İlke değerlendirme tetikleyicileri](./how-to/get-compliance-data.md#evaluation-triggers)bakın.

İlke tanımlarının yapıları hakkında daha fazla bilgi edinmek için [İlke Tanımı Yapısı](./concepts/definition-structure.md) adlı makaleye göz atın.

## <a name="policy-assignment"></a>İlke ataması

İlke ataması, belirli bir kapsamda gerçekleşmesi için atanmış bir ilke tanımıdır. Bu kapsam, bir [yönetim grubundan](../management-groups/overview.md) tek bir kaynağa kadar değişebilir. Terim *kapsamı,* ilke tanımının atandığı tüm kaynakları, kaynak gruplarını, abonelikleri veya yönetim gruplarını ifade eder. İlke atamaları, tüm alt kaynaklar tarafından devralınır. Bu tasarım, kaynak grubuna uygulanan bir ilkenin, bu kaynak grubundaki kaynaklara da uygulandığı anlamına gelir. Ancak, dilerseniz bir alt kapsamı ilke atamasından dışlayabilirsiniz.

Örneğin abonelik kapsamında, ağ kaynaklarının oluşturulmasını önleyen bir ilke atayabilirsiniz. Bu abonelikte ağ altyapısı için tasarlanmış bir kaynak grubunu hariç tutabilirsiniz. Daha sonra, ağ kaynakları oluşturma konusunda güvendiğiniz kullanıcılara bu ağ kaynak grubuna erişim izni verirsiniz.

Başka bir örnekte, yönetim grubu düzeyinde bir kaynak türü izin listesi ilkesi atamak isteyebilirsiniz. Daha sonra bir alt yönetim grubunda veya doğrudan aboneliklerde daha esnek bir ilke (daha fazla kaynak türüne izin veren) atayın. Ancak ilke, açık bir reddetme sistemi olduğundan bu örnek çalışmaz. Bunun yerine, alt yönetim grubunu veya aboneliğini yönetim grubu düzeyinde ilke atamasının dışında bırakmanız gerekir. Daha sonra alt yönetim grubunda veya abonelik düzeyinde daha esnek ilkeyi atayın. Herhangi bir ilke kaynağın reddedilmesiyle sonuçlanırsa, kaynağa izin vermenin tek yolu reddedilen ilkeyi değiştirmektir.

Portaldan ilke tanımlarını ve atamalarını ayarlama hakkında daha fazla bilgi için bkz. [Azure ortamınızdaki uyumlu olmayan kaynakları tanımlamak için bir ilke ataması oluşturma](assign-policy-portal.md). [PowerShell](assign-policy-powershell.md) ve [Azure CLI](assign-policy-azurecli.md) adımları da mevcuttur.

## <a name="policy-parameters"></a>İlke parametreleri

İlke parametreleri, oluşturmanız gereken ilke tanımlarının sayısını azaltarak ilke yönetiminizi basitleştirmeye yardımcı olur. İlke tanımı oluştururken parametreleri belirleyerek bunları daha genel hale getirebilirsiniz. Daha sonra bu ilke tanımını farklı senaryolar için kullanabilirsiniz. Bu işlemi, ilke tanımlarının atamasını yaparken farklı değerler girerek gerçekleştirebilirsiniz. Örneğin, abonelik için bir konum kümesi belirtme.

İlke tanımı oluşturulurken parametreler tanımlanır. Tanımlanan parametreye bir ad ve isteğe bağlı olarak bir değer verilir. Örneğin, *konum* başlıklı bir ilke için parametre tanımlayabilirsiniz. Daha sonra, ilkenin atamasını yaparken *EastUS* veya *WestUS* gibi farklı değerler verebilirsiniz.

İlke parametreleri hakkında daha fazla bilgi için [tanım yapısı - Parametrelere](./concepts/definition-structure.md#parameters)bakın.

## <a name="initiative-definition"></a>Girişim tanımı

Girişim tanımı, tekil kapsamlı bir hedefi gerçekleştirmek üzere belirlenmiş ilke tanımlarının bir koleksiyonudur. Girişim tanımları, ilke tanımlarının yönetimini ve atanmasını basitleştirir. İlke kümelerini gruplandırıp tek bir öğe haline getirerek basitleştirirler. Örneğin, Azure Güvenlik Merkezinizdeki tüm kullanılabilir güvenlik önerilerini izlemeyi hedefleyen **Azure Güvenlik Merkezi'nde İzlemeyi Etkinleştirme** başlıklı bir girişim oluşturabilirsiniz.

> [!NOTE]
> Azure CLI ve Azure PowerShell gibi SDK, girişimlere başvurmak için **PolicySet** adlı özellikleri ve parametreleri kullanır.

Bu girişimin altında sahip olabileceğiniz ilke tanımlarından bazıları şunlardır:

- **Güvenlik Merkezi’ndeki şifrelenmemiş SQL Veritabanı’nı izleme** – Şifrelenmemiş SQL veritabanlarını ve sunucuları izlemek için.
- **Güvenlik Merkezi'ndeki işletim sistemi açıklarını izleyin** – Yapılandırılan taban çizgisini karşılamayan sunucuları izlemek için.
- **Güvenlik Merkezi’ndeki eksik Endpoint Protection’ı izleme** – Yüklü bir bitiş noktası koruma aracısı olmadan sunucuları izlemek için.

## <a name="initiative-assignment"></a>Girişim ataması

İlke ataması gibi, girişim ataması da belirli bir kapsama atanmış olan girişim tanımıdır. Girişim atamaları, her kapsam için birkaç girişim tanımları yapma ihtiyacını azaltır. Bu kapsam, yönetim grubundan tek bir kaynağa da olabilir.

Her girişim farklı kapsamlara atanabilir. Bir girişim hem **subscriptionA** ve **subscriptionB**atanabilir.

## <a name="initiative-parameters"></a>Girişim parametreleri

İlke parametreleri gibi, girişim parametreleri de fazlalıkları azaltarak girişim yönetiminin basitleştirilmesine yardımcı olur. Girişim parametreleri, girişim deki ilke tanımları tarafından kullanılan parametrelerdir.

Örneğin **initiativeC** adına her biri farklı bir parametre türü bekleyen **policyA** ve **policyB** ilke tanımlarına sahip olan bir girişim tanımına sahip olduğunuzu düşünelim:

| İlke | Parametrenin adı |Parametrenin türü  |Not |
|---|---|---|---|
| policyA | allowedLocations | array  |Parametre türü “array” olarak tanımlandığından bu parametre, bir değer için dizilerin bulunduğu bir liste bekler |
| policyB | allowedSingleLocation |string |Parametre türü “array” olarak tanımlandığından bu parametre, bir değer için bir sözcük bekler |

Bu senaryoda **initiativeC** için girişim parametreleri tanımlanırken üç seçeneğiniz vardır:

- Bu girişim dahilinde ilke tanımlarının parametrelerini kullanın: Bu örnekte, *allowedLocations* ve *allowedSingleLocation*, **initiativeC** için girişim parametreleri olur.
- Bu girişim tanımındaki ilke tanımlarının parametrelerine değerler sağlayın. Bu örnekte, **policyA'nın parametresine (izin verilen Konumlar** ve **policyB'nin parametresi)** izin verilen SingleLocation'ına konumların bir listesini sağlayabilirsiniz. Bu girişimin atamasını yaparken değerleri de sağlayabilirsiniz.
- Bu girişimin atamasını yaparken kullanılabilecek bir *değer* seçenekleri listesi sağlayın. Bu girişimi atadığınızda, girişim dahilindeki ilke tanımlarından alınan parametreler yalnızca bu sağlanan listedeki değerleri alabilir.

Bir girişim tanımında değer seçenekleri oluştururken, listenin bir parçası olmadığı için girişim ataması sırasında farklı bir değer giremiyorsunuz.

## <a name="maximum-count-of-azure-policy-objects"></a>Azure İlkesi nesnelerinin maksimum sayısı

[!INCLUDE [policy-limits](../../../includes/azure-policy-limits.md)]

## <a name="recommendations-for-managing-policies"></a>İlkeleri yönetme ile ilgili öneriler

Akılda tutulması gereken birkaç işaretçi ve ipucu şunlardır:

- İlke tanımınızın ortamınızdaki kaynaklar üzerindeki etkisini izlemek için reddet efekti yerine bir denetim efektiyle başlayın. Uygulamalarınızı otomatik ölçeklendirmek için zaten komut dosyalarınız varsa, reddet efekti ayarlamak zaten yürürlükte olan bu tür otomasyon görevlerini engelleyebilir.

- Tanımlar ve atamalar oluştururken kuruluş hiyerarşilerini göz önünde bulundurun. Yönetim grubu veya abonelik düzeyi gibi daha yüksek düzeylerde tanımlar oluşturmanızı öneririz. Ardından, sonraki alt düzeyde atama oluşturun. Bir yönetim grubunda bir tanım oluşturursanız, atama bu yönetim grubu içindeki bir abonelik veya kaynak grubuna indirgenebilir.

- Tek bir ilke tanımı için bile girişim tanımları oluşturmanızı ve atamanızı öneririz.
  Örneğin, politika tanımı *politikasıDefA* var ve girişim tanımı *girişimiDefC*altında oluşturmak . Daha sonra *policyDefB* için *policyDefA'ya*benzer hedefleri olan başka bir politika tanımı oluşturursanız, *girişimdefc* altında ekleyebilir ve birlikte izleyebilirsiniz.

- Bir girişim ataması oluşturduktan sonra, girişime eklenen ilke tanımları da bu girişim atamalarının bir parçası haline gelir.

- Bir girişim ataması değerlendirildiğinde, girişim kapsamındaki tüm politikalar da değerlendirilir.
  Bir politikayı tek tek değerlendirmeniz gerekiyorsa, bir girişimi eklememeniz daha iyidir.

## <a name="video-overview"></a>Genel bakış videosu

Aşağıdaki Azure İlkesi genel bakış videosu Build 2018 etkinliğinde kaydedilmiştir. Slaytlar veya video indirme için, Kanal 9'daki [Azure İlkesi aracılığıyla Azure ortamınızı yönet'i](https://channel9.msdn.com/events/Build/2018/THR2030) ziyaret edin.

> [!VIDEO https://www.youtube.com/embed/dxMaYF2GB7o]

## <a name="next-steps"></a>Sonraki adımlar

Artık Azure İlkesi ve bazı önemli kavramlar ile ilgili bir genel bakışa sahipsiniz, önerdiğimiz diğer adımları aşağıda bulabilirsiniz:

- [Portalı kullanarak bir ilke tanımı atayın.](./assign-policy-portal.md)
- [Azure CLI'yi kullanarak bir ilke tanımı atayın.](./assign-policy-azurecli.md)
- [PowerShell'i kullanarak bir ilke tanımı atayın.](./assign-policy-powershell.md)
