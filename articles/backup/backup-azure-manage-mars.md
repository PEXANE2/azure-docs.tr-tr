---
title: MARS Ajan yedeklemelerini yönetme ve izleme
description: Azure Yedekleme hizmetini kullanarak Microsoft Azure Kurtarma Hizmetleri (MARS) Aracısı yedeklemelerini nasıl yönetip izleyeceğinizi öğrenin.
ms.reviewer: srinathv
ms.topic: conceptual
ms.date: 10/07/2019
ms.openlocfilehash: c11d73edd32c197aac2cec58eeb1cc20e5c6a339
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78673259"
---
# <a name="manage-microsoft-azure-recovery-services-mars-agent-backups-by-using-the-azure-backup-service"></a>Azure Yedekleme hizmetini kullanarak Microsoft Azure Kurtarma Hizmetleri (MARS) Aracı sıyrık larını yönetme

Bu makalede, Microsoft Azure Kurtarma Hizmetleri Aracısı ile yedeklenen dosya ve klasörlerin nasıl yönetilenbir şekilde yönetilen.

## <a name="modify-a-backup-policy"></a>Yedekleme ilkesini değiştirme

Yedekleme ilkesini değiştirdiğinizde, yeni öğeler ekleyebilir, varolan öğeleri yedeklemeden kaldırabilir veya Dosyaların Dışlama Ayarları'nı kullanarak yedeklenmelerini engelleyebilirsiniz.

- **Öğeleri Ekle** bu seçeneği yalnızca yedeklemek için yeni öğeler eklemek için kullanın. Varolan öğeleri kaldırmak için **Öğeleri Kaldır** veya Hariç **Tutma Ayarları** seçeneğini kullanın.  
- **Öğeleri Kaldır,** öğeleri yedeklenmekten kaldırmak için bu seçeneği kullanın.
  - **Öğeleri Kaldır**yerine birimdeki tüm öğeleri kaldırmak için **Dışlama Ayarlarını** kullanın.
  - Bir birimdeki tüm seçimleri temizlemek, değişiklik kapsamı olmaksızın, son yedekleme sırasında bekletme ayarlarına göre tutulan maddelerin eski yedeklemelerinin yapılmasına neden olur.
  - Bu öğelerin yeniden seçilmesi, ilk tam yedeklemeye yol açar ve eski yedeklemelere yeni ilke değişiklikleri uygulanmaz.
  - Birimin tamamının seçilmesi, bekletme ilkesini değiştirmek için herhangi bir kapsam olmadan geçmiş yedeklemeyi korur.
- **Dışlama Ayarları,** belirli öğelerin yedeklenmesini engellemek için bu seçeneği kullanır.

### <a name="add-new-items-to-existing-policy"></a>Varolan ilkeye yeni öğeler ekleme

1. **Eylemler'de,** Yedekleme **Yi**tıklatın.

    ![Windows Server yedeklemesini zamanlama](./media/backup-configure-vault/schedule-first-backup.png)

2. **İlke öğesi sekmesini seçin** ve **dosya ve klasörleriniz için yedekleme zamanlamasını değiştir'i** seçin ve **İleri'yi**tıklatın.

    ![İlke Öğeleri ni seçin](./media/backup-azure-manage-mars/select-policy-items.png)

3. **Zamanlama yedekleme sekmesini değiştir veya durdurun'da,** **yedekleme öğelerinde veya saatlerinde değişiklik yap'ı** seçin ve **İleri'yi**tıklatın.

    ![Yedeklemeyi değiştirme veya zamanlama](./media/backup-azure-manage-mars/modify-schedule-backup.png)

4. **Yedekleme sekmesine öğeleri seç'te,** yedeklemek istediğiniz öğeleri eklemek için **öğeleri ekle'yi** tıklatın.

    ![Yedekleme ekleme öğelerini değiştirme veya zamanlama](./media/backup-azure-manage-mars/modify-schedule-backup-add-items.png)

5. **Öğeleri Seç** penceresinde, eklemek istediğiniz sinekleri veya klasörleri seçin ve **Tamam'ı**tıklatın.

    ![Öğeleri seçin](./media/backup-azure-manage-mars/select-item.png)

