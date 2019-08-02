---
title: Azure Avustralya 'da rol tabanlı Access Control ve Privileged Identity Management
description: Avustralya kamu ilkesinin, düzenlemelerinin ve yasalların belirli ihtiyaçlarını karşılamak için, Avustralya bölgeleri içinde rol tabanlı Access Control ve Privileged Identity Management uygulama hakkında rehberlik.
author: Galey801
ms.service: azure-australia
ms.topic: conceptual
ms.date: 07/22/2019
ms.author: grgale
ms.openlocfilehash: a9451b5dcd572eee27809cf66df889f06da960ed
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68571503"
---
# <a name="role-based-access-control-rbac-and-privileged-identity-management-pim"></a>Rol tabanlı Access Control (RBAC) ve Privileged Identity Management (PıM)

Yönetim ayrıcalığını yönetmek, herhangi bir BT ortamında güvenlik sağlamaya yönelik kritik bir adımdır. En az ayrıcalık güvenliği kullanılarak yönetim ayrıcalığının kısıtlanması, [ACSC önemli 8](https://www.acsc.gov.au/infosec/mitigationstrategies.htm) güvenlik önerileri listesinin bir parçası olan [ACSC ISM](https://acsc.gov.au/infosec/ism/index.htm) ve formları bir gereksinimdir.

Microsoft, Microsoft Azure içinde tam zamanında ve tam erişim uygulamak için bir denetim paketi sağlar. Bu denetimleri anlamak, bulutta etkin bir güvenlik duruşi için önemlidir. Bu kılavuz, denetimlere genel bir bakış ve bunları uygularken önemli tasarım konularını sağlar.

## <a name="role-based-access-control-rbac"></a>Rol Tabanlı Erişim Denetimi (RBAC)

Rol tabanlı Access Control, Microsoft Azure içindeki tüm kaynaklara erişimin yönetimi ve Azure Active Directory (Azure AD) yönetimi için merkezi bir yönetim merkezidir. RBAC, Azure 'da kullanılabilen bir dizi tamamlayıcı özelliğin yanı sıra uygulanabilir. Bu makale, Azure Yönetim Grupları, Azure Active Directory grupları ve Azure Privileged Identity Management (PıM) kullanarak geçerli RBAC uygulamaya odaklanır.

Yüksek düzeyde RBAC uygulamak için üç bileşen gerekir:

![RBAC-genel bakış](media/rbac-overview.png)

* **Güvenlik sorumluları**: Bir güvenlik sorumlusu aşağıdakilerden herhangi biri olabilir; Kullanıcı, Grup, [hizmet sorumluları veya [yönetilen kimlik](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview). Güvenlik sorumlularına Azure Active Directory grupları kullanılarak ayrıcalıklar atanmalıdır.

* **Rol tanımları**: Rol olarak da adlandırılan bir rol tanımı, izin koleksiyonudur. Bu izinler, rol tanımına atanan güvenlik sorumluları tarafından gerçekleştirilebilecek işlemleri tanımlar. Bu işlevsellik, Azure Kaynak rolleri ve Azure Active Directory yönetici rolleri tarafından sağlanır. Azure, özel rollerle genişletilebilir bir dizi yerleşik rol (bağlantı) ile birlikte gelir.

* **Kapsam**: Kapsam, bir rol tanımının geçerli olduğu Azure kaynakları kümesidir. Azure rolleri, Azure Yönetim Grupları kullanılarak Azure kaynaklarına atanabilir.

Bu üç bileşen, rol tanımlarında tanımlanan erişimin Azure Yönetim Grupları ' kapsamında yer alan tüm kaynaklara güvenlik sorumluları sağlamak için birleşir ve bu, rol ataması olarak adlandırılır. Bir güvenlik sorumlusuna birden çok rol tanımı atanabilir ve tek bir kapsama birden çok güvenlik sorumlusu atanabilir.

### <a name="azure-active-directory-groups"></a>Azure Active Directory grupları

Kişilere veya takımlara ayrıcalık atarken, atamanın mümkün olduğu her durumda atama bir Azure Active Directory grubuna bağlanmalıdır ve doğrudan söz konusu kullanıcıya atanmaz. Bu, şirket içi Active Directory uygulamalarından devralınan aynı önerilen uygulamadır. Mümkün olan Azure Active Directory gruplarının ekip başına oluşturulması, oluşturduğunuz Azure Yönetim Grupları mantıksal yapısı ile tamamlayıcı.

Karma bir bulut senaryosunda, şirket içi Windows Server Active Directory güvenlik grupları Azure Active Directory örneğiniz ile eşitlenebilir. Bu Windows Server Active Directory güvenlik gruplarını kullanarak şirket içi RBAC 'yi zaten uyguladıysanız, bu gruplar eşitlendiğinde, Azure kaynaklarınız için RBAC uygulamak üzere kullanılabilir. Aksi takdirde, bulut ortamınız Azure Active Directory uygulamanızda yerleşik olarak bulunan güçlü ayrıcalık yönetimi planını tasarlamak ve uygulamak için temiz bir tablet olarak görülebilir.

### <a name="azure-resource-roles-versus-azure-active-directory-administrator-roles"></a>Azure Kaynak rolleri Azure Active Directory yönetici rollerine karşı

Microsoft Azure, [Azure kaynakları](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) ve [Azure Active Directory Yönetimi](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)için çok çeşitli yerleşik roller sunmaktadır. Her iki rol türü de Azure kaynaklarına ya da Azure AD yöneticilerine belirli bir ayrıntılı erişim sağlar. Azure Kaynak rollerinin Azure AD 'ye yönetici erişimi sağlamak için kullanılamayacağını ve Azure AD rollerinin Azure kaynaklarına özel erişim sağlamayacağını unutmayın.

Yerleşik bir rol kullanılarak bir Azure kaynağına atanabilecek erişim türlerine bazı örnekler şunlardır:

* Bir kullanıcıya abonelikteki sanal makineleri yönetme, başka bir kullanıcıya ise sanal ağları yönetme izni verme
* Bir DBA grubuna abonelikteki SQL veritabanlarını yönetme izni verme
* Bir kullanıcının sanal makineler, web siteleri ve alt ağlar gibi bir kaynak grubundaki tüm kaynakları yönetmesine izin verme
* Bir uygulamaya bir kaynak grubundaki tüm kaynaklara erişim izni verme

Azure AD yönetimi için atanabilecek erişim türlerine örnek olarak şunlar verilebilir:

* Yardım Masası personelinin Kullanıcı parolalarını sıfırlamasına izin ver
* Personelin B2B işbirliği için bir Azure AD örneğine dış kullanıcı davet etmesini sağlar
* Yönetici personelinin oturum açma ve denetim raporlarında okuma erişimine izin ver
* Personelin, parolaları sıfırlama dahil tüm kullanıcıları ve grupları yönetmesine izin verin.

İzin verilen eylemlerin tam listesini anlamak önemlidir. Bu, bir yerleşik rol için, süresi dolan erişimin verilmemesini sağlamak için gerekli değildir. Yerleşik rollerin ve sağladıkları erişimin listesi sürekli olarak gelişmekte, rollerin ve tanımlarının tam listesi, yukarıdaki veya Azure PowerShell cmdlet 'i kullanılarak veya cmdlet 'i kullanılarak görüntülenebilir.

```PowerShell
PS C:\> Get-AzRoleDefinition

Name             : AcrDelete
Id               : <<RoleID>>
IsCustom         : False
Description      : acr delete
Actions          : {Microsoft.ContainerRegistry/registries/artifacts/delete}
NotActions       : {}
DataActions      : {}
NotDataActions   : {}
AssignableScopes : {/}
...
```

veya Azure CLı komutu:

```bash
PS C:\> az role definition list
[
  {
    "assignableScopes": [
      "/"
    ],
    "description": "acr delete",
    "id": "/subscriptions/49b12d1b-4030-431c-8448-39056021c4ab/providers/Microsoft.Authorization/roleDefinitions/c2f4ef07-c644-48eb-af81-4b1b4947fb11",
    "name": "c2f4ef07-c644-48eb-af81-4b1b4947fb11",
    "permissions": [
      {
        "actions": [
          "Microsoft.ContainerRegistry/registries/artifacts/delete"
        ],
        "dataActions": [],
        "notActions": [],
        "notDataActions": []
      }
    ],
    "roleName": "AcrDelete",
    "roleType": "BuiltInRole",
    "type": "Microsoft.Authorization/roleDefinitions"
  },
...
```

Ayrıca, gereken şekilde özel Azure Kaynak rolleri oluşturmak da mümkündür. Bu özel roller Azure portal, PowerShell veya Azure CLı aracılığıyla oluşturulabilir. Özel roller oluştururken, rolün amacının benzersiz olduğundan ve işlevinin var olan bir Azure Kaynak rolü tarafından sağlanmış olmadığından emin olmak önemlidir. Bu, devam eden yönetim karmaşıklığını azaltır ve güvenlik sorumluları gereksiz ayrıcalıkları alma riskini azaltır. Bir örnek, yerleşik Azure Kaynak rolleri, "sanal makine katılımcısı" ve "sanal makine Yöneticisi oturum açma" arasında yer alan özel bir Azure Kaynak rolü oluşturmaktır.

Özel rol, şu erişimi veren var olan katkıda bulunan rolüne bağlı olabilir:

| Azure Kaynağı | Erişim Düzeyi |
| --- | --- |
| Virtual Machines | Yönetebilir, ancak erişemez |
| VM 'ye bağlı sanal ağ | Erişilemiyor |
| VM 'ye bağlı depolama | Erişilemiyor |
|

Özel rol bu temel erişimi koruyabilir, ancak belirlenen kullanıcılara, sanal makinelerin ağ yapılandırmasını değiştirmek için bazı temel ek ayrıcalıkların kullanılmasına izin verir.

Azure Kaynak rolleri Ayrıca Azure Yönetim Grupları aracılığıyla kaynaklara atanamayacak avantajına sahiptir.

### <a name="azure-management-groups"></a>Azure Yönetim Grupları

Azure Yönetim Grupları, rol atamasının bir Azure kiralamanın içindeki tüm aboneliklerine ve kaynaklarına atanmasını yönetmek üzere bir organizasyon tarafından kullanılabilir. Azure Yönetim Grupları, Azure 'da kuruluşunuzun yapısını hiyerarşik olarak eşleme özelliği de dahil olmak üzere yönetim hiyerarşileri oluşturmanıza olanak tanımak üzere tasarlanmıştır. Ayrı mantıksal varlıklar olarak organizasyon iş birimleri oluşturmak, izinlerin her ekibin belirli gereksinimlerine bağlı olarak bir kurum içinde uygulanmasına olanak sağlar. Azure Yönetim Grupları, altı düzey derinlikte bir yönetim hiyerarşisi tanımlamak için kullanılabilir.

![Yönetim Grupları](media/management-groups.png)

Azure Yönetim Grupları, Azure kiralamanın içindeki Azure abonelikleriyle eşlenir. Bu, kuruluşun belirli iş birimlerine ait Azure kaynaklarını ayırt etmesine olanak tanır ve hem maliyet yönetimi hem de ayrıcalık atama üzerinde ayrıntılı bir denetim düzeyi sağlar.

## <a name="privileged-identity-management-pim"></a>Privileged Identity Management (PıM)

Microsoft, Azure Privileged Identity Management aracılığıyla tam zamanında (JıT) ve yeterli erişimi (JEA) uygulamıştır. Bu hizmet, yönetim personelinin Azure kaynaklarına ayrıcalıklı erişimi denetlemesini, yönetmesini ve izlemesini sağlar. PıM, güvenlik sorumlularının yönetim personeli tarafından bir rol için "uygun" hale getirilmelerini sağlar ve kullanıcıların Azure portal veya PowerShell cmdlet 'leri aracılığıyla rolün etkinleştirilmesini ister. Varsayılan olarak, rol ataması 1 ila 72 saat arasında bir dönem için etkinleştirilebilir. Gerekirse, Kullanıcı rol atamalarına bir uzantı isteyebilir ve rol atamasını kalıcı hale getirme seçeneği vardır. İsteğe bağlı olarak, kullanıcılar uygun rollerinin etkinleştirilmesini istemesi durumunda Multi-Factor Authentication gereksinimi zorlanabilir. Rol etkinleştirmenin ayrılan süresi dolduktan sonra, güvenlik sorumlusu artık rol tarafından verilen ayrıcalıklı erişime sahip değildir.

PıM kullanımı, tam zamanında erişimi olmayan ortamlarda gerçekleşebileceğini veya ayrıcalık atamasının rutin denetimlerini yönetmeyecek ortak ayrıcalık atama sorunlarını önler. Yaygın olarak karşılaşılan bir sorun, yükseltilmiş ayrıcalıkların, yükseltilmiş ayrıcalıklar gerektiren görev tamamlandıktan sonra çok uzun süre sonra verilmekte olduğu bir atamadır. Diğer bir sorun da, diğer benzer güvenlik sorumlularını yapılandırırken bir güvenlik sorumlusuna atanan erişimin kopyalaması aracılığıyla bir ortamda yükseltilmiş ayrıcalıkların uzamasına neden olur.

## <a name="key-design-considerations"></a>Önemli tasarım konuları

En az ayrıcalık güvenliği zorlama amacıyla bir RBAC stratejisi tasarlarken aşağıdaki güvenlik gereksinimleri göz önünde bulundurulmalıdır:

* Ayrıcalıklı erişim istekleri onaylanır
* Yönetim ayrıcalıkları, belirli görevleri gerçekleştirmek için gereken en düşük erişimle kısıtlıdır
* Yönetim ayrıcalıkları, belirli görevleri gerçekleştirmek için gereken en az süre ile sınırlıdır
* Verilen yönetim ayrıcalıklarına ilişkin düzenli incelemeler az alınmıştır

RBAC stratejisi tasarlama işlemi, farklı iş rolleri ve yükseltilmiş ayrıcalıklar gerektiren iş türü ve sıklığı arasındaki erişimin farkını anlamak için iş işlevlerinin ayrıntılı bir şekilde gözden geçirilmesini gerektirir. Bir yedekleme operatörü, bir güvenlik yöneticisi ve bir denetçi arasındaki işlev farkı, farklı zamanlarda farklı düzeylerde farklı düzeylerde erişim gerektirir.

## <a name="validate-requests-for-access"></a>Erişim isteklerini doğrulama

Yükseltilmiş ayrıcalıkların açıkça onaylanmış olması gerekir. Bunu desteklemek için, ek ayrıcalıkların tüm isteklerinin meşru olduğunu doğrulamaya yönelik bir onay süreci geliştirmeli ve uygun bir personele sorumlu olması gerekir. Privileged Identity Management rol atamasını onaylamak için birden çok seçenek sağlar. Rol etkinleştirme isteği kendi kendine onaya veya kapıya izin vermek üzere yapılandırılabilir ve tüm rol etkinleştirme isteklerini el ile gözden geçirmek ve onaylamak için aday onaylayanlar gerektirir. Etkinleştirme istekleri, etkinleştirme isteğine, Bilet numaraları gibi ek destekleyici bilgiler de gerektirecek şekilde yapılandırılabilir.

### <a name="restrict-privilege-based-on-duties"></a>Görevleri görevlerine göre kısıtla

Ayrıcalıkların yük ataması yaygın bir BT güvenliği saldırı vektörü olduğundan, güvenlik Ilkelerine verilen ayrıcalık düzeyini kısıtlamak kritik öneme sahiptir. Yönetilen kaynak türleri ve sorumlu takımlar, günlük görevler için gereken en düşük ayrıcalık düzeyi atanabilmesi için değerlendirilmelidir. Günlük görevler için gerekenlerden daha fazla geçen ek ayrıcalıklar yalnızca belirli bir görevi gerçekleştirmek için gereken süre için verilmemelidir. Bunun bir örneği, müşterinin yöneticisine "katkıda bulunan" erişimi sağlamak, ancak bu kişilere, geçici yüksek düzeyde erişim gerektiren belirli bir görevde bir Azure kaynağı için "sahip" izinleri istemesine izin verir.

Bu, her bir yöneticinin en kısa sürede yalnızca yükseltilmiş erişime sahip olmasını sağlar. Bu uygulamalara bağlılık, BT altyapısı için genel saldırı yüzeyini azaltır.

### <a name="regular-evaluation-of-administrative-privilege"></a>Yönetim ayrıcalığının düzenli değerlendirmesi

Bir ortamdaki güvenlik sorumlularının, doğru ayrıcalığa sahip olduğundan emin olmak için düzenli olarak denetlenmesi çok önemlidir. Microsoft Azure, Azure güvenlik sorumlularına atanan ayrıcalıkları denetlemek ve değerlendirmek için bir dizi yöntem sunar. Privileged Identity Management, yönetim personelinin güvenlik Ilkelerine verilen rollerin düzenli olarak "erişim Incelemeleri" gerçekleştirmesini sağlar. Azure Kaynak rolü atamasını ve Azure Active Directory yönetim rolü atamasını denetlemek için erişim gözden geçirmesi uygulanabilir. Bir erişim Incelemesi aşağıdaki özelliklerle yapılandırılabilir:

* **Adı gözden geçirin ve başlangıç ve bitiş tarihlerini gözden geçirin**: İncelemeler, aday kullanıcıların bunları tamamlaması için yeterince uzun olacak şekilde yapılandırılmalıdır.

* **Gözden geçirilecek rol**: Her erişim Incelemesi, tek bir Azure rolüne odaklanır.

* **Aday gözden geçirenler**: İnceleme gerçekleştirmeye yönelik üç seçenek vardır. Bunu gerçekleştirmek için başka birisine atayabilirsiniz, kendiniz yapabilirsiniz veya her bir kullanıcının kendi erişimini incelemesini sağlayabilirsiniz.

* **Kullanıcıların erişim için bir neden sağlamasını iste**: Kullanıcıların, erişim incelemesi tamamlanırken ayrıcalık düzeylerini korumak için bir neden girmesi gerekebilir.

Bekleyen erişim Incelemelerinin ilerlemesi, Azure portal bir pano aracılığıyla istediğiniz zaman izlenebilir. Gözden geçirilmekte olan role erişimi, erişim Incelemesi tamamlanana kadar değişmeden kalır. Ayrıca, bir aday zaman dönemi içinde tüm PıM kullanıcı atamalarını ve etkinleştirmeleri [denetlemek](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-how-to-use-audit-log) mümkündür.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Avustralya 'Da sistem izleme](system-monitor.md)makalesinde bulunan makaleyi gözden geçirin.
