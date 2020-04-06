---
title: Azure NetApp Dosyaları Hakkında SSS | Microsoft Dokümanlar
description: Azure NetApp Dosyaları hakkında sık sorulan soruları yanıtlar.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/03/2020
ms.author: b-juche
ms.openlocfilehash: aebc669a90511e48ddd2a7876553948c04b97710
ms.sourcegitcommit: 67addb783644bafce5713e3ed10b7599a1d5c151
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2020
ms.locfileid: "80667822"
---
# <a name="faqs-about-azure-netapp-files"></a>Azure NetApp Dosyaları Hakkında SSS

Bu makalede, Azure NetApp Dosyaları hakkında sık sorulan sorular (SSS) yanıtlanmaktadır. 

## <a name="networking-faqs"></a>Ağ SSS'leri

### <a name="does-the-nfs-data-path-go-over-the-internet"></a>NFS veri yolu Internet üzerinden mi gidiyor?  

Hayır. NFS veri yolu Internet üzerinden gitmez. Azure NetApp Files, hizmetin kullanılabildiği Azure Sanal Ağı'na (VNet) dağıtılan bir Azure yerel hizmetidir. Azure NetApp Files, yetkin bir alt ağ kullanır ve doğrudan VNet'te bir ağ arabirimi sağlar. 

Ayrıntılar [için Azure NetApp Dosyaları ağ planlaması yönergelerine](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-network-topologies) bakın.  

### <a name="can-i-connect-a-vnet-that-i-already-created-to-the-azure-netapp-files-service"></a>Daha önce oluşturduğum bir VNet'i Azure NetApp Files hizmetine bağlayabilir miyim?

Evet, oluşturduğunuz VNet'leri hizmete bağlayabilirsiniz. 

Ayrıntılar [için Azure NetApp Dosyaları ağ planlaması yönergelerine](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-network-topologies) bakın.  

### <a name="can-i-mount-an-nfs-volume-of-azure-netapp-files-using-dns-fqdn-name"></a>DNS FQDN adını kullanarak Azure NetApp Dosyalarının NFS hacmini takabilir miyim?

Evet, gerekli DNS girişlerini oluşturursanız yapabilirsiniz. Azure NetApp Files, sağlanan birim için hizmet IP'sini sağlar. 

> [!NOTE] 
> Azure NetApp Files, hizmet için gerektiğinde ek IP'ler dağıtabilir.  DNS girişlerinin düzenli aralıklarla güncellenmesi gerekebilir.

## <a name="security-faqs"></a>Güvenlik SSS'leri

### <a name="can-the-network-traffic-between-the-azure-vm-and-the-storage-be-encrypted"></a>Azure VM ile depolama alanı arasındaki ağ trafiği şifrelenebilir mi?

Veri trafiği (NFSv3, NFSv4.1 veya SMBv3 istemcisinden Azure NetApp Dosyaları birimlerine gelen trafik) şifrelenmez. Ancak, bir Azure VM'den (NFS veya SMB istemcisi çalıştıran) Azure NetApp Dosyalarına olan trafik, diğer Azure-VM'den VM'ye kadar güvenlidir. Bu trafik Azure veri merkezi ağına yereldir. 

### <a name="can-the-storage-be-encrypted-at-rest"></a>Depolama istirahatte şifrelenebilir mi?

Tüm Azure NetApp Dosyaları birimleri FIPS 140-2 standardı kullanılarak şifrelenir. Tüm anahtarlar Azure NetApp Files hizmeti tarafından yönetilir. 

### <a name="how-are-encryption-keys-managed"></a>Şifreleme anahtarları nasıl yönetilir? 

Azure NetApp Files için anahtar yönetimi hizmet tarafından yürütilir. Her birim için benzersiz bir XTS-AES-256 veri şifreleme anahtarı oluşturulur. Şifreleme anahtarı hiyerarşisi, tüm birim anahtarlarını şifrelemek ve korumak için kullanılır. Bu şifreleme anahtarları hiçbir zaman şifrelenmemiş bir biçimde görüntülenmez veya raporlmaz. Birim silindiğinde şifreleme anahtarları hemen silinir.

