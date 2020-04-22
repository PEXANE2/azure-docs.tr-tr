---
title: Oracle ASM'yi Azure Linux sanal makinesinde ayarlama | Microsoft Dokümanlar
description: Oracle ASM'yi Azure ortamınızda hızla çalışır hale getirin.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: BorisB2015
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/02/2018
ms.author: borisb
ms.openlocfilehash: aa65b789d02c60ef6042aa62e1c138c0e1bd7224
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81676909"
---
# <a name="set-up-oracle-asm-on-an-azure-linux-virtual-machine"></a>Oracle ASM'yi Azure Linux sanal makinesinde ayarlama  

Azure sanal makineleri tam olarak yapılandırılabilir ve esnek bir bilgi işlem ortamı sağlar. Bu öğretici, Oracle Otomatik Depolama Yönetimi'nin (ASM) kurulumu ve yapılandırmasıyla birlikte temel Azure sanal makine dağıtımını kapsar.  Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Oracle Database VM oluşturma ve bu veritabanına bağlanma
> * Oracle Otomatik Depolama Yönetimi'ni yükleme ve yapılandırma
> * Oracle Grid altyapıyı yükleme ve yapılandırma
> * Oracle ASM yüklemesi başlatma
> * ASM tarafından yönetilen bir Oracle DB oluşturma


CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici için Azure CLI 2.0.4 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme]( /cli/azure/install-azure-cli). 

## <a name="prepare-the-environment"></a>Ortamı hazırlama

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kaynak grubu oluşturmak için [az group create](/cli/azure/group) komutunu kullanın. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği mantıksal bir kapsayıcıdır. Bu örnekte, *eastus* bölgesinde *myResourceGroup* adlı bir kaynak grubu.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

### <a name="create-a-vm"></a>VM oluşturma

Oracle Database görüntüsünü temel alan sanal bir makine oluşturmak ve Oracle ASM kullanacak şekilde yapılandırmak için [az vm create](/cli/azure/vm) komutunu kullanın. 

Aşağıdaki örnek, her biri 50 GB'lık dört bağlı veri diski ile Standard_DS2_v2 boyutunda myVM adında bir VM oluşturur. Varsayılan anahtar konumunda zaten yoksa, SSH anahtarları da oluşturur.  Belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın.  

   ```azurecli-interactive
   az vm create --resource-group myResourceGroup \
    --name myVM \
    --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
    --size Standard_DS2_v2 \
    --generate-ssh-keys \
    --data-disk-sizes-gb 50 50 50 50
   ```

VM'yi oluşturduktan sonra, Azure CLI aşağıdaki örneğe benzer bilgileri görüntüler. Değeri not `publicIpAddress`edin. VM'ye erişmek için bu adresi kullanırsınız.

   ```output
   {
     "fqdns": "",
     "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
     "location": "eastus",
     "macAddress": "00-0D-3A-36-2F-56",
     "powerState": "VM running",
     "privateIpAddress": "10.0.0.4",
     "publicIpAddress": "13.64.104.241",
     "resourceGroup": "myResourceGroup"
   }
   ```

### <a name="connect-to-the-vm"></a>VM’ye bağlanma

VM ile bir SSH oturumu oluşturmak ve ek ayarları yapılandırmak için aşağıdaki komutu kullanın. IP adresini VM'nizin `publicIpAddress` değeriyle değiştirin.

```bash
ssh <publicIpAddress>
```

## <a name="install-oracle-asm"></a>Oracle ASM'yi yükleyin

Oracle ASM'yi yüklemek için aşağıdaki adımları tamamlayın. 

