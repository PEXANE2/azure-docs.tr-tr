---
title: StorSimple 8000 serisi sistem gereksinimleri | Microsoft Dokümanlar
description: Microsoft Azure StorSimple çözümü için yazılım, ağ oluşturma ve yüksek kullanılabilirlik gereksinimlerini ve en iyi uygulamaları açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 09/28/2017
ms.author: alkohli
ms.openlocfilehash: 3032585c6f0a5cc6143eee06b12b6def50cd7cd0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80297717"
---
# <a name="storsimple-8000-series-software-high-availability-and-networking-requirements"></a>StorSimple 8000 serisi yazılım, yüksek kullanılabilirlik ve ağ gereksinimleri

## <a name="overview"></a>Genel Bakış

[!INCLUDE [storsimple-8000-eol-banner](../../includes/storsimple-8000-eol-banner.md)]

Microsoft Azure StorSimple'a hoş geldiniz. Bu makalede, StorSimple aygıtınız ve aygıta erişen depolama istemcileri için önemli sistem gereksinimleri ve en iyi uygulamalar açıklanmaktadır. StorSimple sisteminizi dağıtmadan önce bilgileri dikkatle gözden geçirmenizi ve dağıtım ve sonraki işlem sırasında gerekli olduğu şekilde geri başvurmanızı öneririz.

Sistem gereksinimleri şunlardır:

* **Depolama istemcileri için yazılım gereksinimleri** - desteklenen işletim sistemlerini ve bu işletim sistemleri için ek gereksinimleri açıklar.
* **StorSimple aygıtıiçin ağ gereksinimleri** - iSCSI, bulut veya yönetim trafiğine izin vermek için güvenlik duvarınızda açık olması gereken bağlantı noktaları hakkında bilgi sağlar.
* **StorSimple için yüksek kullanılabilirlik gereksinimleri** - StorSimple aygıtınız ve ana bilgisayarınız için yüksek kullanılabilirlik gereksinimlerini ve en iyi uygulamaları açıklar.

## <a name="software-requirements-for-storage-clients"></a>Depolama istemcileri için yazılım gereksinimleri

Aşağıdaki yazılım gereksinimleri, StorSimple cihazınıza erişen depolama istemcileri içindir.