Azure Özel HSM kullanarak kullanıcı tarafından yönetilen anahtarlar (Kendi Anahtarlarınızı Getir) desteği, ABD Doğu, ABD West2 ve ABD Güney Orta bölgelerinde kontrollü olarak kullanılabilir.  Erişim isteğinde **anffeedback@microsoft.com**bulunabilirsiniz. Kapasite kullanılabilir olduğundan, istekler onaylanacaktır.

### <a name="can-i-configure-the-nfs-export-policy-rules-to-control-access-to-the-azure-netapp-files-service-mount-target"></a>Azure NetApp Files hizmet montaj hedefine erişimi denetlemek için NFS dışa aktarma ilkesi kurallarını yapılandırabilir miyim?


Evet, tek bir NFS dışa aktarma ilkesinde en fazla beş kural yapılandırabilirsiniz.

### <a name="does-azure-netapp-files-support-network-security-groups"></a>Azure NetApp Dosyaları Ağ Güvenlik Gruplarını destekliyor mu?

Hayır, şu anda Ağ Güvenlik Grupları'nı Azure NetApp Dosyaları'nın devredilen alt ağına veya hizmet tarafından oluşturulan ağ arabirimlerine uygulayamazsınız.

### <a name="can-i-use-azure-iam-with-azure-netapp-files"></a>Azure NetApp Dosyaları ile Azure IAM'yi kullanabilir miyim?

Evet, Azure NetApp Files, Azure IAM ile RBAC özelliklerini destekler.

## <a name="performance-faqs"></a>Performans SSS'leri

### <a name="what-should-i-do-to-optimize-or-tune-azure-netapp-files-performance"></a>Azure NetApp Dosyaları performansını optimize etmek veya ayarlamak için ne yapmalıyım?

Performans gereksinimlerine göre aşağıdaki eylemleri yapabilirsiniz: 
- Sanal Makine'nin uygun şekilde boyutlandırDığından emin olun.
- VM için Hızlandırılmış Ağ'ı etkinleştirin.
- Kapasite havuzu için istenen servis düzeyini ve boyutunu seçin.
- Kapasite ve performans için istenen kota boyutuna sahip bir hacim oluşturun.

### <a name="how-do-i-convert-throughput-based-service-levels-of-azure-netapp-files-to-iops"></a>Azure NetApp Dosyalarının iş bölümü ne reistabanlı hizmet düzeylerini IOPS'ye nasıl dönüştürebilirim?

Aşağıdaki formülü kullanarak MB/s'yi IOPS'ye dönüştürebilirsiniz:  

`IOPS = (MBps Throughput / KB per IO) * 1024`

### <a name="how-do-i-change-the-service-level-of-a-volume"></a>Bir birimin hizmet düzeyini nasıl değiştirebilirim?

Bir birimin hizmet düzeyini değiştirme şu anda desteklenmiyor.

### <a name="how-do-i-monitor-azure-netapp-files-performance"></a>Azure NetApp Dosyaları performansını nasıl izleyebilirim?

Azure NetApp Files, toplu performans ölçümleri sağlar. Azure NetApp Dosyaları için kullanım ölçümlerini izlemek için Azure Monitor'u da kullanabilirsiniz.  Azure NetApp Dosyaları için performans ölçümleri listesi için [Azure NetApp Dosyaları](azure-netapp-files-metrics.md) ölçümleri bölümüne bakın.

## <a name="nfs-faqs"></a>NFS Sıkça Sorulan Soru

### <a name="i-want-to-have-a-volume-mounted-automatically-when-an-azure-vm-is-started-or-rebooted--how-do-i-configure-my-host-for-persistent-nfs-volumes"></a>Azure VM başlatıldığında veya yeniden başlatıldığında bir birimin otomatik olarak monte edilmesi ni istiyorum.  Kalıcı NFS birimleri için ana bilgisayarımı nasıl yapılandırıyorum?

Bir NFS biriminin VM başlangıcında veya yeniden başlatında otomatik `/etc/fstab` olarak monte edilebis olması için, ana bilgisayardaki dosyaya bir giriş ekleyin. 

Ayrıntılar [için Windows veya Linux sanal makineleri için bir ses düzeyini Mount veya sökme](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md) ye bakın.  

