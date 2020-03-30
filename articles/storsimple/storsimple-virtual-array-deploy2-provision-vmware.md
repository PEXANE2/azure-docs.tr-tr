---
title: VMware Hüküm StorSimple Sanal Dizi
description: StorSimple Virtual Array dağıtım serisindeki bu ikinci öğretici, VMware'de sanal bir cihaz sağlanmasını içerir.
author: alkohli
ms.assetid: 0425b2a9-d36f-433d-8131-ee0cacef95f8
ms.service: storsimple
ms.topic: conceptual
ms.date: 07/25/2019
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5f5ded3faec3a080022eea70de2cca5d27529c4d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76272088"
---
# <a name="deploy-storsimple-virtual-array---provision-in-vmware"></a>StorSimple Virtual Array dağıtın - VMware'de Hüküm
![](./media/storsimple-virtual-array-deploy2-provision-vmware/vmware4.png)

## <a name="overview"></a>Genel Bakış

[!INCLUDE [storsimple-virtual-array-eol-banner](../../includes/storsimple-virtual-array-eol-banner.md)]

Bu öğretici, VMware ESXi 5.0, 5.5, 6.0 veya 6.5 çalıştıran bir ana bilgisayar sisteminde ki StorSimple Virtual Array'in nasıl sağlanıp bağlanışla düzenesin ve bağlanıştırılabildiğini açıklar. Bu makale, Azure portalında StorSimple Sanal Diziler ve Microsoft Azure Devlet Bulutu'nda dağıtım için geçerlidir.

Sanal cihaz sağlamak ve bağlantı kurmak için yönetici ayrıcalıklarına sahip olmanız gerekir. Sağlama ve ilk kurulum adımlarını tamamlamak yaklaşık 10 dakika sürecektir.

## <a name="provisioning-prerequisites"></a>Ön koşulların sağlanması
VMware ESXi 5.0, 5.5, 6.0 veya 6.5 çalıştıran bir ana bilgisayar sisteminde sanal bir cihaz sağlamanın ön koşulları aşağıdaki gibidir.

### <a name="for-the-storsimple-device-manager-service"></a>StorSimple Cihaz Yöneticisi hizmeti için
Başlamadan önce aşağıdakilerden emin olun:

* [StorSimple Virtual Array için portalı hazırlayın](storsimple-virtual-array-deploy1-portal-prep.md)tüm adımları tamamladınız.
* VMware için sanal cihaz görüntüsünü Azure portalından indirdiniz. Daha fazla bilgi için, **Bkz. Adım 3:** [StorSimple Virtual Array kılavuzu için portalı hazırlayın](storsimple-virtual-array-deploy1-portal-prep.md)sanal cihaz görüntüsünü indirin.

### <a name="for-the-storsimple-virtual-device"></a>StorSimple sanal cihazı için
Sanal cihazı dağıtmadan önce şunlardan emin olun:

* Hyper-V (2008 R2 veya sonraki) çalıştıran bir ana bilgisayar sistemine erişiminiz vardır.
* Ana bilgisayar sistemi sanal cihazınızı sağlamak için aşağıdaki kaynakları ayırabiliyor:

  * En az 4 çekirdek.
  * En az 8 GB RAM. Sanal diziyi dosya sunucusu olarak yapılandırmayı planlıyorsanız, 8 GB 2 milyondan az dosyayı destekler. 2 - 4 milyon dosyayı desteklemek için 16 GB RAM gerekir.
  * Bir ağ arabirimi.
  * Sistem verileri için 500 GB sanal disk.

### <a name="for-the-network-in-datacenter"></a>Veri merkezindeki ağ için
Başlamadan önce aşağıdakilerden emin olun:

* Bir StorSimple sanal aygıtı dağıtmak için ağ gereksinimlerini gözden geçirdiniz ve veri merkezi ağını gereksinimlere göre yapılandırdınız. 

