---
title: Microsoft Azure StorBasit Sanal Dizi sistem gereksinimleri
description: StorSimple Virtual Array'iniz için yazılım ve ağ gereksinimleri hakkında bilgi edinin
author: alkohli
ms.assetid: ea1d3bca-e71b-453d-aa82-440d2638f5e3
ms.service: storsimple
ms.topic: conceptual
ms.date: 07/25/2019
ms.author: alkohli
ms.openlocfilehash: 020208a8b67d248c02fc659d4dc48fa22d333839
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80298817"
---
# <a name="storsimple-virtual-array-system-requirements"></a>StorSimple Sanal Dizini sistem gereksinimleri

[!INCLUDE [storsimple-virtual-array-eol-banner](../../includes/storsimple-virtual-array-eol-banner.md)]

## <a name="overview"></a>Genel Bakış

Bu makalede, Microsoft Azure StorBasit Sanal Diziniz ve diziye erişen depolama istemcileri için önemli sistem gereksinimleri açıklanmaktadır. StorSimple sisteminizi dağıtmadan önce bilgileri dikkatle gözden geçirmenizi ve dağıtım ve sonraki işlem sırasında gerekli olduğu şekilde geri başvurmanızı öneririz.

Sistem gereksinimleri şunlardır:

* **Depolama istemcileri için yazılım gereksinimleri** - desteklenen sanallaştırma platformlarını, web tarayıcılarını, iSCSI başlatıcılarını, Kobİ istemcilerini, minimum sanal cihaz gereksinimlerini ve bu işletim sistemleri için ek gereksinimleri açıklar.
* **StorSimple aygıtıiçin ağ gereksinimleri** - iSCSI, bulut veya yönetim trafiğine izin vermek için güvenlik duvarınızda açık olması gereken bağlantı noktaları hakkında bilgi sağlar.

Bu makalede yayınlanan StorSimple sistem gereksinimleri bilgileri yalnızca StorSimple Virtual Arrays için geçerlidir.

