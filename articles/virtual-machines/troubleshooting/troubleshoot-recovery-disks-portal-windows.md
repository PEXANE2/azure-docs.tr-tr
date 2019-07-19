---
title: Azure portal bir Windows sorun giderme sanal makinesi kullanın | Microsoft Docs
description: Azure portal kullanarak işletim sistemi diskini bir kurtarma VM 'sine bağlayarak Azure 'da Windows sanal makine sorunlarını nasıl giderebileceğinizi öğrenin
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: gwallace
editor: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 08/13/2018
ms.author: genli
ms.openlocfilehash: 8ab6fc75475cd99e3d803450476880175f12d2b6
ms.sourcegitcommit: 1b7b0e1c915f586a906c33d7315a5dc7050a2f34
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/15/2019
ms.locfileid: "67881181"
---
# <a name="troubleshoot-a-windows-vm-by-attaching-the-os-disk-to-a-recovery-vm-using-the-azure-portal"></a>Azure portal kullanarak işletim sistemi diskini bir kurtarma VM 'sine ekleyerek bir Windows sanal makinesi sorunlarını giderme
Azure 'daki Windows sanal makineniz (VM) bir önyükleme veya disk hatasıyla karşılaşırsa, sanal sabit diskin kendisi üzerinde sorun giderme adımları gerçekleştirmeniz gerekebilir. Ortak bir örnek, VM 'nin başarıyla önyükleme yapabilmesini önleyen başarısız bir uygulama güncelleştirmesidir. Bu makalede, tüm hataları onarmak için sanal sabit diskinizi başka bir Windows sanal makineye bağlamak üzere Azure portal kullanma ve ardından özgün VM 'nizi yeniden oluşturma ayrıntıları yer aldığı açıklanır. 

## <a name="recovery-process-overview"></a>Kurtarma işlemine genel bakış
Sorun giderme işlemi aşağıdaki gibidir:

1. Sanal sabit diskleri koruyarak, sorunlarla karşılaşmadan VM 'yi silin.
2. Sorun giderme amacıyla sanal sabit diski başka bir Windows sanal makinesine ekleyin ve bağlayın.
3. Sorun giderme işlemlerini yapacağınız VM'ye bağlanın. Özgün sanal sabit diskteki sorunları gidermek için dosyaları düzenleyin veya herhangi bir araç çalıştırın.
4. Sorun giderme işlemlerini yaptığınız VM’den sanal sabit diski çıkarın.
5. Özgün sanal sabit diski kullanarak bir VM oluşturun.

