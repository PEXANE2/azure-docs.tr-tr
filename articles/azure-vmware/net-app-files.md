---
title: Azure VMware çözümü için NetApp dosyaları
description: Şirket içi sunucular, Azure VMware Çözüm VM 'Leri ve bulut altyapılarında veri geçirmek ve eşitlemek için Azure VMware VM 'leriyle Azure NetApp Files kullanın.
ms.topic: how-to
ms.date: 09/17/2020
ms.openlocfilehash: 0adf137db358d3561365a3a1ed2def338eeeb158
ms.sourcegitcommit: 7374b41bb1469f2e3ef119ffaf735f03f5fad484
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2020
ms.locfileid: "90709117"
---
# <a name="netapp-files-for-azure-vmware-solution"></a>Azure VMware çözümü için NetApp dosyaları

Bu makalede, Azure VMware Çözüm tabanlı iş yükleriyle Azure NetApp Files tümleştirme adımlarını adım adım inceleyeceğiz. [Azure NetApp Files](../azure-netapp-files/azure-netapp-files-introduction.md) , kurumsal dosya iş yüklerini kod değişikliği olmadan buluta geçirmek ve çalıştırmak Için bir Azure hizmetidir. Şirket içi ve bulut altyapılarında verilerin kolayca geçirilmesini mümkün kılar. Hızlı, gelişmiş güvenlik özelliklerine sahip veri eşitleme, anında anlık görüntü, geri yükleme ve Active Directory tümleştirme gibi yetenekler sayesinde geçiş ve DevOps senaryolarını basitleştirir.

## <a name="topology"></a>Topoloji

Bu senaryoda, Azure NetApp Files havuz paylaşımının test etmek ve doğrulanması için, Azure NetApp Files Azure 'da kapasite havuzu, birim havuzu ve alt ağ ile yapılandırılır. NFS protokolünü kullandık. Hem Azure NetApp Files hem de Azure VMware çözümü, Doğu ABD aynı Azure bölgesinde oluşturulur. Azure VMware çözümüne bağlantı Azure ExpressRoute aracılığıyla yapılır.

![Azure VMware Çözüm topolojisi için NetApp dosyaları.](media/net-app-files/net-app-files-topology.png)

## <a name="prerequisites"></a>Önkoşullar 

> [!div class="checklist"]
> * Azure NetApp Files ile Azure aboneliği
> * Azure NetApp dosyası için alt ağ
> * Azure VMware çözümünde Linux sanal makinesi
> * Azure VMware çözümünde Windows VM

## <a name="verify-configuration-of-azure-netapp-files"></a>Azure NetApp Files yapılandırmasını doğrulayın 

İlk olarak, Azure 'da oluşturulan Azure NetApp Files yapılandırmasını bir Premium diskte doğrulayın.

1. Azure portal **depolama**altında **Azure NetApp Files**' ı seçin. Yapılandırılmış Azure NetApp Files listesi gösterilir.

    :::image type="content" source="media/net-app-files/azure-net-app-files-list.png" alt-text="Azure NetApp Files listesi."::: 

2. Bir NetApp hesabı seçerseniz, çeşitli ayarları görüntüleyebilirsiniz. Örneğin, **contoso-anf2**' ı seçerseniz, ayarlarını görürsünüz. 

    :::image type="content" source="media/net-app-files/net-app-settings.png" alt-text="Bir NetApp hesabının ayarlarını görüntüleyin."::: 

3. Yapılandırılan havuzu doğrulamak için **Kapasite havuzlarını** seçin. 

    :::image type="content" source="media/net-app-files/capacity-pools.png" alt-text="Yapılandırılmış havuzu doğrulayın.":::

    Depolama havuzunun Premium diskle 4 TiB olarak yapılandırıldığını görebilirsiniz.

4. Kapasite havuzu altında oluşturulan birimleri görüntülemek için **birimleri** seçin (2. adımdaki ekran görüntüsüne bakın).  

    :::image type="content" source="media/net-app-files/azure-net-app-volumes.png" alt-text="Azure NetApp Files birim.":::

