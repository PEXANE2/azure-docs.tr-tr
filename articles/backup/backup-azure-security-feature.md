---
title: Karma yedeklemeleri koruyan güvenlik özellikleri
description: Yedeklemeleri daha güvenli hale getirmek için Azure Yedekleme'de güvenlik özelliklerini nasıl kullanacağınızı öğrenin
ms.reviewer: utraghuv
ms.topic: conceptual
ms.date: 06/08/2017
ms.openlocfilehash: 7213f26493a118c2cb32f8f9935b4954176b99a2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77586402"
---
# <a name="security-features-to-help-protect-hybrid-backups-that-use-azure-backup"></a>Azure Yedekleme kullanan karma yedeklemeleri korumaya yardımcı olacak güvenlik özellikleri

Kötü amaçlı yazılımlar, fidye yazılımları ve izinsiz giriş gibi güvenlik sorunlarıyla ilgili endişeler artmaktadır. Bu güvenlik sorunları hem para hem de veri açısından pahalıya mal olabilir. Azure Yedekleme, bu tür saldırılara karşı korunmak için artık karma yedeklemelerin korunmasına yardımcı olacak güvenlik özellikleri sağlar. Bu makalede, bir Azure Kurtarma Hizmetleri aracısı ve Azure Yedekleme Sunucusu kullanılarak bu özelliklerin nasıl etkinleştirilip kullanılacağı ele alınır. Bu özellikler şunlardır:

- **Önleme**. Parolayı değiştirmek gibi kritik bir işlem yapıldığında ek bir kimlik doğrulama katmanı eklenir. Bu doğrulama, bu tür işlemlerin yalnızca geçerli Azure kimlik bilgilerine sahip kullanıcılar tarafından gerçekleştirilebilmesini sağlamak içindir.
- **Uyarı .** Yedekleme verilerini silme gibi kritik bir işlem yapıldığında abonelik yöneticisine bir e-posta bildirimi gönderilir. Bu e-posta, kullanıcının bu tür eylemler hakkında hızlı bir şekilde bilgilendirilmesini sağlar.
- **Kurtarma**. Silinen yedekleme verileri silinme tarihinden itibaren 14 gün daha saklanır. Bu, belirli bir süre içinde verilerin kurtarılabilirliğini sağlar, bu nedenle bir saldırı gerçekleşse bile veri kaybı olmaz. Ayrıca, bozuk verilere karşı korumak için daha fazla sayıda minimum kurtarma noktası korunur.

> [!NOTE]
> Altyapıyı hizmet (IaaS) VM yedeklemesi olarak kullanıyorsanız güvenlik özellikleri etkinleştirilmemelidir. Bu özellikler henüz IaaS VM yedeklemesi için mevcut değildir, bu nedenle etkinleştirme herhangi bir etkiye sahip olmayacaktır. Güvenlik özellikleri yalnızca kullanıyorsanız etkinleştirilmelidir: <br/>
>  * **Azure Yedekleme aracısı.** Minimum aracı sürümü 2.0.9052. Bu özellikleri etkinleştirdikten sonra, kritik işlemleri gerçekleştirmek için bu aracı sürümüne yükseltmeniz gerekir. <br/>
>  * **Azure Yedekleme Sunucusu**. Azure Yedekleme Sunucusu güncelleştirmesi ile Minimum Azure Yedekleme aracısı sürümü 2.0.9052 güncelleştirme1. <br/>
>  * **Sistem Merkezi Veri Koruma Yöneticisi**. Veri Koruma Yöneticisi 2012 R2 UR12 veya Veri Koruma Yöneticisi 2016 UR2 ile minimum Azure Yedekleme aracısı sürümü 2.0.9052. <br/>


> [!NOTE]
> Bu özellikler yalnızca Kurtarma Hizmetleri kasası için kullanılabilir. Yeni oluşturulan tüm Kurtarma Hizmetleri kasaları varsayılan olarak bu özelliklere sahiptir. Varolan Kurtarma Hizmetleri kasaları için, kullanıcılar aşağıdaki bölümde belirtilen adımları kullanarak bu özellikleri etkinleştirin. Özellikler etkinleştirildikten sonra, kasaya kayıtlı tüm Kurtarma Hizmetleri aracısı bilgisayarlar, Azure Yedekleme Sunucusu örnekleri ve Veri Koruma Yöneticisi sunucuları için geçerlidir. Bu ayarı etkinleştirmek tek seferlik bir eylemdir ve bu özellikleri etkinleştirdikten sonra devre dışı bırakamazsınız.
>

