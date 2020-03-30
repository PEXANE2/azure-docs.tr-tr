---
title: Azure Stack VM'lerinde dosyaları yedekleme
description: Azure Yığını dosyalarını ve uygulamalarını Azure Yığını ortamınıza yedeklemek ve kurtarmak için Azure Yedekleme'yi kullanın.
ms.topic: conceptual
ms.date: 06/05/2018
ms.openlocfilehash: 2bcdf7f720708db1487d7d5cdaee41dc93c05728
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74172328"
---
# <a name="back-up-files-and-applications-on-azure-stack"></a>Azure Yığını'nda dosyaları ve uygulamaları yedekleme

Azure Yığını'ndaki dosyaları ve uygulamaları korumak (veya yedeklemek) için Azure Yedekleme'yi kullanabilirsiniz. Dosyaları ve uygulamaları yedeklemek için Microsoft Azure Yedekleme Sunucusu'nu Azure Yığını'nda çalışan sanal bir makine olarak yükleyin. Aynı sanal ağdaki herhangi bir Azure Stack sunucusundaki dosyaları koruyabilirsiniz. Azure Yedekleme Sunucusu'nu yükledikten sonra, kısa vadeli yedekleme verileri için kullanılabilen yerel depolama alanını artırmak için Azure diskleri ekleyin. Azure Yedekleme Sunucusu, uzun süreli bekletme için Azure depolama alanını kullanır.

> [!NOTE]
> Azure Yedekleme Sunucusu ve Sistem Merkezi Veri Koruma Yöneticisi (DPM) benzer olsa da, DPM Azure Stack ile kullanılmak üzere desteklenmez.
>

Bu makale, Azure Yedekleme Sunucusu'nun Azure Yığını ortamına yüklenmesini kapsamaz. Azure Yedekleme Sunucusu'nu Azure Yığını'na yüklemek için Azure [Yedekleme Sunucusu'nu yükleyin.](backup-mabs-install-azure-stack.md)

## <a name="back-up-files-and-folders-in-azure-stack-vms-to-azure"></a>Azure Yığın VM'lerinde Dosya ve Klasörleri Azure'a Yedekleme

Azure Yığını sanal makinelerindeki Dosyaları korumak için Azure Yedekleme Sunucusu'nu yapılandırmak için Azure Yedekleme Sunucusu konsolunu açın. Konsolu koruma gruplarını yapılandırmak ve sanal makinelerinizdeki verileri korumak için kullanırsınız.

1. Azure Yedekleme Sunucusu **konsolunda, Koruma'yı** tıklatın ve araç çubuğunda **Yeni Koruma Grubu Oluştur** sihirbazını açmak için **Yeni'yi** tıklatın.

   ![Azure Yedekleme Sunucusu konsolunda korumayı yapılandırma](./media/backup-mabs-files-applications-azure-stack/1-mabs-menu-create-protection-group.png)

    Sihirbazın açılması birkaç saniye sürebilir. Sihirbaz açıldıktan sonra Koruma Grubu **Türü Seç** ekranına ilerlemek için **İleri'yi** tıklatın.

   ![Yeni Koruma grubu sihirbazı açılıyor](./media/backup-mabs-files-applications-azure-stack/2-create-new-protection-group-wiz.png)

2. Koruma **Grubu Türünü Seç** ekranında **Sunucular'ı** seçin ve **İleri'yi**tıklatın.

    ![Yeni Koruma grubu sihirbazı açılıyor](./media/backup-mabs-files-applications-azure-stack/3-select-protection-group-type.png)

    **Grup Üyelerini Seç** ekranı açılır.

    ![Yeni Koruma grubu sihirbazı açılıyor](./media/backup-mabs-files-applications-azure-stack/4-opening-screen-choose-servers.png)

