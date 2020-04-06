---
title: Azure portalını kullanarak VM'leri geri yükleme
description: Azure portalını kullanarak bir kurtarma noktasından bir Azure sanal makinesini geri yükleme
ms.reviewer: geg
ms.topic: conceptual
ms.date: 09/17/2019
ms.openlocfilehash: 4bb0a07db39f5f9953a1e41e55b8be28fcace71b
ms.sourcegitcommit: 67addb783644bafce5713e3ed10b7599a1d5c151
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2020
ms.locfileid: "80668830"
---
# <a name="how-to-restore-azure-vm-data-in-azure-portal"></a>Azure portalında Azure VM verileri nasıl geri yüklenir?

Bu makalede, [Azure Yedekleme](backup-overview.md) Kurtarma Hizmetleri kasalarında depolanan kurtarma noktalarından Azure VM verilerinin nasıl geri yüklenir.

## <a name="restore-options"></a>Geri yükleme seçenekleri

Azure Yedekleme, VM'yi geri yüklemenin çeşitli yollarını sağlar.

**Geri yükleme seçeneği** | **Şey**
--- | ---
**Yeni bir VM oluşturun** | Hızlı bir şekilde oluşturur ve bir geri yükleme noktasından temel bir VM çalışır hale alır.<br/><br/> VM için bir ad belirtebilir, kaynak grubunu ve yerleştirileceği sanal ağı (VNet) seçebilir ve geri yüklenen VM için bir depolama hesabı belirtebilirsiniz. Yeni VM, kaynak VM ile aynı bölgede oluşturulmalıdır.
**Diski geri yükle** | Daha sonra yeni bir VM oluşturmak için kullanılabilecek bir VM diski geri yükler.<br/><br/> Azure Yedekleme, bir VM özelleştirmenize ve oluşturmanıza yardımcı olacak bir şablon sağlar. <br/><br> Geri yükleme işi, özel VM ayarlarını belirtmek ve bir VM oluşturmak için karşıdan yükleyebileceğiniz ve kullanabileceğiniz bir şablon oluşturur.<br/><br/> Diskler, belirttiğiniz Kaynak Grubu'na kopyalanır.<br/><br/> Alternatif olarak, diski varolan bir VM'ye ekleyebilir veya PowerShell kullanarak yeni bir VM oluşturabilirsiniz.<br/><br/> Bu seçenek, VM'yi özelleştirmek, yedekleme sırasında olmayan yapılandırma ayarlarını eklemek veya şablon veya PowerShell kullanılarak yapılandırılması gereken ayarlar eklemek istiyorsanız yararlıdır.
**Mevcut olanı değiştir** | Bir diski geri yükleyebilir ve varolan VM'deki bir diski değiştirmek için kullanabilirsiniz.<br/><br/> Geçerli VM var olmalıdır. Silinmişse, bu seçenek kullanılamaz.<br/><br/> Azure Yedekleme, diski değiştirmeden önce varolan VM'nin anlık görüntüsünü alır ve belirttiğiniz hazırlama konumunda saklar. VM'ye bağlı varolan diskler seçili geri yükleme noktasıyla değiştirilir.<br/><br/> Anlık görüntü kasaya kopyalanır ve saklama ilkesine uygun olarak saklanır. <br/><br/> Diski değiştir işleminden sonra, özgün disk kaynak grubunda tutulur. Gerekli değilse orijinal diskleri el ile silmeyi seçebilirsiniz. <br/><br/>Varolan değiştir şifresiz yönetilen VM'ler için desteklenir. Yönetilmeyen diskler, [genelleştirilmiş VM'ler](https://docs.microsoft.com/azure/virtual-machines/windows/capture-image-resource)veya özel görüntüler [kullanılarak oluşturulan](https://azure.microsoft.com/resources/videos/create-a-custom-virtual-machine-image-in-azure-resource-manager-with-powershell/)VM'ler için desteklenmez.<br/><br/> Geri yükleme noktası nın geçerli VM'den daha fazla veya daha az diski varsa, geri yükleme noktasındaki disk sayısı yalnızca VM yapılandırmasını yansıtır.<br><br> Yedekleme istemci uygulaması geri yükleme gerçekleştirirken bu kaynaklarda izinleri olmadığından, varolan varolan ları bağlantılı kaynaklarla [(kullanıcı tarafından atanan yönetilen kimlik](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) veya Anahtar [Kasa](https://docs.microsoft.com/azure/key-vault/key-vault-overview)gibi) değiştirme desteklenmez.<br/><br/>Klasik VM'ler desteklenmez.
**Çapraz Bölge (ikincil bölge)** | Çapraz Bölge geri yüklemesi, [Azure eşleştirilmiş](https://docs.microsoft.com/azure/best-practices-availability-paired-regions#what-are-paired-regions)bir bölge olan ikincil bölgedeki Azure VM'lerini geri yüklemek için kullanılabilir.<br><br> Yedekleme ikincil bölgede yapılıyorsa, seçili kurtarma noktası için tüm Azure VM'lerini geri yükleyebilirsiniz.<br><br> Bu özellik aşağıdaki seçenekler için kullanılabilir:<br> * [VM oluşturma](https://docs.microsoft.com/azure/backup/backup-azure-arm-restore-vms#create-a-vm) <br> * [Diskleri Geri Yükleme](https://docs.microsoft.com/azure/backup/backup-azure-arm-restore-vms#restore-disks) <br><br> Şu anda [varolan diskleri değiştir](https://docs.microsoft.com/azure/backup/backup-azure-arm-restore-vms#replace-existing-disks) seçeneğini destekliyoruz.<br><br> İzinler<br> İkincil bölgedeki geri yükleme işlemi Yedekleme Yöneticileri ve Uygulama yöneticileri tarafından gerçekleştirilebilir.

> [!NOTE]
> Azure VM'de belirli dosya ve klasörleri de kurtarabilirsiniz. [Daha fazla bilgi edinin](backup-azure-restore-files-from-vm.md).
>
> Azure VM'ler için Azure Yedekleme'nin [en son sürümünü](backup-instant-restore-capability.md) çalıştırıyorsanız (Anında Geri Yükleme olarak bilinir), anlık görüntüler yedi güne kadar saklanır ve yedekleme verileri kasaya gönderilmeden önce anlık görüntülerden bir VM geri yükleyebilirsiniz. Son yedi güne ait bir yedeklemeden bir VM geri yüklemek istiyorsanız, kasadan değil, anlık görüntüden geri yüklemek daha hızlıdır.

## <a name="storage-accounts"></a>Depolama hesapları

Depolama hesapları hakkında bazı ayrıntılar:

- **VM Oluştur**: Yeni bir VM oluşturduğunuzda, VM belirttiğiniz depolama hesabına yerleştirilir.
- **Diski geri yükleme**: Bir diski geri yüklediğinizde, disk belirttiğiniz depolama hesabına kopyalanır. Geri yükleme işi, özel VM ayarlarını belirtmek için karşıdan yükleyebileceğiniz ve kullanabileceğiniz bir şablon oluşturur. Bu şablon belirtilen depolama hesabına yerleştirilir.
- **Diski Değiştir**: Varolan bir VM'de bir diski değiştirdiğinizde, Azure Yedekleme diski değiştirmeden önce varolan VM'nin anlık görüntüsünü alır. Anlık görüntü, belirttiğiniz hazırlama konumunda (depolama hesabı) depolanır. Bu depolama hesabı anlık görüntünün geri yükleme işlemi sırasında geçici olarak depolanması için kullanılır ve bunu yapmak için daha sonra kolayca kaldırılabilecek yeni bir hesap oluşturmanızı öneririz.
- **Depolama hesabı konumu**: Depolama hesabı kasaile aynı bölgede olmalıdır. Yalnızca bu hesaplar görüntülenir. Konumda depolama hesabı yoksa, bir tane oluşturmanız gerekir.
- **Depolama türü**: Blob depolama desteklenmez.
- **Depolama artıklığı**: Bölge yedekli depolama (ZRS) desteklenmez. Hesabın çoğaltma ve artıklık bilgileri, hesap adından sonra parantez içinde gösterilir.
- **Premium depolama**:
  - Premium olmayan VM'leri geri geri alırken, premium depolama hesapları desteklenmez.
  - Yönetilen VM'leri geri geri alırken, ağ kurallarıyla yapılandırılan premium depolama hesapları desteklenmez.

## <a name="before-you-start"></a>Başlamadan önce

Bir VM geri yüklemek için (yeni bir VM oluşturmak), Geri Yükleme VM işlemi için doğru rol tabanlı erişim denetimi (RBAC) [izinlerine](backup-rbac-rs-vault.md#mapping-backup-built-in-roles-to-backup-management-actions) sahip olduğundan emin olun.

İzinlerin yoksa, [bir diski geri](#restore-disks)yükleyebilirsiniz ve disk geri yüklendikten sonra, yeni bir VM oluşturmak için geri yükleme işleminin bir parçası olarak oluşturulan [şablonu kullanabilirsiniz.](#use-templates-to-customize-a-restored-vm)

## <a name="select-a-restore-point"></a>Geri yükleme noktası seçin

1. Geri yüklemek istediğiniz VM ile ilişkili kasada Yedek **öğeler** > **Azure Sanal Makine'yi**tıklatın.
2. VM'yi tıklatın. VM panosunda varsayılan olarak, son 30 güne ait kurtarma noktaları görüntülenir. 30 günden eski kurtarma noktalarını görüntüleyebilir veya tarihlere, zaman aralıklarına ve farklı anlık görüntü tutarlılığı türlerine göre kurtarma noktalarını bulmak için filtre uygulayabilirsiniz.
3. VM'yi geri yüklemek için **VM'yi geri yükle'yi**tıklatın.

    ![Geri yükleme noktası](./media/backup-azure-arm-restore-vms/restore-point.png)

4. Kurtarma için kullanılacak bir geri yükleme noktası seçin.

## <a name="choose-a-vm-restore-configuration"></a>VM geri yükleme yapılandırması seçin

1. **Geri Yükleme yapılandırmasında,** geri yükleme seçeneğini seçin:
    - **Yeni Oluştur**: Yeni bir VM oluşturmak istiyorsanız bu seçeneği kullanın. Basit ayarlarla bir VM oluşturabilir veya diski geri yükleyebilir ve özelleştirilmiş bir VM oluşturabilirsiniz.
    - **Varolanı Değiştir**: Varolan bir VM'deki diskleri değiştirmek istiyorsanız bu seçeneği kullanın.

        ![Yapılandırma sihirbazı geri yükleme](./media/backup-azure-arm-restore-vms/restore-configuration.png)

2. Seçili geri yükleme seçeneğiniz için ayarları belirtin.

## <a name="create-a-vm"></a>VM oluşturma

Geri yükleme [seçeneklerinden](#restore-options)biri olarak, bir geri yükleme noktasından temel ayarları ile hızlı bir şekilde bir VM oluşturabilirsiniz.

1. **Geri Yükleme yapılandırmasında** > **Yeni** > Geri Yükleme Türü oluştur , **sanal makine oluştur'u**seçin.**Restore Type**
2. **Sanal makine adında,** abonelikte bulunmayan bir VM belirtin.
3. **Kaynak grubunda,** yeni VM için varolan bir kaynak grubu seçin veya genel olarak benzersiz bir ada sahip yeni bir kaynak grubu oluşturun. Zaten var olan bir ad atarsanız, Azure gruba VM ile aynı adı atar.
4. **Sanal ağda,** VM'nin yerleştirileceği VNet'i seçin. Abonelikle ilişkili tüm VNet'ler görüntülenir. Alt ağı seçin. İlk alt ağ varsayılan olarak seçilir.
5. **Depolama**Konumu'nda, VM'nin depolama hesabını belirtin. [Daha fazla bilgi edinin](#storage-accounts).

    ![Yapılandırma sihirbazı geri yükleme](./media/backup-azure-arm-restore-vms/recovery-configuration-wizard1.png)

6. **Geri Yükleme yapılandırmasında** **Tamam'ı**seçin. **Geri Yükle'de**geri yükleme işlemini tetiklemek için **Geri Yükle'yi** tıklatın.

## <a name="restore-disks"></a>Diskleri geri yükleme

Geri yükleme [seçeneklerinden](#restore-options)biri olarak, geri yükleme noktasından bir disk oluşturabilirsiniz. Daha sonra disk ile aşağıdakilerden birini yapabilirsiniz:

- Ayarları özelleştirmek ve VM dağıtımını tetiklemek için geri yükleme işlemi sırasında oluşturulan şablonu kullanın. Varsayılan şablon ayarlarını ve VM dağıtımı için şablonu gönderin.
- [Geri yüklenen diskleri](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal) varolan bir VM'ye takın.
- PowerShell'i kullanarak geri yüklenen disklerden [yeni bir VM oluşturun.](https://docs.microsoft.com/azure/backup/backup-azure-vms-automation#create-a-vm-from-restored-disks)

1. **Geri Yükleme yapılandırmasında** > **Yeni** > Geri**Yükleme Türü**Oluştur , **diskleri Geri Yükle'yi**seçin.
2. **Kaynak grubunda,** geri yüklenen diskler için varolan bir kaynak grubu seçin veya genel olarak benzersiz bir ada sahip yeni bir kaynak grubu oluşturun.
3. **Depolama hesabında,** VHD'lerin kopyalanış ettiği hesabı belirtin. [Daha fazla bilgi edinin](#storage-accounts).

    ![Kurtarma yapılandırması tamamlandı](./media/backup-azure-arm-restore-vms/trigger-restore-operation1.png)

4. **Geri Yükleme yapılandırmasında** **Tamam'ı**seçin. **Geri Yükle'de**geri yükleme işlemini tetiklemek için **Geri Yükle'yi** tıklatın.

Sanal makineniz yönetilen diskleri kullandığında ve **sanal makine oluştur** seçeneğini seçtiğinizde, Azure Yedekleme belirtilen depolama hesabını kullanmaz. **Diskleri Geri Yükle** ve **Anında Geri Yükleme**durumunda, depolama hesabı yalnızca şablonu depolamak için kullanılır. Yönetilen diskler belirtilen kaynak grubunda oluşturulur.
Sanal makineniz yönetilmeyen diskler kullandığında, bunlar depolama hesabına blob olarak geri yüklenir.

### <a name="use-templates-to-customize-a-restored-vm"></a>Geri yüklenen vm'yi özelleştirmek için şablonları kullanma

Disk geri yüklendikten sonra, yeni bir VM'yi özelleştirmek ve oluşturmak için geri yükleme işleminin bir parçası olarak oluşturulan şablonu kullanın:

1. İlgili iş için **İş Ayrıntılarını Geri Yükle'yi** açın.

2. **İş Ayrıntılarını Geri Yükle'de**şablon dağıtımını başlatmak için **Şablonu Dağıt'ı** seçin.

    ![İş icabı geri yükleme](./media/backup-azure-arm-restore-vms/restore-job-drill-down1.png)

3. Şablonda sağlanan VM ayarını özelleştirmek için **şablonu edit'i**tıklatın. Daha fazla özelleştirme eklemek istiyorsanız, **parametreleri edit'i**tıklatın.
    - Özel bir şablondan kaynak dağıtma hakkında [daha fazla bilgi edinin.](../azure-resource-manager/templates/deploy-portal.md#deploy-resources-from-custom-template)
    - Şablon yazma hakkında [daha fazla bilgi edinin.](../azure-resource-manager/templates/template-syntax.md)

   ![Yük şablonu dağıtımı](./media/backup-azure-arm-restore-vms/edit-template1.png)

4. VM için özel değerleri girin, **Şartlar ve Koşullar'ı** kabul edin ve **Satın Al'ı**tıklatın.

   ![Şablon dağıtımı gönder](./media/backup-azure-arm-restore-vms/submitting-template1.png)

## <a name="replace-existing-disks"></a>Varolan diskleri değiştirme

Geri yükleme [seçeneklerinden](#restore-options)biri olarak, varolan bir VM diskini seçili geri yükleme noktasıyla değiştirebilirsiniz. Tüm geri yükleme seçeneklerini [gözden geçirin.](#restore-options)

1. **Geri Yükle yapılandırmasında,** **varolan'ı değiştir'i**tıklatın.
2. **Geri Yükle Türü'nde,** **disk/s'leri değiştir'i**seçin. Bu, varolan VM diskleri değiştirin kullanılacak geri yükleme noktasıdır.
3. **Hazırlama**Konumu'nda, geri yükleme işlemi sırasında geçerli yönetilen disklerin anlık görüntülerinin nerede kaydedilmesi gerektiğini belirtin. [Daha fazla bilgi edinin](#storage-accounts).

   ![Yapılandırma sihirbazına geri yükleme Varolan Değiştir](./media/backup-azure-arm-restore-vms/restore-configuration-replace-existing.png)

## <a name="cross-region-restore"></a>Çapraz Bölge Geri Yükleme

Geri yükleme [seçeneklerinden](https://docs.microsoft.com/azure/backup/backup-azure-arm-restore-vms#restore-options)biri olarak, Çapraz Bölge Geri Yükleme (CRR), Azure eşleştirilmiş bir bölge olan ikincil bir bölgede Azure VM'lerini geri yüklemenize olanak tanır.

Önizleme sırasında özelliğin yerleşik olması için lütfen [Başlamadan Önce bölümünü](https://docs.microsoft.com/azure/backup/backup-create-rs-vault#set-cross-region-restore)okuyun.

CRR etkin olup olmadığını görmek için, [Çapraz Bölge Geri Yüklemesini Yapılandır'daki](backup-create-rs-vault.md#configure-cross-region-restore) yönergeleri izleyin

### <a name="view-backup-items-in-secondary-region"></a>Yedekleme öğelerini ikincil bölgede görüntüleme

CRR etkinse, ikincil bölgedeki yedek öğeleri görüntüleyebilirsiniz.

1. Portaldan, Kurtarma **Hizmetleri kasası** > **Yedekleme öğelerine** gidin
2. İkincibölgedeki öğeleri görüntülemek için **İkincil Bölge'yi** tıklatın.

![İkincil bölgedeki sanal makineler](./media/backup-azure-arm-restore-vms/secbackedupitem.png)

![İkincil Bölge Seçin](./media/backup-azure-arm-restore-vms/backupitems-sec.png)

### <a name="restore-in-secondary-region"></a>İkinci bölgede geri yükleme

İkincil bölge geri yükleme kullanıcı deneyimi birincil bölge geri kullanıcı deneyimi benzer olacaktır. Geri yüklemenizi yapılandırmak için Yapılandırmayı Geri Yükle'deki ayrıntıları yapılandırırken, yalnızca ikincil bölge parametrelerini sağlamanız istenir.

![Geri yüklemek için VM'yi seçin](./media/backup-azure-arm-restore-vms/sec-restore.png)

![Geri yükleme noktasını seçin](./media/backup-azure-arm-restore-vms/sec-rp.png)

![Yapılandırmayı geri yükleme](./media/backup-azure-arm-restore-vms/rest-config.png)

![Devam bildiriminde tetikleyici geri yükleme](./media/backup-azure-arm-restore-vms/restorenotifications.png)

- VM'yi geri yüklemek ve oluşturmak için [VM oluştur'a](https://docs.microsoft.com/azure/backup/backup-azure-arm-restore-vms#create-a-vm)bakın.
- Disk olarak geri yüklemek için [diskleri geri yükleme'ye](https://docs.microsoft.com/azure/backup/backup-azure-arm-restore-vms#restore-disks)bakın.

>[!NOTE]
>Geri yükleme tetiklendikten ve veri aktarım aşamasında niçin geri yükleme işi iptal edilemez.

### <a name="monitoring-secondary-region-restore-jobs"></a>İkincil bölgenin geri yüklenmesi işleri izleme

1. Portaldan, Kurtarma **Hizmetleri vault** > **Backup Jobs** gidin
2. İkincibölgedeki öğeleri görüntülemek için **İkincil Bölge'yi** tıklatın.

![Yedek işler filtreuygulanmış](./media/backup-azure-arm-restore-vms/secbackupjobs.png)

## <a name="restore-vms-with-special-configurations"></a>Özel yapılandırmalarla VM'leri geri yükleme

VM'leri geri yüklemeniz gerekebilecek yaygın senaryolar vardır.

**Senaryo** | **Rehber**
--- | ---
**Hibrit Kullanım Avantajı nı kullanarak VM'leri geri yükleme** | Bir Windows VM [Hibrit Kullanım Avantajı (HUB) lisanslama](../virtual-machines/windows/hybrid-use-benefit-licensing.md)kullanıyorsa, diskleri geri yükleyin ve sağlanan şablonu **(Windows_Server**için **Lisans Türü** olarak ayarlanmış) veya PowerShell'i kullanarak yeni bir VM oluşturun.  Bu ayar, VM oluşturulduktan sonra da uygulanabilir.
**Azure veri merkezi felaketi sırasında VM'leri geri yükleme** | Kasa GRS kullanırsa ve VM'nin birincil veri merkezi çökerse, Azure Yedekleme yedeklenmiş VM'leri eşleştirilmiş veri merkezine geri yükler. Eşleştirilmiş veri merkezinde bir depolama hesabı seçin ve normal olarak geri yükleyin. Azure Yedekleme, geri yüklenen VM'yi oluşturmak için eşleştirilmiş bölgedeki bilgi işlem hizmetini kullanır. Veri merkezi esnekliği hakkında [daha fazla bilgi edinin.](../resiliency/resiliency-technical-guidance-recovery-loss-azure-region.md)
**Tek etki alanı denetleyicisi VM'yi tek etki alanında geri yükleme** | Diğer VM'ler gibi VM'yi geri yükleyin. Şunlara dikkat edin:<br/><br/> Etkin Dizin açısından bakıldığında, Azure VM diğer VM'ler gibidir.<br/><br/> Dizin Hizmetleri Geri Yükleme Modu (DSRM) de kullanılabilir, bu nedenle tüm Active Directory kurtarma senaryoları uygulanabilir. Sanallaştırılmış etki alanı denetleyicileri için yedekleme ve dikkatleri geri yükleme hakkında [daha fazla bilgi edinin.](https://docs.microsoft.com/azure/backup/backup-azure-arm-restore-vms#post-restore-steps)
**Tek etki alanında birden çok etki alanı denetleyiciVM'i geri yükleme** | Aynı etki alanında bulunan diğer etki alanı denetleyicilerine ağ üzerinden ulaşılabiliyorsa, etki alanı denetleyicisi herhangi bir VM gibi geri yüklenebilir. Etki alanında kalan son etki alanı denetleyicisiyse veya yalıtılmış bir ağda kurtarma gerçekleştirilirse, [bir orman kurtarma](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/ad-forest-recovery-single-domain-in-multidomain-recovery)kullanın.
**Bir ormanda birden çok etki alanını geri yükleme** | Biz bir [orman kurtarma](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/ad-forest-recovery-single-domain-in-multidomain-recovery)öneririz.
**Çıplak metal geri yükleme** | Azure VM'leri ile şirket içi hipervizörler arasındaki en büyük fark, Azure'da VM konsolu olmamasıdır. Bir konsol, çıplak metal kurtarma (BMR) türü yedekleme kullanarak kurtarma gibi belirli senaryolar için gereklidir. Ancak, kasadan VM geri yükleme BMR için tam bir yedektir.
**Özel ağ yapılandırmaları ile VM'leri geri yükleme** | Özel ağ yapılandırmaları, dahili veya harici yük dengelemesi kullanan VM'leri, birden çok NICS veya birden çok ayrılmış IP adresi kullanarak içerir. Diski [geri yükleme seçeneğini](#restore-disks)kullanarak bu VM'leri geri yükleyin. Bu seçenek, VHD'lerin bir kopyasını belirtilen depolama hesabına dönüştürür ve yapılandırmanıza uygun olarak [dahili](https://azure.microsoft.com/documentation/articles/load-balancer-internal-getstarted/) veya [harici](/azure/load-balancer/quickstart-create-standard-load-balancer-powershell) yük dengeleyicisi, [birden çok NICS](../virtual-machines/windows/multiple-nics.md)veya birden çok ayrılmış IP [adresi](../virtual-network/virtual-network-multiple-ip-addresses-powershell.md)içeren bir VM oluşturabilirsiniz.
**NIC/Subnet'te Ağ Güvenlik Grubu (NSG)** | Azure VM yedekleme, Vnet, subnet ve NIC düzeyinde NSG bilgilerinin Yedeklenmesini ve Geri Yükyükülmesini destekler.
**Bölge Sabitlenmiş VM'ler** | Azure Yedekleme, zonlu sabitlenmiş VM'lerin yedeklenmesini ve geri yüklenmesini destekler. [Daha fazlasını öğrenin](https://azure.microsoft.com/global-infrastructure/availability-zones/)

## <a name="track-the-restore-operation"></a>Geri yükleme işlemini izleme

Geri yükleme işlemini tetikledikten sonra, yedekleme hizmeti izleme için bir iş oluşturur. Azure Yedekleme, portaldaki işle ilgili bildirimler görüntüler. Görünür değillerse, **Bildirimler** simgesini seçin ve ardından İşlem idurumlarını görmek için **Tüm İşleri Görüntüle'yi** seçin.

![Tetikleyiciyi geri yükleme](./media/backup-azure-arm-restore-vms/restore-notification1.png)

 Geri yüklemeyi aşağıdaki gibi izleyin:

1. İş işlemlerini görüntülemek için bildirimler köprübağlantısını tıklatın. Alternatif olarak, kasada Yedek **işleri**tıklatın ve ardından ilgili VM'yi tıklatın.

    ![Kasadaki VM listesi](./media/backup-azure-arm-restore-vms/restore-job-in-progress1.png)

2. Geri yükleme ilerlemesini izlemek için, devam **etme**durumu olan herhangi bir geri yükleme işini tıklatın. Bu, geri yükleme ilerleme hakkında bilgi görüntüleyen ilerleme çubuğunu görüntüler:

    - **Tahmini geri yükleme süresi**: Başlangıçta geri yükleme işlemini tamamlamak için gereken süreyi sağlar. İşlem ilerledikçe, geri yükleme işlemi bittiğinde alınan süre azalır ve sıfıra ulaşır.
    - **Geri yükleme yüzdesi.** Yapılan geri yükleme işleminin yüzdesini gösterir.
    - **Aktarılan bayt sayısı**: Yeni bir VM oluşturarak geri alıyorsanız, aktarılacak toplam bayt sayısına göre aktarılan baytları gösterir.

## <a name="post-restore-steps"></a>Geri yükleme adımları

VM'yi geri aldıktan sonra dikkat edilmesi gereken birkaç şey vardır:

- Yedekleme yapılandırması sırasında bulunan uzantılar yüklenir, ancak etkinleştirilir. Bir sorun görürseniz uzantıları yeniden yükleyin.
- Yedeklenmiş VM'nin statik bir IP adresi varsa, geri yüklenen VM'nin çakışmasını önlemek için dinamik bir IP adresi olacaktır. Geri [yüklenen VM'ye statik bir IP adresi ekleyebilirsiniz.](https://docs.microsoft.com/powershell/module/az.network/set-aznetworkinterfaceipconfig?view=azps-3.5.0#description)
- Geri yüklenen VM'nin kullanılabilirlik kümesi yoktur. Geri yükleme diski seçeneğini kullanıyorsanız, sağlanan şablonu veya PowerShell'i kullanarak diskten bir VM oluştururken [bir kullanılabilirlik kümesi belirtebilirsiniz.](../virtual-machines/windows/tutorial-availability-sets.md)
- Güvenlik nedeniyle ubuntu gibi bulut girişi tabanlı bir Linux dağıtımı kullanıyorsanız, parola geri yüklemeden sonra engellenir. [Parolayı sıfırlamak](../virtual-machines/linux/reset-password.md)için geri yüklenen VM'deki VMAccess uzantısını kullanın. Bu dağıtımlarda SSH anahtarlarını kullanmanızı öneririz, böylece geri yüklemeden sonra parolayı sıfırlamanız gerekmez.
- VM etki alanı denetleyicisi ile ilişkisi bozuldu nedeniyle bir kez geri yüklendikten sonra VM'ye erişemiyorsanız, VM'yi getirmek için aşağıdaki adımları izleyin:
  - Kurtarılan bir VM'ye veri diski olarak OS diskini takın.
  - Azure Aracısı'nın bu [bağlantıyı](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/install-vm-agent-offline)izleyerek yanıt vermediğini tespit ederse VM aracısını el ile yükleyin.
  - Komut satırının VM'ye erişmesine izin vermek için VM'de Seri Konsol erişimini etkinleştirme

  ```cmd
    bcdedit /store <drive letter>:\boot\bcd /enum
    bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /set {bootmgr} displaybootmenu yes
    bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /set {bootmgr} timeout 5
    bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /set {bootmgr} bootems yes
    bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /ems {<<BOOT LOADER IDENTIFIER>>} ON
    bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200
    ```

  - VM yeniden oluşturulduğunda, yerel yönetici hesabını ve parolasını sıfırlamak için Azure portalını kullanın
  - VM'yi etki alanından çıkarmak için Seri konsol erişimini ve CMD'yi kullanma

    ```cmd
    cmd /c "netdom remove <<MachineName>> /domain:<<DomainName>> /userD:<<DomainAdminhere>> /passwordD:<<PasswordHere>> /reboot:10 /Force"
    ```

- VM'ye katıldıktan ve yeniden başlatıldıktan sonra, yerel yönetici kimlik bilgileriyle VM'ye başarılı bir şekilde RDP'yi katabilecek ve VM'yi etki alanına başarıyla geri döndürebileceksiniz.

## <a name="backing-up-restored-vms"></a>Geri yüklenen VM'leri yedekleme

- İlk olarak yedeklenen VM ile aynı ada sahip aynı kaynak grubuna bir VM geri yüklediyseniz, yedekleme geri yüklemeden sonra VM'de devam edin.
- VM'yi farklı bir kaynak grubuna geri yüklediyseniz veya geri yüklenen VM için farklı bir ad belirttiyseniz, geri yüklenen VM için yedekleme ayarlamanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- Geri yükleme işlemi sırasında güçlüklerle karşılaşırsanız, sık karşılaşılan sorunları ve hataları [gözden geçirin.](backup-azure-vms-troubleshoot.md#restore)
- VM geri yüklendikten [sonra, sanal makineleri yönetme](backup-azure-manage-vms.md) hakkında bilgi edinin
