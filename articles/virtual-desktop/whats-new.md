---
title: Windows sanal masaüstündeki yenilikler nelerdir? - Azure
description: Windows sanal masaüstü için yeni özellikler ve ürün güncelleştirmeleri.
author: Heidilohr
ms.topic: overview
ms.date: 09/02/2020
ms.author: helohr
ms.reviewer: thhickli; darank
manager: lizross
ms.custom: references_regions
ms.openlocfilehash: d698470f450f6fe903ab68334764e0918d659d7f
ms.sourcegitcommit: 5ed504a9ddfbd69d4f2d256ec431e634eb38813e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89319823"
---
# <a name="whats-new-in-windows-virtual-desktop"></a>Windows sanal masaüstündeki yenilikler nelerdir?

Windows sanal masaüstü güncelleştirmeleri düzenli aralıklarla yapılır. Bu makalede hakkında bilgi edinebilirsiniz:

- En son güncelleştirmeler
- Yeni özellikler
- Mevcut özelliklerde iyileştirmeler
- Hata düzeltmeleri

Bu makale aylık olarak güncelleştirilir. Yeni güncelleştirmeleri kullanmaya devam etmek için bu adımları en sık kontrol ettiğinizden emin olun.

## <a name="august-2020"></a>Ağustos 2020

Ağustos 2020 ' de değiştirilen özellikler:

