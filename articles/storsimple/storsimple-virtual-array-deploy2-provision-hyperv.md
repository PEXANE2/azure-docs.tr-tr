---
title: Hyper-V ' d a StorSimple Sanal dizisi sağlama | Microsoft Docs
description: StorSimple Sanal dizisi dağıtımının bu ikinci öğreticide, Hyper-V ' d a bir sanal dizi sağlanması gerekir.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: 4354963c-e09d-41ac-9c8b-f21abeae9913
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/25/2019
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d6dfc95820e911781ffa88e2207601703f165839
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87070595"
---
# <a name="deploy-storsimple-virtual-array---provision-in-hyper-v"></a>StorSimple Sanal dizisi dağıtma-Hyper-V içinde sağlama
![Bir sanal diziyi dağıtmak için gereken adımları gösteren diyagram. İkinci adımın ilk bölümü, Hyper-V üzerinde sağlama olarak etiketlenir ve vurgulanır.](./media/storsimple-virtual-array-deploy2-provision-hyperv/hyperv4.png)

## <a name="overview"></a>Genel Bakış

[!INCLUDE [storsimple-virtual-array-eol-banner](../../includes/storsimple-virtual-array-eol-banner.md)]

Bu öğreticide, Windows Server 2012 R2, Windows Server 2012 veya Windows Server 2008 R2 üzerinde Hyper-V çalıştıran bir konak sisteminde StorSimple Sanal dizisinin nasıl sağlanacağı açıklanmaktadır. Bu makale, Azure portal ve Microsoft Azure Kamu bulutta StorSimple Sanal dizilerinin dağıtımına yöneliktir.

Bir sanal dizi sağlamak ve yapılandırmak için yönetici ayrıcalıklarına sahip olmanız gerekir. Sağlama ve ilk kurulum adımlarını tamamlamak yaklaşık 10 dakika sürecektir.

## <a name="provisioning-prerequisites"></a>Sağlama önkoşulları
Burada, Windows Server 2012 R2, Windows Server 2012 veya Windows Server 2008 R2 üzerinde Hyper-V çalıştıran bir konak sisteminde bir sanal dizi sağlamaya yönelik önkoşulları bulacaksınız.

### <a name="for-the-storsimple-device-manager-service"></a>StorSimple Cihaz Yöneticisi hizmeti için
Başlamadan önce aşağıdakilerden emin olun:

