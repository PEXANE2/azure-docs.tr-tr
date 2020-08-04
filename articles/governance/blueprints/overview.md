---
title: Azure Blueprints’e genel bakış
description: Azure şemaları hizmetinin Azure ortamınızda yapıt oluşturmanıza, tanımlamanıza ve dağıtmanıza nasıl olanak sağladığını anlayın.
ms.date: 05/06/2020
ms.topic: overview
ms.openlocfilehash: a8cec34bb5bdd52b22063a4109153c7f455aaa65
ms.sourcegitcommit: 3d56d25d9cf9d3d42600db3e9364a5730e80fa4a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87530433"
---
# <a name="what-is-azure-blueprints"></a>Azure Blueprints nedir?

Mühendislerin veya mimarların projenin ana hatlarını oluşturmak için kullandıkları şemalar gibi Azure Blueprints de bulut mimarlarının ve merkezi bilgi teknolojisi gruplarının bir kuruluşun standartlarına, desenlerine ve gereksinimlerine uygun Azure kaynaklarından oluşan tekrarlanabilir bir küme tanımlamasını sağlar. Azure şemaları, geliştirme ve teslimin geliştirilmesi için ağ gibi bir yerleşik bileşen kümesi ile kurumsal uyumluluk kapsamında yeni ortamları hızla oluşturup sürekli olarak oluşturma olanağı sağlar.

Blueprints, aşağıdakiler gibi çeşitli kaynak şablonlarını ve diğer yapıtları dağıtma sürecini yönetmenin bildirim temelli bir yoludur:

- Rol Atamaları
- İlke Atamaları
- Azure Resource Manager şablonları (ARM şablonları)
- Kaynak Grupları

Azure Blueprints hizmeti, genel olarak dağıtılmış [Azure Cosmos DB](../../cosmos-db/introduction.md) tarafından desteklenir. Şema nesneleri birden çok Azure bölgesinde çoğaltılır. Bu çoğaltma, Azure 'un kaynaklarınızı hangi bölgeden dağıttığı bağımsız olarak, şema nesneleriniz için düşük gecikme süresi, yüksek kullanılabilirlik ve tutarlı erişim sağlar.

## <a name="how-its-different-from-arm-templates"></a>ARM şablonlarından farklı

Bu hizmet, _ortam kurulumu_ aşamasında yardımcı olmak üzere tasarlanmıştır. Bu kurulum genellikle kaynak grupları, ilkeler, rol atamaları ve ARM şablon dağıtımları kümesinden oluşur. Şema, bu _yapıt_ türlerinin her birini bir araya getiren ve bir CI/CD işlem hattı ile birlikte bu paketi oluşturma ve sürümü oluşturmaya olanak tanıyan bir pakettir. Sonuç olarak her biri tek bir işlem içindeki bir aboneliğe atanır ve denetlenip izlenebilir.

Azure şemaları 'nda dağıtım için eklemek istediğiniz neredeyse her şey ARM şablonuyla gerçekleştirilebilir. Ancak, bir ARM şablonu Azure 'da yerel olarak mevcut olmayan bir belgedir; her biri yerel olarak ya da kaynak denetiminde depolanır. Şablon bir veya daha fazla Azure kaynağının dağıtılması için kullanılır ancak bu kaynaklar dağıtıldıktan sonra kullanılan şablonla etkin bir bağlantı veya ilişki kalmaz.

Azure şemaları ile şema tanımı (dağıtılması _gereken_ ) ve şema ataması _(dağıtılan)_ arasındaki ilişki korunur. Bu bağlantı dağıtımların daha iyi izlenmesini ve denetlenmesini destekler. Azure şemaları ayrıca aynı Blueprint tarafından yönetilen birçok aboneliği aynı anda yükseltebilir.

ARM şablonuyla bir Blueprint arasında seçim yapmanız gerekmez. Her bir şema, sıfır veya daha fazla ARM şablon _yapıtlarından_oluşabilir. Bu destek, daha önceki bir ARM şablonları kitaplığı geliştirme ve bakımını yapma çabalarının Azure 'da yeniden kullanılabilir olduğunu gösterir.

## <a name="how-its-different-from-azure-policy"></a>Azure İlkesi ile arasındaki farklar

Şema tutarlılık ve uyumluluk sağlamak için yeniden kullanılabilen Azure bulut hizmetleri, güvenlik ve tasarım uygulamasıyla ilgili belirli bir alana yönelik standart, desen ve gereksinim kümesi oluşturmak için kullanılan bir paket veya kapsayıcıdır.

[İlke](../policy/overview.md) ise yeni dağıtılan ve var olan kaynakların özelliklerine odaklanan varsayılan olarak izin verme ve açıkça reddetme sistemidir. Bir abonelik içindeki kaynakların gereksinimlere ve standartlara uygun olduğunu doğrulayarak bulut yönetimini destekler.

Bir ilkeyi bir şemaya dahil etmek, şemanın atanması sırasında doğru desenin veya tasarımın oluşturulmasını sağlar. Bu ilke şemanın amacıyla uyumluluk sağlamak için ortamda yalnızca onaylanan veya beklenen değişikliklerin yapılmasını garanti eder.

