---
title: Windows Sanal Masaüstü nedir? - Azure
description: Windows Sanal Masaüstü'ne genel bakış.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: overview
ms.date: 04/10/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 927696d029bf1b8742dc0001e03799322f368191
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81261729"
---
# <a name="what-is-windows-virtual-desktop"></a>Windows Sanal Masaüstü nedir? 

Windows Sanal Masaüstü bulutta çalışan bir masaüstü ve uygulama sanallaştırma hizmetidir.

Azure'da Windows Sanal Masaüstü çalıştırdığınızda şunları yapabilirsiniz:

* Ölçeklenebilirlik ile tam bir Windows 10 sağlayan çok oturumlu bir Windows 10 dağıtımı ayarlama
* Office 365 ProPlus'ı sanallaştırın ve çok kullanıcılı sanal senaryolarda çalışacak şekilde optimize edin
* Windows 7 sanal masaüstlerine ücretsiz Genişletilmiş Güvenlik Güncelleştirmeleri sağlayın
* Mevcut Uzak Masaüstü Hizmetleri (RDS) ve Windows Server masaüstü ve uygulamalarınızı herhangi bir bilgisayara taşıyın
* Hem masaüstü hem de uygulamaları sanallaştırma
* Windows 10, Windows Server ve Windows 7 masaüstü ve uygulamalarını birleşik bir yönetim deneyimiyle yönetme

## <a name="introductory-video"></a>Tanıtım videosu

Windows Sanal Masaüstü hakkında bilgi edinin, neden benzersiz olduğunu ve bu videoda yenilikler hakkında bilgi edinin:

<br></br><iframe src="https://www.youtube.com/embed/NQFtI3JLtaU" width="640" height="320" allowFullScreen="true" frameBorder="0"></iframe>