| Desteklenen işletim sistemleri | Sürüm gerekli | Ek gereksinimler/notlar |
| --- | --- | --- |
| Windows Server |2008 R2 SP1, 2012, 2012 R2, 2016 |StorSimple iSCSI birimleri yalnızca aşağıdaki Windows disk türlerinde kullanılmak üzere desteklenir:<ul><li>Temel diskte basit hacim</li><li>Dinamik diskte basit ve yansıtılmış ses düzeyi</li></ul>Yalnızca işletim sisteminde yerel olarak bulunan yazılım iSCSI başlatıcıları desteklenir. Donanım iSCSI başlatıcıları desteklenmez.<br></br>Windows Server 2012 ve 2016 ince sağlama ve ODX özellikleri, StorSimple iSCSI birimi kullanıyorsanız desteklenir.<br><br>StorSimple ince ve tam olarak sağlanan hacimler oluşturabilirsiniz. Kısmen sağlanan hacimler oluşturamaz.<br><br>İnce bir şekilde sağlanan bir hacmin yeniden biçimlendirmesi uzun zaman alabilir. Sesi silmenizi ve yeniden biçimlendirmek yerine yeni bir ses oluşturmanızı öneririz. Ancak, yine de bir birimde yeniden biçim vermeyi tercih ediyorsanız:<ul><li>Run the following command before the reformat to avoid space reclamation delays: <br>`fsutil behavior set disabledeletenotify 1`</br></li><li>Biçimlendirme tamamlandıktan sonra, alan ıslahı yeniden etkinleştirmek için aşağıdaki komutu kullanın:<br>`fsutil behavior set disabledeletenotify 0`</br></li><li>[KB 2878635'te](https://support.microsoft.com/kb/2870270) açıklandığı gibi Windows Server 2012 düzeltmesini Windows Server bilgisayarınıza uygulayın.</li></ul></li></ul></ul> SharePoint için StorSimple Snapshot Manager veya StorSimple Adapter'ı yapılandırıyorsanız, [isteğe bağlı bileşenler için Yazılım gereksinimlerine](#software-requirements-for-optional-components)gidin. |
| VMware ESX |5.5 ve 6.0 |iSCSI istemcisi olarak VMware vSphere ile desteklenir. VAAI-blok özelliği StorSimple cihazlarda VMware vSphere ile desteklenir. |
| Linux RHEL/CentOS |5, 6 ve 7 |Open-iSCSI başlatıcı sürümleri 5, 6 ve 7 ile Linux iSCSI istemcileri için destek. |
| Linux |SUSE Linux 11 | |

> [!NOTE]
> IBM AIX şu anda StorSimple ile desteklenmez.


## <a name="software-requirements-for-optional-components"></a>İsteğe bağlı bileşenler için yazılım gereksinimleri

Aşağıdaki yazılım gereksinimleri isteğe bağlı StorSimple bileşenleri (StorSimple Snapshot Manager ve SharePoint için StorSimple Adaptörü) içindir.

| Bileşen | Ana bilgisayar platformu | Ek gereksinimler/notlar |
| --- | --- | --- |
| StorSimple Snapshot Manager |Windows Server 2008 R2 SP1, 2012, 2012 R2 |Windows Server'da StorSimple Snapshot Manager'ın kullanılması, aynalı dinamik disklerin yedeklenme/geri yüklenme leri ve uygulama tutarlı yedeklemeler için gereklidir.<br> StorSimple Snapshot Manager yalnızca Windows Server 2008 R2 SP1 (64 bit), Windows Server 2012 R2 ve Windows Server 2012'de desteklenir.<ul><li>Window Server 2012 kullanıyorsanız, StorSimple Snapshot Manager'ı yüklemeden önce .NET 3.5-4.5 yüklemeniz gerekir.</li><li>Windows Server 2008 R2 SP1 kullanıyorsanız, StorSimple Snapshot Manager'ı yüklemeden önce Windows Management Framework 3.0'ı yüklemeniz gerekir.</li></ul> |
| SharePoint için StorSimple Bağdaştırıcısı |Windows Server 2008 R2 SP1, 2012, 2012 R2 |<ul><li>SharePoint için StorSimple Adaptörü yalnızca SharePoint 2010 ve SharePoint 2013'te desteklenir.</li><li>RBS SQL Server Enterprise Edition, sürüm 2008 R2 veya 2012 gerektirir.</li></ul> |

## <a name="networking-requirements-for-your-storsimple-device"></a>StorSimple cihazınız için ağ gereksinimleri

StorSimple aygıtınız kilitli bir aygıttır. Ancak, iSCSI, bulut ve yönetim trafiğine izin vermek için güvenlik duvarınızda bağlantı noktalarının açılması gerekir. Aşağıdaki tabloda güvenlik duvarınızda açılması gereken bağlantı noktaları listelenir. Bu tabloda, *gelen* *istemci* isteklerinin cihazınıza eriştikleri yönü ifade eder. *Giden* veya *giden,* StorSimple cihazınızın verileri dağıtımın ötesinde harici olarak gönderdiği yönü ifade eder: örneğin, Internet'e giden.

| Bağlantı Noktası No:<sup>1,2</sup> | Veya dışarıda | Bağlantı noktası kapsamı | Gerekli | Notlar |
| --- | --- | --- | --- | --- |
| TCP 80 (HTTP)<sup>3</sup> |Out |WAN |Hayır |<ul><li>Giden bağlantı noktası güncelleştirmeleri almak için Internet erişimi için kullanılır.</li><li>Giden web proxy kullanıcı yapılandırılabilir.</li><li>Sistem güncelleştirmelerine izin vermek için bu bağlantı noktasının denetleyici sabit IP'ler için de açık olması gerekir.</li></ul> |
| TCP 443 (HTTPS)<sup>3</sup> |Out |WAN |Evet |<ul><li>Giden bağlantı noktası buluttaki verilere erişmek için kullanılır.</li><li>Giden web proxy kullanıcı yapılandırılabilir.</li><li>Sistem güncelleştirmelerine izin vermek için bu bağlantı noktasının denetleyici sabit IP'ler için de açık olması gerekir.</li><li>Bu bağlantı noktası, çöp toplama için her iki denetleyicide de kullanılır.</li></ul> |
| UDP 53 (DNS) |Out |WAN |Bazı durumlarda; notlara bakın. |Bu bağlantı noktası yalnızca Internet tabanlı bir DNS sunucusu kullanıyorsanız gereklidir. |
| UDP 123 (NTP) |Out |WAN |Bazı durumlarda; notlara bakın. |Bu bağlantı noktası yalnızca Internet tabanlı bir NTP sunucusu kullanıyorsanız gereklidir. |
| TCP 9354 |Out |WAN |Evet |Giden bağlantı noktası StorSimple Aygıt Yöneticisi hizmeti ile iletişim kurmak için StorSimple aygıtı tarafından kullanılır. |
| 3260 (iSCSI) |İçindeki |LAN |Hayır |Bu bağlantı noktası iSCSI üzerinden verilere erişmek için kullanılır. |
| 5985 |İçindeki |LAN |Hayır |Gelen bağlantı noktası, StorSimple aygıtıyla iletişim kurmak için StorSimple Snapshot Manager tarafından kullanılır.<br>Bu bağlantı noktası, HTTP üzerinden StorSimple için Windows PowerShell'e uzaktan bağlandığınızda da kullanılır. |
| 5986 |İçindeki |LAN |Hayır |Bu bağlantı noktası, HTTPS üzerinden StorSimple için Windows PowerShell'e uzaktan bağlandığınızda kullanılır. |

<sup>1</sup> Gelen bağlantı noktalarının genel Internet'te açılması gerekmez.

<sup>2</sup> Birden çok bağlantı noktası bir ağ geçidi yapılandırması taşıyorsa, giden yönlendirilmiş trafik sırası, [Aşağıdaki Bağlantı Noktası yönlendirmesinde](#routing-metric)açıklanan bağlantı noktası yönlendirme sırasına göre belirlenir.

<sup>3</sup> StorSimple cihazınızdaki denetleyici sabit IP'ler, doğrudan veya yapılandırılmış web proxy'si aracılığıyla Internet'e bağlanabilmeli ve bağlanabilmeli. Sabit IP adresleri, aygıttaki güncelleştirmelere servis yapmak ve çöp toplama için kullanılır. Aygıt denetleyicileri sabit IP'ler aracılığıyla Internet'e bağlanamıyorsa, StorSimple cihazınızı güncelleyemezsiniz ve çöp toplama düzgün çalışmaz.

> [!IMPORTANT]
> Güvenlik duvarının StorSimple aygıtı ile Azure arasındaki TLS trafiğini değiştirmediğinden veya şifresini çözmediğinden emin olun.


### <a name="url-patterns-for-firewall-rules"></a>Güvenlik duvarı kuralları için URL desenleri

Ağ yöneticileri genellikle gelen ve giden trafiği filtrelemek için URL desenlerini temel alan gelişmiş güvenlik duvarı kurallarını yapılandırabilir. StorSimple aygıtınız ve StorSimple Aygıt Yöneticisi hizmetiniz Azure Hizmet Veri Yolundaki Diğer Microsoft uygulamalarına, Azure Active Directory Access Control,depolama hesapları ve Microsoft Update sunucularına bağlıdır. Bu uygulamalarla ilişkili URL desenleri güvenlik duvarı kurallarını yapılandırmak için kullanılabilir. Bu uygulamalarla ilişkili URL desenlerinin değişebileceğini anlamak önemlidir. Bu da ağ yöneticisinin StorSimple'ınız için güvenlik duvarı kurallarını gerektiği gibi ve gerektiğinde izlemesi ve güncelleştirmesini gerektirir.

Çoğu durumda, StorSimple sabit IP adreslerine göre, giden trafik için güvenlik duvarı kurallarınızı belirlemenizi öneririz. Ancak, güvenli ortamlar oluşturmak için gereken gelişmiş güvenlik duvarı kuralları ayarlamak için aşağıdaki bilgileri kullanabilirsiniz.

> [!NOTE]
> Aygıt (kaynak) IP'leri her zaman tüm etkin ağ arabirimleri için ayarlanmalıdır. Hedef IP'ler Azure [veri merkezi IP aralıklarına](https://www.microsoft.com/en-us/download/confirmation.aspx?id=41653)ayarlanmalıdır.


#### <a name="url-patterns-for-azure-portal"></a>Azure portalı için URL desenleri

| URL deseni | Bileşen/İşlevsellik | Cihaz IP'leri |
| --- | --- | --- |
| `https://*.storsimple.windowsazure.com/*`<br>`https://*.accesscontrol.windows.net/*`<br>`https://*.servicebus.windows.net/*`<br>`https://login.windows.net` |StorSimple Device Manager hizmeti<br>Access Control Service<br>Azure Service Bus<br>Kimlik Doğrulama Hizmeti |Bulut özellikli ağ arabirimleri |
| `https://*.backup.windowsazure.com` |Cihaz kaydı |Yalnızca DATA 0 |
| `https://crl.microsoft.com/pki/*`<br>`https://www.microsoft.com/pki/*` |Sertifika iptali |Bulut özellikli ağ arabirimleri |
| `https://*.core.windows.net/*` <br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` |Azure depolama hesapları ve izleme |Bulut özellikli ağ arabirimleri |
| `https://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`https://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`https://download.microsoft.com`<br>`http://wustat.windows.com`<br>`https://ntservicepack.microsoft.com` |Microsoft Update sunucuları<br> |Denetleyici sabit IP'ler yalnızca |
| `http://*.deploy.akamaitechnologies.com` |Akamai CDN |Denetleyici sabit IP'ler yalnızca |
| `https://*.partners.extranet.microsoft.com/*`<br>`https://dcupload.microsoft.com/`<br>`https://*.support.microsoft.com/` |Destek paketi |Bulut özellikli ağ arabirimleri |

#### <a name="url-patterns-for-azure-government-portal"></a>Azure Resmi portalı için URL desenleri

| URL deseni | Bileşen/İşlevsellik | Cihaz IP'leri |
| --- | --- | --- |
| `https://*.storsimple.windowsazure.us/*`<br>`https://*.accesscontrol.usgovcloudapi.net/*`<br>`https://*.servicebus.usgovcloudapi.net/*`<br>`https://login.microsoftonline.us` |StorSimple Device Manager hizmeti<br>Access Control Service<br>Azure Service Bus<br>Kimlik Doğrulama Hizmeti |Bulut özellikli ağ arabirimleri |
| `https://*.backup.windowsazure.us` |Cihaz kaydı |Yalnızca DATA 0 |
| `https://crl.microsoft.com/pki/*`<br>`https://www.microsoft.com/pki/*` |Sertifika iptali |Bulut özellikli ağ arabirimleri |
| `https://*.core.usgovcloudapi.net/*` <br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` |Azure depolama hesapları ve izleme |Bulut özellikli ağ arabirimleri |
| `https://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`https://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`https://download.microsoft.com`<br>`http://wustat.windows.com`<br>`https://ntservicepack.microsoft.com` |Microsoft Update sunucuları<br> |Denetleyici sabit IP'ler yalnızca |
| `http://*.deploy.akamaitechnologies.com` |Akamai CDN |Denetleyici sabit IP'ler yalnızca |
| `https://*.partners.extranet.microsoft.com/*`<br>`https://dcupload.microsoft.com/`<br>`https://*.support.microsoft.com/` |Destek paketi |Bulut özellikli ağ arabirimleri |

### <a name="routing-metric"></a>Yönlendirme ölçümü

Yönlendirme ölçümü, verileri belirtilen ağlara yönlendiren arabirimler ve ağ geçidiyle ilişkilidir. Yönlendirme ölçümü, aynı hedefe birden çok yolun bulunduğunu öğrenirse, belirli bir hedefe giden en iyi yolu hesaplamak için yönlendirme protokolü tarafından kullanılır. Yönlendirme ölçütü ne kadar düşükse, tercih de o kadar yüksek.

StorSimple bağlamında, birden çok ağ arabirimi ve ağ geçidi kanal trafiğine yapılandırılırsa, arabirimlerin kullanılacağı göreli sırayı belirlemek için yönlendirme ölçümleri devreye girer. Yönlendirme ölçümleri kullanıcı tarafından değiştirilemez. Ancak, StorSimple `Get-HcsRoutingTable` cihazınızdaki yönlendirme tablosunu (ve ölçümleri) yazdırmak için cmdlet'i kullanabilirsiniz. [Sorun Giderme StorSimple dağıtım](storsimple-troubleshoot-deployment.md)Get-HcsRoutingTable cmdlet hakkında daha fazla bilgi.

Güncelleştirme 2 ve sonraki sürümler için kullanılan yönlendirme metrik algoritması aşağıdaki gibi açıklanabilir.

* Ağ arabirimlerine önceden belirlenmiş bir dizi değer atanmıştır.
* Bulut etkin olduğunda veya bulut devre dışı bırakıldığında ancak yapılandırılmış bir ağ geçidiyle çeşitli ağ arabirimlerine atanan değerlerle aşağıda gösterilen bir örnek tablo düşünün. Burada atanan değerlerin yalnızca örnek değerler olduğunu unutmayın.

    | Ağ arabirimi | Bulut özellikli | Ağ geçidi ile bulut devre dışı |
    |-----|---------------|---------------------------|
    | Veri 0  | 1            | -                        |
    | Veri 1  | 2            | 20                       |
    | Veri 2  | 3            | 30                       |
    | Veri 3  | 4            | 40                       |
    | Veri 4  | 5            | 50                       |
    | Veri 5  | 6            | 60                       |


* Bulut trafiğinin ağ arabirimleri üzerinden yönlendirilme sırası:
  
    *Veri 0 > Veri 1 > Tarih 2 > Veri 3 > Veri 4 > Veri 5*
  
    Bu aşağıdaki örnekle açıklanabilir.
  
    İki bulut özellikli ağ arabirimi olan Veri 0 ve Veri 5'e sahip bir StorSimple aygıtı düşünün. Veri 1'den Data 4'e kadar bulut devre dışı bırakılmış, ancak yapılandırılmış bir ağ geçidi vardır. Bu aygıt için trafiğin yönlendirilme sırası:
  
    *Veri 0 (1) > Veri 5 (6) > Veri 1 (20) > Veri 2 (30) > Veri 3 (40) > Veri 4 (50)*
  
    *Parantez içinde sayılar ilgili yönlendirme ölçümlerini gösterir.*
  
    Veri 0 başarısız olursa, bulut trafiği Veri 5 üzerinden yönlendirilecek. Bir ağ geçidinin diğer tüm ağda yapılandırıldığı göz önüne alındığında, hem Veri 0 hem de Veri 5 başarısız olursa, bulut trafiği Veri 1'den geçer.
* Bulut özellikli bir ağ arabirimi başarısız olursa, arabirime bağlanmak için 30 saniye gecikmeyle 3 yeniden deneme yapılır. Tüm yeniden denemeler başarısız olursa, trafik yönlendirme tablosu tarafından belirlenen bir sonraki kullanılabilir bulut etkin arabirimine yönlendirilir. Bulut özellikli tüm ağ arabirimleri başarısız olursa, aygıt diğer denetleyiciye geçmezse (bu durumda yeniden başlatma olmaz).
* iSCSI özellikli bir ağ arabirimi için VIP hatası varsa, 2 saniye gecikmeli 3 yeniden deneme olacaktır. Bu davranış, önceki sürümlerden aynı kaldı. Tüm iSCSI ağ arabirimleri başarısız olursa, bir denetleyici failover (yeniden başlatma eşliğinde) oluşur.
* VIP arızası olduğunda StorSimple cihazınızda da bir uyarı yükseltilir. Daha fazla bilgi [için, hızlı başvuru uyarısı](storsimple-8000-manage-alerts.md)gidin.
* Yeniden denemeler açısından, iSCSI buluttan önce gelecektir.
  
    Aşağıdaki örneği göz önünde bulundurun: Bir StorSimple aygıtının iki ağ arabirimi etkindir: Veri 0 ve Veri 1. Veri 0 bulut etkinken, Veri 1 hem bulut hem de iSCSI özelliklidir. Bulut veya iSCSI için bu aygıttaki başka ağ arabirimleri etkinleştirildi.
  
    Veri 1 başarısız olursa, son iSCSI ağ arabirimi olduğu göz önüne alındığında, bu bir denetleyici diğer denetleyici veri 1 için başarısız sonuçlanır.

### <a name="networking-best-practices"></a>Ağ en iyi uygulamaları

Yukarıdaki ağ gereksinimlerine ek olarak, StorSimple çözümünüzün en iyi performansı için lütfen aşağıdaki en iyi uygulamalara uyun:

* StorSimple cihazınızın her zaman kullanılabilen 40 Mbps bant genişliğine (veya daha fazlası) sahip olduğundan emin olun. Bu bant genişliği diğer uygulamalarla paylaşılmamalıdır (veya QoS ilkelerinin kullanımı yoluyla tahsisat garanti edilmelidir).
* Internet'e ağ bağlantısının her zaman kullanılabilir olduğundan emin olun. Hiçbir Internet bağlantısı da dahil olmak üzere aygıtlara düzensiz veya güvenilmez Internet bağlantıları, desteklenmeyen bir yapılandırmaya neden olur.
* iSCSI ve bulut erişimi için cihazınızda özel ağ arabirimleri oluşturarak iSCSI ve bulut trafiğini yalıtın. Daha fazla bilgi için StorSimple cihazınızdaki [ağ arabirimlerini](storsimple-8000-modify-device-config.md#modify-network-interfaces) nasıl değiştirebilirsiniz.
* Ağ arabirimleriniz için Bağlantı Toplama Denetim Protokolü (LACP) yapılandırması kullanmayın. Bu, desteklenmeyen bir yapılandırmadır.

## <a name="high-availability-requirements-for-storsimple"></a>StorSimple için yüksek kullanılabilirlik gereksinimleri

StorSimple çözümüne dahil olan donanım platformu, veri merkezinizde yüksek oranda kullanılabilir, hataya dayanıklı depolama altyapısı için temel sağlayan kullanılabilirlik ve güvenilirlik özelliklerine sahiptir. Ancak, StorSimple çözümünüzün kullanılabilirliğini sağlamaya yardımcı olmak için uymanız gereken gereksinimler ve en iyi uygulamalar vardır. StorSimple'ı dağıtmadan önce, StorSimple aygıtı ve bağlı ana bilgisayarları için aşağıdaki gereksinimleri ve en iyi uygulamaları dikkatle gözden geçirin.

StorSimple cihazınızın donanım bileşenlerinin izlenmesi ve bakımı hakkında daha fazla bilgi için donanım bileşenlerini ve durumunu ve [StorSimple donanım bileşen değiştirmesini](storsimple-8000-hardware-component-replacement.md) [izlemek için StorSimple Device Manager hizmetini kullanın.](storsimple-8000-monitor-hardware-status.md)

### <a name="high-availability-requirements-and-procedures-for-your-storsimple-device"></a>StorSimple cihazınız için yüksek kullanılabilirlik gereksinimleri ve prosedürleri

StorSimple cihazınızın yüksek kullanılabilirliğini sağlamak için aşağıdaki bilgileri dikkatle inceleyin.

#### <a name="pcms"></a>PCM'ler

StorBasit cihazlar gereksiz, sıcak değiştirilebilir güç ve soğutma modülleri (PCMs) içerir. Her PCM, tüm şasiiçin servis sağlamak için yeterli kapasiteye sahiptir. Yüksek kullanılabilirlik sağlamak için her iki PCM'nin de yüklenmesi gerekir.

* Bir güç kaynağı arızalanırsa kullanılabilirliği sağlamak için PCM'lerinizi farklı güç kaynaklarına bağlayın.
* Bir PCM başarısız olursa, hemen bir değiştirme isteyin.
* Yalnızca değiştirme niz olduğunda ve yüklemeye hazır olduğunuzda başarısız bir PCM'yi çıkarın.
* Her iki PCM'yi de aynı anda çıkarmayın. PCM modülü yedek pil modüllerini içerir. PcM'lerin her ikisinin de kaldırılması pil koruması olmadan kapanmaya neden olur ve aygıt durumu kaydedilmez. Pil hakkında daha fazla bilgi için [yedek pil modüllerini koru'ya](storsimple-8000-battery-replacement.md#maintain-the-backup-battery-module)gidin.

#### <a name="controller-modules"></a>Denetleyici modülleri

StorBasit aygıtlar gereksiz, sıcak değiştirilebilir denetleyici modülleri içerir. Denetleyici modülleri aktif/pasif bir şekilde çalışır. Herhangi bir zamanda, bir denetleyici modülü etkindir ve hizmet sunarken, diğer denetleyici modülü pasiftir. Pasif denetleyici modülü çalışır ve etkin denetleyici modülü başarısız olursa veya kaldırılırsa çalışır hale gelir. Her denetleyici modülü, tüm şasiye hizmet verecek kapasiteye sahiptir. Yüksek kullanılabilirlik sağlamak için her iki denetleyici modülü de yüklenmelidir.

* Her iki denetleyici modülün de her zaman yüklü olduğundan emin olun.
* Denetleyici modülü başarısız olursa, hemen bir değiştirme isteyin.
* Yalnızca değiştirme niz olduğunda ve yüklemeye hazır olduğunuzda başarısız bir denetleyici modüllerini çıkarın. Bir modülün uzun süre kaldırılması hava akışını ve dolayısıyla sistemin soğumasını etkileyecektir.
* Her iki denetleyici modülüne bağlı ağ bağlantılarının aynı olduğundan ve bağlı ağ arabirimlerinin aynı ağ yapılandırmasına sahip olduğundan emin olun.
* Bir denetleyici modülü başarısız olursa veya değiştirilmesi gerekiyorsa, başarısız denetleyici modüldeğiştirmeden önce diğer denetleyici modülü etkin durumda olduğundan emin olun. Denetleyicinin etkin olduğunu doğrulamak için [cihazınızdaki etkin denetleyiciyi tanımlayın'](storsimple-8000-controller-replacement.md#identify-the-active-controller-on-your-device)a gidin.
* Her iki denetleyici modüllerini de aynı anda çıkarmayın. Bir denetleyici arıza devam ediyorsa, bekleme denetleyici modüllerini kapatmayın veya şasiden çıkarmayın.
* Denetleyici başarısız olduktan sonra, her iki denetleyici modüllerini çıkarmadan önce en az beş dakika bekleyin.

#### <a name="network-interfaces"></a>Ağ arabirimleri

StorSimple cihaz denetleyici modüllerinin her birinde dört adet 1 Gigabit ve iki adet 10 Gigabit Ethernet ağ arabirimi bulunur.

* Her iki denetleyici modülüne olan ağ bağlantılarının aynı olduğundan ve denetleyici modülü arabirimlerinin aynı ağ yapılandırmasına sahip olarak bağlı olduğu ağ arabirimlerinden emin olun.
* Mümkün olduğunda, ağ aygıtı nın arızası durumunda hizmet kullanılabilirliğini sağlamak için ağ bağlantılarını farklı anahtarlar arasında dağıtın.
* Kalan tek veya son iSCSI etkin arabiriminin (IP'ler atanmış) fişini çekerken, önce arabirimi devre dışı bırakıp kabloları çıkarın. Arabirim ilk olarak çıkarılırsa, etkin denetleyicinin pasif denetleyicide başarısız olmasına neden olur. Pasif denetleyicide karşılık gelen arabirimler de varsa, her iki denetleyici de tek bir denetleyiciye yerleşmeden önce birden çok kez yeniden başlatılır.
* Her denetleyici modülünden ağa en az iki DATA arabirimi bağlayın.
* İki 10 GbE arabirimini etkinleştirdiyseniz, bunları farklı anahtarlar arasında dağıtın.
* Mümkün olduğunda, sunucuların bir bağlantı, ağ veya arabirim hatasına tolerans gösterebilmesini sağlamak için sunucularda MPIO kullanın.

Yüksek kullanılabilirlik ve performans için cihazınızı ağlandırma hakkında daha fazla bilgi için [StorSimple 8100 cihazınızı yükleyin](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) veya [StorSimple 8600 cihazınızı yükleyin.](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device)

#### <a name="ssds-and-hdds"></a>SSD'ler ve HDD'ler

StorSimple aygıtları, aynalı boşluklar kullanılarak korunan katı hal diskleri (SSD'ler) ve sabit disk sürücüleri (HDD'ler) içerir. Aynalı alanların kullanımı, aygıtın bir veya daha fazla SSD veya HDD'nin arızalanmasına tahammül edebilmesini sağlar.

* Tüm SSD ve HDD modüllerinin yüklü olduğundan emin olun.
* Bir SSD veya HDD başarısız olursa, hemen bir değiştirme isteyin.
* Bir SSD veya HDD başarısız olursa veya değiştirme gerektiriyorsa, yalnızca değiştirilmesi gereken SSD veya HDD'yi kaldırdığınızdan emin olun.
* Herhangi bir anda birden fazla SSD veya HDD'yi sistemden çıkarmayın.
  Belirli tipte (HDD, SSD) 2 veya daha fazla diskin kısa bir süre içinde arızalanması veya ardışık bir arıza sistem arızalarına ve olası veri kaybına neden olabilir. Bu durumda, yardım için [Microsoft Destek'e başvurun.](storsimple-8000-contact-microsoft-support.md)
* Değiştirme sırasında, SSD'lerde ve HDD'lerde sürücüler için **Donanım sağlık** bıçaklarında **Paylaşılan bileşenleri** izleyin. Yeşil denetim durumu disklerin sağlıklı veya tamam olduğunu gösterirken, kırmızı ünlem işareti başarısız bir SSD veya HDD'yi gösterir.
* Bir sistem hatası durumunda korumanız gereken tüm birimler için bulut anlık görüntülerini yapılandırmanızı öneririz.

#### <a name="ebod-enclosure"></a>EBOD muhafazası

StorSimple cihaz modeli 8600, birincil kasaya ek olarak Genişletilmiş Disk Demeti (EBOD) muhafazası içerir. EBOD, aynalı boşluklar kullanılarak korunan EBOD denetleyicileri ve sabit disk sürücüleri (HDD' ler) içerir. Aynalı boşlukların kullanılması, aygıtın bir veya daha fazla HDD'nin arızalanmasına tahammül edebilmesini sağlar. EBOD kasası, yedek SAS kabloları ile birincil kasaya bağlanır.

* Hem EBOD muhafaza denetleyici modüllerinin, hem SAS kablolarının hem de tüm sabit disk sürücülerinin her zaman takılı olduğundan emin olun.
* EBOD muhafaza denetleyici modülü başarısız olursa, derhal bir değiştirme isteyin.
* EBOD muhafaza denetleyici modülü başarısız olursa, başarısız modülü değiştirmeden önce diğer denetleyici modülünün etkin olduğundan emin olun. Denetleyicinin etkin olduğunu doğrulamak için [cihazınızdaki etkin denetleyiciyi tanımlayın'](storsimple-8000-controller-replacement.md#identify-the-active-controller-on-your-device)a gidin.
* EBOD denetleyici modülü değişimi sırasında, **Monitör** > **Donanımı durumuna**erişerek StorSimple Device Manager hizmetindeki bileşenin durumunu sürekli olarak izleyin.
* Bir SAS kablosu arızalanırsa veya değiştirilmesi gerekiyorsa (böyle bir belirleme yapmak için Microsoft Desteği dahil edilmelidir), yalnızca değiştirilmesi gereken SAS kablosunu kaldırdığınızdan emin olun.
* Aynı anda her iki SAS kablosunu da herhangi bir zamanda sistemden çıkarmayın.

### <a name="high-availability-recommendations-for-your-host-computers"></a>Ana bilgisayarlarınız için yüksek kullanılabilirlik önerileri

StorSimple cihazınıza bağlı ana bilgisayarların yüksek kullanılabilirliğini sağlamak için bu en iyi uygulamaları dikkatle inceleyin.

* StorSimple'ı [iki düğümlü dosya sunucusu küme yapılandırmalarıyla][1]yapılandırın. Tek hata noktalarını kaldırarak ve ev sahibi tarafta artıklık la inşa ederek, tüm çözüm son derece kullanılabilir hale gelir.
* Depolama denetleyicilerinin üzerinde başarısız lık sırasında yüksek kullanılabilirlik için Windows Server 2012 (SMB 3.0) ile kullanılabilir sürekli kullanılabilir (CA) paylaşımları kullanın. Dosya sunucusu kümelerini yapılandırmak ve Windows Server 2012 ile sürekli kullanılabilir paylaşımlar yapılandırmak için ek bilgi için bu [video demosu'na](https://channel9.msdn.com/Events/IT-Camps/IT-Camps-On-Demand-Windows-Server-2012/DEMO-Continuously-Available-File-Shares)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [StorSimple sistem limitleri hakkında bilgi edinin.](storsimple-8000-limits.md)
* [StorSimple çözümünüzü nasıl dağıtılayınızı öğrenin.](storsimple-8000-deployment-walkthrough-u2.md)

<!--Reference links-->
[1]: https://technet.microsoft.com/library/cc731844(v=WS.10).aspx