Yönetilen disk kullanan VM için artık bir VM 'nin işletim sistemi diskini değiştirmek üzere Azure PowerShell kullanabiliriz. Artık VM 'yi silip yeniden oluşturma gereksinimi yoktur. Daha fazla bilgi için bkz. [Azure PowerShell kullanarak işletim sistemi diskini bir kurtarma VM 'sine ekleyerek WINDOWS VM sorunlarını giderme](troubleshoot-recovery-disks-windows.md).

> [!NOTE]
> Bu makale, yönetilmeyen disk içeren VM için geçerlidir.

## <a name="determine-boot-issues"></a>Önyükleme sorunlarını belirleme
SANAL makinenizin neden doğru şekilde önyüklenemediğini öğrenmek için önyükleme tanılama VM ekran görüntüsünü inceleyin. Ortak bir örnek, başarısız bir uygulama güncelleştirmesi veya silinmekte veya taşınmakta olan temel bir sanal sabit disk olabilir.

Portalda VM 'nizi seçin ve sonra da **destek + sorun giderme** bölümüne gidin. Ekran görüntüsünü görüntülemek için **önyükleme tanılama** ' ya tıklayın. VM 'nin neden bir sorunla karşılaşmadığını belirlemenize yardımcı olmak için belirli hata mesajlarını veya hata kodlarını aklınızda olun.

![VM önyüklemesi Tanılama Konsolu günlüklerini görüntüleme](./media/troubleshoot-recovery-disks-portal-windows/screenshot-error.png)

VM ekran görüntüsünün bir kopyasını indirmek için **ekran görüntüsünü indir** ' e de tıklayabilirsiniz.

## <a name="view-existing-virtual-hard-disk-details"></a>Var olan sanal sabit disk ayrıntılarını görüntüle
Sanal sabit diskinizi başka bir sanal makineye iliştirebilmeniz için önce sanal sabit diskin (VHD) adını belirlemeniz gerekir.

Sorunlu VM 'yi seçin ve ardından **diskler**' i seçin. Aşağıdaki örnekte gösterildiği gibi işletim sistemi diskinin adını kaydedin:

![Depolama bloblarını seçin](./media/troubleshoot-recovery-disks-portal-windows/view-disk.png)

## <a name="delete-existing-vm"></a>Mevcut VM 'yi Sil
Sanal sabit diskler ve sanal makineler Azure'da iki ayrı kaynaktır. Sanal sabit disk, işletim sisteminin kendisi, uygulamalar ve yapılandırmaların depolandığı yerdir. VM, yalnızca boyutu veya konumu tanımlayan meta verileridir ve bir sanal sabit disk veya sanal ağ arabirim kartı (NIC) gibi kaynaklara başvurur. Her sanal sabit disk, bir VM 'ye eklendiğinde atanan bir kiralamaya sahiptir. Veri diskleri VM çalışırken bile eklenip çıkarılabilir, ancak VM kaynağı silinmedikçe işletim sistemi diski çıkarılamaz. Kira, sanal makine durdurulmuş ve serbest bırakılmış durumda olduğunda bile, işletim sistemi diskini bir VM ile ilişkilendirmeye devam eder.

VM 'nizi kurtarmanın ilk adımı VM kaynağının kendisini silmektir. VM’yi sildiğinizde sanal sabit diskler depolama hesabınızda kalır. VM silindikten sonra, hataları gidermek ve gidermek için sanal sabit diski başka bir VM 'ye iliştirmeniz gerekir.

Portalda VM 'nizi seçin ve **Sil**' e tıklayın:

![Önyükleme hatasını gösteren VM önyükleme tanılaması ekran görüntüsü](./media/troubleshoot-recovery-disks-portal-windows/stop-delete-vm.png)

Sanal sabit diski başka bir VM 'ye iliştirmadan önce VM 'nin silmeyi bitirene kadar bekleyin. Sanal sabit diski başka bir VM 'ye iliştirebilmeniz için VM ile ilişkilendiren sanal sabit disk üzerindeki kira serbest bırakılması gerekir.

## <a name="attach-existing-virtual-hard-disk-to-another-vm"></a>Var olan sanal sabit diski başka bir VM 'ye Ekle
Sonraki birkaç adımda, sorun giderme amacıyla başka bir VM kullanırsınız. Diskin içeriğine gözatabilmek ve düzenleyebilmeniz için, var olan sanal sabit diski bu sorun giderme sanal makinesine iliştiriyor. Bu işlem, örneğin yapılandırma hatalarını düzeltmenize veya ek uygulama ya da sistem günlük dosyalarını incelemenizi sağlar. Sorun giderme amacıyla kullanmak için başka bir VM seçin veya oluşturun.

1. Portaldan kaynak grubunuzu seçin ve ardından sorun giderme sanal makinesini seçin. **Diskler**' i seçin, **Düzenle**' yi seçin ve **veri diski Ekle**' ye tıklayın:

    ![Portala mevcut diski iliştirme](./media/troubleshoot-recovery-disks-portal-windows/attach-existing-disk.png)

2. **Veri diskleri** listesinde, tanımladığınız VM 'nin işletim sistemi diskini seçin. İşletim sistemi diskini görmüyorsanız, sanal makine ve işletim sistemi diskinin aynı bölgede (konum) olduğundan emin olun.
3. Değişiklikleri uygulamak için **Kaydet** ' i seçin.

## <a name="mount-the-attached-data-disk"></a>Bağlı veri diskini bağlama

1. Sanal makinenize bir Uzak Masaüstü bağlantısı açın. Portalda VM 'nizi seçin ve **Bağlan**' a tıklayın. RDP bağlantı dosyasını indirip açın. SANAL makinenizde şu şekilde oturum açmak için kimlik bilgilerinizi girin:

    ![Uzak Masaüstü kullanarak sanal makinenizde oturum açın](./media/troubleshoot-recovery-disks-portal-windows/open-remote-desktop.png)

2. **Sunucu Yöneticisi**açın **ve ardından dosya ve depolama hizmetleri**' ni seçin. 

    ![Sunucu Yöneticisi içinde dosya ve depolama hizmetleri seçin](./media/troubleshoot-recovery-disks-portal-windows/server-manager-select-storage.png)

3. Veri diski otomatik olarak algılanır ve eklenir. Bağlı disklerin bir listesini görmek için **diskler**' i seçin. Sürücü harfi dahil olmak üzere birim bilgilerini görüntülemek için veri diskinizi seçebilirsiniz. Aşağıdaki örnek, **F:** ile bağlı veri diskini gösterir.

    ![Sunucu Yöneticisi 'de disk bağlı ve birim bilgileri](./media/troubleshoot-recovery-disks-portal-windows/server-manager-disk-attached.png)


## <a name="fix-issues-on-original-virtual-hard-disk"></a>Özgün sanal sabit diskteki sorunları çözme
Var olan sanal sabit disk takılı olduğunda, artık gereken tüm bakım ve sorun giderme adımlarını gerçekleştirebilirsiniz. Sorunları giderdikten sonra aşağıdaki adımlarla devam edin.


## <a name="unmount-and-detach-original-virtual-hard-disk"></a>Özgün sanal sabit diski çıkarın ve ayırın
Hatalar çözümlendikten sonra, var olan sanal sabit diski sorun giderme sanal makinenizin dışında ayırın. Sanal sabit diski, sorun giderme VM 'sine iliştirene kadar sanal sabit diskinizi başka bir VM ile kullanamazsınız.

1. RDP oturumundan sanal makinenize **Sunucu Yöneticisi**açın **ve dosya ve depolama hizmetleri**' ni seçin:

    ![Sunucu Yöneticisi dosya ve depolama hizmetleri 'ni seçin](./media/troubleshoot-recovery-disks-portal-windows/server-manager-select-storage.png)

2. **Diskler** ' i seçin ve ardından veri diskinizi seçin. Veri diskinize sağ tıklayıp **Çevrimdışına Al**' ı seçin:

    ![Veri diskini Sunucu Yöneticisi çevrimdışı olarak ayarlama](./media/troubleshoot-recovery-disks-portal-windows/server-manager-set-disk-offline.png)

3. Artık sanal sabit diski VM 'den ayırın. Azure portal VM 'nizi seçin ve **diskler**' e tıklayın. 
4. **Düzenle**' yi seçin, eklediğiniz işletim sistemi diskini seçin ve **Ayır**' ı tıklatın:

    ![Var olan sanal sabit diski ayır](./media/troubleshoot-recovery-disks-portal-windows/detach-disk.png)

    Devam etmeden önce VM veri diskini başarıyla ayırana kadar bekleyin.

## <a name="create-vm-from-original-hard-disk"></a>Özgün sabit diskten VM oluşturma

### <a name="method-1-use-azure-resource-manager-template"></a>Yöntem 1 Azure Resource Manager şablonu kullan
Özgün sanal sabit diskinizden bir VM oluşturmak için [bu Azure Resource Manager şablonunu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-new-or-existing-vnet)kullanın. Şablon, önceki komuttan gelen VHD URL 'sini kullanarak var olan veya yeni bir sanal ağa bir VM dağıtır. **Azure 'A dağıt** düğmesine şu şekilde tıklayın:

![GitHub 'dan VM 'yi şablondan dağıtma](./media/troubleshoot-recovery-disks-portal-windows/deploy-template-from-github.png)

Şablon, dağıtım için Azure portal yüklenir. Yeni VM 'niz ve mevcut Azure kaynaklarınızın adlarını girin ve URL 'YI mevcut sanal sabit diskinize yapıştırın. Dağıtımı başlatmak için **satın al**' a tıklayın:

![Şablondan VM 'yi dağıtma](./media/troubleshoot-recovery-disks-portal-windows/deploy-from-image.png)

### <a name="method-2-create-a-vm-from-the-disk"></a>Yöntem 2 diskten VM oluşturma

1. Azure portal, portaldan kaynak grubunuzu seçin ve ardından işletim sistemi diskini bulun. Disk adını kullanarak da disk araması yapabilirsiniz:

    ![Azure portal disk ara](./media/troubleshoot-recovery-disks-portal-windows/search-disk.png)
1. **Genel bakış**' ı seçin ve ardından **VM oluştur**' u seçin.
    ![Azure portal diskten VM oluşturma](./media/troubleshoot-recovery-disks-portal-windows/create-vm-from-disk.png)
1. VM 'yi oluşturmak için Sihirbazı izleyin.

## <a name="re-enable-boot-diagnostics"></a>Önyükleme tanılamayı yeniden etkinleştirin
VM 'nizi var olan sanal sabit diskten oluşturduğunuzda, önyükleme tanılaması otomatik olarak etkinleştirilmemiş olabilir. Önyükleme tanılamaları durumunu denetlemek ve gerekirse etkinleştirmek için, portalda VM 'nizi seçin. **İzleme**altında **Tanılama ayarları**' na tıklayın. Durumun **Açık**olduğundan ve **önyükleme tanılaması** ' nın yanındaki onay işaretinin seçili olduğundan emin olun. Herhangi bir değişiklik yaparsanız **Kaydet**' e tıklayın:

![Önyükleme tanılama ayarlarını Güncelleştir](./media/troubleshoot-recovery-disks-portal-windows/reenable-boot-diagnostics.png)

## <a name="next-steps"></a>Sonraki adımlar
Sanal makinenize bağlanırken sorun yaşıyorsanız bkz. [Azure VM 'ye YÖNELIK RDP bağlantılarında sorun giderme](troubleshoot-rdp-connection.md). VM 'niz üzerinde çalışan uygulamalara erişme sorunları için bkz. [WINDOWS VM 'de uygulama bağlantı sorunlarını giderme](troubleshoot-app-connection.md).

Kaynak Yöneticisi kullanma hakkında daha fazla bilgi için bkz. [Azure Resource Manager genel bakış](../../azure-resource-manager/resource-group-overview.md).