Oracle ASM yükleme hakkında daha fazla bilgi [için, Oracle Linux 6 için Oracle ASMLib Downloads](https://www.oracle.com/technetwork/server-storage/linux/asmlib/ol6-1709075.html)bakın.  

1. ASM yüklemesine devam etmek için kök olarak giriş yapmanız gerekir:

   ```bash
   sudo su -
   ```
   
2. Oracle ASM bileşenlerini yüklemek için bu ek komutları çalıştırın:

   ```bash
    yum list | grep oracleasm 
    yum -y install kmod-oracleasm.x86_64 
    yum -y install oracleasm-support.x86_64 
    wget https://download.oracle.com/otn_software/asmlib/oracleasmlib-2.0.12-1.el6.x86_64.rpm 
    yum -y install oracleasmlib-2.0.12-1.el6.x86_64.rpm 
    rm -f oracleasmlib-2.0.12-1.el6.x86_64.rpm
   ```

3. Oracle ASM'nin yüklü olduğunu doğrulayın:

   ```bash
   rpm -qa |grep oracleasm
   ```

    Bu komutun çıktısı aşağıdaki bileşenleri listelemelidir:

    ```bash
   oracleasm-support-2.1.10-4.el6.x86_64
   kmod-oracleasm-2.0.8-15.el6_9.x86_64
   oracleasmlib-2.0.12-1.el6.x86_64
    ```

4. ASM, düzgün çalışabilmesi için belirli kullanıcılara ve rollere ihtiyaç dalıyor. Aşağıdaki komutlar, ön koşul kullanıcı hesaplarını ve gruplarını oluşturur: 

   ```bash
    groupadd -g 54345 asmadmin 
    groupadd -g 54346 asmdba 
    groupadd -g 54347 asmoper 
    useradd -u 3000 -g oinstall -G dba,asmadmin,asmdba,asmoper grid 
    usermod -g oinstall -G dba,asmdba,asmadmin oracle
   ```

5. Kullanıcıların ve grupların doğru oluşturulduğunu doğrulayın:

   ```bash
   id grid
   ```

    Bu komutun çıktısı aşağıdaki kullanıcıları ve grupları listelemelidir:

    ```bash
    uid=3000(grid) gid=54321(oinstall) groups=54321(oinstall),54322(dba),54345(asmadmin),54346(asmdba),54347(asmoper)
    ```
 
6. Kullanıcı *ızgarası* için bir klasör oluşturun ve sahibini değiştirin:

   ```bash
   mkdir /u01/app/grid 
   chown grid:oinstall /u01/app/grid
   ```

## <a name="set-up-oracle-asm"></a>Oracle ASM'yi ayarlama

Bu öğretici için varsayılan kullanıcı *ızgara* ve varsayılan grup *asmadmin*olduğunu. *Oracle* kullanıcısının asmadmin grubunun bir parçası olduğundan emin olun. Oracle ASM yüklemenizi ayarlamak için aşağıdaki adımları tamamlayın:

1. Oracle ASM kitaplık sürücüsünün ayarlanması, varsayılan kullanıcıyı (ızgara) ve varsayılan grubu (asmadmin) tanımlamanın yanı sıra sürücüyü önyüklemede başlayacak (y'yi seç) ve önyüklemedeki diskleri taramaya (y'yi seçin) yapılandırmayı içerir. Aşağıdaki komuttan gelen istemleri yanıtlamanız gerekir:

   ```bash
   /usr/sbin/oracleasm configure -i
   ```

   Bu komutun çıktısı aşağıdakine benzer, yanıtlanacak istemlerle durdurmalıdır.

    ```output
   Configuring the Oracle ASM library driver.

   This will configure the on-boot properties of the Oracle ASM library
   driver. The following questions will determine whether the driver is
   loaded on boot and what permissions it will have. The current values
   will be shown in brackets ('[]'). Hitting <ENTER> without typing an
   answer will keep that current value. Ctrl-C will abort.

   Default user to own the driver interface []: grid
   Default group to own the driver interface []: asmadmin
   Start Oracle ASM library driver on boot (y/n) [n]: y
   Scan for Oracle ASM disks on boot (y/n) [y]: y
   Writing Oracle ASM library driver configuration: done
   ```

2. Disk yapılandırmasını görüntüleyin:

   ```bash
   cat /proc/partitions
   ```

   Bu komutun çıktısı, kullanılabilir disklerin aşağıdaki listesine benzer olmalıdır

   ```output
   8       16   14680064 sdb
   8       17   14678976 sdb1
   8        0   52428800 sda
   8        1     512000 sda1
   8        2   51915776 sda2
   8       48   52428800 sdd
   8       64   52428800 sde
   8       80   52428800 sdf
   8       32   52428800 sdc
   11       0       1152 sr0
   ```

3. Aşağıdaki komutu çalıştırarak ve istemleri yanıtlayarak disk */dev/sdc'yi* biçimlendirin:
   - *n* yeni bölüm için
   - birincil bölüm için *p*
   - *İlk* bölümü seçmek için 1
   - varsayılan `enter` ilk silindir için basın
   - varsayılan `enter` son silindir için basın
   - değişiklikleri bölüm tablosuna yazmak için *w* tuşuna basın  

   ```bash
   fdisk /dev/sdc
   ```
   
   Yukarıda verilen yanıtları kullanarak, `fdisk` komut için çıktı aşağıdaki gibi görünmelidir:

   ```output
   Device contains not a valid DOS partition table, or Sun, SGI or OSF disklabel
   Building a new DOS disklabel with disk identifier 0xf865c6ca.
   Changes will remain in memory only, until you decide to write them.
   After that, of course, the previous content won't be recoverable.

   Warning: invalid flag 0x0000 of partition table 4 will be corrected by w(rite)

   The device presents a logical sector size that is smaller than
   the physical sector size. Aligning to a physical sector (or optimal
   I/O) size boundary is recommended, or performance may be impacted.

   WARNING: DOS-compatible mode is deprecated. It's strongly recommended to
           switch off the mode (command 'c') and change display units to
           sectors (command 'u').

   Command (m for help): n
   Command action
     e   extended
     p   primary partition (1-4)
   p
   Partition number (1-4): 1
   First cylinder (1-6527, default 1):
   Using default value 1
   Last cylinder, +cylinders or +size{K,M,G} (1-6527, default 6527):
   Using default value 6527

   Command (m for help): w
   The partition table has been altered!

   Calling ioctl() to re-read partition table.
   Syncing disks.
   ```

4. Yukarıdaki `fdisk` komutu `/dev/sdd`, `/dev/sde`, `/dev/sdf`ve .

5. Disk yapılandırmasını kontrol edin:

   ```bash
   cat /proc/partitions
   ```

   Komutun çıktısı aşağıdaki gibi görünmelidir:

   ```output
   major minor  #blocks  name

     8       16   14680064 sdb
     8       17   14678976 sdb1
     8       32   52428800 sdc
     8       33   52428096 sdc1
     8       48   52428800 sdd
     8       49   52428096 sdd1
     8       64   52428800 sde
     8       65   52428096 sde1
     8       80   52428800 sdf
     8       81   52428096 sdf1
     8        0   52428800 sda
     8        1     512000 sda1
     8        2   51915776 sda2
     11       0    1048575 sr0
   ```

6. Oracle ASM hizmet durumunu kontrol edin ve Oracle ASM hizmetini başlatın:

   ```bash
   service oracleasm status 
   service oracleasm start
   ```

   Komutun çıktısı aşağıdaki gibi görünmelidir:

   ```output
   Checking if ASM is loaded: no
   Checking if /dev/oracleasm is mounted: no
   Initializing the Oracle ASMLib driver:                     [  OK  ]
   Scanning the system for Oracle ASMLib disks:               [  OK  ]
   ```

7. Oracle ASM diskleri oluşturun:

   ```bash
   service oracleasm createdisk ASMSP /dev/sdc1 
   service oracleasm createdisk DATA /dev/sdd1 
   service oracleasm createdisk DATA1 /dev/sde1 
   service oracleasm createdisk FRA /dev/sdf1
   ```

   Komutun çıktısı aşağıdaki gibi görünmelidir:

   ```output
   Marking disk "ASMSP" as an ASM disk:                       [  OK  ]
   Marking disk "DATA" as an ASM disk:                        [  OK  ]
   Marking disk "DATA1" as an ASM disk:                       [  OK  ]
   Marking disk "FRA" as an ASM disk:                         [  OK  ]
   ```

8. Oracle ASM disklerini listele:

   ```bash
   service oracleasm listdisks
   ```

   Komutun çıktısı aşağıdaki Oracle ASM disklerini listelemelidir:

   ```output
    ASMSP
    DATA
    DATA1
    FRA
   ```

9. Kök, oracle ve ızgara kullanıcılarının parolalarını değiştirin. Yükleme sırasında bunları daha sonra kullandığınız gibi **bu yeni parolaları not alın.**

   ```bash
   passwd oracle 
   passwd grid 
   passwd root
   ```

10. Klasör iznini değiştirme:

    ```bash
    chmod -R 775 /opt 
    chown grid:oinstall /opt 
    chown oracle:oinstall /dev/sdc1 
    chown oracle:oinstall /dev/sdd1 
    chown oracle:oinstall /dev/sde1 
    chown oracle:oinstall /dev/sdf1 
    chmod 600 /dev/sdc1 
    chmod 600 /dev/sdd1 
    chmod 600 /dev/sde1 
    chmod 600 /dev/sdf1
    ```

## <a name="download-and-prepare-oracle-grid-infrastructure"></a>Oracle Grid Altyapısını İndirin ve hazırlayın

Oracle Grid Infrastructure yazılımını indirmek ve hazırlamak için aşağıdaki adımları tamamlayın:

1. [Oracle ASM indirme sayfasından Oracle](https://www.oracle.com/technetwork/database/enterprise-edition/downloads/database12c-linux-download-2240591.html)Grid Altyapı indirin. 

   **Linux x86-64 için Oracle Database 12c Release 1 Grid Infrastructure (12.1.0.2.0)** başlıklı indirme altında, iki .zip dosyasını indirin.

2. .zip dosyalarını istemci bilgisayarınıza indirdikten sonra, dosyaları VM'nize kopyalamak için Güvenli Kopyalama Protokolü'nü (SCP) kullanabilirsiniz:

   ```bash
   scp *.zip <publicIpAddress>:.
   ```

3. SSH, .zip dosyalarını /opt klasörüne taşımak için Azure'daki Oracle VM'nize geri döner. Ardından, dosyaların sahibini değiştirin:

   ```bash
   ssh <publicIPAddress>
   sudo mv ./*.zip /opt
   cd /opt
   sudo chown grid:oinstall linuxamd64_12102_grid_1of2.zip
   sudo chown grid:oinstall linuxamd64_12102_grid_2of2.zip
   ```

4. Dosyaların zip'ini aç. (Linux unzip aracını zaten yüklü değilse yükleyin.)

   ```bash
   sudo yum install unzip
   sudo unzip linuxamd64_12102_grid_1of2.zip
   sudo unzip linuxamd64_12102_grid_2of2.zip
   ```

5. İzni değiştir:

   ```bash
   sudo chown -R grid:oinstall /opt/grid
   ```

6. Yapılandırılmış takas alanını güncelleştirin. Oracle Grid bileşenlerinin Grid'i yüklemek için en az 6,8 GB takas alanına ihtiyacı vardır. Azure'daki Oracle Linux görüntüleri için varsayılan takas dosya boyutu yalnızca 2048MB'dır. Güncelleştirilmiş ayarların `/etc/waagent.conf` etkili olması için dosyayı artırmanız `ResourceDisk.SwapSizeMB` ve WALinuxAgent hizmetini yeniden başlatmanız gerekir. Salt okunur bir dosya olduğundan, yazma erişimini etkinleştirmek için dosya izinlerini değiştirmeniz gerekir.

   ```bash
   sudo chmod 777 /etc/waagent.conf  
   vi /etc/waagent.conf
   ```

   Arama `ResourceDisk.SwapSizeMB` ve **değeri 8192**olarak değiştirin. Giriş moduna `insert` girmek, **8192** değerini yazmak ve komut moduna `esc` dönmek için basmak için basmanız gerekir. Değişiklikleri yazmak ve dosyayı bırakmak `:wq` için `enter`yazın ve basın.
   
   > [!NOTE]
   > En iyi performans için `WALinuxAgent` her zaman yerel geçici diskte (geçici disk) oluşturulacak şekilde takas alanını yapılandırmak için her zaman kullanmanızı öneririz. Daha fazla bilgi için, [Linux Azure sanal makinelerinde takas dosyası ekleme](https://support.microsoft.com/en-us/help/4010058/how-to-add-a-swap-file-in-linux-azure-virtual-machines)ye bakın.

## <a name="prepare-your-local-client-and-vm-to-run-x11"></a>X11 çalıştırmak için yerel istemci nizi ve VM'yi hazırlayın
Oracle ASM'nin yapılandırılması, yükleme ve yapılandırmayı tamamlamak için bir grafik arabirimi gerektirir. Bu yüklemeyi kolaylaştırmak için x11 protokolünü kullanıyoruz. Zaten X11 yetenekleri etkin ve yapılandırılmış bir istemci sistemi (Mac veya Linux) kullanıyorsanız - Bu yapılandırma ve kurulum Windows makineleri için özel atlayabilirsiniz. 

1. [PuTTY'yi indirin](https://www.putty.org/) ve [Xming'i](https://xming.en.softonic.com/) Windows bilgisayarınıza indirin. Devam etmeden önce bu uygulamaların her ikisinin de varsayılan değerleriyle yüklenmesini tamamlamanız gerekir.

2. PuTTY'yi yükledikten sonra bir komut istemi açın, PuTTY klasörüne (örneğin, C:\Program `puttygen.exe` Files\PuTTY) değiştirin ve bir anahtar oluşturmak için çalıştırın.

3. PutTY anahtar jeneratörü olarak:
   
   1. Düğmeyi `Generate` seçerek bir tuş oluşturun.
   2. Anahtarın içeriğini kopyalayın (Ctrl+C).
   3. `Save private key` düğmesini seçin.
   4. Bir parolayla anahtarı niçin güvenli olduğu yla `OK`ilgili uyarıyı yoksayın ve sonra seçin.

   ![PuTTY Anahtar Jeneratör ekran görüntüsü](./media/oracle-asm/puttykeygen.png)

4. VM'nizde şu komutları çalıştırın:

   ```bash
   sudo su - grid
   mkdir .ssh 
   cd .ssh
   ```

5. `authorized_keys` adlı bir dosya oluşturun. Bu dosyadaki anahtarın içeriğini yapıştırın ve ardından dosyayı kaydedin.

   > [!NOTE]
   > Anahtar dize `ssh-rsa`içermelidir. Ayrıca, anahtarın içeriği tek bir metin satırı olmalıdır.
   >  

6. İstemci sisteminizde, PuTTY'yi başlatın. **Kategori** bölmesinde, **Bağlantı** > **SSH** > **Auth**gidin. Kimlik **doğrulama için Özel anahtar dosyasında,** daha önce oluşturduğunuz anahtara göz atın.

   ![SSH kimlik doğrulama seçeneklerinin ekran görüntüsü](./media/oracle-asm/setprivatekey.png)

7. **Kategori** bölmesinde, **Bağlantı** > **SSH** > **X11'e**gidin. **X11 yönlendirme** onay kutusunu etkinleştir'i seçin.

   ![SSH X11 yönlendirme seçeneklerinin ekran görüntüsü](./media/oracle-asm/enablex11.png)

8. **Kategori** bölmesinde **Oturum'a**gidin. Oracle ASM VM'nizi `<publicIPaddress>` ana bilgisayar adı iletişim kutusuna girin, yeni `Saved Session` bir ad girin ve sonra tıklayın. `Save`  Kaydedildikten sonra `open` Oracle ASM sanal makinenize bağlanmak için tıklayın.  İlk bağlandığınızda, uzak sistemin kayıt defterinizde önbelleğe alınmadığı konusunda uyarılırsınız. Eklemek `yes` ve devam etmek için tıklayın.

   ![PuTTY oturum seçeneklerinin ekran görüntüsü](./media/oracle-asm/puttysession.png)

## <a name="install-oracle-grid-infrastructure"></a>Oracle Grid Altyapıyı Yükleyin

Oracle Grid Altyapısını yüklemek için aşağıdaki adımları tamamlayın:

1. **Izgara**olarak oturum açın. (Bir parola istenmeden oturum açabilmelisiniz.) 

   > [!NOTE]
   > Windows çalıştırıyorsanız, yüklemeye başlamadan önce Xming'i başlattığınıza emin olun.

   ```bash
   cd /opt/grid
   ./runInstaller
   ```

   Oracle Grid Infrastructure 12c Release 1 Installer açılır. (Yükleyicinin başlaması birkaç dakika sürebilir.)

2. Yükleme **Seçeneğini Seç** sayfasında, **Bağımsız Bir Sunucu için Oracle Grid Altyapısını Yükle ve Yapılandır'ı**seçin.

   ![Yükleyicinin Yükleme Seçeneğini Seç sayfasının ekran görüntüsü](./media/oracle-asm/install01.png)

3. Ürün **Dilleri Seç** sayfasında **İngilizce'nin** veya istediğiniz dilin seçildiğinden emin olun.  `next` öğesine tıklayın.

4. **ASM Disk Grubu Oluştur** sayfasında:
   - Disk grubu için bir ad girin.
   - **Artıklık**altında, **Harici'yi**seçin.
   - **Tahsis Birim Boyutu**altında , seçin **4**.
   - **Disk ekle**altında **ORCLASMSP'yi**seçin.
   - `next` öğesine tıklayın.

5. **ASM Parolasını Belirt** sayfasında, **bu hesaplar için aynı parolaları kullan** seçeneğini seçin ve bir parola girin.

   ![Yükleyicinin ASM Şifre Belirt sayfasının ekran görüntüsü](./media/oracle-asm/install04.png)

6. Yönetim **Seçeneklerini Belirt** sayfasında, EM Bulut Denetimini yapılandırma seçeneğiniz bulunmaktadır. Biz bu seçeneği atlıyoruz `next` - devam etmek için tıklayın. 

7. Ayrıcalıklı **İşletim Sistemi Grupları** sayfasında varsayılan ayarları kullanın. Devam `next` etmek için tıklayın.

8. Yükleme **Konumu Belirt** sayfasında varsayılan ayarları kullanın. Devam `next` etmek için tıklayın.

9. Stok **Oluştur** sayfasında, Stok Dizini'ni `/u01/app/grid/oraInventory`' ' olarak değiştirin Devam `next` etmek için tıklayın.

   ![Yükleyicinin Stok Oluştur sayfasının ekran görüntüsü](./media/oracle-asm/install08.png)

10. Root **komut dosyası yürütme yapılandırma** sayfasında, **Otomatik olarak çalıştırılan yapılandırma komut dosyaları** onay kutusunu seçin. Ardından, **"root" kullanıcı kimlik bilgilerini kullan** seçeneğini seçin ve kök kullanıcı parolasını girin.

    ![Yükleyicinin Root komut dosyası yürütme yapılandırma sayfasının ekran görüntüsü](./media/oracle-asm/install09.png)

11. **Önkoşul Denetimleri Yap** sayfasında, geçerli kurulum hatalarla birlikte başarısız olur. Bu beklenen bir davranıştır. `Fix & Check Again` öğesini seçin.

12. **Fixup Script** iletişim kutusunda. `OK`

13. **Özet** sayfasında, seçtiğiniz ayarları gözden geçirin `Install`ve sonra tıklayın.

    ![Yükleyicinin Özet sayfasının ekran görüntüsü](./media/oracle-asm/install12.png)

14. Yapılandırma komut dosyalarının ayrıcalıklı bir kullanıcı olarak çalıştırılması gerektiğini bildiren bir uyarı iletişim kutusu görüntülenir. Devam `Yes` etmek için tıklayın.

15. **Bitiş** sayfasında, yüklemeyi bitirmek için tıklatın. `Close`

## <a name="set-up-your-oracle-asm-installation"></a>Oracle ASM yüklemenizi ayarlama

Oracle ASM yüklemenizi ayarlamak için aşağıdaki adımları tamamlayın:

1. X11 oturumunuzdan **ızgara**olarak oturumunuzu hala belirttiğinizden emin olun. Terminali canlandırmak `enter` için vurmanız gerekebilir. Ardından Oracle Otomatik Depolama Yönetimi Yapılandırma Yardımcısı'nı başlatın:

   ```bash
   cd /u01/app/grid/product/12.1.0/grid/bin
   ./asmca
   ```

   Oracle ASM Configuration Assistant açılır.

2. **YAPılandırılan ASM: Disk Grupları** iletişim kutusunda, `Create` düğmeyi tıklatın `Show Advanced Options`ve sonra .

3. Disk **Grubu Oluştur** iletişim kutusunda:

   - Disk grubu adı **DATA**girin.
   - **Üye Diskleri Seç'in** **altında, ORCL_DATA** ve **ORCL_DATA1**seçin.
   - **Tahsis Birim Boyutu**altında , seçin **4**.
   - Disk `ok` grubunu oluşturmak için tıklatın.
   - Onay `ok` penceresini kapatmak için tıklatın.

   ![Disk Grubu Oluştur iletişim kutusunun ekran görüntüsü](./media/oracle-asm/asm02.png)

4. **YAPılandırılan ASM: Disk Grupları** iletişim kutusunda, `Create` düğmeyi tıklatın `Show Advanced Options`ve sonra .

5. Disk **Grubu Oluştur** iletişim kutusunda:

   - Disk grubu adını **FRA**girin.
   - **Artıklık**altında, **Harici (yok)** seçin.
   - **Üye Diskleri Seç'in**altında, **ORCL_FRA**seçin.
   - **Tahsis Birim Boyutu**altında , seçin **4**.
   - Disk `ok` grubunu oluşturmak için tıklatın.
   - Onay `ok` penceresini kapatmak için tıklatın.

   ![Disk Grubu Oluştur iletişim kutusunun ekran görüntüsü](./media/oracle-asm/asm04.png)

6. ASM Yapılandırma Yardımcısı'nı kapatmak için **Çıkış'ı** seçin.

   ![Yapılandırılan ASM ekran görüntüsü: Çıkış düğmesi ile Disk Grupları iletişim kutusu](./media/oracle-asm/asm05.png)

## <a name="create-the-database"></a>Veritabanını oluşturma

Oracle veritabanı yazılımı Azure Marketi görüntüsüne zaten yüklenmiş. Veritabanı oluşturmak için aşağıdaki adımları tamamlayın:

1. Kullanıcıları Oracle süper kullanıcısına geçirin ve ardından günlüğe kaydetme için dinleyiciyi başlatma:

   ```bash
   su - oracle
   cd /u01/app/oracle/product/12.1.0/dbhome_1/bin
   ./dbca
   ```

   Veritabanı Yapılandırma Yardımcısı açılır.

2. Veritabanı **İşleyiş** sayfasında. `Create Database`

3. Oluşturma **Modu** sayfasında:

   - Veritabanı için bir ad girin.
   - **Depolama Türü**için Otomatik Depolama **Yönetimi 'nin (ASM)** seçildiğinden emin olun.
   - **Veritabanı Dosyaları Konumu**için varsayılan ASM önerilen konumu kullanın.
   - **Hızlı Kurtarma Alanı**için varsayılan ASM önerilen konumu kullanın.
   - **İdari Şifre** yazın ve **parolayı onaylayın.**
   - olduğundan `create as container database` emin olun.
   - bir `pluggable database name` değer yazın.

4. **Özet** sayfasında, seçtiğiniz ayarları gözden geçirin `Finish` ve ardından veritabanını oluşturmak için tıklatın.

   ![Özet sayfasının ekran görüntüsü](./media/oracle-asm/createdb03.png)

5. Veritabanı oluşturuldu. **Bitiş** sayfasında, bu veritabanını kullanmak ve parolaları değiştirmek için ek hesapların kilidini açma seçeneğiniz vardır. Bunu yapmak isterseniz, **Parola Yönetimi'ni** seçin `close`- aksi takdirde tıklayın.

## <a name="delete-the-vm"></a>VM’yi silin

Azure Marketi'nden Oracle DB görüntüsünde Oracle Otomatik Depolama Yönetimi'ni başarıyla yapılandırıldınız.  Bu VM'ye artık ihtiyacınız olmadığında, kaynak grubunu, VM'yi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu kullanabilirsiniz:

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

[Öğretici: Oracle DataGuard'ı yapılandırın](configure-oracle-dataguard.md)

[Öğretici: Oracle GoldenGate'i yapılandır](Configure-oracle-golden-gate.md)

Gözden Geçirme [Mimar bir Oracle DB](oracle-design.md)
