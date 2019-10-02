---
title: Azure Data Box kullanarak CloudSimple geçişi tarafından Azure VMware çözümü
description: Azure Data Box kullanarak CloudSimple tarafından Azure VMware çözümüne toplu veri geçişi
author: sharaths-cs
ms.author: dikamath
ms.date: 09/27/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 5f19b98fbbbad2f43227bfa2f73eab47bf7b1699
ms.sourcegitcommit: 80da36d4df7991628fd5a3df4b3aa92d55cc5ade
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71817468"
---
# <a name="migrating-data-to-azure-vmware-solution-by-cloudsimple-using-azure-data-box"></a>Azure Data Box kullanarak CloudSimple tarafından Azure VMware çözümüne veri geçirme

Microsoft Azure Data Box bulut çözümü, terabaytlarca veriyi Azure'a hızlı, uygun maliyetli ve güvenilir bir şekilde göndermenizi sağlar. Site özel bir Data Box depolama cihazı gönderilerek güvenli veri aktarımı hızlandırılır. Her depolama cihazının maksimum kullanılabilir depolama kapasite 80 TB'tır ve bölgesel bir kargo firmasıyla veri merkezinize ulaştırılır. Ulaşım sırasında verileri korumalı ve güvenli tutmak için cihaz sağlamlaştırılmıştır.

Azure Data Box kullanarak, VMware verilerinizin toplu olarak özel bulutuna geçişini sağlayabilirsiniz.  Şirket içi vSphere ortamınızdan alınan veriler NFS protokolünü kullanarak Data Box kopyalanır.  Toplu veri geçişi, sanal makinelerin, yapılandırmanın ve ilişkili verilerin zaman içindeki bir kopyasını Data Box kopyalamayı ve Azure 'a sevk edilmesini içerir.

Bu makalede şunları öğreneceksiniz:

* Azure Data Box ayarlanıyor.
* Şirket içi VMware ortamından Data Box NFS kullanarak verileri kopyalama.
* Azure Data Box dönüşü için hazırlanıyor.
* CloudSimple tarafından Azure VMware çözümüne kopyalama için blob verilerini hazırlayın.
* Verileri Azure 'dan özel buluta kopyalama.

## <a name="scenarios"></a>Senaryolar

Azure Data Box, toplu veri geçişi için aşağıdaki senaryolarda kullanılmalıdır.

* Ağ üzerinden temel ve eşitleme farklılıkları olarak kullanılmak üzere CloudSimple tarafından Şirket içinden Azure VMware çözümüne büyük miktarda veri geçirin.
* Büyük miktarda kapalı sanal makineyi (soğuk sanal makineler) geçirin.
* Geliştirme ve test ortamlarını ayarlamak için sanal makine verilerini geçirin.
* Çok sayıda sanal makine şablonu, ISOs, sanal makine diski geçirin.

## <a name="before-you-begin"></a>Başlamadan önce

* Azure portal önkoşulları ve [sıra Data Box](../databox/data-box-deploy-ordered.md) denetleyin.  Sipariş işlemi sırasında, BLOB depolamaya izin veren bir depolama hesabı seçmeniz gerekir.  Data Box aldıktan sonra şirket içi ağınıza bağlanın ve Data Box vSphere yönetim ağınızdan erişilebilen bir IP adresiyle [ayarlayın](../databox/data-box-deploy-set-up.md) .

* Azure VMware çözümünüzün, CloudSimple tarafından sağlandığı aynı Azure bölgesinde bir sanal ağ ve depolama hesabı oluşturun.

