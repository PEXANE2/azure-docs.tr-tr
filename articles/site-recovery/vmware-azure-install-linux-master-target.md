---
title: Azure Site Kurtarma ile Linux VM geri ödeme için ana hedef sunucu yükleme
description: Azure Site Kurtarma'yı kullanarak VMware VM'lerin olağanüstü kurtarma dan Azure'a geri kazanılması sırasında şirket içi bir siteye geri dönüş için bir Linux ana hedef sunucusu nasıl ayarlayayın öğrenin.
author: mayurigupta13
services: site-recovery
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 03/06/2019
ms.author: mayg
ms.openlocfilehash: 5b4d625d28584bb601905e9439c112c845219e54
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73954369"
---
# <a name="install-a-linux-master-target-server-for-failback"></a>Yeniden çalışma için bir Linux ana hedef sunucusu yükleme
Sanal makineleriniz üzerinde Azure'da başarısız olduktan sonra, sanal makineleri şirket içi siteye geri döndürebilirsiniz. Geri başarısız olmak için sanal makineyi Azure'dan şirket içi siteye yeniden korumanız gerekir. Bu işlem için, trafiği almak için şirket içi bir ana hedef sunucuya ihtiyacınız vardır. 

Korumalı sanal makineniz bir Windows sanal makinesiyse, windows ana hedefine ihtiyacınız vardır. Bir Linux sanal makine için, bir Linux ana hedef gerekir. Linux ana hedefi oluşturmayı ve nasıl yükleyeriz öğrenmek için aşağıdaki adımları okuyun.

> [!IMPORTANT]
> 9.10.0 ana hedef sunucusunun piyasaya sürülmesiyle başlayan en son ana hedef sunucu yalnızca Bir Ubuntu 16.04 sunucusuna yüklenebilir. CentOS6.6 sunucularında yeni yüklemelere izin verilmez. Ancak, 9.10.0 sürümünü kullanarak eski ana hedef sunucularınızı yükseltmeye devam edebilirsiniz.
> LVM'deki ana hedef sunucu desteklenmez.

## <a name="overview"></a>Genel Bakış
Bu makalede, bir Linux ana hedefi nasıl yüklenir için talimatlar sağlar.

