---
title: MARS Aracısı yedeklemelerini yönetme ve izleme
description: Azure Backup hizmetini kullanarak Microsoft Azure Kurtarma Hizmetleri (MARS) Aracısı yedeklemelerini yönetme ve izleme hakkında bilgi edinin.
ms.reviewer: srinathv
ms.topic: conceptual
ms.date: 10/07/2019
ms.openlocfilehash: c11d73edd32c197aac2cec58eeb1cc20e5c6a339
ms.sourcegitcommit: bc792d0525d83f00d2329bea054ac45b2495315d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78673259"
---
# <a name="manage-microsoft-azure-recovery-services-mars-agent-backups-by-using-the-azure-backup-service"></a>Azure Backup hizmetini kullanarak Microsoft Azure Kurtarma Hizmetleri (MARS) aracı yedeklemelerini yönetme

Bu makalede, Microsoft Azure Kurtarma Hizmetleri aracısıyla yedeklenen dosya ve klasörlerin nasıl yönetileceği açıklanır.

## <a name="modify-a-backup-policy"></a>Yedekleme ilkesini değiştirme

Yedekleme ilkesini değiştirirken, yeni öğeler ekleyebilir, var olan öğeleri yedekten kaldırabilir veya dışlama ayarları kullanılarak yedeklenen dosyaları dışarıda bırakabilirsiniz.

- **Öğe Ekle** bu seçeneği yalnızca yedeklenecek yeni öğeler eklemek için kullanın. Varolan öğeleri kaldırmak için **öğeleri kaldır** veya **dışlama ayarları** seçeneğini kullanın.  
- **Öğeleri kaldır** öğeleri yedeklenmeden kaldırmak için bu seçeneği kullanın.
  - **Öğeleri kaldırmak**yerine bir birim içindeki tüm öğeleri kaldırmak Için **dışlama ayarlarını** kullanın.
  - Bir birimdeki tüm seçimlerin temizlenmesi, öğelerin eski yedeklerinin, son yedekleme sırasında değişiklik için kapsam olmadan saklama ayarlarına göre korunmasına neden olur.
  - Bu öğeleri yeniden seçtiğinizde, ilk tam yedekleme ve yeni ilke değişiklikleri eski yedeklemelere uygulanmaz.
  - Tüm birim seçimini kaldırmak, bekletme ilkesini değiştirmek için herhangi bir kapsam olmadan son yedeklemeyi korur.
- **Dışlama ayarları** belirli öğelerin yedeklenmesini hariç tutmak için bu seçeneği kullanın.

### <a name="add-new-items-to-existing-policy"></a>Varolan ilkeye yeni öğe ekleme

1. **Eylemler**' de, **yedeklemeyi zamanla**' ya tıklayın.

    ![Windows Server yedeklemesini zamanlama](./media/backup-configure-vault/schedule-first-backup.png)

2. **İlke öğesi Seç** sekmesinde, **dosyalarınız ve klasörleriniz Için Yedekleme zamanlamasını değiştir** ' i seçin ve **İleri**' ye tıklayın.

    ![Ilke öğelerini seçin](./media/backup-azure-manage-mars/select-policy-items.png)

3. **Zamanlamayı yedeklemeyi Değiştir veya Durdur** sekmesinde **yedekleme öğelerinde veya saatlerde değişiklik yap** ' ı seçin ve **İleri**' ye tıklayın.

    ![Yedeklemeyi değiştirin veya zamanlayın](./media/backup-azure-manage-mars/modify-schedule-backup.png)

4. Yedeklenecek **öğeleri seçin** sekmesinde, yedeklemek istediğiniz öğeleri eklemek Için **öğe Ekle** ' ye tıklayın.

    ![Yedekleme öğesi ekleme veya zamanlama](./media/backup-azure-manage-mars/modify-schedule-backup-add-items.png)

5. **Öğeleri seç** penceresinde, eklemek istediğiniz uçveya klasörler ' i seçin ve **Tamam**' a tıklayın.

    ![Öğeleri seçin](./media/backup-azure-manage-mars/select-item.png)

6. Sonraki adımları tamamlayarak işlemi tamamladıktan sonra **son** ' a tıklayın.

