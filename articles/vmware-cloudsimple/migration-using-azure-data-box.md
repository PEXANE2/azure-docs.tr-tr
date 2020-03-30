---
title: Azure VMware Çözümü - Azure Veri Kutusu'ni kullanarak geçiş
description: Azure VMware Çözümüne toplu veri geçirmek için Azure Veri Kutusu'nun nasıl kullanılacağı.
author: sharaths-cs
ms.author: dikamath
ms.date: 09/27/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 65167169248d83ebfec2c49c308673ec9315934e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77019766"
---
# <a name="migrating-data-to-azure-vmware-solution-by-using-azure-data-box"></a>Azure Veri Kutusu'yu kullanarak verileri Azure VMware Çözümüne geçirme

Microsoft Azure Veri Kutusu bulut çözümü, terabaytlarca (TB) verileri Azure'a hızlı, ucuz ve güvenilir bir şekilde göndermenize olanak tanır. Site özel bir Data Box depolama cihazı gönderilerek güvenli veri aktarımı hızlandırılır. Her depolama aygıtı 80 TB maksimum kullanılabilir depolama kapasitesine sahiptir ve bölgesel bir taşıyıcı tarafından veri merkezinize taşınır. Cihaz, aktarım sırasında verilerinizi korumak ve güvence altına almak için sağlam bir kasaya sahiptir.

Veri Kutusu'yu kullanarak, VMware verilerinizi özel bulutunuza toplu olarak aktarabilirsiniz. Şirket içi VMware vSphere ortamınızdaki veriler, Ağ Dosya Sistemi (NFS) protokolü aracılığıyla Veri Kutusu'na kopyalanır. Toplu veri aktarımı, sanal makinelerin, yapılandırmanın ve ilişkili verilerin zamanında bir kopyasını Veri Kutusu'na kaydetmeyi ve ardından el ile Azure'a göndermeyi içerir.

Bu makalede, hakkında bilgi edinin:

* Veri Kutusu'nun ayarlanması.
* Şirket içi VMware ortamından NFS aracılığıyla Veri Kutusu'na veri kopyalama.
* Veri Kutusunun iadesi için hazırlanıyor.
* Azure VMware Solution'a kopyalamak için blob verileri hazırlama.
* Verileri Azure'dan özel bulutunuza kopyalama.

## <a name="scenarios"></a>Senaryolar

Toplu veri geçişi için aşağıdaki senaryolarda Veri Kutusu'nu kullanın:

* Şirket içinde azure VMware Çözümüne büyük miktarda veri geçirmek için. Bu yöntem bir taban çizgisi belirler ve ağ üzerindeki farklılıkları eşitler.
* Kapalı olan çok sayıda sanal makineyi (soğuk sanal makineler) geçirmek için.
* Geliştirme ve test ortamları ayarlamak için sanal makine verilerini geçirmek için.
* Çok sayıda sanal makine şablonu, ISO dosyası ve sanal makine diski geçirmek için.

## <a name="before-you-begin"></a>Başlamadan önce

* Azure portalınız aracılığıyla ön koşulları kontrol edin ve [Veri Kutusu siparişini verin.](../databox/data-box-deploy-ordered.md) Sipariş işlemi sırasında Blob depolamasını sağlayan bir depolama hesabı seçmeniz gerekir. Veri Kutusu aygıtını aldıktan sonra, şirket içi ağınıza bağlayın ve aygıtı vSphere yönetim ağınızdan erişilebilen bir IP adresiyle [kurun.](../databox/data-box-deploy-set-up.md)

* Azure VMware Çözümünüzün sağlandığı aynı bölgede bir sanal ağ ve depolama hesabı oluşturun.

