---
title: IBM zD&T v1'de Uygulama Geliştiricileri Kontrollü Dağıtım (ADCD) ayarlama | Microsoft Dokümanlar
description: Azure Sanal Makinelerde (VM) IBM Z Geliştirme ve Test Ortamı (zD&T) ortamı nı çalıştırın.
services: virtual-machines-linux
ms.service: virtual-machines-linux
documentationcenter: ''
author: njray
manager: edprice
ms.author: edprice
editor: edprice
ms.topic: conceptual
ms.date: 02/22/2019
tags: ''
keywords: ''
ms.openlocfilehash: 66f80c79219090c27da37dfc1d9149df5604961f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68841381"
---
# <a name="set-up-an-application-developers-controlled-distribution-adcd-in-ibm-zdt-v1"></a>IBM zD&T v1'de Uygulama Geliştiricileri Kontrollü Dağıtım (ADCD) kurma

Azure Sanal Makinelerde (VM) IBM Z Geliştirme ve Test Ortamı (zD&T) ortamı çalıştırabilirsiniz. Bu ortam IBM Z Serisi mimarisini taklit eder. IBM Uygulama Geliştiricileri Kontrollü Dağıtımlar (ADCD'ler) adı verilen özelleştirilmiş paketler aracılığıyla kullanıma sunulan çeşitli Z Serisi işletim sistemlerine veya kurulumlarına (Z Örnekleri veya Paketleri olarak da adlandırılır) ev sahipliği yapabilir.

Bu makalede, Azure'da zD&T ortamında bir ADCD örneğinin nasıl ayarlayabileceğiniz gösterilmektedir. ADCD'ler, zD&T'de çalışan geliştirme ve test ortamları için eksiksiz Z Serisi işletim sistemi uygulamaları oluşturur.

ZD&T gibi, ADCD'ler de yalnızca IBM müşterileri ve iş ortakları tarafından kullanılabilir ve yalnızca geliştirme ve test amaçlıdır. Bunlar üretim ortamları için kullanılmamalıdır. [Passport Advantage](https://www.ibm.com/support/knowledgecenter/en/SSTQBD_12.0.0/com.ibm.zsys.rdt.guide.adcd.doc/topics/installation_ps.html) veya [IBM PartnerWorld](https://www.ibm.com/partnerworld/public)üzerinden çok sayıda IBM yükleme paketi indirilebilir.

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

- [ZD&T ortamı][ibm-install-z] daha önce Azure'da ayarlanmış. Bu makalede, daha önce oluşturulan aynı Ubuntu 16.04 VM görüntü kullanıyorsanız varsayar.

- IBM PartnerWorld veya Passport Advantage aracılığıyla ADCD ortamlarına erişim.

- Bir [lisans sunucusu.](https://www.ibm.com/support/knowledgecenter/en/SSTQBD_12.0.0/com.ibm.zsys.rdt.tools.user.guide.doc/topics/zdt_ee.html) Bu IBM zD&T çalıştırmak için gereklidir. Yazılımı oluşturma şekliniz, yazılımı IBM'den nasıl lisansettiğinize bağlıdır:

  - **Donanım tabanlı lisanslama sunucusu,** yazılımın tüm bölümlerine erişmek için gerekli Olan Rasyonel Belirteçleri içeren bir USB donanım aygıtı gerektirir. Bunu IBM'den almanız gerekir.

  - **Yazılım tabanlı lisanslama sunucusu,** lisans anahtarlarının yönetimi için merkezi bir sunucu ayarlamanızı gerektirir. Bu yöntem tercih edilir ve ibm'den aldığınız anahtarları yönetim sunucusunda ayarlamanızı gerektirir.

## <a name="download-the-installation-packages-from-passport-advantage"></a>Yükleme paketlerini Passport Advantage'tan indirin

ADCD ortamına erişim gereklidir. Aşağıdaki adımlar, ibm müşterisi olduğunuzu ve Passport Advantage'ı kullanabileceğinizi varsayar. IBM iş ortakları [IBM PartnerWorld'u](https://www.ibm.com/partnerworld/public)kullanabilir.

> [!NOTE]
> Bu makalede, Bir Windows PC'nin Azure portalına erişmek ve IBM medyasını indirmek için kullanıldığı varsayar. Mac veya Ubuntu masaüstü kullanıyorsanız, IBM ortamını elde etmek için komutlar ve işlem biraz farklı olabilir.

1. [Passport Advantage'a](https://www.ibm.com/software/howtobuy/passportadvantage/paocustomer)giriş yapın.

2. **Yazılım İndirme ve** Medya **Erişimi'ni**seçin.

3. **Program teklifi ve anlaşma numarasını**seçin ve Devam **et'i**tıklatın.

4. Parça açıklamasını veya parça numarasını girin ve **Bulucu'yu**tıklatın.

5. İsteğe bağlı olarak, ürünü ada göre görüntülemek ve görüntülemek için alfabetik sıra listesini tıklatın.

6. **İşletim sistemi alanındaki** **Tüm İşletim Sistemleri'ni** ve Diller **alanındaki** **Tüm Dilleri** seçin. Ardından, **Git'i**tıklatın.

7. Listeyi genişletmek ve karşıdan yüklenecek tek tek medyayı görüntülemek için **dosyaları seç'i** tıklatın.

8. İndirmek istediğiniz paketi doğrulayın, **İndir'i**seçin ve ardından dosyaları istediğiniz dizine indirin.

## <a name="upload-the-adcd-packages"></a>ADCD paketini yükleyin(ler)

Artık paket(ler) varsa, bunları Azure'daki VM'nize yüklemeniz gerekir.

1. Azure portalında, oluşturduğunuz Ubuntu VM ile **bir ssh** oturumu başlatın. VM'nize gidin, **Genel Bakış** bıçağını seçin ve ardından **Bağlan'ı**seçin.

2. **SSH** sekmesini seçin ve ardından ssh komutunu panoya kopyalayın.

3. Kimlik bilgilerinizi ve seçtiğiniz [SSH istemcisini](/azure/virtual-machines/linux/use-remote-desktop) kullanarak VM'nizde oturum açın. Bu demo, Windows komut istemine bir bash kabuğu ekleyen Windows 10 için Linux uzantılarını kullanır. PuTTY de aynı şekilde çalışıyor.

4. Oturum açtığınızda, IBM paketlerini yüklemek için bir dizin oluşturun. Linux'un duruma duyarlı olduğunu unutmayın. Örneğin, bu demo paketlerin aşağıdakilere yüklendiğini varsayar:

        /home/MyUserID/ZDT/adcd/nov2017/volumes

5. [WinSCP](https://winscp.net/eng/index.php)gibi bir SSH istemcisi kullanarak dosyaları yükleyin. SCP SSH'in bir parçası olduğundan, SSH'in kullandığı bağlantı noktası 22'yi kullanır. Yerel bilgisayarınız Windows değilse, SSH oturumunuza [scp komutunu](http://man7.org/linux/man-pages/man1/scp.1.html) yazabilirsiniz.

6. ZD&T için görüntü depolama alanı haline gelen yüklemeyi oluşturduğunuz Azure VM dizinine başlatın.

    > [!NOTE]
    > Emin olun KI **ADCDTOOLS. XML,** **eve/MyUserID/ZDT/adcd/nov2017** dizinine yüklenir. Buna daha sonra ihtiyacınız olacak.

7. Azure'a bağlantınıza bağlı olarak biraz zaman alabilecek dosyaların yüklenmesini bekleyin.

8. Yüklemeler tamamlandığında, birim dizinine gidin ve tüm **gz** birimlerinin sıkıştırMasını azlayın:

    ```
        gunzip \*.gz
    ```
    
![Sıkıştırılmış gz birimlerini gösteren dosya gezgini](media/01-gunzip.png)

## <a name="configure-the-image-storage"></a>Görüntü depolamasını yapılandırın

Bir sonraki adım yüklenen paketi kullanmak için zD&T yapılandırmaktır. zD&T içindeki görüntü depolama işlemi görüntüleri monte etmenizi ve kullanmanıza olanak tanır. SSH veya FTP kullanabilir.

1. **zDTServer'ı**başlatın. Bunu yapmak için, kök düzeyinde olmalıdır. Sırayla aşağıdaki iki komutu girin:
    ```
        sudo su -
        /opt/ibm/zDT/bin/startServer
    ```
2. URL çıktısını komuta not edin ve web sunucusuna erişmek için bu URL'yi kullanın. Benzer görünüyor:
     > https://(VM adınız veya IP adresiniz):9443/ZDTMC/index.html
     >
     > Unutmayın, web erişiminiz 9443 bağlantı noktasını kullanır. Web sunucusunda oturum açmak için bunu kullanın. ZD&T kullanıcı kimliği **zdtadmin** ve şifre **şifredir.**

    ![IBM zD&T Enterprise Edition Hoş Geldiniz ekranı](media/02-welcome.png)

3. Hızlı **Başlangıç** sayfasında, **Yapıla'nın** **altında, Resim Depolama'yı**seçin.

     ![IBM zD&T Enterprise Edition Hızlı Başlangıç ekranı](media/03-quickstart.png)

4. Görsel **depolamayı Yapılandır** sayfasında **SSH Dosya Aktarım Protokolü'nü**seçin.

5. **Ana Bilgisayar adı için** **Localhost** yazın ve görüntüleri yüklediğiniz yerin dizin yolunu girin. Örneğin, /home/MyUserID/ZDT/adcd/nov2017/volumes.

6. VM için **Kullanıcı Kimliği** ve **parolagirin.** ZD&T kullanıcı kimliği ve parolasını kullanmayın.

7. Erişiminiz olduğundan emin olmak için bağlantıyı sınayın ve yapılandırmayı kaydetmek için **Kaydet'i** seçin.

## <a name="configure-the-target-environments"></a>Hedef ortamları yapılandırma

Bir sonraki adım zD&T hedef ortamı yapılandırmaktır. Bu taklit barındırılan ortam, görüntülerinizin çalıştığı yerdir.

1. Hızlı **Başlangıç** sayfasında, **Yapılandırma**altında **Hedef ortamlarını**seçin.

2. Hedef **ortamları yapılandır** sayfasında **Hedef Ekle'yi**seçin.

3. **Linux'u**seçin. IBM iki tür ortamı destekler, Linux ve Cloud (OpenStack), ancak bu demo Linux üzerinde çalışır.

4. Hedef **ortam ekle** sayfasında, **Ana Bilgisayar adı**için **localhost**girin. **SSH bağlantı noktasını** **22**olarak ayarlayın.

5. Hedef **Çevre etiket** kutusuna **MyCICS** gibi bir etiket girin.

     ![Hedef ortam ekranı ekleme](media/04-add-target.png)

## <a name="configure-adcd-and-deploy"></a>ADCD'yi yapılandırın ve dağıtın

Önceki yapılandırma adımlarını tamamladıktan sonra, paket(ler) ve hedef ortamını kullanmak için zD&T'yi yapılandırmanız gerekir. Yine, görüntüleri takmanızı ve kullanmanıza olanak tanıyan zD&T'deki görüntü depolama işlemini kullanırsınız. SSH veya FTP kullanabilir.

1. Hızlı **Başlangıç** sayfasında, **Yapılandırma**altında **ADCD'yi**seçin. Bir ADCD paketinin monte edilemeden önce tamamlanması gereken adımları belirten bir dizi yönerge görüntülenir. Bu, hedef dizine neden daha önce yaptığımız gibi isim verdiğimizi açıklar.

2. Tüm görüntülerin doğru dizinlere yüklendiğini varsayarsak, sağ alttaki **ADCD bağlantısından GÖRÜNTÜ'ye** tıklayın (aşağıdaki ekran görüntüsünde 7. adımda gösterilmiştir).

     ![IBM zD&T Enterprise Edition - ADCD ekranlarını yapılandır](media/05-adcd.png)

## <a name="create-the-image"></a>Görüntü oluşturma

Önceki yapılandırma adımı tamamlandığında, **ADCD Bileşenleri** sayfasını kullanarak görüntü oluştur görüntülenir.

1. Bu birimdeki farklı paketleri görüntülemek için birimi (Bu durumda Kasım 2017) seçin.

2. Bu demo için **Müşteri Bilgi Kontrol Sistemi (CICS) - 5.3'ü**seçin.

3. Resim **adı** kutusuna, **MyCICS Image**gibi görüntü için bir ad yazın.

4. Sağ alttaki **Resim Oluştur** düğmesini seçin.

     ![IBM zD&T Enterprise Edition - ADCD Components ekranLarını kullanarak görüntü oluşturma](media/06-adcd.png)

5. Görüntünün başarıyla dağıtıldığını belirten görünen **pencerede, görüntüleri dağıt'ı**seçin.

6. Görüntüyü **hedef ortam sayfasına** dağıt'ta, önceki sayfada oluşturduğunuz resmi **(MyCICS Image)** ve daha önce oluşturulan hedef ortamı **(MyCICS)** seçin.

7. Bir sonraki ekranda, VM için kimlik bilgilerinizi girin (yani ztadmin kimlik bilgisi değil).

8. Özellikler bölmesine, Merkezi işlemci **(CP)** sayısını, **Sistem belleği (GB)** miktarını ve çalışan görüntünün **dağıtım dizinini** girin. Bu bir demo olduğundan, küçük tutun.

9. **Dağıtmadan sonra Otomatik olarak IPL komutunu z/OS'ye vermek için**kutunun seçildiğinden emin olun.

     ![Özellikler ekranı](media/07-properties.png)

10. **Tamam'ı**seçin.

11. **Görüntüyü Hedef ortam sayfasına dağıt'tan** Görüntü Dağıt'ı'nı seçin. **Deploy Image**

Görüntünüz artık dağıtılabilir ve 3270 terminal emülatörü tarafından monte edilmeye hazırdır.

> [!NOTE]
> Yeterli disk alanınız olmadığını belirten bir hata alırsanız, bölgenin 151 Gb gerektirdiğini unutmayın.

Tebrikler! Artık Azure'da bir IBM ana bilgisayar ortamı çalıştırıyorsunuz.

## <a name="learn-more"></a>Daha fazla bilgi edinin

- [Ana bilgisayar göçü: mitler ve gerçekler](https://docs.microsoft.com/azure/architecture/cloud-adoption/infrastructure/mainframe-migration/myths-and-facts)
- [Azure'da IBM DB2 pureScale](https://docs.microsoft.com/azure/virtual-machines/linux/ibm-db2-purescale-azure)
- [Sorun giderme](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/)
- [Azure geçişiiçin ana bilgisayarın mymystifying](https://azure.microsoft.com/resources/demystifying-mainframe-to-azure-migration/)

<!-- INTERNAL LINKS -->
[microfocus-get-started]: /microfocus/get-started.md
[microfocus-setup]: /microfocus/set-up-micro-focus-on-azure.md
[microfocus-demo]: /microfocus/demo.md
[ibm-get-started]: /ibm/get-started.md
[ibm-install-z]: install-ibm-z-environment.md
[ibm-demo]: /ibm/demo.md