### <a name="add-exclusion-rules-to-existing-policy"></a>Mevcut ilkeye dışlama kuralları Ekle

Yedeklenmesini istemediğiniz dosya ve klasörleri atlamak için dışlama kuralları ekleyebilirsiniz. Bunu, yeni bir ilke tanımlarken veya var olan bir ilkeyi değiştirirken yapabilirsiniz.

1. Eylemler bölmesinden, **yedeklemeyi zamanla**' ya tıklayın. **Yedeklenecek öğeleri seçin** ' e gidin ve **dışlama ayarları**' na tıklayın.

    ![Öğeleri seçin](./media/backup-azure-manage-mars/select-exclusion-settings.png)

2. **Dışlama ayarları**' nda **dışlama Ekle**' ye tıklayın.

    ![Öğeleri seçin](./media/backup-azure-manage-mars/add-exclusion.png)

3. **Hariç tutulacak öğeleri seçin**listesinden dosya ve klasörlere gözatıp dışlamak istediğiniz öğeleri seçin ve **Tamam**' a tıklayın.

    ![Öğeleri seçin](./media/backup-azure-manage-mars/select-items-exclude.png)

4. Varsayılan olarak, seçilen klasörlerin içindeki tüm **alt klasörler** hariç tutulur. Bunu, **Evet** veya **Hayır**' a tıklayarak değiştirebilirsiniz. Aşağıda gösterildiği gibi, dışlanacak dosya türlerini düzenleyebilir ve belirtebilirsiniz:

    ![Öğeleri seçin](./media/backup-azure-manage-mars/subfolders-type.png)

5. Sonraki adımları tamamlayarak işlemi tamamladıktan sonra **son** ' a tıklayın.

### <a name="remove-items-from-existing-policy"></a>Varolan ilkeden öğeleri kaldır

1. Eylemler bölmesinden, **yedeklemeyi zamanla**' ya tıklayın. **Yedeklenecek öğeleri seçin**bölümüne gidin. Listeden, yedekleme zamanlamadan kaldırmak istediğiniz dosya ve klasörleri seçin ve **öğeleri kaldır**' a tıklayın.

    ![Öğeleri seçin](./media/backup-azure-manage-mars/select-items-remove.png)

> [!NOTE]
> Bir birimi ilkeden tamamen kaldırdığınızda dikkatli ilerleyin.  Yeniden eklemeniz gerekiyorsa, yeni bir birim olarak kabul edilir. Sonraki zamanlanmış yedekleme, artımlı yedekleme yerine bir Ilk yedekleme (tam yedekleme) gerçekleştirir. Daha sonra öğeleri daha sonra kaldırmanız ve eklemeniz gerekiyorsa, tam yedekleme yerine artımlı yedekleme sağlamak için **öğeleri kaldırmak** yerine **Dışlamalar ayarlarının** kullanılması önerilir.

2. Sonraki adımları tamamlayarak işlemi tamamladıktan sonra **son** ' a tıklayın.

## <a name="stop-protecting-files-and-folder-backup"></a>Dosya ve klasör yedeklemesini korumayı durdur

Dosya ve klasör yedeklemesini korumayı durdurmanın iki yolu vardır:

- **Korumayı durdurun ve yedekleme verilerini koruyun**.
  - Bu seçenek, gelecekteki tüm yedekleme işlerini korumadan durdurur.
  - Azure Backup hizmet, bekletme ilkesine bağlı olarak yedeklenen kurtarma noktalarını korur.
  - Süre dolmamış kurtarma noktaları için yedeklenmiş verileri geri yükleyebileceksiniz.
  - Korumayı sürdürmeye karar verirseniz, *Yedekleme zamanlamasını yeniden etkinleştir* seçeneğini kullanabilirsiniz. Bundan sonra, veriler yeni bekletme ilkesine göre tutulacaktır.