Windows Sanal Masaüstü hakkında daha fazla video için [çalma listemize](https://www.youtube.com/watch?v=NQFtI3JLtaU&list=PLXtHYVsvn_b8KAKw44YUpghpD6lg-EHev)bakın.

## <a name="key-capabilities"></a>Temel işlevler

Windows Sanal Masaüstü ile ölçeklenebilir ve esnek bir ortam ayarlayabilirsiniz:

* Ek ağ geçidi sunucuları çalıştırmak zorunda kalmadan Azure aboneliğinizde tam bir masaüstü sanallaştırma ortamı oluşturun.
* Çeşitli iş yüklerinizi karşılamak için istediğiniz kadar ana bilgisayar havuzu yayınlayın.
* Azure Galerisi'nden üretim iş yükleri için kendi resminizi veya testinizi getirin.
* Havuzlanmış, çok oturumlu kaynaklarla maliyetleri azaltın. Windows Server'daki Windows Sanal Masaüstü ve Uzak Masaüstü Oturum Ana Bilgisayar (RDSH) rolüne özel yeni Windows 10 Enterprise çok oturumlu özelliğiyle, kullanıcılarınıza aynı kaynakları sağlarken sanal makine ve işletim sistemi (OS) ek yükü sayısını büyük ölçüde azaltabilirsiniz.
* Kişisel (kalıcı) masaüstü bilgisayarlar aracılığıyla bireysel sahiplik sağlayın.

Sanal masaüstlerini dağıtabilir ve yönetebilirsiniz:

* Ana bilgisayar havuzlarını yapılandırmak, uygulama grupları oluşturmak, kullanıcıları atamak ve kaynakları yayımlamak için Windows Virtual Desktop PowerShell ve REST arabirimlerini kullanın.
* Tek bir ana bilgisayar havuzundan tam masaüstü veya tek tek uzak uygulamalar yayınlayın, farklı kullanıcı grupları için ayrı uygulama grupları oluşturun ve hatta görüntü sayısını azaltmak için kullanıcıları birden çok uygulama grubuna atayın.
* Ortamınızı yönetirken, çeşitli yapılandırma veya kullanıcı hatalarını anlamak için roller atamak ve tanılama toplamak için yerleşik yerleşik erişimi kullanın.
* Hataları gidermek için yeni Tanılama hizmetini kullanın.
* Yalnızca görüntüyü ve sanal makineleri yönetin, altyapıyı değil. Uzak Masaüstü Hizmetlerinde yaptığınız gibi Uzak Masaüstü rollerini, sadece Azure aboneliğinizdeki sanal makineleri kişisel olarak yönetmeniz gerekmez.

Ayrıca, kullanıcıları sanal masaüstünüze atayabilir ve bağlayabilirsiniz:

* Kullanıcılar, atandıktan sonra, kullanıcıları yayınlanmış Windows masaüstü ve uygulamalarına bağlamak için herhangi bir Windows Sanal Masaüstü istemcisini başlatabilir. Cihazınızdaki yerel bir uygulama veya Windows Virtual Desktop HTML5 web istemcisi aracılığıyla herhangi bir cihazdan bağlanın.
* Hizmete ters bağlantılar aracılığıyla kullanıcıları güvenli bir şekilde kurun, böylece gelen bağlantı noktalarını açık bırakmak zorunda kalmadan.

## <a name="requirements"></a>Gereksinimler

Windows Sanal Masaüstü'nü kurmak ve kullanıcılarınızı Windows masaüstü ve uygulamalarına başarıyla bağlamak için gereken birkaç şey vardır.

Aşağıdaki İş ses teşekkülleri için destek eklemeyi planlıyoruz, bu nedenle dağıtmayı planladığınız masaüstü ve uygulamalara göre kullanıcılarınız için [uygun lisanslara](https://azure.microsoft.com/pricing/details/virtual-desktop/) sahip olduğunuzdan emin olun:

|İşletim Sistemi|Gerekli lisans|
|---|---|
|Windows 10 Enterprise çoklu oturum veya Windows 10 Enterprise|Microsoft 365 E3, E5, A3, A5, F1, İş<br>Windows E3, E5, A3, A5|
|Windows 7 Enterprise |Microsoft 365 E3, E5, A3, A5, F1, İş<br>Windows E3, E5, A3, A5|
|Windows Server 2012 R2, 2016, 2019|Yazılım Güvencesi ile RDS İstemci Erişim Lisansı (CAL)|

Altyapınızın Windows Sanal Masaüstü'nü desteklemek için aşağıdaki şeylere ihtiyacı vardır:

* [Azure Etkin Dizini](/azure/active-directory/)
* Azure Etkin Dizinile senkronize bir Windows Server Etkin Dizini. Bunu aşağıdakilerden biriyle yapılandırabilirsiniz:
  * Azure AD Connect (karma kuruluşlar için)
  * Azure AD Etki Alanı Hizmetleri (karma veya bulut kuruluşları için)
* Windows Server Etkin Dizini içeren veya bağlı sanal ağ içeren bir Azure aboneliği
  
Windows Sanal Masaüstü için oluşturduğunuz Azure sanal makineleri:

* [Standart etki alanı birleştirilmiş](../active-directory-domain-services/active-directory-ds-comparison.md) veya [Karma AD katıldı.](../active-directory/devices/hybrid-azuread-join-plan.md) Sanal makineler Azure AD'ye katıldı olamaz.
* Aşağıdaki [desteklenen işletim sistemi görüntülerinden](#supported-virtual-machine-os-images)birini çalıştırma.

>[!NOTE]
>Azure aboneliğine ihtiyacınız varsa, [bir aylık ücretsiz deneme sürümüne kaydolabilirsiniz.](https://azure.microsoft.com/free/) Azure'un ücretsiz deneme sürümünü kullanıyorsanız, Windows Server Etkin Dizininizi Azure Etkin Dizini ile senkronize tutmak için Azure AD Etki Alanı Hizmetlerini kullanmanız gerekir.

Windows Sanal Masaüstü için oluşturduğunuz Azure sanal makinelerinin aşağıdaki URL'lere erişimi olmalıdır:

|Adres|Giden TCP bağlantı noktası|Amaç|Servis Etiketi|
|---|---|---|---|
|*.wvd.microsoft.com|443|Servis trafiği|WindowsVirtualDesktop|
|mrsglobalsteus2prod.blob.core.windows.net|443|Aracı ve SXS yığın güncellemeleri|AzureCloud|
|*.core.windows.net|443|Acente trafiği|AzureCloud|
|*.servicebus.windows.net|443|Acente trafiği|AzureCloud|
|prod.warmpath.msftcloudes.com|443|Acente trafiği|AzureCloud|
|catalogartifact.azureedge.net|443|Azure Market|AzureCloud|
|kms.core.windows.net|1688|Windows etkinleştirme|Internet|

>[!IMPORTANT]
>Hizmet sorunlarını önlemek için çoğu durumda URL yerine servis etiketlerini kullanmanızı öneririz. Bu URL'lerin engelini kaldırmak, güvenilir bir Windows Sanal Masaüstü dağıtımı için çok önemlidir. Bu URL'lere erişimi engellemek desteklenmez ve hizmet işlevselliğini etkiler. Bu URL'ler yalnızca Windows Sanal Masaüstü sitelerine ve kaynaklarına karşılık gelir ve Azure Active Directory gibi diğer hizmetler için URL içermez.

Aşağıdaki tabloda, Azure sanal makinelerinizin erişebileceği isteğe bağlı URL'ler listelenemeniz gerekir:

|Adres|Giden TCP bağlantı noktası|Amaç|Servis Etiketi|
|---|---|---|---|
|*.microsoftonline.com|443|MS Online Hizmetlerine Kimlik Doğrulama|None|
|*.events.data.microsoft.com|443|Telemetri Servisi|None|
|www.msftconnecttest.com|443|İşletim sistemi internete bağlı olup olmadığını algılar|None|
|*.prod.do.dsp.mp.microsoft.com|443|Windows Update|None|
|login.windows.net|443|MS Online Hizmetlere Giriş, Office 365|None|
|*.sfx.ms|443|OneDrive istemci yazılımı için güncelleştirmeler|None|
|*.digicert.com|443|Sertifika iptal denetimi|None|


>[!NOTE]
>Windows Sanal Masaüstü'nde şu anda ağ trafiğine izin vermek için beyaz listeye alabileceğiniz IP adresi aralıklarının bir listesi yok. Şu anda yalnızca belirli URL'leri beyaz listeye alma yı destekliyoruz.
>
>Gerekli Azure Etkin Dizin le ilgili URL'ler de dahil olmak üzere Office ile ilgili [URL'lerin](/office365/enterprise/urls-and-ip-address-ranges)listesi için bkz.
>
>Servis trafiğini içeren URL'ler için joker karakter (*) kullanmanız gerekir. Aracıyla ilgili trafik için * kullanmamayı tercih ederseniz, joker karaktersiz URL'leri şu şekilde bulabilirsiniz:
>
>1. Sanal makinelerinizi Windows Sanal Masaüstü ana bilgisayar havuzuna kaydedin.
>2. **Olay görüntüleyicisini** açın ve **Windows günlükleri** > **Application** > **WVD-Agent** gidin ve Olay Kimliği 3702 arayın.
>3. Event ID 3702 altında bulduğunuz URL'leri beyaz listele. Event ID 3702 altındaki URL'ler bölgeye özgür. Sanal makinelerinizi dağıtmak istediğiniz her bölge için ilgili URL'lerle beyaz liste işlemini yinelemeniz gerekir.

Windows Sanal Masaüstü, kullanıcılara sağladığınız Windows masaüstü ve uygulamalarını ve Microsoft tarafından Azure'da bir hizmet olarak barındırılan yönetim çözümünü kapsar. Masaüstü bilgisayarlar ve uygulamalar herhangi bir Azure bölgesindeki sanal makinelerde (VM' ler) dağıtılabilir ve bu Sanal Bilgisayarlara yönelik yönetim çözümü ve verileri ABD'de bulunabilir. Bu, ABD'ye veri aktarımı yla sonuçlanabilir.

En iyi performans için, ağınızın aşağıdaki gereksinimleri karşıladığından emin olun:

* Müşteri ağından ana bilgisayar havuzlarının dağıtıldığı Azure bölgesine gidiş-dönüş (RTT) gecikmesi 150 ms'den az olmalıdır.
* Masaüstü ve uygulamaları barındıran VM'ler yönetim hizmetine bağlandığında ağ trafiği ülke/bölge sınırları dışında akabilir.
* Ağ performansını optimize etmek için, oturum ana bilgisayar larının VM'lerinin yönetim hizmetiyle aynı Azure bölgesinde toplanmış olmasını öneririz.

## <a name="supported-remote-desktop-clients"></a>Desteklenen Uzak Masaüstü istemcileri

Aşağıdaki Uzak Masaüstü istemcileri Windows Sanal Masaüstü'nü destekler:

* [Windows Masaüstü](connect-windows-7-and-10.md)
* [Web](connect-web.md)
* [macOS](connect-macos.md)
* [iOS](connect-ios.md)
* [Android (Önizleme)](connect-android.md)

> [!IMPORTANT]
> Windows Sanal Masaüstü, RemoteApp ve Masaüstü Bağlantıları (RADC) istemcisini veya Uzak Masaüstü Bağlantısı (MSTSC) istemcisini desteklemez.

> [!IMPORTANT]
> Windows Sanal Masaüstü şu anda Windows Mağazası'ndan Uzak Masaüstü istemcisini desteklemiyor. Bu istemci için destek gelecekteki bir sürümde eklenecektir.

Uzak Masaüstü istemcileri aşağıdaki URL'lere erişebilmeli:

|Adres|Giden TCP bağlantı noktası|Amaç|İstemci(ler)|
|---|---|---|---|
|*.wvd.microsoft.com|443|Servis trafiği|Tümü|
|*.servicebus.windows.net|443|Veri sorun giderme|Tümü|
|go.microsoft.com|443|Microsoft FWLinks|Tümü|
|aka.ms|443|Microsoft URL kısaltıcı|Tümü|
|docs.microsoft.com|443|Belgeler|Tümü|
|privacy.microsoft.com|443|Gizlilik bildirimi|Tümü|
|query.prod.cms.rt.microsoft.com|443|İstemci güncellemeleri|Windows Masaüstü|

>[!IMPORTANT]
>Bu URL'lerin açılması güvenilir bir istemci deneyimi için çok önemlidir. Bu URL'lere erişimi engellemek desteklenmez ve hizmet işlevselliğini etkiler. Bu URL'ler yalnızca istemci sitelerine ve kaynaklarına karşılık gelir ve Azure Etkin Dizini gibi diğer hizmetler için URL içermez.

## <a name="supported-virtual-machine-os-images"></a>Desteklenen sanal makine işletim sistemi görüntüleri

Windows Sanal Masaüstü aşağıdaki x64 işletim sistemi görüntülerini destekler:

* Windows 10 Enterprise çoklu oturum, sürüm 1809 veya sonrası
* Windows 10 Enterprise, sürüm 1809 veya sonrası
* Windows 7 Enterprise
* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2

Windows Sanal Masaüstü x86 (32 bit), Windows 10 Enterprise N veya Windows 10 Enterprise KN işletim sistemi görüntülerini desteklemez. Windows 7, sektör boyutu sınırlaması nedeniyle yönetilen Azure Depolama'da barındırılan VHD veya VHDX tabanlı profil çözümlerini de desteklemez.

Kullanılabilir otomasyon ve dağıtım seçenekleri, aşağıdaki tabloda gösterildiği gibi hangi işletim sistemi ve sürümü seçtiğinize bağlıdır: 

|İşletim sistemi|Azure Resim Galerisi|Manuel VM dağıtımı|Azure Kaynak Yöneticisi şablon tümleştirmesi|Azure Marketi'nde sağlama ana bilgisayar havuzları|Windows Sanal Masaüstü Aracısı güncellemeleri|
|--------------------------------------|:------:|:------:|:------:|:------:|:------:|
|Windows 10 çoklu oturum, sürüm 1903|Evet|Evet|Evet|Evet|Automatic|
|Windows 10 çoklu oturum, sürüm 1809|Evet|Evet|Hayır|Hayır|Automatic|
|Windows 10 Enterprise, sürüm 1903|Evet|Evet|Evet|Evet|Automatic|
|Windows 10 Enterprise, sürüm 1809|Evet|Evet|Hayır|Hayır|Automatic|
|Windows 7 Enterprise|Evet|Evet|Hayır|Hayır|El ile|
|Windows Server 2019|Evet|Evet|Hayır|Hayır|Automatic|
|Windows Server 2016|Evet|Evet|Evet|Evet|Automatic|
|Windows Server 2012 R2|Evet|Evet|Hayır|Hayır|Automatic|

## <a name="next-steps"></a>Sonraki adımlar

Başlamak için bir kiracı oluşturmanız gerekir. Kiracı oluşturma hakkında daha fazla bilgi edinmek için kiracı oluşturma öğreticisine devam edin.

> [!div class="nextstepaction"]
> [Windows Sanal Masaüstü'nde kiracı oluşturma](tenant-setup-azure-active-directory.md)
