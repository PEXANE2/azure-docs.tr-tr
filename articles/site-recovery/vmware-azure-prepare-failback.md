---
title: Azure Site Kurtarma ile VMware VM'leri yeniden koruma ve geri ödeme için hazırlayın
description: Azure Site Kurtarma ile başarısız olduktan sonra VMware VM'lerin geri başarısızlığına hazırlanın
ms.topic: conceptual
ms.date: 12/24/2019
ms.openlocfilehash: 5a330f8cba31640d0116ca3d5ccab352ce5b3509
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79257192"
---
# <a name="prepare-for-reprotection-and-failback-of-vmware-vms"></a>VMware VM'lerin yeniden korunması ve geri tepmesi için hazırlanın

Şirket içi VMware VM'leri veya fiziksel sunucuları Azure'da [başarısız](site-recovery-failover.md) olduktan sonra, başarısız olduktan sonra oluşturulan Azure VM'lerini yeniden korursunuz, böylece şirket içi siteye tekrar çoğaltılır. Azure'dan şirket içi çoğaltma ile, hazır olduğunuzda Azure'dan şirket başına bir arıza çalıştırarak geri başarısız olabilirsiniz.

Devam etmeden önce, Azure'dan şirket içi bir siteye nasıl geri dönüleceğine ilişkin bu videoyla hızlı bir genel bakış elde edin.<br /><br />
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video5-Failback-from-Azure-to-On-premises/player]

## <a name="reprotectionfailback-components"></a>Yeniden koruma/geri tepme bileşenleri

Azure'u yeniden koruyup geri alabilmeniz için bir dizi bileşenve ayarı yerine yerleştirmeniz gerekir.

**Bileşen**| **Şey**
--- | ---
**Şirket içi yapılandırma sunucusu** | Şirket içi yapılandırma sunucusu çalışıyor ve Azure'a bağlı olmalıdır.<br/><br/> Geri başarısız olduğunuz VM yapılandırma sunucusu veritabanında bulunması gerekir. Olağanüstü durum yapılandırma sunucusunu etkiliyorsa, hata geri almanın çalıştığından emin olmak için aynı IP adresiyle geri yükleyin.<br/><br/>  Çoğaltılan makinelerin IP adresleri başarısız bir şekilde tutulduysa, Azure VM makineleri ile yapılandırma sunucusunun geri dönüş NIC'i arasında siteden siteye bağlantı (veya ExpressRoute bağlantısı) oluşturulmalıdır. Tutulan IP adresleri için yapılandırma sunucusunun biri kaynak makine bağlantısı, diğeri de Azure geri dönüş bağlantısı olmak üzere iki NIC'e ihtiyacı vardır. Bu, kaynak için alt ağ adres aralıklarının çakışmasını önler ve VM'ler üzerinde başarısız olunmasını önler.
**Azure'da işlem sunucusu** | Şirket içi sitenize geri dönebilmeniz için Azure'da bir işlem sunucusuna ihtiyacınız vardır.<br/><br/> İşlem sunucusu, korunan Azure VM'den veri alır ve şirket içi siteye gönderir.<br/><br/> İşlem sunucusu ve korumalı VM arasında düşük gecikmeli bir ağa ihtiyacınız vardır, bu nedenle işlem sunucusunu daha yüksek çoğaltma performansı için Azure'da dağıtmanızı öneririz.<br/><br/> Kavram kanıtı için, şirket içi işlem sunucusunu ve ExpressRoute'u özel bir bakışla kullanabilirsiniz.<br/><br/> İşlem sunucusu, VM üzerinde başarısız olanın bulunduğu Azure ağında olmalıdır. İşlem sunucusu ayrıca şirket içi yapılandırma sunucusu ve ana hedef sunucu ile iletişim kurabilmeli.
**Ayrı ana hedef sunucusu** | Ana hedef sunucu failback verileri alır ve varsayılan olarak bir Windows ana hedef sunucusu şirket içi yapılandırma sunucusunda çalışır.<br/><br/> Ana hedef sunucuda en fazla 60 disk eklenebilir. Geri başarısız olan VM'ler toplam 60 diskten fazla toplua sahiptir veya büyük hacimlerde trafikte başarısız oluyorsanız, failback için ayrı bir ana hedef sunucu oluşturur.<br/><br/> Makineler çoklu VM tutarlılığı için bir çoğaltma grubuna toplanmışsa, VM'lerin tümü Windows olmalıdır veya tümü Linux olmalıdır. Neden? Çoğaltma grubundaki tüm VM'ler aynı ana hedef sunucuyu kullanmalıdır ve ana hedef sunucu çoğaltılan makinelerden aynı işletim sistemine (aynı veya daha yüksek sürümle) sahip olmalıdır.<br/><br/> Ana hedef sunucunun disklerinde anlık görüntü olmamalıdır, aksi takdirde yeniden koruma ve geri dönüş çalışmaz.<br/><br/> Ana hedefin Paravirtual SCSI denetleyicisi olamaz. Denetleyici yalnızca LSI Logic denetleyicisi olabilir. LSI Logic denetleyicisi olmadan yeniden koruma başarısız olur.
**Failback çoğaltma ilkesi** | Şirket içi siteye geri dönmek için bir geri dönüş ilkesine ihtiyacınız vardır. Bu ilke, Azure'da bir çoğaltma ilkesi oluşturduğunuzda otomatik olarak oluşturulur.<br/><br/> İlke, yapılandırma sunucusu ile otomatik olarak ilişkilendirilir. 15 dakikalık bir RPO eşiğine, 24 saatlik kurtarma noktası tutma ve uygulama tutarlı anlık görüntü sıklığı 60 dakika olarak ayarlanır. İlke düzenlenemez. 
**Siteden siteye VPN/ExpressRoute özel eşleme** | Yeniden koruma ve geri dönüş için siteden siteye VPN bağlantısına veya verileri çoğaltmak için ExpressRoute özel eşleme gerekir. 


