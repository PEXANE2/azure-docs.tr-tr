---
title: Windows sanal masaüstündeki yenilikler nelerdir? - Azure
description: Windows sanal masaüstü için yeni özellikler ve ürün güncelleştirmeleri.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: overview
ms.date: 06/15/2020
ms.author: helohr
ms.reviewer: thhickli; darank
manager: lizross
ms.openlocfilehash: 7fc1655b94689808eb9855340ef20bb816ad515e
ms.sourcegitcommit: 4f1c7df04a03856a756856a75e033d90757bb635
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87922549"
---
# <a name="whats-new-in-windows-virtual-desktop"></a>Windows sanal masaüstündeki yenilikler nelerdir?

Windows sanal masaüstü güncelleştirmeleri düzenli aralıklarla yapılır. Bu makalede hakkında bilgi edinebilirsiniz:

- En son güncelleştirmeler
- Yeni özellikler
- Mevcut özelliklerde iyileştirmeler
- Hata düzeltmeleri

Bu makale aylık olarak güncelleştirilir. Yeni güncelleştirmeleri kullanmaya devam etmek için bu adımları en sık kontrol ettiğinizden emin olun.

## <a name="june-2020"></a>Haziran 2020

Son ay, önizleme aşamasında Windows sanal masaüstü Azure Resource Manager tümleştirmede tanıtıldık. Bu güncelleştirme, hakkında bilgi almak için çok sevdiğimiz birçok yeni özellik içerir. Windows sanal masaüstü 'nün bu sürümü için yenilikler aşağıda verilmiştir.

### <a name="windows-virtual-desktop-is-now-integrated-with-azure-resource-manager-preview"></a>Windows sanal masaüstü artık Azure Resource Manager (Önizleme) ile tümleşiktir

Windows sanal masaüstü artık Azure Resource Manager ile tümleşiktir. En son güncelleştirmede, tüm Windows sanal masaüstü nesneleri artık kaynaklar Azure Resource Manager. Bu güncelleştirme ayrıca Azure rol tabanlı erişim denetimleriyle (RBAC) tümleşiktir. Daha fazla bilgi için bkz. [Azure Resource Manager nedir?](../azure-resource-manager/management/overview.md) .

Bu değişiklik sizin için aşağıdaki gibidir:

- Windows sanal masaüstü artık Azure portal tümleşiktir. Bu, her şeyi portalda doğrudan yönetebileceğiniz, PowerShell, Web uygulamaları veya üçüncü taraf araçları gerekmeden yönetebileceğinizi gösterir. Başlamak için [Azure Portal bir konak havuzu oluşturma](create-host-pools-azure-marketplace.md)öğreticimize göz atın.

- Bu güncelleştirmeden önce, yalnızca bireysel kullanıcılara yönelik RemoteApps ve masaüstleri yayımlayabilirsiniz. Azure Resource Manager, artık kaynakları Azure Active Directory gruplara yayımlayabilirsiniz.

- Windows sanal masaüstü 'nün önceki sürümünde, bir kiracıya veya konak havuzuna atayabileceğiniz dört yerleşik yönetici rolü vardı. Bu roller artık [Azure rol tabanlı erişim denetimi 'nde (Azure RBAC)](../role-based-access-control/overview.md). Bu rolleri, tam ve zengin bir temsilciliizin veren her Windows sanal masaüstü Azure Resource Manager nesnesine uygulayabilirsiniz.

- Bu güncelleştirmede, bir konak havuzunu genişletmek için artık Azure Marketi 'ni veya GitHub şablonunu sürekli olarak çalıştırmanız gerekmez. Bir konak havuzunu genişletmelisiniz, ek oturum Konakları dağıtmak için Azure portal ana bilgisayar havuzunuza gitmeniz ve **+ Ekle** ' yi seçmeniz gerekir.