### <a name="why-does-the-df-command-on-nfs-client-not-show-the-provisioned-volume-size"></a>NFS istemcisindeki DF komutu neden verilen birim boyutunu göstermiyor?

DF'de bildirilen ses düzeyi, Azure NetApp Dosyaları biriminin büyüyebileceği maksimum boyutdur. DF komutundaki Azure NetApp Dosyaları biriminin boyutu, birimin kotasını veya boyutunu yansıtmaz.  Azure NetApp Dosyaları ses boyutunu veya kotasını Azure portalı veya API üzerinden alabilirsiniz.

### <a name="what-nfs-version-does-azure-netapp-files-support"></a>Azure NetApp Dosyaları hangi NFS sürümünü destekliyor?

Azure NetApp Files, NFSv3 ve NFSv4.1'i destekler. NFS sürümünü kullanarak [bir birim oluşturabilirsiniz.](azure-netapp-files-create-volumes.md) 

### <a name="how-do-i-enable-root-squashing"></a>Kök ezmeyi nasıl etkinleştirebilirim?

Kök ezme şu anda desteklenmez.

## <a name="smb-faqs"></a>Kobİ Sıkça Sorulan Soru

### <a name="is-an-active-directory-connection-required-for-smb-access"></a>Kobİ erişimi için Active Directory bağlantısı gerekli midir? 

Evet, Bir Kobİ birimi dağıtmadan önce etkin dizin bağlantısı oluşturmanız gerekir. Belirtilen Etki Alanı Denetleyicilerine başarılı bir bağlantı için Azure NetApp Dosyaları'nın devredilen alt ağı tarafından erişilebilmelidir.  Ayrıntılar için [Bir SMB birimi oluşturma](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-create-volumes-smb) bilgisine bakın. 

### <a name="how-many-active-directory-connections-are-supported"></a>Kaç Tane Active Directory bağlantısı desteklenir?

Azure NetApp Files, AD bağlantıları farklı NetApp hesaplarında olsa bile, tek bir *bölgede*birden fazla Active Directory (AD) bağlantısını desteklemez. Ancak, AD bağlantıları farklı bölgelerde olduğu sürece, tek bir *abonelikte*birden çok AD bağlantınız olabilir. Tek bir bölgede birden çok AD bağlantısına ihtiyacınız varsa, bunu yapmak için ayrı abonelikler kullanabilirsiniz. 

NetApp hesabı başına bir AD bağlantısı yapılandırılır; AD bağlantısı yalnızca oluşturulduğu NetApp hesabı üzerinden görülebilir.

### <a name="does-azure-netapp-files-support-azure-active-directory"></a>Azure NetApp Dosyaları Azure Active Directory'yi destekliyor mu? 