## <a name="step-by-step-provisioning"></a>Adım adım sağlama
Sanal bir aygıt sağlamak ve bağlanmak için aşağıdaki adımları gerçekleştirmeniz gerekir:

1. Ana bilgisayar sisteminin minimum sanal cihaz gereksinimlerini karşılamak için yeterli kaynağa sahip olduğundan emin olun.
2. Hipervizörünüzde sanal bir cihaz sağlama.
3. Sanal cihazı çalıştırın ve IP adresini alın.

## <a name="step-1-ensure-host-system-meets-minimum-virtual-device-requirements"></a>Adım 1: Ana bilgisayar sisteminin minimum sanal cihaz gereksinimlerini karşıladığından emin olun
Sanal bir aygıt oluşturmak için şunları yapmanız gerekir:

* VMware ESXi Server 5.0, 5.5, 6.0 veya 6.5 çalıştıran bir ana bilgisayar sistemine erişim.
* ESXi ana bilgisayarını yönetmek için sisteminizde VMware vSphere istemcisi yüklü.

  * En az 4 çekirdek.
  * En az 8 GB RAM. Sanal diziyi dosya sunucusu olarak yapılandırmayı planlıyorsanız, 8 GB 2 milyondan az dosyayı destekler. 2 - 4 milyon dosyayı desteklemek için 16 GB RAM gerekir.
  * İnternet trafiği için ağa bağlı bir ağ arabirimi. Cihazın en iyi şekilde çalışabilmesi için minimum Internet bant genişliği 5 Mbps olmalıdır.
  * Veri için 500 GB sanal disk.

## <a name="step-2-provision-a-virtual-device-in-hypervisor"></a>Adım 2: Hipervizörde sanal bir cihaz sağlama
Hiper yöneticinizde sanal cihaz sağlamak için aşağıdaki adımları gerçekleştirin.

1. Sanal cihaz görüntüsünü sisteminize kopyalayın. Bu sanal görüntüyü Azure portalı ndan indirdiniz.

   1. En son resim dosyasını indirdiğinizden emin olun. Görüntüyü daha önce indirdiyseniz, en son resme sahip olduğunuzdan emin olmak için yeniden indirin. En son görüntü (bir yerine) iki dosya vardır.
   2. Bu görüntüyü yordamın ilerleyen bölümlerinde kullanacağınız için kopyaladığınız konumu not edin.

2. vSphere istemcisini kullanarak ESXi sunucusunda oturum açın. Sanal makine oluşturmak için yönetici ayrıcalıklarınızın olması gerekir.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image1.png)
3. vSphere istemcisinde, sol bölmedeki stok bölümünde ESXi Server'ı seçin.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image2.png)
4. VMDK dosyasını ESXi sunucusuna yükleyin. Sağ bölmedeki **Yapılandırma** sekmesine gidin. **Donanım**altında, **Depolama'yı**seçin.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image3.png)
5. Sağ taraftaki bölmede **Datastores** (Veri depoları) öğesini seçerek VMDK dosyasını yüklemek istediğiniz yeri belirleyin. Veri deposu işletim sistemi ve veri diskleri için yeterli boş alana sahip olmalıdır.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image4.png)
6. Sağ tıklayıp **Browse Datastore** (Veri Deposuna Göz At) öğesini seçin.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image5.png)
7. **Datastore Browser** (Veri Deposu Tarayıcısı) penceresi açılır.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image6.png)
8. Araç çubuğunda, ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image7.png) yeni bir klasör oluşturmak için simgeyi tıklatın. Klasör adını belirtin ve not edin. Sanal makine oluştururken bu klasör adını kullanacaksınız (önerilen yöntemdir). **Tamam**'a tıklayın.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image8.png)
9. Yeni klasör **Datastore Browser** (Veri Deposu Tarayıcısı) penceresinin sol tarafında görünür.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image9.png)
10. Yükle simgesine ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image10.png) tıklayın ve **Dosyayükle'yi**seçin.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image11.png)
11. İndirdiğiniz VMDK dosyalarını bulun. İki dosya vardır. Karşıya yüklemek için dosyalardan birini seçin.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image12m.png)
12. **Aç**'a tıklayın. VMDK dosyası belirtilen veri deposuna yüklenmeye başlar. Dosyanın karşıya yüklenmesi birkaç dakika sürebilir.
13. Karşıya yükleme işlemi tamamlandıktan sonra dosyayı oluşturduğunuz veri deposunda görebilirsiniz.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image14.png)

    Şimdi ikinci VMDK dosyasını da aynı ver deposuna yükleyin.
