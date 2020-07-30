---
title: Azure AD 'ye katılmış cihazlarda yerel yöneticileri yönetme
description: Azure rollerini bir Windows cihazının yerel Yöneticiler grubuna atamayı öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: how-to
ms.date: 06/28/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: ravenn
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5d3082e3dc45102bc8700c7d1285ef832d09712a
ms.sourcegitcommit: e71da24cc108efc2c194007f976f74dd596ab013
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87419827"
---
# <a name="how-to-manage-the-local-administrators-group-on-azure-ad-joined-devices"></a>Azure AD 'ye katılmış cihazlarda yerel Yöneticiler grubunu yönetme

Bir Windows cihazını yönetmek için yerel Yöneticiler grubunun bir üyesi olmanız gerekir. Azure Active Directory (Azure AD) JOIN işleminin bir parçası olarak, Azure AD bu grubun üyeliğini bir cihazda güncelleştirir. Üyelik güncelleştirmesini, iş gereksinimlerinizi karşılayacak şekilde özelleştirebilirsiniz. Bir üyelik güncelleştirmesi, örneğin, yardım masası personelinizin bir cihazda yönetici hakları gerektiren görevler kullanmasını sağlamak istiyorsanız yararlıdır.

Bu makalede, yerel yönetici üyeliği güncelleştirmesinin nasıl çalıştığı ve bir Azure AD birleşimi sırasında nasıl özelleştirebileceğiniz açıklanmaktadır. Bu makalenin içeriği, **karma bir Azure AD 'ye katılmış** cihazlar için geçerlidir.

## <a name="how-it-works"></a>Nasıl çalışır?

Azure AD JOIN kullanarak bir Windows cihazını Azure AD 'ye bağladığınızda, Azure AD aşağıdaki güvenlik sorumlularını cihazdaki yerel Yöneticiler grubuna ekler:

- Azure AD Genel Yöneticisi rolü
- Azure AD Cihaz Yöneticisi rolü 
- Azure AD JOIN 'i gerçekleştiren Kullanıcı   

Yerel Yöneticiler grubuna Azure AD rolleri ekleyerek, cihazdaki herhangi bir şeyi değiştirmeden bir cihazı Azure AD 'de yönetebileceğini kullanıcıları güncelleştirebilirsiniz. Şu anda, bir yönetici rolüne gruplar atayamazsınız.
Azure AD, en az ayrıcalık (PoLP) ilkesini desteklemek için Azure AD cihaz yöneticisi rolünü yerel Yöneticiler grubuna da ekler. Genel yöneticilere ek olarak, bir cihazı yönetmek için *yalnızca* cihaz yöneticisi rolüne atanan kullanıcılara da izin verebilirsiniz. 

## <a name="manage-the-global-administrators-role"></a>Genel Yöneticiler rolünü yönetme

Genel yönetici rolünün üyeliğini görüntülemek ve güncelleştirmek için bkz.:

- [Azure Active Directory bir yönetici rolünün tüm üyelerini görüntüleme](../users-groups-roles/directory-manage-roles-portal.md)
- [Azure Active Directory’de kullanıcılara yönetici rolü atama](../fundamentals/active-directory-users-assign-role-azure-portal.md)


## <a name="manage-the-device-administrator-role"></a>Cihaz Yöneticisi rolünü yönetme 

Azure portal, **cihazlar** sayfasında cihaz yöneticisi rolünü yönetebilirsiniz. **Cihazlar** sayfasını açmak için:

