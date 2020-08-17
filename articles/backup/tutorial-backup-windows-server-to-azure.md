---
title: Öğretici-Windows Server 'ı Azure 'a yedekleme
description: Bu öğretici, şirket içi Windows Server’ların bir Kurtarma Hizmetleri kasasında yedeklenmesi işlemini açıklar.
ms.topic: tutorial
ms.date: 08/22/2018
ms.custom: mvc
ms.openlocfilehash: d2990b5950cf8812367c3a59c6cace39e4085e2a
ms.sourcegitcommit: 64ad2c8effa70506591b88abaa8836d64621e166
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88261914"
---
# <a name="back-up-windows-server-to-azure"></a>Windows Server’ı Azure’da Yedekleme

Windows Server'ınızı bozulmalara, saldırılara ve olağanüstü durumlara karşı korumak için Azure Backup’ı kullanabilirsiniz. Azure Backup, Microsoft Azure Kurtarma Hizmetleri (MARS) aracısı olarak bilinen basit bir araç sağlar. Windows Server Sistem Durumu aracılığıyla sunucu yapılandırmasını, dosyaları ve klasörleri korumak için Windows Server’a MARS aracısı yüklenir. Bu öğretici, Windows Server’ınızı Azure’da yedeklemek için MARS Aracısını nasıl kullanabileceğinizi açıklar. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
>
> * MARS Aracısını indirip ayarlama
> * Sunucunuzun yedeklemeleri için yedekleme zamanlarını ve bekletme zamanlamasını yapılandırma
> * İsteğe bağlı yedekleme gerçekleştirin

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

<https://portal.azure.com> adresinden Azure portalında oturum açın.

## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

Windows Server’ı yedekleyebilmeniz için önce yedeklemelerin veya geri yükleme noktalarının depolanacağı bir yer oluşturmanız gerekir. [Kurtarma Hizmetleri kasası](backup-azure-recovery-services-vault-overview.md), Windows Server’ınızdan yedeklemeleri depolayan Azure’daki bir kapsayıcıdır. Azure portalında Kurtarma Hizmetleri kasası oluşturmak için aşağıdaki adımları izleyin.

1. Sol taraftaki menüden **Tüm hizmetler**’i seçin ve hizmet listesinde **Kurtarma Hizmetleri** yazın. **Kurtarma Hizmetleri kasalarını**seçin.

   ![Kurtarma Hizmetleri kasasını aç](./media/tutorial-backup-windows-server-to-azure/full-browser-open-rs-vault_2.png)

2. **Kurtarma Hizmetleri kasaları** menüsünde **Ekle**' yi seçin.

   ![Kasa için bilgi sağlama](./media/tutorial-backup-windows-server-to-azure/provide-vault-detail-2.png)

3. **Kurtarma Hizmetleri Kasası** menüsünde,

    * **Ad** alanına *myRecoveryServicesVault* yazın.
    * **Abonelik** bölümünde geçerli abonelik kimliği görüntülenir.
    * **Kaynak grubu**için **var olanı kullan** ' ı seçin ve *myresourcegroup*öğesini seçin. *Myresourcegroup* yoksa, **Yeni oluştur** ' u seçin ve *myresourcegroup*yazın.
    * **Konum** açılan menüsünden *Batı Avrupa*’yı seçin.
    * Kurtarma Hizmetleri kasanızı oluşturmak için **Oluştur** ' u seçin.

Kasanız oluşturulduktan sonra Kurtarma Hizmetleri kasaları listesinde görünür.

## <a name="download-recovery-services-agent"></a>Kurtarma Hizmetleri aracısını indirme

Microsoft Azure Kurtarma Hizmetleri (MARS) aracısı, Windows Server ile Kurtarma Hizmetleri kasanız arasında bir ilişkilendirme oluşturur. Aşağıdaki yordamda, aracının sunucunuza nasıl indirileceği açıklanmaktadır.

1. Kurtarma Hizmetleri kasalarının listesinden, ilgili panoyu seçmek için **myRecoveryServicesVault** seçeneğini belirleyin.

   ![Panoyu açmak için kasayı seçin](./media/tutorial-backup-windows-server-to-azure/open-vault-from-list.png)

2. Kasa panosu menüsünde **Yedekle**' yi seçin.

3. **Yedekleme Hedefi** menüsünde:

   * **iş yükünüz nerede çalışıyor?** için **Şirket içi** ' i seçin
   * **Neleri yedeklemek istiyorsunuz?** için **Dosyalar ve klasörler**'i ve **Sistem Durumu**’nu seçin

   ![Yedekleme hedefi menüsü](./media/tutorial-backup-windows-server-to-azure/backup-goal.png)

4. **Altyapıyı hazırla menüsünü açmak** Için **altyapıyı hazırla** ' yı seçin.

5. **Altyapıyı hazırla** menüsünde, *MARSAgentInstaller.exe*indirmek Için **Windows Server veya Windows istemcisi için aracıyı indir** ' i seçin.

    ![Windows Server veya Windows Istemcisi için aracıyı indirin](./media/tutorial-backup-windows-server-to-azure/prepare-infrastructure.png)

    Yükleyici ayrı bir tarayıcı açar ve **MARSAgentInstaller.exe** dosyasını indirir.

6. İndirilen dosyayı çalıştırmadan önce altyapıyı hazırla menüsünde, **kasa kimlik bilgileri** dosyasını **İndir** ve Kaydet ' i seçin. MARS Aracısı ile Kurtarma Hizmetleri kasası arasında bağlantı kurmak için kasa kimlik bilgileri gereklidir.

    ![Kasa kimlik bilgilerini indirme](./media/tutorial-backup-windows-server-to-azure/download-vault-credentials.png)