6. Sonraki adımları tamamlayın ve işlemi tamamlamak için **Bitir'i** tıklatın.

### <a name="add-exclusion-rules-to-existing-policy"></a>Varolan ilke için Dışlama kuralları ekleme

Yedeklenmesini istemediğiniz dosya ve klasörleri atlamak için dışlama kuralları ekleyebilirsiniz. Bunu, yeni bir ilke tanımlarken veya varolan bir ilkeyi değiştirirken yapabilirsiniz.

1. Eylemler bölmesinden Yedekleme **Yi**tıklatın. Öğeleri **Yedekleme'ye seç'e** gidin ve **Dışlama Ayarları'nı**tıklatın.

    ![Öğeleri seçin](./media/backup-azure-manage-mars/select-exclusion-settings.png)

2. **Dışlama**Ayarları'nda, **Dışlama Ekle'yi**tıklatın.

    ![Öğeleri seçin](./media/backup-azure-manage-mars/add-exclusion.png)

3. **Öğeleri Seç'ten Dışlaya,** dosyalara ve klasörlere göz atın ve hariç tutmak istediğiniz öğeleri seçin ve **Tamam'ı**tıklatın.

    ![Öğeleri seçin](./media/backup-azure-manage-mars/select-items-exclude.png)

4. Varsayılan olarak, seçili klasörlerdeki tüm **Alt klasörler** hariç tutulur. **Evet** veya **Hayır'ı**seçerek bunu değiştirebilirsiniz. Aşağıda gösterildiği gibi hariç tutmak için dosya türlerini disep belirtebilirsiniz:

    ![Öğeleri seçin](./media/backup-azure-manage-mars/subfolders-type.png)

5. Sonraki adımları tamamlayın ve işlemi tamamlamak için **Bitir'i** tıklatın.

### <a name="remove-items-from-existing-policy"></a>Öğeleri varolan ilkeden kaldırma

1. Eylemler bölmesinden Yedekleme **Yi**tıklatın. Yedekleme **öğelerini seç'e**gidin. Listeden, yedekleme zamanlamasından kaldırmak istediğiniz dosya ve klasörleri seçin ve **öğeleri kaldır'ı**tıklatın.

    ![Öğeleri seçin](./media/backup-azure-manage-mars/select-items-remove.png)

> [!NOTE]
> Bir birimi ilkeden tamamen kaldırdığınızda dikkatli olun.  Yeniden eklemeniz gerekirse, yeni bir birim olarak kabul edilir. Sonraki zamanlanmış yedekleme, Artımlı Yedekleme yerine Bir Başlangıç Yedekleme (tam yedekleme) gerçekleştirecektir. Daha sonra öğeleri geçici olarak kaldırmanız ve eklemeniz gerekiyorsa, tam yedekleme yerine artımlı yedekleme sağlamak için **Öğeleri Kaldır** yerine **Dışlama Ayarları'nı** kullanmanız önerilir.

2. Sonraki adımları tamamlayın ve işlemi tamamlamak için **Bitir'i** tıklatın.

## <a name="stop-protecting-files-and-folder-backup"></a>Dosya ve Klasör yedeklemeyi korumayı durdurma

Dosya ve Klasörler yedeklemesini korumayı durdurmanın iki yolu vardır:

- **Korumayı durdurun ve yedekleme verilerini koruyun.**
  - Bu seçenek, gelecekteki tüm yedekleme işlerinin korunmasını durdurur.
  - Azure Yedekleme hizmeti, bekletme ilkesini temel alan yedeklenen kurtarma noktalarını korur.
  - Süresi dolmamış kurtarma noktaları için yedeklenen verileri geri yükleyebilirsiniz.
  - Korumayı sürdürmeye karar verirseniz, *yedek zamanlamayı yeniden etkinleştir* seçeneğini kullanabilirsiniz. Bundan sonra, veriler yeni bekletme ilkesine göre korunur.
