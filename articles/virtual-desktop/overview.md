---
title: Windows Sanal Masaüstü nedir? - Azure
description: Windows sanal masaüstü 'ne genel bakış.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: overview
ms.date: 04/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: fd4268ecad91a023683d1d6f955178fb87d78f87
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2020
ms.locfileid: "82612852"
---
# <a name="what-is-windows-virtual-desktop"></a>Windows Sanal Masaüstü nedir? 

Windows sanal masaüstü, bulutta çalışan bir masaüstü ve uygulama sanallaştırma hizmetidir.

Azure 'da Windows sanal masaüstü 'Nü çalıştırdığınızda şunları yapabilirsiniz:

* Ölçeklenebilirlik ile tam bir Windows 10 sunan çoklu oturum Windows 10 dağıtımı ayarlama
* Office 365 ProPlus 'ı sanallaştırın ve çoklu Kullanıcı sanal senaryolarında çalıştırmak için iyileştirin
* Ücretsiz genişletilmiş güvenlik güncelleştirmeleri ile Windows 7 sanal masaüstlerini sağlama
* Mevcut Uzak Masaüstü Hizmetleri (RDS) ve Windows Server Masaüstlerinizi ve uygulamalarınızı herhangi bir bilgisayara getirme
* Masaüstlerini ve uygulamaları sanallaştırın
* Windows 10, Windows Server ve Windows 7 Masaüstü ve uygulamalarını birleştirilmiş bir yönetim deneyimiyle yönetin

## <a name="introductory-video"></a>Tanıtım Videosu

Windows sanal masaüstü hakkında, neden benzersiz olduğunu ve bu videodaki yenilikler hakkında bilgi edinin:

<br></br><iframe src="https://www.youtube.com/embed/NQFtI3JLtaU" width="640" height="320" allowFullScreen="true" frameBorder="0"></iframe>

