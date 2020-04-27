---
title: Azure sanal makinelerinde bir Docker kapsayıcısında Micro Focus Enterprise Server 4,0 çalıştırma
description: Azure sanal makinelerinde bir Docker kapsayıcısında mikro odak kurumsal sunucusunu çalıştırarak IBM z/OS ana iş yüklerinizi yeniden barındırın.
services: virtual-machines-linux
documentationcenter: ''
author: njray
ms.author: sread
ms.date: 04/02/2019
ms.topic: article
ms.service: multiple
ms.openlocfilehash: 30d99c3f4767eb50361f7074c0d508fcf309faca
ms.sourcegitcommit: f7fb9e7867798f46c80fe052b5ee73b9151b0e0b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2020
ms.locfileid: "61488485"
---
# <a name="run-micro-focus-enterprise-server-40-in-a-docker-container-on-azure"></a>Azure 'daki bir Docker kapsayıcısında mikro odak kurumsal sunucu 4,0 ' i çalıştırın

Micro Focus Enterprise Server 4,0 ' i Azure 'daki bir Docker kapsayıcısında çalıştırabilirsiniz. Bu öğreticide nasıl yapılacağı gösterilmektedir. Enterprise Server için Windows CICS (müşteri bilgileri denetim sistemi) acctdemo gösterisini kullanır.

Docker, uygulamalara taşınabilirlik ve yalıtım ekler. Örneğin, bir Windows VM 'den bir Docker görüntüsünü başka bir şekilde veya bir depodan Docker ile bir Windows Server 'a aktarabilirsiniz. Docker görüntüsü, kurumsal sunucu yüklemesi gerekmeden aynı yapılandırmaya sahip yeni konumda çalışır. Bu, görüntünün bir parçasıdır. Lisanslama konuları hala geçerlidir.

Bu öğreticide, Azure Marketi 'nden kapsayıcılar sanal makinesi (VM) **Ile Windows 2016 Datacenter** yüklenir. Bu VM, **Docker 18.09.0**içerir. Aşağıdaki adımlarda, kapsayıcıyı dağıtma, çalıştırma ve sonra da bir 3270 öykünücüsü ile bağlanma işlemlerinin nasıl yapılacağı gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce Şu önkoşullara göz atın:

- Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

