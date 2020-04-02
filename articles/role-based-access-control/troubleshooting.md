---
title: Sorun Giderme Azure RBAC
description: Azure rol tabanlı erişim denetimi (Azure RBAC) ile ilgili sorun giderme sorunları.
services: azure-portal
documentationcenter: na
author: rolyon
manager: mtillman
ms.assetid: df42cca2-02d6-4f3c-9d56-260e1eb7dc44
ms.service: role-based-access-control
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/18/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: seohack1
ms.openlocfilehash: 09d5b7a126a1b8832bfe40e2e25dd4000d5d9155
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80548281"
---
# <a name="troubleshoot-azure-rbac"></a>Sorun Giderme Azure RBAC

Bu makalede, rolleri kullanırken ne beklemeniz gerektiğini bilmeniz ve erişim sorunlarını gidermeniz için Azure rol tabanlı erişim denetimi (Azure RBAC) ile ilgili bazı sık sorulan soruları yanıtlar.

## <a name="azure-role-assignments-limit"></a>Azure rol atamaları sınırı

Azure, abonelik başına en fazla **2000** rol atamasını destekler. Bir rol atamaya çalıştığınızda "Artık rol atamaları oluşturulamaz (kod: RoleAssignmentLimitExceeded)" hata iletisini alırsanız, abonelikteki rol atamalarının sayısını azaltmaya çalışın.

> [!NOTE]
> Abonelik başına **2000** rol atamaları sınırı sabittir ve artırılamaz.

Bu sınıra yaklaşıyorsanız, rol atamalarının sayısını azaltmanın bazı yolları şunlardır:

- Gruplara kullanıcı ekleyin ve bunun yerine gruplara roller atayın. 
- Birden çok yerleşik rolü özel bir rolle birleştirin. 
- Abonelik veya yönetim grubu gibi daha yüksek bir kapsamda ortak rol atamaları yapın.
- Azure AD Premium P2'niz varsa, rol atamalarını kalıcı olarak atananlar yerine [Azure AD Ayrıcalıklı Kimlik Yönetimi'nde](../active-directory/privileged-identity-management/pim-configure.md) uygun hale getirin. 
- Ek bir abonelik ekleyin. 

