---
title: Premium dosya paylaşımı ile SQL Server FCI - Azure Sanal Makineler
description: Bu makalede, Azure sanal makinelerde bir premium dosya paylaşımı kullanarak bir SQL Server failover cluster örneğinin nasıl oluşturulutamamolduğu açıklanmaktadır.
services: virtual-machines
documentationCenter: na
author: MashaMSFT
editor: monicar
tags: azure-service-management
ms.assetid: 9fc761b1-21ad-4d79-bebc-a2f094ec214d
ms.service: virtual-machines-sql
ms.custom: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 10/09/2019
ms.author: mathoma
ms.openlocfilehash: 9595ee87801fa4ce187a50197fc58d6c448eac24
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78303231"
---
# <a name="configure-a-sql-server-failover-cluster-instance-with-premium-file-share-on-azure-virtual-machines"></a>Azure sanal makinelerde premium dosya paylaşımı yla sql server failover cluster örneğini yapılandırma

Bu makalede, [bir premium dosya paylaşımı](../../../storage/files/storage-how-to-create-premium-fileshare.md)kullanarak Azure sanal makinelerde sql server failover cluster instance (FCI) nasıl oluşturulacak açıklanmaktadır.

Premium dosya paylaşımları, SQL Server 2012 için Failover Cluster Örnekleri nde veya daha sonra Windows Server 2012 veya daha sonra kullanım için tamamen desteklenen SSD destekli, sürekli olarak düşük gecikmeli dosya paylaşımlarıdır. Premium dosya paylaşımları, herhangi bir kesinti olmaksızın bir dosya paylaşımını yeniden boyutlandırmanızı ve ölçeklendirmenize olanak sağlayarak size daha fazla esneklik sağlar.


## <a name="before-you-begin"></a>Başlamadan önce

Başlamadan önce bilmeniz gereken ve yerine sahip olmanız gereken birkaç şey vardır.

Bu teknolojiler hakkında operasyonel bir anlayışa sahip olmalısınız:

- [Windows küme teknolojileri](/windows-server/failover-clustering/failover-clustering-overview)
- [SQL Server Failover Küme Örnekleri](/sql/sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server)

Dikkat edilmesi gereken bir şey, bir Azure IaaS VM failover kümesinde sunucu başına tek bir NIC (küme düğümü) ve tek bir alt ağ öneririz. Azure ağ, bir Azure IaaS VM konuk kümesinde ek NIC'leri ve alt ağları gereksiz kılan fiziksel artıklığa sahiptir. Küme doğrulama raporu, düğümlere yalnızca tek bir ağda erişilebildiği konusunda sizi uyarır. Bu uyarıyı Azure IaaS VM başarısız kümelerinde yoksayabilirsiniz.

Ayrıca bu teknolojiler hakkında genel bir anlayışa sahip olmalısınız:

- [Azure premium dosya paylaşımı](../../../storage/files/storage-how-to-create-premium-fileshare.md)
- [Azure kaynak grupları](../../../azure-resource-manager/management/manage-resource-groups-portal.md)