14. vSphere istemcisi penceresine dönün. ESXi sunucusu seçildiğinde, sağ tıklayın ve **Yeni Sanal Makine'yi**seçin.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image15.png)
15. **Yeni Sanal Makine Oluştur** penceresi görüntülenir. **Yapılandırma** sayfasında **Özel** seçeneğini belirleyin. **İleri**'ye tıklayın.
    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image16.png)
16. Ad **ve Konum** sayfasında, sanal makinenizin adını belirtin. Bu ad, Adım 8'de daha önce belirttiğiniz klasör adı (önerilen en iyi uygulama) ile eşleşmelidir.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image17.png)
17. **Depolama** sayfasında, VM'nizi sağlamak için kullanmak istediğiniz bir veri deposu seçin.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image18.png)
18. Sanal **Makine Sürümü** sayfasında, **Sanal Makine Sürümü seçin: 8**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image19.png)
19. Konuk **İşletim Sistemi** sayfasında, **Konuk İşletim Sistemi'ni** **Windows**olarak seçin. Açılan listeden **Sürüm**için **Microsoft Windows Server 2012 (64 bit)** seçeneğini belirleyin.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image20.png)
20. **CPU'lar** sayfasında, **toplam çekirdek sayısının** 4 (veya daha fazla) olması için sanal yuva başına **çekirdek sayısını** ve **çekirdek sayısını** ayarlayın. **İleri**'ye tıklayın.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image21.png)
21. **Bellek** sayfasında 8 GB (veya daha fazla) RAM belirtin. **İleri**'ye tıklayın.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image22.png)
22. **Ağ** sayfasında, ağ arabirimlerinin sayısını belirtin. Minimum gereksinim bir ağ arabirimidir.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image23.png)
23. **SCSI Denetleyicisi** sayfasında varsayılan **LSI Logic SAS denetleyicisini kabul edin.**

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image24.png)
24. Disk **seç** sayfasında **varolan bir sanal disk kullan'ı**seçin. **İleri**'ye tıklayın.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image25.png)
25. **Varolan Disk** seç sayfasında, **Disk Dosya Yolu** **altında, Gözat'ı**tıklatın. Bu, **Veri Depoları'na Gözat** iletişim kutusunu açar. VMDK'yı yüklediğiniz konuma gidin. Şimdi, başlangıçta yüklediğiniz iki dosya birleştirildikçe veri deposunda yalnızca bir dosya görürsünüz. Dosyayı seçin ve **Tamam'ı**tıklatın. **İleri**'ye tıklayın.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image26.png)
26. Gelişmiş **Seçenekler** sayfasında varsayılanı kabul edin ve **İleri'yi**tıklatın.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image27.png)
27. **Ready to Complete** (Tamamlanmak İçin Hazır) sayfasında yeni sanal makineyle ilgili tüm ayarları gözden geçirin. **Tamamlanmadan önce sanal makine ayarlarını denetle'nin denetle'sini.** **Devam**’a tıklayın.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image28.png)
28. Sanal **Makineler Özellikleri** sayfasında, **Donanım** sekmesinde aygıt donanımını bulun. **Yeni Sabit Disk'i**seçin. **Ekle**’ye tıklayın.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image29.png)
29. **Donanım Ekle** penceresi görürsünüz. Aygıt **Türü** sayfasında, **eklemek istediğiniz aygıt türünü seçin**, Sabit **Disk'i**seçin ve **İleri'yi**tıklatın.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image30.png)
30. Disk **seç** sayfasında yeni **bir sanal disk oluştur'u**seçin. **İleri**'ye tıklayın.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image31.png)
31. Disk **Oluştur** sayfasında, Disk **Boyutunu** 500 GB (veya daha fazla) olarak değiştirin. 500 GB minimum gereksinim olsa da, her zaman daha büyük bir disk sağlayabilir. Sağlanan diski genişletemeyeceğinizveya küçültemeyeceğiniz dikkat edin. Diskin boyutu hakkında daha fazla bilgi için, en [iyi uygulamalar belgesinde](storsimple-ova-best-practices.md)boyutlandırma bölümünü gözden geçirin. **Disk Sağlama**altında, **İnce Hüküm**seçin. **İleri**'ye tıklayın.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image32.png)
32. Gelişmiş **Seçenekler** sayfasında varsayılanı kabul edin.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image33.png)
33. **Tamamlanına Hazır** sayfasında, disk seçeneklerini gözden geçirin. **Son**'a tıklayın.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image34.png)
34. Sanal Makine Özellikleri sayfasına dönün. Sanal makinenize yeni bir sabit disk eklenir. **Son**'a tıklayın.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image35.png)
35. Sağ bölmede seçilen sanal makineniz ile **Özet** sekmesine gidin. Sanal makinenizin ayarlarını gözden geçirin.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image36.png)