Rol atamalarının sayısını almak [için, Azure portalındaki Access denetimi (IAM) sayfasında grafiği](role-assignments-list-portal.md#list-number-of-role-assignments) görüntüleyebilirsiniz. Aşağıdaki Azure PowerShell komutlarını da kullanabilirsiniz:

```azurepowershell
$scope = "/subscriptions/<subscriptionId>"
$ras = Get-AzRoleAssignment -Scope $scope | Where-Object {$_.scope.StartsWith($scope)}
$ras.Count
```

## <a name="problems-with-azure-role-assignments"></a>Azure rol atamalarıyla ilgili sorunlar

- Ekle rol **Add** > **ataması** seçeneği devre dışı bırakıldığı ndan veya "Nesne kimliğine sahip istemcinin eylem gerçekleştirme yetkisi yoktur" izinleri hatası aldığınız için **Access denetimindeki (IAM)** Azure portalına bir rol ataması `Microsoft.Authorization/roleAssignments/write` ekleyemiyorsanız, rolü atamaya çalıştığınız kapsamda [Sahibi](built-in-roles.md#owner) veya Kullanıcı Erişim [Yöneticisi](built-in-roles.md#user-access-administrator) gibi izni olan bir rolle şu anda oturum açmış durumda olup olmadığınızı denetleyin.

## <a name="problems-with-custom-roles"></a>Özel rollerle ilgili sorunlar

- Özel bir rol oluşturma yla ilgili adımlara ihtiyacınız varsa, [Azure portalı](custom-roles-portal.md) (şu anda önizlemede), [Azure PowerShell'i](tutorial-custom-role-powershell.md)veya [Azure CLI'yi](tutorial-custom-role-cli.md)kullanarak özel rol öğreticilerine bakın.
- Varolan bir özel rolü güncelleştiremiyorsanız, Şu anda [Sahip](built-in-roles.md#owner) veya [Kullanıcı Erişim Yöneticisi](built-in-roles.md#user-access-administrator)gibi `Microsoft.Authorization/roleDefinition/write` izni olan bir rol atanmış bir kullanıcıyla oturum açıp oturum açmadığınızı denetleyin.
- Özel bir rolü silemiyorsanız ve "Varolan rol atamaları na başvuruyor (kod: RoleDefinitionHasAssignments)" hata iletisini alıyorsanız, özel rolü kullanmaya devam eden rol atamaları vardır. Bu rol atamalarını kaldırın ve özel rolü silmeyi tekrar deneyin.
- Yeni bir özel rol oluşturmaya çalıştığınızda "Rol tanımı sınırı aşıldı. Yeni bir özel rol oluşturmaya çalıştığınızda, kullanılmayan özel rolleri sildiğinizde artık rol tanımı oluşturulamaz (kod: RoleDefinitionLimitExceeded)." Azure, bir dizinde **5000'e** kadar özel rolü destekler. (Azure Almanya ve Azure China 21Vianet için sınır 2000 özel roldür.)
- Özel bir rolü güncelleştirmeye çalıştığınızda "Istemcinin 'Microsoft.Authorization/roleDefinitions/write' kapsamında 'Microsoft.Authorization/roleDefinitions/write' eylemini gerçekleştirme iznine sahip olmasına benzer bir hata alırsanız, özel bir rolü güncelleştirmeye çalıştığınızda bağlantılı abonelik bulunamadı, dizinde bir veya daha fazla [devratılabilir kapsamın](role-definitions.md#assignablescopes) silinip silinmediğini denetleyin. Kapsam silinmişse, şu anda bir self servis çözüm olmadığı için destek bileti oluşturun.

## <a name="custom-roles-and-management-groups"></a>Özel roller ve yönetim grupları

- Özel bir rolde `AssignableScopes` yalnızca bir yönetim grubu tanımlayabilirsiniz. Bir yönetim grubu `AssignableScopes` eklemek şu anda önizlemededir.
- Yönetim grubu `DataActions` kapsamında özel roller atanamaz.
- Azure Kaynak Yöneticisi, rol tanımının devredilebilir kapsamında yönetim grubunun varlığını doğrulamaz.
- Özel roller ve yönetim grupları hakkında daha fazla bilgi için [bkz.](../governance/management-groups/overview.md#custom-rbac-role-definition-and-assignment)

## <a name="transferring-a-subscription-to-a-different-directory"></a>Aboneliği farklı bir dizine aktarma

- Bir aboneliği farklı bir Azure REKLAM dizinine aktarma adımlarına ihtiyacınız varsa, azure [aboneliğinin sahipliğini başka bir hesaba aktar'a](../cost-management-billing/manage/billing-subscription-transfer.md)bakın.
- Bir aboneliği farklı bir Azure REKLAM dizinine aktarıyorsanız, tüm rol atamaları kaynak Azure REKLAM dizininden **kalıcı olarak** silinir ve hedef Azure REKLAM dizinine geçirilmeyecek. Hedef dizindeki rol atamalarınızı yeniden oluşturmanız gerekir. Ayrıca, Azure kaynakları için yönetilen kimlikleri el ile yeniden oluşturmanız gerekir. Daha fazla bilgi için, [yönetilen kimliklerle ilgili SSS'lere ve bilinen sorunlara](../active-directory/managed-identities-azure-resources/known-issues.md)bakın.
- Bir Azure AD Global Yöneticisiyseniz ve dizinler arasında aktarıldıktan sonra bir aboneye erişiminiz yoksa, abonelmeye erişmek için [erişiminizi](elevate-access-global-admin.md) geçici olarak yükseltmek **için Azure kaynaklarının Access yönetimini** kullanın.

## <a name="issues-with-service-admins-or-co-admins"></a>Hizmet yöneticileri veya ortak yöneticilerle ilgili sorunlar

- Hizmet yöneticisi veya Yardımcı yöneticilerle sorun yaşıyorsanız, [Azure abonelik yöneticileri ekle ve değiştirme,](../cost-management-billing/manage/add-change-subscription-administrator.md) Azure rolleri ve Azure REKLAM yöneticisi [rolleri'ni](rbac-and-directory-admin-roles.md)görün.

## <a name="access-denied-or-permission-errors"></a>Erişim reddedildi veya izin hataları

- Bir kaynak oluşturmaya çalıştığınızda "Nesne kimliği ne olan istemcinin kapsam üzerinde işlem yapma yetkisi yoktur (kod: YetkilendirmeBaşarısız)" izinleri hatası alırsanız, şu anda seçili kapsamda kaynağa izin yazan bir rol atanmış bir kullanıcıyla oturum açmış durumda olup olmadığınızı denetleyin. Örneğin bir kaynak grubundaki sanal makineleri yönetmek için kaynak grubunda (veya üst kapsamda) [Sanal Makine Katılımcısı](built-in-roles.md#virtual-machine-contributor) rolüne sahip olmanız gerekir. Yerleşik rollerin izinlerinin yer aldığı liste için bkz. [Azure kaynakları için yerleşik roller](built-in-roles.md).
- Bir destek bileti oluşturmaya veya güncelleştirmeye çalıştığınızda "Destek isteği oluşturma izniniz yok" izinleri hatası alırsanız, Destek `Microsoft.Support/supportTickets/write` [İsteği Katılımcısı](built-in-roles.md#support-request-contributor)gibi izni olan bir rol le şu anda oturum açmış durumda olup olmadığınızı kontrol edin.

## <a name="role-assignments-with-unknown-security-principal"></a>Bilinmeyen güvenlik ilkesiyle rol atamaları

Bir güvenlik ilkesine (kullanıcı, grup, hizmet sorumlusu veya yönetilen kimlik) bir rol atarsanız ve daha sonra rol atamasını kaldırmadan bu güvenlik ilkesini silerseniz, rol atamasının güvenlik ilkesi türü **Bilinmeyen**olarak listelenir. Aşağıdaki ekran görüntüsünde Azure portalında bir örnek gösterilir. Güvenlik soyadı adı **Kimlik silinmiş** olarak listelenir ve **Kimlik artık yok.** 

![Web uygulaması kaynak grubu](./media/troubleshooting/unknown-security-principal.png)

Bu rol atamasını Azure PowerShell'i kullanarak `DisplayName` listelerseniz, boş ve Bilinmeyene ayarlanmış bir `ObjectType` küme görürsünüz. Örneğin, [Get-AzRoleAssignment](/powershell/module/az.resources/get-azroleassignment) aşağıdakilere benzer bir rol ataması döndürür:

```
RoleAssignmentId   : /subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Authorization/roleAssignments/22222222-2222-2222-2222-222222222222
Scope              : /subscriptions/11111111-1111-1111-1111-111111111111
DisplayName        :
SignInName         :
RoleDefinitionName : Storage Blob Data Contributor
RoleDefinitionId   : ba92f5b4-2d11-453d-a403-e96b0029c9fe
ObjectId           : 33333333-3333-3333-3333-333333333333
ObjectType         : Unknown
CanDelegate        : False
```

Benzer şekilde, bu rol ataması Azure CLI'yi `principalName`kullanarak listelerseniz, boş bir . Örneğin, [az rol atama listesi](/cli/azure/role/assignment#az-role-assignment-list) aşağıdakilere benzer bir rol ataması döndürür:

```
{
    "canDelegate": null,
    "id": "/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Authorization/roleAssignments/22222222-2222-2222-2222-222222222222",
    "name": "22222222-2222-2222-2222-222222222222",
    "principalId": "33333333-3333-3333-3333-333333333333",
    "principalName": "",
    "roleDefinitionId": "/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Authorization/roleDefinitions/ba92f5b4-2d11-453d-a403-e96b0029c9fe",
    "roleDefinitionName": "Storage Blob Data Contributor",
    "scope": "/subscriptions/11111111-1111-1111-1111-111111111111",
    "type": "Microsoft.Authorization/roleAssignments"
}
```

Bu rol atamaları bırakmak bir sorun değildir, ancak diğer rol atamaları benzer adımları kullanarak bunları kaldırabilirsiniz. Rol atamalarının nasıl kaldırılış ları hakkında bilgi için [Azure portalına](role-assignments-portal.md#remove-a-role-assignment), [Azure PowerShell](role-assignments-powershell.md#remove-a-role-assignment)veya [Azure CLI'ye](role-assignments-cli.md#remove-a-role-assignment) bakın

PowerShell'de, nesne kimliği ve rol tanımı adını kullanarak rol atamalarını kaldırmaya çalışırsanız ve birden fazla rol ataması parametrelerinizle eşleşirse, hata iletisi alırsınız: "Sağlanan bilgiler rol atamasının haritasını çıkarmaz". Aşağıdaki hata iletisinin bir örneğini gösterir:

```
PS C:\> Remove-AzRoleAssignment -ObjectId 33333333-3333-3333-3333-333333333333 -RoleDefinitionName "Storage Blob Data Contributor"

Remove-AzRoleAssignment : The provided information does not map to a role assignment.
At line:1 char:1
+ Remove-AzRoleAssignment -ObjectId 33333333-3333-3333-3333-333333333333 ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
+ CategoryInfo          : CloseError: (:) [Remove-AzRoleAssignment], KeyNotFoundException
+ FullyQualifiedErrorId : Microsoft.Azure.Commands.Resources.RemoveAzureRoleAssignmentCommand
```

Bu hata iletisini alırsanız, `-Scope` `-ResourceGroupName` parametreleri de belirttiğinden emin olun.

```
PS C:\> Remove-AzRoleAssignment -ObjectId 33333333-3333-3333-3333-333333333333 -RoleDefinitionName "Storage Blob Data Contributor" - Scope /subscriptions/11111111-1111-1111-1111-111111111111
```

## <a name="role-assignment-changes-are-not-being-detected"></a>Rol atama değişiklikleri algılanmıyor

Azure Kaynak Yöneticisi bazen performansı artırmak için yapılandırmaları ve verileri önbelleğe adar. Rol atamaları eklediğinizde veya kaldırdığınızda, değişikliklerin etkili olması 30 dakika kadar sürebilir. Azure portalını, Azure PowerShell'i veya Azure CLI'yi kullanıyorsanız, oturumunuzu oturum uzatarak ve oturum açarak rol atama değişikliklerinizin yenilenmesini zorlayabilirsiniz. REST API çağrılarıyla rol atama değişiklikleri yapıyorsanız, erişim belirtecinizi yenileyerek yenilenmeye zorlayabilirsiniz.

Yönetim grubu kapsamında bir rol ataması ekliyor veya `DataActions`kaldırıyorsanız ve rolü varsa, veri düzlemindeki erişim birkaç saat için güncelleştirilemeyebilir. Bu yalnızca yönetim grubu kapsamı ve veri düzlemi için geçerlidir.

## <a name="web-app-features-that-require-write-access"></a>Yazma erişimi gerektiren Web uygulaması özellikleri

Bir kullanıcıya tek bir web uygulamasına salt okunur erişim izni verirseniz, beklemediğiniz bazı özellikler devre dışı bırakılır. Aşağıdaki yönetim yetenekleri, bir web uygulamasına (Katkıda Bulunan veya Sahip) **yazma** erişimi gerektirir ve salt okunur senaryoda kullanılamaz.

* Komutlar (başlangıç, durdurma, vb.)
* Genel yapılandırma, ölçek ayarları, yedekleme ayarları ve izleme ayarları gibi ayarları değiştirme
* Yayımlama kimlik bilgilerine ve uygulama ayarları ve bağlantı dizeleri gibi diğer sırlara erişme
* Akış günlükleri
* Tanılama günlükleri yapılandırması
* Konsol (komut istemi)
* Etkin ve son dağıtımlar (yerel git sürekli dağıtım için)
* Tahmini harcama
* Web testleri
* Sanal ağ (yalnızca sanal ağ daha önce yazma erişimi olan bir kullanıcı tarafından yapılandırıldıysa okuyucu tarafından görülebilir).

Bu kutucuklardan hiçbirine erişemiyorsanız, yöneticinizden Web uygulamasına Katkıda Bulunan'dan erişim istemeniz gerekir.

## <a name="web-app-resources-that-require-write-access"></a>Yazma erişimi gerektiren web uygulaması kaynakları

Web uygulamaları, etkileşimde olan birkaç farklı kaynağın varlığıyla karmaşıktır. Aşağıda, birkaç web sitesi içeren tipik bir kaynak grubu vereme

![Web uygulaması kaynak grubu](./media/troubleshooting/website-resource-model.png)

Sonuç olarak, birisine yalnızca web uygulamasına erişim izni verirseniz, Azure portalındaki web sitesi bıçaküzerindeki işlevlerin çoğu devre dışı bırakılır.

Bu öğeler, web sitenize karşılık gelen **Uygulama Hizmeti planına** **yazma** erişimi gerektirir:  

* Web uygulamasının fiyatlandırma katmanını görüntüleme (Ücretsiz veya Standart)  
* Ölçek yapılandırması (örnek sayısı, sanal makine boyutu, otomatik ölçek ayarları)  
* Kotalar (depolama, bant genişliği, CPU)  

Bu öğeler, web sitenizi içeren tüm **Kaynak grubuna** **yazma** erişimi gerektirir:  

* TLS/SSL Sertifikaları ve ciltlemeleri (TLS/SSL sertifikaları aynı kaynak grubundaki siteler ve coğrafi konum daki siteler arasında paylaşılabilir)  
* Uyarı kuralları  
* Otomatik ölçeklendirme ayarları  
* Uygulama öngörüleri bileşenleri  
* Web testleri  

## <a name="virtual-machine-features-that-require-write-access"></a>Yazma erişimi gerektiren sanal makine özellikleri

Web uygulamalarına benzer şekilde, sanal makine bıçağındaki bazı özellikler sanal makineye veya kaynak grubundaki diğer kaynaklara yazma erişimi gerektirir.

Sanal makineler Alan adları, sanal ağlar, depolama hesapları ve uyarı kurallarıyla ilişkilidir.

Bu öğeler **Sanal makineye** **yazma** erişimi gerektirir:

* Uç Noktalar  
* IP adresleri  
* Diskler  
* Uzantıları  

Bunlar, hem Sanal **makineye**hem de içinde olduğu **Kaynak grubuna** (Etki Alanı adı ile birlikte) **yazma** erişimi gerektirir:  

* Kullanılabilirlik kümesi  
* Yük dengeli seti  
* Uyarı kuralları  

Bu kutucuklardan hiçbirine erişemiyorsanız, yöneticinizden Kaynak grubuna Katkıda Bulunan'dan erişim isteyin.

## <a name="azure-functions-and-write-access"></a>Azure Fonksiyonları ve yazma erişimi

[Azure İşlevlerinin](../azure-functions/functions-overview.md) bazı özellikleri yazma erişimi gerektirir. Örneğin, bir kullanıcıya [Okuyucu](built-in-roles.md#reader) rolü atanmışsa, bir işlev uygulaması içindeki işlevleri görüntüleyemez. Portal görüntülenir **(Erişim yok)**.

![İşlev uygulamaları erişim yok](./media/troubleshooting/functionapps-noaccess.png)

Okuyucu **Platform özellikleri** sekmesini tıklatabilir ve ardından bir işlev uygulamasıyla ilgili bazı ayarları (web uygulamasına benzer) görüntülemek için **Tüm ayarları** tıklatabilir, ancak bu ayarların hiçbirini değiştiremez. Bu özelliklere erişmek için [Katılımcı](built-in-roles.md#contributor) rolüne ihtiyacınız olacaktır.

## <a name="next-steps"></a>Sonraki adımlar

- [Konuk kullanıcılar için sorun giderme](role-assignments-external-users.md#troubleshoot)
- [RBAC ve Azure portalını kullanarak Azure kaynaklarına erişimi yönetme](role-assignments-portal.md)
- [Azure kaynaklarında RBAC değişiklikleri için etkinlik günlüklerini görüntüleme](change-history-report.md)