1. [Azure Portal](https://portal.azure.com) genel yönetici olarak oturum açın.
1. *Azure Active Directory*'yi bulun ve seçin.
1. **Yönet** bölümünde, **cihazlar**' a tıklayın.
1. **Cihazlar** sayfasında, **cihaz ayarları**' na tıklayın.

Cihaz Yöneticisi rolünü değiştirmek için, **Azure AD 'ye katılmış cihazlarda ek yerel Yöneticiler**yapılandırın.  

![Ek yerel Yöneticiler](./media/assign-local-admin/10.png)

>[!NOTE]
> Bu seçenek Azure AD Premium kiracı gerektirir. 

Cihaz yöneticileri tüm Azure AD 'ye katılmış cihazlara atanır. Cihaz yöneticilerini belirli bir cihaz kümesiyle kapsamyükleyemezsiniz. Cihaz Yöneticisi rolünü güncelleştirme, etkilenen kullanıcılar üzerinde bir etkisi olması gerekmez. Bir kullanıcının zaten oturum açmış olduğu cihazlarda, aşağıdaki *her iki* eylem gerçekleştiğinde ayrıcalık yükselme gerçekleşir:

- Azure AD 'nin uygun ayrıcalıklara sahip yeni bir birincil yenileme belirteci vermesi için 4 saat geçti. 
- Kullanıcı oturumu kapatır ve kendi profilini yenilemek için kilitleme/kilit açma değil, oturum kapatır.

>[!NOTE]
> Yukarıdaki eylemler, ilgili cihazda daha önce oturum açmamış kullanıcılar için geçerli değildir. Bu durumda, yönetici ayrıcalıkları cihazdaki ilk oturum açtıktan hemen sonra uygulanır. 

## <a name="manage-administrator-privileges-using-azure-ad-groups-preview"></a>Azure AD gruplarını kullanarak yönetici ayrıcalıklarını yönetme (Önizleme)

>[!NOTE]
> Bu özellik şu anda önizleme sürümündedir.

Windows 10 2004 Güncelleştirmesi ile başlayarak, Azure AD 'ye katılmış cihazlarda [Kısıtlanmış Gruplar](/windows/client-management/mdm/policy-csp-restrictedgroups) MDM ilkesiyle yönetici ayrıcalıklarını yönetmek IÇIN Azure AD Grupları ' nı kullanabilirsiniz. Bu ilke, Azure AD 'ye katılmış bir cihazdaki tek tek kullanıcıları veya Azure AD gruplarını yerel Yöneticiler grubuna atamanıza izin verir ve farklı cihaz grupları için ayrı Yöneticiler yapılandırmak için ayrıntı düzeyi sağlar. 

Şu anda, bu ilkeyi yönetmek için Intune 'da Kullanıcı arabirimi yoktur ve [özel OMA-URI ayarları](/mem/intune/configuration/custom-settings-windows-10)kullanılarak yapılandırılması gerekir. Bu ilkeyle ilgili birkaç önemli noktalar: 

- İlke aracılığıyla Azure AD grupları eklemek, gruplar API 'sini yürüterek grubun SID 'sinin elde edilebilir olmasını gerektirir. SID, `securityIdentifier` gruplar API 'sindeki özelliği tarafından tanımlanır.
- Kısıtlanmış Gruplar ilkesi zorlandığında, grubun Üyeler listesinde olmayan geçerli üyeleri kaldırılır. Bu nedenle, bu ilkeyi yeni üyeler veya gruplarla uygulamak, cihazı, cihaz yöneticisi rolünü ve genel yönetici rolünü cihazdan katılmış olan mevcut yöneticiler olarak kaldırır. Mevcut üyeleri kaldırmayı önlemek için, onları kısıtlı gruplar ilkesindeki Üyeler listesinin bir parçası olarak yapılandırmanız gerekir. 
- Bu ilke, yalnızca bir Windows 10 cihazında, Yöneticiler, kullanıcılar, konuklar, Power Users, uzak masaüstü kullanıcıları ve uzaktan yönetim kullanıcıları üzerinde bilinen ve bilinen gruplar için geçerlidir. 
- Kısıtlanmış Gruplar ilkesini kullanarak yerel yöneticileri yönetmek, karma Azure AD 'ye katılmış veya Azure AD 'ye kayıtlı cihazlar için geçerli değildir.
- Kısıtlı Gruplar ilkesi Windows 10 2004 güncelleştirmesinden önce mevcut olsa da, Azure AD gruplarını cihazın yerel Yöneticiler grubunun üyeleri olarak desteklemeiyordu. 

## <a name="manage-regular-users"></a>Normal kullanıcıları yönetme

Azure AD, varsayılan olarak, Azure AD JOIN 'in cihazdaki yönetici grubuna eklediği kullanıcıyı ekler. Normal kullanıcıların yerel yönetici haline gelmesini engellemek istiyorsanız aşağıdaki seçeneklere sahipsiniz:

- [Windows Autopilot](/windows/deployment/windows-autopilot/windows-10-autopilot) -Windows Autopilot, birincil kullanıcının bir yerel yönetici haline gelmesini önleyen bir seçenek sunar. Bunu, [bir Autopilot profili oluşturarak](/intune/enrollment-autopilot#create-an-autopilot-deployment-profile)gerçekleştirebilirsiniz.
- [Toplu kayıt](/intune/windows-bulk-enroll) -bir toplu kayıt bağlamında gerçekleştirilen BIR Azure AD JOIN, otomatik olarak oluşturulan bir kullanıcı bağlamında gerçekleşir. Bir cihaz birleştirildikten sonra oturum açan kullanıcılar Yöneticiler grubuna eklenmez.   

## <a name="manually-elevate-a-user-on-a-device"></a>Bir cihazdaki kullanıcıyı el ile yükseltme 

Azure AD JOIN işlemini kullanmanın yanı sıra, düzenli bir kullanıcıyı belirli bir cihazda yerel yönetici olacak şekilde el ile de indirebilirsiniz. Bu adım, yerel Yöneticiler grubunun zaten bir üyesi olmanızı gerektirir. 

**Windows 10 1709** sürümünden başlayarak bu görevi **Ayarlar-> hesaplarından (diğer kullanıcılar >** gerçekleştirebilirsiniz. **İş veya okul kullanıcısı ekle**' yi seçin, kullanıcının **Kullanıcı HESABı** bölümüne ait UPN 'sini girin ve **Hesap türü** altında *yönetici* ' yi seçin.  
 
Ayrıca, komut istemi kullanarak da kullanıcı ekleyebilirsiniz:

- Kiracı kullanıcılarınız şirket içi Active Directory eşitlendiğinde, kullanın `net localgroup administrators /add "Contoso\username"` .
- Azure AD 'de kiracı kullanıcılarınız oluşturulduysa, şunu kullanın`net localgroup administrators /add "AzureAD\UserUpn"`

## <a name="considerations"></a>Dikkat edilmesi gerekenler 

Cihaz Yöneticisi rolüne Grup atayamazsınız, yalnızca bireysel kullanıcılara izin verilir.

Cihaz yöneticileri tüm Azure AD 'ye katılmış cihazlara atanır. Bunlar, belirli bir cihaz kümesinin kapsamına alınamaz.

Kullanıcıları Cihaz Yöneticisi rolünden kaldırdığınızda, sunucuda oturum açtıkları sürece bir cihazda yerel yönetici ayrıcalığına sahip olmaya devam eder. Yeni bir birincil yenileme belirteci verildiğinde, bir sonraki oturum açma işlemi sırasında ayrıcalık iptal edilir. Ayrıcalık yükselmesine benzer olan bu iptal, 4 saate kadar sürebilir.

## <a name="next-steps"></a>Sonraki adımlar

- Azure portal'da cihaz yönetimine ilişkin genel bir bakış edinmek için bkz. [Azure portal'ı kullanarak cihazları yönetme](device-management-azure-portal.md)
- Cihaz tabanlı koşullu erişim hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory cihaz tabanlı koşullu erişim ilkelerini yapılandırma](../conditional-access/require-managed-devices.md).