- Mikro odak yazılımı ve geçerli bir lisans (veya deneme lisansı). Mevcut bir Micro Focus müşterisiyseniz, mikro odak temsilcinizle iletişime geçin. Aksi takdirde, [bir deneme isteyin](https://www.microfocus.com/products/enterprise-suite/enterprise-server/trial/).

     > [!NOTE]
     > Docker tanıtım dosyaları Enterprise Server 4,0 ' ye dahildir. Bu öğretici, ENT\_Server\_dockerfile 'ları destekliyor\_4,0\_Windows. zip ' i kullanır. Bu, kurumsal sunucu yükleme dosyasına eriştiğiniz yerden erişin veya başlamak için *Micro Focus* ' a gidin.

- [Kurumsal sunucu ve Kurumsal Geliştirici](https://www.microfocus.com/documentation/enterprise-developer/#")belgeleri.

## <a name="create-a-vm"></a>VM oluşturma

1. Medyayı ENT\_Server\_dockerfile 'ları destekliyor\_4,0\_Windows. zip dosyasından güvenli hale getirin. ES-Docker-prod-XXXXXXXX. mflik lisanslama dosyasının güvenliğini sağlayın (Docker görüntülerini derlemek için gereklidir).

2. VM 'yi oluşturun. Bunu yapmak için, Azure portal açın, sol üstteki **kaynak oluştur** ' u seçin ve *Windows Server*'a göre filtreleyin. Sonuçlarda **Windows Server 2016 Datacenter – kapsayıcılar**' ı seçin.

     ![Azure portal arama sonuçları](media/01-portal.png)

3. VM 'nin özelliklerini yapılandırmak için örnek ayrıntıları ' nı seçin:

    1. Bir VM boyutu seçin. Bu öğreticide, 2 vCPU ve 7 GB bellek içeren **Standart BIR DS2\_v2** VM kullanmayı düşünün.

    2. Dağıtmak istediğiniz **bölgeyi** ve **kaynak grubunu** seçin.

    3. **Kullanılabilirlik seçenekleri**için varsayılan ayarı kullanın.

    4. **Kullanıcı adı**için, kullanmak istediğiniz yönetici hesabını ve parolayı yazın.

    5. **3389 RDP bağlantı noktasının** açık olduğundan emin olun. VM 'de oturum açabilmeniz için yalnızca bu bağlantı noktasının herkese açık olması gerekir. Ardından tüm varsayılan değerleri kabul edin ve **gözden geçir + oluştur**' a tıklayın.

     ![Sanal makine bölmesi oluştur](media/container-02.png)

4. Dağıtımın bitmesini bekleyin (birkaç dakika). Bir ileti, sanal makinenizin oluşturulduğunu belirtir.

5. VM 'nizin **genel bakış** dikey penceresine gitmek Için **Kaynağa Git** ' e tıklayın.

6. Sağ tarafta **Bağlan** düğmesine tıklayın. **Sanal makineye bağlan** seçenekleri sağda görünür.

7. VM 'ye eklemenize olanak tanıyan RDP dosyasını indirmek için **RDP dosyasını indir** düğmesine tıklayın.

8. Dosya indirmeyi tamamladıktan sonra, açın ve VM için oluşturduğunuz Kullanıcı adını ve parolayı yazın.

     > [!NOTE]
     > Oturum açmak için şirket kimlik bilgilerinizi kullanmayın. (RDP istemcisi bunları kullanmak isteyebileceğiniz varsayılır. Değilsiniz.)

9.  **Diğer seçimler**' ı seçin, ardından VM kimlik bilgilerinizi seçin.

Bu noktada, VM çalışır ve RDP aracılığıyla iliştirilir. Oturumunuz açıldı ve bir sonraki adım için hazırsınız.

## <a name="create-a-sandbox-directory-and-upload-the-zip-file"></a>Korumalı alan dizini oluşturma ve ZIP dosyasını karşıya yükleme

1.  VM üzerinde tanıtım ve lisans dosyalarını karşıya yükleyebileceğiniz bir dizin oluşturun. Örneğin, **C:\\Sandbox**.

2.  **ENT\_Server\_dockerfile 'ları destekliyor\_4,0\_Windows. ZIP** ve **es-Docker-prod-xxxxxxxx. mflik** dosyasını oluşturduğunuz dizine yükleyin.

3.  ZIP dosyasının içeriğini, ayıklama işlemi tarafından oluşturulan **ENT\_Server\_dockerfile 'ları destekliyor\_4,0\_Windows** dizinine ayıklayın. Bu dizin bir Benioku dosyası (. html ve. txt dosyası) ve iki alt dizin, **Enterpriseserver** ve **örnekleri**içerir.

4.  **Es-Docker-prod-xxxxxxxx. mflik** \\'i C:\\Sandbox ENT\_Server\_dockerfile 'ları destekliyor\_4,0\_Windows\\enterpriseserver ve C:\\Sandbox\\ENT\_Server\_dockerfile 'ları destekliyor\_4,0\_Windows\\örnekleri\\CICS dizinleri olarak kopyalayın.

> [!NOTE]
> Lisanslama dosyasını her iki dizine da kopyalamadığınızdan emin olun. Görüntülerin doğru lisanslanmış olduğundan emin olmak için Docker derleme adımının olması gerekir.

## <a name="check-docker-version-and-create-base-image"></a>Docker sürümünü denetleyin ve temel görüntü oluşturun

> [!IMPORTANT]
> Uygun Docker görüntüsünü oluşturmak, iki adımlı bir işlemdir. İlk olarak, Enterprise Server 4,0 temel görüntüsünü oluşturun.Ardından x64 platformu için başka bir görüntü oluşturun. Bir x86 (32-bit) görüntüsü oluşturabilseniz de 64 bit görüntüsünü kullanın.

1. Bir komut istemi açın.

2. Docker 'ın yüklü olduğundan emin olun. Komut istemine şunları yazın:

    ```
        docker version
    ```

     Örneğin, bu yazıldığında sürüm 18.09.0 idi.

3. Dizini değiştirmek için **CD \Sandbox\ ent_server_dockerfiles_4.0_windows \EnterpriseServer**yazın.

4. İlk temel görüntü için derleme işlemini başlatmak üzere **Bld. bat IAcceptEula** yazın. Bu işlemin çalışması için birkaç dakika bekleyin. Sonuçlarda, biri x64 ve diğeri x86 için oluşturulmuş iki resme dikkat edin:

     ![Görüntüleri gösterme Komut penceresi](media/container-04.png)

5. CICS demo 'in son görüntüsünü oluşturmak için, **CD\\korumalı\\\_sunucu\_dockerfile 'ları destekliyor\_4,0\_Windows\\örnekleri\\CICS**yazarak CICS dizinine geçin.

6. Görüntüyü oluşturmak için, **Bld. bat x64**yazın. İşlemin çalışması için birkaç dakika bekleyin ve görüntünün oluşturulduğunu belirten ileti.

7. VM 'de yüklü olan tüm Docker görüntülerinin listesini göstermek için **Docker görüntülerini** yazın. **Mikro Focus/es-acctdemo** 'in bunlardan biri olduğundan emin olun.

     ![Es-acctdemo görüntüsünü gösteren Komut penceresi](media/container-05.png)

## <a name="run-the-image"></a>Görüntüyü çalıştırma 

1.  Enterprise Server 4,0 ve acctdemo uygulamasını başlatmak için komut isteminde şunu yazın:

    ```
         docker run -p 16002:86/tcp -p 16002:86/udp -p 9040-9050:9040-9050 -p 9000-9010:9000-9010 -ti --network="nat" --rm microfocus/es-acctdemo:win_4.0_x64
    ```

2.  [X3270](http://x3270.bgp.nu/) gibi bir 3270 Terminal öykünücüsü yükleyip, bağlantı noktası 9040 üzerinden, çalıştıran görüntüye eklemek için kullanın.

3.  Acctdemo kapsayıcısının IP adresini alın. Docker, yönettiği kapsayıcılar için bir DHCP sunucusu işlevi görebilir:

    1.  Çalışan kapsayıcının KIMLIĞINI alın. Komut istemine **Docker PS** YAZıN ve kimliği (Bu örnekte**22a0fe3159d0** ) göz önünde bulun. Bir sonraki adım için kaydedin.

    2.  Acctdemo kapsayıcısının IP adresini almak için önceki adımdaki kapsayıcı KIMLIĞINI aşağıdaki gibi kullanın:

    ```
       docker inspect <containerID> --format="{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}"
    ```

       Örneğin:

    ```   
        docker inspect 22a0fe3159d0 --format="{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}"
    ```
4. Acctdemo görüntüsünün IP adresini aklınızda tut. Örneğin, aşağıdaki çıktıda bulunan adres 172.19.202.52 ' dir.

     ![IP adresini gösteren Komut penceresi](media/container-06.png)

5. Öykünücüyü kullanarak görüntüyü bağlayın. Öykünücüyü, acctdemo görüntüsünün adresini ve 9040 numaralı bağlantı noktasını kullanacak şekilde yapılandırın. İşte bu **172.19.202.52:9040**. Sizinki benzer olacaktır. **CICS-CICS** Screen ekranında açılır.

    ![CICS Screen 'e oturum açma](media/container-07.png)

6. **Kullanıcı kimliği** Için **Sysad** ve **parola**için **sysad** girerek CICS bölgesinde oturum açın.

7. Öykünücünün keymap 'ini kullanarak ekranı temizleyin. X3270 için, **keymap** menü seçeneğini belirleyin.

8. Acctdemo uygulamasını başlatmak için **ACCT**yazın. Uygulama için ilk ekran görüntülenir.

     ![Hesap tanıtımı ekranı](media/container-08.png)

9. Görüntüleme hesabı türleriyle denemeler yapın. Örneğin, Istek için **D** , **Hesap**için **11111** yazın. Deneyebileceğiniz diğer hesap numaraları 22222, 33333 vb...

     ![Hesap tanıtımı ekranı](media/container-09.png)

10. Enterprise Server Yönetim konsolunu göstermek için komut istemine gidin ve **Başlat http: 172.19.202.52:86** yazın.

     ![Enterprise Server Yönetim Konsolu](media/container-010.png)

İşte bu kadar! Artık bir Docker kapsayıcısında bir CICS uygulaması çalıştırıyorsunuz ve yönetiyorsunuz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure 'da Micro Focus Enterprise Server 4,0 ve Enterprise Developer 4,0 'yi yükler](./set-up-micro-focus-azure.md)
- [Ana bilgisayar uygulaması geçişi](/azure/architecture/cloud-adoption/infrastructure/mainframe-migration/application-strategies)