> [!IMPORTANT]
> Şu anda, SQL Server azure sanal makinelerde küme örnekleri üzerinde başarısız sadece [SQL Server IaaS Agent Extension](virtual-machines-windows-sql-server-agent-extension.md) [hafif yönetim modu](virtual-machines-windows-sql-register-with-resource-provider.md#management-modes) ile desteklenir. Tam uzantı modundan hafifliğe geçiş yapmak için, ilgili VM'ler için **SQL Virtual Machine** kaynağını silin ve bunları hafif modda SQL VM kaynak sağlayıcısına kaydedin. Azure portalını kullanarak **SQL Virtual Machine** kaynağını silerken, doğru Sanal **Makine'nin yanındaki onay kutusunu temizleyin.** Tam uzantı, otomatik yedekleme, yama ve gelişmiş portal yönetimi gibi özellikleri destekler. Aracı hafif yönetim modunda yeniden yüklendikten sonra bu özellikler SQL VM'ler için çalışmaz.

Premium dosya paylaşımları IOPS ve birçok iş yükünün ihtiyaçlarını karşılayacak kapasiteler boyunca sağlar. IO yoğun iş yükleri için, yönetilen premium disklere veya ultra disklere dayalı olarak [SQL Server Failover Cluster Instances with Storage Spaces Direct'i](virtual-machines-windows-portal-sql-create-failover-cluster.md)düşünün.  

Ortamınızın IOPS etkinliğini kontrol edin ve bir dağıtım veya geçiş başlatmadan önce premium dosya paylaşımlarının ihtiyacınız olan IOPS'yi sağlayacağını doğrulayın. SQL Server Data, Log ve Temp DB dosyaları için gereken toplam IOPS (Disk Aktarımları/saniye) ve veri (Disk Bayt/saniye) izlemek için Windows Performance Monitor disk sayaçlarını kullanın.

Birçok iş yükü Patlama IO var, bu yüzden ağır kullanım dönemlerinde kontrol etmek ve maksimum IOPS ve ortalama IOPS hem de not iyi bir fikirdir. Premium dosya paylaşımları, hisse boyutuna göre IOPS sağlar. Premium dosya paylaşımları, IO'nuzu bir saate kadar temel tutarı üçkatına çıkarmak için ücretsiz patlama yapmanızı da sağlar.

Premium dosya paylaşımı performansı hakkında daha fazla bilgi için [Dosya paylaşımı performans katmanlarına](https://docs.microsoft.com/azure/storage/files/storage-files-planning#storage-tiers)bakın.

### <a name="licensing-and-pricing"></a>Lisanslama ve fiyatlandırma

Azure sanal makinelerinde, kullandıkça öde (PAYG) veya kendi lisansını getir (BYOL) VM görüntülerini kullanarak SQL Server'ı lisanslayabilirsiniz. Seçtiğiniz görüntü türü, nasıl ücretlendirildiğinizi etkiler.

Olabildiğince öde lisanslama ile, SQL Server'ın Azure sanal makinelerindeki başarısız küme örneği (FCI), pasif düğümler de dahil olmak üzere FCI'Nın tüm düğümleri için ücrete tabi dir. Daha fazla bilgi için [SQL Server Enterprise Sanal Makineler Fiyatlandırması'na](https://azure.microsoft.com/pricing/details/virtual-machines/sql-server-enterprise/)bakın.

Yazılım Güvencesi ile Kurumsal Sözleşmeniz varsa, her etkin düğüm için bir serbest pasif FCI düğümü kullanabilirsiniz. Azure'daki bu avantajdan yararlanmak için BYOL VM görüntülerini kullanın ve fci'nin hem etkin hem de pasif düğümlerinde aynı lisansı kullanın. Daha fazla bilgi için [Kurumsal Sözleşme'ye](https://www.microsoft.com/Licensing/licensing-programs/enterprise.aspx)bakın.

Azure sanal makinelerde SQL Server için gittikçe öde ve BYOL lisanslamayı karşılaştırmak için sql [vm'lerle başlayın'a](virtual-machines-windows-sql-server-iaas-overview.md#get-started-with-sql-vms)bakın.

SQL Server lisansı hakkında tam bilgi için [Fiyatlandırma'ya](https://www.microsoft.com/sql-server/sql-server-2017-pricing)bakın.

### <a name="filestream"></a>Dosya akışı

Filestream, premium dosya paylaşımı olan bir başarısız küme için desteklenmez. Filestream'i kullanmak için, [Doğrudan Depolama Alanları'nı](virtual-machines-windows-portal-sql-create-failover-cluster.md)kullanarak kümenizi dağıtın.

## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki adımları tamamlamadan önce aşağıdakileri zaten almış olmalısınız:

- Microsoft Azure aboneliği.
- Azure sanal makinelerinde bir Windows etki alanı.
- Hem Azure sanal makinelerinde hem de Active Directory'de nesne oluşturma izinlerine sahip bir etki alanı kullanıcı hesabı.
- SQL Server hizmetini çalıştırmak için bir etki alanı kullanıcı hesabı ve dosya paylaşımı montaj ile sanal makineye giriş yapabilirsiniz.  
- Bu bileşenler için yeterli IP adresi alanına sahip bir Azure sanal ağı ve alt ağı:
   - İki sanal makine.
   - Failover kümesi IP adresi.
   - Her FCI için bir IP adresi.
- DNS, etki alanı denetleyicilerini işaret ederek Azure ağında yapılandırıldı.
- Veri dosyalarınız için veritabanınızın depolama kotasına bağlı olarak kümelenmiş sürücü olarak kullanılacak [bir premium dosya paylaşımı.](../../../storage/files/storage-how-to-create-premium-fileshare.md)
- Windows Server 2012 R2 ve daha büyükseniz, bulut tanıkları Windows 2016 ve daha yeni için desteklendiklerinden, dosya paylaşımı tanığı olarak kullanmak için başka bir dosya paylaşımına ihtiyacınız olacaktır. Başka bir Azure dosya paylaşımı kullanabilir veya ayrı bir sanal makinede dosya paylaşımı kullanabilirsiniz. Başka bir Azure dosya paylaşımı kullanacaksanız, kümelenmiş sürücünüz için kullanılan premium dosya paylaşımıyla aynı işlemle monte edebilirsiniz. 

Bu ön koşullar yerine, üzerinde failover küme oluşturmaya başlayabilirsiniz. İlk adım sanal makineleri oluşturmaktır.

## <a name="step-1-create-the-virtual-machines"></a>Adım 1: Sanal makineleri oluşturun

1. Aboneliğiniz ile [Azure portalında](https://portal.azure.com) oturum açın.

1. [Azure kullanılabilirlik kümesi oluşturun.](../tutorial-availability-sets.md)

   Kullanılabilirlik, sanal makineleri hata etki alanları arasında gruplayı olarak ayarlar ve etki alanlarını günceller. Uygulamanızın ağ anahtarı veya bir sunucu rafının güç ünitesi gibi tek hata noktalarından etkilenmemesini sağlar.

   Sanal makineleriniz için kaynak grubunu oluşturmadıysanız, azure kullanılabilirlik kümesi oluştururken bunu yapın. Kullanılabilirlik kümesini oluşturmak için Azure portalını kullanıyorsanız aşağıdaki adımları izleyin:

   1. Azure portalında, Azure Marketi'ni açmak için **kaynak oluştur'u** seçin. **Kullanılabilirlik kümesini**ara.
   1. **Kullanılabilirlik Kümesi'ni**seçin.
   1. **Oluştur'u**seçin.
   1. **Kullanılabilirlik oluştur kümesi**altında, şu değerleri sağlayın:
      - **Adı**: Kullanılabilirlik kümesi için bir ad.
      - **Abonelik**: Azure aboneliğiniz.
      - **Kaynak grubu**: Varolan bir grubu kullanmak istiyorsanız, **varolan** grubu seçin'i tıklatın ve ardından listeden grubu seçin. Aksi takdirde, **yeni oluştur'u** seçin ve grup için bir ad girin.
      - **Konum**: Sanal makinelerinizi oluşturmayı planladığınız konumu ayarlayın.
      - **Hata etki alanları**: Varsayılanı kullanın (**3**).
      - **Etki alanlarını güncelleştir**: Varsayılanı kullanın (**5**).
   1. Kullanılabilirlik kümesini oluşturmak için **Oluştur'u** seçin.

1. Kullanılabilirlik kümesinde sanal makineleri oluşturun.

   Azure kullanılabilirlik kümesinde iki SQL Server sanal makinesi ni sağlama. Talimatlar için Azure [portalında SQL Server sanal makinesi sağlama](virtual-machines-windows-portal-sql-server-provision.md)konusuna bakın.

   Her iki sanal makineyi de yerleştirin:

   - Kullanılabilirlik kümenizle aynı Azure kaynak grubunda.
   - Etki alanı denetleyicinizle aynı ağda.
   - Hem sanal makineler hem de kümede kullanabileceğiniz tüm FCI'lar için yeterli IP adres alanına sahip bir alt ağda.
   - Azure kullanılabilirlik kümesinde.

      >[!IMPORTANT]
      >Sanal bir makine oluşturduktan sonra kullanılabilirlik kümesini ayarlayamaz veya değiştiremezsiniz.

   Azure Marketi'nden bir resim seçin. Windows Server ve SQL Server içeren bir Azure Marketi resmi veya yalnızca Windows Server içeren bir resim kullanabilirsiniz. Ayrıntılar için Azure [sanal makinelerdeki SQL Server'a Genel Bakış](virtual-machines-windows-sql-server-iaas-overview.md)bölümüne bakın.

   Azure Galerisi'ndeki resmi SQL Server görüntüleri yüklü bir SQL Server örneğini, SQL Server yükleme yazılımını ve gerekli anahtarı içerir.

   >[!IMPORTANT]
   > Sanal makineyi oluşturduktan sonra, önceden yüklenmiş bağımsız SQL Server örneğini kaldırın. Failover kümesini ve premium dosya paylaşımını depolama alanı olarak ayarladıktan sonra SQL Server FCI'ı oluşturmak için önceden yüklenmiş SQL Server ortamını kullanırsınız.

   Alternatif olarak, yalnızca işletim sistemi içeren Azure Marketi görüntülerini kullanabilirsiniz. Bir **Windows Server 2016 Datacenter** görüntüsünü seçin ve failover kümesini ve premium dosya paylaşımını depolama olarak ayarladıktan sonra SQL Server FCI'ı yükleyin. Bu resim SQL Server yükleme ortamı içermez. SQL Server yükleme ortamını her sunucu için çalıştırabileceğiniz bir konuma yerleştirin.

1. Azure sanal makinelerinizi oluşturduktan sonra RDP kullanarak her birine bağlanın.

   RDP kullanarak sanal bir makineye ilk bağlandığınızda, bir komut istemi bilgisayarın ağda bulunabilir olmasını isteyip istemediğinizi sorar. **Evet'i**seçin.

1. SQL Server tabanlı sanal makine görüntülerinden birini kullanıyorsanız, SQL Server örneğini kaldırın.

   1. **Programlar ve Özellikler'de** **Microsoft SQL Server 201_ (64 bit) seçeneğine** sağ tıklayın ve **Kaldır/Değiştir'i**seçin.
   1. **Kaldır**’ı seçin.
   1. Varsayılan örneği seçin.
   1. Veritabanı Altyapısı **Hizmetleri**altındaki tüm özellikleri kaldırın. **Paylaşılan Özellikleri**kaldırmayın. Aşağıdaki ekran görüntüsü gibi bir şey görürsünüz:

        ![Özellikleri Seçin](./media/virtual-machines-windows-portal-sql-create-failover-cluster/03-remove-features.png)

   1. **İleri'yi**seçin ve ardından **Kaldır'ı**seçin.

1. <span id="ports"> </span> Güvenlik duvarı bağlantı noktalarını açın.  

   Her sanal makinede, Windows Güvenlik Duvarı'ndaki şu bağlantı noktalarını açın:

   | Amaç | TCP bağlantı noktası | Notlar
   | ------ | ------ | ------
   | SQL Server | 1433 | SQL Server'ın varsayılan örnekleri için normal bağlantı noktası. Galeriden bir resim kullandıysanız, bu bağlantı noktası otomatik olarak açılır.
   | Durum yoklaması | 59999 | Herhangi bir açık TCP bağlantı noktası. Daha sonraki bir adımda, yük dengeleyici [sistem sondasını](#probe) ve kümeyi bu bağlantı noktasını kullanmak üzere yapılandırın.
   | Dosya paylaşımı | 445 | Dosya paylaşım hizmeti tarafından kullanılan bağlantı noktası.

1. [Sanal makineleri önceden varolan etki alanınıza ekleyin.](virtual-machines-windows-portal-sql-availability-group-prereq.md#joinDomain)

Sanal makineleri oluşturduktan ve yapılandırdıktan sonra, premium dosya paylaşımını yapılandırabilirsiniz.

## <a name="step-2-mount-the-premium-file-share"></a>Adım 2: Premium dosya paylaşımını monte edin

1. [Azure portalında](https://portal.azure.com) oturum açın ve depolama hesabınıza gidin.
1. **Dosya hizmeti** altında **Dosya Paylaşımları'na** gidin ve SQL depolama alanınız için kullanmak istediğiniz premium dosya paylaşımını seçin.
1. Dosya paylaşımınızın bağlantı dizesini getirmek için **Bağlan'ı** seçin.
1. Açılan listeden kullanmak istediğiniz sürücü harfini seçin ve ardından her iki kod bloğunu da Notepad'e kopyalayın.


   :::image type="content" source="media/virtual-machines-windows-portal-sql-create-failover-cluster-premium-file-share/premium-file-storage-commands.png" alt-text="Her iki PowerShell komutunu da dosya paylaşımı bağlantı portalından kopyalayın":::

1. SQL Server FCI'nizin hizmet hesabı için kullanacağı hesapla SQL Server VM'ye bağlanmak için RDP'yi kullanın.
1. Yönetimden powershell komut konsolu açın.
1. Portalda çalışırken daha önce kaydettiğiniz komutları çalıştırın.
1. Dosya Gezgini veya **Çalıştır** iletişim kutusunu (Windows logo tuşu + r) kullanarak paylaşıma gidin. Ağ yolunu `\\storageaccountname.file.core.windows.net\filesharename`kullanın. Örneğin, `\\sqlvmstorageaccount.file.core.windows.net\sqlpremiumfileshare`

1. SQL Data dosyalarınızı yerleştirmek için yeni bağlanan dosya paylaşımında en az bir klasör oluşturun.
1. Kümeye katılacak her SQL Server VM'de bu adımları yineleyin.

  > [!IMPORTANT]
  > - Veri ve Günlük dosyaları için Bu paylaşımın IOPS ve alan kapasitesini kaydetmek için yedekleme dosyaları için ayrı bir dosya paylaşımı kullanmayı düşünün. Yedekleme dosyaları için premium veya standart dosya paylaşımı kullanabilirsiniz.
  > - Windows 2012 R2 ve daha büyükseniz, dosya paylaşımı tanığı olarak kullanacağınız dosya paylaşımınızı takmak için aynı adımları izleyin. 

## <a name="step-3-configure-the-failover-cluster"></a>Adım 3: Failover kümesini yapılandırın

Bir sonraki adım, failover kümesini yapılandırmaktır. Bu adımda, aşağıdaki alt adımları tamamlarsınız:

1. Windows Server Failover Kümeleme özelliğini ekleyin.
1. Kümeyi doğrulayın.
1. Failover kümesini oluşturun.
1. Bulut tanığı (Windows Server 2016 ve daha yeni için) veya dosya paylaşım tanığı (Windows Server 2012 R2 ve daha büyük ler için) oluşturun.


### <a name="add-windows-server-failover-clustering"></a>Windows Server Failover Kümeleme ekleme

1. Yerel yöneticilerin üyesi olan ve Active Directory'de nesne oluşturma izni olan bir etki alanı hesabı kullanarak RDP'li ilk sanal makineye bağlanın. Yapılandırmanın geri kalanı için bu hesabı kullanın.

1. [Her sanal makineye Failover Kümeleme ekleyin.](virtual-machines-windows-portal-sql-availability-group-prereq.md#add-failover-clustering-features-to-both-sql-server-vms)

   UI'den Failover Clustering'i yüklemek için her iki sanal makinede de şu adımları izleyin:
   1. **Sunucu Yöneticisi'nde**Yönet'i **ve**ardından Rol ve **Özellikler Ekle'yi**seçin.
   1. Rolleri **ve Özellikleri Ekle Sihirbazı'nda,** **Özellikleri Seç'e**gelene kadar **İleri'yi** seçin.
   1. **Özellikleri Seç'te** **Failover Kümeleme'yi**seçin. Gerekli tüm özellikleri ve yönetim araçlarını ekleyin. **Özellikler Ekle'yi**seçin.
   1. **Sonraki'ni**seçin ve ardından özellikleri yüklemek için **Bitir'i** seçin.

   PowerShell'i kullanarak Failover Clustering'i yüklemek için, bir yönetici PowerShell oturumundan aşağıdaki komut dosyasını sanal makinelerden birinde çalıştırın:

   ```powershell
   $nodes = ("<node1>","<node2>")
   Invoke-Command  $nodes {Install-WindowsFeature Failover-Clustering -IncludeAllSubFeature -IncludeManagementTools}
   ```

### <a name="validate-the-cluster"></a>Kümeyi doğrulama

B)'deki kümeyi ui'de veya PowerShell kullanarak doğrulayın.

UI kullanarak kümedoğrulamak için sanal makinelerden biri üzerinde aşağıdaki adımları izleyin:

1. **Server Manager**altında, **Araçlar'ı**seçin ve ardından **Failover Cluster Manager'ı**seçin.
1. **Failover Cluster Manager**altında, **Eylem'i**seçin ve ardından **Yapılandırmayı Doğrula'yı**seçin.
1. **Sonraki'ni**seçin.
1. **Sunucuları Seçin veya Küme**altında, her iki sanal makinenin adlarını girin.
1. **Test seçenekleri**altında, **seçtiğim yalnızca testleri çalıştır'ı seçin.** **Sonraki'ni**seçin.
1. **Test Seçimi**altında, burada gösterildiği gibi, **Depolama** ve Depolama **Alanları Doğrudan**dışında tüm testleri seçin:

   :::image type="content" source="media/virtual-machines-windows-portal-sql-create-failover-cluster-premium-file-share/cluster-validation.png" alt-text="Küme doğrulama testlerini seçin":::

1. **Sonraki'ni**seçin.
1. **Onay**altında , **İleri'yi**seçin.

**Yapılandırmayı Doğrula Sihirbazı** doğrulama testlerini çalıştırın.

PowerShell'i kullanarak kümeyi doğrulamak için, sanal makinelerden birinde yönetici PowerShell oturumundan aşağıdaki komut dosyasını çalıştırın:

   ```powershell
   Test-Cluster –Node ("<node1>","<node2>") –Include "Inventory", "Network", "System Configuration"
   ```

Kümeyi doğruladıktan sonra, başarısız kümeyi oluşturun.

### <a name="create-the-failover-cluster"></a>Yük devretme kümesini oluşturma

Failover kümesini oluşturmak için şunları yapmanız gerekir:
- Küme düğümleri olacak sanal makinelerin adları.
- Failover kümesi için bir ad
- Failover kümesi için bir IP adresi. Küme düğümleri ile aynı Azure sanal ağında ve alt ağında kullanılmayan bir IP adresi kullanabilirsiniz.

#### <a name="windows-server-2012-through-windows-server-2016"></a>Windows Server 2012 ile Windows Server 2016

Aşağıdaki PowerShell komut dosyası, Windows Server 2016 üzerinden Windows Server 2012 için bir başarısız küme oluşturur. Komut dosyasını düğümlerin (sanal makine adları) adlarıyla ve Azure sanal ağındaki kullanılabilir BIR IP adresiyle güncelleştirin.

```powershell
New-Cluster -Name <FailoverCluster-Name> -Node ("<node1>","<node2>") –StaticAddress <n.n.n.n> -NoStorage
```   

#### <a name="windows-server-2019"></a>Windows Server 2019

Aşağıdaki PowerShell komut dosyası, Windows Server 2019 için bir başarısız küme oluşturur. Daha fazla bilgi için [bkz: Failover kümesi: Cluster Network Object](https://blogs.windows.com/windowsexperience/2018/08/14/announcing-windows-server-2019-insider-preview-build-17733/#W0YAxO8BfwBRbkzG.97). Komut dosyasını düğümlerin (sanal makine adları) adlarıyla ve Azure sanal ağındaki kullanılabilir BIR IP adresiyle güncelleştirin.

```powershell
New-Cluster -Name <FailoverCluster-Name> -Node ("<node1>","<node2>") –StaticAddress <n.n.n.n> -NoStorage -ManagementPointNetworkType Singleton 
```


### <a name="create-a-cloud-witness-win-2016-"></a>Bulut tanığı oluşturma (Win 2016 +)

Windows Server 2016 ve daha büyük bir ondaysanız, bir Bulut Tanığı oluşturmanız gerekir. Cloud Witness, Azure depolama blob'unda depolanan yeni bir küme çoğunluk tanığı türüdür. Bu, tanık paylaşımı barındıran veya ayrı bir dosya paylaşımı kullanan ayrı bir VM gereksinimini ortadan kaldırır.

1. [Failover kümesi için bir bulut tanığı oluşturun.](https://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness)

1. Bir blob kapsayıcı oluşturun.

1. Erişim anahtarlarını ve konteyner URL'sini kaydedin.

### <a name="configure-quorum"></a>Yeterlik yapılandırılması 

Windows Server 2016 ve üzeri için, kümeyi yeni oluşturduğunuz bulut tanığını kullanacak şekilde yapılandırın. Tüm adımları izleyin [Kullanıcı arabiriminde çoğunluk tanık yapılandırmak.](https://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness#to-configure-cloud-witness-as-a-quorum-witness)

Windows Server 2012 R2 ve daha büyük leri için, [kullanıcı arabirimindeki çoğunluk tanığını yapılandırmada](https://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness#to-configure-cloud-witness-as-a-quorum-witness) aynı adımları izleyin, ancak **Quorum Witness'ı seç** sayfasında, dosya paylaşımı tanığı seçeneğini **yapılandır'ı** seçin. İster ayrı bir sanal makinede yapılandırdığınız ister Azure'dan monte edilmiş olsun, dosya paylaşım tanığı olarak ayırdığınız dosya paylaşımını belirtin. 


## <a name="step-4-test-cluster-failover"></a>Adım 4: Test kümesi başarısız

Kümenizin başarısız lığı test edin. **Failover Cluster**Manager'da, kümenizi sağ tıklatın ve **Daha Fazla Eylem** > **Taşı Çekirdek Küme Kaynak** > **Seç düğümünü**seçin ve ardından kümenin diğer düğümünü seçin. Çekirdek küme kaynağını kümenin her düğümüne taşıyın ve sonra birincil düğüme geri taşıyın. Kümeyi her düğüme başarıyla taşıyabiliyorsanız, SQL Server'ı yüklemeye hazırsınız.  

:::image type="content" source="media/virtual-machines-windows-portal-sql-create-failover-cluster-premium-file-share/test-cluster-failover.png" alt-text="Çekirdek kaynağı diğer düğümlere taşıyarak küme nin başarısız olmasını test etme":::

## <a name="step-5-create-the-sql-server-fci"></a>Adım 5: SQL Server FCI oluşturun

Failover kümesini yapılandırdıktan sonra SQL Server FCI'ı oluşturabilirsiniz.

1. RDP kullanarak ilk sanal makineye bağlanın.

1. **Failover Cluster**Manager'da, tüm Çekirdek Küme Kaynaklarının ilk sanal makinede olduğundan emin olun. Gerekirse, tüm kaynakları bu sanal makineye taşıyın.

1. Yükleme ortamını bulun. Sanal makine Azure Marketi görüntülerinden birini kullanıyorsa, `C:\SQLServer_<version number>_Full`ortam . **Kurulum'u**seçin.

1. SQL **Server Yükleme Merkezi'nde** **Yükleme'yi**seçin.

1. **Küme yüklemesi üzerinden Yeni SQL Server failover'ı**seçin. SQL Server FCI'ı yüklemek için sihirbazdaki yönergeleri izleyin.

   FCI veri dizinlerinin premium dosya paylaşımında olması gerekir. Bu formda, paylaşımın tam yolunu `\\storageaccountname.file.core.windows.net\filesharename\foldername`girin: . Bir uyarı görünür ve bir dosya sunucusunu veri dizini olarak belirttiğinizi söyler. Bu uyarı bekleniyor. Dosya paylaşımında ısrar ettiğinizde VM'ye rdp'd kullanıcı hesabının, olası hataları önlemek için SQL Server hizmetinin kullandığı hesapla aynı olduğundan emin olun.

   :::image type="content" source="media/virtual-machines-windows-portal-sql-create-failover-cluster-premium-file-share/use-file-share-as-data-directories.png" alt-text="Dosya paylaşımını SQL veri dizinleri olarak kullanma":::

1. Sihirbazdaki adımları tamamladıktan sonra, Kurulum ilk düğüme bir SQL Server FCI yükler.

1. Kurulum FCI'ı ilk düğüme yükledikten sonra RDP kullanarak ikinci düğüme bağlanın.

1. SQL **Server Yükleme Merkezi'ni**açın. **Yükleme'yi**seçin.

1. **SQL Server failover kümesine düğüm ekle'yi**seçin. SQL Server'ı yüklemek ve sunucuyu FCI'a eklemek için sihirbazdaki yönergeleri izleyin.

   >[!NOTE]
   >SQL Server ile bir Azure Marketi galerisi resmi kullandıysanız, görüntüye SQL Server araçları eklenmiştir. Bu resimlerden birini kullanmadıysanız, SQL Server araçlarını ayrı olarak yükleyin. Bkz. [SQL Server Management Studio (SSMS) indirin.](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="step-6-create-the-azure-load-balancer"></a>Adım 6: Azure yük bakiyesini oluşturun

Azure sanal makinelerinde kümeler, aynı anda tek küme düğümünde olması gereken bir IP adresini tutmak için bir yük dengeleyicisi kullanır. Bu çözümde, yük bakiyeleyici sql server FCI için IP adresini tutar.

Daha fazla bilgi için azure [yük bakiyesi oluştur ve yapılandırma](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer)konusuna bakın.

### <a name="create-the-load-balancer-in-the-azure-portal"></a>Azure portalında yük bakiyesini oluşturma

Yük dengeleyicisini oluşturmak için:

1. Azure portalında, sanal makineleri içeren kaynak grubuna gidin.

1. **Ekle'yi**seçin. **Yük Dengeleyicisi**için Azure Marketi'nde arama yapın. **Yük Dengeleyici'ni**seçin.

1. **Oluştur'u**seçin.

1. Aşağıdaki değerleri kullanarak yük dengeleyicisini ayarlayın:

   - **Abonelik**: Azure aboneliğiniz.
   - **Kaynak grubu**: Sanal makinelerinizi içeren kaynak grubudur.
   - **Adı**: Yük dengeleyicisini tanımlayan ad.
   - **Bölge**: Sanal makinelerinizi içeren Azure konumu.
   - **Türü**: Ya kamu ya da özel. Özel yük dengeleyicisi sanal ağ içinden erişilebilir. Çoğu Azure uygulaması özel yük dengeleyicisi kullanabilir. Uygulamanızın doğrudan internet üzerinden SQL Server'a erişmesi gerekiyorsa, ortak yük dengeleyicikullanın.
   - **SKU**: Standart.
   - **Sanal ağ**: Sanal makinelerle aynı ağdır.
   - **IP adresi atama**: Statik. 
   - **Özel IP adresi**: SQL Server FCI küme ağ kaynağına atadığınız IP adresi.

   Aşağıdaki resimde **Yük dengeleyicisi oluştur** ui gösterilmektedir:

   ![Yük dengeleyicisini ayarlama](./media/virtual-machines-windows-portal-sql-create-failover-cluster/30-load-balancer-create.png)
   

### <a name="configure-the-load-balancer-backend-pool"></a>Yük dengeleyici arka uç havuzunu yapılandırma

1. Sanal makineleri içeren Azure kaynak grubuna dönün ve yeni yük dengeleyicisini bulun. Kaynak grubundaki görünümü yenilemeniz gerekebilir. Yük dengeleyicisini seçin.

1. **Arka uç havuzlarını**seçin ve sonra **Ekle'yi**seçin.

1. Arka uç havuzunu VM'leri içeren kullanılabilirlik kümesiyle ilişkilendirin.

1. **Hedef ağ IP yapılandırmaları**altında VIRTUAL **MACHINE'i** seçin ve küme düğümleri olarak katılacak sanal makineleri seçin. FCI'a ev sahipliği yapacak tüm sanal makineleri dahil edin.

1. Arka uç havuzunu oluşturmak için **Tamam'ı** seçin.

### <a name="configure-a-load-balancer-health-probe"></a>Yük dengeleyici durum araştırması yapılandırma

1. Yük dengeleyici **bıçağında, Sağlık sondalarını**seçin.

1. **Ekle'yi**seçin.

1. Ekle **sağlık sondası** bıçağında, <span id="probe"> </span> aşağıdaki sağlık sondası parametrelerini ayarlayın.

   - **Adı**: Sağlık sondası için bir ad.
   - **Protokol**: TCP.
   - **Bağlantı Noktası**: [Bu adımda](#ports)sağlık sondası için güvenlik duvarında oluşturduğunuz bağlantı noktası. Bu makalede, örnek TCP `59999`bağlantı noktasını kullanır.
   - **Aralık**: 5 Saniye.
   - **Sağlıksız eşik**: 2 ardışık başarısızlık.

1. **Tamam'ı**seçin.

### <a name="set-load-balancing-rules"></a>Yük dengeleme kurallarını ayarlama

1. Yük dengeleyici bıçağında **Yük dengeleme kurallarını**seçin.

1. **Ekle'yi**seçin.

1. Yük dengeleme kuralı parametrelerini ayarlayın:

   - **Adı**: Yük dengeleme kurallarının adı.
   - **Frontend IP adresi**: SQL Server FCI küme ağ kaynağının IP adresi.
   - **Bağlantı Noktası**: SQL Server FCI TCP bağlantı noktası. Varsayılan örnek bağlantı noktası 1433'dür.
   - **Backend portu**: **Kayan IP'yi (doğrudan sunucu iadesi)** etkinleştirdiğinizde **Bağlantı Noktası** değeriyle aynı bağlantı noktasını kullanır.
   - **Arka uç havuzu**: Daha önce yapılandırdığınız arka uç havuzu adı.
   - **Sağlık sondası**: Daha önce yapılandırdığınız sağlık sondası.
   - **Oturum kalıcılığı**: Yok.
   - **Boşta zaman /dakika (dakika)**: 4.
   - **Kayan IP (doğrudan sunucu iadesi)**: Etkin.

1. **Tamam'ı**seçin.

## <a name="step-7-configure-the-cluster-for-the-probe"></a>Adım 7: Sonda için kümeyi yapılandır

PowerShell'de küme sondası bağlantı noktası parametresini ayarlayın.

Küme sondası bağlantı noktası parametresini ayarlamak için, aşağıdaki komut dosyasındaki değişkenleri ortamınızdaki değerlerle güncelleştirin. Açı braketlerini`<` ( `>`ve ) komut dosyasından çıkarın.

   ```powershell
   $ClusterNetworkName = "<Cluster Network Name>"
   $IPResourceName = "<SQL Server FCI IP Address Resource Name>" 
   $ILBIP = "<n.n.n.n>" 
   [int]$ProbePort = <nnnnn>

   Import-Module FailoverClusters

   Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```

Aşağıdaki liste, güncelleştirmeniz gereken değerleri açıklar:

   - `<Cluster Network Name>`: Ağ için Windows Server Failover Cluster adı. **Failover Cluster Manager** > **Networks'te**ağa sağ tıklayın ve **Özellikler'i**seçin. Doğru değer **Genel** sekmesinde **Ad'ın** altındadır.

   - `<SQL Server FCI IP Address Resource Name>`: SQL Server FCI IP adresi kaynak adı. **Failover Cluster Manager** > **Roles'da,** SQL Server FCI rolü altında, Sunucu **Adı**altında, IP adresi kaynağına sağ tıklayın ve **Özellikleri**seçin. Doğru değer **Genel** sekmesinde **Ad'ın** altındadır.

   - `<ILBIP>`: ILB IP adresi. Bu adres, Azure portalında ILB ön uç adresi olarak yapılandırılır. Bu aynı zamanda SQL Server FCI IP adresidir. **Failover Cluster Manager'da** `<SQL Server FCI IP Address Resource Name>`bulabilirsiniz.  

   - `<nnnnn>`: Yük dengeleyici sağlık probunda yapılandırdığınız prob bağlantı noktası. Kullanılmayan TCP bağlantı noktası geçerlidir.

>[!IMPORTANT]
>Küme parametresi için alt ağ maskesi TCP IP `255.255.255.255`yayın adresi olmalıdır: .

Küme sondasını ayarladıktan sonra PowerShell'deki tüm küme parametrelerini görebilirsiniz. Bu komut dosyalarını çalıştırın:

   ```powershell
   Get-ClusterResource $IPResourceName | Get-ClusterParameter
  ```

## <a name="step-8-test-fci-failover"></a>Adım 8: Test FCI failover

Küme işlevselliğini doğrulamak için FCI'ın başarısız olmasını test edin. Aşağıdaki adımları uygulayın:

1. RDP kullanarak SQL Server FCI küme düğümlerinden birine bağlanın.

1. **Failover Cluster Yöneticisi'ni**aç. **Roller'i**seçin. Hangi düğümün SQL Server FCI rolüne sahip olduğunu fark edin.

1. SQL Server FCI rolünü sağ tıklatın.

1. **Taşı'yı**seçin ve ardından **Mümkün Olan En İyi Düğümü**seçin.

**Failover Cluster Manager** rolü gösterir ve kaynakları çevrimdışı olur. Kaynaklar daha sonra hareket ve diğer düğümde çevrimiçi geri gelir.

### <a name="test-connectivity"></a>Bağlantıyı test etme

Bağlantıyı sınamak için, aynı sanal ağdaki başka bir sanal makinede oturum açın. **SQL Server Management Studio'yu** açın ve SQL Server FCI adına bağlanın.

>[!NOTE]
>Gerekirse [SQL Server Management Studio'yı indirebilirsiniz.](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="limitations"></a>Sınırlamalar

Azure sanal makineleri, Windows Server 2019'da Microsoft Distributed Transaction Coordinator 'i (MSDTC) Kümelenmiş Paylaşılan Birimler (CSV) üzerinde depolama alanı ve [standart yük dengeleyicisiyle](../../../load-balancer/load-balancer-standard-overview.md)destekler.

Azure sanal makinelerinde MSDTC, Windows Server 2016 veya daha önceki makinelerde desteklenmez, çünkü:

- Kümelenmiş MSDTC kaynağı paylaşılan depolamayı kullanacak şekilde yapılandırılamaz. Windows Server 2016'da, bir MSDTC kaynağı oluşturursanız, depolama alanı olsa bile kullanılabilir paylaşılan depolama alanı gösterilmez. Bu sorun Windows Server 2019'da giderilmiştir.
- Temel yük dengeleyicisi RPC bağlantı noktalarını işlemez.

## <a name="see-also"></a>Ayrıca bkz.

- [Windows küme teknolojileri](/windows-server/failover-clustering/failover-clustering-overview)
- [SQL Server Failover Küme Örnekleri](/sql/sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server)