Bu makalenin sonunda veya Azure Kurtarma [Hizmetleri Forumu'nda](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)yorum veya soru gönderin.

## <a name="prerequisites"></a>Ön koşullar

* Ana hedefi dağıtacağınız ana bilgisayarı seçmek için, geri lemenin şirket içinde varolan bir sanal makineye mi yoksa yeni bir sanal makineye mi olacağını belirleyin. 
    * Varolan bir sanal makine için, ana hedefin ana bilgisayarısanal makinenin veri depolarına erişebilmeli.
    * Şirket içi sanal makine yoksa (Alternatif Konum Kurtarma durumunda), failback sanal makine ana hedefle aynı ana bilgisayarda oluşturulur. Ana hedefi yüklemek için herhangi bir ESXi ana bilgisayar ını seçebilirsiniz.
* Ana hedef, işlem sunucusu ve yapılandırma sunucusuyla iletişim kurabilen bir ağüzerinde olmalıdır.
* Ana hedefin sürümü, işlem sunucusunun ve yapılandırma sunucusunun sürümlerine eşit veya daha erken olmalıdır. Örneğin, yapılandırma sunucusunun sürümü 9,4 ise, ana hedefin sürümü 9,4 veya 9,3 olabilir, ancak 9,5 değil.
* Ana hedef sadece bir VMware sanal makine değil, fiziksel bir sunucu olabilir.

## <a name="sizing-guidelines-for-creating-master-target-server"></a>Ana hedef sunucu oluşturmak için yönergeleri boyutlandırma

Ana hedefi aşağıdaki boyutlandırma yönergelerine uygun olarak oluşturun:
- **RAM**: 6 GB veya daha fazla
- **İşletim sistemi boyutu:** 100 GB veya daha fazla (işletim sistemi yüklemek için)
- **Bekletme sürücüsü için ek disk boyutu**: 1 TB
- **CPU çekirdekleri**: 4 çekirdek veya daha fazla

Aşağıdaki Ubuntu çekirdekleri desteklenir.


|Çekirdek Serisi  |En fazla destek  |
|---------|---------|
|4.4      |4.4.0-81-genel         |
|4.8      |4.8.0-56-jenerik         |
|4.10     |4.10.0-24-jenerik        |


## <a name="deploy-the-master-target-server"></a>Ana hedef sunucuyu dağıtma

### <a name="install-ubuntu-16042-minimal"></a>Ubuntu 16.04.2 Minimal yükleyin

Ubuntu 16.04.2 64-bit işletim sistemini kurmak için aşağıdaki adımları izleyin.

1.   [Download linkine](http://old-releases.ubuntu.com/releases/16.04.2/ubuntu-16.04.2-server-amd64.iso)gidin, en yakın aynaseçin ve bir Ubuntu 16.04.2 minimal 64-bit ISO indirin.
DVD sürücüsünde bir Ubuntu 16.04.2 minimal 64-bit ISO tutun ve sistemi başlatın.

1.  Tercih ettiğiniz dil olarak **İngilizce'yi** seçin ve sonra **Enter'u**seçin.
    
    ![Dil seçin](./media/vmware-azure-install-linux-master-target/image1.png)
1. **Ubuntu Server yükle'yi**seçin ve sonra **Enter'u**seçin.

    ![Ubuntu Server Yükle'yi seçin](./media/vmware-azure-install-linux-master-target/image2.png)

1.  Tercih ettiğiniz dil olarak **İngilizce'yi** seçin ve sonra **Enter'u**seçin.

    ![Tercih ettiğiniz dil olarak İngilizce'yi seçin](./media/vmware-azure-install-linux-master-target/image3.png)

1. **Saat Dilimi** seçenekleri listesinden uygun seçeneği seçin ve sonra **Enter'u**seçin.

    ![Doğru saat dilimini seçin](./media/vmware-azure-install-linux-master-target/image4.png)

1. **Hayır** 'ı (varsayılan seçenek) seçin ve sonra **Enter'u**seçin.

     ![Klavyeyi yapılandırma](./media/vmware-azure-install-linux-master-target/image5.png)
1. Klavyenin menşe ülkesi/bölgesi olarak **İngilizce (ABD)** seçin ve ardından **Enter'u**seçin.

1. Klavye düzeni olarak **İngilizce (ABD)** seçeneğini belirleyin ve sonra **Enter'u**seçin.

1. Sunucunuzun ana bilgisayar adını **Hostname** kutusuna girin ve ardından **Devam et'i**seçin.

1. Bir kullanıcı hesabı oluşturmak için, kullanıcı adını girin ve sonra **Devam'ı**seçin.

      ![Kullanıcı hesabı oluşturma](./media/vmware-azure-install-linux-master-target/image9.png)

1. Yeni kullanıcı hesabının parolasını girin ve sonra **Devam et'i**seçin.

1.  Yeni kullanıcının parolasını onaylayın ve sonra **Devam et'i**seçin.

    ![Parolaları onaylama](./media/vmware-azure-install-linux-master-target/image11.png)

1.  Ev dizininizi şifrelemek için bir sonraki seçimde **Hayır** 'ı (varsayılan seçenek) seçin ve sonra **Enter'u**seçin.

1. Görüntülenen saat dilimi doğruysa **Evet** 'i (varsayılan seçenek) seçin ve sonra **Enter'u**seçin. Saat diliminizi yeniden yapılandırmak için **Hayır'ı**seçin.

1. Bölümleme yöntemi seçeneklerinden **Rehberli'yi**seçin - diskin tamamını kullanın ve sonra **Enter'u**seçin.

     ![Bölümleme yöntemi seçeneğini seçin](./media/vmware-azure-install-linux-master-target/image14.png)

1.  **Bölüm seçeneklerine seç diskinden** uygun diski seçin ve sonra **Enter'u**seçin.

    ![Diski seçin](./media/vmware-azure-install-linux-master-target/image15.png)

1.  Değişiklikleri diske yazmak için **Evet'i** seçin ve sonra **Enter'u**seçin.

    ![Varsayılan seçeneği seçin](./media/vmware-azure-install-linux-master-target/image16-ubuntu.png)

1.  Yapılandırılan proxy seçiminde varsayılan seçeneği seçin, **Devam et'i**seçin ve sonra **Enter'u**seçin.
     
     ![Yükseltmeleri nasıl yöneteceklerini seçin](./media/vmware-azure-install-linux-master-target/image17-ubuntu.png)

1.  Sisteminizdeki yükseltmeleri yönetmek için seçimde **otomatik güncelleştirme yok** seçeneğini seçin ve ardından **Enter'u**seçin.

     ![Yükseltmeleri nasıl yöneteceklerini seçin](./media/vmware-azure-install-linux-master-target/image18-ubuntu.png)

    > [!WARNING]
    > Azure Site Kurtarma ana hedef sunucusu Ubuntu'nun çok özel bir sürümünü gerektirdiğinden, çekirdek yükseltmelerinin sanal makine için devre dışı bırakıldığından emin olmanız gerekir. Bunlar etkinleştirilirse, normal yükseltmeler ana hedef sunucunun arızaya neden olur. **Otomatik güncelleştirme yok** seçeneğini seçtiğinizden emin olun.

1.  Varsayılan seçenekleri seçin. SSH connect için openSSH istiyorsanız **OpenSSH sunucu** seçeneğini seçin ve ardından **Devam et'i**seçin.

    ![Yazılım seçin](./media/vmware-azure-install-linux-master-target/image19-ubuntu.png)

1. GRUB önyükleme yükleyicisini yüklemek için yapılan seçimde **Evet'i**seçin ve sonra **Enter'u**seçin.
     
    ![GRUB önyükleme yükleyici](./media/vmware-azure-install-linux-master-target/image20.png)


1. Önyükleme yükleyici yüklemesi için uygun cihazı seçin (tercihen **/dev/sda**), ve sonra **Enter'u**seçin.
     
    ![Uygun cihazı seçin](./media/vmware-azure-install-linux-master-target/image21.png)

1. **Devam et'i**seçin ve ardından yüklemeyi bitirmek için **Enter'u** seçin.

    ![Yüklemeyi sonla](./media/vmware-azure-install-linux-master-target/image22.png)

1. Yükleme tamamlandıktan sonra, yeni kullanıcı kimlik bilgileriyle VM'de oturum açın. (Daha fazla bilgi için **Adım 10'a** bakın.)

1. ROOT kullanıcı parolasını ayarlamak için aşağıdaki ekran görüntüsünde açıklanan adımları kullanın. Ardından ROOT kullanıcısı olarak oturum açın.

    ![ROOT kullanıcı parolasını ayarlama](./media/vmware-azure-install-linux-master-target/image23.png)


### <a name="configure-the-machine-as-a-master-target-server"></a>Makineyi ana hedef sunucu olarak yapılandırma

Bir Linux sanal makine, disk her SCSI sabit disk için kimlik almak **için. EnableUUID = TRUE** parametresi etkinleştirilmesi gerekir. Bu parametreyi etkinleştirmek için aşağıdaki adımları izleyin:

1. Sanal makinenizi kapatın.

2. Sol bölmedeki sanal makinenin girişini sağ tıklatın ve ardından **Ayarları Edit'i**seçin.

3. **Seçenekler** sekmesini seçin.

4. Sol bölmede **Gelişmiş** > **Genel'i**seçin ve ardından ekranın sağ alt kısmındaki **Yapılandırma Parametreleri** düğmesini seçin.

    ![Yapılandırma parametresi aç](./media/vmware-azure-install-linux-master-target/image24-ubuntu.png) 

    Makine çalışırken **Yapılandırma Parametreleri** seçeneği kullanılamıyor. Bu sekmeyi etkin hale getirmek için sanal makineyi kapatın.

5. Diskli bir satır olup olmadığını **görün. EnableUUID** zaten var.

   - Değer varsa ve **False**olarak ayarlanmışsa, değeri **True**olarak değiştirin. (Değerler büyük/küçük harf duyarlı değildir.)

   - Değer varsa ve **True**olarak ayarlanmışsa, **İptal**et'i seçin.

   - Değer yoksa **Satır Ekle'yi**seçin.

   - Ad sütununa **disk ekleyin. EnableUUID**ve sonra değeri **TRUE**olarak ayarlayın.

     ![Disk olup olmadığını kontrol ediyorum. EnableUUID zaten var](./media/vmware-azure-install-linux-master-target/image25.png)

#### <a name="disable-kernel-upgrades"></a>Çekirdek yükseltmelerini devre dışı

Azure Site Kurtarma ana hedef sunucusu, çekirdek yükseltmelerinin sanal makine için devre dışı bırakıldığından emin olmak için Ubuntu'nun belirli bir sürümünü gerektirir. Çekirdek yükseltmeleri etkinse, ana hedef sunucunun arızasına neden olabilir.

#### <a name="download-and-install-additional-packages"></a>Ek paketleri indirin ve yükleyin

> [!NOTE]
> Ek paketleri indirmek ve yüklemek için Internet bağlantısına sahip olduğundan emin olun. Internet bağlantınız yoksa, bu Deb paketlerini el ile bulmanız ve yüklemeniz gerekir.

 `apt-get install -y multipath-tools lsscsi python-pyasn1 lvm2 kpartx`

### <a name="get-the-installer-for-setup"></a>Kurulum için yükleyiciyi alın

Ana hedefiniz Internet bağlantısına sahipse, yükleyiciyi indirmek için aşağıdaki adımları kullanabilirsiniz. Aksi takdirde, yükleyiciyi işlem sunucusundan kopyalayıp yükleyebilirsiniz.

#### <a name="download-the-master-target-installation-packages"></a>Ana hedef yükleme paketlerini indirin

[En son Linux ana hedef yükleme bitlerini indirin.](https://aka.ms/latestlinuxmobsvc)

Linux kullanarak indirmek için yazın:

`wget https://aka.ms/latestlinuxmobsvc -O latestlinuxmobsvc.tar.gz`

> [!WARNING]
> Ev dizininizdeki yükleyiciyi indirdiğinizden ve fermuarını kaldırdığınızdan emin olun. **/usr/Local**adresine fermuarını açıyorsanız, yükleme başarısız olur.


#### <a name="access-the-installer-from-the-process-server"></a>İşlem sunucusundan yükleyiciye erişin

1. İşlem sunucusunda **C:\Program Files (x86)\Microsoft Azure Site Kurtarma\ev\svsystems\pushinstallsvc\repository**adresine gidin.

2. İşlem sunucusundan gerekli yükleyici dosyasını kopyalayın ve ev dizininizde **en son linuxmobsvc.tar.gz** olarak kaydedin.


### <a name="apply-custom-configuration-changes"></a>Özel yapılandırma değişiklikleri uygulama

Özel yapılandırma değişiklikleri uygulamak için ROOT kullanıcısı olarak aşağıdaki adımları kullanın:

1. İkiliyi untar etmek için aşağıdaki komutu çalıştırın.

    `tar -xvf latestlinuxmobsvc.tar.gz`

    ![Çalıştırılabilmek için komutun ekran görüntüsü](./media/vmware-azure-install-linux-master-target/image16.png)

2. İzin vermek için aşağıdaki komutu çalıştırın.

    `chmod 755 ./ApplyCustomChanges.sh`


3. Betiği çalıştırmak için aşağıdaki komutu çalıştırın.
    
    `./ApplyCustomChanges.sh`

> [!NOTE]
> Komut dosyasını sunucuda yalnızca bir kez çalıştırın. Ardından sunucuyu kapatın. Bir sonraki bölümde açıklandığı gibi, bir disk ekledikten sonra sunucuyu yeniden başlatın.

### <a name="add-a-retention-disk-to-the-linux-master-target-virtual-machine"></a>Linux ana hedef sanal makineye bir saklama diski ekleme

Bekletme diski oluşturmak için aşağıdaki adımları kullanın:

1. Linux ana hedef sanal makineye yeni bir 1-TB disk takın ve sonra makineyi başlatın.

2. Bekletme diskinin çok kimliğini öğrenmek için **çok -ll** komutunu kullanın: **multipath -ll**

    ![Çok Kimlik](./media/vmware-azure-install-linux-master-target/image27.png)

3. Sürücüyü biçimlendirin ve yeni sürücüde bir dosya sistemi oluşturun: **mkfs.ext4 /dev/mapper/\<Saklama diskinin çoklu yol numarası>. **
    
    ![Dosya sistemi](./media/vmware-azure-install-linux-master-target/image23-centos.png)

4. Dosya sistemini oluşturduktan sonra bekletme diskini monte edin.

    ```
    mkdir /mnt/retention
    mount /dev/mapper/<Retention disk's multipath id> /mnt/retention
    ```

5. Sistem her başlatında bekletme sürücüsüne monte etmek için **fstab** girişini oluşturun.
    
    `vi /etc/fstab`
    
    Dosyayı düzenlemeye başlamak için **Ekle'yi** seçin. Yeni bir satır oluşturun ve ardından aşağıdaki metni ekleyin. Disk çok kimliğini önceki komutun vurgulanan çok kimliğini temel alın.

    **/dev/mapper/\<Saklama diskleri multipath id> /mnt/retention ext4 rw 0 0**

    **Esc'yi**seçin ve ardından editör penceresini kapatmak için **:wq** (yazma ve bırakma) yazın.

### <a name="install-the-master-target"></a>Ana hedefi yükleme

> [!IMPORTANT]
> Ana hedef sunucunun sürümü, işlem sunucusunun ve yapılandırma sunucusunun sürümlerine eşit veya daha erken olmalıdır. Bu koşul karşılanmazsa, yeniden koruma başarılı olur, ancak çoğaltma başarısız olur.


> [!NOTE]
> Ana hedef sunucuyu yüklemeden önce, sanal makinedeki **/etc/hosts** dosyasının tüm ağ bağdaştırıcılarıyla ilişkili IP adresleriyle yerel ana bilgisayar adını eşleyen girişler içerip içerdiğini denetleyin.

1. Yapılandırma sunucusundaki **C:\ProgramData\Microsoft Azure Site Kurtarma\private\connection.passphrase'den parolayı** kopyalayın. Sonra aşağıdaki komutu çalıştırarak aynı yerel dizinde **passphrase.txt** olarak kaydedin:

    `echo <passphrase> >passphrase.txt`

    Örnek: 

       `echo itUx70I47uxDuUVY >passphrase.txt`
    

2. Yapılandırma sunucusunun IP adresini not edin. Ana hedef sunucuyu yüklemek ve sunucuyu yapılandırma sunucusuna kaydettirmek için aşağıdaki komutu çalıştırın.

    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <ConfigurationServer IP Address> -P passphrase.txt
    ```

    Örnek: 
    
    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i 104.40.75.37 -P passphrase.txt
    ```

Senaryo bitene kadar bekle. Ana hedef başarılı bir şekilde kaydolursa, ana hedef portalın **Site Kurtarma Altyapısı** sayfasında listelenir.


#### <a name="install-the-master-target-by-using-interactive-installation"></a>Etkileşimli yükleme yi kullanarak ana hedefi yükleyin

1. Ana hedefi yüklemek için aşağıdaki komutu çalıştırın. Aracı rolü için **ana hedefi**seçin.

    ```
    ./install
    ```

2. Yükleme için varsayılan konumu seçin ve devam etmek için **Enter'u** seçin.

    ![Ana hedefin yüklenmesi için varsayılan konum seçme](./media/vmware-azure-install-linux-master-target/image17.png)

Yükleme tamamlandıktan sonra komut satırını kullanarak yapılandırma sunucusunu kaydedin.

1. Yapılandırma sunucusunun IP adresini not edin. Bir sonraki adımda ona ihtiyacın var.

2. Ana hedef sunucuyu yüklemek ve sunucuyu yapılandırma sunucusuna kaydettirmek için aşağıdaki komutu çalıştırın.

    ```
    ./install -q -d /usr/local/ASR -r MT -v VmWare
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <ConfigurationServer IP Address> -P passphrase.txt
    ```
    Örnek: 

    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i 104.40.75.37 -P passphrase.txt
    ```

     Senaryo bitene kadar bekle. Ana hedef başarıyla kaydedilirse, ana hedef portalın **Site Kurtarma Altyapısı** sayfasında listelenir.


### <a name="install-vmware-tools--open-vm-tools-on-the-master-target-server"></a>Ana hedef sunucuya VMware araçları / açık vm-araçları yükleyin

Veri depolarını keşfedebilmek için ana hedefe VMware araçları veya açık vm-araçları yüklemeniz gerekir. Araçlar yüklenmezse, yeniden koruma ekranı veri depolarında listelenmez. VMware araçlarının yüklendikten sonra yeniden başlatmanız gerekir.

### <a name="upgrade-the-master-target-server"></a>Ana hedef sunucuyu yükseltme

Yükleyiciyi çalıştırın. Aracının ana hedefe yüklenmiş olduğunu otomatik olarak algılar. Yükseltmek için **Y'yi**seçin.  Kurulum tamamlandıktan sonra, aşağıdaki komutu kullanarak yüklenen ana hedefin sürümünü denetleyin:

`cat /usr/local/.vx_version`


**Sürüm** alanının ana hedefin sürüm numarasını verdiğini görürsünüz.

## <a name="common-issues"></a>Genel sorunlar

* Ana hedef gibi herhangi bir yönetim bileşeninde Storage vMotion'ı açmadığınızdan emin olun. Ana hedef başarılı bir yeniden korumadan sonra hareket ederse, sanal makine diskleri (VMDKs) ayrılamaz. Bu durumda, failback başarısız olur.

* Ana hedefsanal makinede herhangi bir anlık görüntü olmamalıdır. Anlık görüntüler varsa, geri alma başarısız olur.

* Bazı özel NIC yapılandırmaları nedeniyle, ağ arabirimi başlangıç sırasında devre dışı bırakılır ve ana hedef aracı sıcağı aparat edemez. Aşağıdaki özelliklerin doğru şekilde ayarlandıklarının emin olun. Ethernet kart dosyasının /etc/sysconfig/network-scripts/ifcfg-eth*'sinde bu özellikleri kontrol edin.
    * BOOTPROTO=dhcp
    * ONBOOT=Evet


## <a name="next-steps"></a>Sonraki adımlar
Ana hedefin kurulumu ve kaydı tamamlandıktan sonra, yapılandırma sunucusuna genel bakış **altında, Site Kurtarma Altyapısı'ndaki** **ana hedef** bölümünde ana hedefin göründüğünü görebilirsiniz.

Şimdi [yeniden koruma](vmware-azure-reprotect.md)ile devam edebilirsiniz, failback ardından.