## <a name="install-and-register-the-agent"></a>Aracıyı yükleme ve kaydetme

1. İndirilen **MARSagentinstaller.exe** dosyasını bulup üzerine çift tıklayın.
2. **Microsoft Azure Kurtarma Hizmetleri Aracısı Kurulum Sihirbazı** görüntülenir. Sihirbaza giderek, istendiğinde aşağıdaki bilgileri sağlayın ve **Kaydet**' i seçin.
   * Yükleme ve önbellek klasörü için konum.
   * İnternet'e bağlanmak için bir ara sunucu kullanıyorsanız ara sunucu bilgileri.
   * Kimliği doğrulanmış bir ara sunucu kullanıyorsanız kullanıcı adı ve parola bilgileriniz.

     ![Microsoft Azure Kurtarma Hizmetleri Aracısı Kurulum Sihirbazı](./media/tutorial-backup-windows-server-to-azure/mars-installer.png)

3. Sihirbazın sonunda, **kayda devam et** ' i seçin ve önceki yordamda Indirdiğiniz **kasa kimlik bilgileri** dosyasını sağlayın.

4. İstendiğinde, Windows Server’dan yedeklemeleri şifrelemek için bir şifreleme parolası sağlayın. Microsoft parola kaybolursa parolayı kurtaramasından bu yana parolayı güvenli bir konuma kaydedin.

5. **Son**’u seçin.

## <a name="configure-backup-and-retention"></a>Yedekleme ve Bekletmeyi Yapılandırma

Windows Server’da, Azure’daki yedeklemelerin ne zaman gerçekleşeceğini zamanlamak için Microsoft Azure Kurtarma Hizmetleri aracısını kullanırsınız. Aracıyı indirdiğiniz sunucuda aşağıdaki adımları yürütün.

1. Microsoft Azure Kurtarma Hizmetleri aracısını açın. Bunu, makinenizde **Microsoft Azure Backup** aramasını yaparak bulabilirsiniz.

2. Kurtarma Hizmetleri Aracısı konsolunda, **Eylemler bölmesi**altında **yedeklemeyi zamanla** ' yı seçin.

    ![Yedeklemeyi Zamanla](./media/tutorial-backup-windows-server-to-azure/mars-schedule-backup.png)

3. **İleri ' yi** seçerek **yedeklenecek öğeleri seçin** sayfasına gidin.

4. **Öğe Ekle** ' yi seçin ve açılan Iletişim kutusundan **sistem durumu** ve yedeklemek istediğiniz dosyalar veya klasörler ' i seçin. Ardından **Tamam**’ı seçin.

5. **İleri**’yi seçin.

6. **Yedekleme zamanlamasını belirtin (sistem durumu)** sayfasında, sistem durumu için yedeklemelerin tetiklenmesi gereken gün veya haftanın saatini belirtin ve **İleri ' yi**seçin.

7. **Bekletme Ilkesi Seç (sistem durumu)** sayfasında, sistem durumu için yedekleme kopyası Için bekletme ilkesini seçin ve **İleri ' yi**seçin.

8. Benzer şekilde, seçilen dosyalar ve klasörler için yedekleme zamanlamasını ve bekletme ilkesini seçin.

9. **Ilk yedekleme türünü seçin** sayfasında, **ağ üzerinden otomatik olarak**' ı seçin ve **İleri**' yi seçin.

10. **Onay** sayfasında, bilgileri gözden geçirin ve **son**' u seçin.

11. Sihirbaz Yedekleme zamanlamasını oluşturmayı tamamladıktan sonra **Kapat**' ı seçin.

## <a name="perform-an-on-demand-backup"></a>İsteğe bağlı yedekleme gerçekleştirme

Yedekleme işlerinin çalıştırıldığı zaman zamanlamayı oluşturdunuz. Ancak, sunucuyu yedeklediniz. Sunucunuz için veri dayanıklılığı sağlamak üzere isteğe bağlı yedekleme çalıştırmak için bir olağanüstü durum kurtarma en iyi uygulamadır.

1. Microsoft Azure Kurtarma Hizmetleri Aracısı konsolunda **Şimdi Yedekle**' yi seçin.

    ![Şimdi Yedekle](./media/tutorial-backup-windows-server-to-azure/backup-now.png)

2. **Şimdi Yedekle** sihirbazında, yedeklemek istediğiniz **dosya ve klasörler** veya **sistem durumu** ' ndan birini seçin ve **İleri ' yi** seçin.
3. **Onay** sayfasında, **Şimdi Yedekle** sihirbazının sunucunuzu yedeklemek için kullandığı ayarları gözden geçirin. Ardından **Yedekle**' yi seçin.
4. Sihirbazı kapatmak için **Kapat** ' ı seçin. Yedekleme işlemi tamamlanmadan önce sihirbazı kapatırsanız, sihirbaz arka planda çalışmaya devam eder.
5. İlk yedekleme tamamlandıktan sonra, MARS aracısı konsolunun **İşler** bölmesinde **İş tamamlandı** durumu görüntülenir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure portalı kullanarak şu işlemleri gerçekleştirdiniz:

> [!div class="checklist"]
>
> * Kurtarma Hizmetleri kasası oluşturma
> * Microsoft Azure Kurtarma Hizmetleri aracısını indirme
> * Aracıyı yükleme
> * Windows Server için yedeklemeyi yapılandırma
> * İsteğe bağlı yedekleme gerçekleştirme

Azure’dan Windows Server’a dosyaları kurtarmak için sonraki öğreticiye devam edin

> [!div class="nextstepaction"]
> [Azure’dan Windows Server’a dosyaları kurtarma](./tutorial-backup-restore-files-windows-server.md)