Sanal makineniz sağlanır. Bir sonraki adım bu makineyi açmak ve IP adresini almaktır.

> [!NOTE]
> Sanal dizinize VMware araçları yüklememenizi öneririz (yukarıda belirtildiği gibi). VMware araçlarının yüklenmesi desteklenmeyen bir yapılandırmaya neden olabilir.

## <a name="step-3-start-the-virtual-device-and-get-the-ip"></a>Adım 3: Sanal cihazı başlatın ve IP'yi alın
Sanal cihazınızı başlatmak ve bağlantı kurmak için aşağıdaki adımları izleyin.

#### <a name="to-start-the-virtual-device"></a>Sanal cihazı başlatmak için
1. Sanal cihazı başlatın. vSphere Configuration Manager'da, sol bölmede cihazınızı seçin ve bağlam menüsünü açmak için sağ tıklayın. **Power** (Güç) ve ardından **Power on** (Aç) seçimini yapın. Bu işlemin ardından makinenizin açılması gerekir. vSphere istemcisinin alt takim son **görevler** bölmesinde durumu görüntüleyebilirsiniz.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image37.png)
2. Kurulum görevlerinin tamamlanması birkaç dakika sürer. Aygıt çalışmaya başladıktan sonra **Konsol** sekmesine gidin. Alternatif olarak, imleci konsol penceresinden doğrultabilir ve Ctrl+Alt+Insert tuşuna basabilirsiniz. Varsayılan kullanıcı *StorSimpleAdmin* ve varsayılan şifre *Password1*olduğunu.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image38.png)
3. Güvenlik nedeniyle cihazın yönetici parolasının ilk oturum açma işleminin ardından değiştirilmesi gerekir. Parolayı değiştirmeniz istenir.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image39.png)
4. En az 8 karakterden oluşan bir parola girin. Parola, bu gereksinimlerin 4'te 3'ü içermelidir: büyük harf, küçük harf, sayısal ve özel karakterler. Onaylamak için parolayı yeniden girin. Parolanın değiştiği size bildirilir.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image40.png)
5. Parola başarıyla değiştirildikten sonra, sanal aygıt yeniden başlatılabilir. Yeniden başlatmanın tamamlanmasını bekleyin. Aygıtın Windows PowerShell konsolu bir ilerleme çubuğuyla birlikte görüntülenebilir.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image41.png)
6. Adım 6-8 yalnızca DHCP bulunmayan bir ortamdaki önyükleme süreci için geçerlidir. DHCP ortamındaysanız bu adımları atlayıp 9. adımla devam edebilirsiniz. Cihazınızı DHCP olmayan bir ortamda başlattıysanız, aşağıdaki ekranı görürsünüz.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image42m.png)

   Ardından, ağı yapılandırın.