- Aşağıdaki Azure bölgelerinde bağlantı gecikmesini azaltmak için performansı geliştirdik: 

    - Birleşik Krallık
    - Fransa
    - Norveç
    - Güney Kore

   Bu değişikliklerin kullanıcılarınızı nasıl etkileyeceğini gösteren genel bir fikir edinmek için [deneyim Estimator](https://azure.microsoft.com/services/virtual-desktop/assessment/) ' i kullanabilirsiniz.

- Microsoft Store uzak masaüstü Istemcisi (v 10.2.1522 +) artık genel kullanıma sunulmuştur! Microsoft Store uzak masaüstü Istemcisinin bu sürümü Windows sanal masaüstü ile uyumludur. Ayrıca, geliştirilmiş kullanıcı deneyimleri için yenilenmiş UI akışları geliştirdik. Bu güncelleştirme, akıcı tasarımı, hafif ve koyu modlarını ve diğer birçok heyecan verici değişikliği içerir. Ayrıca, istemciyi iOS, macOS ve Android istemcileri ile aynı temel Uzak Masaüstü Protokolü (RDP) altyapısını kullanacak şekilde de buraya verdik. Bu, tüm platformlarda daha hızlı bir hızda yeni özellikler sunmamızı sağlar. [Istemcisini indirip](https://www.microsoft.com/p/microsoft-remote-desktop/9wzdncrfj3ps?rtc=1&activetab=pivot:overviewtab) deneyin!

- Takımlar masaüstü istemcisinde (sürüm 1.3.00.21759), istemcinin yalnızca sohbet, kanallar ve takvimdeki UTC saat dilimini gösterdiğimiz bir sorunu düzelttik. Güncelleştirilmiş istemci artık bunun yerine uzak oturumun saat dilimini gösterir.

- Azure Advisor artık Windows sanal masaüstü 'nün bir parçasıdır. Windows sanal masaüstüne Azure portal aracılığıyla eriştiğinizde, Windows sanal masaüstü ortamınızı iyileştirmek için öneriler görebilirsiniz. [Azure Danışmanı](azure-advisor.md)hakkında daha fazla bilgi edinin.

- Azure CLı artık Windows `az desktopvirtualization` sanal masaüstü dağıtımlarınızı otomatikleştirmenize yardımcı olmak Için Windows sanal masaüstü 'nü () desteklemektedir. Uzantı komutlarının bir listesi için [desktopvirtualization](/cli/azure/ext/desktopvirtualization/?view=azure-cli-latest) 'a göz atın.

- Dağıtım şablonlarımızı Windows sanal masaüstü Azure Resource Manager arabirimleriyle tamamen uyumlu hale getirmek için güncelleştirdik. Şablonları [GitHub](https://github.com/Azure/RDS-Templates/tree/master/ARM-wvd-templates)' da bulabilirsiniz.

- Windows sanal masaüstü US Gov portalı artık genel önizlemeye sunuldu. Daha fazla bilgi edinmek için [duyurumuza](https://azure.microsoft.com/updates/windows-virtual-desktop-is-now-available-in-the-azure-government-cloud-in-preview/)bakın.

## <a name="july-2020"></a>Temmuz 2020  

Temmuz, Azure Kaynak Yönetimi tümleştirmesiyle Windows sanal masaüstü 'Nün genel kullanıma sunulmasına gelmiştir.

Bu yeni sürümle değiştirilen özellikler şunlardır: 

- "Spring 2020 sürümü" artık "Windows sanal masaüstü (klasik)" olarak bilinir, ancak "Spring yayını" artık yalnızca "Windows sanal masaüstü" olur. 2019 Daha fazla bilgi için [Bu blog gönderisine](https://azure.microsoft.com/blog/new-windows-virtual-desktop-capabilities-now-generally-available/)göz atın. 

Yeni özellikler hakkında daha fazla bilgi edinmek için [Bu blog gönderisine](https://techcommunity.microsoft.com/t5/itops-talk-blog/windows-virtual-desktop-spring-update-enters-public-preview/ba-p/1340245)göz atın. 

### <a name="autoscaling-tool-update"></a>Otomatik ölçeklendirme aracı güncelleştirmesi

Otomatik ölçeklendirme aracının Önizlemedeki en son sürümü genel kullanıma sunulmuştur. Bu araç, bir konak havuzu içindeki oturum ana bilgisayarı sanal makinelerini (VM 'Ler) otomatik olarak kapatmak ve yeniden başlatmak için bir Azure Otomasyonu hesabı ve Azure Logic app kullanır ve altyapı maliyetlerini azaltır. [Azure Otomasyonu 'nu kullanarak ölçek oturum Konakları](set-up-scaling-script.md)hakkında daha fazla bilgi edinin.

### <a name="azure-portal"></a>Azure portal

Artık Windows sanal masaüstündeki Azure portal aşağıdaki işlemleri yapabilirsiniz: 

- Kullanıcıları doğrudan kişisel masaüstü oturumu konaklarına atama  
- Konak havuzları için doğrulama ortamı ayarını değiştirme 

### <a name="diagnostics"></a>Tanılama

Log Analytics çalışma alanı için yeni önceden oluşturulmuş sorgular yayımladık. Sorgulara erişmek için **günlüklere** gidin ve **Kategori**altında **Windows sanal masaüstü**' nü seçin. [Tanılama özelliği için Log Analytics kullanma](diagnostics-log-analytics.md)hakkında daha fazla bilgi edinin.

### <a name="update-for-remote-desktop-client-for-android"></a>Android için Uzak Masaüstü istemcisi güncelleştirmesi

[Android Için uzak masaüstü istemcisi](https://play.google.com/store/apps/details?id=com.microsoft.rdc.androidx) artık Windows sanal masaüstü bağlantılarını desteklemektedir. 10.0.7 sürümünden itibaren, Android istemcisi geliştirilmiş Kullanıcı deneyimi için yeni bir kullanıcı arabirimi sunar. İstemci ayrıca Windows sanal masaüstü çalışma alanlarına abone olurken koşullu erişimi etkinleştirmek için Android cihazlarda Microsoft Authenticator ile tümleşir.  

Uzak Masaüstü istemcisinin önceki sürümü artık "Uzak Masaüstü 8" olarak adlandırılmaktadır. İstemcinin önceki sürümünde bulunan mevcut tüm bağlantılar yeni istemciye sorunsuz bir şekilde aktarılır. Yeni istemci iOS ve macOS istemcileri ile aynı temel RDP Core altyapısına yeniden yazıldı ve tüm platformlarda yeni özelliklerin daha hızlı sürümüdür. 

### <a name="teams-update"></a>Takımlar güncelleştirmesi

Windows sanal masaüstü için Microsoft ekiplerinde iyileştirmeler yaptık. En önemlisi Windows sanal masaüstü artık Windows Masaüstü istemcisi için ses ve video iyileştirmeyi desteklemektedir. Yeniden yönlendirme, çağrılar ve toplantılar halinde ses veya video kullandıklarında kullanıcılar arasında doğrudan yollar oluşturarak gecikme süresini geliştirir. Daha az mesafe daha az sıçrama anlamına gelir, bu da aramaların daha yumuşak görünmesini sağlar. [Windows Sanal Masaüstü ' nde kullanım ekipleri](teams-on-wvd.md)hakkında daha fazla bilgi edinin.

## <a name="june-2020"></a>Haziran 2020

Son ay, önizleme aşamasında Windows sanal masaüstü Azure Resource Manager tümleştirmede tanıtıldık. Bu güncelleştirme, hakkında bilgi almak için çok sevdiğimiz birçok yeni özellik içerir. Windows sanal masaüstü 'nün bu sürümü için yenilikler aşağıda verilmiştir.

### <a name="windows-virtual-desktop-is-now-integrated-with-azure-resource-manager"></a>Windows sanal masaüstü artık Azure Resource Manager ile tümleşiktir

Windows sanal masaüstü artık Azure Resource Manager ile tümleşiktir. En son güncelleştirmede, tüm Windows sanal masaüstü nesneleri artık kaynaklar Azure Resource Manager. Bu güncelleştirme ayrıca Azure rol tabanlı erişim denetimi (Azure RBAC) ile tümleşiktir. Daha fazla bilgi için bkz. [Azure Resource Manager nedir?](../azure-resource-manager/management/overview.md) .

Bu değişiklik sizin için aşağıdaki gibidir:

- Windows sanal masaüstü artık Azure portal tümleşiktir. Bu, her şeyi portalda doğrudan yönetebileceğiniz, PowerShell, Web uygulamaları veya üçüncü taraf araçları gerekmeden yönetebileceğinizi gösterir. Başlamak için [Azure Portal bir konak havuzu oluşturma](create-host-pools-azure-marketplace.md)öğreticimize göz atın.

- Bu güncelleştirmeden önce, yalnızca bireysel kullanıcılara yönelik RemoteApps ve masaüstleri yayımlayabilirsiniz. Azure Resource Manager, artık kaynakları Azure Active Directory gruplara yayımlayabilirsiniz.

- Windows sanal masaüstü 'nün önceki sürümünde, bir kiracıya veya konak havuzuna atayabileceğiniz dört yerleşik yönetici rolü vardı. Bu roller artık [Azure rol tabanlı erişim denetimi 'nde (Azure RBAC)](../role-based-access-control/overview.md). Bu rolleri, tam ve zengin bir temsilciliizin veren her Windows sanal masaüstü Azure Resource Manager nesnesine uygulayabilirsiniz.

- Bu güncelleştirmede, bir konak havuzunu genişletmek için artık Azure Marketi 'ni veya GitHub şablonunu sürekli olarak çalıştırmanız gerekmez. Bir konak havuzunu genişletmelisiniz, ek oturum Konakları dağıtmak için Azure portal ana bilgisayar havuzunuza gitmeniz ve **+ Ekle** ' yi seçmeniz gerekir.

- Konak havuzu dağıtımı artık [Azure Paylaşılan görüntü galerisiyle](../virtual-machines/windows/shared-image-galleries.md)tamamen tümleşiktir. Paylaşılan görüntü Galerisi, görüntü sürümü oluşturma da dahil olmak üzere sanal makine (VM) görüntü tanımlarını depolayan ayrı bir Azure hizmetidir. Ayrıca, yerel dağıtım için görüntülerinizi kopyalamak ve diğer Azure bölgelerine göndermek için genel çoğaltma kullanabilirsiniz.

- PowerShell veya tanılama hizmeti Web uygulaması tarafından yapılması için kullanılan izleme işlevleri artık Azure portal Log Analytics öğesine taşınmıştır. Artık raporlarınızı görselleştirmek için iki seçeneğiniz de vardır. Görsel raporlar oluşturmak için kusto sorguları çalıştırabilir ve çalışma kitaplarını kullanabilirsiniz.

- Artık Windows sanal masaüstü 'Nü kullanmak için Azure Active Directory (Azure AD) onayını tamamlamayı gerekli değilsiniz. Bu güncelleştirmede, Azure aboneliğinizdeki Azure AD kiracısı kullanıcılarınızın kimliğini doğrular ve yöneticiler için Azure RBAC denetimleri sağlar.


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

## <a name="client-updates"></a>İstemci güncelleştirmeleri

Windows sanal masaüstü ve Uzak Masaüstü Hizmetleri istemcilerimize yönelik güncelleştirmeler hakkında bilgi edinmek için şu makalelere göz atın:

- [Windows](/windows-server/remote/remote-desktop-services/clients/windowsdesktop-whatsnew)
- [macOS](/windows-server/remote/remote-desktop-services/clients/mac-whatsnew)
- [iOS](/windows-server/remote/remote-desktop-services/clients/ios-whatsnew)
- [Android](/windows-server/remote/remote-desktop-services/clients/android-whatsnew)
- [Web](/windows-server/remote/remote-desktop-services/clients/web-client-whatsnew)

## <a name="next-steps"></a>Sonraki adımlar

[Windows sanal masaüstü yol haritası Microsoft 365](https://www.microsoft.com/microsoft-365/roadmap?filters=Windows%20Virtual%20Desktop)gelecek planlar hakkında bilgi edinin.

