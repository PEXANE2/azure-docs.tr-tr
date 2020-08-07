---
title: Yönetilen kimliklerle ilgili SSS ve bilinen sorunlar-Azure AD
description: Azure kaynakları için yönetilen kimliklerle ilgili bilinen sorunlar.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 2097381a-a7ec-4e3b-b4ff-5d2fb17403b6
ms.service: active-directory
ms.subservice: msi
ms.devlang: ''
ms.topic: conceptual
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 08/06/2020
ms.author: markvi
ms.collection: M365-identity-device-management
ms.custom: has-adal-ref
ms.openlocfilehash: 484f5443702b1151dc3f83af11b5f12a2c0a177d
ms.sourcegitcommit: 4e5560887b8f10539d7564eedaff4316adb27e2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87902244"
---
# <a name="faqs-and-known-issues-with-managed-identities-for-azure-resources"></a>Azure kaynakları için yönetilen kimliklerle ilgili SSS ve bilinen sorunlar

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

## <a name="frequently-asked-questions-faqs"></a>Sık sorulan sorular (SSS)

> [!NOTE]
> Azure kaynakları için yönetilen kimlikler, Yönetilen Hizmet Kimliği (MSI) olarak bilinen hizmetin yeni adıdır.


### <a name="how-can-you-find-resources-that-have-a-managed-identity"></a>Yönetilen bir kimliğe sahip kaynakları nasıl bulabilirsiniz?

Aşağıdaki Azure CLı komutunu kullanarak, sistem tarafından atanan yönetilen kimliğe sahip kaynakların listesini bulabilirsiniz: 

`az resource list --query "[?identity.type=='SystemAssigned'].{Name:name,  principalId:identity.principalId}" --output table`




### <a name="do-managed-identities-have-a-backing-app-object"></a>Yönetilen kimliklerin bir yedekleme uygulaması nesnesi var mı?

Hayır. Yönetilen kimlikler ve Azure AD Uygulaması kayıtları, dizinde aynı şey değildir. 

Uygulama kayıtları iki bileşeni vardır: uygulama nesnesi + bir hizmet sorumlusu nesnesi. Azure kaynakları için Yönetilen kimlikler şu bileşenlerden yalnızca birine sahiptir: bir hizmet sorumlusu nesnesi. 

Yönetilen kimliklerin dizinde uygulama nesnesi yoktur, bu da genellikle MS Graph için uygulama izinleri vermek için kullanılır. Bunun yerine, Yönetilen kimlikler için MS Graph izinlerinin doğrudan hizmet sorumlusuna verilmesi gerekir.  

### <a name="does-managed-identities-for-azure-resources-work-with-azure-cloud-services"></a>Azure kaynakları için Yönetilen kimlikler Azure Cloud Services ile çalışır mi?

Hayır, Azure Cloud Services Azure kaynakları için yönetilen kimlikleri desteklemeye yönelik bir plan yoktur.

### <a name="what-is-the-credential-associated-with-a-managed-identity-how-long-is-it-valid-and-how-often-is-it-rotated"></a>Yönetilen bir kimlikle ilişkili kimlik bilgileri nedir? Ne kadar geçerlidir ve ne sıklıkta döndürülür?

> [!NOTE]
> Yönetilen kimliklerin kimlik doğrulaması, bildirimde bulunulmadan değişikliğe tabi olan dahili bir uygulama ayrıntısıyla yapılır.

Yönetilen kimlikler sertifika tabanlı kimlik doğrulaması kullanır. Her yönetilen kimliğin kimlik bilgileri 90 gün süre sonu içerir ve 45 gün sonra alınır.

### <a name="what-is-the-security-boundary-of-managed-identities-for-azure-resources"></a>Azure kaynakları için yönetilen kimliklerin güvenlik sınırı nedir?

Kimliğin güvenlik sınırı, eklendiği kaynaktır. Örneğin, Azure kaynakları için yönetilen kimliklere sahip bir sanal makine için güvenlik sınırı, sanal makinedir. Bu VM üzerinde çalışan herhangi bir kod, Azure kaynakları uç noktası ve istek belirteçleri için yönetilen kimlikleri çağırabilir. Azure kaynakları için yönetilen kimlikleri destekleyen diğer kaynaklarla benzer deneyimdir.

