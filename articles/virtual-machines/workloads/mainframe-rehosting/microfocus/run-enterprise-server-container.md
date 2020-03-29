---
title: Micro Focus Enterprise Server 4.0'ı Azure Sanal Makinelerde Docker Konteynerinde Çalıştırın
description: Micro Focus Enterprise Server'ı Azure Sanal Makineler'deki Docker konteynerinde çalıştırarak IBM z/OS ana bilgisayar yüklerinizi yeniden barındırın.
services: virtual-machines-linux
documentationcenter: ''
author: njray
ms.author: sread
ms.date: 04/02/2019
ms.topic: article
ms.service: multiple
ms.openlocfilehash: 30d99c3f4767eb50361f7074c0d508fcf309faca
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "61488485"
---
# <a name="run-micro-focus-enterprise-server-40-in-a-docker-container-on-azure"></a>Micro Focus Enterprise Server 4.0'ı Azure'daki Docker Konteynerinde Çalıştırın

Micro Focus Enterprise Server 4.0'ı Azure'daki docker konteynerinde çalıştırabilirsiniz. Bu öğretici nasıl gösterir. Enterprise Server için Windows CICS (Müşteri Bilgi Kontrol Sistemi) acctdemo gösterisini kullanır.

Docker uygulamalara taşınabilirlik ve yalıtım ekler. Örneğin, docker görüntüsünü bir Windows VM'den başka bir windows vm'de çalıştırmak için veya docker ile bir Windows sunucusuna aktarabilirsiniz. Docker görüntüsü, Enterprise Server'ı yüklemek zorunda kalmadan yeni konumda aynı yapılandırmayla çalışır. Görüntünün bir parçası. Lisansla ilgili hususlar hala geçerlidir.

Bu öğretici, Azure Marketi'nden **Windows 2016 Veri Merkezi'ni Kapsayıcılar** sanal makinesiyle (VM) yükler. Bu VM **Docker 18.09.0**içerir. Aşağıdaki adımlar, kapsayıcıyı nasıl dağıtabileceğinizi, çalıştırabileceğinizi ve 3270 emülatörüyle nasıl bağlanabileceğinizi gösterir.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce, şu ön koşullara göz atın:

- Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

