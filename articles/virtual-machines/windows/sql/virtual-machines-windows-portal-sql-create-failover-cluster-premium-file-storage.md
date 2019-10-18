---
title: Premium dosya paylaşımıyla SQL Server FCı-Azure sanal makineleri
description: Bu makalede, Azure sanal makinelerinde Premium dosya paylaşma kullanılarak SQL Server yük devretme kümesi örneği oluşturma işlemi açıklanmaktadır.
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
ms.openlocfilehash: b281344084cb558ab490e9e3c24774311ede7866
ms.sourcegitcommit: f29fec8ec945921cc3a89a6e7086127cc1bc1759
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72529425"
---
# <a name="configure-sql-server-failover-cluster-instance-with-premium-file-share-on-azure-virtual-machines"></a>Azure sanal makinelerinde Premium dosya paylaşımıyla SQL Server yük devretme kümesi örneğini yapılandırma

Bu makalede, [Premium dosya paylaşımının](../../../storage/files/storage-how-to-create-premium-fileshare.md)kullanıldığı Azure sanal makinelerinde SQL Server yük devretme kümesi örneği (FCI) oluşturma işlemi açıklanmaktadır. 

Premium dosya paylaşımları, Windows Server 2012 ve daha yeni bir sürüm olan SQL Server 2012 ve üzeri için yük devretme kümesi örneğiyle birlikte kullanılmak üzere tam olarak desteklenen SSD destekli tutarlı dosya paylaşımlardır. Premium dosya paylaşımları size daha fazla esneklik sağlar. böylece, herhangi bir kesinti olmadan dosya paylaşımını yeniden boyutlandırabilir ve ölçeklendirebilirsiniz. 


## <a name="before-you-begin"></a>Başlamadan önce

Bilmeniz gereken birkaç nokta ve devam etmeden önce ihtiyacınız olan birkaç şey vardır.

Aşağıdaki teknolojilerde işlemsel olarak anlaşılmalıdır:

- [Windows küme teknolojileri](/windows-server/failover-clustering/failover-clustering-overview)
- [Yük devretme kümesi örnekleri SQL Server](/sql/sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server).

Önemli bir farklılık, bir Azure IaaS VM yük devretme kümesinde, sunucu başına (küme düğümü) ve tek bir alt ağ için tek bir NIC önerilir. Azure ağ iletişimi, Azure IaaS VM Konuk kümesinde ek NIC 'Lerin ve alt ağların gereksiz olmasını sağlayan fiziksel yedekliliğe sahiptir. Küme doğrulama raporu, düğümlerin yalnızca tek bir ağda ulaşılabilir olduğunu bildiren bir uyarı verebilir, ancak bu uyarı Azure IaaS VM yük devretme kümelerinde güvenle yoksayılabilir. 

Ayrıca, aşağıdaki teknolojilerde genel olarak anlaşılmalıdır:

- [Azure Premium dosya paylaşma](../../../storage/files/storage-how-to-create-premium-fileshare.md)
- [Azure Kaynak grupları](../../../azure-resource-manager/manage-resource-groups-portal.md)