Windows sanal masaüstü hakkında daha fazla video için [çalma listemize](https://www.youtube.com/watch?v=NQFtI3JLtaU&list=PLXtHYVsvn_b8KAKw44YUpghpD6lg-EHev)bakın.

## <a name="key-capabilities"></a>Temel işlevler

Windows sanal masaüstü ile ölçeklenebilir ve esnek bir ortam ayarlayabilirsiniz:

* Ek ağ geçidi sunucuları çalıştırmak zorunda kalmadan Azure aboneliğinizde tam bir masaüstü sanallaştırma ortamı oluşturun.
* Farklı iş yüklerinize uyum sağlamak için ihtiyacınız olan sayıda konak havuzu yayımlayın.
* Azure Galerisi 'nden üretim iş yükleri veya test için kendi görüntünüzü getirin.
* Havuza alınmış, çoklu oturum kaynaklarıyla maliyetleri azaltın. Windows Server 'da Windows sanal masaüstü ve Uzak Masaüstü Oturumu Ana Bilgisayarı (RDSH) rolü için özel Windows 10 Enterprise çoklu oturum özelliği sayesinde, kullanıcılarınıza aynı kaynakları sağlarken sanal makine sayısını ve işletim sistemi (OS) ek yükünü büyük ölçüde azaltabilirsiniz.
* Kişisel (kalıcı) masaüstleri aracılığıyla tek bir sahiplik sağlayın.

Sanal masaüstlerini dağıtabilir ve yönetebilirsiniz:

* Konak havuzlarını yapılandırmak, uygulama grupları oluşturmak, kullanıcı atamak ve kaynakları yayımlamak için Windows sanal masaüstü PowerShell ve REST arabirimlerini kullanın.
* Tek bir konak havuzundan tam masaüstü veya ayrı ayrı uzak uygulamalar yayımlayın, farklı Kullanıcı kümeleri için ayrı ayrı uygulama grupları oluşturun, hatta görüntü sayısını azaltmak için kullanıcıları birden çok uygulama grubuna atayın.
* Ortamınızı yönetirken, rol atamak ve çeşitli yapılandırma veya kullanıcı hatalarını anlamak için tanılama toplamak üzere yerleşik olarak atanmış erişimi kullanın.
* Hataları gidermek için yeni tanılama hizmetini kullanın.
* Altyapıyı değil, yalnızca görüntüyü ve sanal makineleri yönetin. Yalnızca Azure aboneliğinizdeki sanal makineler Uzak Masaüstü Hizmetleri gibi uzak masaüstü rollerini kişisel olarak yönetmeniz gerekmez.

Ayrıca, sanal masaüstlerine Kullanıcı atayabilir ve bunları bağlayabilirsiniz:

* Kullanıcılar, atandıktan sonra kullanıcıları yayınlanan Windows Masaüstü ve uygulamalarına bağlamak için herhangi bir Windows sanal masaüstü istemcisini başlatabilir. Cihazınızdaki veya Windows sanal masaüstü HTML5 Web istemcisinde bulunan yerel bir uygulama aracılığıyla herhangi bir cihazdan bağlantı yapın.
* Hizmete ters bağlantılar aracılığıyla kullanıcıları güvenle oluşturun, bu nedenle herhangi bir gelen bağlantı noktasını açık bırakmanız gerekmez.

## <a name="requirements"></a>Gereksinimler

Windows sanal masaüstü 'Nü kurmak ve kullanıcılarınızı Windows Masaüstü ve uygulamalarına başarıyla bağlamak için gereken birkaç nokta vardır.

Aşağıdaki Işletim sistemleri için destek eklemeyi planlıyoruz; bu nedenle, dağıtmayı planladığınız masaüstü ve uygulamalara bağlı olarak kullanıcılarınız için [uygun lisanslara](https://azure.microsoft.com/pricing/details/virtual-desktop/) sahip olduğunuzdan emin olun:

|İşletim Sistemi|Gerekli lisans|
|---|---|
|Windows 10 Enterprise çoklu oturum veya Windows 10 Enterprise|Microsoft 365 E3, E5, a3, A5, F3, Iş<br>Windows E3, E5, a3, a5|
|Windows 7 Enterprise |Microsoft 365 E3, E5, a3, A5, F3, Iş<br>Windows E3, E5, a3, a5|
|Windows Server 2012 R2, 2016, 2019|Yazılım güvencesi içeren RDS Istemci erişim lisansı (CAL)|

Altyapınız Windows sanal masaüstünü desteklemek için aşağıdaki şeylere ihtiyaç duyuyor:

* Bir [Azure Active Directory](/azure/active-directory/)
* Bir Windows Server, Azure Active Directory eşitlenmiş Active Directory. Bunu, aşağıdakilerden biriyle yapılandırabilirsiniz:
  * Azure AD Connect (Hibrit kuruluşlar için)
  * Azure AD Domain Services (karma veya bulut kurumları için)
* Windows Server 'a bağlı olan veya içeren bir sanal ağ içeren bir Azure aboneliği Active Directory
  
Windows sanal masaüstü için oluşturduğunuz Azure sanal makineleri şu şekilde olmalıdır:

* [Standart etki alanına katılmış](../active-directory-domain-services/active-directory-ds-comparison.md) veya [karma ad 'ye katılmış](../active-directory/devices/hybrid-azuread-join-plan.md). Sanal makineler Azure AD 'ye katılmış olamaz.
* Aşağıdaki [desteklenen işletim sistemi görüntülerinden](#supported-virtual-machine-os-images)birini çalıştırın.

>[!NOTE]
>Bir Azure aboneliğine ihtiyacınız varsa, bir [aylık ücretsiz deneme için kaydolabilirsiniz](https://azure.microsoft.com/free/). Azure 'un ücretsiz deneme sürümünü kullanıyorsanız, Windows Server Active Directory Azure Active Directory ile eşitlenmiş halde tutmak için Azure AD Domain Services kullanmanız gerekir.

Windows sanal masaüstü için oluşturduğunuz Azure sanal makinelerinin aşağıdaki URL 'Lere erişimi olmalıdır:

|Adres|Giden TCP bağlantı noktası|Amaç|Hizmet etiketi|
|---|---|---|---|
|*. wvd.microsoft.com|443|Hizmet trafiği|WindowsVirtualDesktop|
|mrsglobalsteus2prod.blob.core.windows.net|443|Aracı ve SXS yığın güncelleştirmeleri|AzureCloud|
|*.core.windows.net|443|Aracı trafiği|AzureCloud|
|*.servicebus.windows.net|443|Aracı trafiği|AzureCloud|
|prod.warmpath.msftcloudes.com|443|Aracı trafiği|AzureCloud|
|catalogartifact.azureedge.net|443|Azure Market|AzureCloud|
|kms.core.windows.net|1688|Windows etkinleştirme|Internet|
|wvdportalstorageblob.blob.core.windows.net|443|Azure portal desteği|AzureCloud|

>[!IMPORTANT]
>Hizmet sorunlarını engellemek için çoğu durumda URL yerine hizmet etiketlerini kullanmanızı öneririz. Güvenilir bir Windows sanal masaüstü dağıtımı için bu URL 'Lerin engellemesini kaldırma işlemi gereklidir. Bu URL 'lere erişimin engellenmesi desteklenmez ve hizmet işlevlerini etkiler. Bu URL 'Ler yalnızca Windows sanal masaüstü siteleri ve kaynaklarına karşılık gelir ve Azure Active Directory gibi diğer hizmetlere yönelik URL 'Ler içermez.
>
>Windows sanal masaüstü artık FQDN etiketini destekliyor.

Aşağıdaki tabloda, Azure sanal makinelerinizin erişebileceği isteğe bağlı URL 'Ler listelenmektedir:

|Adres|Giden TCP bağlantı noktası|Amaç|Hizmet etiketi|
|---|---|---|---|
|*.microsoftonline.com|443|MS Online Services ile kimlik doğrulama|Hiçbiri|
|*. events.data.microsoft.com|443|Telemetri hizmeti|Hiçbiri|
|www.msftconnecttest.com|443|İşletim sisteminin Internet 'e bağlı olup olmadığını algılar|Hiçbiri|
|*. prod.do.dsp.mp.microsoft.com|443|Windows Update|Hiçbiri|
|login.windows.net|443|MS online hizmetlerinde oturum açın, Office 365|Hiçbiri|
|*. sfx.ms|443|OneDrive istemci yazılımı güncelleştirmeleri|Hiçbiri|
|*. digicert.com|443|Sertifika iptal denetimi|Hiçbiri|


>[!NOTE]
>Windows sanal masaüstü, ağ trafiğine izin vermek için beyaz listeye sahip bir IP adresi aralığı listesine sahip değil. Şu anda yalnızca belirli URL 'Lerin beyaz listeye alınmasını destekliyoruz.
>
>Gerekli Azure Active Directory ilgili URL 'Ler dahil olmak üzere Office ile ilgili URL 'Lerin bir listesi için bkz. [office 365 URL 'leri ve IP adresi aralıkları](/office365/enterprise/urls-and-ip-address-ranges).
>
>Hizmet trafiğiyle ilgili URL 'Ler için (*) joker karakterini kullanmanız gerekir. Aracıyla ilgili trafik için * kullanmayı tercih ediyorsanız, joker karakterleri olmayan URL 'Leri bulma hakkında bilgi edinebilirsiniz:
>
>1. Sanal makinelerinizi Windows sanal masaüstü ana bilgisayar havuzuna kaydedin.
>2. **Olay Görüntüleyicisi** 'ni açın ve **Windows günlükleri** > **uygulaması** > **WVD-Agent** ' a gidin ve olay kimliği 3702 ' i arayın.
>3. Olay KIMLIĞI 3702 altında bulduğunuz URL 'Leri beyaz listeye ekleyin. Olay KIMLIĞI 3702 altındaki URL 'Ler bölgeye özeldir. Beyaz Listeleme işlemini, sanal makinelerinizi dağıtmak istediğiniz her bölge için ilgili URL 'lerle tekrarlamanız gerekecektir.

Windows sanal masaüstü, kullanıcılara ve Microsoft tarafından Azure 'da bir hizmet olarak barındırılan yönetim çözümüne ait olan Windows Masaüstü ve uygulamalarını içerir. Masaüstleri ve uygulamalar, herhangi bir Azure bölgesindeki sanal makinelerde (VM) dağıtılabilir ve bu VM 'Lerin yönetim çözümü ve verileri Birleşik Devletler yer alır. Bu, Birleşik Devletler veri aktarımına neden olabilir.

En iyi performans için, ağınızın aşağıdaki gereksinimleri karşıladığından emin olun:

* İstemci ağından, ana bilgisayar havuzlarının dağıtıldığı Azure bölgesine gidiş dönüş (RTT) gecikmesi 150 MS 'den az olmalıdır.
* Masaüstleri ve uygulamalar barındıran VM 'Ler Yönetim hizmetine bağlandığında ağ trafiği ülke/bölge kenarlıklarının dışına akabilir.
* Ağ performansını iyileştirmek için, oturum ana bilgisayarının VM 'lerinin yönetim hizmeti ile aynı Azure bölgesinde birlikte bulunmasını öneririz.

## <a name="supported-remote-desktop-clients"></a>Desteklenen uzak masaüstü istemcileri

Aşağıdaki uzak masaüstü istemcileri Windows sanal masaüstünü destekler:

* [Windows Masaüstü](connect-windows-7-and-10.md)
* [Web](connect-web.md)
* [Mac OS](connect-macos.md)
* [iOS](connect-ios.md)
* [Android (Önizleme)](connect-android.md)

> [!IMPORTANT]
> Windows sanal masaüstü, RemoteApp ve Masaüstü bağlantıları (ÇDC) istemcisini veya Uzak Masaüstü Bağlantısı (MSTSC) istemcisini desteklemez.

> [!IMPORTANT]
> Windows sanal masaüstü Şu anda Windows Mağazası 'ndan uzak masaüstü istemcisini desteklememektedir. Bu istemciye yönelik destek gelecekteki bir sürüme eklenecektir.

Uzak Masaüstü istemcilerinin aşağıdaki URL 'Lere erişimi olmalıdır:

|Adres|Giden TCP bağlantı noktası|Amaç|İstemci (ler)|
|---|---|---|---|
|*. wvd.microsoft.com|443|Hizmet trafiği|Tümü|
|*.servicebus.windows.net|443|Sorun giderme verileri|Tümü|
|go.microsoft.com|443|Microsoft FWLinks|Tümü|
|aka.ms|443|Microsoft URL kısaltalayıcı|Tümü|
|docs.microsoft.com|443|Belgeler|Tümü|
|privacy.microsoft.com|443|Gizlilik bildirimi|Tümü|
|query.prod.cms.rt.microsoft.com|443|İstemci güncelleştirmeleri|Windows Masaüstü|

>[!IMPORTANT]
>Bu URL 'Leri açmak, güvenilir bir istemci deneyimi için gereklidir. Bu URL 'lere erişimin engellenmesi desteklenmez ve hizmet işlevlerini etkiler. Bu URL 'Ler yalnızca istemci sitelerine ve kaynaklarına karşılık gelir ve Azure Active Directory gibi diğer hizmetlere yönelik URL 'Ler içermez.

## <a name="supported-virtual-machine-os-images"></a>Desteklenen sanal makine işletim sistemi görüntüleri

Windows sanal masaüstü aşağıdaki x64 işletim sistemi görüntülerini destekler:

* Windows 10 Enterprise multi-session, sürüm 1809 veya üzeri
* Windows 10 Enterprise, sürüm 1809 veya üzeri
* Windows 7 Enterprise
* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2

Windows sanal masaüstü, x86 (32-bit), Windows 10 Enterprise N veya Windows 10 Enterprise KN işletim sistemi görüntülerini desteklemez. Windows 7, bir sektör boyutu sınırlaması nedeniyle yönetilen Azure depolama üzerinde barındırılan VHD veya VHDX tabanlı profil çözümlerini de desteklemez.

Kullanılabilir Otomasyon ve dağıtım seçenekleri, aşağıdaki tabloda gösterildiği gibi, hangi işletim sistemi ve sürümü seçdiğinize bağlıdır: 

|İşletim sistemi|Azure görüntü Galerisi|El ile VM dağıtımı|Azure Resource Manager şablonu tümleştirmesi|Azure Market 'te konak havuzları sağlama|Windows sanal masaüstü Aracısı güncelleştirmeleri|
|--------------------------------------|:------:|:------:|:------:|:------:|:------:|
|Windows 10 çoklu oturum, sürüm 1903|Yes|Yes|Yes|Yes|Automatic|
|Windows 10 çoklu oturum, sürüm 1809|Yes|Yes|Hayır|Hayır|Automatic|
|Windows 10 Enterprise, sürüm 1903|Yes|Yes|Yes|Yes|Automatic|
|Windows 10 Enterprise, sürüm 1809|Yes|Yes|Hayır|Hayır|Automatic|
|Windows 7 Enterprise|Yes|Yes|Hayır|Hayır|El ile|
|Windows Server 2019|Yes|Yes|Hayır|Hayır|Automatic|
|Windows Server 2016|Yes|Yes|Yes|Yes|Automatic|
|Windows Server 2012 R2|Yes|Yes|Hayır|Hayır|Automatic|

## <a name="next-steps"></a>Sonraki adımlar

Windows sanal masaüstü Fall 2019 sürümünü kullanıyorsanız, [Windows sanal masaüstü 'nde kiracı oluşturma bölümünde](./virtual-desktop-fall-2019/tenant-setup-azure-active-directory.md)öğreticimize başlamanızı sağlayabilirsiniz.

Windows sanal masaüstü Spring 2020 sürümünü kullanıyorsanız bunun yerine bir konak havuzu oluşturmanız gerekir. Kullanmaya başlamak için aşağıdaki Öğreticiye gidin.

> [!div class="nextstepaction"]
> [Azure portal bir konak havuzu oluşturun](create-host-pools-azure-marketplace.md)