### <a name="what-identity-will-imds-default-to-if-dont-specify-the-identity-in-the-request"></a>İstekte kimliği belirtmezseniz hangi kimlik varsayılan olarak silinecek?

- Sistem tarafından atanan yönetilen kimlik etkinse ve istekte hiçbir kimlik belirtilmemişse, ıDS, sistem tarafından atanan yönetilen kimliğe varsayılan olarak ayarlanır.
- Sistem tarafından atanan yönetilen kimlik etkin değilse ve yalnızca bir Kullanıcı yönetilen kimlik varsa, ıSE 'ler varsayılan olarak bu tek kullanıcı tarafından atanan yönetilen kimlik olur. 
- Sistem tarafından atanan yönetilen kimlik etkinleştirilmemişse ve birden çok kullanıcı atanmış Yönetilen kimlikler varsa, istekte yönetilen bir kimlik belirtilmesi gerekir.



### <a name="will-managed-identities-be-recreated-automatically-if-i-move-a-subscription-to-another-directory"></a>Bir aboneliği başka bir dizine taşıdığımda, Yönetilen kimlikler otomatik olarak yeniden oluşturulur mi?

Hayır. Bir aboneliği başka bir dizine taşırsanız, bunları el ile yeniden oluşturmanız ve Azure rol atamalarını yeniden sağlamanız gerekir.
- Sistem tarafından atanan Yönetilen kimlikler için: devre dışı bırakın ve yeniden etkinleştirin. 
- Kullanıcı tarafından atanan Yönetilen kimlikler için: silin, yeniden oluşturun ve bunları gereken kaynaklara yeniden ekleyin (örneğin, sanal makineler)

### <a name="can-i-use-a-managed-identity-to-access-a-resource-in-a-different-directorytenant"></a>Farklı bir dizin/Kiracıdaki bir kaynağa erişmek için yönetilen bir kimlik kullanabilir miyim?

Hayır. Yönetilen kimlikler Şu anda çapraz dizin senaryolarını desteklemez. 

### <a name="what-azure-rbac-permissions-are-required-to-managed-identity-on-a-resource"></a>Bir kaynaktaki yönetilen kimlik için hangi Azure RBAC izinleri gerekir? 