3. Grup **Üyelerini Seç** ekranında, alt öğeler listesini genişletmek için tıklatın. **+** Korumak istediğiniz tüm öğeler için onay kutusunu seçin. Tüm öğeler seçildikten sonra **İleri'yi**tıklatın.

    ![Yeni Koruma grubu sihirbazı açılıyor](./media/backup-mabs-files-applications-azure-stack/5-select-group-members.png)

    Microsoft, bir koruma ilkesini paylaşacak tüm verileri tek bir koruma grubuna koymanızı önerir. Koruma gruplarının planlanması ve dağıtılması hakkında tam bilgi için, [Koruma Gruplarını Dağıt](https://docs.microsoft.com/system-center/dpm/create-dpm-protection-groups?view=sc-dpm-1801)Sistem Merkezi DPM makalesine bakın.

4. Veri **Koruma Yöntemini Seç** ekranında, koruma grubu için bir ad yazın. Kullanarak kısa süreli koruma istiyorum için onay kutusunu **seçin:** ve **çevrimiçi koruma istiyorum.** **İleri**'ye tıklayın.

    ![Yeni Koruma grubu sihirbazı açılıyor](./media/backup-mabs-files-applications-azure-stack/6-select-data-protection-method.png)

    Ben **online koruma istiyorum**seçmek için, öncelikle kullanarak kısa vadeli koruma istiyorum seçmelisiniz: Disk. **I want short-term protection using:** Azure Yedekleme Sunucusu teyp için koruma sağlamaz, bu nedenle disk kısa süreli koruma için tek seçenektir.

5. Kısa **Vadeli Hedefler Belirt** ekranında, diske kaydedilen kurtarma noktalarının ne kadar süreyle tutuleceğini ve artımlı yedeklemeleri ne zaman kaydedeceğinizi seçin. **İleri**'ye tıklayın.

    > [!IMPORTANT]
    > Azure **not** Yedekleme Sunucusu'na bağlı disklerde operasyonel kurtarma (yedekleme) verilerini beş günden uzun süre saklamamalısınız.
    >

    ![Yeni Koruma grubu sihirbazı açılıyor](./media/backup-mabs-files-applications-azure-stack/7-select-short-term-goals.png)

    Artımlı yedeklemeler için bir aralık seçmek yerine, her zamanlanan kurtarma noktasından hemen önce bir ekspres tam yedekleme çalıştırmak için, **kurtarma noktasından hemen önceyi**tıklatın. Uygulama iş yüklerini koruyorsanız, Azure Yedekleme Sunucusu Eşitleme sıklık zamanlamasına göre kurtarma noktaları oluşturur (uygulama artımlı yedeklemeleri desteklemeleri koşuluyla). Uygulama artımlı yedeklemeleri desteklemiyorsa, Azure Yedekleme Sunucusu ekspres tam yedekleme çalıştırıyor.

    **Dosya kurtarma noktaları**için, kurtarma noktaları oluşturmak için ne zaman belirtin. Kurtarma noktalarının oluşturulduğu haftanın saat lerini ve günlerini ayarlamak için **Değiştir'i** tıklatın.

6. Gözden **Geçir disk ayırma** ekranında, koruma grubu için ayrılan depolama havuzu disk alanını gözden geçirin.

    **Toplam Veri boyutu,** yedeklemek istediğiniz verilerin boyutudur ve Azure Yedekleme Sunucusu'nda **sağlanacak Disk alanı** koruma grubu için önerilen alandır. Azure Yedekleme Sunucusu, ayarlara göre ideal yedekleme hacmini seçer. Ancak, yedekleme birimi seçeneklerini Disk ayırma ayrıntıları'nda düzenleyebilirsiniz. İş yükleri için açılan menüden tercih edilen depolamayı seçin. Düzenlemeleriniz, Kullanılabilir Disk Depolaması bölmesindeki Toplam Depolama ve Boş Depolama değerlerini değiştirir. Underprovisioned alan, Azure Yedekleme Sunucusu'nun gelecekte yedeklemelerle sorunsuz bir şekilde devam etmek için ses birimine eklemenizi önerdiği depolama alanıdır.

7. **Çoğaltma oluşturma yöntemini**seç'te, ilk tam veri çoğaltma işlemini nasıl işlemek istediğinizi seçin. Ağ üzerinden çoğaltmaya karar verirseniz, Azure yoğun olmayan bir zaman seçmenizi önerir. Büyük miktarda veya en uygun ağ koşullarından daha az veri için, verileri çıkarılabilir ortam kullanarak çoğaltmayı düşünün.

8. **Tutarlılık Denetimi Seçenekleri** kısmında, tutarlılık denetimlerinin nasıl otomatikleştirilmesini istediğinizi seçin. Tutarlılık denetimlerinin yalnızca veri çoğaltma tutarsız olduğunda veya zamanlamaya göre çalışmasını etkinleştirin. Otomatik tutarlılık denetimini yapılandırmak istemiyorsanız, istediğiniz zaman manuel denetim çalıştırın:
    * Azure Yedekleme Sunucusu konsolunun **Koruma** alanında, koruma grubuna sağ tıklayın ve **Tutarlılık Denetimini Gerçekleştir'i**seçin.

9. Azure'a yedeklemeyi seçerseniz, **Çevrimiçi koruma verisini belirt** sayfasında, Azure'a yedeklemek istediğiniz iş yüklerinin seçildiğinden emin olun.

10. **Çevrimiçi yedekleme zamanlamasını belirt'te,** Azure'a artımlı yedeklemelerin ne zaman oluşması gerektiğini belirtin.

    Yedeklemeleri her gün/hafta/ay/yıl yapılacak şekilde ve çalışacakları saat/tarihi belirterek zamanlayabilirsiniz. Yedeklemeler günde iki kez gerçekleşebilir. Yedekleme işi her çalıştığında, Azure Yedekleme Sunucusu diskinde depolanan yedeklenmiş verilerin kopyasından Azure'da bir veri kurtarma noktası oluşturulur.

11. **Çevrimiçi bekletme ilkesini belirtin,** günlük/haftalık/aylık/yıllık yedeklemelerden oluşturulan kurtarma noktalarının Azure'da nasıl tutulduğunu belirtin.

12. **Çevrimiçi çoğaltmayı**seç'te, verilerin ilk tam çoğaltmasının nasıl oluştuğunu belirtin.

13. **Özet'te**ayarlarınızı gözden geçirin. **Grup Oluştur'u**tıklattığınızda, ilk veri çoğaltma oluşur. Veri çoğaltma bittiğinde, **Durum** sayfasında, koruma grubu durumu **Tamam**olarak gösterir. İlk yedekleme işi koruma grubu ayarlarına uygun olarak gerçekleşir.

## <a name="recover-file-data"></a>Dosya verilerini kurtarma

Sanal makinenize veri kurtarmak için Azure Yedekleme Sunucusu konsolunu kullanın.

1. Azure Yedekleme Sunucusu konsolunda, gezinti çubuğunda **Kurtarma'yı** tıklatın ve kurtarmak istediğiniz verilere göz atın. Sonuçlar bölmesinde verileri seçin.

2. Kurtarma noktaları bölümündeki takvimde, kalın renkteki tarihler kurtarma noktalarının kullanılabilir olduğunu gösterir. Kurtarmak için tarihi seçin.

3. **Kurtarılabilir öğe** bölmesinde, kurtarmak istediğiniz öğeyi seçin.

4. Kurtarma Sihirbazı’nı açmak için **Eylemler** bölmesindeki **Kurtar**’a tıklayın.

5. Verileri aşağıdaki şekilde kurtarabilirsiniz:

    * **Özgün konuma geri kazan** - İstemci bilgisayar VPN üzerinden bağlıysa, bu seçenek çalışmaz. Bunun yerine alternatif bir konum kullanın ve ardından bu konumdan verileri kopyalayın.
    * **Başka bir konuma geri kazan**

6. Kurtarma seçeneklerini belirtin:

    * **Varolan sürüm kurtarma davranışı**için **kopya oluştur,** **atla**veya **Üzerine Yaz'ı**seçin. Overwrite yalnızca özgün konuma geri kazanılırken kullanılabilir.
    * **Güvenlik geri yükleme** **için, hedef bilgisayarın ayarlarını uygula'yı** seçin veya kurtarma noktası **sürümünün güvenlik ayarlarını uygulayın.**
    * **Ağ bant genişliği kullanımı nın daraltlanması**için, ağ bant genişliği kullanımının daraltılmasını etkinleştirmek için **Değiştir'i** tıklatın.
    * **Bildirim** **Kurtarma tamamlandığında e-posta gönder'i**tıklatın ve bildirimi alacak alıcıları belirtin. E-posta adreslerini virgülle ayırın.
    * Seçimleri yaptıktan sonra **İleri'yi** tıklatın

7. Kurtarma ayarlarınızı gözden geçirin ve **Kurtar**'ı tıklatın.

    >[!Note]
    >Kurtarma işi devam ederken, seçili kurtarma öğeleri için tüm eşitleme işleri iptal edilir.

Modern Yedekleme Depolama (MBS) kullanıyorsanız, File Server son kullanıcı kurtarma (EUR) desteklenmez. File Server EUR, Modern Yedekleme Depolama'nın kullanmadığı Birim Gölge Kopyalama Hizmeti'ne (VSS) bağımlıdır. EUR etkinse, verileri kurtarmak için aşağıdaki adımları kullanın:

1. Korumalı dosyalara gidin ve dosya adını sağ tıklatın ve **Özellikler'i**seçin.

2. **Özellikler** menüsünde, **Önceki Sürümler'i** tıklatın ve kurtarmak istediğiniz sürümü seçin.

## <a name="view-azure-backup-server-with-a-vault"></a>Azure Yedekleme Sunucusu'nun kasayla görüntüle

Azure portalındaki Azure Yedekleme Sunucusu varlıklarını görüntülemek için aşağıdaki adımları izleyebilirsiniz:

1. Kurtarma Hizmetleri kasası açık.
2. Yedekleme Altyapısı'nı tıklatın.
3. Yedekleme Yönetimi Sunucularını görüntüleyin.

## <a name="next-steps"></a>Sonraki adımlar

Diğer iş yüklerini korumak için Azure Yedekleme Sunucusu'nu kullanma hakkında bilgi için aşağıdaki makalelerden birine bakın:

* [SharePoint çiftliğini yedekle](https://docs.microsoft.com/azure/backup/backup-mabs-sharepoint-azure-stack)
* [SQL sunucusunu yedekleme](https://docs.microsoft.com/azure/backup/backup-mabs-sql-azure-stack)