- **Korumayı durdurun ve yedekleme verilerini silin.**
  - Bu seçenek, gelecekteki tüm yedekleme işlerinin verilerinizi korumasını durdurur ve tüm kurtarma noktalarını siler.
  - Bir mesaj la birlikte bir silme Yedekleme veri uyarısı e-postası alırsınız *Bu Yedekleme öğesine ait verileriniz silinmiştir. Bu veriler 14 gün süreyle geçici olarak kullanılabilir hale gelecektir ve ardından kalıcı olarak silinir* ve *verilerinizi kurtarmak için Yedekleme öğesini 14 gün içinde yeniden koruma* önerisi önerilir.
  - Korumayı sürdürmek için, silme işleminden sonraki 14 gün içinde yeniden koruyun.

### <a name="stop-protection-and-retain-backup-data"></a>Korumayı durdurun ve yedekleme verilerini koruyun

1. MARS yönetim konsolunu açın, **Eylemler bölmesine**gidin ve **Yedekleme Yi seç' i seçin.**

    ![Zamanlanmış bir yedeklemeyi değiştirin veya durdurun.](./media/backup-azure-manage-mars/mars-actions.png)
1. **İlke Öğesi Seç** sayfasında, **dosya ve klasörleriniz için yedek zamanlama değiştir'i** seçin ve **İleri'yi**tıklatın.

    ![Zamanlanmış bir yedeklemeyi değiştirin veya durdurun.](./media/backup-azure-manage-mars/select-policy-item-retain-data.png)
1. **Zamanlanmış Yedekleme** sayfasını Değiştir veya Durdur'dan, **bu yedekleme zamanlamasını kullanmayı durdur'u seçin, ancak bir zamanlama yeniden etkinleştirilene kadar depolanan yedekleri saklayın.** Ardından **İleri'yi**seçin.

    ![Zamanlanmış bir yedeklemeyi değiştirin veya durdurun.](./media/backup-azure-manage-mars/stop-schedule-backup.png)
1. **Zamanlatma Zamanlanan Yedekleme'de** bilgileri gözden geçirin ve **Bitir'i**tıklatın.

    ![Zamanlanmış bir yedeklemeyi değiştirin veya durdurun.](./media/backup-azure-manage-mars/pause-schedule-backup.png)
1. **Yedekleme işlemini değiştir'de** zamanlama yedekleme duraklamanızın başarı durumunu kontrol edin ve bitirmek için **yakına** tıklayın.

### <a name="stop-protection-and-delete-backup-data"></a>Korumayı durdurun ve yedekleme verilerini silin

1. MARS yönetim konsolunu açın, **Eylemler** bölmesine gidin ve **YedeklemeYi Zamanla'yı**seçin.
2. Zamanlanmış Yedekleme sayfasını **Değiştir veya Durdur'dan,** **bu yedekleme zamanlamasını kullanmayı durdur'u seçin ve depolanan tüm yedeklemeleri silin.** Ardından **İleri'yi**seçin.

    ![Zamanlanmış bir yedeklemeyi değiştirin veya durdurun.](./media/backup-azure-delete-vault/modify-schedule-backup.png)

3. **Zamanlanmış Yedeklemeyi Durdur** sayfasından **Finish'i**seçin.

    ![Zamanlanmış bir yedeklemeyi durdurun.](./media/backup-azure-delete-vault/stop-schedule-backup.png)