- Sistem tarafından atanan yönetilen kimlik: kaynak üzerinde yazma izinlerine sahip olmanız gerekir. Örneğin sanal makineler için Microsoft.Compute/virtualMachines/write iznine ihtiyaç duyulur. Bu eylem, [sanal makine katılımcısı](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#virtual-machine-contributor)gibi kaynağa özgü yerleşik rollere dahildir.
- Kullanıcı tarafından atanan yönetilen kimlik: kaynak üzerinde yazma izinlerine sahip olmanız gerekir. Örneğin sanal makineler için Microsoft.Compute/virtualMachines/write iznine ihtiyaç duyulur. Yönetilen kimliğe göre [yönetilen kimlik operatörü](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#managed-identity-operator) rolü atamaya ek olarak.



## <a name="known-issues"></a>Bilinen sorunlar

### <a name="automation-script-fails-when-attempting-schema-export-for-managed-identities-for-azure-resources-extension"></a>"Otomasyon betiği", Azure kaynakları uzantısı için Yönetilen kimlikler için şema dışarı aktarma denemesi sırasında başarısız oluyor

Bir VM 'de Azure kaynakları için Yönetilen kimlikler etkinleştirildiğinde, VM için "Otomasyon betiği" özelliğini veya kaynak grubunu kullanmaya çalışırken aşağıdaki hata gösterilir:

![Azure kaynakları için Yönetilen kimlikler Otomasyon betiği dışarı aktarma hatası](./media/msi-known-issues/automation-script-export-error.png)

Azure kaynakları için Yönetilen kimlikler VM Uzantısı (2019 Ocak 'ta kullanımdan kaldırma için planlanmış) Şu anda, şemasını bir kaynak grubu şablonuna aktarma özelliğini desteklememektedir. Sonuç olarak, oluşturulan şablon kaynaktaki Azure kaynakları için yönetilen kimlikleri etkinleştirmek üzere yapılandırma parametrelerini göstermez. Bu bölümler, [bir Azure VM 'de bir şablonları kullanarak Azure kaynakları için yönetilen kimlikleri yapılandırma](qs-configure-template-windows-vm.md)içindeki örnekleri izleyerek el ile eklenebilir.

Şema dışa aktarma işlevselliği Azure kaynakları için Yönetilen kimlikler için kullanılabilir hale geldiğinde (Ocak 2019 ' de kullanımdan kaldırma için planlanmış), [VM uzantıları Içeren kaynak gruplarını dışarı aktarma](../../virtual-machines/extensions/export-templates.md#supported-virtual-machine-extensions)bölümünde listelenecektir.

### <a name="vm-fails-to-start-after-being-moved-from-resource-group-or-subscription"></a>VM, kaynak grubu veya abonelikten taşındıktan sonra başlatılamaz

Bir VM 'yi çalışır durumda taşırsanız, taşıma sırasında çalışmaya devam eder. Ancak, taşıma işleminden sonra VM durdurulur ve yeniden başlatılırsa başlatılamaz. Bu sorun, VM 'nin Azure kaynakları kimliği için yönetilen kimliklere yönelik başvuruyu güncelleştirmediği ve eski kaynak grubunda ona işaret etmeye devam ettiğinden meydana gelir.

**Geçici çözüm** 
 
Azure kaynakları için yönetilen kimliklerin doğru değerlerini kullanabilmesi için VM 'de bir güncelleştirme tetikleyin. Azure kaynak kimliği için yönetilen kimliklerin başvurusunu güncelleştirmek üzere bir VM özelliği değişikliği yapabilirsiniz. Örneğin, aşağıdaki komutla sanal makinede yeni bir etiket değeri ayarlayabilirsiniz:

```azurecli-interactive
 az  vm update -n <VM Name> -g <Resource Group> --set tags.fixVM=1
```
 
Bu komut, VM 'de 1 değeri ile yeni bir "fixVM" etiketi ayarlar. 
 
Bu özelliği ayarlayarak, VM Azure kaynakları Kaynak URI 'SI için doğru yönetilen kimliklerle güncelleştirilir ve sonra VM 'yi başlatabilmelisiniz. 
 
VM başlatıldıktan sonra, etiket aşağıdaki komut kullanılarak kaldırılabilir:

```azurecli-interactive
az vm update -n <VM Name> -g <Resource Group> --remove tags.fixVM
```



### <a name="transferring-a-subscription-between-azure-ad-directories"></a>Azure AD dizinleri arasında abonelik aktarma

Yönetilen kimlikler bir abonelik taşındığında/başka bir dizine aktarıldığında güncellenmez. Sonuç olarak, var olan sistem tarafından atanan veya Kullanıcı tarafından atanan Yönetilen kimlikler bozulur. 

Başka bir dizine taşınmış bir abonelikte Yönetilen kimlikler için geçici çözüm:

 - Sistem tarafından atanan Yönetilen kimlikler için: devre dışı bırakın ve yeniden etkinleştirin. 
 - Kullanıcı tarafından atanan Yönetilen kimlikler için: silin, yeniden oluşturun ve bunları gereken kaynaklara yeniden ekleyin (örneğin, sanal makineler)

Daha fazla bilgi için bkz. [Azure aboneliğini farklı bir Azure AD dizinine aktarma (Önizleme)](../../role-based-access-control/transfer-subscription.md).

### <a name="moving-a-user-assigned-managed-identity-to-a-different-resource-groupsubscription"></a>Kullanıcı tarafından atanan yönetilen bir kimliği farklı bir kaynak grubuna/aboneliğe taşıma

Kullanıcı tarafından atanan bir yönetilen kimliğin farklı bir kaynak grubuna taşınması desteklenmez.