Bir ilke, şema tanımındaki birçok _yapıtlardan_ biri olarak eklenebilir. Şemalar ayrıca ilkeler ve girişimlerle parametrelerin kullanılmasını da destekler.

## <a name="blueprint-definition"></a>Şema tanımı

Şemalar, _yapıtlardan_ meydana gelir. Azure şemaları Şu anda yapıt olarak aşağıdaki kaynakları desteklemektedir:

|Kaynak  | Hiyerarşi seçenekleri| Açıklama  |
|---------|---------|---------|
|Kaynak Grupları | Abonelik | Şema içindeki diğer yapıtlar tarafından kullanılacak yeni bir kaynak grubu oluşturur.  Bu yer tutucu kaynak grupları, kaynakları tam olarak istediğiniz şekilde düzenlemenize olanak tanır ve dahil edilen ilke ve rol atama yapıtları ve ARM şablonları için bir kapsam sınırlayıcısı sağlar. |
|ARM şablonu | Abonelik, Kaynak Grubu | İç içe ve bağlı şablonlar dahil olmak üzere şablonlar, karmaşık ortamları oluşturmak için kullanılır. Örnek ortamlar: SharePoint grubu, Azure Otomasyonu Durum Yapılandırması veya Log Analytics çalışma alanı. |
|İlke Ataması | Abonelik, Kaynak Grubu | Bir ilkenin veya girişimin, şemanın atanmış olduğu aboneliğe atanmasını sağlar. İlke veya girişim, şema tanım konumunun kapsamında olmalıdır. İlke veya girişimde parametre varsa bu parametreler şema oluşturma veya şema ataması sırasında atanır. |
|Rol Ataması | Abonelik, Kaynak Grubu | Kaynaklarınıza her zaman doğru kişilerin doğru şekilde erişmesini sağlamak için var olan bir kullanıcıyı veya grubu yerleşik role ekleyin. Rol atamaları aboneliğin tamamı için tanımlanabilir veya şemada bulunan belirli bir kaynak grubuna yerleştirilebilir. |

### <a name="blueprint-definition-locations"></a>Şema tanımı konumlarını şeması

Şema tanımı oluştururken şemanın kaydedileceği yeri de tanımlarsınız. Planlar, **katkıda** bulunan erişimi olan bir [yönetim grubuna](../management-groups/overview.md) veya aboneliğe kaydedilebilir. Konum bir yönetim grubumu ise, bu yönetim grubunun herhangi bir alt aboneliğine atamak için şema kullanılabilir.

### <a name="blueprint-parameters"></a>Şema parametreleri

Planlar, parametreleri bir ilke/girişim ya da ARM şablonuna geçirebilir. Bir şemaya yeni bir _yapıt_ eklendiğinde yazar her bir şema ataması için tanımlı bir değer sunar veya her şema atamasının atama sırasında değer sağlamasına izin verir. Bu esneklik şemanın tüm kullanımları için önceden tanımlanmış bir değer tanımlama veya bu kararın atama sırasında verilmesi seçeneğini sunar.

> [!NOTE]
> Şema kendi parametrelerine sahip olabilir ancak bu özellik şu an için yalnızca şemanın portal yerine REST API ile oluşturulması durumunda kullanılabilir.

Daha fazla bilgi için bkz. [şema parametreleri](./concepts/parameters.md).

### <a name="blueprint-publishing"></a>Şema yayımlama

Şema ilk oluşturulduğunda **Taslak** modunda olur. Atanmaya hazır olduğunda **Yayımlandı** durumuna alınması gerekir. Yayımlamak için bir **Sürüm** dizesi (harfler, sayılar ve kısa çizgiler, maksimum 20 karakter) ve isteğe bağlı **Değişiklik notları** tanımlanması gerekir. **Sürüm** değeri şemada daha sonra yapılacak değişikliklerin tanımlanmasını sağlar ve her sürümün ayrıca atanmasını mümkün hale getirir. Sürüm oluşturma aynı zamanda aynı şemanın farklı **Sürümlerinin** aynı aboneliğe atanabileceği anlamına da gelir. Şema üzerinde ek değişiklikler yapıldığında, **Published** 
 **yayımlanmamış değişiklikler**olduğu gibi yayımlanan**Sürüm** yine de mevcuttur. Değişiklikler tamamlandıktan sonra güncelleştirilen şema yeni ve benzersiz bir **Sürüm** değeriyle **Yayımlanır** ve atamaya hazır hale gelir.

## <a name="blueprint-assignment"></a>Şema ataması

Bir şema 'in **yayımlanan** her **sürümü** var olan bir aboneliğe atanabilir (en fazla 90 karakter uzunluğunda bir ad olabilir). Portalda varsayılan olarak en son **Yayımlanmış** olan şema **Sürümü** kullanılır. Yapıt parametreleri (veya şema parametreleri) varsa atama işlemi sırasında ilgili parametreler tanımlanır.