> [!IMPORTANT]
> Şu anda Azure sanal makinelerinde SQL Server yük devretme kümesi örnekleri yalnızca [SQL Server IaaS aracı uzantısının](virtual-machines-windows-sql-server-agent-extension.md) [basit](virtual-machines-windows-sql-register-with-resource-provider.md#register-with-sql-vm-resource-provider) yönetim moduyla desteklenir. Tam uzantı modundan hafif olarak geçiş yapmak için, sanal makinelerin "SQL sanal makinesi" kaynağını silin ve ardından bunları `lightweight` modunda SQL VM kaynak sağlayıcısı 'na kaydedin. Azure portal kullanarak "SQL sanal makinesi" kaynağı silinirken gerçek sanal makinenin seçimini kaldırın. Tam uzantı otomatik yedekleme, düzeltme eki uygulama ve gelişmiş Portal yönetimi gibi özellikleri destekler. Aracı hafif yönetim modunda yeniden yüklendikten sonra bu özellikler SQL VM 'Leri için çalışmaz.

### <a name="workload-consideration"></a>İş yükü değerlendirmesi

Premium dosya paylaşımları, ıOPS 'yi ve çok sayıda iş yükünün ihtiyaçlarını karşılayacak kapasiteyi sağlar. Ancak, GÇ yoğun iş yükleri için, yönetilen Premium disklere veya ultra disklere göre [depolama alanları doğrudan FCI SQL Server](virtual-machines-windows-portal-sql-create-failover-cluster.md) göz önünde bulundurun.  

Geçerli ortamınızın ıOPS etkinliğini denetleyin ve bir dağıtıma veya geçişe başlamadan önce Premium dosyalarının ihtiyacınız olan ıOPS 'yi sağlayabildiğini doğrulayın. Windows performans Izleyicisi disk sayaçlarını kullanın ve SQL Server verileri, günlüğü ve geçici DB dosyaları için gereken toplam ıOPS (disk aktarımı/sn) ve üretilen iş (disk bayt/sn) sayısını izleyin. Çok sayıda iş yükü, yoğun kullanım dönemleri sırasında kontrol etmek ve maksimum ıOPS 'nin yanı sıra ortalama ıOPS 'yi de göz önünde bulundurarak iyi bir fikir olacak. Premium dosya paylaşımları, paylaşımın boyutuna bağlı olarak ıOPS sağlar. Premium dosyalar Ayrıca, GÇ 'nizi bir saate kadar üç temel miktarı üç katına kaymanıza olanak sağlayan, yük patlaması de sağlar. 

Premium dosya paylaşma performansı hakkında daha fazla bilgi için bkz. [dosya paylaşımının performans katmanları](https://docs.microsoft.com/en-us/azure/storage/files/storage-files-planning#file-share-performance-tiers). 

### <a name="licensing-and-pricing"></a>Lisanslama ve fiyatlandırma

Azure sanal makineler 'de, Kullandıkça Öde (PAYG) veya kendi lisansını getir (KLG) VM görüntülerini kullanarak SQL Server lisans verebilirsiniz. Seçtiğiniz görüntünün türü ücretlendirileceğini etkiler.

PAYG lisanslaması sayesinde, Azure sanal makinelerinde SQL Server yük devretme kümesi örneği (FCı), Pasif düğümler de dahil olmak üzere tüm FCı düğümleri için ücret doğurur. Daha fazla bilgi için bkz. [sanal makine fiyatlandırması SQL Server Enterprise](https://azure.microsoft.com/pricing/details/virtual-machines/sql-server-enterprise/). 

Yazılım Güvencesi kapsamındaki Kurumsal Anlaşma müşterilerin her etkin düğüm için bir ücretsiz pasif FCı düğümü kullanma hakkı vardır. Azure 'da Bu avantajdan faydalanmak için, KLG VM görüntülerini kullanın ve ardından FCı 'nin hem etkin hem de pasif düğümlerinde aynı lisansı kullanın. Daha fazla bilgi için bkz. [Kurumsal Anlaşma](https://www.microsoft.com/Licensing/licensing-programs/enterprise.aspx).

Azure sanal makinelerinde SQL Server PAYG ve KLG lisanslamayı karşılaştırmak için bkz. [SQL VM 'leri kullanmaya başlama](virtual-machines-windows-sql-server-iaas-overview.md#get-started-with-sql-vms).

Lisanslama SQL Server hakkında tüm bilgiler için bkz. [fiyatlandırma](https://www.microsoft.com/sql-server/sql-server-2017-pricing).

### <a name="limitations"></a>Sınırlamalar

- Premium dosya paylaşımıyla yük devretme kümesi için FILESTREAM desteklenmez. FILESTREAM kullanmak için [depolama alanları doğrudan](virtual-machines-windows-portal-sql-create-failover-cluster.md)kullanarak kümenizi dağıtın. 

## <a name="prerequisites"></a>Önkoşullar 

Bu makaledeki yönergeleri uygulamadan önce Şu durumda olmalıdır:

- Microsoft Azure aboneliği.
- Azure sanal makinelerinde bir Windows etki alanı.
- Azure sanal makinesinde nesne oluşturma izni olan bir hesap.
- Aşağıdaki bileşenler için yeterli IP adresi alanına sahip bir Azure sanal ağı ve alt ağı:
   - Her iki sanal makine.
   - Yük devretme kümesi IP adresi.
   - Her FCı için bir IP adresi.
- Etki alanı denetleyicilerine işaret eden Azure ağı üzerinde yapılandırılmış DNS.
- Veri dosyalarınız için veritabanınızın depolama kotasına dayalı bir [Premium dosya paylaşma](../../../storage/files/storage-how-to-create-premium-fileshare.md) . 
- Veri dosyalarınız için kullanılan Premium dosya paylaşımından farklı yedeklemeler için dosya paylaşma. Bu dosya paylaşımının standart ya da Premium olması olabilir. 

Bu önkoşulları yerine, yük devretme kümenizi oluşturmaya devam edebilirsiniz. İlk adım, sanal makineleri oluşturmaktır.

## <a name="step-1-create-virtual-machines"></a>1\. Adım: sanal makine oluşturma

1. Aboneliğinizle [Azure Portal](https://portal.azure.com) oturum açın.

1. [Azure kullanılabilirlik kümesi oluşturun](../tutorial-availability-sets.md).

   Kullanılabilirlik kümesi, sanal makineleri hata etki alanları ve güncelleştirme etki alanları genelinde gruplandırır. Kullanılabilirlik kümesi, uygulamanızın ağ anahtarı veya bir sunucu rafı güç birimi gibi tek hata noktalarından etkilenmemesini sağlar.

   Sanal makineleriniz için kaynak grubu oluşturmadıysanız, bir Azure kullanılabilirlik kümesi oluştururken bunu yapın. Kullanılabilirlik kümesini oluşturmak için Azure portal kullanıyorsanız, aşağıdaki adımları uygulayın:

   - Azure portal, Azure Marketi 'ni açmak için **+** ' e tıklayın. **Kullanılabilirlik kümesi**araması yapın.
   - **Kullanılabilirlik kümesi**' ne tıklayın.
   - **Oluştur**’a tıklayın.
   - **Kullanılabilirlik kümesi oluştur** dikey penceresinde aşağıdaki değerleri ayarlayın:
      - **Ad**: kullanılabilirlik kümesi için bir ad.
      - **Abonelik**: Azure aboneliğiniz.
      - **Kaynak grubu**: var olan bir grubu kullanmak istiyorsanız, **var olanı kullan** ' a tıklayın ve açılan listeden grubu seçin. Aksi takdirde, **Yeni oluştur** ' u seçin ve grup için bir ad yazın.
      - **Konum**: sanal makinelerinizi oluşturmayı planladığınız konumu ayarlayın.
      - **Hata etki alanları**: varsayılanı kullanın (3).
      - **Etki alanlarını güncelleştir**: varsayılan (5) kullanın.
   - Kullanılabilirlik kümesini oluşturmak için **Oluştur** ' a tıklayın.

1. Kullanılabilirlik kümesinde sanal makineleri oluşturun.

   Azure kullanılabilirlik kümesinde iki SQL Server sanal makine sağlayın. Yönergeler için bkz. [Azure portal SQL Server sanal makinesi sağlama](virtual-machines-windows-portal-sql-server-provision.md).

   Her iki sanal makineyi de Yerleştir:

   - Kullanılabilirlik ayarlamış olduğunuz Azure Kaynak grubunda yer alan.
   - Etki alanı denetleyiciyle aynı ağda.
   - Her iki sanal makine için yeterli IP adresi alanı olan bir alt ağda, bu kümede son olarak kullanabileceğiniz tüm FCU.
   - Azure kullanılabilirlik kümesi 'nde.   

      >[!IMPORTANT]
      >Bir sanal makine oluşturulduktan sonra kullanılabilirlik kümesini ayarlayamazsınız veya değiştiremezsiniz.

   Azure Marketi 'nden bir görüntü seçin. Windows Server ve SQL Server veya yalnızca Windows Server içeren bir market görüntüsü kullanabilirsiniz. Ayrıntılar için bkz. [Azure sanal makinelerinde SQL Server genel bakış](virtual-machines-windows-sql-server-iaas-overview.md)

   Azure galerisindeki resmi SQL Server görüntüleri, yüklü bir SQL Server örneğini, ayrıca SQL Server yükleme yazılımını ve gerekli anahtarı içerir.

   >[!IMPORTANT]
   > Sanal makineyi oluşturduktan sonra, önceden yüklenmiş tek başına SQL Server örneğini kaldırın. Yük devretme kümesini ve Premium dosya paylaşımından depolama alanı olarak yapılandırdıktan sonra, SQL Server FCı oluşturmak için önceden yüklenmiş SQL Server medyasını kullanacaksınız. 

   Alternatif olarak, Azure Marketi görüntülerini yalnızca işletim sistemiyle kullanabilirsiniz. Bir **Windows Server 2016 Datacenter** görüntüsü seçin ve yük devretme kümesini ve Premium dosya paylaşımınızı depolama olarak yapılandırdıktan sonra SQL Server FCI 'yi yükleyebilirsiniz. Bu görüntü SQL Server yükleme medyası içermiyor. Yükleme medyasını her sunucu için SQL Server yüklemesini çalıştırabileceğiniz bir konuma yerleştirin. 

1. Azure sanal makinelerinizi oluşturduktan sonra, RDP ile her sanal makineye bağlanın.

   RDP ile bir sanal makineye ilk kez bağlandığınızda, bilgisayar ağ üzerinde bu BILGISAYARıN keşfedilmesini ister misiniz? **Evet**'e tıklayın.

1. SQL Server tabanlı sanal makine görüntülerinden birini kullanıyorsanız, SQL Server örneğini kaldırın.

   - **Programlar ve Özellikler**' de **Microsoft SQL Server 201_ (64-bit)** öğesine sağ tıklayın ve **Kaldır/Değiştir**' e tıklayın.
   - **Kaldır**' a tıklayın.
   - Varsayılan örneği seçin.
   - **Veritabanı motoru Hizmetleri**altındaki tüm özellikleri kaldırın. **Paylaşılan özellikleri**kaldırmayın. Aşağıdaki resme bakın:

      ![Özellikleri Kaldır](./media/virtual-machines-windows-portal-sql-create-failover-cluster/03-remove-features.png)

   - **İleri**' ye ve ardından **Kaldır**' a tıklayın.

1. <a name="ports"></a>Güvenlik Duvarı bağlantı noktalarını açın.

   Her bir sanal makinede, Windows Güvenlik Duvarı 'nda aşağıdaki bağlantı noktalarını açın.

   | Amaç | TCP Bağlantı Noktası | Notlar
   | ------ | ------ | ------
   | SQL Server | 1433 | Varsayılan SQL Server örnekleri için normal bağlantı noktası. Galeriden bir görüntü kullandıysanız, bu bağlantı noktası otomatik olarak açılır.
   | Durum yoklaması | 59999 | Açık herhangi bir TCP bağlantı noktası. Sonraki bir adımda, yük dengeleyici [durum araştırmasını](#probe) ve kümeyi Bu bağlantı noktasını kullanacak şekilde yapılandırın.   
   | Dosya paylaşımı | 445 | Dosya paylaşma hizmeti tarafından kullanılan bağlantı noktası. 

1. [Sanal makineleri önceden var olan etki alanına ekleyin](virtual-machines-windows-portal-sql-availability-group-prereq.md#joinDomain).

Sanal makineler oluşturulup yapılandırıldıktan sonra Premium dosya paylaşımından yapılandırma yapabilirsiniz.

## <a name="step-2-mount-premium-file-share"></a>2\. Adım: Premium dosya paylaşma bağlama

1. [Azure Portal](https://portal.azure.com) oturum açın ve depolama hesabınıza gidin.
1. **Dosya hizmeti** altında **dosya paylaşımları** ' na gidin ve SQL depolama alanınızı kullanmak istediğiniz Premium dosya paylaşımını seçin. 
1. Dosya paylaşımınızın bağlantı dizesini görüntülemek için **Bağlan** ' ı seçin. 
1. Açılan kutudan kullanmak istediğiniz sürücü harfini seçin ve ardından her iki kod bloğunu bir not defteri 'ne kopyalayın.

   :::image type="content" source="media/virtual-machines-windows-portal-sql-create-failover-cluster-premium-file-storage/premium-file-storage-commands.png" alt-text="Her iki PowerShell komutunu da dosya paylaşımının Connect portalından Kopyala":::

1. SQL Server FCı 'nizin hizmet hesabı için kullanacağı hesabı kullanarak SQL Server VM RDP. 
1. Bir yönetim PowerShell komut konsolu başlatın. 
1. Daha önce kaydettiğiniz portaldan komutları çalıştırın. 
1. @No__t_1 ağ yolunu kullanarak dosya Gezgini ya da **Çalıştır** iletişim kutusu (Windows tuşu + r) ile paylaşıma gidin. Örnek: `\\sqlvmstorageaccount.file.core.windows.net\sqlpremiumfileshare`

1. SQL veri dosyalarınızı içine yerleştirmek için yeni bağlı dosya paylaşımında en az bir klasör oluşturun. 
1. Kümeye katılacak her bir SQL Server VM bu adımları yineleyin. 

  > [!IMPORTANT]
  > Bu paylaşımın ıOPS ve boyut kapasitesini veri ve günlük dosyası için kaydetmek üzere yedekleme dosyaları için ayrı bir dosya paylaşma kullanmayı düşünün. Yedekleme dosyaları için Premium veya standart dosya paylaşımından birini kullanabilirsiniz

## <a name="step-3-configure-failover-cluster-with-file-share"></a>3\. Adım: yük devretme kümesini dosya paylaşımıyla yapılandırma 

Bir sonraki adım, yük devretme kümesini yapılandırmaktır. Bu adımda, aşağıdaki alt adımları yapacaksınız:

1. Windows Yük Devretme Kümelemesi özelliği Ekle
1. Kümeyi doğrulama
1. Yük devretme kümesi oluşturma
1. Bulut tanığını oluşturma


### <a name="add-windows-failover-clustering-feature"></a>Windows Yük Devretme Kümelemesi özelliği Ekle

1. Başlamak için, yerel Yöneticiler üyesi olan bir etki alanı hesabı kullanarak RDP ile ilk sanal makineye bağlanın ve Active Directory nesne oluşturma izinleri vardır. Yapılandırmanın geri kalanı için bu hesabı kullanın.

1. [Her bir sanal makineye Yük Devretme Kümelemesi özelliği ekleyin](virtual-machines-windows-portal-sql-availability-group-prereq.md#add-failover-clustering-features-to-both-sql-server-vms).

   Yük Devretme Kümelemesi özelliğini kullanıcı arabiriminden yüklemek için, her iki sanal makinede aşağıdaki adımları uygulayın.
   - **Sunucu Yöneticisi**, **Yönet**' e ve ardından **rol ve Özellik Ekle**' ye tıklayın.
   - **Rol ve Özellik Ekleme Sihirbazı**' nda, **özellikleri seçerken** **İleri** ' ye tıklayın.
   - **Özellikleri Seç**bölümünde **Yük Devretme Kümelemesi**' ne tıklayın. Tüm gerekli özellikleri ve yönetim araçlarını dahil edin. **Özellik Ekle**' ye tıklayın.
   - **İleri** ' ye tıklayın ve sonra özellikleri yüklemek için **son** ' a tıklayın.

   Yük Devretme Kümelemesi özelliğini PowerShell ile yüklemek için sanal makinelerden birindeki yönetici PowerShell oturumundan aşağıdaki betiği çalıştırın.

   ```powershell
   $nodes = ("<node1>","<node2>")
   Invoke-Command  $nodes {Install-WindowsFeature Failover-Clustering -IncludeAllSubFeature -IncludeManagementTools}
   ```

### <a name="validate-the-cluster"></a>Kümeyi doğrulama

Bu kılavuz, [Kümeyi Doğrula](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-31-run-cluster-validation)altındaki yönergelere başvurur.

Kullanıcı arabirimindeki veya PowerShell ile kümeyi doğrulayın.

Kümeyi kullanıcı arabirimiyle doğrulamak için sanal makinelerden birindeki aşağıdaki adımları uygulayın.

1. **Sunucu Yöneticisi** **Araçlar**' a ve ardından **Yük devretme kümesi Yöneticisi**' e tıklayın.
1. **Yük devretme kümesi Yöneticisi**' de **eylem**' e ve ardından **Yapılandırmayı Doğrula...** ' ya tıklayın.
1. **İleri**’ye tıklayın.
1. **Sunucu veya küme Seç**' de, her iki sanal makinenin adını yazın.
1. **Test seçeneklerinde** **yalnızca Seçdiğim Testleri Çalıştır**' ı seçin. **İleri**’ye tıklayın.
1. **Test seçimi**üzerinde, **depolama** ve **depolama alanları doğrudan**hariç tüm testleri ekleyin. Aşağıdaki resme bakın:

   :::image type="content" source="media/virtual-machines-windows-portal-sql-create-failover-cluster-premium-file-storage/cluster-validation.png" alt-text="Küme doğrulama testleri":::

1. **İleri**’ye tıklayın.
1. **Onay**sırasında **İleri**' ye tıklayın.

**Yapılandırma Doğrulama Sihirbazı** doğrulama testlerini çalıştırır.

Kümeyi PowerShell ile doğrulamak için, sanal makinelerden birindeki yönetici PowerShell oturumundan aşağıdaki betiği çalıştırın.

   ```powershell
   Test-Cluster –Node ("<node1>","<node2>") –Include "Inventory", "Network", "System Configuration"
   ```

Kümeyi doğruladıktan sonra, yük devretme kümesini oluşturun.

### <a name="create-the-failover-cluster"></a>Yük devretme kümesi oluşturma


Yük devretme kümesini oluşturmak için şunlar gerekir:
- Küme düğümleri haline gelen sanal makinelerin adları.
- Yük devretme kümesi için bir ad
- Yük devretme kümesi için bir IP adresi. Küme düğümleri ile aynı Azure sanal ağı ve alt ağı üzerinde kullanılmayan bir IP adresi kullanabilirsiniz.

#### <a name="windows-server-2012-2016"></a>Windows Server 2012-2016

Aşağıdaki PowerShell, **Windows Server 2012-2016**için bir yük devretme kümesi oluşturur. Betiği, düğümlerin adlarıyla (sanal makine adları) ve kullanılabilir bir IP adresini Azure VNET 'ten güncelleştirin:

```powershell
New-Cluster -Name <FailoverCluster-Name> -Node ("<node1>","<node2>") –StaticAddress <n.n.n.n> -NoStorage
```   

#### <a name="windows-server-2019"></a>Windows Server 2019

Aşağıdaki PowerShell, Windows Server 2019 için bir yük devretme kümesi oluşturur.  Daha fazla bilgi için blog [Yük devretme kümesini gözden geçirin: küme ağ nesnesi](https://blogs.windows.com/windowsexperience/2018/08/14/announcing-windows-server-2019-insider-preview-build-17733/#W0YAxO8BfwBRbkzG.97).  Betiği, düğümlerin adlarıyla (sanal makine adları) ve kullanılabilir bir IP adresini Azure VNET 'ten güncelleştirin:

```powershell
New-Cluster -Name <FailoverCluster-Name> -Node ("<node1>","<node2>") –StaticAddress <n.n.n.n> -NoStorage -ManagementPointNetworkType Singleton 
```


### <a name="create-a-cloud-witness"></a>Bulut tanığı oluşturma

Bulut tanığı, Azure Depolama Blobu depolanan yeni bir küme çekirdeği tanığı türüdür. Bu, bir tanık paylaşımının barındırıldığı ayrı bir VM gereksinimini ortadan kaldırır.

1. [Yük devretme kümesi için bir bulut tanığı oluşturun](https://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness).

1. Blob kapsayıcısı oluşturun.

1. Erişim anahtarlarını ve kapsayıcı URL 'sini kaydedin.

1. Yük devretme kümesi çekirdek tanığını yapılandırın. Bkz. [Kullanıcı arabirimindeki Kullanıcı arabiriminde çekirdek tanığını yapılandırma](https://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness#to-configure-cloud-witness-as-a-quorum-witness) .


## <a name="step-4-test-cluster-failover"></a>4\. Adım: test kümesi yük devretmesi

Kümenizin yük devretmesini test edin. Yük Devretme Kümesi Yöneticisi, kümenize sağ tıklayıp > **daha fazla eylem** > **çekirdek küme kaynağını taşıyın** > **düğümü seçin** ve kümenin diğer düğümünü seçin. Çekirdek küme kaynağını kümenin her düğümüne taşıyın ve ardından birincil düğüme geri taşıyın. Kümeyi her düğüme başarıyla taşıyabiliyorsanız SQL Server yüklemeye hazırsınız demektir.  

:::image type="content" source="media/virtual-machines-windows-portal-sql-create-failover-cluster-premium-file-storage/test-cluster-failover.png" alt-text="Çekirdek kaynağı diğer düğümlere taşıyarak küme yük devretmesini test etme":::

## <a name="step-5-create-sql-server-fci"></a>5\. Adım: SQL Server FCı oluşturma

Yük devretme kümesini yapılandırdıktan sonra, SQL Server FCı 'yi oluşturabilirsiniz.

1. RDP ile ilk sanal makineye bağlanın.

1. **Yük devretme kümesi Yöneticisi**, tüm küme çekirdek kaynaklarının ilk sanal makinede olduğundan emin olun. Gerekirse, tüm kaynakları bu sanal makineye taşıyın.

1. Yükleme medyasını bulun. Sanal makine Azure Market görüntülerinden birini kullanıyorsa, medya `C:\SQLServer_<version number>_Full` bulunur. **Kurulum**'a tıklayın.

1. **SQL Server Yükleme Merkezi**'nde **yükleme**' ye tıklayın.

1. **Yeni SQL Server yük devretme kümesi yüklemesi**' ne tıklayın. SQL Server FCı 'yi yüklemek için sihirbazdaki yönergeleri izleyin.

   FCı veri dizinlerinin Premium dosya paylaşımında olması gerekir. Paylaşımın tam yolunu, `\\storageaccountname.file.core.windows.net\filesharename\foldername` biçiminde yazın. Veri dizini olarak bir dosya sunucusu belirtistediğinizi bildiren bir uyarı görüntülenir. Bu beklenen bir durumdur. Dosya paylaşımının devam eden hesabının, SQL Server hizmetin olası hatalardan kaçınmak için kullandığı hesapla aynı olduğundan emin olun. 

   :::image type="content" source="media/virtual-machines-windows-portal-sql-create-failover-cluster-premium-file-storage/use-file-share-as-data-directories.png" alt-text="Dosya paylaşımının SQL veri dizinleri olarak kullanılması":::

1. Sihirbazı tamamladıktan sonra, kurulum ilk düğümde bir SQL Server FCı yükler.

1. Kurulum, FCı 'yı ilk düğüme başarıyla yükledikten sonra, RDP ile ikinci düğüme bağlanın.

1. **SQL Server yükleme merkezini**açın. **Yükleme**' ye tıklayın.

1. **SQL Server yük devretme kümesine düğüm Ekle**' ye tıklayın. SQL Server 'ı yüklemek ve bu sunucuyu FCı 'ye eklemek için sihirbazdaki yönergeleri izleyin.

   >[!NOTE]
   >SQL Server ile bir Azure Marketi Galeri görüntüsü kullandıysanız, görüntüye SQL Server Araçlar eklenmiştir. Bu görüntüyü kullanmıyorsanız, SQL Server araçlarını ayrı olarak yükleyebilirsiniz. Bkz. [Download SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx).

## <a name="step-6-create-azure-load-balancer"></a>6\. Adım: Azure yük dengeleyici oluşturma

Azure sanal makinelerinde kümeler, aynı anda tek bir küme düğümünde olması gereken bir IP adresini tutmak için bir yük dengeleyici kullanır. Bu çözümde, yük dengeleyici SQL Server FCı için IP adresini tutar.

[Bir Azure yük dengeleyici oluşturun ve yapılandırın](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer).

### <a name="create-the-load-balancer-in-the-azure-portal"></a>Azure portal yük dengeleyiciyi oluşturun

Yük dengeleyiciyi oluşturmak için:

1. Azure portal, sanal makineler ile kaynak grubuna gidin.

1. **+ Ekle**'ye tıklayın. Market 'Te **Load Balancer**arayın. **Load Balancer**' ye tıklayın.

1. **Oluştur**’a tıklayın.

1. Yük dengeleyiciyi şu şekilde yapılandırın:

   - **Abonelik**: Azure aboneliğiniz.
   - **Kaynak grubu**: sanal makinelerinize göre aynı kaynak grubunu kullanın.
   - **Ad**: yük dengeleyiciyi tanımlayan bir ad.
   - **Bölge**: sanal makinelerinizde aynı Azure konumunu kullanın.
   - **Tür**: yük dengeleyici ortak veya özel olabilir. Aynı VNET içinden bir özel yük dengeleyiciye erişilebilir. Azure uygulamalarının çoğu özel yük dengeleyici kullanabilir. Uygulamanızın doğrudan Internet üzerinden SQL Server erişmesi gerekiyorsa, ortak yük dengeleyici kullanın.
   - **SKU**: yük dengeleyiciniz için SKU standart olmalıdır. 
   - **Sanal ağ**: sanal makinelerle aynı ağ.
   - **IP adresi ataması**: IP adresi ataması statik olmalıdır. 
   - **Özel IP adresi**: SQL Server FCI küme ağı kaynağına ATADıĞıNıZ aynı IP adresi.
   Aşağıdaki resme bakın:

   ![CreateLoadBalancer](./media/virtual-machines-windows-portal-sql-create-failover-cluster/30-load-balancer-create.png)
   

### <a name="configure-the-load-balancer-backend-pool"></a>Yük dengeleyici arka uç havuzunu yapılandırma

1. Sanal makineler ile Azure Kaynak grubuna dönün ve yeni yük dengeleyiciyi bulun. Kaynak grubundaki görünümü yenilemeniz gerekebilir. Yük dengeleyiciye tıklayın.

1. Arka uç havuzu eklemek için **arka uç havuzları** ' na tıklayıp **+ Ekle** ' ye tıklayın.

1. Arka uç havuzunu VM 'Leri içeren kullanılabilirlik kümesiyle ilişkilendirin.

1. **Hedef ağ IP yapılandırması**altında, **sanal makine** ' yi işaretleyin ve küme düğümleri olarak katılacak sanal makineleri seçin. FCı 'yı barındıracak tüm sanal makineleri eklediğinizden emin olun. 

1. Arka uç havuzunu oluşturmak için **Tamam** ' ı tıklatın.

### <a name="configure-a-load-balancer-health-probe"></a>Yük dengeleyici durum araştırması yapılandırma

1. Yük dengeleyici dikey penceresinde **sistem durumu araştırmaları**' na tıklayın.

1. **+ Ekle**'ye tıklayın.

1. **Sistem durumu araştırması Ekle** dikey penceresinde sistem <a name="probe"> </a>durumu araştırma parametrelerini ayarlayın:

   - **Ad**: sistem durumu araştırması için bir ad.
   - **Protokol**: TCP.
   - **Bağlantı noktası**: [Bu adımda](#ports)sistem durumu araştırması için güvenlik duvarında oluşturduğunuz bağlantı noktasına ayarlanır. Bu makalede örnek, `59999` olan TCP bağlantı noktasını kullanır.
   - **Aralık**: 5 saniye.
   - **Sağlıksız eşik**: 2 ardışık başarısızlık.

1. Tamam'a tıklayın.

### <a name="set-load-balancing-rules"></a>Yük Dengeleme kurallarını ayarlama

1. Yük dengeleyici dikey penceresinde, **Yük Dengeleme kuralları**' na tıklayın.

1. **+ Ekle**'ye tıklayın.

1. Yük Dengeleme kuralları parametrelerini ayarlayın:

   - **Ad**: Yük Dengeleme kuralları için bir ad.
   - **Ön uç IP adresi**: SQL Server FCI kümesi ağ kaynağının IP adresini kullanın.
   - **Bağlantı noktası**: SQL Server FCı TCP bağlantı noktası için ayarlandı. Varsayılan örnek bağlantı noktası 1433 ' dir.
   - **Arka uç bağlantı noktası**: Bu değer, **kayan IP 'yi (doğrudan sunucu dönüşü)** etkinleştirdiğinizde **bağlantı noktası** değeriyle aynı bağlantı noktasını kullanır.
   - **Arka uç havuzu**: daha önce yapılandırdığınız arka uç havuzu adını kullanın.
   - **Sistem durumu araştırması**: daha önce yapılandırdığınız sistem durumu araştırmasını kullanın.
   - **Oturum kalıcılığı**: yok.
   - **Boşta kalma zaman aşımı (dakika)** : 4.
   - **Kayan IP (doğrudan sunucu dönüşü)** : etkin

1. **Tamam**’a tıklayın.

## <a name="step-7-configure-cluster-for-probe"></a>7\. Adım: araştırma için kümeyi yapılandırma

PowerShell 'de küme araştırması bağlantı noktası parametresini ayarlayın.

Küme araştırması bağlantı noktası parametresini ayarlamak için aşağıdaki betikteki değişkenleri ortamınızdaki değerlerle güncelleştirin. Betiğe `<>` açılı ayraçları kaldırın. 

   ```powershell
   $ClusterNetworkName = "<Cluster Network Name>"
   $IPResourceName = "<SQL Server FCI IP Address Resource Name>" 
   $ILBIP = "<n.n.n.n>" 
   [int]$ProbePort = <nnnnn>

   Import-Module FailoverClusters

   Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```

Yukarıdaki komut dosyasında ortamınızın değerlerini ayarlayın. Aşağıdaki listede değerler açıklanmaktadır:

   - `<Cluster Network Name>`: ağ için Windows Server yük devretme kümesi adı. **Yük Devretme Kümesi Yöneticisi** > **ağlarda**ağa sağ tıklayıp **Özellikler**' e tıklayın. Doğru değer **genel** sekmesinde **ad** ' ın altında bulunur. 

   - `<SQL Server FCI IP Address Resource Name>`: SQL Server FCı IP adresi kaynak adı. **Yük Devretme Kümesi Yöneticisi** > **rolleri**SQL Server FCI rolü altında, **sunucu adı**altında, IP adresi kaynağına sağ tıklayın ve **Özellikler**' e tıklayın. Doğru değer **genel** sekmesinde **ad** ' ın altında bulunur. 

   - `<ILBIP>`: ıLB IP adresi. Bu adres, Azure portal ıLB ön uç adresi olarak yapılandırılır. Bu ayrıca SQL Server FCı IP adresidir. **Yük devretme kümesi Yöneticisi** , `<SQL Server FCI IP Address Resource Name>` ' i bulduğunuz aynı Özellikler sayfasında bulabilirsiniz.  

   - `<nnnnn>`: Yük Dengeleyici Sistem durumu araştırmasına yapılandırdığınız araştırma bağlantı noktasıdır. Kullanılmayan tüm TCP bağlantı noktaları geçerlidir. 

>[!IMPORTANT]
>Küme parametresinin alt ağ maskesi TCP IP yayını adresi olmalıdır: `255.255.255.255`.

Küme araştırmasını ayarladıktan sonra, PowerShell 'de tüm küme parametrelerini görebilirsiniz. Şu betiği çalıştırın:

   ```powershell
   Get-ClusterResource $IPResourceName | Get-ClusterParameter 
  ```

## <a name="step-8-test-fci-failover"></a>8\. Adım: FCı yük devretmesini test etme

Küme işlevselliğini doğrulamak için FCı yük devretmesini test edin. Aşağıdaki adımları uygulayın:

1. RDP ile SQL Server FCı kümesi düğümlerinden birine bağlanın.

1. **Yük devretme kümesi Yöneticisi**açın. **Roller**' e tıklayın. Hangi düğümün SQL Server FCı rolüne sahip olduğuna dikkat edin.

1. SQL Server FCı rolüne sağ tıklayın.

1. **Taşı** ' ya tıklayın ve **mümkün olan en iyi düğüme**tıklayın.

**Yük devretme kümesi Yöneticisi** rolü ve kaynakları çevrimdışı olarak görünür. Kaynaklar daha sonra başka bir düğüme taşınır ve çevrimiçi duruma gelir.

### <a name="test-connectivity"></a>Bağlantıyı test etme

Bağlantıyı sınamak için aynı sanal ağdaki başka bir sanal makinede oturum açın. **SQL Server Management Studio** açın ve SQL Server FCI adına bağlanın.

>[!NOTE]
>Gerekirse, [SQL Server Management Studio indirebilirsiniz](https://msdn.microsoft.com/library/mt238290.aspx).

## <a name="limitations"></a>Sınırlamalar

Azure sanal makineleri, kümelenmiş paylaşılan birimlerde (CSV) ve [Standart yük dengeleyicide](../../../load-balancer/load-balancer-standard-overview.md)depolama Ile Windows Server 2019 ' de Microsoft Dağıtılmış işlem DÜZENLEYICISI (MSDTC) ' i destekler.

Azure sanal makinelerinde, Windows Server 2016 ve önceki sürümlerde MSDTC desteklenmez çünkü:

- Kümelenmiş MSDTC kaynağı, paylaşılan depolama alanını kullanacak şekilde yapılandırılamaz. Windows Server 2016 ile bir MSDTC kaynağı oluşturursanız, depolama alanı orada olsa bile, kullanılabilir herhangi bir paylaşılan depolama alanı göstermez. Bu sorun Windows Server 2019 ' de düzeltildi.
- Temel yük dengeleyici RPC bağlantı noktalarını işlemez.

## <a name="see-also"></a>Ayrıca Bkz.

- [Windows küme teknolojileri](/windows-server/failover-clustering/failover-clustering-overview)
- [Yük devretme kümesi örnekleri SQL Server](/sql/sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server).