* Azure [sanal](cloudsimple-azure-network-connection.md) ağdaki adımları takip ederek ExpressRoute kullanarak [CloudSimple'a bağlanın](virtual-network-connection.md)adımlarını izleyerek özel bulutunuzdan depolama hesabının oluşturulduğu sanal ağa bir Azure sanal ağ bağlantısı oluşturun.

## <a name="set-up-data-box-for-nfs"></a>NFS için Veri Kutusu'nun ayarlanış

Öğretici: Kablo ve [Azure Veri Kutunuza bağlanın](../databox/data-box-deploy-set-up.md)"Cihazınıza bağlanın" bölümündeki adımları izleyerek Veri Kutunuza yerel web Web UI'nıza bağlanın.  NFS istemcilerine erişime izin verecek şekilde Veri Kutusu'na yapılandırın:

1. Yerel web arama sürümünde **Bağlan ve kopya** la sayfasına gidin. **NFS ayarları**altında, **NFS istemci erişimini**seçin. 

    ![NFS istemci erişimini yapılandırma 1](media/nfs-client-access.png)

2. VMware ESXi ana bilgisayarlarının IP adresini girin ve **Ekle'yi**seçin. Bu adımı yineleyerek vSphere kümenizdeki tüm ana bilgisayarların erişimini yapılandırabilirsiniz. **Tamam'ı**seçin.

    ![NFS istemci erişimini yapılandırma 2](media/nfs-client-access2.png)
> [!IMPORTANT]
> **Her zaman paylaşım altında kopyalamak istediğiniz dosyalar için bir klasör oluşturun ve sonra dosyaları bu klasöre kopyalayın.** Blok blobu ve sayfa blobu paylaşımları altında oluşturulan klasör, verilerin blob olarak karşıya yüklendiği kapsayıcıyı temsil eder. Dosyaları doğrudan depolama hesabındaki *kök* klasörüne kopyalayamazsınız.

Blok blobu ve sayfa blobu paylaşımlarının altında birinci düzeydeki varlıklar kapsayıcılar, ikinci düzeydeki varlıklar ise bloblardır. Azure Dosyaları'nın paylaşımları altında, birinci düzey varlıklar paylaşımdır ve ikinci düzey varlıklar dosyadır.

Aşağıdaki tabloda, Data Box üzerindeki paylaşımların UNC yolu ve verilerin karşıya yüklendiği Azure Depolama yolu URL'si gösterilmektedir. Son Azure depolama yolu URL'si, UNC paylaşım yolundan türetilebilir.
 
|                   |                                                            |
|-------------------|--------------------------------------------------------------------------------|
| Azure Blok blobları | <li>Paylaşımların UNC yolu: `//<DeviceIPAddress>/<StorageAccountName_BlockBlob>/<ContainerName>/files/a.txt`</li><li>Azure Depolama URL’si: `https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/files/a.txt`</li> |  
| Azure Sayfa blobları  | <li>Paylaşımların UNC yolu: `//<DeviceIPAddres>/<StorageAccountName_PageBlob>/<ContainerName>/files/a.txt`</li><li>Azure Depolama URL’si: `https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/files/a.txt`</li>   |  
| Azure Dosyaları       |<li>Paylaşımların UNC yolu: `//<DeviceIPAddres>/<StorageAccountName_AzFile>/<ShareName>/files/a.txt`</li><li>Azure Depolama URL’si: `https://<StorageAccountName>.file.core.windows.net/<ShareName>/files/a.txt`</li>        |

> [!NOTE]
> VMware verilerini kopyalamak için Azure Blok bloblarını kullanın.

## <a name="mount-the-nfs-share-as-a-datastore-on-your-on-premises-vcenter-cluster-and-copy-the-data"></a>Şirket içi vCenter kümenizde NFS paylaşımını veri deposu olarak monte edin ve verileri kopyalayın

Verileri NFS veri deposuna kopyalamak için veri kutunuzdaki NFS paylaşımı, şirket içi vCenter kümenize veya VMware ESXi ana bilgisayarınıza veri deposu olarak monte edilmelidir:

1. Şirket içi vCenter sunucunuzda oturum açın.

2. Datacenter'ı sağ tıklatın, **Depolama'yı**seçin, **Yeni Veri Deposu'yu**seçin ve sonra **İleri'yi**seçin. **Datacenter**

   ![Yeni veri deposu ekleme](media/databox-migration-add-datastore.png)

3. Veri Deposu Ekle sihirbazının 1. **NFS** **Type**

   ![Yeni datastore ekle - yazın](media/databox-migration-add-datastore-type.png)

4. Sihirbazın 2. **NFS 3** **Next**

   ![Yeni datastore ekle - NFS sürümü](media/databox-migration-add-datastore-nfs-version.png)

5. Sihirbazın 3. Sunucu için Veri Kutunuzun IP adresini kullanabilirsiniz. Klasör yolu `/<StorageAccountName_BlockBlob>/<ContainerName>/` biçimde olacaktır.

   ![Yeni datastore ekleme - NFS yapılandırması](media/databox-migration-add-datastore-nfs-configuration.png)

6. Sihirbazın 4. **Next**  Bir kümede, sanal makinelerin geçişini sağlamak için tüm ana bilgisayarları seçin.

   ![Yeni datastore ekle - Ana bilgisayarları seçin](media/databox-migration-add-datastore-nfs-select-hosts.png)

7. Sihirbazın 5. **Finish**

## <a name="copy-data-to-the-data-box-nfs-datastore"></a>Verileri Veri Kutusu NFS veri deposuna kopyalama

Sanal makineler yeni veri deposuna geçirilebilir veya klonlanabilir.  Geçiş yapmak istediğiniz kullanılmayan sanal makineler, **depolama vMotion** seçeneğini kullanarak Veri Kutusu NFS veri deposuna geçirilebilir. Etkin sanal makineler Veri Kutusu NFS veri deposuna klonlanabilir.

* **Taşınabilen**sanal makineleri tanımlayın ve listeleyin.
* Klonlanmış olması gereken sanal makineleri tanımlayın ve **listelenin.**

### <a name="move-a-virtual-machine-to-a-data-box-datastore"></a>Sanal makineyi Veri Kutusu veri deposuna taşıma

1. Veri Kutusu veri deposuna taşımak istediğiniz sanal makineyi sağ tıklatın ve sonra **Geçir'i**seçin.

    ![Sanal makineyi geçirin](media/databox-migration-vm-migrate.png)

2. Yalnızca geçiş türü için **depolamayı değiştir'i** seçin ve ardından **İleri'yi**seçin.

    ![Sanal makineyi geçirin - yalnızca depolama](media/databox-migration-vm-migrate-change-storage.png)

3. Hedef olarak **Databox-Datastore'u** seçin ve sonra **İleri'yi**seçin.

    ![Sanal makineyi geçirin - datastore'u seçin](media/databox-migration-vm-migrate-change-storage-select-datastore.png)

4. Bilgileri gözden geçirin ve **Finish'i**seçin.

5. Ek sanal makineler için 1'den 4'e kadar olan adımları yineleyin.

> [!TIP]
> Aynı güç durumunda olan (açık veya kapalı) birden çok sanal makine seçebilir ve bunları toplu olarak geçirebilirsiniz.

Sanal makine Veri Kutusu'ndan NFS veri deposuna geçirilir. Tüm sanal makineler geçirildikten sonra, azure VMware Çözümüne veri geçişine hazırlık olarak etkin sanal makineleri kapatabilir (kapatabilirsiniz).

### <a name="clone-a-virtual-machine-or-a-virtual-machine-template-to-the-data-box-datastore"></a>Veri Kutusu veri deposuna sanal bir makine veya sanal makine şablonu oluşturma

1. Klonlamak istediğiniz sanal makineyi veya sanal makine şablonunu sağ tıklatın. Sanal **Makineye Klon** > **Klon'u**seçin.

    ![Sanal makine klonu](media/databox-migration-vm-clone.png)

2. Klonlanan sanal makine veya sanal makine şablonu için bir ad seçin.

3. Klonlanan nesneyi koymak istediğiniz klasörü seçin ve sonra **İleri'yi**seçin.

4. Klonlanan nesneyi koymak istediğiniz kümeyi veya kaynak havuzunu seçin ve sonra **İleri'yi**seçin.

5. Depolama konumu olarak **Databox-Datastore'u** seçin ve ardından **İleri'yi**seçin.

    ![Sanal makine klonu - datastore seçin](media/databox-migration-vm-clone-select-datastore.png)

6. Klonlanan nesne için herhangi bir seçeneği özelleştirmek istiyorsanız, özelleştirme seçeneklerini seçin ve sonra **İleri'yi**seçin.

7. Yapılandırmaları gözden geçirin ve **Finish'i**seçin.

8. Ek sanal makineler veya sanal makine şablonları için 1'den 7'ye kadar olan adımları yineleyin.

Sanal makineler klonlanacak ve Data Box'tan NFS veri deposunda depolanacaktır. Sanal makineler klonlandıktan sonra, Azure VMware Çözümüne veri geçişine hazırlık olarak kapatıldıklarından emin olun.

### <a name="copy-iso-files-to-the-data-box-datastore"></a>ISO dosyalarını Veri Kutusu veri deposuna kopyalama

1. Şirket içi vCenter web Kullanıcı Arama Hizmeti'nden **Depolama**alanına gidin.  **Databox-Datastore'u** seçin ve **ardından Dosyalar'ı**seçin. ISO dosyalarını depolamak için yeni bir klasör oluşturun.

    ![COPY ISO - yeni klasör oluşturmak](media/databox-migration-create-folder.png)

2. ISO dosyalarının depolanacağı klasör için bir ad sağlayın.

3. Açmak için yeni oluşturulan klasörü çift tıklatın.

4. **Dosyaları Yükle'yi** ve ardından yüklemek istediğiniz ISO dosyalarını seçin.
    
    ![COPY ISO - dosya yükleme](media/databox-migration-upload-iso.png)

> [!TIP]
> Şirket içi veri deponuzda zaten ISO dosyaları varsa, dosyaları veri kutusu NFS veri deposuna **kopyalamak için** dosyaları ve Kopyala'yı seçebilirsiniz.


## <a name="prepare-data-box-for-return"></a>İade için Veri Kutusu Hazırlama

Tüm sanal makine verileri, sanal makine şablonu verileri ve tüm ISO dosyaları Data Box NFS datastore'a kopyalandıktan sonra, veri deposunun bağlantısını vCenter'ınızdan kesebilirsiniz. Veri deposunun bağlantısını kesmeden önce tüm sanal makineler ve sanal makine şablonları envanterden çıkarılmalıdır.

### <a name="remove-objects-from-inventory"></a>Nesneleri stoktan kaldırma

1. Şirket içi vCenter web Kullanıcı Arama Hizmeti'nden **Depolama**alanına gidin. **Databox-Datastore'u** seçin ve ardından **VM'leri**seçin.

    ![Sanal makineleri envanterden çıkarın - kapalı](media/databox-migration-select-databox-vm.png)

2. Tüm sanal makinelerin kapatıldığından emin olun.

3. Tüm sanal makineleri seçin, sağ tıklatın ve ardından **envanterden kaldır'ı**seçin.

    ![Sanal makineleri envanterden kaldırma](media/databox-migration-remove-vm-from-inventory.png)

4. **Klasörlerde VM Şablonları'nı** seçin ve ardından adım 3'ü tekrarlayın.

### <a name="remove-the-data-box-nfs-datastore-from-vcenter"></a>Veri Kutusu NFS veri deposunu vCenter'dan kaldırma

Veri Kutusu NFS veri deposu, iadeye hazırlanmadan önce VMware ESXi ana bilgisayarlarından bağlantı kesilmelidir.

1. Şirket içi vCenter web Kullanıcı Arama Hizmeti'nden **Depolama**alanına gidin.

2. **Databox-Datastore'a** sağ tıklayın ve **Unmount Datastore'u**seçin.

    ![Unmount Veri Kutusu veri deposu](media/databox-migration-unmount-datastore.png)

3. Veri deposunun monte edildiği tüm ESXi ana bilgisayarlarını seçin ve **Tamam'ı**seçin.

    ![Veri Kutusu veri deposunun montajına devam edin - ana bilgisayarları seçin](media/databox-migration-unmount-datastore-select-hosts.png)

4. İnceleme ve herhangi bir uyarı kabul ve **Tamam**seçin.

### <a name="prepare-data-box-for-return-and-then-return-it"></a>Veri Kutusunu iade için hazırlayın ve iade edin

Azure Veri Kutusu'nu Iade etme makalesinde özetlenen adımları izleyin ve Veri Kutusu'nu döndürmek [için veri yüklemeyi Azure'a doğrulayın.](../databox/data-box-deploy-picked-up.md) Azure depolama hesabınızda veri kopyasının durumunu kontrol edin. Durum tamamlandıktan sonra, Azure depolama hesabınızdaki verileri doğrulayabilirsiniz.

## <a name="copy-data-from-azure-storage-to-azure-vmware-solution"></a>Azure depolamadan Azure VMware Çözümüne verileri kopyalama

Veri Kutusu cihazınıza kopyalanan veriler, Veri Kutunuzun sipariş durumu tamamlandıktan sonra Azure depolama hesabınızda kullanılabilir. Veriler artık Azure VMware Çözümünüze kopyalanabilir. Depolama hesabındaki veriler, NFS protokolü kullanılarak özel bulutunuzun vSAN veri deposuna kopyalanmalıdır. 

İlk olarak, **AzCopy**kullanarak Blob depolama verilerini Azure'daki bir Linux sanal makinesinde yönetilen bir diske kopyalayın. Yönetilen diski NFS aracılığıyla kullanılabilir hale getirin, NFS paylaşımını özel bulutunuzda veri deposu olarak monte edin ve ardından verileri kopyalayın. Bu yöntem, verilerin özel bulutunuza daha hızlı kopyalanmasını sağlar.

### <a name="copy-data-to-your-private-cloud-using-a-linux-virtual-machine-and-managed-disks-and-then-export-as-nfs-share"></a>Linux sanal makinesi ve yönetilen diskler kullanarak verileri özel bulutunuza kopyalayın ve NFS paylaşımı olarak dışa aktarın

1. Azure'da depolama hesabınızın oluşturulduğu ve özel bulutunuzla azure sanal ağ bağlantısıolan aynı bölgede bir [Linux sanal makinesi](../virtual-machines/linux/quick-create-portal.md) oluşturun.

2. Depolama kapasitesi blob veri miktarından daha büyük olan yönetilen bir disk oluşturun ve [Linux sanal makinenize takın.](../virtual-machines/linux/attach-disk-portal.md)  Blob veri miktarı, kullanılabilir en büyük yönetilen diskin kapasitesinden büyükse, verilerin birden çok adımda veya birden çok yönetilen disk kullanılarak kopyalanmalıdır.

3. Linux sanal makinesine bağlanın ve yönetilen diski monte edin.

4. [Linux sanal makinenize AzCopy yükleyin.](../storage/common/storage-use-azcopy-v10.md)

5. AzCopy kullanarak Verileri Azure Blob depolama alanınızdan yönetilen diske indirin.  Komut sözdizimi: `azcopy copy "https://<storage-account-name>.blob.core.windows.net/<container-name>/*" "<local-directory-path>/"`.  Azure `<storage-account-name>` depolama hesabı adınız `<container-name>` ve Veri Kutusu üzerinden kopyalanan verileri tutan kapsayıcıyla değiştirin.

6. NFS sunucusunu Linux sanal makinenize yükleyin:

    - Bir Ubuntu/Debian dağılımında: `sudo apt install nfs-kernel-server`.
    - Kurumsal Linux dağıtımında: `sudo yum install nfs-utils`.

7. Azure Blob depolama sundaki verilerin kopyalandığı yönetilen diskinizdeki klasörün izinini değiştirin.  NFS payı olarak dışa aktarmak istediğiniz tüm klasörlerin izinlerini değiştirin.

    ```bash
    chmod -R 755 /<folder>/<subfolder>
    chown nfsnobody:nfsnobody /<folder>/<subfolder>
    ```

8. `/etc/exports` Dosyayı düzenleyerek NFS paylaşımına erişmek için istemci IP adresleri için izinler atayın.

    ```bash
    sudo vi /etc/exports
    ```
    
    Özel bulutunuzun her ESXi ana bilgisayarı IP'si için dosyaya aşağıdaki satırları girin.  Birden çok klasör için paylaşım lar oluşturuyorsanız, tüm klasörleri ekleyin.

    ```bash
    /<folder>/<subfolder> <ESXiNode1IP>(rw,sync,no_root_squash,no_subtree_check)
    /<folder>/<subfolder> <ESXiNode2IP>(rw,sync,no_root_squash,no_subtree_check)
    .
    .
    ```

9. Komutu kullanarak NFS `sudo exportfs -a` hisselerini dışa aktarın.

10. Komutunu kullanarak NFS çekirdek sunucusunu yeniden başlatın. `sudo systemctl restart nfs-kernel-server`


### <a name="mount-the-linux-virtual-machine-nfs-share-as-a-datastore-on-a-private-cloud-vcenter-cluster-and-then-copy-data"></a>Linux sanal makine NFS'nin özel bir bulut vCenter kümesinde veri deposu olarak paylaştığını ve ardından verileri kopyalamasını

Linux sanal makinenizden NFS payı, özel bulut vCenter kümenizde bir veri deposu olarak monte edilmelidir. Monte edildikten sonra veriler NFS veri deposundan özel bulut vSAN veri deposuna kopyalanabilir.

1. Özel bulut vCenter sunucunuzda oturum açın.

2. Datacenter'ı sağ tıklatın, **Depolama'yı**seçin, **Yeni Veri Deposu'yu**seçin ve sonra **İleri'yi**seçin. **Datacenter**

   ![Yeni veri deposu ekleme](media/databox-migration-add-datastore.png)

3. Veri Deposu Ekle sihirbazının 1. **NFS**

   ![Yeni datastore ekle - yazın](media/databox-migration-add-datastore-type.png)

4. Sihirbazın 2. **NFS 3** **Next**

   ![Yeni datastore ekle - NFS sürümü](media/databox-migration-add-datastore-nfs-version.png)

5. Sihirbazın 3.  Sunucu için Linux sanal makinenizin IP adresini kullanabilirsiniz.  Klasör yolu `/<folder>/<subfolder>/` biçimde olacaktır.

   ![Yeni datastore ekleme - NFS yapılandırması](media/databox-migration-add-datastore-nfs-configuration.png)

6. Sihirbazın 4. **Next**  Bir kümede, sanal makinelerin geçişini sağlamak için tüm ana bilgisayarları seçin.

   ![Yeni datastore ekle - Ana bilgisayarları seçin](media/databox-migration-add-datastore-nfs-select-hosts.png)

7. Sihirbazın adım 5'inde, özeti gözden geçirin ve **ardından Finish'i**seçin.

### <a name="add-virtual-machines-and-virtual-machine-templates-from-an-nfs-datastore-to-the-inventory"></a>Bir NFS veri deposundan envantere sanal makineler ve sanal makine şablonları ekleyin

1. Özel bulut vCenter web Kullanıcı Aracı'nızdan **Storage'a**gidin.  Bir Linux sanal makine NFS datastore seçin ve sonra **Dosyalar**seçin.

    ![NFS datastore'dan dosyaları seçme](media/databox-migration-datastore-select-files.png)

2. Sanal makine veya sanal makine şablonu içeren bir klasör seçin.  Ayrıntılar bölmesinde, sanal makine için bir .vmx dosyası veya sanal makine şablonu için .vmtx dosyası seçin.

3. Sanal makineyi özel bulut vCenter'ınıza kaydetmek için **VM** kaydedin'i seçin.

    ![Sanal makineyi kaydettirin](media/databox-migration-datastore-register-vm.png)

4. Sanal makinenin kaydolmasını istediğiniz veri merkezi, klasör ve küme/kaynak havuzunu seçin.

4. Tüm sanal makineler ve sanal makine şablonları için 3 ve 4 adımlarını yineleyin.

5. ISO dosyalarını içeren klasöre gidin.  ISO dosyalarını seçin **Copy to** ve ardından dosyaları vSAN veri deponuzdaki bir klasöre kopyalamak için Kopyala'yı seçin.

Sanal makineler ve sanal makine şablonları artık özel bulut vCenter'ınızda kullanılabilir. Bu sanal makineler, açmadan önce NFS veri deposundan vSAN veri deposuna taşınmalıdır. **Depolama vMotion** seçeneğini kullanabilir ve sanal makineler için hedef olarak vSAN veri mağazasını seçebilirsiniz.

Sanal makine şablonları Linux sanal makineniz NFS datastore'dan vSAN veri mağazanıza klonlanmalıdır.

### <a name="clean-up-your-linux-virtual-machine"></a>Linux sanal makinenizi temizleyin

Tüm veriler özel bulutunuza kopyalandıktan sonra, NFS veri deposunu özel bulutunuzdan kaldırabilirsiniz:

1. Tüm sanal makinelerin ve şablonların vSAN veri mağazanıza taşındığından ve klonlandırıldığından emin olun.

2. NFS veri deposundaki tüm sanal makine şablonlarını stoktan kaldırın.

3. Linux sanal makine veri mağazasını özel bulut vCenter'ınızdan boşaltın.

4. Sanal makineyi ve yönetilen diski Azure'dan silin.

5. Veri Kutusu tarafından aktarılan verileri depolama hesabınızda tutmak istemiyorsanız, Azure depolama hesabını silin.  
    


## <a name="next-steps"></a>Sonraki adımlar

* [Veri Kutusu](../databox/data-box-overview.md)hakkında daha fazla bilgi edinin.
* [İş yüklerini özel bulutunuza geçirmek](migrate-workloads.md)için farklı seçenekler hakkında daha fazla bilgi edinin.