- Micro Focus yazılımı ve geçerli bir lisans (veya deneme lisansı). Mevcut bir Micro Focus müşterisiyseniz, Micro Focus temsilcinize başvurun. Aksi takdirde, [bir deneme isteyin.](https://www.microfocus.com/products/enterprise-suite/enterprise-server/trial/)

     > [!NOTE]
     > Docker tanıtım dosyaları Enterprise Server 4.0'a dahildir. Bu öğretici\_ent\_server\_dockerfiles\_4.0 windows.zip kullanır. Enterprise Server yükleme dosyasına eriştin veya başlamak için *Micro Focus'a* gidin.

- [Enterprise Server ve Enterprise Developer](https://www.microfocus.com/documentation/enterprise-developer/#")için belgeler .

## <a name="create-a-vm"></a>VM oluşturma

1. Ent\_sunucusu\_dockerfiles\_4.0\_windows.zip dosyasından medya güvenli. ES-Docker-Prod-XXXXXXXX.mflic lisans dosyası (Docker görüntüleri oluşturmak için gerekli) güvenli.

2. VM'yi oluşturun. Bunu yapmak için Azure portalını açın, sol üstten **kaynak oluştur'u** seçin ve *windows sunucusuna*göre filtre uygulayın. Sonuçlarda, **Kapsayıcılar ile Windows Server 2016 Datacenter 'ı**seçin.

     ![Azure portalı arama sonuçları](media/01-portal.png)

3. VM özelliklerini yapılandırmak için örnek ayrıntıları seçin:

    1. Bir VM boyutu seçin. Bu öğretici için, 2 vCPUs ve 7 GB bellek içeren **standart\_ds2 v2** VM kullanmayı düşünün.

    2. Dağıtmak istediğiniz **Bölge** ve **Kaynak Grubu'nu** seçin.

    3. **Kullanılabilirlik seçenekleri**için varsayılan ayarı kullanın.

    4. **Kullanıcı adı**için, kullanmak istediğiniz yönetici hesabını ve parolayı yazın.

    5. Bağlantı **noktası 3389 RDP'nin** açık olduğundan emin olun. VM'de oturum açabilmeniz için yalnızca bu bağlantı noktasının herkese açık olması gerekir. Ardından tüm varsayılan değerleri kabul edin ve **Gözden Geçir+ oluştur'u**tıklatın.

     ![Sanal makine bölmesi oluşturma](media/container-02.png)

4. Dağıtımın bitmesini bekleyin (birkaç dakika). İleti, VM'nizin oluşturulduğunu belirtir.

5. VM'iniz için **Genel Bakış** bıçağına gitmek için **Kaynağa Git'i** tıklatın.

6. Sağdaki **Bağlan** düğmesini tıklatın. **Sanal makineye Bağlan** seçenekleri sağda görünür.

7. VM'ye eklemenize olanak tanıyan RDP dosyasını indirmek için **RDP Dosyasını İndir** düğmesini tıklatın.

8. Dosya indirmeyi bitirdikten sonra, dosyayı açın ve VM için oluşturduğunuz kullanıcı adı ve parolayı yazın.

     > [!NOTE]
     > Oturum etmek için şirket kimlik bilgilerinizi kullanmayın. (RDP istemcisi bunları kullanmak isteyebileceğini varsayar. Yok.)

9.  **Daha Fazla Seçenek'i**seçin ve ardından VM kimlik bilgilerinizi seçin.

Bu noktada, VM çalışıyor ve RDP ile bağlı. İmzalandın ve bir sonraki adım için hazırsın.

## <a name="create-a-sandbox-directory-and-upload-the-zip-file"></a>Bir sandbox dizini oluşturun ve zip dosyasını yükleyin

1.  Demo ve lisans dosyalarını yükleyebileceğiniz VM'de bir dizin oluşturun. Örneğin, **C:\\Sandbox**.

2.  Oluşturduğunuz dizine **ent\_\_server dockerfiles\_4.0\_windows.zip** ve **ES-Docker-Prod-XXXXXXXX.mflic** dosyasını yükleyin.

3.  Zip dosyasının içeriğini, ayıklama işlemi tarafından oluşturulan **ent\_server\_dockerfiles\_4.0\_windows** dizinine ayıklayın. Bu dizin bir okuma dosyası (.html ve .txt dosyası olarak) ve iki alt dizin, **EnterpriseServer** ve **Örnekler**içerir.

4.  **KOPYA ES-Docker-Prod-XXXXXXXX.mflic** C:\\Sandbox\\\_kayıt\_sunucusu\_dockerfiles\_\\4.0 windows\\EnterpriseServer\\ve\_\_C:\_Sandbox kayıt sunucusu dockerfiles 4.0\_windows\\Örnekler\\CICS dizinleri.

> [!NOTE]
> Lisans dosyasını her iki diziniçin de kopyaladığınızdan emin olun. Görüntülerin düzgün bir şekilde lisanslı olduğundan emin olmak için Docker yapı adımı için gereklidirler.

## <a name="check-docker-version-and-create-base-image"></a>Docker sürümünü denetleyin ve temel görüntü oluşturun

> [!IMPORTANT]
> Uygun Docker görüntüsünü oluşturmak iki aşamalı bir işlemdir. İlk olarak, Enterprise Server 4.0 temel görüntüsünü oluşturun.Sonra x64 platformu için başka bir görüntü oluşturun. X86 (32 bit) görüntü oluşturabiliyor sanız da, 64 bit lik görüntüyü kullanın.

1. Bir komut istemi açın.

2. Docker'ın yüklü olup olmadığını kontrol edin. Komut istemine şunları yazın:

    ```
        docker version
    ```

     Örneğin, sürüm 18.09.0 bu yazıldığında oldu.

3. Dizini değiştirmek için **cd \Sandbox\ent_server_dockerfiles_4.0_windows\EnterpriseServer**yazın.

4. İlk temel görüntü için yapı işlemini başlatmak için **bld.bat IacceptEULA** yazın. Bu işlemin çalışması için birkaç dakika bekleyin. Sonuçlarda, biri x64, diğeri x86 için olmak üzere oluşturulan iki görüntüye dikkat edin:

     ![Görüntüleri gösteren komut penceresi](media/container-04.png)

5. CICS gösterimi için son görüntüyü oluşturmak **için,\\cd Sandbox\\kayıt\_sunucusu\_dockerfiles\_4.0\\\\\_windows Örnekler CICS**yazarak CICS dizinine geçin.

6. Görüntüyü oluşturmak için **bld.bat x64**yazın. İşlemin çalışması ve görüntünün oluşturulduğunu belirten ileti için birkaç dakika bekleyin.

7. VM'de yüklü olan tüm Docker görüntülerinin listesini görüntülemek için **docker görüntüleri** yazın. **Microfocus/es-acctdemo** bunlardan biri olduğundan emin olun.

     ![es-acctdemo görüntüsünü gösteren komut penceresi](media/container-05.png)

## <a name="run-the-image"></a>Görüntüyü çalıştırma 

1.  Enterprise Server 4.0 ve acctdemo uygulamasını komut istemi türünden başlatmak için:

    ```
         docker run -p 16002:86/tcp -p 16002:86/udp -p 9040-9050:9040-9050 -p 9000-9010:9000-9010 -ti --network="nat" --rm microfocus/es-acctdemo:win_4.0_x64
    ```

2.  [X3270](http://x3270.bgp.nu/) gibi bir 3270 terminal emülatörü yükleyin ve çalışan görüntüye 9040 portu üzerinden takmak için kullanın.

3.  Acctdemo kapsayıcısının IP adresini alın, böylece Docker yönettiği kapsayıcılar için BIR DHCP sunucusu olarak hareket edebilir:

    1.  Çalışan konteynerin kimliğini alın. Komut isteminde **Docker ps** yazın ve bu örnekte ID **(22a0fe3159d0)** not edin. Bir sonraki adıma sakla.

    2.  Acctdemo kapsayıcısının IP adresini almak için, önceki adımdaki konteyner kimliğini aşağıdaki gibi kullanın:

    ```
       docker inspect <containerID> --format="{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}"
    ```

       Örnek:

    ```   
        docker inspect 22a0fe3159d0 --format="{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}"
    ```
4. Acctdemo resminin IP adresine dikkat edin. Örneğin, aşağıdaki çıktıdaki adres 172.19.202.52'dir.

     ![IP adresini gösteren komut penceresi](media/container-06.png)

5. Emülatör kullanarak görüntüyü monte edin. Acctdemo görüntü ve port 9040 adresini kullanmak için emülatör yapılandırın. Burada, **172.19.202.52:9040**. Seninki de benzer olacak. **CICS ekranına Signon** açılır.

    ![CICS ekranında oturum açma](media/container-07.png)

6. **Password**için **USERID** ve **SYSAD** için **SYSAD'ı** girerek CICS Bölgesi'ne giriş girin.

7. Emülatörün anahtar haritasını kullanarak ekranı temizleyin. x3270 için **Keymap** menüsü seçeneğini seçin.

8. Acctdemo uygulamasını başlatmak için **ACCT**yazın. Uygulamanın başlangıç ekranı görüntülenir.

     ![Hesap Demo ekranı](media/container-08.png)

9. Görüntü hesabı türleri ile denemeler. Örneğin, İstek için **D** ve **HESAP**için **11111** yazın. Deneyecek diğer hesap numaraları 22222, 33333 ve benzeridir.

     ![Hesap Demo ekranı](media/container-09.png)

10. Enterprise Server Administration konsolunu görüntülemek için komut istemine gidin ve başlangıç yazın **http:172.19.202.52:86**

     ![Kurumsal Sunucu Yönetimi konsolu](media/container-010.png)

İşte bu kadar! Şimdi bir Docker konteynerinde cics uygulamasını çalıştırıyor ve yönetiyorsunuz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure'da Micro Focus Enterprise Server 4.0 ve Enterprise Developer 4.0'ı yükleyin](./set-up-micro-focus-azure.md)
- [Ana bilgisayar uygulaması geçişi](/azure/architecture/cloud-adoption/infrastructure/mainframe-migration/application-strategies)