* Özel bulutunuzda, [ExpressRoute kullanarak Azure sanal ağını CloudSimple 'A bağlama](virtual-network-connection.md) makalesinde bahsedilen adımlar kullanılarak depolama hesabının oluşturulduğu sanal ağa [Azure sanal ağ bağlantısı](cloudsimple-azure-network-connection.md) oluşturun

## <a name="set-up-azure-data-box-for-nfs"></a>NFS Azure Data Box ayarlama

Azure Data Box makalesinde bahsedilen adımları kullanarak yerel Web Kullanıcı arabirimine bağlanın ve bu makalede [öğreticiye bağlanın: kablo ve Azure Data Box bağlama](../databox/data-box-deploy-set-up.md).  NFS istemcilerine erişime izin vermek için Data Box yapılandırın.

1. Yerel web arabiriminde **Bağlan ve kopyala** sayfasına gidin. **NFS ayarları** bölümünde **NFS istemci erişimi**'ne tıklayın. 

    ![NFS istemci erişimini yapılandırma 1](media/nfs-client-access.png)

2. VMware ESXi konaklarının IP adresini girin ve **Ekle**' ye tıklayın. Bu adımı yineleyerek vSphere kümenizdeki tüm konaklar için erişimi yapılandırabilirsiniz. **Tamam**’a tıklayın.

    ![NFS istemci erişimini yapılandırma 2](media/nfs-client-access2.png)
> [!IMPORTANT]
> **Her zaman kopyalamayı düşündüğünüz dosyalar için paylaşımda bir klasör oluşturun ve ardından dosyaları bu klasöre kopyalayın**. Blok blobu ve sayfa blobu paylaşımları altında oluşturulan klasör, verilerin blob olarak karşıya yüklendiği kapsayıcıyı temsil eder. Dosyaları depolama hesabındaki *root* klasörüne doğrudan kopyalayamazsınız.

Blok blobu ve sayfa blobu paylaşımlarının altında birinci düzeydeki varlıklar kapsayıcılar, ikinci düzeydeki varlıklar ise bloblardır. Azure Dosyaları paylaşımlarında birinci düzeydeki varlıklar paylaşımlar, ikinci düzeydeki varlıklar ise dosyalardır.

Aşağıdaki tabloda, Data Box üzerindeki paylaşımların UNC yolu ve verilerin karşıya yüklendiği Azure Depolama yolu URL'si gösterilmektedir. Son Azure depolama yolu URL'si, UNC paylaşım yolundan türetilebilir.
 
|                   |                                                            |
|-------------------|--------------------------------------------------------------------------------|
| Azure Blok blobları | <li>Paylaşımların UNC yolu: `//<DeviceIPAddress>/<StorageAccountName_BlockBlob>/<ContainerName>/files/a.txt`</li><li>Azure Depolama URL’si: `https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/files/a.txt`</li> |  
| Azure Sayfa blobları  | <li>Paylaşımların UNC yolu: `//<DeviceIPAddres>/<StorageAccountName_PageBlob>/<ContainerName>/files/a.txt`</li><li>Azure Depolama URL’si: `https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/files/a.txt`</li>   |  
| Azure Dosyaları       |<li>Paylaşımların UNC yolu: `//<DeviceIPAddres>/<StorageAccountName_AzFile>/<ShareName>/files/a.txt`</li><li>Azure Depolama URL’si: `https://<StorageAccountName>.file.core.windows.net/<ShareName>/files/a.txt`</li>        |

> [!NOTE]
> VMware verilerini kopyalamak için Azure blok Bloblarını kullanın.

## <a name="mount-nfs-share-as-a-datastore-on-on-premises-vcenter-cluster-and-copy-data"></a>Şirket içi vCenter kümesinde bir veri deposu olarak NFS paylaşma bağlama ve verileri kopyalama

Azure Data Box NFS paylaşımının şirket içi vCenter kümenizde veya verileri kopyalamak için VMware ESXi ana bilgisayarında bir veri deposu olarak bağlanması gerekir.  Bağlandıktan sonra, veriler NFS veri deposuna kopyalanabilir.

1. Şirket içi vCenter sunucunuzda oturum açın.

2. **Veri merkezine**sağ tıklayın, **depolama** Ekle ' yi seçin **Yeni veri deposuna** tıklayın ve **İleri** ' ye tıklayın.

   ![Yeni veri deposu Ekle](media/databox-migration-add-datastore.png)

3. Veri deposu ekleme Sihirbazı 'nın adım 1 ' de, **NFS**türü ' nü seçin.

   ![Yeni veri deposu Ekle-tür](media/databox-migration-add-datastore-type.png)

4. 2\. adımda NFS **3** ' ü seçin ve **İleri**' ye tıklayın.

   ![Yeni veri deposu ekleme-NFS sürümü](media/databox-migration-add-datastore-nfs-version.png)

5. Adım 3 ' te, veri deposu, yol ve sunucu için adı belirtin.  Sunucu için Data Box IP adresini kullanabilirsiniz.  Klasör yolu `/<StorageAccountName_BlockBlob>/<ContainerName>/` biçiminde olacaktır.

   ![Yeni veri deposu ekleme-NFS yapılandırması](media/databox-migration-add-datastore-nfs-configuration.png)

6. 4\. adımda, veri deposunun bağlanmasını istediğiniz ESXi konaklarınızı seçin ve **İleri**' ye tıklayın.  Bir kümede, sanal makinelerin geçişini sağlamak için tüm Konaklar ' ı seçin.

   ![Yeni veri deposu Ekle-Konakları seçin](media/databox-migration-add-datastore-nfs-select-hosts.png)

7. 5\. adımda Özeti gözden geçirin ve **son** ' a tıklayın.

## <a name="copy-data-to-data-box-nfs-datastore"></a>Data Box NFS veri deposuna veri kopyalama

Sanal makineler, yeni veri deposuna geçirilebilir veya kopyalanabilir.  Geçirilmesi gereken tüm kullanılmayan sanal makineler, **Depolama vMotion**KULLANıLARAK Data Box NFS veri deposuna geçirilebilir.  Etkin sanal makineler Data Box NFS veri deposuna **kopyalanabilir** .

* **Taşınabilecek**sanal makinelerin listesini belirler.
* **Kopyalanabilmesi**gereken sanal makinelerin listesini belirler.

### <a name="move-virtual-machine-to-data-box-datastore"></a>Sanal makineyi Data Box veri deposuna Taşı

1. Data Box veri deposuna taşımak istediğiniz sanal makineye sağ tıklayın ve **geçir**' i seçin.

    ![Sanal makineyi geçirme](media/databox-migration-vm-migrate.png)

2. Yalnızca geçiş türü için **depolamayı Değiştir** ' i seçin ve **İleri**' ye tıklayın.

    ![Sanal makineyi geçirme-yalnızca depolama](media/databox-migration-vm-migrate-change-storage.png)

3. Hedef olarak Data Box veri deposunu seçin ve **İleri**' ye tıklayın.

    ![Sanal makineyi geçirme-veri deposu seçme](media/databox-migration-vm-migrate-change-storage-select-datastore.png)

4. Bilgileri gözden geçirin ve **son**' a tıklayın.

5. Diğer sanal makineler için 1 ile 4 arasındaki adımları tekrarlayın.

> [!TIP]
> Aynı güç durumunda birden fazla sanal makine seçebilirsiniz (açık veya kapalı) ve bunları toplu olarak geçirebilirsiniz.

Sanal makine için depolama, Azure Data Box 'ten NFS veri deposuna geçirilir.  Tüm sanal makineler geçirildikten sonra, Azure VMware çözümüne veri geçişi hazırlığı sırasında sanal makinelerin gücünü kapatabilirsiniz.

### <a name="clone-a-virtual-machine-or-a-virtual-machine-template-to-data-box-datastore"></a>Bir sanal makineyi veya sanal makine şablonunu Data Box veri deposuna kopyalama

1. Kopyalamak istediğiniz bir sanal makineye veya sanal makine şablonuna sağ tıklayın.  **Kopyala**, **sanal makineye Kopyala** ' yı seçin

    ![Sanal makine kopyası](media/databox-migration-vm-clone.png)

2. Kopyalanmış sanal makine veya sanal makine şablonu için bir ad seçin.

3. Klonlanan nesneyi yerleştirmek istediğiniz klasörü seçin ve **İleri**' ye tıklayın.

4. Klonlanan nesneyi istediğiniz kümeyi veya kaynak havuzunu seçin ve **İleri**' ye tıklayın.

5. Depolama konumu olarak NFS veri deposu Azure Data Box seçin ve **İleri**' ye tıklayın.

    ![Sanal makine kopyalama-veri deposu seçme](media/databox-migration-vm-clone-select-datastore.png)

6. Kopyalanan nesne için herhangi bir seçeneği özelleştirmek istiyorsanız Özelleştir seçeneklerini belirleyin ve **İleri**' ye tıklayın.

7. Konfigürasyonları gözden geçirin ve **son**' a tıklayın.

8. Ek sanal makineler veya sanal makine şablonları için 1 ile 7 arasındaki adımları yineleyin.

Sanal makineler Azure Data Box ' den NFS veri deposuna kopyalanır ve depolanır.  Sanal makineler kopyalandıktan sonra, verilerin Azure VMware çözümüne geçirilmesi için klonlanan sanal makinelerin gücünün etkinleştirildiğinden emin olun.

### <a name="copy-iso-files-to-data-box-datastore"></a>ISO dosyalarını Data Box veri deposuna Kopyala

1. Şirket içi vCenter Web Kullanıcı arabiriminizden **depolama alanına**gidin.  NFS veri deposu Data Box seçin ve **dosyalar**' a tıklayın.  ISO dosyalarını depolamak için **Yeni bir klasör** oluşturun.

    ![ISO Kopyala-yeni klasör oluştur](media/databox-migration-create-folder.png)

2. ISO dosyalarının depolanacağı klasör için bir ad verin.

3. İçeriğe erişmek için yeni oluşturulan klasöre çift tıklayın.

4. **Dosyaları karşıya yükle** ' ye tıklayın ve karşıya yüklemek Istediğiniz isos 'yi seçin.
    
    ![ISO kopyalama-dosyaları karşıya yükleme](media/databox-migration-upload-iso.png)

> [!TIP]
> Şirket içi veri deposunda ISO dosyalarınız zaten varsa, dosyaları seçip Data Box NFS veri deposuna **kopyalayabilirsiniz** .


## <a name="prepare-azure-data-box-for-return"></a>Azure Data Box Return için hazırla

Tüm sanal makine verileri, sanal makine şablonu verileri ve tüm ISO dosyaları Data Box NFS veri deposuna kopyalandıktan sonra, veri deposu vCenter 'ınızdan kesilebilir.  Veri deposunun bağlantısı kesilmeden önce tüm sanal makinelerin ve sanal makine şablonlarının envanterden kaldırılması gerekir.

### <a name="remove-objects-from-inventory"></a>Nesneleri envanterden kaldır

1. Şirket içi vCenter Web Kullanıcı arabiriminizden **depolama alanına**gidin.  NFS veri deposu Data Box seçin ve **VM 'ler**' e tıklayın.

2. Tüm sanal makinelerin gücünün kapalı olduğundan emin olun.

    ![Sanal makineleri envanterden kaldırma-kapalı](media/databox-migration-select-databox-vm.png)

3. Tüm sanal makineleri seçin, sağ tıklayın ve **stoktan kaldır**' ı seçin.

    ![Sanal makineleri envanterden kaldır](media/databox-migration-remove-vm-from-inventory.png)

4. Üstteki düğmelerden **Klasörler Içinde VM şablonları '** nı seçin ve 3. adımı yineleyin. 

### <a name="remove-azure-data-box-nfs-datastore-from-vcenter"></a>Azure Data Box NFS veri deposunu vCenter 'dan kaldırma

Data Box NFS veri deposu, return için hazırlanmadan önce VMware ESXi konaklarından bağlantısı kesilmelidir.

1. Şirket içi vCenter Web Kullanıcı arabiriminizden **depolama alanına**gidin.

2. NFS veri deposuna Data Box sağ tıklayın ve **veri deposunu**çıkar ' ı seçin.

    ![Data Box veri deposu çıkarın](media/databox-migration-unmount-datastore.png)

3. Veri deposunun bağlı olduğu tüm ESXi Konakları ' nı seçin ve **Tamam**' ı tıklatın.

    ![Data Box veri deposundan çıkarın-Konakları seçin](media/databox-migration-unmount-datastore-select-hosts.png)

4. Uyarıları gözden geçirin ve kabul edin ve **Tamam 'a**tıklayın.

### <a name="prepare-data-box-for-return-and-return-the-data-box"></a>Data Box Return için hazırlayın ve Data Box döndürün

Data Box döndürmek için Azure Data Box geri dönüş makalesinde anlatılan adımları izleyin [ve verileri Azure 'a yükleyin](../databox/data-box-deploy-picked-up.md) .  Azure depolama hesabınıza veri kopyalama durumunu kontrol edin ve durum tamamlandı ' yı seçtikten sonra Azure Depolama hesabınızdaki verileri doğrulayabilirsiniz.

## <a name="copy-data-from-azure-storage-to-azure-vmware-solution-by-cloudsimple"></a>Azure depolama 'dan Azure VMware çözümüne CloudSimple 'a veri kopyalama

Azure Data Box kopyalanmış veriler, Data Box sipariş durumu tamamlandı olarak gösterildikten sonra Azure depolama hesabınızda kullanıma sunulacaktır.  Veriler artık CloudSimple tarafından Azure VMware çözümünüze kopyalanabilir.  Depolama hesabındaki verilerin NFS protokolü kullanılarak özel bulutunuzun vSAN veri deposuna kopyalanması gerekir.  İlk olarak, **AzCopy**kullanarak Azure 'Daki bir Linux sanal makinesindeki bir Linux sanal makinesinde bulunan bir yönetilen diske Blob deposu verileri kopyalayın.  Yönetilen diski NFS protokolü aracılığıyla kullanılabilir hale getirin ve NFS paylaşımından özel bulutunuzda bir veri deposu olarak bağlayın ve verileri kopyalayın.  Bu yöntem, özel bulutunuzda verilerin daha hızlı bir şekilde kopyalanmasını sağlar. 

### <a name="copy-data-to-your-private-cloud-using-a-linux-virtual-machine-and-managed-disks-and-export-as-nfs-share"></a>Linux sanal makinesi ve yönetilen diskler kullanarak özel bulutunuzda verileri kopyalama ve NFS paylaşma olarak dışarı aktarma

1. Azure 'da depolama hesabınızın oluşturulduğu bölgedeki bir [Linux sanal makinesi](../virtual-machines/linux/quick-create-portal.md) oluşturun ve özel bulutunuz Için bir Azure sanal ağ bağlantısı vardır.

2. Blob verilerinden daha büyük olan bir yönetilen disk oluşturun ve [Linux sanal makinenize ekleyin](../virtual-machines/linux/attach-disk-portal.md).  Blob verileri miktarı en büyük yönetilen diskten fazlaysa, verilerin birden çok adımda kopyalanması veya birden çok yönetilen disk kullanılması gerekir.

3. Linux sanal makinesine bağlanın ve yönetilen diski bağlayın.

4. [Linux sanal makinenize AzCopy 'i](../storage/common/storage-use-azcopy-v10.md)yükler.

5. Azure Blob depolamaınızdan, AzCopy kullanarak yönetilen diske verileri indirin.  Komut sözdizimi: `azcopy copy "https://<storage-account-name>.blob.core.windows.net/<container-name>/*" "<local-directory-path>/"`.  @No__t-0 ' i Azure depolama hesabı adınızla değiştirin ve Azure Data Box kullanılarak kopyalanmış verileri içeren kapsayıcı ile `<container-name>` ' i kullanın.

6. Linux sanal makinenize NFS sunucusu 'NU yükler.

    1. Ubuntu/detem dağıtımında: `sudo apt install nfs-kernel-server`.
    2. Enterprise Linux dağıtımında: `sudo yum install nfs-utils`.

7. Yönetilen diskinizde, Azure Blob Mağazası 'ndaki verilerin kopyalandığı klasörün iznini değiştirin.  NFS paylaşma olarak dışarı aktarmak istediğiniz tüm klasörler için izinleri değiştirin.

    ```bash
    chmod -R 755 /<folder>/<subfolder>
    chown nfsnobody:nfsnobody /<folder>/<subfolder>
    ```

8. @No__t-0 dosyasını düzenleyerek, istemci IP adreslerine erişimi NFS paylaşımıyla ilgili izinler atayın.

    ```bash
    sudo vi /etc/exports
    ```
    
    Özel bulutunuzun her ESXi ana bilgisayar IP 'si için dosyada aşağıdaki satırları girin.  Birden çok klasör için paylaşımlar oluşturuyorsanız, tüm klasörleri ekleyin.

    ```bash
    /<folder>/<subfolder> <ESXiNode1IP>(rw,sync,no_root_squash,no_subtree_check)
    /<folder>/<subfolder> <ESXiNode2IP>(rw,sync,no_root_squash,no_subtree_check)
    .
    .
    ```

9. @No__t-0 komutunu kullanarak NFS paylaşımlarını dışarı aktarın.

10. NFS çekirdek sunucusunu `sudo systemctl restart nfs-kernel-server` komutunu kullanarak yeniden başlatın.


### <a name="mount-linux-virtual-machine-nfs-share-as-a-datastore-on-private-cloud-vcenter-cluster-and-copy-data"></a>Linux sanal makinesi NFS paylaşımından özel bulut vCenter kümesinde veri deposu olarak bağlama ve verileri kopyalama

Linux sanal makinenizden NFS paylaşımının, verileri kopyalamak için özel bulut vCenter kümenize bir veri deposu olarak bağlanması gerekir.  Bağlandıktan sonra, veriler NFS veri deposundan özel bulut vSAN veri deposuna kopyalanabilir.

1. Özel bulut vCenter sunucunuzda oturum açın.

2. **Veri merkezine**sağ tıklayın, **depolama** Ekle ' yi seçin **Yeni veri deposuna** tıklayın ve **İleri** ' ye tıklayın.

   ![Yeni veri deposu Ekle](media/databox-migration-add-datastore.png)

3. Veri deposu ekleme Sihirbazı 'nın adım 1 ' de, **NFS**türü ' nü seçin.

   ![Yeni veri deposu Ekle-tür](media/databox-migration-add-datastore-type.png)

4. 2\. adımda NFS **3** ' ü seçin ve **İleri**' ye tıklayın.

   ![Yeni veri deposu ekleme-NFS sürümü](media/databox-migration-add-datastore-nfs-version.png)

5. Adım 3 ' te, veri deposu, yol ve sunucu için adı belirtin.  Sunucu için Linux sanal makinenizin IP adresini kullanabilirsiniz.  Klasör yolu `/<folder>/<subfolder>/` biçiminde olacaktır.

   ![Yeni veri deposu ekleme-NFS yapılandırması](media/databox-migration-add-datastore-nfs-configuration.png)

6. 4\. adımda, veri deposunun bağlanmasını istediğiniz ESXi konaklarınızı seçin ve **İleri**' ye tıklayın.  Bir kümede, sanal makinelerin geçişini sağlamak için tüm Konaklar ' ı seçin.

   ![Yeni veri deposu Ekle-Konakları seçin](media/databox-migration-add-datastore-nfs-select-hosts.png)

7. 5\. adımda Özeti gözden geçirin ve **son** ' a tıklayın.

### <a name="add-virtual-machines-and-virtual-machine-templates-from-nfs-datastore-to-the-inventory"></a>NFS veri deposundan stoğa sanal makineler ve sanal makine şablonları ekleyin

1. Özel bulut vCenter Web Kullanıcı arabiriminizden **depolama alanına**gidin.  Linux sanal makine NFS veri deposu ' nu seçin ve **dosyalar**' a tıklayın.

    ![NFS veri deposundaki dosyaları seçme](media/databox-migration-datastore-select-files.png)

2. Bir sanal makine veya sanal makine şablonu içeren bir klasör seçin.  Ayrıntılar bölmesinden bir sanal makine için `.vmx` dosyası veya bir sanal makine şablonu için `.vmtx` dosyası seçin.

3. Sanal makineyi özel bulut vCenter 'a kaydetmek için **VM 'Yi kaydet** ' e tıklayın.

    ![Sanal makineyi Kaydet](media/databox-migration-datastore-register-vm.png)

4. Sanal makinenin kaydedilmesini istediğiniz veri merkezini, klasörü ve küme/kaynak havuzunu seçin.

4. Tüm sanal makineler ve sanal makine şablonları için 3. ve 4. adımı yineleyin.

5. ISO dosyalarını içeren klasöre gidin.  ISO dosyaları ' nı seçin ve vSAN veri deposundaki bir klasöre **kopyalayın** .

Sanal makineler ve sanal makine şablonları artık özel bulut vCenter 'unuzda kullanılabilir.  Bu sanal makineler, ' i kapatmadan önce NFS veri deposu 'ndan vSAN veri deposuna taşınmalıdır.  Sanal makinelerin Depolama vMotion ' i ve sanal makine için hedef olarak vSAN veri deposu ' nu seçebilirsiniz.

Sanal makine şablonları, Linux sanal makine NFS veri deposundan vSAN veri deposuna kopyalanmalıdır.

### <a name="clean-up-of-your-linux-virtual-machine"></a>Linux sanal makinenizi Temizleme

Tüm veriler özel buluta kopyalandıktan sonra, NFS veri deposunu özel Bulutınızdan kaldırabilirsiniz.

1. Tüm sanal makinelerin ve şablonların vSAN veri deposuna taşındığından ve kopyalandığından emin olun.

2. Tüm sanal makine şablonlarını envanterden NFS veri deposundan kaldırın.

3. Linux sanal makine veri deposunu özel bulut vCenter 'ınızdan çıkarın.

4. Sanal makineyi ve yönetilen diski Azure 'dan silin.

5. Depolama hesabınızda Azure Data Box kullanarak aktarılan verileri tutmak istemiyorsanız, Azure Depolama hesabını silin.  
    


## <a name="next-steps"></a>Sonraki adımlar

* [Azure Data Box](../databox/data-box-overview.md) hakkında daha fazla bilgi edinin
* [İş yüklerini özel buluta geçirmeye](migrate-workloads.md) yönelik farklı seçenekler hakkında daha fazla bilgi edinin