Hem [Azure Etkin Dizin (AD) Etki Alanı Hizmetleri](https://docs.microsoft.com/azure/active-directory-domain-services/overview) hem de Active [Directory Domain Services (AD DS)](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview) desteklenir. Azure NetApp Dosyaları ile varolan Active Directory etki alanı denetleyicilerini kullanabilirsiniz. Etki alanı denetleyicileri Azure'da sanal makine olarak veya ExpressRoute veya S2S VPN aracılığıyla şirket içinde bulunabilir. Azure NetApp Files, şu anda [Azure Active Directory](https://azure.microsoft.com/resources/videos/azure-active-directory-overview/) için AD join'i desteklememektedir.

Azure Active Directory Etki Alanı Hizmetleri ile Azure NetApp Dosyalarını kullanıyorsanız, kuruluş birimi yolu NetApp hesabınız için Active Directory'yi yapılandırdığınızda dır. `OU=AADDC Computers`

### <a name="what-versions-of-windows-server-active-directory-are-supported"></a>Windows Server Active Directory'nin hangi sürümleri desteklenir?

Azure NetApp Files, Active Directory Domain Services'ın Windows Server 2008r2SP1-2019 sürümlerini destekler.

### <a name="why-does-the-available-space-on-my-smb-client-not-show-the-provisioned-size"></a>Kobİ istemcimdeki kullanılabilir alan neden verilen boyutu göstermiyor?

Kobİ istemcisi tarafından bildirilen birim boyutu, Azure NetApp Dosyaları biriminin büyüyebileceği maksimum boyutdur. SMB istemcide gösterildiği gibi Azure NetApp Dosyaları biriminin boyutu, birimin kotasını veya boyutunu yansıtmaz. Azure NetApp Dosyaları ses boyutunu veya kotasını Azure portalı veya API üzerinden alabilirsiniz.

<!--
### Does Azure NetApp Files support Kerberos encryption?

Yes, by default, Azure NetApp Files supports both AES-128 and AES-256 encryption for traffic between the service and the targeted Active Directory domain controllers. See [Create an SMB volume for Azure NetApp Files](azure-netapp-files-create-volumes-smb.md) for requirements. 
-->

<!--
### Does Azure NetApp Files support LDAP signing? 

Yes, Azure NetApp Files supports LDAP signing by default. This functionality enables secure LDAP lookups between the Azure NetApp Files service and the user-specified [Active Directory Domain Services domain controllers](https://docs.microsoft.com/windows/win32/ad/active-directory-domain-services). For more information, see [ADV190023 | Microsoft Guidance for Enabling LDAP Channel Binding and LDAP Signing](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/ADV190023).
--> 

## <a name="capacity-management-faqs"></a>Kapasite yönetimi SSS'leri

### <a name="how-do-i-monitor-usage-for-capacity-pool-and-volume-of-azure-netapp-files"></a>Azure NetApp Dosyalarının kapasite havuzu ve hacmi için kullanımı nasıl izleyebilirim? 

Azure NetApp Files kapasite havuzu ve birim kullanım ölçümleri sağlar. Azure NetApp Dosyaları'nın kullanımını izlemek için Azure Monitor'u da kullanabilirsiniz. Ayrıntılar [için Azure NetApp Dosyaları](azure-netapp-files-metrics.md) ölçümleri'ne bakın. 

### <a name="can-i-manage-azure-netapp-files-through-azure-storage-explorer"></a>Azure NetApp Dosyalarını Azure Depolama Gezgini üzerinden yönetebilir miyim?

Hayır. Azure NetApp Dosyaları Azure Depolama Gezgini tarafından desteklenmez.

### <a name="how-do-i-determine-if-a-directory-is-approaching-the-limit-size"></a>Bir dizinin sınır boyutuna yaklaşıp yaklaşmadığını nasıl belirleyebilirim?

Bir dizin `stat` maksimum boyut sınırına (320 MB) yaklaşıp yaklaşmadığını görmek için istemcinin komutunu kullanabilirsiniz.

320 MB dizin için blok sayısı 655360'dır ve her blok boyutu 512 bayttır.  (Yani, 320x1024x1024/512.)  

Örnekler:

    [makam@cycrh6rtp07 ~]$ stat bin
    File: 'bin'
    Size: 4096            Blocks: 8          IO Block: 65536  directory

    [makam@cycrh6rtp07 ~]$ stat tmp
    File: 'tmp'
    Size: 12288           Blocks: 24         IO Block: 65536  directory
 
    [makam@cycrh6rtp07 ~]$ stat tmp1
    File: 'tmp1'
    Size: 4096            Blocks: 8          IO Block: 65536  directory

## <a name="data-migration-and-protection-faqs"></a>Veri geçişi ve koruma SSS'leri

### <a name="how-do-i-migrate-data-to-azure-netapp-files"></a>Verileri Azure NetApp Dosyalarına nasıl aktarıyorum?
Azure NetApp Files NFS ve Kobİ birimleri sağlar.  Verileri hizmete geçirmek için dosya tabanlı herhangi bir kopyalama aracı kullanabilirsiniz. 

NetApp, SaaS tabanlı bir çözüm, [NetApp Cloud Sync](https://cloud.netapp.com/cloud-sync-service)sunuyor.  Çözüm, NFS veya Kobİ verilerini Azure NetApp Files NFS dışa aktarır veya Kobİ paylaşımlarına çoğaltmanızı sağlar. 

Verileri kopyalamak için çok çeşitli boş araçlar da kullanabilirsiniz. NFS için, kaynak verileri bir Azure NetApp Files birimine kopyalamak ve senkronize etmek için [rsync](https://rsync.samba.org/examples.html) gibi iş yükleri araçlarını kullanabilirsiniz. Kobİ için iş yüklerini [robocopy'yi](https://docs.microsoft.com/windows-server/administration/windows-commands/robocopy) aynı şekilde kullanabilirsiniz.  Bu araçlar, dosya veya klasör izinlerini de çoğaltabilir. 

Şirket içinde Azure NetApp Dosyalarına veri geçişi için gereksinimler aşağıdaki gibidir: 

- Azure NetApp Dosyalarının hedef Azure bölgesinde kullanılabilir olduğundan emin olun.
- Kaynak ve Azure NetApp Dosyaları hedef hacmi IP adresi arasındaki ağ bağlantısını doğrulayın. Şirket içi ve Azure NetApp Files hizmeti arasında veri aktarımı ExpressRoute üzerinden desteklenir.
- Hedef Azure NetApp Dosyaları hacmini oluşturun.
- Tercih ettiğiniz dosya kopyalama aracını kullanarak kaynak verilerini hedef hacmine aktarın.

### <a name="how-do-i-create-a-copy-of-an-azure-netapp-files-volume-in-another-azure-region"></a>Başka bir Azure bölgesinde bir Azure NetApp Dosyaları biriminin kopyasını nasıl oluştururum?
    
Azure NetApp Files NFS ve Kobİ birimleri sağlar.  Herhangi bir dosya tabanlı kopyalama aracı, Azure bölgeleri arasındaki verileri çoğaltmak için kullanılabilir. 

NetApp, SaaS tabanlı bir çözüm, [NetApp Cloud Sync](https://cloud.netapp.com/cloud-sync-service)sunuyor.  Çözüm, NFS veya Kobİ verilerini Azure NetApp Files NFS dışa aktarır veya Kobİ paylaşımlarına çoğaltmanızı sağlar. 

Verileri kopyalamak için çok çeşitli boş araçlar da kullanabilirsiniz. NFS için, kaynak verileri bir Azure NetApp Files birimine kopyalamak ve senkronize etmek için [rsync](https://rsync.samba.org/examples.html) gibi iş yükleri araçlarını kullanabilirsiniz. Kobİ için iş yüklerini [robocopy'yi](https://docs.microsoft.com/windows-server/administration/windows-commands/robocopy) aynı şekilde kullanabilirsiniz.  Bu araçlar, dosya veya klasör izinlerini de çoğaltabilir. 

Azure NetApp Dosyaları hacmini başka bir Azure bölgesine çoğaltma gereksinimleri aşağıdaki gibidir: 
- Azure NetApp Dosyalarının hedef Azure bölgesinde kullanılabilir olduğundan emin olun.
- Her bölgedeki VNets arasındaki ağ bağlantısını doğrulayın. Şu anda, VNets arasında küresel bakış desteklenmez.  Bir ExpressRoute devresi ile bağlantı kurarak veya Bir S2S VPN bağlantısı kullanarak VNets arasında bağlantı kurabilirsiniz. 
- Hedef Azure NetApp Dosyaları hacmini oluşturun.
- Tercih ettiğiniz dosya kopyalama aracını kullanarak kaynak verilerini hedef hacmine aktarın.

### <a name="is-migration-with-azure-data-box-supported"></a>Azure Veri Kutusu ile geçiş desteklenen mi?

Hayır. Azure Veri Kutusu şu anda Azure NetApp Dosyalarını desteklememektedir. 

### <a name="is-migration-with-azure-importexport-service-supported"></a>Azure İçe Aktarma/Dışa Aktarma hizmetiyle geçiş desteklendi mi?

Hayır. Azure İçe Alma/Dışa Aktarma hizmeti şu anda Azure NetApp Dosyalarını desteklememektedir.

## <a name="next-steps"></a>Sonraki adımlar  

- [Microsoft Azure ExpressRoute SSS](https://docs.microsoft.com/azure/expressroute/expressroute-faqs)
- [Microsoft Azure Sanal Ağ SSS](https://docs.microsoft.com/azure/virtual-network/virtual-networks-faq)
- [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)
- [Azure Data Box](https://docs.microsoft.com/azure/databox-family/)
- [Azure NetApp Dosyaları için Kobİ performansı hakkında SSS'ler](azure-netapp-files-smb-performance.md)
