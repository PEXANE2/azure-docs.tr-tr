---
title: Azure Yedekleme Sunucusundan veri kurtarma
description: Bu kasaya kayıtlı herhangi bir Azure Yedekleme Sunucusundan koruduğunuz verileri Kurtarma Hizmetleri kasasına kurtarın.
ms.topic: conceptual
ms.date: 07/09/2019
ms.openlocfilehash: 2a89697899fc244848854978de4b25e79ef6f184
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74173494"
---
# <a name="recover-data-from-azure-backup-server"></a>Azure Backup Sunucusu’ndan veri kurtarma

Yedeklediğiniz verileri kurtarma hizmetleri kasasına kurtarmak için Azure Yedekleme Sunucusu'nu kullanabilirsiniz. Bunu yapma işlemi Azure Yedekleme Sunucusu yönetim konsoluna entegre edilmiştir ve diğer Azure Yedekleme bileşenlerinin kurtarma iş akışına benzer.

> [!NOTE]
> Bu makale, [UR7 veya sonraki sürümlerle birlikte Sistem Merkezi Veri Koruma Yöneticisi 2012 R2](https://support.microsoft.com/kb/3065246)için geçerlidir, [en son Azure Yedekleme aracısı](https://aka.ms/azurebackup_agent)ile birlikte.
>
>

Azure Yedekleme Sunucusundan veri kurtarmak için:

1. Azure Yedekleme Sunucusu yönetim konsolunun **Kurtarma** sekmesinden **'Dış DPM Ekle'** (ekranın sol üst kısmında) seçeneğini tıklatın.

    ![Harici DPM Ekle](./media/backup-azure-alternate-dpm-server/add-external-dpm.png)
2. Verilerin kurtarıldığı **Azure Yedekleme Sunucusuyla** ilişkili kasadan yeni **kasa kimlik bilgilerini** indirin, Kurtarma Hizmetleri kasasına kayıtlı Azure Yedekleme Sunucuları listesinden Azure Yedekleme Sunucusu'nu seçin ve verileri kurtarılan sunucuyla ilişkili şifreleme **parolasını** sağlayın.

    ![Harici DPM Kimlik Bilgileri](./media/backup-azure-alternate-dpm-server/external-dpm-credentials.png)

   > [!NOTE]
   > Yalnızca aynı kayıt kasasıyla ilişkili Azure Yedekleme Sunucuları birbirlerinin verilerini kurtarabilir.
   >
   >

    Harici Azure Yedekleme Sunucusu başarıyla eklendikten sonra, **Kurtarma** sekmesinden harici sunucunun ve yerel Azure Yedekleme Sunucusunun verilerine göz atabilirsiniz.
3. Harici Azure Yedekleme Sunucusu tarafından korunan kullanılabilir üretim sunucuları listesine göz atın ve uygun veri kaynağını seçin.

    ![Harici DPM Sunucusuna Gözatın](./media/backup-azure-alternate-dpm-server/browse-external-dpm.png)
4. **Kurtarma noktalarının** düşüşünden **ayı ve yılı** seçin, kurtarma noktasının oluşturulduğu zaman için gerekli Kurtarma **tarihini** seçin ve **Kurtarma süresini**seçin.

    Alt bölmede, herhangi bir konuma göz atılabilir ve kurtarılabilir bir dosya ve klasör listesi görüntülenir.

    ![Harici DPM Sunucu Kurtarma Noktaları](./media/backup-azure-alternate-dpm-server/external-dpm-recoverypoint.png)
5. Uygun öğeyi sağ tıklatın ve **Kurtar'ı**tıklatın.

    ![Harici DPM kurtarma](./media/backup-azure-alternate-dpm-server/recover.png)
6. Seçimi **Kurtarma'yı**gözden geçirin. Yedek kopyanın kurtarıldığı verileri ve zamanı ve yedek kopyanın oluşturulduğu kaynağı doğrulayın. Seçim yanlışsa, uygun kurtarma noktasını seçmek için kurtarma sekmesine geri dönmek için **İptal'i** tıklatın. Seçim doğruysa, **İleri'yi**tıklatın.

    ![Dış DPM kurtarma özeti](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-summary.png)
7. **Alternatif bir konuma Kurtar'ı**seçin. Kurtarma için doğru konuma **göz atın.**

    ![Harici DPM kurtarma alternatif konum](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-alternate-location.png)
8. **Kopya oluşturmak**için ilgili seçeneği seçin , **Atla**veya **Üzerine Yaz.**

   * **Kopya oluştur** - bir ad çakışma varsa dosyanın bir kopyasını oluşturur.
   * **Atla** - bir ad çakışması varsa, özgün dosyayı bırakan dosyayı kurtarmaz.
   * **Overwrite** - bir ad çakışma varsa, dosyanın varolan kopyasını n üzerine yazar.

     **Güvenliği Geri Yüklemek**için uygun seçeneği seçin. Verilerin kurtarıldığı hedef bilgisayarın güvenlik ayarlarını veya kurtarma noktası oluşturulduğu sırada ürün için geçerli olan güvenlik ayarlarını uygulayabilirsiniz.

     Kurtarma başarıyla tamamlandıktan sonra **Bildirim** gönderilip gönderilmediğini belirleyin.

     ![Harici DPM Kurtarma Bildirimleri](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-notifications.png)
9. **Özet** ekranı şimdiye kadar seçilen seçenekleri listeler. **'Kurtar'ı**tıklattığınızda, veriler uygun şirket içi konuma kurtarılır.

    ![Harici DPM Kurtarma Seçenekleri Özeti](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-options-summary.png)

   > [!NOTE]
   > Kurtarma işi Azure Yedekleme Sunucusu'nun **İzleme** sekmesinde izlenebilir.
   >
   >

    ![Kurtarma Yı izleme](./media/backup-azure-alternate-dpm-server/monitoring-recovery.png)
10. Harici DPM sunucusunun görünümünü kaldırmak için DPM sunucusunun **Kurtarma** sekmesinde **Dış DPM'yi** temizle'yi tıklatabilirsiniz.

    ![Dış DPM'yi Temizle](./media/backup-azure-alternate-dpm-server/clear-external-dpm.png)

## <a name="troubleshooting-error-messages"></a>Sorun giderme hata iletileri

| Hayır. | Hata İletisi | Sorun giderme adımları |
|:---:|:--- |:--- |
| 1. |Bu sunucu, kasa kimlik bilgisi tarafından belirtilen kasaya kayıtlı değildir. |**Sebep:** Bu hata, seçilen kasa kimlik bilgileri dosyası, kurtarma nın denendiği Azure Yedekleme Sunucusuile ilişkili Kurtarma Hizmetleri kasasına ait olmadığında ortaya çıkar. <br> **Çözünürlük:** Azure Yedekleme Sunucusu'nun kayıtlı olduğu Kurtarma Hizmetleri kasasından kasa kimlik bilgilerini indirin. |
| 2. |Kurtarılabilir veriler kullanılamıyor veya seçili sunucu Bir DPM sunucusu değil. |**Sebep:** Kurtarma Hizmetleri kasasına kayıtlı başka azure yedekleme sunucusu yoktur veya sunucular meta verileri henüz yüklememiş veya seçili sunucu Bir Azure Yedekleme Sunucusu değildir (Windows Server veya Windows İstemci kullanarak). <br> **Çözünürlük:** Kurtarma Hizmetleri kasasına kayıtlı başka Azure Yedekleme Sunucuları varsa, en son Azure Yedekleme aracısının yüklendiğinden emin olun. <br>Kurtarma Hizmetleri kasasına kayıtlı başka Azure Yedekleme Sunucuları varsa, kurtarma işlemini başlatmak için yüklemeden sonra bir gün bekleyin. Gece işi, tüm korumalı yedeklemeler için meta verileri buluta yükler. Veriler kurtarma için kullanılabilir olacaktır. |
| 3. |Bu kasaya başka bir DPM sunucusu kayıtlı değil. |**Sebep:** Kurtarmanın denendiği kasaya kayıtlı başka Azure Yedekleme Sunucusu yok.<br>**Çözünürlük:** Kurtarma Hizmetleri kasasına kayıtlı başka Azure Yedekleme Sunucuları varsa, en son Azure Yedekleme aracısının yüklendiğinden emin olun.<br>Kurtarma Hizmetleri kasasına kayıtlı başka Azure Yedekleme Sunucuları varsa, kurtarma işlemini başlatmak için yüklemeden sonra bir gün bekleyin. Gece işi, tüm korumalı yedeklemelerin meta verilerini buluta yükler. Veriler kurtarma için kullanılabilir olacaktır. |
| 4. |Sağlanan şifreleme geçiş tümceciği aşağıdaki sunucuyla ilişkili parolayla eşleşmez: ** \<sunucu adı>** |**Sebep:** Azure Yedekleme Sunucusu'nun kurtarılan verilerinden verileri şifreleme işleminde kullanılan şifreleme parolası, sağlanan şifreleme parolasıyla eşleşmez. Aracı verilerin şifresini çözemiyor. Bu nedenle kurtarma başarısız olur.<br>**Çözünürlük:** Lütfen verileri kurtarılan Azure Yedekleme Sunucusu ile ilişkili şifreleme parolasını tam olarak sağlayın. |

## <a name="next-steps"></a>Sonraki adımlar

Diğer SSS belgelerini okuyun:

* Azure VM yedeklemeleri hakkında [sık sorulan sorular](backup-azure-vm-backup-faq.md)
* Azure Backup aracısıyla ilgili [yaygın sorular](backup-azure-file-folder-backup-faq.md)
