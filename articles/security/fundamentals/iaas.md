---
title: Azure'daki IaaS iş yükleri için en iyi güvenlik uygulamaları | Microsoft Dokümanlar
description: " İş yüklerinin Azure IaaS'a geçişi, tasarımlarımızı yeniden değerlendirme fırsatları getiriyor "
services: security
documentationcenter: na
author: terrylanfear
manager: rkarlin
editor: TomSh
ms.assetid: 02c5b7d2-a77f-4e7f-9a1e-40247c57e7e2
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2019
ms.author: terrylan
ms.openlocfilehash: fadf07f312c86f8ca15f5a97ebbe99e84bcffc89
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80548240"
---
# <a name="security-best-practices-for-iaas-workloads-in-azure"></a>Azure'da IaaS iş yükleri için en iyi güvenlik yöntemleri
Bu makalede, VM'ler ve işletim sistemleri için güvenlik en iyi uygulamaları açıklanmaktadır.

En iyi uygulamalar fikir birliğine dayanır ve geçerli Azure platformu özellikleri ve özellik kümeleriyle çalışır. Görüşler ve teknolojiler zaman içinde değişebileceğinden, bu makale bu değişiklikleri yansıtacak şekilde güncelleştirilir.

Hizmet olarak altyapı (IaaS) senaryolarının çoğunda, [Azure sanal makineleri (VM' ler)](/azure/virtual-machines/) bulut bilgi işlem kullanan kuruluşlar için ana iş yükütür. Bu gerçek, kuruluşların iş yüklerini yavaşça buluta geçirmek istediği [karma senaryolarda](https://social.technet.microsoft.com/wiki/contents/articles/18120.hybrid-cloud-infrastructure-design-considerations.aspx) açıkça görülmektedir. Bu tür senaryolarda, [IaaS için genel güvenlik hususlarını](https://social.technet.microsoft.com/wiki/contents/articles/3808.security-considerations-for-infrastructure-as-a-service-iaas.aspx)izleyin ve tüm VM'lerinize güvenlik en iyi uygulamalarını uygulayın.

## <a name="protect-vms-by-using-authentication-and-access-control"></a>Kimlik doğrulama ve erişim denetimini kullanarak VM'leri koruyun
VM'lerinizi korumanın ilk adımı, yalnızca yetkili kullanıcıların yeni VM'ler kurup VM'lere erişebilmesini sağlamaktır.

> [!NOTE]
> Azure'daki Linux VM'lerinin güvenliğini artırmak için Azure AD kimlik doğrulaması ile tümleştirebilirsiniz. [Linux VM'leri için Azure AD kimlik doğrulamasını](/azure/virtual-machines/linux/login-using-aad)kullandığınızda, VM'lere erişime izin veren veya reddeden ilkeleri merkezi olarak denetler ve uygularsınız.
>
>

**En iyi uygulama**: VM erişimini kontrol edin.   
**Ayrıntı**: Kuruluşunuzdaki kaynaklar için sözleşmeler oluşturmak ve özelleştirilmiş ilkeler oluşturmak için [Azure ilkelerini](/azure/azure-policy/azure-policy-introduction) kullanın. Bu ilkeleri kaynak grupları gibi kaynaklara [uygulayın.](/azure/azure-resource-manager/resource-group-overview) Kaynak grubuna ait VM'ler ilkelerini devralır.

Kuruluşunuzun çok sayıda aboneliği varsa, söz konusu aboneliklerde erişimi, ilkeleri ve uyumluluğu yönetmek için verimli bir yönteme ihtiyacınız olabilir. [Azure yönetim grupları,](/azure/azure-resource-manager/management-groups-overview) aboneliklerin üzerinde bir kapsam düzeyi sağlar. Abonelikleri yönetim gruplarına (kapsayıcılar) düzenler ve yönetim koşullarınızı bu gruplara uygularsınız. Yönetim grubundaki tüm abonelikler, gruba uygulanan koşulları otomatik olarak devralır. Yönetim grupları, sahip olabileceğiniz abonelik türüne bakılmaksızın kurumsal düzeyde yönetimi büyük ölçekte sunar.

**En iyi yöntem**: VM'leri kurulumunuz ve dağıtımınızda değişkenliği azaltın.   
**Ayrıntı**: Dağıtım seçeneklerinizi güçlendirmek ve ortamınızdaki VM'lerin anlaşılmasını ve envanterini çıkarmak için [Azure Kaynak Yöneticisi](/azure/azure-resource-manager/resource-group-authoring-templates) şablonlarını kullanın.

**En iyi uygulama**: Güvenli ayrıcalıklı erişim.   
**Ayrıntı**: Kullanıcıların [VM'lere](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/plan/security-best-practices/implementing-least-privilege-administrative-models) erişmesini ve ayarlamasını sağlamak için en az ayrıcalık yaklaşımı ve yerleşik Azure rolleri kullanın:

- [Sanal Makine Katılımcısı](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor): VM'leri yönetebilir, ancak bağlı oldukları sanal ağ veya depolama hesabını yönetemez.
- [Klasik Sanal Makine Oluşturuması](../../role-based-access-control/built-in-roles.md#classic-virtual-machine-contributor): Klasik dağıtım modelini kullanarak oluşturulan VM'leri yönetebilir, ancak SANAL M'lerin bağlı olduğu sanal ağ veya depolama hesabını yönetemez.
- [Güvenlik Yöneticisi](../../role-based-access-control/built-in-roles.md#security-admin): Yalnızca Güvenlik Merkezi'nde: Güvenlik ilkelerini görüntüleyebilir, güvenlik durumlarını görüntüleyebilir, güvenlik ilkelerini ve önerileri görüntüleyebilir, uyarıları ve önerileri görüntüleyebilir, uyarıları ve önerileri reddedebilir.
- [DevTest Labs Kullanıcı](../../role-based-access-control/built-in-roles.md#devtest-labs-user): Her şeyi görüntüleyebilir ve VM'leri bağlayabilir, başlatabilir, yeniden başlatabilir ve kapatabilir.

Abonelik yöneticileriniz ve yardımcı yöneticileriniz bu ayarı değiştirerek onları abonelikteki tüm VM'lerin yöneticisi yapabilir. Tüm abonelik yöneticilerinizin ve yardımcı yöneticilerinizin makinelerinizden herhangi birinde oturum açmasına güvendiğinizden emin olun.

> [!NOTE]
> Aynı yaşam döngüsüne sahip VM'leri aynı kaynak grubunda birleştirmenizi öneririz. Kaynak gruplarını kullanarak, kaynaklarınız için faturalandırma maliyetlerini dağıtabilir, izleyebilir ve toyedekleyebilirsiniz.
>
>

VM erişimini ve kurulumlarını kontrol eden kuruluşlar genel VM güvenliklerini geliştirir.

## <a name="use-multiple-vms-for-better-availability"></a>Daha iyi kullanılabilirlik için birden fazla VM kullanma
VM'niz yüksek kullanılabilirliğe sahip olması gereken kritik uygulamaları çalıştırıyorsa, birden çok VM kullanmanızı şiddetle öneririz. Daha iyi kullanılabilirlik için, [kullanılabilirlik kümesi](../../virtual-machines/windows/manage-availability.md#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy) ni veya kullanılabilirlik [bölgelerini](../../availability-zones/az-overview.md)kullanın.

Kullanılabilirlik kümesi, azure veri merkezinde dağıtılırken içine yerleştirdiğiniz VM kaynaklarının birbirlerinden yalıtılmış olmasını sağlamak için Azure'da kullanabileceğiniz mantıksal bir gruplandırmadır. Azure, birden çok fiziksel sunucu, bilgisayar rafları, depolama birimleri ve ağ anahtarları arasında çalışan bir kullanılabilirlik kümesine yerleştirdiğiniz VM'lerin çalışmasını sağlar. Bir donanım veya Azure yazılım hatası oluşursa, yalnızca VM'lerinizin bir alt kümesi etkilenir ve genel uygulamanız müşterileriniz tarafından kullanılabilir durumda olmaya devam eder. Kullanılabilirlik kümeleri, güvenilir bulut çözümleri oluşturmak istediğinizde önemli bir özelliktir.

## <a name="protect-against-malware"></a>Kötü amaçlı yazılıma karşı koruma
Virüsleri, casus yazılımları ve diğer kötü amaçlı yazılımları tanımlamaya ve kaldırmaya yardımcı olmak için kötü amaçlı yazılımdan koruma yüklemeniz gerekir. [Microsoft Antimalware](antimalware.md) veya Microsoft iş ortağının uç nokta koruma çözümlerini[(Trend Micro,](https://help.deepsecurity.trendmicro.com/Welcome.html) [Broadcom,](https://www.broadcom.com/products) [McAfee,](https://www.mcafee.com/us/products.aspx) [Windows Defender](https://www.microsoft.com/windows/comprehensive-security)ve System [Center Endpoint Protection)](/configmgr/protect/deploy-use/endpoint-protection)yükleyebilirsiniz.

Microsoft Antimalware gerçek zamanlı koruma, zamanlanmış tarama, kötü amaçlı yazılım düzeltme, imza güncelleştirmeleri, motor güncelleştirmeleri, örnek raporlama ve dışlama olay toplama gibi özellikler içerir. Üretim ortamınızdan ayrı olarak barındırılan ortamlar için, VM'lerinizin ve bulut hizmetlerinizin korunmasına yardımcı olmak için kötü amaçlı yazılımdan koruma uzantısı kullanabilirsiniz.

Dağıtım kolaylığı ve yerleşik algılamalar (uyarılar ve olaylar) için Microsoft Kötü Amaçlı Yazılımdan Koruma ve iş ortağı çözümlerini [Azure Güvenlik Merkezi](../../security-center/index.yml) ile tümleştirebilirsiniz.

**En iyi uygulama**: Kötü amaçlı yazılımlara karşı korumak için bir kötü amaçlı yazılımdan koruma çözümü yükleyin.   
**Ayrıntı**: [Microsoft iş ortağı çözümü veya Microsoft Antimalware yükleme](../../security-center/security-center-install-endpoint-protection.md)

**En iyi uygulama**: Korumanızın durumunu izlemek için kötü amaçlı yazılımdan koruma çözümünüzü Güvenlik Merkezi ile entegre edin.   
**Ayrıntı**: [Güvenlik Merkezi ile uç nokta koruma sorunlarını yönetme](../../security-center/security-center-partner-integration.md)

## <a name="manage-your-vm-updates"></a>VM güncellemelerinizi yönetme
Azure VM'ler, şirket içi tüm VM'ler gibi kullanıcı tarafından yönetilecek şekilde dir. Azure bunlara Windows güncelleştirmelerini göndermez. VM güncellemelerinizi yönetmeniz gerekir.

**En iyi uygulama**: VM'lerinizi güncel tutun.   
**Ayrıntı**: Azure'da, şirket içi ortamlarda veya diğer bulut sağlayıcılarında dağıtılan Windows ve Linux bilgisayarlarınızın işletim sistemi güncelleştirmelerini yönetmek için Azure Otomasyonu'nda [Güncelleştirme Yönetimi](../../automation/automation-update-management.md) çözümlerini kullanın. Tüm aracı bilgisayarlardaki kullanılabilir güncelleştirmelerin durumunu hızla değerlendirebilir ve sunucular için gerekli güncelleştirmeleri yükleme işlemini yönetebilirsiniz.

Güncelleştirme Yönetimi tarafından yönetilen bilgisayarlar değerlendirme ve güncelleştirme dağıtımı yapmak için aşağıdaki yapılandırmaları kullanır:

- Windows veya Linux için Microsoft Monitoring Agent (MMA)
- Linux için PowerShell İstenen Durum Yapılandırması (DSC)
- Otomasyon Karma Runbook Çalışanı
- Windows bilgisayarları için Microsoft Update veya Windows Server Update Services (WSUS)

Windows Update kullanıyorsanız, otomatik Windows Update ayarını etkin bırakın.

**En iyi yöntem**: Dağıtım sırasında, oluşturduğunuz görüntülerin en son Windows güncelleştirmeleri turunu içerdiğinden emin olun.   
**Ayrıntı**: Her dağıtımın ilk adımı olarak tüm Windows güncelleştirmelerini denetleyin ve yükleyin. Bu ölçü, siz veya kendi kitaplığınızdan gelen görüntüleri dağıtırken uygulamak için özellikle önemlidir. Azure Marketi'ndeki görüntüler varsayılan olarak otomatik olarak güncelleştirilse de, genel kullanıma açık tan sonra bir gecikme süresi (birkaç haftaya kadar) olabilir.

**En iyi uygulama**: Os'un yeni bir sürümünü zorlamak için VM'lerinizi düzenli olarak yeniden dağıtın.   
**Ayrıntı**: VM'nizi azure [kaynak yöneticisi şablonuyla](../../azure-resource-manager/templates/template-syntax.md) tanımlayın, böylece kolayca yeniden dağıtabilirsiniz. Şablon kullanmak, ihtiyacınız olduğunda yamalı ve güvenli bir VM sağlar.

**En iyi yöntem**: VM'lere hızla güvenlik güncelleştirmeleri uygulayın.   
**Ayrıntı**: Eksik güvenlik güncelleştirmelerini belirlemek ve uygulamak için Azure Güvenlik Merkezi'ni (Ücretsiz katman veya Standart katman) [etkinleştirin.](../../security-center/security-center-apply-system-updates.md)

**En iyi yöntem**: En son güvenlik güncelleştirmelerini yükleyin.   
**Ayrıntı**: Müşterilerin Azure'a taşıdıkları ilk iş yüklerinden bazıları laboratuarlar ve dışa dönük sistemlerdir. Azure VM'leriniz internet erişimine açık olması gereken uygulamaları veya hizmetleri barındırıyorsa, düzeltme konusunda dikkatli olun. İşletim sisteminin ötesinde yama. İş ortağı uygulamalarında yamalanmamış güvenlik açıkları, iyi yama yönetimi yerindeyse önlenebilecek sorunlara da yol açabilir.

**En iyi yöntem**: Bir yedekleme çözümlerini dağıtın ve test edin.   
**Ayrıntı**: Yedeklemenin, diğer işlemleri sizin yaptığınız gibi ele alınması gerekir. Bu, buluta uzanan üretim ortamınızın bir parçası olan sistemler için geçerlidir.

Test ve geliştirme sistemleri, şirket içi ortamlarla ilgili deneyimlerine bağlı olarak, kullanıcıların alıştıklarına benzer geri yükleme özellikleri sağlayan yedekleme stratejileri izlemelidir. Azure'a taşınan üretim iş yükleri, mümkün olduğunda varolan yedekleme çözümleriyle tümleştirmelidir. Veya yedekleme gereksinimlerinizi karşılamaya yardımcı olmak için [Azure Yedekleme'yi](../../backup/backup-azure-vms-first-look-arm.md) kullanabilirsiniz.

Yazılım güncelleştirme ilkelerini zorlamayan kuruluşlar, bilinen ve önceden sabit olan güvenlik açıklarından yararlanan tehditlere daha fazla maruz kalır. Şirketlerin sektör yönetmeliklerine uymak için, bulutta bulunan iş yüklerinin güvenliğini sağlamaya yardımcı olmak için çalışkan olduklarını ve doğru güvenlik denetimlerini kullandıklarını kanıtlamaları gerekir.

Geleneksel bir veri merkezi ve Azure IaaS için yazılım güncelleştirme en iyi uygulamaları birçok benzerliğe sahiptir. Geçerli yazılım güncelleştirme ilkelerinizi Azure'da bulunan VM'leri içerecek şekilde değerlendirmenizi öneririz.

## <a name="manage-your-vm-security-posture"></a>VM güvenlik duruşunuzu yönetme
Siber tehditler gelişiyor. VM'lerinizi korumak, tehditleri hızla algılayabilen, kaynaklarınıza yetkisiz erişimi önleyebilen, uyarıları tetikleyen ve yanlış pozitif leri azaltabilen bir izleme yeteneği gerektirir.

[Windows](../../security-center/security-center-virtual-machine.md) ve [Linux VM'lerinizin](../../security-center/security-center-linux-virtual-machine.md)güvenlik duruşunu izlemek için [Azure Güvenlik Merkezi'ni](../../security-center/security-center-intro.md)kullanın. Güvenlik Merkezi'nde, aşağıdaki özelliklerden yararlanarak VM'lerinizi koruyun:

- Önerilen yapılandırma kurallarıyla işletim sistemi güvenlik ayarlarını uygulayın.
- Sistem güvenliğini ve eksik olabilecek kritik güncelleştirmeleri tanımlayın ve indirin.
- Son nokta kötü amaçlı yazılımdan koruma için öneriler dağıtın.
- Disk şifrelemeyi doğrulayın.
- Güvenlik açıklarını değerlendirin ve düzeltin.
- Tehditleri algıla.

Güvenlik Merkezi tehditleri etkin bir şekilde izleyebilir ve olası tehditler güvenlik uyarılarında açığa alınır. İlişkili tehditler, güvenlik olayı adı verilen tek bir görünümde toplanır.

Güvenlik Merkezi verileri [Azure Monitor günlüklerinde](/azure/log-analytics/log-analytics-overview)depolar. Azure Monitor günlükleri, uygulamalarınızın ve kaynaklarınızın işleyişi hakkında bilgi veren bir sorgu dili ve analiz motoru sağlar. Veriler ayrıca Azure [Monitor'dan,](../../batch/monitoring-overview.md)yönetim çözümlerinden ve buluttaki veya şirket içinde sanal makinelere yüklenen aracılardan da toplanır. Bu paylaşılan işlevsellik, ortamınızın eksiksiz bir resmini oluşturmanıza yardımcı olur.

VM'leri için güçlü güvenlik zorlamayan kuruluşlar, yetkisiz kullanıcıların güvenlik denetimlerini atlatma girişimlerinden habersiz kalır.

## <a name="monitor-vm-performance"></a>VM performansını izleyin
VM süreçleri olması gerekenden daha fazla kaynak tüketdiğinde kaynak kötüye kullanımı sorun olabilir. VM ile ilgili performans sorunları, kullanılabilirlik güvenlik ilkesini ihlal eden hizmet kesintisine neden olabilir. Yüksek CPU veya bellek kullanımı bir hizmet reddi (DoS) saldırısı gösterebilir, çünkü bu, Özellikle IIS veya diğer web sunucuları barındıran VM'ler için önemlidir. Bir sorun oluşurken VM erişimini yalnızca reaktif olarak izlemekle kalmamış, aynı zamanda normal çalışma sırasında ölçülen temel performansa karşı proaktif olarak izlemek zorunludur.

Kaynağınızın sağlığında görünürlük elde etmek için [Azure Monitor'u](/azure/monitoring-and-diagnostics/monitoring-overview-metrics) kullanmanızı öneririz. Azure Monitör özellikleri:

- [Kaynak tanılama günlüğü dosyaları](../../azure-monitor/platform/platform-logs-overview.md): VM kaynaklarınızı izler ve performansı ve kullanılabilirliği tehlikeye atabilecek olası sorunları tanımlar.
- [Azure Tanılama uzantısı](/azure/azure-monitor/platform/diagnostics-extension-overview): Windows VM'lerde izleme ve tanılama özellikleri sağlar. Uzantıyı [Azure Kaynak Yöneticisi şablonunun](/azure/virtual-machines/windows/extensions-diagnostics-template)bir parçası olarak ekleyerek bu özellikleri etkinleştirebilirsiniz.

VM performansını izlemeyen kuruluşlar, performans kalıplarında belirli değişikliklerin normal mi yoksa anormal mi olduğunu belirleyemez. Normalden daha fazla kaynak tüketen bir VM, harici bir kaynaktan gelen bir saldırıyı veya VM'de çalışan tehlikeye atılmış bir işlemi gösterebilir.

## <a name="encrypt-your-virtual-hard-disk-files"></a>Sanal sabit disk dosyalarınızı şifreleme
Şifreleme anahtarlarınız ve sırlarınızla birlikte önyükleme hacminizi ve veri birimlerinizi depolama alanında korumaya yardımcı olmak için sanal sabit disklerinizi (VHD'leri) şifrelemenizi öneririz.

[Azure Disk Şifreleme,](../azure-security-disk-encryption-overview.md) Windows ve Linux IaaS sanal makine disklerinizi şifrelemenize yardımcı olur. Azure Disk Şifreleme, işletim sistemi ve veri diskleri için birim şifrelemesağlamak için Windows'un endüstri standardı [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) özelliğini ve Linux'un [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) özelliğini kullanır. Çözüm, anahtar kasa aboneliğinizdeki disk şifreleme anahtarlarını ve sırlarını kontrol etmenize ve yönetmenize yardımcı olmak için [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) ile entegre edilmiştir. Çözüm ayrıca, sanal makine diskleri üzerindeki tüm verilerin Azure Depolama'da dinlenime de şifrelenmesini sağlar.

Azure Disk Şifrelemesi'ni kullanmak için en iyi uygulamalar şunlardır:

**En iyi yöntem**: VM'lerde şifrelemeyi etkinleştirin.   
**Ayrıntı**: Azure Disk Şifreleme, anahtar kasanızın şifreleme anahtarlarını oluşturur ve yazar. Anahtar kasanızda şifreleme anahtarlarını yönetmek için Azure AD kimlik doğrulaması gerekir. Bu amaçla bir Azure AD uygulaması oluşturun. Kimlik doğrulama amacıyla, istemci gizli tabanlı kimlik doğrulaması veya [istemci sertifikası tabanlı Azure AD kimlik doğrulaması](../../active-directory/authentication/active-directory-certificate-based-authentication-get-started.md)kullanabilirsiniz.

**En iyi yöntem:** Şifreleme anahtarları için ek bir güvenlik katmanı için anahtar şifreleme anahtarı (KEK) kullanın. Anahtar kasanıza bir KEK ekleyin.   
**Ayrıntı**: Anahtar kasasında anahtar şifreleme anahtarı oluşturmak için [Add-AzKeyVaultKey](/powershell/module/az.keyvault/add-azkeyvaultkey) cmdlet'i kullanın. Ayrıca, anahtar yönetimi için şirket içi donanım güvenlik modülünüzden (HSM) bir KEK de içe aktarabilirsiniz. Daha fazla bilgi için [Key Vault belgelerine](../../key-vault/key-vault-hsm-protected-keys.md)bakın. Bir anahtar şifreleme anahtarı belirtildiğinde, Azure Disk Şifreleme, Key Vault'a yazmadan önce şifreleme sırlarını sarmak için bu anahtarı kullanır. Bu anahtarın emanet kopyasını şirket içi anahtar yönetimi HSM'de tutmak, anahtarların yanlışlıkla silinmesine karşı ek koruma sağlar.

**En iyi yöntem**: Diskler şifrelenmeden önce [anlık görüntü](../../virtual-machines/windows/snapshot-copy-managed-disk.md) ve/veya yedekleme alın. Şifreleme sırasında beklenmeyen bir hata olursa yedeklemeler bir kurtarma seçeneği sağlar.   
**Ayrıntı**: Yönetilen disklere sahip VM'ler şifreleme oluşmadan önce yedekleme gerektirir. Yedekleme yapıldıktan *sonra,-skipVmBackup* parametresini belirterek yönetilen diskleri şifrelemek için **Set-AzVMDiskEncryptionExtension** cmdlet'ini kullanabilirsiniz. Şifreli VM'leri yedekleme ve geri yükleme hakkında daha fazla bilgi için [Azure Yedekleme](../../backup/backup-azure-vms-encryption.md) makalesine bakın.

**En iyi yöntem**: Şifreleme sırlarının bölgesel sınırları aşmadığından emin olmak için Azure Disk Şifreleme'nin aynı bölgede bulunabilmesi için anahtar kasasına ve VM'lere ihtiyacı vardır.   
**Detay**: Şifrelenecek VM ile aynı bölgede bulunan bir anahtar kasası oluşturun ve kullanın.

Azure Disk Şifreleme'yi uyguladığınız zaman, aşağıdaki iş gereksinimlerini karşılayabilirsiniz:

- IaaS VM'ler, kuruluş güvenliği ve uyumluluk gereksinimlerini karşılamak için endüstri standardı şifreleme teknolojisi ile dinlenerek güvence altına alınır.
- IaaS VM'ler müşteri kontrollü anahtarlar ve ilkeler altında başlar ve bunların kullanımını anahtar kasanızda denetleyebilirsiniz.

## <a name="restrict-direct-internet-connectivity"></a>Doğrudan internet bağlantısını kısıtlama
VM doğrudan internet bağlantısını izleyin ve kısıtlayın. Saldırganlar, açık yönetim bağlantı noktaları için genel bulut IP aralıklarını sürekli olarak tarar ve yaygın parolalar ve bilinen yamasız güvenlik açıkları gibi "kolay" saldırılar girişiminde bulunmayı dener. Aşağıdaki tablo, bu saldırılara karşı korunmaya yardımcı olacak en iyi uygulamaları listeler:

**En iyi yöntem**: Ağ yönlendirmeve güvenlik yanlışlıkla maruz kalma önlemek.   
**Ayrıntı**: Yalnızca merkezi ağ grubunun ağ kaynaklarına izin verdiğinden emin olmak için RBAC'ı kullanın.

**En iyi uygulama**: "Herhangi bir" kaynak IP adresinden erişime izin veren açıkvm'leri tanımlayın ve düzeltin.   
**Ayrıntı**: Azure Güvenlik Merkezi'ni kullanın. Güvenlik Merkezi, ağ güvenlik gruplarınızın "herhangi bir" kaynak IP adresinden erişime izin veren bir veya daha fazla gelen kuralı varsa, Internet'e bakan uç noktalar üzerinden erişimi kısıtlamanızı önerir. Güvenlik Merkezi, gerçekten erişimi gereken kaynak IP adreslerine [erişimi kısıtlamak](../../security-center/security-center-network-recommendations.md) için bu gelen kuralları düzenlemenizi önerir.

**En iyi yöntem**: Yönetim bağlantı noktalarını (RDP, SSH) kısıtlayın.   
**Ayrıntı**: Azure VM'lerinize gelen trafiği kilitlemek ve gerektiğinde VM'lere bağlanmak için kolay erişim sağlarken saldırılara maruz kalmayı azaltmak için [tam zamanında (JIT) VM erişimi](../../security-center/security-center-just-in-time.md) kullanılabilir. JIT etkinleştirildiğinde, Güvenlik Merkezi bir ağ güvenlik grubu kuralı oluşturarak Azure VM'lerinize gelen trafiği kilitler. Gelen trafiğin kilitlendiği VM'deki bağlantı noktalarını seçersiniz. Bu bağlantı noktaları JIT çözümü tarafından kontrol edilir.

## <a name="next-steps"></a>Sonraki adımlar
Azure'u kullanarak bulut çözümlerinizi tasarlarken, dağıtırken ve yönetirken kullanabileceğiniz daha fazla güvenlik en iyi uygulamaları için Azure güvenlik en iyi uygulamaları ve [desenleri](best-practices-and-patterns.md) görün.

Azure güvenliği ve ilgili Microsoft hizmetleri hakkında daha genel bilgi sağlamak için aşağıdaki kaynaklar kullanılabilir:
* [Azure Güvenlik Ekibi Blogu](https://blogs.msdn.microsoft.com/azuresecurity/) - Azure Güvenlik'teki en son bilgiler için
* [Microsoft Güvenlik Yanıt Merkezi](https://technet.microsoft.com/library/dn440717.aspx) - Azure ile ilgili sorunlar da dahil olmak üzere Microsoft güvenlik açıklarının rapor edilebildiği veya e-posta yoluylasecure@microsoft.com