## <a name="permissions-in-azure-blueprints"></a>Azure Blueprints'te izinler

Şemaları kullanmak için [Rol tabanlı erişim denetimi](../../role-based-access-control/overview.md) (RBAC) ile gerekli izinlerin verilmiş olması gerekir. Şema oluşturmak için hesabınız şu izinlere sahip olmalıdır:

- `Microsoft.Blueprint/blueprints/write` - Şema tanımı oluştur
- `Microsoft.Blueprint/blueprints/artifacts/write` - Şema tanımında yapıt oluştur
- `Microsoft.Blueprint/blueprints/versions/write` - Şema yayımla

Şemaları silmek için hesabınız şu izinlere sahip olmalıdır:

- `Microsoft.Blueprint/blueprints/delete`
- `Microsoft.Blueprint/blueprints/artifacts/delete`
- `Microsoft.Blueprint/blueprints/versions/delete`

> [!NOTE]
> Şema tanımı izinleri, kaydedildiği veya yönetim grubu ya da abonelik kapsamında kaydedildiği yere devralınmalıdır.

Bir şemayı atamak veya atamasını kaldırmak için hesabınız şu izinlere sahip olmalıdır:

- `Microsoft.Blueprint/blueprintAssignments/write` - Şema ata
- `Microsoft.Blueprint/blueprintAssignments/delete` - Şemanın atamasını kaldır

> [!NOTE]
> Şema atamaları abonelikte oluşturulduğundan şema atama ve atamasını kaldırma izinlerinin abonelik kapsamında verilmesi veya abonelik kapsamında devralınması gerekir.

Aşağıdaki yerleşik roller kullanılabilir:

|Azure rolü | Açıklama |
|-|-|
|[Sahibi](../../role-based-access-control/built-in-roles.md#owner) | Diğer izinlerin yanı sıra, tüm Azure Blueprint ilgili izinleri içerir. |
|[Katkıda Bulunan](../../role-based-access-control/built-in-roles.md#contributor) | Diğer izinlerin yanı sıra, şema tanımları oluşturup silebilir, ancak şema atama izinlerine sahip değildir. |
|[Blueprint Katılımcısı](../../role-based-access-control/built-in-roles.md#blueprint-contributor) | Şema tanımlarını yönetebilir, ancak atamazsınız. |
|[Blueprint Işleci](../../role-based-access-control/built-in-roles.md#blueprint-operator) | Varolan yayımlanmış şemaları atayabilir, ancak yeni şema tanımları oluşturamaz. Blueprint ataması, yalnızca atama Kullanıcı tarafından atanan yönetilen kimlikle yapıldığında işe yarar. |

Bu yerleşik roller güvenlik gereksinimlerinize uygun değilse [özel rol](../../role-based-access-control/custom-roles.md) oluşturabilirsiniz.

> [!NOTE]
> Sistem tarafından atanan yönetilen kimlik kullanılıyorsa, Azure şemaları için hizmet sorumlusu, dağıtımı etkinleştirmek için atanan abonelikte **sahip** rolünü gerektirir. Portalı kullanıyorsanız bu rol dağıtım için otomatik olarak verilir ve iptal edilir. REST API kullanıyorsanız bu rolün el ile verilmesi gerekir ancak dağıtım tamamlandıktan sonra iptal işlemi otomatik olarak gerçekleştirilir. Kullanıcı tarafından atanan yönetilen kimlik kullanılıyorsa, yalnızca şema atamasını oluşturan kullanıcı, `Microsoft.Blueprint/blueprintAssignments/write` hem **sahip** hem de **şema işleci** yerleşik rollerinde bulunan izne ihtiyaç duyuyor.

## <a name="naming-limits"></a>Adlandırma sınırları

Belirli alanlar için aşağıdaki sınırlamalar mevcuttur:

|Nesne|Alan|İzin verilen karakterler|En çok, Uzunluk|
|-|-|-|-|
|Şema|Name|harfler, rakamlar, tireler ve noktalar|48|
|Şema|Sürüm|harfler, rakamlar, tireler ve noktalar|20|
|Şema ataması|Name|harfler, rakamlar, tireler ve noktalar|90|
|Blueprint yapıtı|Name|harfler, rakamlar, tireler ve noktalar|48|

## <a name="video-overview"></a>Genel bakış videosu

Azure 'a yönelik olarak aşağıdaki genel bakış Azure Fridays. Video indirme için, Channel 9 ' da Azure 'a [yönelik bir genel bakış olan Azure Fridays](https://channel9.msdn.com/Shows/Azure-Friday/An-overview-of-Azure-Blueprints) adresini ziyaret edin.

> [!VIDEO https://www.youtube.com/embed/cQ9D-d6KkMY]

## <a name="next-steps"></a>Sonraki adımlar

- [Bir şema-Portal oluşturun](./create-blueprint-portal.md).
- [Bir şema-PowerShell oluşturun](./create-blueprint-powershell.md).
- [Şema-REST API oluşturun](./create-blueprint-rest-api.md).