## <a name="ports-for-reprotectionfailback"></a>Yeniden koruma/geri dönüş için bağlantı noktaları

Bir dizi bağlantı noktası yeniden koruma/geri tepme için açık olmalıdır. Aşağıdaki grafik, bağlantı noktalarını ve yeniden koruma/geri tepme akışını göstermektedir.

![Arıza ve geri dönüş için bağlantı noktaları](./media/vmware-azure-reprotect/failover-failback.png)


## <a name="deploy-a-process-server-in-azure"></a>Azure'da bir işlem sunucusu dağıtma

1. Hata geri almak için Azure'da [bir işlem sunucusu ayarlayın.](vmware-azure-set-up-process-server-azure.md)
2. Azure VM'lerin işlem sunucusuna erişebilmesini sağlayın. 
3. Siteden siteye VPN bağlantısının veya ExpressRoute özel izleme ağının işlem sunucusundan şirket içi siteye veri göndermek için yeterli bant genişliğine sahip olduğundan emin olun.

## <a name="deploy-a-separate-master-target-server"></a>Ayrı bir ana hedef sunucu dağıtma

1. Ana hedef sunucu [gereksinimlerine ve sınırlamalarına](#reprotectionfailback-components)dikkat edin.
2. Yeniden korumak ve geri başarısız olmak istediğiniz VM'lerin işletim sistemiyle eşleşecek bir [Windows](site-recovery-plan-capacity-vmware.md#deploy-additional-master-target-servers) veya [Linux](vmware-azure-install-linux-master-target.md) ana hedef sunucusu oluşturun.
3. Ana hedef sunucu için Storage vMotion'ı kullanmadığınızdan emin olun veya geri dönüş başarısız olabilir. Diskler kullanılamadığı ndan VM makinesi başlatılamıyor.
    - Bunu önlemek için ana hedef sunucuyu vMotion listenizden hariç tinleyin.
    - Ana hedef yeniden korumadan sonra bir Depolama vMotion görevine girerse, ana hedef sunucuya bağlı korumalı VM diskleri vMotion görevinin hedefine taşınır. Bundan sonra başarısız olmaya çalışırsanız, diskler bulunamadığından disk ayırma başarısız olur. Daha sonra depolama hesaplarınızdaki diskleri bulmak zordur. Bu durumda, bunları el ile bulun ve VM'ye takın. Bundan sonra, şirket içi VM önyüklenebilir.

4. Varolan Windows ana hedef sunucusuna bir bekletme sürücüsü ekleyin. Yeni bir disk ekleyin ve sürücüyü biçimlendirin. Bekletme sürücüsü, VM'nin şirket içi siteye geri kopyalandığında noktaları zamanında durdurmak için kullanılır. Bu ölçütleri not edin. Bunlar karşılanmıyorsa, sürücü ana hedef sunucu için listelenmez:
    - Birim, çoğaltma hedefi gibi başka bir amaç için kullanılmaz ve kilit modunda değildir.
    - Birim bir önbellek birimi değildir. İşlem sunucusu ve ana hedef için özel yükleme hacmi bekletme birimi için uygun değildir. İşlem sunucusu ve ana hedef bir birim üzerine yüklendiğinde, birim ana hedefin önbellek birimidir.
    - Birimin dosya sistemi türü FAT veya FAT32 değildir.
    - Birim kapasitesi sıfır değil.
    - Windows için varsayılan bekletme birimi R birimidir.
    - Linux için varsayılan saklama hacmi /mnt/retention'dır.

5. Varolan bir işlem sunucusu kullanıyorsanız sürücü ekleyin. Yeni sürücü son adımda gereksinimleri karşılamalıdır. Bekletme sürücüsü yoksa, portaldaki seçim açılır listesinde görünmez. Şirket içi ana hedefe bir sürücü ekledikten sonra, sürücünün portaldaki seçimde görünmesi 15 dakika kadar sürer. Sürücü 15 dakika sonra görünmüyorsa yapılandırma sunucusunu yenileyebilirsiniz.
6. Ana hedef sunucuya VMware araçlarını veya açık vm-araçlarını yükleyin. Araçlar olmadan, ana hedefin ESXi ana bilgisayarındaki veri depoları algılanamıyor.
7. Diski ayarlayın. VMware'deki ana hedef VM'nin yapılandırma parametrelerinde EnableUUID=true ayarı. Bu satır yoksa, ekleyin. Bu ayar, VMDK'ya tutarlı bir UUID sağlamak için gereklidir, böylece doğru şekilde bağlanır.
8. vCenter Server erişim gereksinimlerini kontrol edin:
    - Geri başarısız olduğunuz VM, VMware vCenter Server tarafından yönetilen bir ESXi ana bilgisayarındaysa, çoğaltılan verileri sanal makinenin disklerine yazmak için ana hedef sunucunun şirket içi VM Sanal Makine Diski (VMDK) dosyasına erişmesi gerekir. Şirket içi VM veri deposunun okuma/yazma erişimine sahip ana hedef ana bilgisayara monte edilmiş olduğundan emin olun.
    - VM bir VMware vCenter Server tarafından yönetilen bir ESXi ana bilgisayarda değilse, Site Kurtarma yeniden koruma sırasında yeni bir VM oluşturur. Bu VM, ana hedef sunucu VM'i oluşturduğunuz ESXi ana bilgisayarda oluşturulur. İstediğiniz ana bilgisayarda VM oluşturmak için ESXi ana bilgisayarını dikkatle seçin. VM’nin sabit diski, üzerinde ana hedef sunucunun çalıştığı ana bilgisayar tarafından erişilebilen bir veri deposunda olmalıdır.
    - Başka bir seçenek, şirket içi VM zaten failback için varsa, bir failback yapmadan önce silmektir. Failback sonra ana hedef ESXi ana bilgisayar olarak aynı ana bilgisayarda yeni bir VM oluşturur. Alternatif bir konuma geri döndüğünüzde, veriler aynı veri deposuna ve şirket içi ana hedef sunucu tarafından kullanılan aynı ESXi ana bilgisayara kurtarılır.
9. VMware VM'lere geri dönemeyen fiziksel makineler için, makineyi yeniden korumadan önce ana hedef sunucunun çalıştırdığı ana bilgisayarını bulmanız gerekir.
10. Ana hedef VM'nin bağlı olduğu ESXi ana bilgisayarının en az bir sanal makine dosya sistemi (VMFS) veri deposu na sahip olup olmadığını kontrol edin. VMFS veri depoları eklenmişse, yeniden koruma ayarlarındaki veri deposu girişi boştur ve devam edemezsiniz.


## <a name="next-steps"></a>Sonraki adımlar

VM'yi [yeniden koruyun.](vmware-azure-reprotect.md)
