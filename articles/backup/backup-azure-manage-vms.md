---
title: Azure VM yedeklemelerini yönetme ve izleme
description: Azure Yedekleme hizmetini kullanarak Azure VM yedeklemelerini nasıl yönetip izleyeceğinizi öğrenin.
ms.reviewer: sogup
ms.topic: conceptual
ms.date: 09/18/2019
ms.openlocfilehash: dd4e9dc199048b3faf3da0cadfdf60bdcb26c5bc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79248157"
---
# <a name="manage-azure-vm-backups-with-azure-backup-service"></a>Azure Yedekleme hizmetiyle Azure VM yedeklemelerini yönetme

Bu makalede, [Azure Yedekleme hizmetini](backup-overview.md)kullanarak yedeklenen Azure sanal makinelerinin (VM'ler) nasıl yönetilenbir şekilde yönetilen. Makale ayrıca kasa panosunda bulabileceğiniz yedekleme bilgilerini de özetler.

Azure portalında, Kurtarma Hizmetleri kasa panosu aşağıdakiler dahil olmak üzere kasa bilgilerine erişim sağlar:

* En son yedekleme, aynı zamanda en son geri yükleme noktasıdır.
* Yedekleme ilkesi.
* Tüm yedekleme anlık görüntülerinin toplam boyutu.
* Yedeklemeler için etkinleştirilen VM sayısı.

Paneli kullanarak ve tek tek VM'lere delerek yedeklemeleri yönetebilirsiniz. Makine yedeklemelerine başlamak için panodaki kasayı açın.

![Kaydırıcı ile tam pano görünümü](./media/backup-azure-manage-vms/bottom-slider.png)

## <a name="view-vms-on-the-dashboard"></a>Panoda VM'leri görüntüleme

Kasa panosundaki VM'leri görüntülemek için:

1. [Azure portalında](https://portal.azure.com/)oturum açın.
2. Hub menüsünde **Gözat'ı**seçin. Kaynak listesinde **Kurtarma Hizmetleri** yazın. Siz yazarken, liste girişinize göre filtrelenir. **Kurtarma Hizmetleri kasalarını**seçin.

    ![Kurtarma Hizmetleri kasası oluşturma](./media/backup-azure-manage-vms/browse-to-rs-vaults.png)

3. Kullanım kolaylığı için kasaya sağ tıklayın ve **panoya Sabitle'yi**seçin.
4. Kasa panosunu aç.

    ![Kasa panosunu ve Ayarlar bölmesini açma](./media/backup-azure-manage-vms/full-view-rs-vault.png)

5. Yedek **Öğeler** döşemesinde **Azure Sanal Makineleri'ni**seçin.

    ![Yedek Öğeler döşemesini açma](./media/backup-azure-manage-vms/contoso-vault-1606.png)

6. Yedek **Öğeler** bölmesinde, korumalı VM listesini görüntüleyebilirsiniz. Bu örnekte, kasa bir sanal makineyi korur: demobackup.  

    ![Yedekleme Öğeleri bölmesini görüntüleme](./media/backup-azure-manage-vms/backup-items-blade-select-item.png)

7. Kasa öğesinin panosundan yedekleme ilkelerini değiştirin, isteğe bağlı yedekleme çalıştırın, VM'lerin korunmasını durdurun veya devam ettirin, yedekleme verilerini silin, geri yükleme noktalarını görüntüleyin ve geri yükleme çalıştırın.

    ![Yedekleme Öğeleri panosu ve Ayarlar bölmesi](./media/backup-azure-manage-vms/item-dashboard-settings.png)

## <a name="manage-backup-policy-for-a-vm"></a>VM için yedekleme ilkesini yönetme

Bir yedekleme ilkesini yönetmek için:

1. [Azure portalında](https://portal.azure.com/)oturum açın. Kasa panosunu aç.
2. Yedek **Öğeler** döşemesinde **Azure Sanal Makineleri'ni**seçin.

    ![Yedek Öğeler döşemesini açma](./media/backup-azure-manage-vms/contoso-vault-1606.png)

3. Yedekleme **Öğeleri** bölmesinde, korumalı VM'lerin listesini ve son yedekleme durumunu en son geri yükleme noktaları süresiyle görüntüleyebilirsiniz.

    ![Yedekleme Öğeleri bölmesini görüntüleme](./media/backup-azure-manage-vms/backup-items-blade-select-item.png)

4. Kasa öğesinin panosundan bir yedekleme ilkesi seçebilirsiniz.

   * İlkeleri değiştirmek için farklı bir ilke seçin ve sonra **Kaydet'i**seçin. Yeni ilke hemen kasaya uygulanır.

     ![Yedekleme ilkesi seçme](./media/backup-azure-manage-vms/backup-policy-create-new.png)

## <a name="run-an-on-demand-backup"></a>İsteğe bağlı yedekleme çalıştırma

Korumasını ayarladıktan sonra vm'nin isteğe bağlı yedeklemesini çalıştırabilirsiniz. Bu ayrıntıları aklınızda bulundurun:

* İlk yedekleme beklemedeyse, isteğe bağlı yedekleme Kurtarma Hizmetleri kasasındaV'ın tam bir kopyasını oluşturur.
* İlk yedekleme tamamlanırsa, isteğe bağlı yedekleme yalnızca önceki anlık görüntüdeki değişiklikleri Kurtarma Hizmetleri kasasına gönderir. Diğer bir deyişle, daha sonra yedekler her zaman artımlı.
* İsteğe bağlı yedeklemeiçin bekletme aralığı, yedeklemeyi tetiklediğınızda belirttiğiniz bekletme değeridir.

İsteğe bağlı yedeklemeyi tetiklemek için:

1. **Korumalı Öğe'nin**altındaki [kasa öğesi panosunda](#view-vms-on-the-dashboard) **Yedek Öğe'yi**seçin.

    ![Yedekleme şimdi seçeneği](./media/backup-azure-manage-vms/backup-now-button.png)

2. **Yedekleme Yönetim Türü'nden**Azure **Sanal Makinesi'ni**seçin. **Yedekleme Öğesi (Azure Sanal Makine)** bölmesi görüntülenir.
3. İsteğe bağlı yedekleme oluşturmak için bir VM ve **Yedek Şimdi'yi** seçin. **Şimdi Yedek** bölmesi görüntülenir.
4. Yedekleme **Kasası** alanında, yedeklemenin tutulması için bir tarih belirtin.

    ![Yedekleme Şimdi takvimi](./media/backup-azure-manage-vms/backup-now-check.png)

5. Yedekleme işini çalıştırmak için **Tamam'ı** seçin.

Kasa panosunda işin ilerlemesini izlemek için Yedek **İşler** döşemesini seçin.

## <a name="stop-protecting-a-vm"></a>VM'yi korumayı durdurma

VM'yi korumayı durdurmanın iki yolu vardır:

* **Korumayı durdurun ve yedekleme verilerini koruyun.** Bu seçenek, gelecekteki tüm yedekleme işlerinin VM'nizi korumasını durdurur; ancak, Azure Yedekleme hizmeti yedeklenen kurtarma noktalarını korur.  Kurtarma noktalarını kasada tutmak için ödeme yapmanız gerekir (ayrıntılar için [Azure Yedekleme fiyatlandırmasına](https://azure.microsoft.com/pricing/details/backup/) bakın). Gerekirse VM'yi geri yükleyebilirsiniz. VM korumasını sürdürmeye karar verirseniz, *Devam yedekleme* seçeneğini kullanabilirsiniz.
* **Korumayı durdurun ve yedekleme verilerini silin.** Bu seçenek, gelecekteki tüm yedekleme işlerinin VM'nizi korumasını durdurur ve tüm kurtarma noktalarını siler. VM'yi geri yükleyemezsinve Devam *yedekleme* seçeneğini kullanamazsınız.

>[!NOTE]
>Yedeklemeleri durdurmadan bir veri kaynağını silerseniz, yeni yedeklemeler başarısız olur. Eski kurtarma noktaları ilke göre sona erer, ancak yedeklemeleri durdurup verileri silene kadar son bir kurtarma noktası her zaman tutulur.
>

### <a name="stop-protection-and-retain-backup-data"></a>Korumayı durdurun ve yedekleme verilerini koruyun

Korumayı durdurmak ve VM verilerini saklamak için:

1. Kasa [öğesinin panosunda](#view-vms-on-the-dashboard)yedeklemeyi **durdur'u**seçin.
2. **Yedekleme Verilerini Koru'u**seçin ve seçiminizi gerektiği gibi onaylayın. İstersenize yorum ekleyin. Öğenin adından emin değilseniz, adı görüntülemek için ünlem işaretinin üzerine titreyin.

    ![Yedekleme verilerini koruyun](./media/backup-azure-manage-vms/retain-backup-data.png)

Bildirim, yedekleme işlerinin durdurulduğunu bilmenizi sağlar.

### <a name="stop-protection-and-delete-backup-data"></a>Korumayı durdurun ve yedekleme verilerini silin

VM'nin korumasını durdurmak ve verileri silmek için:

1. Kasa [öğesinin panosunda](#view-vms-on-the-dashboard)yedeklemeyi **durdur'u**seçin.
2. **Yedekleme Verilerini Sil'i**seçin ve seçiminizi gerektiği gibi onaylayın. Yedekleme öğesinin adını girin ve isterseniz yorum ekleyin.

    ![Yedekleme verilerini silme](./media/backup-azure-manage-vms/delete-backup-data1.png)

## <a name="resume-protection-of-a-vm"></a>VM'nin özgeçmiş koruması

[Durdurma korumasını](#stop-protection-and-retain-backup-data) seçtiyseniz ve VM korumasını durdurma sırasında yedekleme verisi seçeneğini koruduysanız, **Devam yedekleme'yi**kullanabilirsiniz. [Korumayı Durdur'u seçip yedekleme verilerini silme](#stop-protection-and-delete-backup-data) veya [yedekleme verilerini silme](#delete-backup-data)seçeneğini seçerseniz bu seçenek kullanılamaz.

VM için korumayı sürdürmek için:

1. Kasa [öğesinin panosunda,](#view-vms-on-the-dashboard) **Yedeklemeye Devam'ı**seçin.

2. VM için ilke atamak için [yedekleme ilkelerini yönet'teki](#manage-backup-policy-for-a-vm) adımları izleyin. VM'nin ilk koruma ilkesini seçmeniz gerekmez.
3. Yedekleme ilkesini VM'ye uyguladıktan sonra aşağıdaki iletiyi görürsünüz:

    ![Başarıyla korunan vm'yi gösteren ileti](./media/backup-azure-manage-vms/success-message.png)

## <a name="delete-backup-data"></a>Yedekleme verilerini silme

Bir VM'nin yedekleme verilerini silmenin iki yolu vardır:

* Kasa öğesi panosundan yedeklemeyi durdur'u seçin ve [korumayı durdur ve yedekleme verilerini sil](#stop-protection-and-delete-backup-data) seçeneğini yönergeleri izleyin.

  ![Yedeklemeyi Durdur'u seçin](./media/backup-azure-manage-vms/stop-backup-buttom.png)

* Kasa öğesi panosundan yedekleme verilerini sil'i'ni seçin. VM korumasını durdurma sırasında [korumayı durdurmayı ve yedekleme veri](#stop-protection-and-retain-backup-data) seçeneğini korumayı seçtiyseniz bu seçenek etkinleştirilir

  ![Yedeklemeyi Sil'i seçin](./media/backup-azure-manage-vms/delete-backup-buttom.png)

  * Kasa [öğesi panosunda,](#view-vms-on-the-dashboard) **yedekleme verilerini sil'i**seçin.
  * Kurtarma noktalarını silmek istediğinizi doğrulamak için yedekleme öğesinin adını yazın.

    ![Yedekleme verilerini silme](./media/backup-azure-manage-vms/delete-backup-data1.png)

  * Öğenin yedekleme verilerini silmek için **Sil'i**seçin. Bildirim iletisi, yedekleme verilerinin silindiğini bilmenizi sağlar.

Verilerinizi korumak için Azure Yedekleme yumuşak silme özelliğini içerir. Yumuşak silme ile, bir VM'nin yedeklemesi (tüm kurtarma noktaları) silindikten sonra bile, yedekleme verileri 14 gün daha saklanır. Daha fazla bilgi için [yumuşak silme belgelerine](https://docs.microsoft.com/azure/backup/backup-azure-security-feature-cloud)bakın.

  > [!NOTE]
  > Yedekleme verilerini sildiğinizde, ilişkili tüm kurtarma noktalarını silersiniz. Silmek için belirli kurtarma noktalarını seçemezsiniz.

### <a name="backup-item-where-primary-data-source-no-longer-exists"></a>Birincil veri kaynağının artık var olmadığı yedekleme öğesi

* Azure yedeklemesi için yapılandırılan Azure VM'leri korumayı durdurmadan silinir veya taşınırsa, hem zamanlanmış yedekleme işleri hem de isteğe bağlı (geçici) yedekleme işleri UserErrorVmNotFoundV2 hatasıyla başarısız olur. Yedekleme ön denetimi yalnızca başarısız isteğe bağlı yedekleme işleri için kritik olarak görünür (başarısız zamanlanmış işler görüntülenmez).
* Bu yedekleme öğeleri, kullanıcı tarafından belirlenen yedekleme ve bekletme ilkesine bağlı kalarak sistemde etkin kalır. Bu Azure VM'leri için yedeklenen veriler bekletme ilkesine göre korunur. Süresi dolan kurtarma noktaları (son kurtarma noktası hariç) yedekleme ilkesinde ayarlanan bekletme aralığına göre temizlenir.
* Kullanıcılara, son kurtarma noktası sonsuza kadar tutulduğu ndan ve kullanıcıya geçerli yedekleme fiyatlandırmasına göre ücretlendirildiği için, silme kaynaklarıiçin yedek öğe/veri artık gerekli değilse, ek bir maliyetten kaçınmak için birincil veri kaynağının artık bulunmadığı yedek öğeleri silmeleri önerilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure VM'leri VM ayarlarından nasıl yedekleyeceğimiz öğrenin.](backup-azure-vms-first-look-arm.md)
* [VM'leri](backup-azure-arm-restore-vms.md)nasıl geri yükleyin i öğrenin.
* [Azure VM yedeklemelerini](backup-azure-monitor-vms.md)nasıl izleyeceğinizi öğrenin.
