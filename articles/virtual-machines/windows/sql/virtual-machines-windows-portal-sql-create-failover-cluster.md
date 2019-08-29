---
title: SQL Server FCı-Azure sanal makineleri | Microsoft Docs
description: Bu makalede, Azure sanal makinelerinde SQL Server yük devretme kümesi örneği oluşturma işlemi açıklanmaktadır.
services: virtual-machines
documentationCenter: na
author: MikeRayMSFT
manager: craigg
editor: monicar
tags: azure-service-management
ms.assetid: 9fc761b1-21ad-4d79-bebc-a2f094ec214d
ms.service: virtual-machines-sql
ms.custom: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/11/2018
ms.author: mikeray
ms.openlocfilehash: 3ff9a694dca0d2a205c27569a7c744f482b662ec
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70100649"
---
# <a name="configure-sql-server-failover-cluster-instance-on-azure-virtual-machines"></a>Azure sanal makinelerinde SQL Server yük devretme kümesi örneğini yapılandırma

Bu makalede, Kaynak Yöneticisi modelinde Azure sanal makinelerinde SQL Server yük devretme kümesi örneği (FCı) oluşturma işlemi açıklanmaktadır. Bu çözüm, Windows [Server 2016 Datacenter Edition depolama alanları doğrudan \(S2D\) ](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview) kullanarak bir Windows kümesindeki düğümler (Azure VM 'ler) arasındaki depolama alanını (veri disklerini) eşitleyen yazılım tabanlı bir sanal San olarak kullanır. S2D, Windows Server 2016 ' de yenidir.

Aşağıdaki diyagramda Azure sanal makinelerinde tüm çözüm gösterilmektedir:

![Kullanılabilirlik Grubu](./media/virtual-machines-windows-portal-sql-create-failover-cluster/00-sql-fci-s2d-complete-solution.png)

Önceki diyagramda şunları gösterir:

- Bir Windows Yük devretme kümesinde iki Azure sanal makinesi. Bir sanal makine bir yük devretme kümesinde olduğunda, bu, *küme düğümü*veya *düğümleri*olarak da adlandırılır.
- Her sanal makinenin iki veya daha fazla veri diski vardır.
- S2D veri diskindeki verileri eşitler ve eşitlenmiş depolamayı bir depolama havuzu olarak gösterir.
- Depolama havuzu, yük devretme kümesine bir küme paylaşılan birimi (CSV) gösterir.
- SQL Server FCı kümesi rolü, veri sürücüleri için CSV 'yi kullanır.
- SQL Server FCı için IP adresini tutacak bir Azure yük dengeleyici.
- Azure kullanılabilirlik kümesi tüm kaynakları tutar.

   >[!NOTE]
   >Tüm Azure kaynakları diyagramdaki, aynı kaynak grubunda bulunur.

S2D hakkında daha fazla bilgi için bkz. [Windows Server 2016 Datacenter \(Edition\)depolama alanları doğrudan S2D](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview).

S2D iki tür mimariyi destekler-yakınsanmış ve hiper yakınsama. Bu belgedeki mimari hiper yakınsamalı. Hiper yakınsama altyapısı, depolama alanını kümelenmiş uygulamayı barındıran aynı sunuculara koyar. Bu mimaride, depolama her SQL Server FCı düğümünde bulunur.

## <a name="licensing-and-pricing"></a>Lisanslama ve fiyatlandırma

Azure sanal makineler 'de, Kullandıkça Öde (PAYG) veya kendi lisansını getir (KLG) VM görüntülerini kullanarak SQL Server lisanslayabilirim. Seçtiğiniz görüntünün türü ücretlendirileceğini etkiler.

PAYG lisanslaması sayesinde, Azure sanal makinelerinde SQL Server yük devretme kümesi örneği (FCı), Pasif düğümler de dahil olmak üzere tüm FCı düğümleri için ücret doğurur. Daha fazla bilgi için bkz. [sanal makine fiyatlandırması SQL Server Enterprise](https://azure.microsoft.com/pricing/details/virtual-machines/sql-server-enterprise/). 

Yazılım Güvencesi kapsamındaki Kurumsal Anlaşma müşterilerin her etkin düğüm için bir ücretsiz pasif FCı düğümü kullanma hakkı vardır. Azure 'Da Bu avantajdan faydalanmak için, KLG VM görüntülerini kullanın ve ardından FCı 'nin hem etkin hem de pasif düğümlerinde aynı lisansı kullanın. Daha fazla bilgi için bkz. [Kurumsal Anlaşma](https://www.microsoft.com/Licensing/licensing-programs/enterprise.aspx).

Azure sanal makinelerinde SQL Server PAYG ve KLG lisanslamayı karşılaştırmak için bkz. [SQL VM 'leri kullanmaya başlama](virtual-machines-windows-sql-server-iaas-overview.md#get-started-with-sql-vms).

Lisanslama SQL Server hakkında tüm bilgiler için bkz. [fiyatlandırma](https://www.microsoft.com/sql-server/sql-server-2017-pricing).

### <a name="example-azure-template"></a>Örnek Azure şablonu

Tüm çözümü bir şablondan Azure 'da oluşturabilirsiniz. GitHub [Azure hızlı başlangıç şablonlarında](https://github.com/MSBrett/azure-quickstart-templates/tree/master/sql-server-2016-fci-existing-vnet-and-ad)bir şablon örneği mevcuttur. Bu örnek, belirli bir iş yükü için tasarlanmamıştır veya test edilmemiştir. Etki alanına bağlı S2D depolaması olan bir SQL Server FCı oluşturmak için şablonu çalıştırabilirsiniz. Şablonu değerlendirebilir ve sizin amacınıza göre değiştirebilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce

Bilmeniz gereken birkaç nokta ve devam etmeden önce ihtiyacınız olan birkaç şey vardır.

### <a name="what-to-know"></a>Ne bilmelidir
Aşağıdaki teknolojilerde işlemsel olarak anlaşılmalıdır:

- [Windows küme teknolojileri](https://docs.microsoft.com/windows-server/failover-clustering/failover-clustering-overview)
- [Yük devretme kümesi örnekleri SQL Server](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server).

Önemli bir farklılık, bir Azure IaaS VM konuk yük devretme kümesinde, sunucu başına (küme düğümü) ve tek bir alt ağ için tek bir NIC önerilir. Azure ağındaki fiziksel yedeklilik nedeniyle Azure IaaS VM konuk kümesinde ek NIC ve alt ağ kullanılmasına gerek yoktur. Küme doğrulama raporu, düğümlerin yalnızca tek bir ağ üzerinde erişilebilir durumda olduğuna dair bir uyarı gösterse de bu uyarı Azure IaaS VM konuk yük devretme kümelerinde güvenli bir şekilde yoksayılabilir. 

Ayrıca, aşağıdaki teknolojilerde genel olarak anlaşılmalıdır:

- [Windows Server 2016 ' de Depolama Alanları Doğrudan kullanan hiper yakınsama çözümü](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct)
- [Azure kaynak grupları](../../../azure-resource-manager/manage-resource-groups-portal.md)

> [!IMPORTANT]
> Şu anda [SQL Server IaaS Aracısı uzantısı](virtual-machines-windows-sql-server-agent-extension.md) , Azure 'da SQL Server FCI için desteklenmez. Uzantıyı FCı 'ye katılan VM 'lerden kaldırmanızı öneririz. Bu uzantı, otomatik yedekleme ve düzeltme eki uygulama gibi özellikleri ve SQL için bazı portal özelliklerini destekler. Aracı kaldırıldıktan sonra bu özellikler SQL VM 'Leri için çalışmaz.

### <a name="what-to-have"></a>Nelerin olması gerekir

Bu makaledeki yönergeleri uygulamadan önce Şu durumda olmalıdır:

- Microsoft Azure aboneliği.
- Azure sanal makinelerinde bir Windows etki alanı.
- Azure sanal makinesinde nesne oluşturma izni olan bir hesap.
- Aşağıdaki bileşenler için yeterli IP adresi alanına sahip bir Azure sanal ağı ve alt ağı:
   - Her iki sanal makine.
   - Yük devretme kümesi IP adresi.
   - Her FCı için bir IP adresi.
- Etki alanı denetleyicilerine işaret eden Azure ağı üzerinde yapılandırılmış DNS.

Bu önkoşulları yerine, yük devretme kümenizi oluşturmaya devam edebilirsiniz. İlk adım, sanal makineleri oluşturmaktır.

## <a name="step-1-create-virtual-machines"></a>1\. adım: Sanal makineler oluşturma

1. Aboneliğinizle [Azure Portal](https://portal.azure.com) oturum açın.

1. [Azure kullanılabilirlik kümesi oluşturun](../tutorial-availability-sets.md).

   Kullanılabilirlik kümesi, sanal makineleri hata etki alanları ve güncelleştirme etki alanları genelinde gruplandırır. Kullanılabilirlik kümesi, uygulamanızın ağ anahtarı veya bir sunucu rafı güç birimi gibi tek hata noktalarından etkilenmemesini sağlar.

   Sanal makineleriniz için kaynak grubu oluşturmadıysanız, bir Azure kullanılabilirlik kümesi oluştururken bunu yapın. Kullanılabilirlik kümesini oluşturmak için Azure portal kullanıyorsanız, aşağıdaki adımları uygulayın:

   - Azure Portal Azure Marketi ' ni **+** açmak için tıklayın. **Kullanılabilirlik kümesi**araması yapın.
   - **Kullanılabilirlik kümesi**' ne tıklayın.
   - **Oluştur**'a tıklayın.
   - **Kullanılabilirlik kümesi oluştur** dikey penceresinde aşağıdaki değerleri ayarlayın:
      - **Ad**: Kullanılabilirlik kümesi için bir ad.
      - **Abonelik**: Azure aboneliğiniz.
      - **Kaynak grubu**: Var olan bir grubu kullanmak istiyorsanız, **var olanı kullan** ' a tıklayın ve açılan listeden grubu seçin. Aksi takdirde, **Yeni oluştur** ' u seçin ve grup için bir ad yazın.
      - **Konum**: Sanal makinelerinizi oluşturmayı planladığınız konumu ayarlayın.
      - **Hata etki alanları**: Varsayılanı kullanın (3).
      - **Güncelleştirme etki alanları**: Varsayılanı kullanın (5).
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

   SQL Server Lisansı için nasıl ödeme yapmak istediğinize göre doğru görüntüyü seçin:

   - **Kullanım lisansı başına ödeme**: Bu görüntülerin ikinci başına maliyeti SQL Server Lisanslama içerir:
      - **Windows Server Datacenter 2016 üzerinde SQL Server 2016 Enterprise**
      - **Windows Server Datacenter 2016 üzerinde SQL Server 2016 standardı**
      - **Windows Server Datacenter 2016 üzerinde SQL Server 2016 geliştirici**

   - **Kendi lisansını getir (KLG)**

      - **KLG Windows Server Datacenter 2016 üzerinde SQL Server 2016 Enterprise**
      - **KLG Windows Server Datacenter 2016 üzerinde SQL Server 2016 standardı**

   >[!IMPORTANT]
   >Sanal makineyi oluşturduktan sonra, önceden yüklenmiş tek başına SQL Server örneğini kaldırın. Yük devretme kümesini ve S2D yapılandırmasını yapılandırdıktan sonra, SQL Server FCı 'yi oluşturmak için önceden yüklenmiş SQL Server medyasını kullanacaksınız.

   Alternatif olarak, Azure Marketi görüntülerini yalnızca işletim sistemiyle kullanabilirsiniz. Bir **Windows Server 2016 Datacenter** görüntüsü seçin ve yük devretme KÜMESINI ve S2D yapılandırmasını yapılandırdıktan sonra SQL Server FCI 'yi yükleyebilirsiniz. Bu görüntü SQL Server yükleme medyası içermiyor. Yükleme medyasını her sunucu için SQL Server yüklemesini çalıştırabileceğiniz bir konuma yerleştirin.

1. Azure sanal makinelerinizi oluşturduktan sonra, RDP ile her sanal makineye bağlanın.

   RDP ile bir sanal makineye ilk kez bağlandığınızda, bilgisayar ağ üzerinde bu BILGISAYARıN keşfedilmesini ister misiniz? Yordamı tamamlamak için **Evet**yüklemesini desteklemesi gerekir.

1. SQL Server tabanlı sanal makine görüntülerinden birini kullanıyorsanız, SQL Server örneğini kaldırın.

   - **Programlar ve Özellikler**' de **Microsoft SQL Server 2016 (64-bit)** öğesine sağ tıklayın ve **Kaldır/Değiştir**' e tıklayın.
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
   | Durum araştırması | 59999 | Açık herhangi bir TCP bağlantı noktası. Sonraki bir adımda, yük dengeleyici [durum araştırmasını](#probe) ve kümeyi Bu bağlantı noktasını kullanacak şekilde yapılandırın.  

1. Sanal makineye depolama ekleyin. Ayrıntılı bilgi için bkz. [depolama ekleme](../disks-types.md).

   Her iki sanal makine için en az iki veri diski gerekir.

   Ham diskler ekleyin-NTFS biçimli diskler değildir.
      >[!NOTE]
      >NTFS biçimli diskler eklerseniz, S2D 'yi yalnızca disk uygunluk denetimi olmadan etkinleştirebilirsiniz.  

   Her VM 'ye en az iki Premium SSD ekleyin. En az P30 (1 TB) disk kullanmanızı öneririz.

   Konak önbelleğini **salt okunurdur**olarak ayarlayın.

   Üretim ortamlarında kullandığınız depolama kapasitesi, iş yükünüze göre değişir. Bu makalede açıklanan değerler tanıtım ve test içindir.

1. [Sanal makineleri önceden var olan etki alanına ekleyin](virtual-machines-windows-portal-sql-availability-group-prereq.md#joinDomain).

Sanal makineler oluşturulup yapılandırıldıktan sonra, yük devretme kümesini yapılandırabilirsiniz.

## <a name="step-2-configure-the-windows-failover-cluster-with-s2d"></a>2\. adım: Windows Yük devretme kümesini S2D ile yapılandırma

Sonraki adım, yük devretme kümesini S2D ile yapılandırmaktır. Bu adımda, aşağıdaki alt adımları yapacaksınız:

1. Windows Yük Devretme Kümelemesi özelliği Ekle
1. Kümeyi doğrulama
1. Yük devretme kümesi oluşturma
1. Bulut tanığını oluşturma
1. Depolama ekleme

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

Başvuru için, sonraki adımlarda [Windows Server 2016 ' de depolama alanları doğrudan kullanarak hiper yakınsama çözümünün](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-3-configure-storage-spaces-direct)3. adımı altındaki yönergeleri izleyin.

### <a name="validate-the-cluster"></a>Kümeyi doğrulama

Bu kılavuz, [Kümeyi Doğrula](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-31-run-cluster-validation)altındaki yönergelere başvurur.

Kullanıcı arabirimindeki veya PowerShell ile kümeyi doğrulayın.

Kümeyi kullanıcı arabirimiyle doğrulamak için sanal makinelerden birindeki aşağıdaki adımları uygulayın.

1. **Sunucu Yöneticisi** **Araçlar**' a ve ardından **Yük devretme kümesi Yöneticisi**' e tıklayın.
1. **Yük devretme kümesi Yöneticisi**' de **eylem**' e ve ardından **Yapılandırmayı Doğrula...** ' ya tıklayın.
1. **İleri**'ye tıklayın.
1. **Sunucu veya küme Seç**' de, her iki sanal makinenin adını yazın.
1. **Test seçeneklerinde** **yalnızca Seçdiğim Testleri Çalıştır**' ı seçin. **İleri**'ye tıklayın.
1. **Test seçimi**üzerinde, **depolama**hariç tüm testleri ekleyin. Aşağıdaki resme bakın:

   ![Testleri doğrula](./media/virtual-machines-windows-portal-sql-create-failover-cluster/10-validate-cluster-test.png)

1. **İleri**'ye tıklayın.
1. **Onay**sırasında **İleri**' ye tıklayın.

**Yapılandırma Doğrulama Sihirbazı** doğrulama testlerini çalıştırır.

Kümeyi PowerShell ile doğrulamak için, sanal makinelerden birindeki yönetici PowerShell oturumundan aşağıdaki betiği çalıştırın.

   ```powershell
   Test-Cluster –Node ("<node1>","<node2>") –Include "Storage Spaces Direct", "Inventory", "Network", "System Configuration"
   ```

Kümeyi doğruladıktan sonra, yük devretme kümesini oluşturun.

### <a name="create-the-failover-cluster"></a>Yük devretme kümesi oluşturma

Bu kılavuz [Yük devretme kümesini oluşturmak](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-32-create-a-cluster)için kullanılır.

Yük devretme kümesini oluşturmak için şunlar gerekir:
- Küme düğümleri haline gelen sanal makinelerin adları.
- Yük devretme kümesi için bir ad
- Yük devretme kümesi için bir IP adresi. Küme düğümleri ile aynı Azure sanal ağı ve alt ağı üzerinde kullanılmayan bir IP adresi kullanabilirsiniz.

Aşağıdaki PowerShell bir yük devretme kümesi oluşturur. Betiği, düğümlerin adlarıyla (sanal makine adları) ve kullanılabilir bir IP adresini Azure VNET 'ten güncelleştirin:

```powershell
New-Cluster -Name <FailoverCluster-Name> -Node ("<node1>","<node2>") –StaticAddress <n.n.n.n> -NoStorage
```   

### <a name="create-a-cloud-witness"></a>Bulut tanığı oluşturma

Bulut tanığı, Azure Depolama Blobu depolanan yeni bir küme çekirdeği tanığı türüdür. Bu, bir tanık paylaşımının barındırıldığı ayrı bir sanal makinenin gereksinimini ortadan kaldırır.

1. [Yük devretme kümesi için bir bulut tanığı oluşturun](https://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness).

1. Blob kapsayıcısı oluşturun.

1. Erişim anahtarlarını ve kapsayıcı URL 'sini kaydedin.

1. Yük devretme kümesi çekirdek tanığını yapılandırın. Bkz. [Kullanıcı arabirimindeki Kullanıcı arabiriminde çekirdek tanığını yapılandırma](https://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness#to-configure-cloud-witness-as-a-quorum-witness) .

### <a name="add-storage"></a>Depolama ekleme

S2D için disklerin boş ve bölüm ya da diğer veriler olmadan boş olması gerekir. Diskleri temizlemek için [Bu kılavuzdaki adımları](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-34-clean-disks)izleyin.

1. [Depolama alanları doğrudan \(\)S2D özelliğini etkinleştirin](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-35-enable-storage-spaces-direct).

   Aşağıdaki PowerShell depolama alanları doğrudan 'ı sunar.  

   ```powershell
   Enable-ClusterS2D
   ```

   **Yük devretme kümesi Yöneticisi**, artık depolama havuzunu görebilirsiniz.

1. [Bir birim oluşturun](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-36-create-volumes).

   S2D özelliklerinden biri, etkinleştirdiğiniz zaman otomatik olarak bir depolama havuzu oluşturmasıdır. Artık bir birim oluşturmaya hazırsınız. PowerShell komutunu `New-Volume` , biçimlendirme, kümeye ekleme ve küme paylaşılan birimi (CSV) oluşturma gibi birim oluşturma işlemini otomatikleştirir. Aşağıdaki örnek 800 gigabayt (GB) CSV oluşturur.

   ```powershell
   New-Volume -StoragePoolFriendlyName S2D* -FriendlyName VDisk01 -FileSystem CSVFS_REFS -Size 800GB
   ```   

   Bu komut tamamlandıktan sonra, bir küme kaynağı olarak 800 GB bir birim bağlanır. Birim: `C:\ClusterStorage\Volume1\`.

   Aşağıdaki diyagramda S2D ile küme paylaşılan birimi gösterilmektedir:

   ![ClusterSharedVolume](./media/virtual-machines-windows-portal-sql-create-failover-cluster/15-cluster-shared-volume.png)

## <a name="step-3-test-failover-cluster-failover"></a>3\. adım: Yük devretme kümesi yük devretmesini

Yük Devretme Kümesi Yöneticisi, depolama kaynağını diğer küme düğümüne taşıyabildiğinizi doğrulayın. **Yük devretme kümesi Yöneticisi** ile yük devretme kümesine bağlanabilir ve depolamayı bir düğümden diğerine taşırsanız, FCI 'yi yapılandırmaya hazırsanız.

## <a name="step-4-create-sql-server-fci"></a>4\. Adım: SQL Server FCı oluştur

Yük devretme kümesini ve depolama dahil tüm küme bileşenlerini yapılandırdıktan sonra, SQL Server FCı 'yi oluşturabilirsiniz.

1. RDP ile ilk sanal makineye bağlanın.

1. **Yük devretme kümesi Yöneticisi**, tüm küme çekirdek kaynaklarının ilk sanal makinede olduğundan emin olun. Gerekirse, tüm kaynakları bu sanal makineye taşıyın.

1. Yükleme medyasını bulun. Sanal makine Azure Marketi görüntülerinden birini kullanıyorsa medya konumunda `C:\SQLServer_<version number>_Full`bulunur. **Kurulum**'a tıklayın.

1. **SQL Server Yükleme Merkezi**'nde **yükleme**' ye tıklayın.

1. **Yeni SQL Server yük devretme kümesi yüklemesi**' ne tıklayın. SQL Server FCı 'yi yüklemek için sihirbazdaki yönergeleri izleyin.

   FCı veri dizinlerinin kümelenmiş depolamada olması gerekir. S2D ile, paylaşılan bir disk değildir, ancak her bir sunucudaki bir birime bağlama noktasıdır. S2D her iki düğüm arasındaki birimi eşitler. Birim, kümeye küme paylaşılan birimi olarak sunulur. Veri dizinleri için CSV bağlama noktasını kullanın.

   ![Veri dizinleri](./media/virtual-machines-windows-portal-sql-create-failover-cluster/20-data-dicrectories.png)

1. Sihirbazı tamamladıktan sonra, kurulum ilk düğümde bir SQL Server FCı yükler.

1. Kurulum, FCı 'yı ilk düğüme başarıyla yükledikten sonra, RDP ile ikinci düğüme bağlanın.

1. **SQL Server yükleme merkezini**açın. **Yükleme**' ye tıklayın.

1. **SQL Server yük devretme kümesine düğüm Ekle**' ye tıklayın. SQL Server 'ı yüklemek ve bu sunucuyu FCı 'ye eklemek için sihirbazdaki yönergeleri izleyin.

   >[!NOTE]
   >SQL Server ile bir Azure Marketi Galeri görüntüsü kullandıysanız, görüntüye SQL Server Araçlar eklenmiştir. Bu görüntüyü kullanmıyorsanız, SQL Server araçlarını ayrı olarak yükleyebilirsiniz. Bkz. [Download SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx).

## <a name="step-5-create-azure-load-balancer"></a>5\. Adım: Azure yük dengeleyici oluşturma

Azure sanal makinelerinde kümeler, aynı anda tek bir küme düğümünde olması gereken bir IP adresini tutmak için bir yük dengeleyici kullanır. Bu çözümde, yük dengeleyici SQL Server FCı için IP adresini tutar.

[Bir Azure yük dengeleyici oluşturun ve yapılandırın](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer).

### <a name="create-the-load-balancer-in-the-azure-portal"></a>Azure portal yük dengeleyiciyi oluşturun

Yük dengeleyiciyi oluşturmak için:

1. Azure portal, sanal makineler ile kaynak grubuna gidin.

1. **+ Ekle**'ye tıklayın. Market 'Te **Load Balancer**arayın. **Load Balancer**' ye tıklayın.

1. **Oluştur**'a tıklayın.

1. Yük dengeleyiciyi şu şekilde yapılandırın:

   - **Ad**: Yük dengeleyiciyi tanımlayan bir ad.
   - **Şunu yazın**: Yük dengeleyici ortak veya özel olabilir. Aynı VNET içinden bir özel yük dengeleyiciye erişilebilir. Azure uygulamalarının çoğu özel yük dengeleyici kullanabilir. Uygulamanızın doğrudan Internet üzerinden SQL Server erişmesi gerekiyorsa, ortak yük dengeleyici kullanın.
   - **Sanal ağ**: Sanal makinelerle aynı ağ.
   - **Alt ağ**: Sanal makinelerle aynı alt ağ.
   - **Özel IP adresi**: SQL Server FCı küme ağı kaynağına atadığınız aynı IP adresi.
   - **abonelik**: Azure aboneliğiniz.
   - **Kaynak grubu**: Sanal makinelerinize göre aynı kaynak grubunu kullanın.
   - **Konum**: Sanal makinelerinizde aynı Azure konumunu kullanın.
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

   - **Ad**: Sistem durumu araştırması için bir ad.
   - **Protokol**: TCP.
   - **Bağlantı noktası**: [Bu adımdaki](#ports)sistem durumu araştırması için güvenlik duvarında oluşturduğunuz bağlantı noktasına ayarlayın. Bu makalede, örnek TCP bağlantı noktasını `59999`kullanır.
   - **Aralık**: 5 saniye.
   - **Sağlıksız eşik**: 2 ardışık başarısızlık.

1. Tamam'a tıklayın.

### <a name="set-load-balancing-rules"></a>Yük Dengeleme kurallarını ayarlama

1. Yük dengeleyici dikey penceresinde **Yük Dengeleme kuralları**' na tıklayın.

1. **+ Ekle**'ye tıklayın.

1. Yük Dengeleme kuralları parametrelerini ayarlayın:

   - **Ad**: Yük Dengeleme kuralları için bir ad.
   - **Ön uç IP adresi**: SQL Server FCı küme ağı kaynağı için IP adresini kullanın.
   - **Bağlantı noktası**: SQL Server FCı TCP bağlantı noktası için ayarlayın. Varsayılan örnek bağlantı noktası 1433 ' dir.
   - **Arka uç bağlantı noktası**: Bu değer, **kayan IP 'yi (doğrudan sunucu dönüşü)** etkinleştirdiğinizde **bağlantı noktası** değeriyle aynı bağlantı noktasını kullanır.
   - **Arka uç havuzu**: Daha önce yapılandırdığınız arka uç havuzu adını kullanın.
   - **Durum araştırması**: Daha önce yapılandırdığınız sistem durumu araştırmasını kullanın.
   - **Oturum kalıcılığı**: Yok.
   - **Boşta kalma zaman aşımı (dakika)** : 4.
   - **Kayan IP (doğrudan sunucu dönüşü)** : Enabled

1. **Tamam**'ı tıklatın.

## <a name="step-6-configure-cluster-for-probe"></a>6\. Adım: Kümeyi araştırma için yapılandırma

PowerShell 'de küme araştırması bağlantı noktası parametresini ayarlayın.

Küme araştırması bağlantı noktası parametresini ayarlamak için aşağıdaki betikteki değişkenleri ortamınızdaki değerlerle güncelleştirin. Betikten açılı ayraçları `<>` kaldırın. 

   ```powershell
   $ClusterNetworkName = "<Cluster Network Name>"
   $IPResourceName = "<SQL Server FCI IP Address Resource Name>" 
   $ILBIP = "<n.n.n.n>" 
   [int]$ProbePort = <nnnnn>

   Import-Module FailoverClusters

   Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```

Yukarıdaki komut dosyasında ortamınızın değerlerini ayarlayın. Aşağıdaki listede değerler açıklanmaktadır:

   - `<Cluster Network Name>`: Ağ için Windows Server yük devretme kümesi adı. **Yük devretme kümesi Yöneticisi** > **ağlarda**ağa sağ tıklayıp **Özellikler**' e tıklayın. Doğru değer **genel** sekmesinde **ad** ' ın altında bulunur. 

   - `<SQL Server FCI IP Address Resource Name>`: SQL Server FCı IP adresi kaynak adı. YükdevretmekümesiYöneticisi > **Roller**' de, SQL Server FCI rolü altında, **sunucu adı**altında, IP adresi kaynağına sağ tıklayın ve **Özellikler**' e tıklayın. Doğru değer **genel** sekmesinde **ad** ' ın altında bulunur. 

   - `<ILBIP>`: ILB IP adresi. Bu adres, Azure portal ıLB ön uç adresi olarak yapılandırılır. Bu ayrıca SQL Server FCı IP adresidir. **Yük devretme kümesi Yöneticisi** içinde bulduğunuz aynı Özellikler sayfasında `<SQL Server FCI IP Address Resource Name>`bulabilirsiniz.  

   - `<nnnnn>`: , Yük dengeleyici sistem durumu araştırmasına yapılandırdığınız araştırma bağlantı noktasıdır. Kullanılmayan tüm TCP bağlantı noktaları geçerlidir. 

>[!IMPORTANT]
>Küme parametresinin alt ağ maskesi TCP IP yayını adresi olmalıdır: `255.255.255.255`.

Küme araştırmasını ayarladıktan sonra, PowerShell 'deki tüm küme parametrelerini görebilirsiniz. Şu betiği çalıştırın:

   ```powershell
   Get-ClusterResource $IPResourceName | Get-ClusterParameter 
  ```

## <a name="step-7-test-fci-failover"></a>7\. Adım: Test FCı yük devretmesi

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

[Uzak Masaüstü ile S2D kurma (Azure)](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/rds-storage-spaces-direct-deployment)

[Depolama alanları doğrudan Ile hiper yakınsama çözümü](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct).

[Depolama alanı doğrudan genel bakış](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview)

[S2D için SQL Server desteği](https://blogs.technet.microsoft.com/dataplatforminsider/2016/09/27/sql-server-2016-now-supports-windows-server-2016-storage-spaces-direct/)