- **Korumayı durdurun ve yedekleme verilerini silin**.
  - Bu seçenek, gelecekteki tüm yedekleme işlerinin verilerinizi korumasını durdurur ve tüm kurtarma noktalarını siler.
  - *Bu yedekleme öğesi için verileriniz silinmiş bir iletiyle birlikte yedekleme verilerini sil uyarı e-postasını alırsınız. Bu veriler 14 gün boyunca geçici olarak kullanılabilir olacak, sonrasında kalıcı olarak silinecek* ve önerilen eylem, *verilerinizi kurtarmak için 14 gün içinde yedekleme öğesini yeniden koruacaktır.*
  - Korumayı sürdürmesini sağlamak için silme işleminden 14 gün içinde yeniden koruma yapın.

### <a name="stop-protection-and-retain-backup-data"></a>Korumayı durdurun ve yedekleme verilerini koruyun

1. MARS yönetim konsolunu açın, **eylemler bölmesine**gidin ve **yedeklemeyi zamanla**' yı seçin.

    ![Zamanlanmış bir yedeklemeyi değiştirin veya durdurun.](./media/backup-azure-manage-mars/mars-actions.png)
1. **Ilke öğesi Seç** sayfasında **dosyalarınız ve klasörleriniz için bir yedekleme zamanlaması Değiştir** ' i seçin ve **İleri**' ye tıklayın.

    ![Zamanlanmış bir yedeklemeyi değiştirin veya durdurun.](./media/backup-azure-manage-mars/select-policy-item-retain-data.png)
1. **Zamanlanmış bir yedeklemeyi Değiştir veya Durdur** sayfasında, **Bu yedekleme zamanlamasını kullanmayı Durdur ' u seçin, ancak bir zamanlama yeniden etkinleştirilinceye kadar depolanan yedeklemeleri saklayın**. Sonra **İleri**’yi seçin.

    ![Zamanlanmış bir yedeklemeyi değiştirin veya durdurun.](./media/backup-azure-manage-mars/stop-schedule-backup.png)
1. **Zamanlanan yedeklemeyi Duraklat** bölümünde bilgileri gözden geçirin ve **son**' a tıklayın.

    ![Zamanlanmış bir yedeklemeyi değiştirin veya durdurun.](./media/backup-azure-manage-mars/pause-schedule-backup.png)
1. **Yedekleme Işlemini Değiştir** bölümünde, zaman çizelgesi yedekleme duraklamanızın başarılı durumunda olduğunu denetleyin ve sona **çıkmak için Kapat** ' a tıklayın.

### <a name="stop-protection-and-delete-backup-data"></a>Korumayı Durdur ve yedekleme verilerini sil

1. MARS yönetim konsolunu açın, **Eylemler** bölmesine gidin ve **yedeklemeyi zamanla**' yı seçin.
2. **Zamanlanmış bir yedeklemeyi Değiştir veya Durdur** sayfasında, **Bu yedekleme zamanlamasını kullanarak Durdur ' u seçin ve tüm depolanan yedeklemeleri silin**. Sonra **İleri**’yi seçin.

    ![Zamanlanmış bir yedeklemeyi değiştirin veya durdurun.](./media/backup-azure-delete-vault/modify-schedule-backup.png)

3. **Zamanlanan yedeklemeyi durdur** sayfasında **son**' u seçin.

    ![Zamanlanmış bir yedeklemeyi durdurun.](./media/backup-azure-delete-vault/stop-schedule-backup.png)
