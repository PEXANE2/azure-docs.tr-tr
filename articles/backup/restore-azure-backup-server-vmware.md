---
title: VMware VM’lerini Azure Backup Sunucusu ile geri yükleme
description: VMware vCenter/ESXi sunucusunda çalışan VMware VM'leri geri yüklemek için Azure Yedekleme Sunucusu'nı (MABS) kullanın.
ms.topic: conceptual
ms.date: 08/18/2019
ms.openlocfilehash: ab2fb4f8f79fa5a664f5cb0ba1bb537c1df658c2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77212345"
---
# <a name="restore-vmware-virtual-machines"></a>VMware sanal makinelerini geri yükleme

Bu makalede, VMware VM kurtarma noktalarını geri yüklemek için Microsoft Azure Yedekleme Sunucusu'nun (MABS) nasıl kullanılacağı açıklanmaktadır. Verileri kurtarmak için MABS'yi kullanma hakkında genel bilgi [için](https://docs.microsoft.com/azure/backup/backup-azure-alternate-dpm-server)bkz. MABS Yönetici Konsolunda kurtarılabilir verileri bulmanın iki yolu vardır : arama yapın veya göz atın. Verileri kurtarırken, verileri veya VM'yi aynı konuma geri yükleyebilir veya geri yüklemek istemeyebilirsiniz. Bu nedenle, MABS VMware VM yedeklemeleri için üç kurtarma seçeneğini destekler:

* **Orijinal konum kurtarma (OLR)** - Korumalı bir VM'yi orijinal konumuna geri yüklemek için OLR kullanın. Bir VM'yi yalnızca yedekleme oluştuğundan beri disk eklenmediyse veya silinmemişse orijinal konumuna geri yükleyebilirsiniz. Diskler eklendiyse veya silindiyse, alternatif konum kurtarma yı kullanmanız gerekir.

* **Alternatif konum kurtarma (ALR)** - Orijinal VM eksikolduğunda veya orijinal VM'yi rahatsız etmek istemiyorsanız, VM'yi alternatif bir konuma kurtarın. Bir VM'yi alternatif bir konuma kurtarmak için, bir ESXi ana bilgisayarının, kaynak havuzunun, klasörün ve depolama veri deposunun ve yolunun konumunu sağlamanız gerekir. Geri yüklenen VM'yi orijinal VM'den ayırt etmeye yardımcı olmak için, MABS VM'nin adına "-Kurtarıldı" ekler.

* **Tek tek dosya konumu kurtarma (ILR)** - Korumalı VM bir Windows Server VM ise, VM içindeki tek tek dosyalar/klasörler MABS'nin ILR özelliği kullanılarak kurtarılabilir. Tek tek dosyaları kurtarmak için, bu makalenin ilerleyen saatlerinde yordamı görün.

## <a name="restore-a-recovery-point"></a>Kurtarma noktasını geri yükleme

1. MABS Yönetici Konsolunda Kurtarma görünümünü tıklatın.

2. Kurtarmak istediğiniz VM'yi bulmak için Gözat bölmesini, göz atın veya filtreleyin. Bir VM veya klasör seçtikten sonra, bölme için Kurtarma noktaları kullanılabilir kurtarma noktalarını görüntüler.

    ![Kullanılabilir kurtarma noktaları](./media/restore-azure-backup-server-vmware/recovery-points.png)

3. Alan **için Kurtarma noktalarında,** kurtarma noktasının alındığı tarihi seçmek için takvim ve açılır menüleri kullanın. Kalın takvim tarihleri kullanılabilir kurtarma noktaları var.

4. Araç şeridinde Kurtarma **Sihirbazı'nı**açmak için **Kurtar'ı** tıklatın.

    ![Kurtarma Sihirbazı, Gözden Kurtarma Seçimi](./media/restore-azure-backup-server-vmware/recovery-wizard.png)

5. **Kurtarma Seçeneklerini Belirt** ekranına ilerlemek için **İleri'yi** tıklatın.

6. Kurtarma **Seçeneklerini Belirt** ekranında, ağ bant genişliği azaltmayı etkinleştirmek istiyorsanız **Değiştir'i**tıklatın. Ağ azaltmayı devre dışı bırakmak için **İleri'yi**tıklatın. Bu sihirbaz ekranında VMware VM'ler için başka seçenek yok. Ağ bant genişliği gazını değiştirmeyi seçerseniz, Throttle iletişim kutusunda, açmak için **ağ bant genişliği kullanımını azaltmayı etkinleştir'i** seçin. Etkinleştirildikten sonra **Ayarlar** ve **Çalışma Çizelgesi'ni**yapılandırın.

7. Kurtarma **Türü Seç** ekranında, özgün örneğe mi yoksa yeni bir konuma mı kurtarılacak larını seçin ve **İleri'yi**tıklatın.

     * **Özgün örneğine Kurtar'ı**seçerseniz, sihirbazda daha fazla seçim yapmanız gerekmez. Özgün örneğin verileri kullanılır.

     * **Herhangi bir ana bilgisayarda sanal makine olarak Kurtar'ı**seçerseniz, **Hedef'i Belirt** ekranında **ESXi Ana Bilgisayar, Kaynak Havuzu, Klasör** ve **Yol**bilgilerini sağlayın.

      ![Kurtarma Türünü Seçin](./media/restore-azure-backup-server-vmware/recovery-type.png)

8. **Özet** ekranında, ayarlarınızı gözden geçirin ve kurtarma işlemini başlatmak için **Kurtar'ı** tıklatın. **Kurtarma durumu** ekranı kurtarma işleminin ilerlemesini gösterir.

## <a name="restore-an-individual-file-from-a-vm"></a>Tek bir dosyayı VM'den geri yükleme

Tek tek dosyaları korumalı bir VM kurtarma noktasından geri yükleyebilirsiniz. Bu özellik yalnızca Windows Server VM'leri için kullanılabilir. Tek tek dosyaları geri getirmek, kurtarma işlemini başlatmadan önce VMDK'ya göz atmanız ve istediğiniz dosyayı bulmanız dışında tüm VM'yi geri almaya benzer. Tek bir dosyayı kurtarmak veya Windows Server VM'den dosyaları seçmek için:

>[!NOTE]
>Tek bir dosyayı VM'den geri geri, yalnızca Windows VM ve Disk Kurtarma Noktaları için kullanılabilir.

1. MABS Yönetici Konsolunda **Kurtarma** görünümünü tıklatın.

2. Kurtarmak istediğiniz VM'yi bulmak için **Gözat** bölmesini, göz atın veya filtreleyin. Bir VM veya klasör seçtikten sonra, bölme için Kurtarma noktaları kullanılabilir kurtarma noktalarını görüntüler.

    ![Kullanılabilir kurtarma noktaları](./media/restore-azure-backup-server-vmware/vmware-rp-disk.png)

3. Için **Kurtarma Noktaları:** bölmede, istenen kurtarma noktası(lar) içeren tarihi seçmek için takvimi kullanın. Yedekleme ilkesinin nasıl yapılandırıldığına bağlı olarak, tarihlerbirden fazla kurtarma noktasına sahip olabilir. Kurtarma noktasının alındığı günü seçtikten sonra, doğru **Kurtarma süresini**seçtiğinizden emin olun. Seçili tarihte birden çok kurtarma noktası varsa, Kurtarma süresi açılır menüsünde seçerek kurtarma noktanızı seçin. Kurtarma noktasını seçtikten sonra, kurtarılabilir öğelerin listesi **Yol:bölmede** görünür.

4. Kurtarmak istediğiniz dosyaları bulmak **için, Yol** bölmesinde, açmak için **Kurtarılabilir öğe** sütunundaki öğeyi çift tıklatın. Kurtarmak istediğiniz dosyayı, dosyaları veya klasörleri seçin. Birden çok öğe seçmek için, her öğeyi seçerken **Ctrl** tuşuna basın. **Kurtarılabilir Öğe** sütununda görünen dosya veya klasörlerin listesini aramak için **Yol** bölmesini kullanın. **Aşağıdaki arama listesi** alt klasörlerde arama yapmaz. Alt klasörlerde arama yapmak için klasörü çift tıklatın. Alt klasörden üst klasöre geçmek için **Yukarı** düğmesini kullanın. Birden çok öğe (dosya ve klasör) seçebilirsiniz, ancak aynı üst klasörde olmalıdır. Aynı kurtarma işinde birden çok klasörden öğeleri kurtaramazsınız.

    ![Kurtarma Seçimini Gözden Geçirin](./media/restore-azure-backup-server-vmware/vmware-rp-disk-ilr-2.png)

5. Kurtarma için öğe(ler) seçtiyseniz, Yönetici Konsolu araç şeridinde Kurtarma **Sihirbazı'nı**açmak için **Kurtar'ı** tıklatın. Kurtarma Sihirbazı'nda, **Gözden Geçirme Kurtarma Seçimi** ekranı kurtarılacak seçili öğeleri gösterir.

6. Kurtarma **Seçeneklerini Belirt** ekranında, ağ bant genişliği azaltmayı etkinleştirmek istiyorsanız **Değiştir'i**tıklatın. Ağ azaltmayı devre dışı bırakmak için **İleri'yi**tıklatın. Bu sihirbaz ekranında VMware VM'ler için başka seçenek yok. Ağ bant genişliği gazını değiştirmeyi seçerseniz, Throttle iletişim kutusunda, açmak için **ağ bant genişliği kullanımını azaltmayı etkinleştir'i** seçin. Etkinleştirildikten sonra **Ayarlar** ve **Çalışma Çizelgesi'ni**yapılandırın.
7. Kurtarma **Türünü Seç** ekranında **İleri'yi**tıklatın. Dosyanızı veya klasörünüzü yalnızca bir ağ klasörüne kurtarabilirsiniz.
8. Hedef **Belirt** ekranında, dosya veya klasörleriniz için ağ konumunu bulmak için **Gözat'ı** tıklatın. MABS, kurtarılan tüm öğelerin kopyalandığı bir klasör oluşturur. Klasör adı, MABS_day-ay-yıl öneki vardır. Kurtarılan dosya veya klasör için bir konum seçtiğinizde, o konumun ayrıntıları (Hedef, Hedef yolu ve kullanılabilir alan) sağlanır.

    ![Dosyaları kurtarmak için konumu belirtin](./media/restore-azure-backup-server-vmware/specify-destination.png)

9. Kurtarma **Seçeneklerini Belirt** ekranında, hangi güvenlik ayarını uygulayacağınızı seçin. Ağ bant genişliği kullanımını azaltmayı değiştirmeyi tercih edebilirsiniz, ancak azaltma varsayılan olarak devre dışı bırakılır. Ayrıca, **SAN Kurtarma** ve **Bildirim** etkin değildir.
10. **Özet** ekranında, ayarlarınızı gözden geçirin ve kurtarma işlemini başlatmak için **Kurtar'ı** tıklatın. **Kurtarma durumu** ekranı kurtarma işleminin ilerlemesini gösterir.

## <a name="next-steps"></a>Sonraki adımlar

Azure Yedekleme Sunucusu'nu kullanırken sorun giderme sorunları [için Azure Yedekleme Sunucusu için sorun giderme kılavuzunu](./backup-azure-mabs-troubleshoot.md)inceleyin.