- Konak havuzu dağıtımı artık [Azure Paylaşılan görüntü galerisiyle](../virtual-machines/windows/shared-image-galleries.md)tamamen tümleşiktir. Paylaşılan görüntü Galerisi, görüntü sürümü oluşturma da dahil olmak üzere sanal makine (VM) görüntü tanımlarını depolayan ayrı bir Azure hizmetidir. Ayrıca, yerel dağıtım için görüntülerinizi kopyalamak ve diğer Azure bölgelerine göndermek için genel çoğaltma kullanabilirsiniz.

- PowerShell veya tanılama hizmeti Web uygulaması tarafından yapılması için kullanılan izleme işlevleri artık Azure portal Log Analytics öğesine taşınmıştır. Artık raporlarınızı görselleştirmek için iki seçeneğiniz de vardır. Görsel raporlar oluşturmak için kusto sorguları çalıştırabilir ve çalışma kitaplarını kullanabilirsiniz.

- Artık Windows sanal masaüstü 'Nü kullanmak için Azure Active Directory (Azure AD) onayını tamamlamayı gerekli değilsiniz. Bu güncelleştirmede, Azure aboneliğinizdeki Azure AD kiracısı kullanıcılarınızın kimliğini doğrular ve yöneticiler için RBAC denetimleri sağlar.


### <a name="powershell-support"></a>PowerShell desteği

Bu güncelleştirmeyle Azure PowerShell az Module 'e yeni AzWvd cmdlet 'leri ekledik. Bu yeni modül, .NET Core üzerinde çalışan PowerShell Core 'da desteklenir.

Modülünü yüklemek için [Windows sanal masaüstü Için PowerShell modülünü ayarlama](powershell-module.md)bölümündeki yönergeleri izleyin.

[Azwvd PowerShell başvurusunda](/powershell/module/az.desktopvirtualization/?view=azps-4.2.0#desktopvirtualization)kullanılabilir komutların listesini de görebilirsiniz.

Yeni özellikler hakkında daha fazla bilgi için [blog gönderimize](https://techcommunity.microsoft.com/t5/itops-talk-blog/windows-virtual-desktop-spring-update-enters-public-preview/ba-p/1340245)göz atın. 

### <a name="additional-gateways"></a>Ek ağ geçitleri

Bağlantı gecikmesini azaltmak için Güney Afrika 'ya yeni bir ağ geçidi kümesi ekledik.

### <a name="microsoft-teams-on-windows-virtual-desktop-preview"></a>Windows sanal masaüstündeki Microsoft ekipleri (Önizleme)

Windows sanal masaüstü için Microsoft ekiplerinde bazı geliştirmeler yaptık. En önemlisi Windows sanal masaüstü artık çağrılar için ses ve görsel yeniden yönlendirmeyi desteklemektedir. Yeniden yönlendirme, kullanıcılar ses veya video kullanarak çağrı yaparken doğrudan yollar oluşturarak gecikme süresini geliştirir. Daha az mesafe daha az sıçrama anlamına gelir, bu da aramaların daha yumuşak görünmesini sağlar.

Daha fazla bilgi edinmek için [blog gönderimize](https://azure.microsoft.com/updates/windows-virtual-desktop-media-optimization-for-microsoft-teams-is-now-available-in-public-preview/)bakın.

## <a name="next-steps"></a>Sonraki adımlar

[Windows sanal masaüstü yol haritası Microsoft 365](https://www.microsoft.com/microsoft-365/roadmap?filters=Windows%20Virtual%20Desktop)gelecek planlar hakkında bilgi edinin.

Windows sanal masaüstü ve Uzak Masaüstü Hizmetleri istemcilerimize yönelik güncelleştirmeler hakkında bilgi edinmek için şu makalelere göz atın:

- [Windows](/windows-server/remote/remote-desktop-services/clients/windowsdesktop-whatsnew)
- [macOS](/windows-server/remote/remote-desktop-services/clients/mac-whatsnew)
- [iOS](/windows-server/remote/remote-desktop-services/clients/ios-whatsnew)
- [Android](/windows-server/remote/remote-desktop-services/clients/android-whatsnew)
- [Web](/windows-server/remote/remote-desktop-services/clients/web-client-whatsnew)
