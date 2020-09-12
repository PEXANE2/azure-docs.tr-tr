---
title: Güvenliğe genel bakış
description: Azure Arc etkin sunucularla ilgili güvenlik bilgileri (Önizleme).
ms.topic: conceptual
ms.date: 08/31/2020
ms.openlocfilehash: 17641fab9933d9d6a60c2b21912f755acc01a6dd
ms.sourcegitcommit: bf1340bb706cf31bb002128e272b8322f37d53dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89447986"
---
# <a name="azure-arc-for-servers-preview-security-overview"></a>Sunucular için Azure Arc (Önizleme) güvenliğine genel bakış

Bu makalede, kuruluşunuzda Azure Arc etkin sunucuları dağıtılmadan önce değerlendirmeniz gereken güvenlik yapılandırması ve önemli noktalar açıklanmaktadır.

## <a name="identity-and-access-control"></a>Kimlik ve erişim denetimi

Her Azure Arc etkin sunucusu, şirket içinde veya başka bir bulut ortamı çalıştıran sunucuyu temsil eden bir Azure aboneliğinde kaynak grubunun parçası olarak yönetilen bir kimliğe sahiptir. Bu kaynağa erişim, standart [Azure rol tabanlı erişim denetimi](../../role-based-access-control/overview.md)tarafından denetlenir. Azure portal [**Access Control (IAM)**](../../role-based-access-control/role-assignments-portal.md#access-control-iam) sayfasından, Azure Arc etkin sunucunuza kimlerin erişebileceğini doğrulayabilirsiniz.

:::image type="content" source="./media/security-overview/access-control-page.png" alt-text="Azure Arc etkin sunucu erişim denetimi" border="false" lightbox="./media/security-overview/access-control-page.png":::

Kaynak üzerinde [katılımcı](../../role-based-access-control/built-in-roles.md#contributor) veya yönetici rolü erişimi verilen kullanıcılar ve uygulamalar, makinede [uzantıları](manage-vm-extensions.md) dağıtma veya silme dahil olmak üzere kaynakta değişiklik yapabilir. Uzantılar, ayrıcalıklı bir bağlamda çalışan rastgele betikler içerebilir, bu nedenle Azure Kaynak üzerinde herhangi bir katkıda bulunan ve Azure dışı sunucu 'nun dolaylı Yöneticisi olacak şekilde düşünün.

**Azure bağlı makine ekleme** rolü, ölçekli ekleme için kullanılabilir ve yalnızca Azure 'Da yeni yay özellikli sunucular okuyabilir veya oluşturabilir. Zaten kayıtlı olan sunucuları silmek veya uzantıları yönetmek için kullanılamaz. En iyi uygulama olarak, bu rolü yalnızca, ölçek olarak makine eklemek için kullanılan Azure Active Directory (Azure AD) hizmet sorumlusuna atamayı öneririz.

**Azure bağlı makine kaynak yöneticisi** rolünün üyesi olan kullanıcılar bir makineyi okuyabilir, değiştirebilir, yeniden ekleyebilir ve silebilir. Bu rol, yay etkin sunucuların yönetimini desteklemek için tasarlanmıştır, ancak kaynak grubunda veya abonelikte diğer kaynakları desteklemez.

## <a name="agent-security-and-permissions"></a>Aracı güvenliği ve izinleri

Azure bağlı makine aracısını (azcmagent) yönetmek için, Windows 'da Kullanıcı hesabınızın yerel Yöneticiler grubunun bir üyesi olması ve Linux üzerinde kök erişim izinlerinizin olması gerekir.

Azure bağlı makine Aracısı, makinenizde çalışan üç hizmetten oluşur.

* Karma Instance Metadata Service (hımds) hizmeti, Arc 'ın tüm temel işlevlerinden sorumludur. Bu, Azure 'a sinyal gönderilmesini ve diğer uygulamalar için bir yerel örnek meta veri hizmeti sunarak, makinenin Azure Kaynak KIMLIĞI hakkında bilgi edinmek ve diğer Azure hizmetlerinde kimlik doğrulaması yapmak için Azure AD belirteçlerini almayı içerir. Bu hizmet, Windows üzerinde ayrıcalıksız bir sanal hizmet hesabı olarak ve Linux üzerinde **hımds** kullanıcısı olarak çalışır.

* Konuk yapılandırma hizmeti (GCService) makinede Azure Ilkesinin değerlendirilmesinden sorumludur.

* Konuk yapılandırma uzantısı hizmeti (ExtensionService), makinede uzantıları (aracılar, betikler veya diğer yazılımlar) yüklemeden, güncelleştirmeden ve silmekten sorumludur.

Konuk yapılandırma ve uzantı Hizmetleri Windows üzerinde yerel sistem olarak ve Linux üzerinde kök olarak çalışır.

## <a name="using-a-managed-identity-with-arc-enabled-servers"></a>Bir yönetilen kimliği, Arc etkin sunucularla kullanma

Varsayılan olarak, Arc tarafından kullanılan Azure Active Directory sistem tarafından atanan kimlik yalnızca Azure 'da Arc etkin sunucusunun durumunu güncelleştirmek için kullanılabilir. Örneğin, *en son görülen* sinyal durumu. Sunucunuzdaki bir uygulama diğer Azure hizmetlerine erişmek için sistem tarafından atanan kimliği kullanıyorsa, isteğe bağlı olarak kimliğe ek roller atayabilirsiniz.

Karma Instance Metadata Service makinede çalışan herhangi bir uygulama tarafından erişilebilir olsa da, yalnızca yetkili uygulamalar sistem tarafından atanan kimlik için bir Azure AD belirteci isteyebilir. Belirteç URI 'sine ilk erişim denemesinde, hizmet, yalnızca güvenilen çağıranların okuyabileceği dosya sistemindeki bir konumda rastgele oluşturulmuş bir şifreleme blobu oluşturacaktır. Çağıranın dosyayı okuması (uygun izni olması gerekir) ve Azure AD belirtecini başarıyla almak için yetkilendirme üstbilgisindeki Dosya içeriğiyle isteği yeniden denemesi gerekir.

* Windows 'da, çağıran, blobu okumak için yerel **Yöneticiler** grubunun veya **karma aracı uzantısı uygulamaları** grubunun bir üyesi olmalıdır.

* Linux 'ta, blobu okumak için çağıranın **hımds** grubunun bir üyesi olması gerekir.

## <a name="using-disk-encryption"></a>Disk şifrelemeyi kullanma

Azure bağlı makine Aracısı, Azure hizmeti ile iletişim kurmak için ortak anahtar kimlik doğrulaması kullanır. Azure yaya bir sunucu ekledikten sonra, bir özel anahtar diske kaydedilir ve aracı Azure ile her iletişim kurduğunda kullanılır. Çalınırsa, özel anahtar hizmetle iletişim kurmak için başka bir sunucuda kullanılabilir ve asıl sunucu gibi davranır. Bu, sistem tarafından atanan kimliğe ve kimliğin erişimi olan herhangi bir kaynağa erişim almayı içerir. Özel anahtar dosyası, yalnızca **hımds** hesabı erişiminin okumasına izin verecek şekilde korunur. Çevrimdışı saldırıları engellemek için, sunucunuzun işletim sistemi biriminde tam disk şifrelemesi (örneğin, BitLocker, dm-crypt, vb.) kullanımını önemle öneririz.

## <a name="next-steps"></a>Sonraki adımlar

Birden çok karma makinede yay etkin sunucuları (Önizleme) değerlendirmeden veya etkinleştirmeden önce, gereksinimleri anlamak için [bağlı makine aracısına genel bakış](agent-overview.md) ' ı ve aracı hakkındaki teknik ayrıntıları ve dağıtım yöntemlerini gözden geçirin.