4. El ile oluşturmanız gereken bir güvenlik PIN 'ı (kişisel kimlik numarası) girmeniz istenir. Bunu yapmak için öncelikle Azure portal oturum açın.
5. **Kurtarma Hizmetleri kasası** > **ayarları** > **Özellikler**' e gidin.
6. **GÜVENLIK PIN**'ı altında **Oluştur**' u seçin. Bu PIN 'ı kopyalayın. PIN yalnızca beş dakika için geçerlidir.
7. Yönetim konsolunda PIN 'i yapıştırın ve **Tamam**' ı seçin.

    ![Güvenlik PIN 'ı oluştur.](./media/backup-azure-delete-vault/security-pin.png)

8. **Yedekleme Ilerlemesini Değiştir** sayfasında aşağıdaki ileti görüntülenir: *silinen yedekleme verileri 14 gün boyunca saklanacaktır. Bu süreden sonra, yedekleme verileri kalıcı olarak silinir.*  

    ![Yedekleme altyapısını silin.](./media/backup-azure-delete-vault/deleted-backup-data.png)

Şirket içi yedekleme öğelerini sildikten sonra, portaldan sonraki adımları izleyin.

## <a name="re-enable-protection"></a>Korumayı yeniden etkinleştirin

Verileri korurken korumayı durdurduysanız ve korumayı sürdürmeye karar verdiyseniz Yedekleme zamanlamasını değiştir seçeneğini kullanarak yedeklemeyi yeniden etkinleştirebilirsiniz.

1. **Eylemler** üzerinde **yedeklemeyi zamanla**' yı seçin.
1. **Yedekleme zamanlamasını yeniden etkinleştir ' i seçin. Ayrıca, yedekleme öğelerini veya zamanları değiştirebilir** ve **İleri**' ye tıklayabilirsiniz.<br>

    ![Yedekleme altyapısını silin.](./media/backup-azure-manage-mars/re-enable-policy-next.png)
1. **Yedeklenecek öğeleri seçin**bölümünde **İleri**' ye tıklayın.

    ![Yedekleme altyapısını silin.](./media/backup-azure-manage-mars/re-enable-next.png)
1. **Yedekleme zamanlamasını belirtin**bölümünde Yedekleme zamanlamasını belirtip **İleri**' ye tıklayın.
1. **Bekletme Ilkesi Seç**bölümünde, saklama süresini belirtin ve **İleri**' ye tıklayın.
1. Son olarak, **onay** ekranında, ilke ayrıntılarını gözden geçirin ve **son**' a tıklayın.

## <a name="re-generate-passphrase"></a>Parolayı yeniden oluştur

Bir parola, şirket içi veya yerel makinenizi, MARS aracısını veya Azure 'dan veya Azure 'dan yedekleme sırasında verileri şifrelemek ve şifrelerini çözmek için kullanılır. Parolayı kaybettiyseniz veya unuttuysanız, bu adımları izleyerek, parolayı yeniden oluşturabilirsiniz (makineniz hala kurtarma hizmetleri kasası ile kaydedilir ve yedekleme yapılandırılmışsa):

- MARS Aracısı konsolundan **Eylemler bölmesi** ' ne gidin > **Özellikleri Değiştir** >. Ardından **şifreleme sekmesine**gidin.<br>
- **Parolayı Değiştir** onay kutusunu seçin.<br>
- Yeni bir parola girin veya **parola oluştur**' a tıklayın.
- Yeni parolayı kaydetmek için, **Araştır** ' a tıklayın.

    ![Parola oluştur.](./media/backup-azure-manage-mars/passphrase.png)
- Değişiklikleri uygulamak için **Tamam** ' ı tıklatın.  Kurtarma Hizmetleri Kasası için Azure portal [güvenlik özelliği](https://docs.microsoft.com/azure/backup/backup-azure-security-feature#enable-security-features) etkinleştirilmişse, güvenlik PIN 'ini girmeniz istenir. PIN 'i almak için bu [makalede](https://docs.microsoft.com/azure/backup/backup-azure-security-feature#authentication-to-perform-critical-operations)listelenen adımları izleyin.<br>
- Portaldan güvenlik PIN 'ini yapıştırın ve değişiklikleri uygulamak için **Tamam** ' a tıklayın.<br>

    ![Parola oluştur.](./media/backup-azure-manage-mars/passphrase2.png)
- Parolanın, Azure Key Vault tercihen farklı bir konuma (kaynak makineden başka) güvenli bir şekilde kaydedildiğinden emin olun. MARS aracılarıyla yedeklenmekte olan birden fazla makineniz varsa tüm parolaları izleyin.


## <a name="next-steps"></a>Sonraki adımlar

- Desteklenen senaryolar ve sınırlamalar hakkında daha fazla bilgi için, [Mars aracısının destek matrisine](https://docs.microsoft.com/azure/backup/backup-support-matrix-mars-agent)bakın.
- [İsteğe bağlı yedekleme ilkesi saklama davranışı](backup-windows-with-mars-agent.md#set-up-on-demand-backup-policy-retention-behavior)hakkında daha fazla bilgi edinin.