* 8000 serisi cihazlar [için StorSimple 8000 serisi cihazınız için Sistem gereksinimlerine](storsimple-system-requirements.md)gidin.
* 7000 serisi cihazlar [için StorSimple 5000-7000 serisi cihazınız için Sistem gereksinimlerine](http://onlinehelp.storsimple.com/1_StorSimple_System_Requirements)gidin.

## <a name="software-requirements"></a>Yazılım gereksinimleri
Yazılım gereksinimleri desteklenen web tarayıcıları, Kobİ sürümleri, sanallaştırma platformları ve minimum sanal cihaz gereksinimleri hakkında bilgi içerir.

### <a name="supported-virtualization-platforms"></a>Desteklenen sanallaştırma platformları
| **Hiper Yönetici** | **Sürüm** |
| --- | --- |
| Hyper-V |Windows Server 2008 R2 SP1 ve sonrası |
| VMware ESXi |5.0, 5.5, 6.0 ve 6.5. |

> [!IMPORTANT]
> StorSimple Virtual Array'inize VMware araçları yüklemeyin; bu, desteklenmeyen bir yapılandırmaya neden olur.

### <a name="virtual-device-requirements"></a>Sanal cihaz gereksinimleri
| **Bileşen** | **Gereksinim** |
| --- | --- |
| Minimum sanal işlemci sayısı (çekirdek) |4 |
| Minimum bellek (RAM) |8 GB <br> Bir dosya sunucusu için, 2 milyondan az dosya için 8 GB ve 2 - 4 milyon dosya için 16 GB|
| Disk alanı<sup>1</sup> |İşletim sistemi diski - 80 GB <br></br>Veri diski - 500 GB - 8 TB |
| Minimum ağ arabirimi(ler) sayısı |1 |
| İnternet bant genişliği<sup>2</sup> |Minimum bant genişliği gereklidir: 5 Mbps <br> Önerilen bant genişliği: 100 Mbps <br> Veri aktarım hızı Internet bant genişliği ile ölçekler. Örneğin, günlük yedeklemeler bir günde tamamlanmadığından, 100 GB verinin 5 Mbps hızında aktarılması 2 gün sürer ve bu da yedekleme hatalarına yol açabilir. 100 Mbps bant genişliği ile 100 GB veri 2,5 saat içinde aktarılabilir.   |

<sup>1</sup> - İnce temin

<sup>2</sup> - Ağ gereksinimleri günlük veri değişim hızına bağlı olarak değişebilir. Örneğin, bir aygıtın bir gün içinde 10 GB veya daha fazla değişikliği yedeklemesi gerekiyorsa, 5 Mbps bağlantı üzerindeki günlük yedekleme 4,25 saate kadar sürebilir (veriler sıkıştırılamadıysa veya çoğaltılamadıysa).

### <a name="supported-web-browsers"></a>Desteklenen web tarayıcıları
| **Bileşen** | **Sürüm** | **Ek gereksinimler/notlar** |
| --- | --- | --- |
| Microsoft Edge |En son sürüm | |
| Internet Explorer |En son sürüm |Internet Explorer 11 ile test edilmiştir |
| Google Chrome |En son sürüm |Chrome 46 ile test edilmiştir |

### <a name="supported-storage-clients"></a>Desteklenen depolama istemcileri
Aşağıdaki yazılım gereksinimleri, StorSimple Virtual Array'inize (iSCSI sunucusu olarak yapılandırılan) erişen iSCSI başlatıcıları içindir.

| **Desteklenen işletim sistemleri** | **Sürüm gerekli** | **Ek gereksinimler/notlar** |
| --- | --- | --- |
| Windows Server |2008R2 SP1, 2012, 2012R2 |StorSimple ince ve tam olarak sağlanan hacimler oluşturabilirsiniz. Kısmen sağlanan hacimler oluşturamaz. StorSimple iSCSI birimleri yalnızca: <ul><li>Windows temel disklerde basit birimler.</li><li>Bir birimi biçimlendirmek için Windows NTFS.</li> |

Aşağıdaki yazılım gereksinimleri, StorSimple Virtual Array'inize (dosya sunucusu olarak yapılandırılan) erişen Kobİ istemcileri içindir.

| **SMB Sürümü** |
| --- |
| Kobİ 2.x |
| SMB 3.0 |
| Kobİ 3.02 |

> [!IMPORTANT]
> Windows Şifreleme Dosya Sistemi (EFS) tarafından korunan dosyaları StorSimple Virtual Array dosya sunucusuna kopyalamayın veya saklamayın; bu, desteklenmeyen bir yapılandırmaya neden olur.


### <a name="supported-storage-format"></a>Desteklenen depolama biçimi
Yalnızca Azure bloğu blob depolama sı desteklenir. Sayfa lekeleri desteklenmez. Blok [lekeleri ve sayfa lekeleri hakkında](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs)daha fazla bilgi .

## <a name="networking-requirements"></a>Ağ gereksinimleri
Aşağıdaki tabloda, iSCSI, SMB, bulut veya yönetim trafiğine izin vermek için güvenlik duvarınızda açılması gereken bağlantı noktaları listelenildi. Bu tabloda, *gelen* *istemci* isteklerinin cihazınıza eriştikleri yönü ifade eder. *Giden* veya *giden,* StorSimple cihazınızın verileri dağıtımın ötesinde harici olarak gönderdiği yönü ifade eder: örneğin, Internet'e giden.

| **Bağlantı Noktası No:<sup>1</sup>** | **Veya dışarıda** | **Bağlantı noktası kapsamı** | **Gerekli** | **Notlar** |
| --- | --- | --- | --- | --- |
| TCP 80 (HTTP) |Out |WAN |Hayır |Giden bağlantı noktası güncelleştirmeleri almak için Internet erişimi için kullanılır. <br></br>Giden web proxy kullanıcı yapılandırılabilir. |
| TCP 443 (HTTPS) |Out |WAN |Evet |Giden bağlantı noktası buluttaki verilere erişmek için kullanılır. <br></br>Giden web proxy kullanıcı yapılandırılabilir. |
| UDP 53 (DNS) |Out |WAN |Bazı durumlarda; notlara bakın. |Bu bağlantı noktası yalnızca Internet tabanlı bir DNS sunucusu kullanıyorsanız gereklidir. <br></br> Bir dosya sunucusu dağıtıyorsanız, yerel DNS sunucusu kullanmanızı öneririz. |
| UDP 123 (NTP) |Out |WAN |Bazı durumlarda; notlara bakın. |Bu bağlantı noktası yalnızca Internet tabanlı bir NTP sunucusu kullanıyorsanız gereklidir.<br></br> Bir dosya sunucusu dağıtıyorsanız, Active Directory etki alanı denetleyicilerinizle zaman eşitlemenizi öneririz. |
| TCP 80 (HTTP) |İçindeki |LAN |Evet |Bu, yerel yönetim için StorSimple aygıtında yerel Kullanıcı Arabirimi için gelen bağlantı noktasıdır. <br></br> YEREL UI'ye HTTP üzerinden erişmenin otomatik olarak HTTPS'ye yönlendireceğini unutmayın. |
| TCP 443 (HTTPS) |İçindeki |LAN |Evet |Bu, yerel yönetim için StorSimple aygıtında yerel Kullanıcı Arabirimi için gelen bağlantı noktasıdır. |
| TCP 3260 (iSCSI) |İçindeki |LAN |Hayır |Bu bağlantı noktası iSCSI üzerinden verilere erişmek için kullanılır. |

<sup>1</sup> Gelen bağlantı noktalarının genel Internet'te açılması gerekmez.

> [!IMPORTANT]
> Güvenlik duvarının StorSimple aygıtı ile Azure arasındaki TLS trafiğini değiştirmediğinden veya şifresini çözmediğinden emin olun.
> 
> 

### <a name="url-patterns-for-firewall-rules"></a>Güvenlik duvarı kuralları için URL desenleri
Ağ yöneticileri genellikle gelen ve giden trafiği filtrelemek için URL desenlerini temel alan gelişmiş güvenlik duvarı kurallarını yapılandırabilir. Sanal diziniz ve StorSimple Aygıt Yöneticisi hizmetiniz Azure Hizmet Veri Yolundaki Diğer Microsoft uygulamalarına, Azure Active Directory Access Control,depolama hesapları ve Microsoft Update sunucularına bağlıdır. Bu uygulamalarla ilişkili URL desenleri güvenlik duvarı kurallarını yapılandırmak için kullanılabilir. Bu uygulamalarla ilişkili URL desenlerinin değişebileceğini anlamak önemlidir. Bu da ağ yöneticisinin StorSimple'ınız için güvenlik duvarı kurallarını gerektiği gibi ve gerektiğinde izlemesi ve güncelleştirmesini gerektirir. 

Çoğu durumda, StorSimple sabit IP adreslerine göre, giden trafik için güvenlik duvarı kurallarınızı belirlemenizi öneririz. Ancak, güvenli ortamlar oluşturmak için gereken gelişmiş güvenlik duvarı kuralları ayarlamak için aşağıdaki bilgileri kullanabilirsiniz.

> [!NOTE]
> 
> * Aygıt (kaynak) IP'leri her zaman bulut özellikli tüm ağ arabirimlerine ayarlanmalıdır. 
> * Hedef IP'ler Azure [veri merkezi IP aralıklarına](https://www.microsoft.com/download/confirmation.aspx?id=41653)ayarlanmalıdır.
> 
> 

| URL deseni | Bileşen/İşlevsellik |
| --- | --- |
| `https://*.storsimple.windowsazure.com/*`<br>`https://*.accesscontrol.windows.net/*`<br>`https://*.servicebus.windows.net/*` <br>`https://login.windows.net`|StorSimple Device Manager hizmeti<br>Access Control Service<br>Azure Service Bus<br>Kimlik Doğrulama Hizmeti|
| `http://*.backup.windowsazure.com` |Cihaz kaydı |
| `https://crl.microsoft.com/pki/*`<br>`https://www.microsoft.com/pki/*` |Sertifika iptali |
| `https://*.core.windows.net/*`<br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` |Azure depolama hesapları ve izleme |
| `https://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`https://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`https://download.microsoft.com`<br>`http://wustat.windows.com`<br>`https://ntservicepack.microsoft.com` |Microsoft Update sunucuları<br> |
| `http://*.deploy.akamaitechnologies.com` |Akamai CDN |
| `https://*.partners.extranet.microsoft.com/*` |Destek paketi |
| `https://*.data.microsoft.com` |Windows'da Telemetri hizmeti, [müşteri deneyimi ve tanıt telemetri sunuyoruz](https://support.microsoft.com/en-us/kb/3068708) bakın |

## <a name="next-steps"></a>Sonraki adımlar
* [StorSimple Virtual Array'inizi dağıtmak için portalı hazırlayın](storsimple-virtual-array-deploy1-portal-prep.md)