* [Portal 'ı StorSimple Sanal dizisi Için hazırlama](storsimple-virtual-array-deploy1-portal-prep.md)bölümündeki tüm adımları tamamladınız.
* Hyper-V için sanal dizi görüntüsünü Azure portal indirdiniz. Daha fazla bilgi için bkz. 3. Adım: [Portal 'ı StorSimple Sanal dizisi Için hazırlama adlı](storsimple-virtual-array-deploy1-portal-prep.md) **Sanal dizi görüntüsünü indirme** .

  > [!IMPORTANT]
  > StorSimple Sanal dizisinde çalışan yazılım yalnızca StorSimple Aygıt Yöneticisi hizmeti ile birlikte kullanılabilir.
  >
  >

### <a name="for-the-storsimple-virtual-array"></a>StorSimple Sanal dizisi için
Bir sanal diziyi dağıtmadan önce, aşağıdakileri yaptığınızdan emin olun:

* Windows Server 2008 R2 veya üzeri bir cihaza bir cihaz sağlamak için kullanılabilecek Hyper-V çalıştıran bir konak sistemine erişiminiz var.
* Konak sistemi, sanal dizinizi sağlamak için aşağıdaki kaynakları ayırabiliyor:

  * En az 4 çekirdek.
  * En az 8 GB RAM. Sanal diziyi dosya sunucusu olarak yapılandırmayı planlıyorsanız, 8 GB 'dan daha az 2.000.000 dosya desteklenir. 2-4 milyon dosyayı desteklemek için 16 GB RAM gerekir.
  * Bir ağ arabirimi.
  * Veriler için 500 GB sanal disk.

### <a name="for-the-network-in-the-datacenter"></a>Veri merkezindeki ağ için
Başlamadan önce, bir StorSimple Sanal dizisi dağıtmak ve veri merkezi ağını uygun şekilde yapılandırmak için ağ gereksinimlerini gözden geçirin. Daha fazla bilgi için bkz. [StorSimple Sanal dizisi ağ gereksinimleri](storsimple-ova-system-requirements.md#networking-requirements).

## <a name="step-by-step-provisioning"></a>Adım adım sağlama
Bir sanal diziyi sağlamak ve bu diziye bağlanmak için aşağıdaki adımları gerçekleştirmeniz gerekir:

1. Konak sisteminin en düşük sanal dizi gereksinimlerini karşılamak için yeterli kaynaklara sahip olduğundan emin olun.
2. Hiper yöneticiniz üzerinde sanal bir dizi sağlayın.
3. Sanal diziyi başlatın ve IP adresini alın.

Bu adımların her biri aşağıdaki bölümlerde açıklanmıştır.

## <a name="step-1-ensure-that-the-host-system-meets-minimum-virtual-array-requirements"></a>1. Adım: konak sisteminin en düşük sanal dizi gereksinimlerini karşıladığından emin olun
Bir sanal dizi oluşturmak için şunlar gerekir:

* Windows Server 2012 R2, Windows Server 2012 veya Windows Server 2008 R2 SP1 üzerinde yüklü Hyper-V rolü.
* Ana bilgisayara bağlı Microsoft Windows istemcisine yüklenmiş Microsoft Hyper-V Yöneticisi.

Sanal diziyi oluşturmakta olduğunuz temeldeki donanımın (ana bilgisayar sistemi) aşağıdaki kaynakları sanal diziniz için ayırabildiğinden emin olun:

* En az 4 çekirdek.
* En az 8 GB RAM. Sanal diziyi dosya sunucusu olarak yapılandırmayı planlıyorsanız, 8 GB 'dan daha az 2.000.000 dosya desteklenir. 2-4 milyon dosyayı desteklemek için 16 GB RAM gerekir.
* Bir ağ arabirimi.
* Sistem verileri için 500 GB sanal disk.

## <a name="step-2-provision-a-virtual-array-in-hypervisor"></a>2. Adım: Hiper yöneticide sanal dizi sağlama
Hiper yöneticinizde cihaz sağlamak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-provision-a-virtual-array"></a>Bir sanal dizi sağlamak için
1. Windows Server ana bilgisayarınızda, sanal dizi görüntüsünü bir yerel sürücüye kopyalayın. Bu görüntüyü (VHD veya VHDX) Azure portal aracılığıyla indirdiniz. Bu görüntüyü yordamın ilerleyen bölümlerinde kullanacağınız için kopyaladığınız konumu not edin.
2. **Sunucu Yöneticisi**'ni açın. Sağ üst köşede **Araçlar**'a tıklayın ve **Hyper-V Yöneticisi**'ni seçin.

   ![Hyper-V Yöneticisi öğesi vurgulanmış genişletilmiş bir Araçlar menüsünü gösteren Sunucu Yöneticisi ekran görüntüsü.](./media/storsimple-virtual-array-deploy2-provision-hyperv/image1.png)  

   Windows Server 2008 R2 çalıştırıyorsanız, Hyper-V Yöneticisi 'Ni açın. Sunucu Yöneticisi, Hyper- **v > Hyper-v yöneticisi > roller**' e tıklayın.
3. **Hyper-V Yöneticisi**'nin kapsam bölmesinde sistem düğümünüze sağ tıklayarak bağlam menüsünü açın ve **Yeni** > **Sanal Makine**'ye tıklayın.

   ![Yeni ve sanal makine öğeleriyle vurgulanan bir sistem düğümünün kısayol menüsünü gösteren Hyper-V Yöneticisi 'nin ekran görüntüsü.](./media/storsimple-virtual-array-deploy2-provision-hyperv/image2.png)
4. Yeni Sanal Makine Sihirbazı'nın **Başlamadan önce** sayfasında **İleri**'ye tıklayın.
5. **Ad ve konum belirtin** sayfasında, sanal diziniz Için bir **ad** girin. **İleri**’ye tıklayın.

   ![Adı ve konumu belirtin sayfasının doldurulmuş ad kutusunu ve bir sonraki düğmeyi gösteren ekran görüntüsü.](./media/storsimple-virtual-array-deploy2-provision-hyperv/image4.png)
6. **Üretimi belirle** sayfasında, cihaz görüntüsü türünü seçin ve ardından **İleri**' ye tıklayın. Windows Server 2008 R2 kullanıyorsanız Bu sayfa görünmez.

   * Windows Server 2012 veya üzeri bir. vhdx görüntüsünü indirdiyseniz **2. nesil** seçeneğini belirleyin.
   * Windows Server 2008 R2 veya üzeri için bir. vhd görüntüsü indirdiyseniz **1. kuşak ' i** seçin.

   ![Kuşak 2 seçeneğinin seçili olduğunu ve Ileri düğmesinin vurgulandığını belirten üretimi belirt sayfasının ekran görüntüsü.](./media/storsimple-virtual-array-deploy2-provision-hyperv/image5.png)
7. **Bellek ata** sayfasında **Başlangıç belleği** değerini en az **8192 MB** yapın, dinamik bellek özelliğini etkinleştirmeyin ve **İleri**'ye tıklayın.

   ![Başlangıç belleği kutusunda 8192 değerini gösteren bellek ata sayfasının ekran görüntüsü. Ileri düğmesi vurgulanır.](./media/storsimple-virtual-array-deploy2-provision-hyperv/image6.png)  
8. **Ağı yapılandır** sayfasında İnternete bağlı olan sanal anahtarı belirtin ve **İleri**'ye tıklayın.

   ![İletişim kutusunda seçilen Hyper V sanal anahtar öğesini ve Ileri düğmesini vurgulanmış olan ağ yapılandırma sayfasının ekran görüntüsü.](./media/storsimple-virtual-array-deploy2-provision-hyperv/image7.png)
9. **Sanal sabit diski bağla** sayfasında, **var olan bir sanal sabit diski kullan**' ı seçin, sanal dizi görüntüsünün konumunu (. vhdx veya. vhd) belirtin ve ardından **İleri**' ye tıklayın.

   ![Var olan bir sanal sabit diski kullan seçeneğinin bulunduğu sanal sabit diski bağla sayfasının ekran görüntüsü, girilen bir konum ve bir sonraki düğme vurgulanır.](./media/storsimple-virtual-array-deploy2-provision-hyperv/image8m.png)
10. **Özet** sayfasını gözden geçirin ve **Son**'a tıklayarak sanal makineyi oluşturun.

    ![Bir sanal makinenin açıklamasını gösteren yeni sanal makine Sihirbazı sayfasının ekran görüntüsü. Son düğmesi vurgulanır.](./media/storsimple-virtual-array-deploy2-provision-hyperv/image9.png)
11. Minimum gereksinimleri karşılamak için 4 çekirdeğe ihtiyacınız vardır. 4 sanal işlemci eklemek için **Hyper-V Yöneticisi** penceresinde ana bilgisayar sisteminizi seçin. Sağ tarafta, **Sanal Makineler** listesinin altında bulunan bölmede az önce oluşturduğunuz sanal makineyi bulun. Makine adına sağ tıklayın ve **Ayarlar**'ı seçin.

    ![Hyper-V Yöneticisi penceresinin ekran görüntüsü. Listede bir makine seçilir ve ayarlar öğesi bu makinenin kısayol menüsünde vurgulanır.](./media/storsimple-virtual-array-deploy2-provision-hyperv/image10.png)
12. **Ayarlar** sayfasında sol taraftaki bölmeden **İşlemci**'yi seçin. Sağ taraftaki bölmede **sanal işlemci sayısını** 4 (veya üzeri) olarak ayarlayın. **Uygula**’ya tıklayın.

    ![Işlemci öğesi vurgulanmış olan ayarlar sayfasının ekran görüntüsü, sanal işlemcilerin sayısı dört olarak ayarlanır ve Uygula düğmesi vurgulanır.](./media/storsimple-virtual-array-deploy2-provision-hyperv/image11.png)
13. Minimum gereksinimleri karşılamak için, 500 GB sanal veri diski de eklemeniz gerekir. **Ayarlar** sayfasında:

    1. Sol taraftaki bölmede **SCSI Denetleyicisi**'ni seçin.
    2. Sağ taraftaki bölmede **Sabit Sürücü**'yü seçin ve **Ekle**'ye tıklayın.

    ![Seçilen SCSI denetleyicisi öğesi, seçilen sabit sürücü öğesi ve Ekle düğmesi vurgulanmış şekilde ayarlar sayfasının ekran görüntüsü.](./media/storsimple-virtual-array-deploy2-provision-hyperv/image12.png)
14. **Sabit sürücü** sayfasında **Sanal sabit disk** seçeneğini belirleyin ve **Yeni**'ye tıklayın. **Yeni Sanal Sabit Disk Sihirbazı** açılır.

    ![Sabit sürücü öğesi seçili olan ayarlar sayfasının ekran görüntüsü, sanal sabit disk seçeneği seçilidir ve yeni düğme vurgulanır.](./media/storsimple-virtual-array-deploy2-provision-hyperv/image13.png)
15. Yeni Sanal Sabit Disk Sihirbazı'nın **Başlamadan önce** sayfasında **İleri**'ye tıklayın.
16. **Disk Biçimini Seç** sayfasında varsayılan seçenek olan **VHDX** biçimini kabul edin. **İleri**’ye tıklayın. Windows Server 2008 R2 çalıştırıyorsa bu ekran sunulmaz.

    ![VHDX seçeneği seçili ve sonraki Düğme vurgulanmış şekilde disk biçimi Seç sayfasının ekran görüntüsü.](./media/storsimple-virtual-array-deploy2-provision-hyperv/image15.png)
17. **Disk Türünü Seç** sayfasında sanal sabit disk türünü **Dinamik olarak genişletilen** (önerilen) olarak ayarlayın. **Sabit boyutlu** diski de seçebilirsiniz ancak daha uzun süre beklemeniz gerekebilir. **Fark kayıt** seçeneğini kullanmamanızı öneririz. **İleri**’ye tıklayın. Windows Server 2012 R2 ve Windows Server 2012 ' de **, Windows** Server 2008 R2 ' de varsayılan seçenektir; varsayılan olarak **sabit boyutudur**.

    ![Dinamik olarak genişleyen seçenek seçiliyken ve sonraki Düğme vurgulanmış şekilde disk türü seç sayfasının ekran görüntüsü.](./media/storsimple-virtual-array-deploy2-provision-hyperv/image16.png)
18. **Ad ve Konum Belirtin** sayfasında veri diski için bir **ad** ve **konum** (göz atabilirsiniz) belirtin. **İleri**’ye tıklayın.

    ![Ad ve konum kutularındaki değerleri gösteren adı ve konumu belirtin sayfasının ekran görüntüsü. Ileri düğmesi vurgulanır.](./media/storsimple-virtual-array-deploy2-provision-hyperv/image17.png)
19. **Diski Yapılandır** sayfasında, **Yeni boş bir sanal sabit disk oluştur** seçeneğini BELIRLEYIN ve boyutu **500 GB** (veya daha fazla) olarak belirtin. 500 GB en düşük gereksinimdir, her zaman daha büyük bir disk sağlayabilirsiniz. Sağlandıktan sonra diski genişletemez veya daraltılamayacağını unutmayın. Sağlanacak disk boyutu hakkında daha fazla bilgi için [en iyi uygulamalar belgesindeki](storsimple-ova-best-practices.md)boyutlandırma bölümünü gözden geçirin. **İleri**’ye tıklayın.

    ![Yeni boş bir sanal sabit disk Oluştur seçiliyken disk yapılandır sayfasının ekran görüntüsü, boyut 500 olarak ayarlanır ve sonraki düğme vurgulanır.](./media/storsimple-virtual-array-deploy2-provision-hyperv/image18.png)
20. **Özet** sayfasında sanal veri diskinizin ayrıntılarını gözden geçirin ve her şey yolunda görünüyorsa **Son**'a tıklayarak diski oluşturun. Sihirbaz kapanır ve makinenize bir sanal sabit disk eklenir.

    ![Bir sanal sabit diskin açıklamasını gösteren yeni sanal sabit disk Sihirbazı sayfasının ekran görüntüsü. Son düğmesi vurgulanır.](./media/storsimple-virtual-array-deploy2-provision-hyperv/image19.png)
21. **Ayarlar** sayfasına geri dönün. **Tamam**'a tıklayarak **Ayarlar** sayfasını kapatın ve Hyper-V Yöneticisi penceresine dönün.

    ![Ayarlar sayfasının ekran görüntüsü. Tamam düğmesi vurgulanır.](./media/storsimple-virtual-array-deploy2-provision-hyperv/image20.png)

## <a name="step-3-start-the-virtual-array-and-get-the-ip"></a>3. Adım: sanal diziyi başlatın ve IP 'yi alın
Sanal dizinizi başlatmak ve buna bağlanmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-start-the-virtual-array"></a>Sanal diziyi başlatmak için
1. Sanal diziyi başlatın.

   ![Hyper-V Yöneticisi penceresinin ekran görüntüsü. Yeni dizi listede seçilir ve başlangıç öğesi bu dizinin kısayol menüsünde vurgulanır.](./media/storsimple-virtual-array-deploy2-provision-hyperv/image21.png)
2. Cihaz çalışmaya başladıktan sonra cihazı ve **Bağlan**'ı seçin.

   ![Hyper-V Yöneticisi penceresinin ekran görüntüsü. Yeni dizi seçilir, durumu çalışıyor ve Connect öğesi kısayol menüsünde vurgulanır.](./media/storsimple-virtual-array-deploy2-provision-hyperv/image22.png)
3. Cihazın hazırlanmaya yönelik olarak 5-10 dakika beklemeniz gerekebilir. Konsolda ilerleme durumunu gösteren bir durum iletisi görüntülenir. Cihaz hazır olduktan sonra **Eylem** bölümüne gidin. `Ctrl + Alt + Delete`Sanal dizide oturum açmak için tuşuna basın. Varsayılan Kullanıcı *Storsimpleadmin* ' dir ve varsayılan parola *Parola1*' dir.

   ![Oturum açma kutusunda belirsiz karakterler gösteren vSphere istemci konsolu sekmesinin ekran görüntüsü.](./media/storsimple-virtual-array-deploy2-provision-hyperv/image23.png)
4. Güvenlik nedeniyle cihazın yönetici parolasının ilk oturum açma işleminin ardından değiştirilmesi gerekir. Parolayı değiştirmeniz istenir.

   ![VSphere istemci konsolu sekmesinin ekran görüntüsü. sayfadaki metin parolanın değiştirilmesi gerektiğini belirtir.](./media/storsimple-virtual-array-deploy2-provision-hyperv/image24.png)

   En az 8 karakterden oluşan bir parola girin. Parolanın şu 4 gereksinimden en az 3 tanesini karşılaması gerekir: büyük harf, küçük harf, rakam ve özel karakter. Onaylamak için parolayı yeniden girin. Parolanın değiştirildiği bildirilir.

   ![VSphere istemci konsolu sekmesinin ekran görüntüsü. sayfadaki metin parolanın değiştiğini belirtir.](./media/storsimple-virtual-array-deploy2-provision-hyperv/image25.png)
5. Parola başarıyla değiştirildikten sonra, sanal dizi yeniden başlatılabilir. Cihazın başlatılmasını bekleyin.

   ![Ana StorSimpleAdmin sayfasının ekran görüntüsü. Sayfadaki metin, kullanıcıdan sistem olay bildirimi hizmetini beklemesini ister.](./media/storsimple-virtual-array-deploy2-provision-hyperv/image26.png)

    Cihazın Windows PowerShell konsolu ve ilerleme çubuğu gösterilir.

   ![İlerleme çubuğu içeren bir konsol penceresini gösteren ekran görüntüsü. Penceredeki metin ilk kurulumun devam ettiğinden emin olur ve kullanıcıdan beklemesini ister.](./media/storsimple-virtual-array-deploy2-provision-hyperv/image27.png)
6. Adım 6-8 yalnızca DHCP bulunmayan bir ortamdaki önyükleme süreci için geçerlidir. DHCP ortamındaysanız bu adımları atlayıp 9. adımla devam edebilirsiniz. Cihazınızı DHCP dışı ortamda önyüklerseniz, aşağıdaki ekranı görürsünüz.

   ![Cihazı tanımlayan metin içeren bir konsol penceresini gösteren ekran görüntüsü. Komut istemi "denetleyiciyi" okur ve giriş için hazırlanın.](./media/storsimple-virtual-array-deploy2-provision-hyperv/image28m.png)

    Sonra, ağı yapılandırın.
7. `Get-HcsIpAddress`Sanal diziniz üzerinde etkin olan ağ arabirimlerini listelemek için komutunu kullanın. Cihazınızda tek bir ağ arabirimi varsa `Ethernet` varsayılan adı atanır.

   ![Get-Hcsıpaddress komutunun çıkışıyla bir konsol penceresi gösteren ekran görüntüsü. "Ethernet", cihazın adı olarak listelenir.](./media/storsimple-virtual-array-deploy2-provision-hyperv/image29m.png)
8. Ağı yapılandırmak için `Set-HcsIpAddress` cmdlet'ini kullanın. Aşağıdaki örneğe bakın:

    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

    ![Get-Help set-Hcsıpaddress komutunun çıkışıyla birlikte bir konsol penceresi gösteren ekran görüntüsü ve set-Hcsıpaddress komutunun doğru kullanımı.](./media/storsimple-virtual-array-deploy2-provision-hyperv/image30.png)
9. İlk kurulum işlemleri tamamlandıktan ve cihaz önyüklendikten sonra cihaz başlık metnini görürsünüz. Cihazı yönetmek için başlık metninde görüntülenen IP adresini ve URL'yi not edin. Sanal dizininizdeki Web Kullanıcı arabirimine bağlanmak ve yerel kurulumu ve kaydı gerçekleştirmek için bu IP adresini kullanın.

   ![Cihaz başlık metniyle bir konsol penceresi gösteren ekran görüntüsü. Bu metin, cihaz IP adresini ve URL 'YI içerir.](./media/storsimple-virtual-array-deploy2-provision-hyperv/image31m.png)
10. Seçim Bu adımı yalnızca cihazınızı kamu bulutuna dağıtıyorsanız gerçekleştirin. Artık cihazınızda Federal bilgi Işleme standardı (FIPS) modunu Birleşik Devletler etkinleştirirsiniz. FIPS 140 standardı, önemli verilerin korunması için ABD Federal Kamu bilgisayar sistemleri tarafından kullanılmak üzere onaylanan şifreleme algoritmalarını tanımlar.

    1. FIPS modunu etkinleştirmek için aşağıdaki cmdlet 'i çalıştırın:

        `Enable-HcsFIPSMode`
    2. FIPS modunu etkinleştirdikten sonra şifreleme doğrulamaları geçerli olacak şekilde cihazınızı yeniden başlatın.

       > [!NOTE]
       > Cihazınızda FIPS modunu etkinleştirebilir veya devre dışı bırakabilirsiniz. Cihaz FIPS ve FIPS olmayan mod arasında alternatif olarak desteklenmez.
       >
       >

Cihazınız en düşük yapılandırma gereksinimlerini karşılamıyorsa, Başlık metninde aşağıdaki hatayı görürsünüz (aşağıda gösterilmiştir). Cihaz yapılandırmasını minimum gereksinimleri karşılayacak şekilde değiştirin. Ardından cihazı yeniden başlatıp bağlantı kurabilirsiniz. 1. adımda en düşük yapılandırma gereksinimlerine bakın: konak sisteminin en düşük sanal dizi gereksinimlerini karşıladığından emin olun.

![Cihaz başlık metniyle bir konsol penceresi gösteren ekran görüntüsü. Bu metin, sorunu gidermeye yönelik bir URL sağlayan bir hata iletisi içerir.](./media/storsimple-virtual-array-deploy2-provision-hyperv/image32.png)

Yerel Web Kullanıcı arabirimini kullanarak ilk yapılandırma sırasında başka bir hata varsa, aşağıdaki iş akışlarına başvurun:

* [Web UI kurulumu sorunlarını gidermek](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors)için tanılama testlerini çalıştırın.
* [Günlük paketi oluşturun ve günlük dosyalarını görüntüleyin](storsimple-ova-web-ui-admin.md#generate-a-log-package).

## <a name="next-steps"></a>Sonraki adımlar
* [StorSimple Sanal dizinizi dosya sunucusu olarak ayarlama](storsimple-virtual-array-deploy3-fs-setup.md)
* [StorSimple Sanal dizinizi Iscsı sunucusu olarak ayarlama](storsimple-virtual-array-deploy3-iscsi-setup.md)