## <a name="enable-security-features"></a>Güvenlik özelliklerini etkinleştirme

Kurtarma Hizmetleri kasası oluşturuyorsanız, tüm güvenlik özelliklerini kullanabilirsiniz. Varolan bir kasayla çalışıyorsanız, aşağıdaki adımları izleyerek güvenlik özelliklerini etkinleştirin:

1. Azure kimlik bilgilerinizi kullanarak Azure portalında oturum açın.
2. **Gözat'ı**seçin ve **Kurtarma Hizmetleri**yazın.

    ![Azure portalı Gözat seçeneğinin ekran görüntüsü](./media/backup-azure-security-feature/browse-to-rs-vaults.png) <br/>

    Kurtarma hizmetleri kasalarının listesi görünür. Bu listeden bir kasa seçin. Seçilen kasa panosu açılır.
3. Kasanın altında görünen öğeler listesinden **Ayarlar'ın**altında **Özellikler'i**tıklatın.

    ![Kurtarma Hizmetleri kasa seçeneklerinin ekran görüntüsü](./media/backup-azure-security-feature/vault-list-properties.png)
4. **Güvenlik Ayarları**altında, **Güncelleştir'i**tıklatın.

    ![Kurtarma Hizmetleri kasa özelliklerinin ekran görüntüsü](./media/backup-azure-security-feature/security-settings-update.png)

    Güncelleştirme bağlantısı, özelliklerin bir özetini sağlayan ve bunları etkinleştirmenizi sağlayan **Güvenlik Ayarları** bıçaklarını açar.
5. Açılan listeden **Azure Çok Faktörlü Kimlik Doğrulaması'nı yapılandırıldınız mı?** [Azure Multi-Factor Authentication](../active-directory/authentication/multi-factor-authentication.md) Etkinleştirilirse, Azure portalında oturum açarken başka bir aygıttan (örneğin, bir cep telefonu) kimlik doğrulamanız istenir.

   Yedekleme'de kritik işlemleri gerçekleştirdiğiniz zaman, Azure portalında kullanılabilen bir güvenlik PIN'i girmeniz gerekir. Azure Çok Faktörlü Kimlik Doğrulaması'nı etkinleştirmek bir güvenlik katmanı ekler. Yalnızca geçerli Azure kimlik bilgilerine sahip ve ikinci bir aygıttan kimlik doğrulaması yapılan yetkili kullanıcılar Azure portalına erişebilir.
6. Güvenlik ayarlarını kaydetmek için **Etkinleştir'i** seçin ve **Kaydet'i**tıklatın. Önceki adımda **Azure Çok Faktörlü Kimlik Doğrulaması'nı yapılandırdığınız değerden** bir değer seçtikten sonra **Etkinleştir'i** seçebilirsiniz.

    ![Güvenlik ayarlarının ekran görüntüsü](./media/backup-azure-security-feature/enable-security-settings-dpm-update.png)

## <a name="recover-deleted-backup-data"></a>Silinen yedekleme verilerini kurtarma

Yedekleme, silinen yedekleme verilerini 14 gün daha saklar ve **yedekleme yi silmek için yedekleme** işlemini durdurun işlemi gerçekleştirilirse hemen silmez. Bu verileri 14 günlük süre içinde geri yüklemek için, ne kullandığınıza bağlı olarak aşağıdaki adımları izleyin:

**Azure Kurtarma Hizmetleri aracısı** kullanıcıları için:

1. Yedeklemelerin gerçekleştiği bilgisayar hala kullanılabilirdurumdaysa, silinen veri kaynaklarını yeniden koruyun ve tüm eski kurtarma noktalarından kurtarmak için Azure Kurtarma Hizmetleri'ndeki [aynı makinede Kurtarma verilerini](backup-azure-restore-windows-server.md#use-instant-restore-to-recover-data-to-the-same-machine) kullanın.
2. Bu bilgisayar kullanılamıyorsa, bu verileri almak için başka bir Azure Kurtarma Hizmetleri bilgisayarKullanmak için [alternatif bir makinede Kurtar'ı](backup-azure-restore-windows-server.md#use-instant-restore-to-restore-data-to-an-alternate-machine) kullanın.

**Azure Yedekleme Sunucusu** kullanıcıları için:

1. Yedeklemelerin gerçekleştiği sunucu hala kullanılabilirse, silinen veri kaynaklarını yeniden koruyun ve tüm eski kurtarma noktalarından kurtarmak için **Verileri Kurtar Özelliği'ni** kullanın.
2. Bu sunucu kullanılamıyorsa, bu verileri almak için başka bir Azure Yedekleme Sunucusu örneğini kullanmak için [başka bir Azure Yedekleme Sunucusu'ndan verileri kurtar'ı](backup-azure-alternate-dpm-server.md) kullanın.

**Veri Koruma Yöneticisi** kullanıcıları için:

1. Yedeklemelerin gerçekleştiği sunucu hala kullanılabilirse, silinen veri kaynaklarını yeniden koruyun ve tüm eski kurtarma noktalarından kurtarmak için **Verileri Kurtar Özelliği'ni** kullanın.
2. Bu sunucu kullanılamıyorsa, bu verileri almak için başka bir Veri Koruma Yöneticisi sunucusu kullanmak için [Dış DPM Ekle'yi](backup-azure-alternate-dpm-server.md) kullanın.

## <a name="prevent-attacks"></a>Saldırıları önleme

Yalnızca geçerli kullanıcıların çeşitli işlemleri gerçekleştirebilmesini sağlamak için denetimler eklendi. Bunlar arasında fazladan bir kimlik doğrulama katmanı eklemek ve kurtarma amacıyla minimum bekletme aralığını korumak yer alıyor.

### <a name="authentication-to-perform-critical-operations"></a>Kritik işlemleri gerçekleştirmek için kimlik doğrulaması

Kritik işlemler için fazladan bir kimlik doğrulama katmanı eklemenin bir parçası olarak, Verileri Sil ve **Geçiş Cümlesi** işlemlerini değiştir **ile Korumayı Durdur'u** gerçekleştirirken bir güvenlik PIN'i girmeniz istenir.

> [!NOTE]
>
> Şu anda, DPM ve MABS için **Delete verileriyle Korumayı Durdur** una yönelik güvenlik pini desteklenmez.

Bu PIN'i almak için:

1. Azure Portal’da oturum açın.
2. **Kurtarma Hizmetleri kasa** > **Ayarları** > **Özellikleri**göz atın.
3. **Güvenlik PIN'inin**altında **Oluştur'u**tıklatın. Bu, Azure Kurtarma Hizmetleri aracısı kullanıcı arabirimine girilecek PIN'i içeren bir bıçak açar.
    Bu PIN yalnızca beş dakika için geçerlidir ve bu süre den sonra otomatik olarak oluşturulur.

### <a name="maintain-a-minimum-retention-range"></a>Minimum bekletme aralığını koruyun

Her zaman geçerli sayıda kurtarma noktası olduğundan emin olmak için aşağıdaki denetimler eklenmiştir:

- Günlük bekletme için en az **yedi** günlük bekletme yapılmalıdır.
- Haftalık bekletme için en az **dört** haftalık bekletme yapılmalıdır.
- Aylık saklama için, en az **üç** aylık bekletme yapılmalıdır.
- Yıllık bekletme için, en az **bir** yıllık bekletme yapılmalıdır.

## <a name="notifications-for-critical-operations"></a>Kritik işlemler için bildirimler

Genellikle, kritik bir işlem gerçekleştirildiğinde, abonelik yöneticisine işlemle ilgili ayrıntıları içeren bir e-posta bildirimi gönderilir. Azure portalını kullanarak bu bildirimler için ek e-posta alıcıları yapılandırabilirsiniz.

Bu makalede belirtilen güvenlik özellikleri, hedeflenen saldırılara karşı savunma mekanizmaları sağlar. Daha da önemlisi, bir saldırı gerçekleşirse, bu özellikler verilerinizi kurtarma olanağı sağlar.

## <a name="troubleshooting-errors"></a>Hatalarda sorun giderme

| İşlem | Hata ayrıntıları | Çözüm |
| --- | --- | --- |
| İlke değişikliği |Yedekleme ilkesi değiştirilemedi. Hata: Geçerli işlem bir iç hizmet hatası nedeniyle başarısız oldu [0x29834]. Lütfen bir süre sonra işlemi yeniden deneyin. Sorun devam ederse, lütfen Microsoft Desteği'ne başvurun. |**Neden:**<br/>Bu hata, güvenlik ayarları etkinleştirildiğinde, bekletme aralığını yukarıda belirtilen minimum değerlerin altında azaltmaya çalıştığınızda ve desteklenmeyen sürümde olduğunuzda (desteklenen sürümler bu makalenin ilk notunda belirtilir) gelir. <br/>**Önerilen Eylem:**<br/> Bu durumda, ilkeyle ilgili güncelleştirmelere devam etmek için bekletme süresini belirtilen minimum bekletme süresinin (günlük için yedi gün, haftalık hafta için dört hafta, aylık için üç hafta veya yıllık bir yıl için) ayarlamanız gerekir. İsteğe bağlı olarak, tercih edilen yaklaşım, tüm güvenlik güncelleştirmelerini sağlamak için yedekleme aracısını, Azure Yedekleme Sunucusunu ve/veya DPM UR'yi güncelleştirmek tir. |
| Parolasözcülerini Değiştir |Girilen güvenlik PINi yanlıştır. (Kimlik numarası: 100130) Bu işlemi tamamlamak için doğru Güvenlik PIN'ini sağlayın. |**Neden:**<br/> Bu hata, kritik işlemi gerçekleştirirken geçersiz veya süresi dolmuş Güvenlik PIN'ini girdiğinizde (değişiklik parolası gibi) ortaya çıkar. <br/>**Önerilen Eylem:**<br/> İşlemi tamamlamak için geçerli Güvenlik PIN'ini girmeniz gerekir. PIN'i almak için Azure portalında oturum açın ve Güvenlik PINi Oluşturma özellikleri > > Özellikler > Kurtarma Hizmetleri kasasına gidin. Parolayı değiştirmek için bu PIN'i kullanın. |
| Parolasözcülerini Değiştir |İşlem başarısız oldu. Kimlik Numarası: 120002 |**Neden:**<br/>Bu hata, güvenlik ayarları etkinleştirildiğinde, parolayı değiştirmeye çalıştığınızda ve desteklenmeyen sürümde olduğunuzda (bu makalenin ilk notunda belirtilen geçerli sürümler) gelir.<br/>**Önerilen Eylem:**<br/> Parolayı değiştirmek için önce yedekleme aracısını en az 2.0.9052 sürümüne, Azure Yedekleme sunucusuna en az güncelleme 1'e ve/veya DPM'yi en az DPM 2012 R2 UR12 veya DPM 2016 UR2'ye (aşağıdaki indirme bağlantıları) güncelleştirmeniz ve ardından geçerli Güvenlik PIN'ini girmeniz gerekir. PIN'i almak için Azure portalında oturum açın ve Güvenlik PINi Oluşturma özellikleri > > Özellikler > Kurtarma Hizmetleri kasasına gidin. Parolayı değiştirmek için bu PIN'i kullanın. |

## <a name="next-steps"></a>Sonraki adımlar

- Bu özellikleri etkinleştirmek için [Azure Kurtarma Hizmetleri kasası ile başlayın.](backup-azure-vms-first-look-arm.md)
- Windows bilgisayarlarını korumaya ve yedekleme verilerinizi saldırılara karşı korumaya yardımcı olmak için [en son Azure Kurtarma Hizmetleri aracısını indirin.](https://aka.ms/azurebackup_agent)
- İş yüklerini korumaya ve yedekleme verilerinizi saldırılara karşı korumaya yardımcı olmak için [en son Azure Yedekleme Sunucusu'nu indirin.](https://support.microsoft.com/help/4457852/microsoft-azure-backup-server-v3)
- İş yüklerini korumaya ve yedekleme verilerinizi saldırılara karşı korumaya yardımcı olmak için [System Center 2012 R2 Veri Koruma Yöneticisi için UR12'yi indirin](https://support.microsoft.com/help/3209592/update-rollup-12-for-system-center-2012-r2-data-protection-manager) veya [System Center 2016 Veri Koruma Yöneticisi için UR2'yi indirin.](https://support.microsoft.com/help/3209593/update-rollup-2-for-system-center-2016-data-protection-manager)