5. Yapılandırmasını görüntülemek için bir birim seçin.  

    :::image type="content" source="media/net-app-files/configuration-of-volume.png" alt-text="Bir birimin yapılandırma ayrıntıları.":::

    200 GiB ile anfpool birim havuzunun NetApp dosya paylaşımında oluşturulduğunu görebilirsiniz. Azure VMware Çözüm VM 'lerine bağlanmak üzere NFS yolunu sağlayan Azure NetApp Files için özel bir IP sanal ağı oluşturuldu. 

## <a name="protocols-supported-by-azure-netapp-files"></a>Azure NetApp Files tarafından desteklenen protokoller  

Azure NetApp Files, sunucu Ileti bloğu (SMB) ve ağ dosya sistemi (NFS) paylaşma eşlemesini destekler. 

- **SMB paylaşma**: bir SMB birimini dağıtmak için öncelikle bir Active Directory bağlantısı oluşturmanız gerekir. Başarılı bir bağlantı için, belirtilen etki alanı denetleyicilerinin Azure NetApp Files Temsilcili alt ağı tarafından erişilebilir olması gerekir. Active Directory Azure NetApp Files hesabında yapılandırıldıktan sonra, SMB birimleri oluşturulurken seçilebilir bir öğe olarak görünür. 

- **NFS paylaşma**: NFS (NFSv3 ve NFSv 4.1), SMBv3 veya ikili bir protokol (NFSV3 ve SMB) kullanarak birim oluşturmaya katkıda bulunur Azure NetApp Files. Birimin kapasite kullanımı, havuzunun sağlanan kapasitesinden sayılır. NFS, bir komut satırı kullanarak bir Linux sunucusuna bağlanabilir.

## <a name="create-azure-netapp-files"></a>Azure NetApp Files oluştur 