7. Sanal `Get-HcsIpAddress` cihazınızda etkinleştirilen ağ arabirimlerini listelemek için komutu kullanın. Cihazınızda tek bir ağ arabirimi varsa `Ethernet` varsayılan adı atanır.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image43m.png)
8. Ağı yapılandırmak için `Set-HcsIpAddress` cmdlet'ini kullanın. Aşağıda bir örnek gösterilmiştir:

    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image44.png)
9. İlk kurulum işlemleri tamamlandıktan ve cihaz önyüklendikten sonra cihaz başlık metnini görürsünüz. Cihazı yönetmek için başlık metninde görüntülenen IP adresini ve URL'yi not edin. Bu IP adresini sanal cihazınızın web arama sitesine bağlanmak ve yerel kurulum ve kaydı tamamlamak için kullanacaksınız.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image45.png)
10. (İsteğe bağlı) Bu adımı yalnızca Cihazınızı Devlet Bulutu'nda dağıtıyorsanız gerçekleştirin. Artık cihazınızda Abd Federal Bilgi İşlem Standardı (FIPS) modunu etkinleştireceksiniz. FIPS 140 standardı, hassas verilerin korunması için ABD Federal hükümet bilgisayar sistemleri tarafından kullanılmak üzere onaylanan şifreleme algoritmalarını tanımlar.

    1. FIPS modunu etkinleştirmek için aşağıdaki cmdlet'i çalıştırın:

        `Enable-HcsFIPSMode`
    2. Fips modunu etkinleştirdikten sonra cihazınızı yeniden başlatın, böylece şifreleme doğrulamaları etkili olur.

       > [!NOTE]
       > Aygıtınızda FIPS modunu etkinleştirebilir veya devre dışı kullanabilirsiniz. Aygıtı FIPS ve FIPS olmayan mod arasında alternatif olarak değiştirin desteklenmez.
       >
       >

Cihazınız minimum yapılandırma gereksinimlerini karşılamıyorsa başlık metninde hata iletisi görüntülenir (aşağıda gösterilmiştir). Cihaz yapılandırmasını minimum gereksinimleri karşılayacak şekilde değiştirmeniz gerekir. Ardından cihazı yeniden başlatıp bağlantı kurabilirsiniz. Minimum yapılandırma gereksinimleri için bkz. [1. Adım: Ana bilgisayarın minimum cihaz gereksinimlerini karşıladığından emin olma](#step-1-ensure-host-system-meets-minimum-virtual-device-requirements).

![](./media/storsimple-virtual-array-deploy2-provision-vmware/image46.png)

Yerel web Web Web Web Web'i kullanarak ilk yapılandırma sırasında başka bir hatayla karşılaşıyorsanız, aşağıdaki iş akışlarına bakın:

* [Web UI kurulumunun giderilen sorun gidermek](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors)için tanılama testlerini çalıştırın.
* [Günlük paketi oluşturun ve günlük dosyalarını görüntüleyin.](storsimple-ova-web-ui-admin.md#generate-a-log-package)

## <a name="next-steps"></a>Sonraki adımlar
* [StorSimple Virtual Array'inizi dosya sunucusu olarak ayarlama](storsimple-virtual-array-deploy3-fs-setup.md)
* [StorSimple Virtual Array'inizi iSCSI sunucusu olarak ayarlama](storsimple-virtual-array-deploy3-iscsi-setup.md)