4. El ile oluşturmanız gereken bir güvenlik PIN'i (kişisel kimlik numarası) girmeniz istenir. Bunu yapmak için önce Azure portalında oturum açın.
5. Kurtarma **Hizmetleri kasa** > **Ayarları** > **Özellikleri'ne**gidin.
6. **Güvenlik PIN'i**altında **Oluştur'u**seçin. Bu PIN'i kopyalayın. PIN yalnızca beş dakika için geçerlidir.
7. Yönetim konsolunda PIN'i yapıştırın ve ardından **Tamam'ı**seçin.

    ![Bir güvenlik PIN'i oluşturun.](./media/backup-azure-delete-vault/security-pin.png)

8. Yedekleme **İlerlemesini Değiştir** sayfasında aşağıdaki ileti görüntülenir: *Silinen yedekleme verileri 14 gün boyunca saklanır. Bu süre den sonra, yedekleme verileri kalıcı olarak silinir.*  

    ![Yedekleme altyapısını silin.](./media/backup-azure-delete-vault/deleted-backup-data.png)

Şirket içi yedekleme öğelerini sildikten sonra portaldan sonraki adımları izleyin.

## <a name="re-enable-protection"></a>Yeniden koruma yı etkinleştirme

Verileri tutarken korumayı durdurduysanız ve korumayı sürdürmeye karar verdiyseniz, yedekleme ilkesini değiştir'i kullanarak yedekleme zamanlamasını yeniden etkinleştirebilirsiniz.

1. **Eylemler'de** **Yedeklemeyi Zamanla'yı**seçin.
1. **Yedekleme zamanlamasını Yeniden etkinleştir'i seçin. Ayrıca yedekleme öğelerini veya zamanlarını değiştirebilir** ve **İleri'yi**tıklatabilirsiniz.<br>

    ![Yedekleme altyapısını silin.](./media/backup-azure-manage-mars/re-enable-policy-next.png)
1. **Yedeklemeye Öğeleri Seç'te** **İleri'yi**tıklatın.

    ![Yedekleme altyapısını silin.](./media/backup-azure-manage-mars/re-enable-next.png)
1. **Yedekleme Çizelgesi Belirt'** te, yedekleme zamanlamasını belirtin ve **İleri'yi**tıklatın.
1. **Bekletme İlkesi Seç'te**bekletme süresini belirtin ve **İleri'yi**tıklatın.
1. Son olarak **Onay** ekranında, ilke ayrıntılarını gözden geçirin ve **Bitir'i**tıklatın.

## <a name="re-generate-passphrase"></a>Parolayı yeniden oluşturma

Bir parola, Mars aracısını Kullanarak Azure'a veya Azure'dan şirket içi veya yerel makinenizi yedeklerken veya geri alırken verileri şifrelemek ve şifresini çözmek için kullanılır. Parolayı kaybettiyseniz veya unuttuysanız, aşağıdaki adımları izleyerek parolayı yeniden oluşturabilirsiniz (makineniz hala Kurtarma Hizmetleri Kasası'na kayıtlı ve yedekleme yapılandırılır sayılsa bile).

- MARS aracı konsolundan, **> Eylemleri Bölme** > **Özellikleri Değiştir'e** gidin. Ardından **Şifreleme sekmesine**gidin.<br>
- **Geçiş Cümlesi onay** kutusunu değiştir'i seçin.<br>
- Yeni bir parola girin veya **Parola İfadesi Oluştur'u**tıklatın.
- Yeni parolayı kaydetmek için **Gözat'ı** tıklatın.

    ![Parola oluşturma.](./media/backup-azure-manage-mars/passphrase.png)
- Değişiklikleri uygulamak için **Tamam'ı** tıklatın.  Kurtarma Hizmetleri Kasası için Azure portalında [Güvenlik Özelliği](https://docs.microsoft.com/azure/backup/backup-azure-security-feature#enable-security-features) etkinleştirilirse, Güvenlik PIN'ini girmeniz istenir. PIN'i almak için bu [makalede](https://docs.microsoft.com/azure/backup/backup-azure-security-feature#authentication-to-perform-critical-operations)listelenen adımları izleyin.<br>
- Güvenlik PIN'ini portaldan yapıştırın ve değişiklikleri uygulamak için **Tamam'ı** tıklatın.<br>

    ![Parola oluşturma.](./media/backup-azure-manage-mars/passphrase2.png)
- Parolanın tercihen Azure Anahtar Kasası'nda, alternatif bir konumda (kaynak makine dışında) güvenli bir şekilde kaydedilmesini sağlayın. MARS ajanlarıyla yedeklenen birden fazla makineniz varsa tüm parolaları takip edin.


## <a name="next-steps"></a>Sonraki adımlar

- Desteklenen senaryolar ve sınırlamalar hakkında bilgi için [MARS Aracısı Için Destek Matrisi'ne](https://docs.microsoft.com/azure/backup/backup-support-matrix-mars-agent)bakın.
- [İsteğe bağlı yedekleme ilkesi tutma davranışı](backup-windows-with-mars-agent.md#set-up-on-demand-backup-policy-retention-behavior)hakkında daha fazla bilgi edinin.
