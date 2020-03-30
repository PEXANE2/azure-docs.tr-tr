---
title: Azure NetApp Dosyaları (SMB) ile Windows'da SAP NW için Azure VM'ler HA| Microsoft Dokümanlar
description: SAP uygulamaları için Azure NetApp Dosyaları (SMB) ile Windows'daki Azure VM'lerde SAP NetWeaver için yüksek kullanılabilirlik
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: rdeltcheva
manager: juergent
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 5e514964-c907-4324-b659-16dd825f6f87
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 10/29/2019
ms.author: radeltch
ms.openlocfilehash: b41db629c5308348f632b3dc51c75822ba361c60
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77591362"
---
# <a name="high-availability-for-sap-netweaver-on-azure-vms-on-windows-with-azure-netapp-filessmb-for-sap-applications"></a>SAP uygulamaları için Azure NetApp Files(SMB) ile Windows'daki Azure VM'lerde SAP NetWeaver için yüksek kullanılabilirlik

[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md

[anf-azure-doc]:https://docs.microsoft.com/azure/azure-netapp-files/
[anf-avail-matrix]:https://azure.microsoft.com/global-infrastructure/services/?products=storage&regions=all
[anf-register]:https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-register
[anf-sap-applications-azure]:https://www.netapp.com/us/media/tr-4746.pdf

[2205917]:https://launchpad.support.sap.com/#/notes/2205917
[1944799]:https://launchpad.support.sap.com/#/notes/1944799
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[1984787]:https://launchpad.support.sap.com/#/notes/1984787
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[1410736]:https://launchpad.support.sap.com/#/notes/1410736

[sap-swcenter]:https://support.sap.com/en/my-support/software-downloads.html

[suse-ha-guide]:https://www.suse.com/products/sles-for-sap/resource-library/sap-best-practices/
[suse-drbd-guide]:https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha_techguides/book_sleha_techguides.html
[suse-ha-12sp3-relnotes]:https://www.suse.com/releasenotes/x86_64/SLE-HA/12-SP3/

[template-multisid-xscs]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs-md%2Fazuredeploy.json
[template-converged]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-converged-md%2Fazuredeploy.json
[template-file-server]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-file-server-md%2Fazuredeploy.json

[sap-hana-ha]:sap-hana-high-availability.md
[nfs-ha]:high-availability-guide-suse-nfs.md

Bu makalede, [Azure NetApp Dosyalarında](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-introduction/) [Kobİ](https://docs.microsoft.com/windows/win32/fileio/microsoft-smb-protocol-and-cifs-protocol-overview) kullanarak sanal makinelerin nasıl dağıtılanın, yapılandırılabilmek, küme çerçevesini yükleyin ve Windows VM'lerde yüksek oranda kullanılabilir bir SAP NetWeaver 7.50 sistemi yüklenir.  

Veritabanı katmanı bu makalede ayrıntılı olarak ele alınmıyor. Azure [sanal ağının](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) zaten oluşturulduğunu varsayıyoruz.  

Önce aşağıdaki SAP Notlarını ve bildirilerini okuyun:

* [Azure NetApp Dosyaları belgeleri][anf-azure-doc] 
* SAP Not [1928533][1928533], içerir:  
  * SAP yazılımının dağıtımı için desteklenen Azure VM boyutlarının listesi
  * Azure VM boyutları için önemli kapasite bilgileri
  * Desteklenen SAP yazılımı ve işletim sistemi (OS) ve veritabanı kombinasyonları
  * Microsoft Azure'da Windows için gerekli SAP çekirdeği sürümü
* SAP Note [2015553,][2015553] Azure'da SAP destekli SAP yazılım dağıtımları için ön koşulları listeler.
* SAP Note [2178632,][2178632] Azure'da SAP için bildirilen tüm izleme ölçümleri hakkında ayrıntılı bilgilere sahiptir.
* SAP Note [1999351,][1999351] SAP için Azure Gelişmiş İzleme Uzantısı için ek sorun giderme bilgilerine sahiptir.
* SAP Note [2287140,](https://launchpad.support.sap.com/#/notes/2287140) Kobİ 3.x protokolünün SAP destekli CA özelliği için ön koşulları listeler.
* SAP Note [2802770,](https://launchpad.support.sap.com/#/notes/2802770) Windows 2012 ve 2016'da yavaş çalışan SAP işlem AL11 için sorun giderme bilgilerine sahiptir.
* SAP Note [1911507,](https://launchpad.support.sap.com/#/notes/1911507) Windows Server'da SMB 3.0 protokolü ile dosya paylaşımı için saydam hata özelliği hakkında bilgilere sahiptir.
* SAP Note [662452,](https://launchpad.support.sap.com/#/notes/662452) veri erişimleri sırasında kötü dosya sistemi performansını/hatalarını gidermek için öneri (8,3 ad oluşturmayı devre dışı kalmıştır).
* [Azure'daki SAP ASCS/SCS örnekleri için Windows failover kümesine ve dosya paylaşımına SAP NetWeaver yüksek kullanılabilirliği yükleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-installation-wsfc-file-share) 
* [SAP NetWeaver için Azure Sanal Makineler yüksek kullanılabilirlik mimarisi ve senaryoları](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-architecture-scenarios)
* [ASCS küme yapılandırmasında sonda bağlantı noktası ekleme](sap-high-availability-installation-wsfc-file-share.md)
* [Failover Kümesine (A)SCS Örneğinin Yüklenmesi](https://www.sap.com/documents/2017/07/f453332f-c97c-0010-82c7-eda71af511fa.html)
* [Azure NetApp Files için SMB birimi oluşturma](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-create-volumes-smb#requirements-for-active-directory-connections)
* [Azure NetApp Dosyalarını Kullanarak Microsoft Azure'daki NetApp SAP Uygulamaları][anf-sap-applications-azure]

## <a name="overview"></a>Genel Bakış

SAP, bir Windows failover kümesinde SAP ASCS/SCS örneğini kümelemek için yeni bir yaklaşım ve paylaşılan diskleri kümeleme alternatifi geliştirdi. Küme paylaşılan diskleri kullanmak yerine, SAP genel ana bilgisayar dosyalarını dağıtmak için Bir Kobİ dosya paylaşımı kullanabilirsiniz. Azure NetApp Files, Active Directory'yi kullanarak NTFS ACL ile SMBv3'ü (NFS ile birlikte) destekler. Azure NetApp Files otomatik olarak yüksek oranda kullanılabilir (bir PaaS hizmeti olduğu için). Bu özellikler, Azure NetApp Files'ı SAP global için Kobİ dosya paylaşımını barındırmak için mükemmel bir seçenek haline getirin.  
Hem [Azure Etkin Dizin (AD) Etki Alanı Hizmetleri](https://docs.microsoft.com/azure/active-directory-domain-services/overview) hem de Active [Directory Domain Services (AD DS)](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview) desteklenir. Azure NetApp Dosyaları ile varolan Active Directory etki alanı denetleyicilerini kullanabilirsiniz. Etki alanı denetleyicileri Azure'da sanal makine olarak veya ExpressRoute veya S2S VPN aracılığıyla şirket içinde olabilir. Bu makalede, Bir Azure VM etki alanı denetleyicisi kullanacağız.  
SAP Netweaver merkezi hizmetleri için yüksek kullanılabilirlik (HA) paylaşılan depolama gerektirir. Windows'da bunu başarmak için şimdiye kadar SOFS kümesi ni oluşturmak veya SIOS gibi küme paylaşılan disk s/w kullanmak gerekiyordu. Artık Azure NetApp Dosyalarında dağıtılan paylaşılan depolama alanını kullanarak SAP Netweaver HA'ya ulaşmak mümkün. Paylaşılan depolama alanı için Azure NetApp Dosyalarını kullanmak, SOFS veya SIOS gereksinimini ortadan kaldırır.  

> [!NOTE]
> SAP ASCS/SCS örneklerini bir dosya paylaşımı kullanarak kümeleme sap NetWeaver 7.40 (ve sonrası) için, SAP Kernel 7.49 (ve daha sonra) ile desteklenir.  

![SMB payı ile SAP ASCS/SCS HA Mimarisi](./media/virtual-machines-shared-sap-high-availability-guide/high-availability-windows-azure-netapp-files-smb.png)

Kobİ dosya paylaşımı için ön koşullar şunlardır:
* SMB 3.0 (veya sonraki) protokolü.
* Active Directory kullanıcı grupları ve bilgisayar$ bilgisayar nesnesi için Active Directory erişim denetim listeleri (ALA)'ler ayarlama yeteneği.
* Dosya paylaşımı HA etkin olmalıdır.

SAP Central hizmetlerinin bu başvuru mimarisindeki payı Azure NetApp Files tarafından sunulur:

![SMB payı ile SAP ASCS/SCS HA Mimarisi](./media/virtual-machines-shared-sap-high-availability-guide/high-availability-windows-azure-netapp-files-smb-detail.png)

## <a name="create-and-mount-smb-volume-for-azure-netapp-files"></a>Azure NetApp Dosyaları için SMB hacmi oluşturma ve takma

Azure NetApp Dosyalarını kullanmaya hazırlık olarak aşağıdaki adımları gerçekleştirin.  

1. [Azure NetApp Dosyalarına Kaydolmak için](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-register) aşağıdaki adımları izleyin  
2. [NetApp hesabı oluştur'da](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-create-netapp-account) açıklanan adımları izleyerek Azure NetApp hesabı oluşturun  
3. [Kapasite havuzu ayarlama'daki](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-set-up-capacity-pool) yönergeleri izleyerek kapasite havuzu ayarlama
4. Azure NetApp Files kaynakları, devredilen alt ağda yer almalıdır. Temsilci alt ağı oluşturmak [için Azure NetApp Files'a bir alt ağ delegesi](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-delegate-subnet) yönergeleriizleyin.  

> [!IMPORTANT]
> Bir Kobİ birimi oluşturmadan önce Active Directory bağlantıları oluşturmanız gerekir. Etkin [Dizin bağlantıları nın gereksinimlerini gözden geçirin.](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-create-volumes-smb#requirements-for-active-directory-connections)  

5. [Etkin Dizin bağlantısı oluştur'da](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-create-volumes-smb#create-an-active-directory-connection) açıklandığı gibi Etkin Dizin bağlantısı oluşturma  
6. [SMB toplu](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-create-volumes-smb#add-an-smb-volume) luk ekle'deki yönergeleri izleyerek SMB Azure NetApp Files SMB birimi oluşturma  
7. Windows Virtual Machine'inizde Kobİ ses düzeyini monte edin.

> [!TIP]
> [Azure NetApp](https://portal.azure.com/#home) Dosyaları hacmini nasıl monte edinebileceğinizle ilgili talimatları bulabilirsiniz, Azure Portalı'nda Azure NetApp Files nesnesine doğru gezinerseniz, **Birimler** bıçağına tıklayın ve **ardından Talimatlara Bağlarsınız.**  

## <a name="prepare-the-infrastructure-for-sap-ha-by-using-a-windows-failover-cluster"></a>Windows failover kümesini kullanarak SAP HA için altyapıyı hazırlama 

1. [Gerekli DNS IP adreslerini ayarlama](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-shared-disk#b22d7b3b-4343-40ff-a319-097e13f62f9e)  
2. [SAP sanal makineleri için statik IP adreslerini ayarlayın.](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-shared-disk#84c019fe-8c58-4dac-9e54-173efd4b2c30)
3. [Azure dahili yük dengeleyicisi için statik bir IP adresi ayarlayın.](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-shared-disk#7a8f3e9b-0624-4051-9e41-b73fff816a9e)
4. [Azure dahili yük dengeleyicisi için varsayılan ASCS/SCS yük dengeleme kurallarını ayarlayın.](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-shared-disk#f19bd997-154d-4583-a46e-7f5a69d0153c)
5. [Azure dahili yük dengeleyicisi için ASCS/SCS varsayılan yük dengeleme kurallarını değiştirin.](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-shared-disk#fe0bd8b5-2b43-45e3-8295-80bee5415716)
6. [Etki alanına Windows sanal makineleri ekleyin.](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-shared-disk#e69e9a34-4601-47a3-a41c-d2e11c626c0c)
7. [SAP ASCS/SCS örneğinin her iki küme düğümüne de kayıt defteri girişleri ekleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-shared-disk#661035b2-4d0f-4d31-86f8-dc0a50d78158)
8. [SAP ASCS/SCS örneği için windows server failover kümesi ni ayarlama](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-shared-disk#0d67f090-7928-43e0-8772-5ccbf8f59aab)
9. Windows Server 2016 kullanıyorsanız, Azure Bulut [Tanığı'nı](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness)yapılandırmanızı öneririz.


## <a name="install-sap-ascs-instance-on-both-nodes"></a>Her iki düğüme de SAP ASCS örneğini yükleme

SAP'nin aşağıdaki yazılımına ihtiyacınız var:
   * SAP Yazılım Provisioning Manager (SWPM) yükleme aracı sürümü SPS25 veya daha sonra.
   * SAP Çekirdeği 7.49 veya sonrası
   * [Kümelenmiş SAP ASCS/SCS örneği için sanal ana bilgisayar adı oluşturma'da](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-installation-wsfc-shared-disk#a97ad604-9094-44fe-a364-f89cb39bf097)açıklandığı gibi, kümelenmiş SAP ASCS/SCS örneği için sanal ana bilgisayar adı oluşturun.

> [!NOTE]
> SAP ASCS/SCS örneklerini bir dosya paylaşımı kullanarak kümeleme sap NetWeaver 7.40 (ve sonrası) için, SAP Kernel 7.49 (ve daha sonra) ile desteklenir.  

### <a name="install-an-ascsscs-instance-on-the-first-ascsscs-cluster-node"></a>İlk ASCS/SCS küme düğümüne BIR ASCS/SCS örneği yükleme

1. İlk küme düğümüne bir SAP ASCS/SCS örneği yükleyin. SAP SWPM yükleme aracını çalıştırın ve şuna gidin: **Product** > **DBMS** > Installation > Application Server ABAP (veya Java) > Yüksek Kullanılabilirlik Sistemi > ASCS/SCS örneği > İlk küme düğümü.  

2. SWPM'de Küme paylaşım Yapılandırması olarak **Dosya Paylaşım Kümesi'ni** seçin.  
3. Sap **Sistem Küme Parametreleri**adımsorulduğunda, **Dosya Paylaşım Ana Bilgisayar Adı**olarak oluşturduğunuz Azure NetApp Files SMB paylaşımının ana bilgisayar adını girin.  Bu örnekte, Kobİ payı ana bilgisayar adı **anfsmb-9562'dir.** 

> [!IMPORTANT]
> SWPM'deki ön koşul denetleyicisi Sonuçları sürekli kullanılabilirlik özelliği koşulunun karşılanmadığını [gösteriyorsa, Windows'da artık var olmayan paylaşılan bir klasöre erişmeye çalıştığınızda Gecikmeli hata iletisindeki](https://support.microsoft.com/help/2820470/delayed-error-message-when-you-try-to-access-a-shared-folder-that-no-l)yönergeleri izleyerek ele alınabilir.  

> [!TIP]
> SWPM'deki Ön koşul denetleyici sonuçları Swap Boyutu koşulunun yerine getirilmediğini gösteriyorsa, Bilgisayarım>Sistem Özellikleri>Performans Ayarları> Gelişmiş> Sanal bellek> Değiştir'e yönlendirerek SWAP boyutunu ayarlayabilirsiniz.  

4. PowerShell kullanarak sap küme `SAP-SID-IP` kaynağıolan sonda bağlantı noktasını yapılandırın. [Bu yapılandırmayı, Configure prob bağlantı noktasında](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-installation-wsfc-shared-disk#10822f4f-32e7-4871-b63a-9b86c76ce761)açıklandığı gibi SAP ASCS/SCS küme düğümlerinden birinde gerçekleştirin.

### <a name="install-an-ascsscs-instance-on-the-second-ascsscs-cluster-node"></a>İkinci ASCS/SCS küme düğümüne bir ASCS/SCS örneği yükleme

1. İkinci küme düğümüne bir SAP ASCS/SCS örneği yükleyin. SAP SWPM yükleme aracını çalıştırın, ardından Ek küme düğümü > Yüksek Kullanılabilirlik Sistemi > Yüksek Kullanılabilirlik Sistemi > **Product** > **DBMS** > Installation > Application Server ABAP (veya Java) > gidin.  

### <a name="install-a-dbms-instance-and-sap-application-servers"></a>DBMS örneği ve SAP uygulama sunucuları yükleme

SAP yüklemenizi yükleyerek tamamlayın:

   * Bir DBMS örneği  
   * Birincil SAP uygulama sunucusu  
   * Ek bir SAP uygulama sunucusu  

## <a name="test-the-sap-ascsscs-instance-failover"></a>SAP ASCS/SCS örneği başarısızını test edin 

### <a name="fail-over-from-cluster-node-a-to-cluster-node-b-and-back"></a>Küme düğümü A'dan B kümesi düğümüne ve arkaya geçme
Bu test senaryosunda küme düğümü sapascs1 nod a olarak, küme nodu sapascs2 düğüm bolarak b olarak ifade edecektir.

1. Küme kaynaklarının A düğümüzerinde çalıştığını ![doğrulayın.](./media/virtual-machines-shared-sap-high-availability-guide/high-availability-windows-azure-netapp-files-smb-figure-1.png)  

2. Küme düğümü A'yı yeniden başlatın. SAP küme kaynakları küme düğümüne taşınır ![B. Şekil 2: Windows Server failover küme kaynakları üzerinde çalışan b kümesi kaynaklarını başarısız lık testinden sonra](./media/virtual-machines-shared-sap-high-availability-guide/high-availability-windows-azure-netapp-files-smb-figure-2.png)  


## <a name="lock-entry-test"></a>Kilit giriş testi

1.SAP Enqueue Çoğaltma Sunucusunun (ERS) etkin olduğunu doğrulayın  
2. SAP sistemine giriş yapın, işlem SU01'i çalıştırın ve değişiklik modunda bir kullanıcı kimliği açın. Bu SAP kilit girişi oluşturur.  
3. SAP sisteminde oturum açtığınızda, IŞLEM ST12'ye yönlendirilerek kilit girişini görüntüleyin.  
4. A küme düğümünden B kümesi düğümüne KADAR ASCS kaynakları üzerinde başarısız.  
5. SAP ASCS/SCS küme kaynakları nın başarısız olması için oluşturulan kilit girişinin korunup korunduğunu doğrulayın.  

![Şekil 3: Kilit girişi başarısız testten sonra korunur](./media/virtual-machines-shared-sap-high-availability-guide/high-availability-windows-azure-netapp-files-smb-figure-3.png)  

Daha fazla bilgi [için, ERS ile ASCS'de Enqueue Failover için Sorun Giderme](https://wiki.scn.sap.com/wiki/display/SI/Troubleshooting+for+Enqueue+Failover+in+ASCS+with+ERS)
## <a name="next-steps"></a>Sonraki adımlar

* [AZURE Sanal Makineler SAP için planlama ve uygulama][planning-guide]
* [SAP için Azure Sanal Makineler dağıtımı][deployment-guide]
* [SAP için Azure Sanal Makineler DBMS dağıtımı][dbms-guide]
* SAP'nin olağanüstü durum kurtarma durumu için yüksek kullanılabilirlik ve plan oluşturmayı öğrenmek için 
* Azure'da HANA (büyük örnekler), [Azure'da SAP HANA (büyük örnekler) yüksek kullanılabilirlik ve olağanüstü durum kurtarma](hana-overview-high-availability-disaster-recovery.md)çalışmalarına bakın.
* Azure Sanal M'lerde SAP HANA'nın yüksek kullanılabilirlik oluşturmasını ve olağanüstü kurtarma yı planlamayı öğrenmek için Azure [Sanal Makinelerde (VM) SAP HANA'nın Yüksek Kullanılabilirliği bölümüne][sap-hana-ha] bakın
