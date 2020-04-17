---
title: SQL Server DB'lerini Azure VM'de yönetme ve izleme
description: Bu makalede, Azure VM'de çalışan SQL Server veritabanlarının nasıl yönetilen ve izlenir.
ms.topic: conceptual
ms.date: 09/11/2019
ms.openlocfilehash: 14e3a4797fe60a3d1857f1e6d947fa0c669bdcfe
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81537313"
---
# <a name="manage-and-monitor-backed-up-sql-server-databases"></a>Yedeklenmiş SQL Server veritabanlarını yönetme ve izleme

Bu makalede, Bir Azure sanal makinede (VM) çalışan ve [Azure Yedekleme](backup-overview.md) hizmeti tarafından Azure Yedekleme Kurtarma Hizmetleri kasasına yedeklenen SQL Server veritabanlarını yönetmek ve izlemek için kullanılan yaygın görevler açıklanmaktadır. İşleri ve uyarıları izlemeyi, veritabanı korumasını durdurmayı ve devam ettirmeyi, yedekleme işlerini çalıştırmayı ve yedeklemelerden bir VM kaydını silmeyi öğreneceksiniz.

SQL Server veritabanlarınız için henüz yedekleme yapılandırmadıysanız, [Azure VM'lerinde SQL Server veritabanlarını yedekle](backup-azure-sql-database.md)

## <a name="monitor-manual-backup-jobs-in-the-portal"></a>Portaldaki manuel yedekleme işlerini izleme

Azure Yedekleme, **Yedekleme işleri** portalında el ile tetiklenen tüm işleri gösterir. Bu portalda gördüğünüz işler veritabanı bulma ve kaydetme ve yedekleme ve geri yükleme işlemleri içerir.

![Yedekleme işleri portalı](./media/backup-azure-sql-database/jobs-list.png)

> [!NOTE]
> **Yedekleme işleri** portalı zamanlanmış yedekleme işleri göstermez. Bir sonraki bölümde açıklandığı gibi zamanlanmış yedekleme işlerini izlemek için SQL Server Management Studio'yu kullanın.
>

İzleme senaryoları hakkında daha fazla bilgi için [Azure portalında İzleme](backup-azure-monitoring-built-in-monitor.md) ve [Azure Monitörünü kullanarak İzleme'ye](backup-azure-monitoring-use-azuremonitor.md)gidin.  

## <a name="view-backup-alerts"></a>Yedekleme uyarılarını görüntüleme

Günlük yedeklemeleri her 15 dakikada bir oluştuğundan, yedekleme işlerini izlemek sıkıcı olabilir. Azure Yedekleme, e-posta uyarıları göndererek izlemeyi kolaylaştırır. E-posta uyarıları şunlardır:

- Tüm yedekleme hataları için tetiklenir.
- Hata kodu ile veritabanı düzeyinde konsolide.
- Yalnızca bir veritabanının ilk yedekleme hatası için gönderildi.

Veritabanı yedekleme uyarılarını izlemek için:

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Kasa panosunda **Uyarılar ve Olaylar'ı**seçin.

   ![Uyarıları ve Olayları Seçin](./media/backup-azure-sql-database/vault-menu-alerts-events.png)

3. **Uyarılar ve Olaylar'da** **Yedek Uyarıları**seçin.

   ![Yedek Uyarıları Seçin](./media/backup-azure-sql-database/backup-alerts-dashboard.png)

## <a name="stop-protection-for-a-sql-server-database"></a>SQL Server veritabanı için korumayı durdurma

BIR SQL Server veritabanını birkaç şekilde yedeklemeyi durdurabilirsiniz:

- Gelecekteki tüm yedekleme işlerini durdurun ve tüm kurtarma noktalarını silin.
- Gelecekteki tüm yedekleme işlerini durdurun ve kurtarma noktalarını sağlam bırakın.

Kurtarma noktalarını bırakmayı seçerseniz, şu ayrıntıları aklınızda bulundurun:

- Tüm kurtarma noktaları sonsuza kadar bozulmadan kalacak, tüm budama veri korumak ile durdurma koruma duracaktır.
- Korunan örnek ve tüketilen depolama için ücretlendirilirsiniz. Daha fazla bilgi için Azure [Yedekleme fiyatlandırması'na](https://azure.microsoft.com/pricing/details/backup/)bakın.
- Yedeklemeleri durdurmadan bir veri kaynağını silerseniz, yeni yedeklemeler başarısız olur. Eski kurtarma noktaları ilke göre sona erer, ancak yedeklemeleri durdurup verileri silene kadar son bir kurtarma noktası her zaman tutulur.

Veritabanı nın korumasını durdurmak için:

1. Kasa panosunda **Yedek Öğeler'i**seçin.

2. **Yedekleme Yönetim Türü**altında, Azure **VM'de SQL'i**seçin.

    ![Azure VM'de SQL'i seçin](./media/backup-azure-sql-database/sql-restore-backup-items.png)

3. Korumayı durdurmak istediğiniz veritabanını seçin.

    ![Korumayı durdurmak için veritabanını seçin](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)

4. Veritabanı menüsünde **yedeklemeyi durdur'u**seçin.

    ![Yedeklemeyi Durdur'u seçin](./media/backup-azure-sql-database/stop-db-button.png)

5. **Yedeklemeyi Durdur** menüsünde, verileri saklayıp saklamayacağınızveya silmeyi seçin. İstersen, bir neden ve yorum sağlayın.

    ![Yedeklemeyi Durdur menüsündeki verileri tutma veya silme](./media/backup-azure-sql-database/stop-backup-button.png)

6. **Yedeklemeyi Durdur'u**seçin.

> [!NOTE]
>
>Veri silme seçeneği hakkında daha fazla bilgi için aşağıdaki SSS bölümüne bakın:
>
>- [Otomatik korumalı bir örnekten bir veritabanını silersem, yedeklemelere ne olur?](faq-backup-sql-server.md#if-i-delete-a-database-from-an-autoprotected-instance-what-will-happen-to-the-backups)
>- [Otomatik korumalı bir veritabanının yedekleme işlemini durdurursam davranışı ne olacak?](faq-backup-sql-server.md#if-i-change-the-name-of-the-database-after-it-has-been-protected-what-will-be-the-behavior)
>
>

## <a name="resume-protection-for-a-sql-database"></a>SQL veritabanı için özgeçmiş koruması

SQL veritabanı nın korumasını durdurduğunuzda, **Yedekleme Verilerini Koru** seçeneğini seçerseniz, daha sonra korumayı sürdürebilirsiniz. Yedekleme verilerini saklamazsanız, korumaya devam edemezsiniz.

BIR SQL veritabanı için korumayı sürdürmek için:

1. Yedekleme öğesini açın ve **Yedeklemeye Devam'ı**seçin.

    ![Veritabanı korumasını sürdürmek için Devam yedeklemesini seçin](./media/backup-azure-sql-database/resume-backup-button.png)

2. Yedekleme **ilkesi** menüsünde bir ilke seçin ve sonra **Kaydet'i**seçin.

## <a name="run-an-on-demand-backup"></a>İsteğe bağlı yedekleme çalıştırma

Farklı türde isteğe bağlı yedeklemeler çalıştırabilirsiniz:

- Tam yedekleme
- Yalnızca kopyala tam yedekleme
- Değişiklik yedeği
- Günlük yedekleme

Yalnızca Kopyala tam yedekleme için bekletme süresini belirtmeniz gerekirken, isteğe bağlı tam yedekleme için bekletme aralığı geçerli andan itibaren otomatik olarak 45 gün olarak ayarlanır.

Daha fazla bilgi için [SQL Server yedekleme türlerine](backup-architecture.md#sql-server-backup-types)bakın.

## <a name="unregister-a-sql-server-instance"></a>SQL Server örneğini kayıt dışı

Korumayı devre dışı düşürdükten sonra ancak kasayı silmeden önce bir SQL Server örneğini kaldırın:

1. Kasa panosunda, **Yönet**altında Yedek **Altyapı'yı**seçin.  

   ![Yedekleme Altyapıseçin](./media/backup-azure-sql-database/backup-infrastructure-button.png)

2. **Yönetim Sunucuları**altında, **Korumalı Sunucular'ı**seçin.

   ![Korumalı Sunucuları Seçin](./media/backup-azure-sql-database/protected-servers.png)

3. **Korumalı Sunucularda,** kaydını kısımak için sunucuyu seçin. Kasayı silmek için tüm sunucuların kaydını silmeniz gerekir.

4. Korumalı sunucuya sağ tıklayın ve **Kayıt Dışı'yı**seçin.

   ![Sil'i Seçin](./media/backup-azure-sql-database/delete-protected-server.jpg)

## <a name="modify-policy"></a>İlkeyi değiştir

Yedekleme sıklığını veya bekletme aralığını değiştirmek için ilkeyi değiştirin.

> [!NOTE]
> Bekletme süresindeki herhangi bir değişiklik, yenilerinin yanı sıra tüm eski kurtarma noktalarına geriye dönük olarak uygulanacaktır.

Kasa panosunda,**Yedekleme İlkelerini** **Yönet'e** > gidin ve yönetmek istediğiniz ilkeyi seçin.

  ![Yedekleme ilkesini yönetme](./media/backup-azure-sql-database/modify-backup-policy.png)

  ![Yedekleme ilkesini değiştirme](./media/backup-azure-sql-database/modify-backup-policy-impact.png)

İlke değişikliği, ilişkili tüm Yedekleme Öğeleri'ni etkileyecek ve ilgili **yapılandırma koruma** işlerini tetikler.

### <a name="inconsistent-policy"></a>Tutarsız ilke

Bazen, bir değişiklik ilkesi işlemi bazı yedekleme öğeleri için **tutarsız** bir ilke sürümüne yol açabilir. Bu, bir değişiklik ilkesi işlemi tetiklendikten sonra ilgili **yapılandırma koruma** işi yedekleme öğesi için başarısız olduğunda olur. Yedekleme öğesi görünümünde aşağıdaki gibi görünür:

  ![Tutarsız ilke](./media/backup-azure-sql-database/inconsistent-policy.png)

Etkilenen tüm öğelerin ilke sürümünü tek bir tıklamayla düzeltebilirsiniz:

  ![Tutarsız ilkeyi düzeltme](./media/backup-azure-sql-database/fix-inconsistent-policy.png)

## <a name="re-register-extension-on-the-sql-server-vm"></a>SQL Server VM'de uzantıyı yeniden kaydedin

Bazen, VM'deki iş yükü uzantısı şu veya bu nedenle etkilenebilir. Bu gibi durumlarda, VM'de tetiklenen tüm işlemler başarısız olmaya başlar. Daha sonra VM'deki uzantıyı yeniden kaydetmeniz gerekebilir. **Yeniden kayıt** işlemi, işlemlerin devam etmesi için VM'deki iş yükü yedekleme uzantısını yeniden yükler.

Bu seçeneği dikkatli kullanın; zaten sağlıklı bir uzantılı bir VM'de tetiklendiğinde, bu işlem uzantın yeniden başlatılmasına neden olur. Bu, devam eden tüm işlerin başarısız olmasıyla sonuçlanabilir. Yeniden kaydetme işlemini tetiklemeden önce [belirtilerden](backup-sql-server-azure-troubleshoot.md#re-registration-failures) birini veya birkaçını lütfen kontrol edin.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için, [SQL Server veritabanındaki Sorun Gideri yedeklemelerine](backup-sql-server-azure-troubleshoot.md)bakın.
