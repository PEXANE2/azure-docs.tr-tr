---
title: Azure Site Kurtarma ile VMware/fiziksel sunucular için Mobilite aracısını yönetme
description: Azure Site Kurtarma hizmetini kullanarak VMware VM'lerinin ve fiziksel sunucuların Azure'a olağanüstü kurtarma durumu için Mobilite Hizmeti aracısını yönetin.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 03/25/2019
ms.author: ramamill
ms.openlocfilehash: 9be758c286e072b0fbefc5f8b20b7accc4e6741b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79256971"
---
# <a name="manage-the-mobility-agent"></a>Mobility aracısını yönetme 

VMware VM'lerinin ve fiziksel sunucuların Azure'da olağanüstü kurtarma durumu kurtarması için Azure Site Kurtarma'yı kullandığınızda sunucunuzda mobilite aracısı ayarlarsınız. Mobilite aracısı, korumalı makineniz, yapılandırma sunucusu/ölçeklendirme işlem sunucunuz arasındaki iletişimi koordine eder ve veri çoğaltmaişlemini yönetir. Bu makalede, dağıtıldıktan sonra mobilite aracıyönetmek için ortak görevleri özetlenir.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="update-mobility-service-from-azure-portal"></a>Azure portalından mobilite hizmetini güncelleştirme

1. Başlamadan önce, korumalı makinelerde Mobilite Hizmetini güncelleştirmeden önce yapılandırma sunucusunun, ölçeklendirme işlem sunucularının ve dağıtımınızın bir parçası olan ana hedef sunucuların güncelleştirilmesini sağlayın.
2. Portalda **kopyalanmış öğeleri**> tonoz açın.
3. Yapılandırma sunucusu en son sürümse, "Yeni Site kurtarma çoğaltma aracısı güncelleştirmesi kullanılabilir" başlıklı bir bildirim görürsünüz. Yüklemek için tıklayın."

     ![Çoğaltılan öğeler penceresi](./media/vmware-azure-install-mobility-service/replicated-item-notif.png)

4. Bildirimi tıklatın ve **Aracı güncelleştirmesinde**Mobilite hizmetini yükseltmek istediğiniz makineleri seçin. Ardından **Tamam**'a tıklayın.

     ![Çoğaltılan öğeler VM listesi](./media/vmware-azure-install-mobility-service/update-okpng.png)

5. Seçilen makinelerin her biri için Mobilite Hizmeti Güncelleştirme işi başlar.

## <a name="update-mobility-service-through-powershell-script-on-windows-server"></a>Windows sunucusundaki powershell komut dosyası aracılığıyla Mobilite hizmetini güncelleştirme

Başlamadan önce, korumalı makinelerde Mobilite Hizmetini güncelleştirmeden önce yapılandırma sunucusunun, ölçeklendirme işlem sunucularının ve dağıtımınızın bir parçası olan ana hedef sunucuların güncelleştirilmesini sağlayın.

Power shell cmdlet ile sunucudaki mobilite hizmetini yükseltmek için aşağıdaki komut dosyasını kullanın

```azurepowershell
Update-AzRecoveryServicesAsrMobilityService -ReplicationProtectedItem $rpi -Account $fabric.fabricSpecificDetails.RunAsAccounts[0]
```

## <a name="update-mobility-service-manually-on-each-protected-server"></a>Korumalı her sunucuda mobilite hizmetini el ile güncelleştirin

1. Başlamadan önce, korumalı makinelerde Mobilite Hizmetini güncelleştirmeden önce yapılandırma sunucusunun, ölçeklendirme işlem sunucularının ve dağıtımınızın bir parçası olan ana hedef sunucuların güncelleştirilmesini sağlayın.

2. [Aracı yıyükleyicisini sunucunun](vmware-physical-mobility-service-overview.md#locate-installer-files) işletim sistemine göre bulun.

>[!IMPORTANT]
> Azure IaaS VM'yi bir Azure bölgesinden diğerine çoğaltırsanız, bu yöntemi kullanmayın. Mevcut tüm seçenekler hakkında bilgi almak için [kılavuzumuza](azure-to-azure-autoupdate.md) bakın.

3. Yükleme dosyasını korumalı makineye kopyalayın ve hareket aracısını güncelleştirmek için çalıştırın.

## <a name="update-account-used-for-push-installation-of-mobility-service"></a>Mobilit hizmetinin push kurulumu için kullanılan güncelleme hesabı

Mobilit hizmetinin itme yüklemesini etkinleştirmek için Site Kurtarma'yı dağıttığınızda, Site Kurtarma işlem sunucusunun makinelere erişmek ve makine için çoğaltma etkinleştirildiğinde hizmeti yüklemek için kullandığı bir hesap belirlemişsiniz. Bu hesabın kimlik bilgilerini güncelleştirmek istiyorsanız, [aşağıdaki yönergeleri](vmware-azure-manage-configuration-server.md#modify-credentials-for-mobility-service-installation)izleyin.

## <a name="uninstall-mobility-service"></a>Mobilite hizmetini kaldır

### <a name="on-a-windows-machine"></a>Windows makinesinde

UI'den veya komut isteminden kaldırın.

- **Kullanıcı Arabirimi**: Makinenin Kontrol Panelinde **Programlar'ı**seçin. **Microsoft Azure Site Kurtarma Mobilite Hizmeti/Ana Hedef sunucu** > **Kaldır'ı**seçin.
- **Komut isteminden**: Makinede yönetici olarak komut istemi penceresini açın. Şu komutu çalıştırın: 
    ```
    MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
    ```

### <a name="on-a-linux-machine"></a>Linux makinesinde
1. Linux **makinesinde, kök** kullanıcı olarak oturum açın.
2. Bir terminalde /usr/local/ASR'ye gidin.
3. Şu komutu çalıştırın:
    ```
    uninstall.sh -Y
   ```
   
## <a name="install-site-recovery-vss-provider-on-source-machine"></a>Site Kurtarma VSS sağlayıcısını kaynak makinesine yükleme

Uygulama tutarlılık noktaları oluşturmak için kaynak makinede Azure Site Kurtarma VSS sağlayıcısı gereklidir. Sağlayıcının yüklenmesi itme yüklemesi ile başarılı olmadıysa, el ile yüklemek için aşağıdaki yönergeleri izleyin.

1. Admin cmd pencereyi açın.
2. Mobilite hizmeti yükleme konumuna gidin. (Örneğin - C:\Program Dosyaları (x86)\Microsoft Azure Site Kurtarma\agent)
3. Komut dosyası InMageVSSProvider_Uninstall.cmd çalıştırın. Bu, zaten varsa hizmeti kaldıracaktır.
4. VSS sağlayıcısını el ile yüklemek için komut InMageVSSProvider_Install.cmd komut dosyasını çalıştırın.

## <a name="next-steps"></a>Sonraki adımlar

- [VMware VM'ler için olağanüstü durum kurtarma yı ayarlama](vmware-azure-tutorial.md)
- [Fiziksel sunucular için olağanüstü durum kurtarma yı ayarlama](physical-azure-disaster-recovery.md)