1. [Azure Portal](https://rc.portal.azure.com/#home)oturum açın. Azure Hizmetleri altında **Azure NetApp Files**' yi seçin. 

2. Yeni bir Azure NetApp Files birimi oluşturmak için **+ Ekle** ' yi seçin. 

3. Gerekli alanları (ad, abonelik, kaynak grubu ve konum) doldurarak **Oluştur**' u seçin. 

## <a name="add-capacity-pools-into-azure-netapp-files"></a>Kapasite havuzlarını Azure NetApp Files ekleyin 

1. Azure portal **Azure NetApp Files**' i seçin, **Kapasite havuzları** ' nı seçin ve **Havuz Ekle**' ye tıklayın. 

2. Birim havuzu adı, hizmet düzeyi ve disk boyutu (FQDN) veya IP ve ağırlık için gerekli ayrıntıları girin. **Ekle**’yi seçin.

    :::image type="content" source="media/net-app-files/add-capacity-pool.png" alt-text="NetApp dosyalarına kapasite havuzu ekleyin.":::

3. Kapasite havuzu altında birimler oluşturmak için, arama bölmesinden **birimler** ' i seçin ve **+ Birim Ekle**' yi seçin. 
 
4. Aşağıdaki ekran görüntüsünde gösterildiği gibi gerekli alanları girin.

    :::image type="content" source="media/net-app-files/create-a-volume.png" alt-text="Kapasite havuzunun altına birim ekleyin.":::

5. Sonraki sayfada protokol türü paylaşma ' yı seçin. NFS paylaşımınız varsa sürümleri seçin ve bir SMB paylaşımındaysa Active Directory etki alanını seçin.  

6. Bu bir NFS paylaşımındaysa, protokol türü paylaşımının eriştiği kaynak IP adresini ekleyin. **Gözden geçir ve oluştur**’u seçin. 

    :::image type="content" source="media/net-app-files/select-volume-details.png" alt-text="Toplu ayrıntıları seçin.":::
 
    Azure portal Azure NetApp Files altında, NFS paylaşımınız kullanıma hazırdır.

    :::image type="content" source="media/net-app-files/share-ready.png" alt-text="NFS paylaşma işlemi hazırlanıyor.":::

## <a name="mount-an-nfs-file-share-on-your-azure-vmware-solution-vms"></a>Azure VMware Çözüm sanal makinelerinize bir NFS dosya paylaşımının bağlama

### <a name="mount-an-nfs-file-share-on-an-azure-vmware-solution-windows-vm"></a>Azure VMware çözümünde bir NFS dosya paylaşımından bağlama Windows VM

- Bir komut istemi açın ve NFS dosya paylaşımından eşlemek için komutunu çalıştırın. Aşağıdaki ekran görüntülerinde, Azure VMware çözümünde bir Windows sanal makinesinde eşlenen paylaşılan sürücü gösterilmektedir.  

    :::image type="content" source="media/net-app-files/share-mapped-to-windows-vm.png" alt-text="Azure VMware çözümünde bir Windows sanal makinesinde eşlenen sürücüyü paylaşma.":::
 
    :::image type="content" source="media/net-app-files/mapped-to-windows-drive.png" alt-text="Windows VM 'de eşlenen sürücüyü paylaşma.":::

### <a name="mount-an-nfs-file-share-on-an-azure-vmware-solution-linux-vm"></a>Azure VMware çözümü Linux VM 'ye bir NFS dosya paylaşma bağlama

#### <a name="setting-up-your-azure-instance"></a>Azure örneğinizi ayarlama

1. Azure portal Azure örneğinizi, biriminizle aynı sanal ağda tanımlanan bir alt ağ ile ilişkilendirin.

    > [!NOTE]
    > Alt ağ, NFS bağlantı noktalarında (2049, 111), UDP ve TCP trafiğine izin veren bir ağ güvenlik grubu kuralına ihtiyaç duyuyor.

2. Bir SSH istemcisi açın ve Azure örneğinize bağlanın. Daha fazla bilgi için bkz. [Azure 'Da Windows Ile SSH anahtarlarını kullanma](../virtual-machines/linux/ssh-from-windows.md).

3. Azure örneğinize NFS istemcisini yükler:
   - Red Hat Enterprise Linux veya SUSE Linux örneğinde: `sudo yum install -y nfs-utils`
   - Ubuntu veya debir örneği üzerinde: `sudo apt-get install nfs-common` 

#### <a name="mounting-your-file-system"></a>Dosya sisteminizi bağlama

1. Azure örneğiniz üzerinde yeni bir dizin oluşturun: `sudo mkdir ANFPOOL`

2. Bir bağlama hedefi IP adresi seçin.

3. Dosya sisteminizi bağlayın: `sudo mount -t nfs -o rw,hard,rsize=1048576,wsize=1048576,vers=3,tcp 10.22.3.4:/ANFPOOL ANFPOOL`

    :::image type="content" source="media/net-app-files/root-test.png" alt-text="Kök testi.":::

## <a name="create-an-active-directory-connection-for-an-smb-share"></a>SMB paylaşımıyla ilgili Active Directory bağlantısı oluşturma

1. Azure portal bir NetApp hesabı seçin.

2. Azure NetApp Files altında **Active Directory bağlantılar**' ı seçin.

    :::image type="content" source="media/net-app-files/active-directory-connections.png" alt-text="Active Directory bağlantıları."::: 

3. Active Directory SMB paylaşımıyla birleştirmek için **Birleştir** ' i seçin. Aşağıdaki ekran görüntüsünde SMB paylaşımının etki alanı ayrıntıları gösterilmektedir.

    :::image type="content" source="media/net-app-files/active-directory-connect-details.png" alt-text="SMB paylaşımının etki alanı ayrıntıları."::: 

    Azure SMB dosya paylaşımınız kullanıma hazırlanıyor.  

    :::image type="content" source="media/net-app-files/smb-file-share-details.png" alt-text="SMB dosya paylaşma işlemi hazırlanıyor."::: 

4. SMB payını Microsoft Azure VMware Çözüm VM 'niz ile eşleyin. Önceki ekran görüntüsünde gösterildiği gibi SMB bağlama yolunu kullanın. Daha fazla bilgi için bkz. [Windows 10 ' da ağ sürücüsüne eşleme](https://support.microsoft.com/help/4026635/windows-10-map-a-network-drive)

## <a name="next-steps"></a>Sonraki adımlar
- [Azure NetApp Files depolama hiyerarşisi](../azure-netapp-files/azure-netapp-files-understand-storage-hierarchy.md)hakkında daha fazla bilgi edinin.
- Bkz. [Azure VMware Çözüm VM 'Lerinin yaşam döngüsü yönetimi](lifecycle-management-of-azure-vmware-solution-vms.md)
- Bkz. [Azure VMware çözümünü bir hub ve bağlı bileşen mimarisinde tümleştirme](concepts-avs-hub-and-spoke-integration.md)
