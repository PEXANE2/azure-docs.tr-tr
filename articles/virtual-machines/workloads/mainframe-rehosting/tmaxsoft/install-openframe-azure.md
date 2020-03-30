---
title: Azure Sanal Makinelere TmaxSoft OpenFrame'i yükleyin
description: Azure Sanal Makinelerde (VM) TmaxSoft OpenFrame ortamını kullanarak IBM z/OS ana bilgisayar iş yüklerinizi yeniden barındırın.
services: virtual-machines-linux
documentationcenter: ''
author: njray
ms.author: larryme
ms.date: 04/02/2019
ms.topic: article
ms.service: virtual-machines-linux
ms.openlocfilehash: 1ad6e52c421d9cfec4640d3a330b5507d6ed3e9b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72436054"
---
# <a name="install-tmaxsoft-openframe-on-azure"></a>Azure'da TmaxSoft OpenFrame'i yükleme

Azure'da geliştirme, demolar, testler veya üretim iş yüklerine uygun bir OpenFrame ortamını nasıl ayarlacağınızı öğrenin. Bu öğretici her adımda size yol alar.

OpenFrame, Azure'da ana bilgisayar öykünme ortamını oluşturan birden çok bileşen içerir. Örneğin, OpenFrame çevrimiçi hizmetleri IBM Müşteri Bilgi Kontrol Sistemi (CICS) ve OpenFrame Toplu Iş Partisi gibi ana bilgisayar ara yazılımlarının yerine, TJES bileşeni yle IBM ana bilgisayarın İş Giriş Alt Sistemi'nin (JES) yerini alır.

OpenFrame, Oracle Database, Microsoft SQL Server, IBM Db2 ve MySQL dahil olmak üzere tüm ilişkisel veritabanıyla çalışır. OpenFrame'in bu yüklemesi TmaxSoft Tibero ilişkisel veritabanını kullanır. Hem OpenFrame hem de Tibero bir Linux işletim sistemi üzerinde çalışır. Bu öğretici, desteklenen diğer Linux dağıtımlarını kullananıza rağmen CentOS 7.3'e yükler. OpenFrame uygulama sunucusu ve Tibero veritabanı tek bir sanal makineye (VM) yüklenir.

Öğretici, OpenFrame paketi bileşenlerinin yüklenmesinde size bir adım atar. Bazıları ayrı olarak yüklenmelidir.

Ana OpenFrame bileşenleri:

- Gerekli kurulum paketleri.
- Tibero veritabanı.
- Açık Veritabanı Bağlantısı (ODBC), OpenFrame'deki uygulamalar tarafından Tibero veritabanıyla iletişim kurmak için kullanılır.
- OpenFrame Base, tüm sistemi yöneten ara yazılım.
- OpenFrame Toplu İş, ana bilgisayarın toplu iş sistemlerinin yerini alan çözüm.
- TACF, sistemlere ve kaynaklara kullanıcı erişimini kontrol eden bir hizmet modülü.
- ProSort, toplu işlemler için bir sıralama aracı.
- OFCOBOL, ana bilgisayarın COBOL programlarını yorumlayan bir derleyici.
- OFASM, ana bilgisayarın assembler programlarını yorumlayan bir derleyici.
- OpenFrame Server Type C (OSC), ana bilgisayarın ara bilgisayarı nın ve IBM CICS'nin yerini alan çözümdür.
- Java Enterprise User Solution (JEUS), Java Enterprise Edition 6 için onaylı bir web uygulama sunucusu.
- OFGW, 3270 dinleyici sağlayan OpenFrame ağ geçidi bileşeni.
- OFManager, OpenFrame'in web ortamında ki çalışma ve yönetim işlevlerini sağlayan bir çözümdür.

Diğer gerekli OpenFrame bileşenleri:

- OSI, ana bilgisayar ara yazılım ve IMS DC yerine çözüm.
- TJES, ana bilgisayarın JES ortamını sağlayan çözümdür.
- OFTSAM, (V)SAM dosyalarının açık sistemde kullanılmasını sağlayan çözümdür.
- OFHiDB, ana bilgisayarın IMS DB yerine çözüm.
- OFPLI, ana bilgisayarın PL/I programlarını yorumlayan bir derleyici.
- PROTRIEVE, ana bilgisayar dili CA-Easytrieve yürüten bir çözüm.
- OFMiner, ana bilgisayar varlıklarını analiz eden ve bunları Azure'a geçiren bir çözümdür.

## <a name="architecture"></a>Mimari

Aşağıdaki şekil, bu öğreticide yüklü olan OpenFrame 7.0 mimari bileşenlerine genel bir bakış sağlar:

![OpenFrame bileşenleri](media/openframe-02.png)

## <a name="azure-system-requirements"></a>Azure sistem gereksinimleri

Aşağıdaki tabloda Azure'daki yükleme gereksinimleri listelenistır.
<!-- markdownlint-disable MD033 -->

<table>
<thead>
    <tr><th>Gereksinim</th><th>Açıklama</th></tr>
</thead>
<tbody>
<tr><td>Azure'da desteklenen Linux dağıtımları
</td>
<td>
Linux x86 2.6 (32-bit, 64-bit)<br/>
Kırmızı Şapka 7.x<br/>
CentOS 7.x<br/>
</td>
</tr>
<tr><td>Donanım
</td>
<td>Çekirdek: 2 (minimum)<br/>
Bellek: 4 GB (minimum)<br/>
Takas alanı: 1 GB (minimum)<br/>
Sabit disk: 100 GB (minimum)<br/>
</td>
</tr>
<tr><td>Windows kullanıcıları için isteğe bağlı yazılım
</td>
<td>PuTTY: VM özelliklerini yapılandırmak için bu kılavuzda kullanılır<br/>
WinSCP: Kullanabileceğiniz popüler bir SFTP istemcisi ve FTP istemcisi<br/>
Windows için Eclipse: TmaxSoft tarafından desteklenen bir geliştirme platformu<br/>
(Microsoft Visual Studio şu anda desteklenmiyor)
</td>
</tr>
</tbody>
</table>

<!-- markdownlint-enable MD033 -->

## <a name="prerequisites"></a>Ön koşullar

Gerekli tüm yazılımları bir araya getirmek ve tüm manuel işlemleri tamamlamak için birkaç gün harcamayı planlayın.

Başlamadan önce aşağıdakileri yapın:

- OpenFrame yükleme ortamını TmaxSoft'tan alın. Mevcut bir TmaxSoft müşterisiyseniz, lisanslı bir kopya için TmaxSoft temsilcinize başvurun. Aksi takdirde, [TmaxSoft](https://www.tmaxsoft.com/contact/)bir deneme sürümü isteyin.

- 'ye e-posta göndererek <support@tmaxsoft.com>OpenFrame belgelerini isteyin.

- Zaten bir Azure aboneliğiniz yoksa bir Azure aboneliği alın. Başlamadan önce ücretsiz bir [hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) da oluşturabilirsiniz.

- İsteğe bağlı. Azure VM'ye erişimi kuruluşunuzdaki izin verilen kullanıcılarla sınırlayan bir siteden siteye VPN tüneli veya atlama kutusu ayarlayın. Bu adım gerekli değildir, ancak en iyi uygulamadır.

## <a name="set-up-a-vm-on-azure-for-openframe-and-tibero"></a>OpenFrame ve Tibero için Azure'da bir VM ayarlama

OpenFrame ortamını çeşitli dağıtım desenleri kullanarak ayarlayabilirsiniz, ancak aşağıdaki yordam, OpenFrame uygulama sunucusunun ve Tibero veritabanının tek bir VM'de nasıl dağıtılacağını gösterir. Daha büyük ortamlarda ve büyük iş yükleri için en iyi yöntem, veritabanını daha iyi performans için kendi VM'inde ayrı ayrı dağıtmaktır.

**VM oluşturmak için**

1. Azure <https://portal.azure.com> portalına gidin ve hesabınızda oturum açın.

2. **Sanal makineler**'e tıklayın.

    ![Azure portalında kaynak listesi](media/vm-01.png)

3. **Ekle**’ye tıklayın.

    ![Azure portalında seçenek ekleme](media/vm-02.png)

4. **İşletim Sistemleri'nin**sağında , **Daha fazla**tıklayın.

     ![Azure portalında daha fazla seçenek](media/vm-03.png)

5. Bu gezinmeyi tam olarak takip etmek için **CentOS tabanlı 7.3'e** tıklayın veya desteklenen başka bir Linux dağıtımı seçebilirsiniz.

     ![Azure portalında İşletim Sistemi seçenekleri](media/vm-04.png)

6. Temel **Ayarlar** ayarlarında **Ad,** **Kullanıcı adı,** **Kimlik Doğrulama türü,** **Abonelik** (Kullandıkça Öde, AWS ödeme stilidir) ve **Kaynak grubuna** (varolan bir tane kullanın veya bir TmaxSoft grubu oluşturun) girin.

7. Tamamlandığında **(Kimlik Doğrulama türü**için ortak/özel anahtar çifti dahil), **Gönder'i**tıklatın.

> [!NOTE]
> **Kimlik Doğrulama türü**için bir SSH ortak anahtarı kullanıyorsanız, ortak/özel anahtar çiftini oluşturmak için sonraki bölümdeki adımlara bakın ve ardından buradaki adımları devam ettirin.

### <a name="generate-a-publicprivate-key-pair"></a>Ortak/özel anahtar çifti oluşturma

Windows işletim sistemi kullanıyorsanız, ortak/özel anahtar çifti oluşturmak için PuTTYgen gerekir.

Ortak anahtar serbestçe paylaşılabilir, ancak özel anahtar tamamen gizli tutulmalı ve asla başka bir partiyle paylaşılmamamalıdır. Anahtarları girdikten sonra, **SSH ortak anahtarını** yapılandırmaya yapıştırmanız gerekir ve bu anahtarı Linux VM'ye yükleyin. Kullanıcı hesabının \~ev\_dizininin /.ssh dizininde yetkili anahtarlar içinde saklanır. Linux VM, SSH istemcisinde ilişkili **SSH özel anahtarını** (bizim durumumuzda, PuTTY) sağladığınızda bağlantıyı tanıyabilir ve doğrulayabiliyor.

Yeni bireylerin VM'ye erişmelerini verirken: 

- Her yeni birey PuTTYgen kullanarak kendi ortak/özel anahtarlarını oluşturur.
- Kişiler kendi özel anahtarlarını ayrı olarak saklar ve ortak anahtar bilgilerini VM yöneticisine gönderir.
- Yönetici, \~/.ssh/authorized\_keys dosyasına ortak anahtarın içeriğini yapıştırır.
- Yeni birey PuTTY üzerinden bağlanır.

**Ortak/özel anahtar çifti oluşturmak için**

1.  PuTTYgen'i <https://www.putty.org/> indirin ve tüm varsayılan ayarları kullanarak yükleyin.

2.  PuTTYgen'i açmak için C:\\Program Files\\PuTTY'deki PuTTY yükleme dizinini bulun.

    ![PuTTY arabirimi](media/puttygen-01.png)

3.  **Generate** (Oluştur) düğmesine tıklayın.

    ![PuTTY Anahtar Jeneratör iletişim kutusu](media/puttygen-02.png)

4.  Nesilden sonra, hem ortak anahtarı hem de özel anahtarı kaydedin. **Sanal makine \> Oluştur Temelleri** bölmesinin SSH ortak anahtar bölümüne ortak **anahtarın** içeriğini yapıştırın (önceki bölümde 6 ve 7 numaralı basamaklarda gösterilmiştir).

    ![PuTTY Anahtar Jeneratör iletişim kutusu](media/puttygen-03.png)

### <a name="configure-vm-features"></a>VM özelliklerini yapılandırma

1. Azure portalında, **boyut** kılıcı seçin'de istediğiniz Linux makine donanım ayarlarını seçin. Hem Tibero hem de OpenFrame'i yüklemek için *minimum* gereksinimler, bu örnek yüklemede gösterildiği gibi 2 CPU ve 4 GB RAM'dir:

    ![Sanal makine oluşturun - Temel Bilgiler](media/create-vm-01.png)

2. **3 Ayarları** tıklatın ve isteğe bağlı özellikleri yapılandırmak için varsayılan ayarları kullanın.
3. Ödeme bilgilerinizi gözden geçirin.

    ![Sanal makine oluşturma - Satın alma](media/create-vm-02.png)

4. Seçimlerinizi gönderin. Azure VM dağıtmaya başlar. Bu işlem genellikle birkaç dakika sürer.

5. VM dağıtıldığında, panosu yapılandırma sırasında seçilen tüm ayarları göstererek görüntülenir. **Genel IP adresini**not alın.

    ![Azure panosunda tmax](media/create-vm-03.png)

6. PuTTY’yi açın.

7. **Ana Bilgisayar Adı**için, kullanıcı adınızı ve kopyaladığınız genel IP adresini yazın. Örneğin, **kullanıcı\@adı publicip**.

    ![PuTTY Yapılandırma iletişim kutusu](media/putty-01.png)

8. **Kategori** kutusunda Bağlantı ** \> SSH \> Auth'u**tıklatın. **Özel anahtar** dosyanıza giden yolu sağlayın.

    ![PuTTY Yapılandırma iletişim kutusu](media/putty-02.png)

9. PuTTY penceresini başlatmak için **Aç'ı** tıklatın. Başarılı olursanız, Azure'da çalışan yeni CentOS VM'nize bağlanırsınız.

10. Kök kullanıcı olarak oturum açmak için **sudo bash**yazın.

    ![Komut penceresinde kök kullanıcı girişi](media/putty-03.png)

## <a name="set-up-the-environment-and-packages"></a>Ortamı ve paketleri ayarlama

VM oluşturulduğuna ve oturum açtığınıza göre, birkaç kurulum adımı gerçekleştirmeniz ve gerekli ön yükleme paketlerini yüklemeniz gerekir.

1. Hosts dosyasını düzenlemek **için** vi kullanarak demo adını yerel`vi /etc/hosts`IP adresine eşle ( ). Bizim IP 192.168.96.148 ofdemo olduğunu varsayarsak, bu değişiklikten önce:

    ```vi
    127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4 
    ::1              localhost localhost.localdomain localhost6 localhost6.localdomain 
    <IP Address>    <your hostname>
    ```

     Bu değişiklikten sonra:

    ```vi
    127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4 
    ::1              localhost localhost.localdomain localhost6 localhost6.localdomain 
    192.168.96.148   ofdemo
    ```

2. Gruplar ve kullanıcılar oluşturun:

    ```vi
    [root@ofdemo ~]# adduser -d /home/oframe7 oframe7 
    [root@ofdemo ~]# passwd oframe7
    ```

3. Kullanıcı oframe7 için parolayı değiştirin:

    ```vi
    New password: 
    Retype new password: 
    passwd: all authentication tokens updated successfully.
    ```

4. /etc/sysctl.conf'taki çekirdek parametrelerini güncelleştirin:

    ```vi
    [root@ofdemo ~]# vi /etc/sysctl.conf
    kernel.shmall = 7294967296 
    kernel.sem = 10000 32000 10000 10000
    ```

5. Çekirdek parametrelerini yeniden başlatmadan dinamik olarak yenileyin:

    ```vi
    [root@ofdemo ~]# /sbin/sysctl -p
    ```

6. Gerekli paketleri alın: Sunucunun Internet'e bağlı olduğundan emin olun, aşağıdaki paketleri indirin ve sonra yükleyin:

     - dos2unix
     - glibc
     - glibc.i686 glibc.x86\_64
     - libaio
     - Ncurses

          > [!NOTE]
          > ncurses paketini yükledikten sonra, aşağıdaki sembolik bağlantıları oluşturun:
         ```
         ln -s /usr/lib64/libncurses.so.5.9 /usr/lib/libtermcap.so
         ln -s /usr/lib64/libncurses.so.5.9 /usr/lib/libtermcap.so.2
         ```

     - Gcc
     - gcc-c++
     - libaio-devel.x86\_64
     - strace
     - ltrace
     - Gdb

7. Java RPM yüklemesi durumunda aşağıdakileri yapın:

```
root@ofdemo ~]# rpm -ivh jdk-7u79-linux-x64.rpm
[root@ofdemo ~]# vi .bash_profile

# JAVA ENV
export JAVA_HOME=/usr/java/jdk1.7.0_79/
export PATH=$JAVA_HOME/bin:$PATH
export CLASSPATH=$CLASSPATH:$JAVA_HOME/jre/lib/ext:$JAVA_HOME/lib/tools.jar

[root@ofdemo ~]# source /etc/profile
[root@ofdemo ~]# java –version

java version "1.7.0_79"
Java(TM) SE Runtime Environment (build 1.7.0_79-b15)
Java HotSpot(TM) 64-Bit Server VM (build 24.79-b02, mixed mode)

[root@ofdemo ~]# echo $JAVA_HOME /usr/java/jdk1.7.0_79/
```

## <a name="install-the-tibero-database"></a>Tibero veritabanını yükleyin

Tibero, Azure'daki OpenFrame ortamında ki birkaç önemli işlevi sağlar:

- Tibero, çeşitli sistem işlevleri için OpenFrame dahili veri deposu olarak kullanılır.
- KSDS, RRDS ve ESDS dahil olmak üzere VSAM dosyaları, veri depolama için dahili olarak Tibero veritabanını kullanır.
- TACF veri deposu Tibero'da depolanır.
- OpenFrame katalog bilgileri Tibero'da saklanır.
- Tibero veritabanı, uygulama verilerini depolamak için IBM Db2'nin yerine kullanılabilir.

**Tibero'yu yüklemek için**

1. Tibero ikili yükleyici dosyasının mevcut olduğunu doğrulayın ve sürüm numarasını gözden geçirin.
2. Tibero yazılımını Tibero kullanıcı hesabına (oframe) kopyalayın. Örnek:

    ```
    [oframe7@ofdemo ~]$ tar -xzvf tibero6-bin-6_rel_FS04-linux64-121793-opt-tested.tar.gz 
    [oframe7@ofdemo ~]$ mv license.xml /opt/tmaxdb/tibero6/license/
    ```

3. Vi '\_`vi .bash_profile`da .bash profilini açın ve aşağıdakileri yapıştırın:

    ```
    # Tibero6 ENV
    export TB_HOME=/opt/tmaxdb/tibero6 
    export TB_SID=TVSAM export TB_PROF_DIR=$TB_HOME/bin/prof 
    export LD_LIBRARY_PATH=$TB_HOME/lib:$TB_HOME/client/lib:$LD_LIBRARY_PATH 
    export PATH=$TB_HOME/bin:$TB_HOME/client/bin:$PATH
    ```

4. Bash profilini yürütmek için komut istemi türünde:

    ```
    source .bash_profile
    ```

5. İpucu dosyasını (Tibero için bir yapılandırma dosyası) oluşturun ve vi ile açın. Örnek:

    ```
    [oframe7@ofdemo ~]$ sh $TB_HOME/config/gen_tip.sh
    [oframe7@ofdemo ~]$ vi $TB_HOME/config/$TB_SID.tip
    ```

6. TB \$\_HOME/client/config/tbdsn.tbr'yi değiştirin ve gösterildiği gibi localhost yerine 127.0.0.1 koyun:

    ```
    TVSAM=( 
    (INSTANCE=(HOST=127.0.0.1)
        (PT=8629)
    (DB_NAME=TVSAM)
          )
     )
    ```

7. Veritabanını oluşturun. Şu çıktı görünür:

    ```
    Change core dump dir to /opt/tmaxdb/tibero6/bin/prof.
    Listener port = 8629
    Tibero 6
    TmaxData Corporation Copyright (c) 2008-. All rights reserved.
    Tibero instance started up (NOMOUNT mode).
     /--------------------- newmount sql ------------------------/
    create database character set MSWIN949 national character set UTF16;
    /-----------------------------------------------------------/
    Database created.
    Change core dump dir to /opt/tmaxdb/tibero6/bin/prof.
    Listener port = 8629
    Tibero 6
    TmaxData Corporation Copyright (c) 2008-. All rights reserved.
    Tibero instance started up (NORMAL mode).
    /opt/tmaxdb/tibero6/bin/tbsvr
    ………………………..
    Creating agent table...
    Done.
    For details, check /opt/tmaxdb/tibero6/instance/TVSAM/log/system_init.log.
    ************************************************** 
    * Tibero Database TVSAM is created successfully on Fri Aug 12 19:10:43 UTC 2016.
    *     Tibero home directory ($TB_HOME) =
    *         /opt/tmaxdb/tibero6
    *     Tibero service ID ($TB_SID) = TVSAM
    *     Tibero binary path =
    *         /opt/tmaxdb/tibero6/bin:/opt/tmaxdb/tibero6/client/bin
    *     Initialization parameter file =
    *         /opt/tmaxdb/tibero6/config/TVSAM.tip
    * 
    * Make sure that you always set up environment variables $TB_HOME and
    * $TB_SID properly before you run Tibero.
     ******************************************************************************
    ```

8. Tibero'yu geri dönüştürmek için önce `tbdown` komutu kullanarak kapatın. Örnek:

    ```
    [oframe7@ofdemo ~]$$ tbdown 
    Tibero instance terminated (NORMAL mode).
    ```

9. Şimdi çizme Tibero kullanarak `tbboot`. Örnek:

    ```
    [oframe7@ofdemo ~]$ tbboot
    Change core dump dir to /opt/tmaxdb/tibero6/bin/prof. Listener port = 8629

    Tibero 6  
    TmaxData Corporation Copyright (c) 2008-. All rights reserved.
    Tibero instance started up (NORMAL mode).
    ```

10. Bir tablo alanı oluşturmak için, SYS kullanıcısını (sys/tmax) kullanarak veritabanına erişin ve ardından varsayılan birim ve TACF için gerekli tablo alanını oluşturun:

    ```
    [oframe7@ofdemo ~]$ tbsql tibero/tmax
    tbSQL 6  
    TmaxData Corporation Copyright (c) 2008-. All rights reserved.
    Connected to Tibero.
    ```

11. Şimdi aşağıdaki SQL komutlarını yazın:

    ```
    SQL> create tablespace "DEFVOL" datafile 'DEFVOL.dbf' size 500M autoextend on; create tablespace "TACF00" datafile 'TACF00.dbf' size 500M autoextend on; create tablespace "OFM_REPOSITORY" datafile 'ofm_repository.dbf' size 300M autoextend on;
    SQL> Tablespace 'DEFVOL' created.
    SQL> Tablespace 'TACF00' created.
    SQL> Tablespace ' OFM_REPOSITORY ' created.
    SQL> SQL> Disconnected.
    ```

12. Boot Tibero ve Tibero işlemlerinin çalıştığını doğrulayın:

    ```
    [oframe7@ofdemo ~]$ tbboot 
    ps -ef | egrep tbsvr
    ```

Çıktı:

![Tibero çıkışı](media/tibero-01.png)

## <a name="install-odbc"></a>ODBC'yi Yükleyin

OpenFrame'deki uygulamalar, açık kaynak kodlu unixODBC projesi tarafından sağlanan ODBC API'sini kullanarak Tibero veritabanı ile iletişim kurar.

ODBC'yi yüklemek için:

1. UnixODBC-2.3.4.tar.gz yükleyici dosyasının mevcut olduğunu doğrulayın veya komutu `wget unixODBC-2.3.4.tar.gz` kullanın. Örnek:

     ```
     [oframe7@ofdemo ~]$ wget ftp://ftp.unixodbc.org/pub/unixODBC/unixODBC-2.3.4.tar.gz
     ```

2. İkilinin zip'ini aç. Örnek:

     ```
     [oframe7@ofdemo ~]$ tar -zxvf unixODBC-2.3.4.tar.gz
     ```

3. UnixODBC-2.3.4 dizinine gidin ve kontrol makinesi bilgilerini kullanarak Makefile'yı oluşturun. Örnek:

     ```
     [oframe7@ofdemo unixODBC-2.3.4]$ ./configure --prefix=/opt/tmaxapp/unixODBC/ --sysconfdir=/opt/tmaxapp/unixODBC/etc
     ```

     Varsayılan olarak, unixODBC /usr /local'a `--prefix` yüklenir, bu nedenle konumu değiştirmek için bir değer geçer. Benzer şekilde, yapılandırma dosyaları varsayılan olarak /etc'ye yüklenir, böylece `--sysconfdir` istenen konumun değerini geçirir.

4. Makefile'yı Yürüt:`[oframe7@ofdemo unixODBC-2.3.4]$ make`

5. Derledikten sonra yürütülebilir dosyayı program dizininde kopyalayın. Örnek:

     ```
     [oframe7@ofdemo unixODBC-2.3.4]$ make install
     ```

6. Bash profilini (`vi ~/.bash_profile`) ve aşağıdakileri eklemek için vi'yi kullanın:

     ```
     # UNIX ODBC ENV 
     export ODBC_HOME=$HOME/unixODBC 
     export PATH=$ODBC_HOME/bin:$PATH 
     export LD_LIBRARY_PATH=$ODBC_HOME/lib:$LD_LIBRARY_PATH 
     export ODBCINI=$HOME/unixODBC/etc/odbc.ini 
     export ODBCSYSINI=$HOME
     ```

7. ODBC'yi uygulayın. Aşağıdaki dosyaları buna göre edin. Örnek:

     ```
     [oframe7@ofdemo unixODBC-2.3.4]$ source ~/.bash_profile

     [oframe7@ofdemo ~]$ cd

     [oframe7@ofdemo ~]$ odbcinst -j unixODBC 2.3.4
     DRIVERS............: /home/oframe7/odbcinst.ini
     SYSTEM DATA SOURCES: /home/oframe7/odbc.ini
     FILE DATA SOURCES..: /home/oframe7/ODBCDataSources
     USER DATA SOURCES..: /home/oframe7/unixODBC/etc/odbc.ini
     SQLULEN Size.......: 8
     SQLLEN Size........: 8
     SQLSETPOSIROW Size.: 8

     [oframe7@ofdemo ~]$ vi odbcinst.ini

     [Tibero]
     Description = Tibero ODBC driver for Tibero6
     Driver = /opt/tmaxdb/tibero6/client/lib/libtbodbc.so
     Setup = 
     FileUsage = 
     CPTimeout = 
     CPReuse = 
     Driver Logging = 7

     [ODBC]
     Trace = NO 
     TraceFile = /home/oframe7/odbc.log 
     ForceTrace = Yes 
     Pooling = No 
     DEBUG = 1

     [oframe7@ofdemo ~]$ vi odbc.ini

     [TVSAM]
     Description = Tibero ODBC driver for Tibero6 
     Driver = Tibero 
     DSN = TVSAM 
     SID = TVSAM 
     User = tibero 
     password = tmax
     ```

8. Sembolik bir bağlantı oluşturun ve Tibero veritabanı bağlantısını doğrulayın:

     ```
     [oframe7@ofdemo ~]$ ln $ODBC_HOME/lib/libodbc.so $ODBC_HOME/lib/libodbc.so.1 [oframe7@ofdemo ~]$ ln $ODBC_HOME/lib/libodbcinst.so 
     $ODBC_HOME/lib/libodbcinst.so.1

     [oframe7@ofdemo lib]$ isql TVSAM tibero tmax
     ```

Aşağıdaki çıktı görüntülenir:

![SQL'e bağlı olduğunu gösteren ODBC çıktısı](media/odbc-01.png)

## <a name="install-openframe-base"></a>OpenFrame Base'i yükleme

Temel uygulama sunucusu, Işlem işleme sunucu işlemleri de dahil olmak üzere OpenFrame'in Azure'daki sistemi yönetmek için kullandığı tek tek hizmetlerden önce yüklenir.

**OpenFrame Base'i yüklemek için**

1. Tibero yüklemesinin başarılı olduğundan emin olun, ardından\_aşağıdaki\_\_OpenFrame Base7 0 Linux\_x86\_64.bin yükleyici dosyası ve base.properties yapılandırma dosyasının mevcut olduğunu doğrulayın.

2. Bash profilini aşağıdaki Tibero'ya özgü bilgilerle güncelleştirin:

     ```bash
     alias ofhome='cd $OPENFRAME_HOME'
     alias ulog='cd $OPENFRAME_HOME/log/tmax/ulog'
     alias sysjcl='cd $OPENFRAME_HOME/volume_default/SYS1.JCLLIB'
     alias sysload='cd $OPENFRAME_HOME/volume_default/SYS1.LOADLIB'
     alias sysproc='cd $OPENFRAME_HOME/volume_default/SYS1.PROCLIB'
     alias oscsrc='cd $OPENFRAME_HOME/osc/oivp'
     alias osisrc='cd $OPENFRAME_HOME/osi/oivp'
     alias defvol='cd $OPENFRAME_HOME/volume_default'
     ```

3. Bash profilini çalıştırın:`[oframe7@ofdemo ~]$ . .bash_profile`
4. Tibero işlemlerinin çalışmadığından emin olun. Örnek:

     ```linux
     [oframe7@ofdemo ~]$ ps -ef|grep tbsvr
     ```

    ![Temel](media/base-01.png)

     > [!IMPORTANT]
     > Kurulumdan önce Tibero'yu başlattığınıza emin olun.

5. [technet.tmaxsoft.com'da](https://technet.tmaxsoft.com/en/front/main/main.do) lisans oluşturun ve OpenFrame Base, Batch, TACF, OSC lisanslarını uygun klasöre koyun:

     ```
     [oframe7@ofdemo ~]$ cp license.dat /opt/tmaxapp/OpenFrame/core/license/
     [oframe7@ofdemo ~]$ cp lictjes.dat lictacf.dat licosc.dat $OPENFRAME_HOME/license/
     ```

6. OpenFrame Base ikili ve base.properties dosyalarını indirin:

     ```
     [oframe7@ofdemo ~]$ vi base.properties
     OPENFRAME_HOME= <appropriate location for installation> ex. /opt/tmaxapp/OpenFrame TP_HOST_NAME=<your IP Hostname> ex. ofdemo 
     TP_HOST_IP=<your IP Address> ex. 192.168.96.148 
     TP_SHMKEY=63481 
     TP_TPORTNO=6623 
     TP_UNBLOCK_PORT=6291 
     TP_NODE_NAME=NODE1 
     TP_NODE_LIST=NODE1 
     MASCAT_NAME=SYS1.MASTER.ICFCAT 
     MASCAT_CREATE=YES 
     DEFAULT_VOLSER=DEFVOL 
     VOLADD_DEFINE=YES TSAM_USERNAME=tibero 
     TSAM_PASSWORD=tmax 
     TSAM_DATABASE=oframe 
     DATASET_SHMKEY=63211 
     DSLOCK_DATA=SYS1.DSLOCK.DATA 
     DSLOCK_LOG=SYS1.DSLOCK.LOG 
     DSLOCK_SEQ=dslock_seq.dat 
     DSLOCK_CREATE=YES 
     OPENFRAME_LICENSE_PATH=/opt/tmaxapp/license/OPENFRAME TMAX_LICENSE_PATH=/opt/tmaxapp/license/TMAX
     ```

7. Base.properties dosyasını kullanarak yükleyiciyi çalıştırın. Örnek:

    ```
    [oframe7@ofdemo ~]$ chmod a+x OpenFrame_Base7_0_Linux_x86_64.bin 
    [oframe7@ofdemo ~]$ ./OpenFrame_Base7_0_Linux_x86_64.bin -f base.properties
    ```

    Tamamlandığında, Yükleme Tamamlandı iletisi diplayed.

8. Komutu kullanarak OpenFrame Base `ls -ltr` dizin yapısını doğrulayın. Örnek:

     ```
     [oframe7@ofdemo OpenFrame]$ ls -ltr
     total 44

     drwxrwxr-x. 4 oframe7 oframe7 61 Nov 30 16:57 UninstallerData 
     drwxrwxr-x. 2 oframe7 oframe7 4096 Nov 30 16:57 bin 
     drwxrwxr-x. 2 oframe7 oframe7 4096 Nov 30 16:57 cpm drwxrwxr-x. 2 oframe7 oframe7 4096 Nov 30 16:57 data 
     drwxrwxr-x. 2 oframe7 oframe7 4096 Nov 30 16:57 include 
     drwxrwxr-x. 2 oframe7 oframe7 8192 Nov 30 16:57 lib 
     drwxrwxr-x. 6 oframe7 oframe7 48 Nov 30 16:57 log 
     drwxrwxr-x. 2 oframe7 oframe7 6 Nov 30 16:57 profile 
     drwxrwxr-x. 7 oframe7 oframe7 62 Nov 30 16:57 sample 
     drwxrwxr-x. 2 oframe7 oframe7 6 Nov 30 16:57 schema 
     drwxrwxr-x. 2 oframe7 oframe7 6 Nov 30 16:57 temp 
     drwxrwxr-x. 3 oframe7 oframe7 16 Nov 30 16:57 shared 
     drwxrwxr-x. 2 oframe7 oframe7 4096 Nov 30 16:58 license 
     drwxrwxr-x. 23 oframe7 oframe7 4096 Nov 30 16:58 core 
     drwxrwxr-x. 2 oframe7 oframe7 4096 Nov 30 16:58 config 
     drwxrwxr-x. 2 oframe7 oframe7 4096 Nov 30 16:58 scripts 
     drwxrwxr-x. 2 oframe7 oframe7 25 Nov 30 16:58 volume_default
     ```

9. OpenFrame Tabanını Başlat:

     ```
     [oframe7@ofdemo ~]$ cp /usr/lib/libtermcap.so.2 $TMAXDIR/lib
     Startup Tmax Server
     [oframe7@ofdemo ~]$ tmboot
     ```

     ![tmboot komut çıkışı](media/base-02.png)

10. Si'deki tmadmin komutunu kullanarak işlem durumunun hazır olduğunu doğrulayın. RDY, her işlem için **durum** sütununda görüntülenir:

     ![tmadmin komut çıktısı](media/base-03.png)

11. OpenFrame Tabanını kapatın:

     ```
     [oframe7@ofdemo ~]$ tmdown 
     Do you really want to down whole Tmax? (y : n): y

     TMDOWN for node(NODE1) is starting: 
     TMDOWN: SERVER(ofrsasvr:36) downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: SERVER(ofrdsedt:39) downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: SERVER(vtammgr:43) downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: SERVER(ofrcmsvr:40) downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: SERVER(ofrdmsvr:38) downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: SERVER(ofrlhsvr:37) downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: SERVER(ofruisvr:41) downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: SERVER(ofrsmlog:42) downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: CLH downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: CLL downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: TLM downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: TMM downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: TMAX is down
     ```

## <a name="install-openframe-batch"></a>OpenFrame Toplu İşlemini Yükleme

OpenFrame Toplu İşlem, ana bilgisayar toplu iş ortamlarını simüle eden birkaç bileşenden oluşur ve Azure'da toplu iş çalıştırmak için kullanılır.

**Toplu İş'i yüklemek için**

1. Temel yüklemenin başarılı olduğundan emin olun,\_ardından\_OpenFrame\_Batch7\_\_0\_\_Fix2 MVS Linux x86 64.bin yükleyici dosyası ve batch.properties yapılandırma dosyasının mevcut olduğunu doğrulayın:

2. Komut isteminde, `vi batch.properties` toplu iş.özellikler dosyasını vi kullanarak yeniden yazmak için yazın.

3. Parametreleri aşağıdaki gibi değiştirin:

     ```
     OPENFRAME_HOME = /opt/tmaxapp/OpenFrame
     DEFAULT_VOLSER=DEFVOL 
     TP_NODE_NAME=NODE1 
     TP_NODE_LIST=NODE1 
     RESOURCE_SHMKEY=66991 
     #JOBQ_DATASET_CREATE=YES 
     #OUTPUTQ_DATASET_CREATE=YES 
     DEFAULT_JCLLIB_CREATE=YES 
     DEFAULT_PROCLIB_CREATE=YES 
     DEFAULT_USERLIB_CREATE=YES 
     TJES_USERNAME=tibero 
     TJES_PASSWORD=tmax 
     TJES_DATABASE=oframe 
     BATCH_TABLE_CREATE=YES
     ```

4. Toplu iş yükleyicisini yürütmek için komut istemi türünde:

     ```
     ./OpenFrame_Batch7_0_Fix2_MVS_Linux_x86_64.bin -f batch.properties
     ```

5. Yükleme tamamlandığında, komut istemine yazarak `tmboot` yüklü OpenFrame paketlerini başlatın.

    ![tmboot çıkışı](media/tmboot-01.png)

6. OpenFrame işlemini denetlemek için komut istemine yazın. `tmadmin`

    ![Tmax Admin ekranı](media/tmadmin-01.png)

7. Aşağıdaki komutları uygulayın:

     ```
     $$2 NODE1 (tmadm): quit 
     ADM quit for node (NODE1)
     ```

8. Toplu `tmdown` İş'i başlatmak ve kapatmak için komutu kullanın:

     ```
     [oframe7@ofdemo ~]$tmdown
     Do you really want to down whole Tmax? (y : n): y

     TMDOWN for node(NODE1) is starting: 
     TMDOWN: SERVER(ofrsasvr:36) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjmsvr:44) downed: Wed Sep  7 16:01:46 2016
     TMDOWN: SERVER(vtammgr: 43) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(ofrcmsvr:40) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjmsvr:45) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjmsvr:46) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(ofrdmsvr:38) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjmsvr:47) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(ofrdsedt:39) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjschd:54) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjinit:55) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjmsvr:48) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjspbk:57) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjmsvr:49) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjmsvr:50) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjmsvr:51) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(ofrlhsvr:37) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjmsvr:52) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjmsvr:53) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjhist:56) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(ofruisvr:41) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmtsmgr:59) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(ofrpmsvr:58) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(ofrsmlog:42) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: CLL downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: TLM downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: CLH downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: TMM downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: TMAX is down
     ```

## <a name="install-tacf"></a>TACF'yi yükleyin

TACF Manager RACF güvenliği aracılığıyla sistemlere ve kaynaklara kullanıcı erişimini kontrol eden bir OpenFrame hizmet modülüdür.

**TACF'yi yüklemek için**

1. \_OpenFrame\_Tacf7 0\_Fix2\_\_Linux x86\_64.bin yükleyici dosyası ve tacf.properties yapılandırma dosyasının mevcut olduğunu doğrulayın.
2. Toplu İşlem yüklemesinin başarılı olduğundan emin olun, sonra tacf.properties dosyasını açmak için vi kullanın (`vi tacf.properties`).
3. TACF parametrelerini değiştirin:

     ```
     OPENFRAME_HOME=/opt/tmaxapp/OpenFrame 
     USE_OS_AUTH=NO 
     TACF_USERNAME=tibero 
     TACF_PASSWORD=tmax 
     TACF_DATABASE=oframe 
     TACF_TABLESPACE=TACF00 
     TACF_TABLE_CREATE=YES
     ```

4. TACF yükleyicisini tamamladıktan sonra, TACF ortam değişkenlerini uygulayın. Komut istemine şunları yazın:

     ```
     source \~/.bash\_profile
     ```

5. TACF yükleyicisini çalıştırın. Komut istemine şunları yazın:

     ```
     ./OpenFrame_Tacf7_0_Fix2_Linux_x86_64.bin -f tacf.properties
     ```

     Çıktı şuna benzer:

     ```
     Wed Dec 07 17:36:42 EDT 2016
     Free Memory: 18703 kB 
     Total Memory: 28800 kB

     4 Command Line Args: 
     0:  -f 1:  tacf.properties 
     2:  -m 
     3:  SILENT 
     java.class.path: 
     /tmp/install.dir.41422/InstallerData 
     /tmp/install.dir.41422/InstallerData/installer.zip 
     ZGUtil.CLASS_PATH: 
     /tmp/install.dir.41422/InstallerData 
     tmp/install.dir.41422/InstallerData/installer.zip 
     sun.boot.class.path: 
     /tmp/install.dir.41422/Linux/resource/jre/lib/resources.jar /tmp/install.dir.41422/Linux/resource/jre/lib/rt.jar /tmp/install.dir.41422/Linux/resource/jre/lib/sunrsasign.jar /tmp/install.dir.41422/Linux/resource/jre/lib/jsse.jar /tmp/install.dir.41422/Linux/resource/jre/lib/jce.jar /tmp/install.dir.41422/Linux/resource/jre/lib/charsets.jar /tmp/install.dir.41422/Linux/resource/jre/lib/jfr.jar /tmp/install.dir.41422/Linux/resource/jre/classes
     ```

6. Komut isteminde OpenFrame'i yeniden başlatmak için yazın. `tmboot` Çıktı şuna benzer:

     ```
     TMBOOT for node(NODE1) is starting: 
     Welcome to Tmax demo system: it will expire 2016/11/4 
     Today: 2016/9/7 
     TMBOOT: TMM is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: CLL is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: CLH is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: TLM(tlm) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(ofrsasvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(ofrlhsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(ofrdmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(ofrdsedt) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(ofrcmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(ofruisvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(ofrsmlog) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(vtammgr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjschd) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjinit) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjhist) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjspbk) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(ofrpmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmtsmgr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(tmsvr) is starting: Wed Sep  7 17:48:53 2016
     ```

7. `si` Komutta kullanarak `tmadmin` işlem durumunun hazır olduğunu doğrulayın. Örnek:

     ```
     [oframe7\@ofdemo \~]\$ tmadmin
     ```

     **Durum** sütununda RDY görüntülenir:

    ![Durum sütunundaki RDY](media/tmboot-02.png)

8. Aşağıdaki komutları uygulayın:

     ```
     $$2 NODE1 (tmadm): quit 
     DM quit for node (NODE1)

     [oframe7@ofdemo ~]$ tacfmgr 
     Input USERNAME  : ROOT 
     Input PASSWORD  : SYS1

     TACFMGR: TACF MANAGER START!!!
     QUIT TACFMGR: TACF MANAGER END!!!

     [oframe7@ofdemo ~]$ tmdow
     ```

9. Komutu kullanarak sunucuyu `tmdown` kapatın. Çıktı şuna benzer:

     ```
     [oframe7@ofdemo ~]$ tmdown 
     Do you really want to down whole Tmax? (y : n): y

     TMDOWN for node(NODE1) is starting: 
     TMDOWN: SERVER(ofrlhsvr:37) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(ofrdsedt:39) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(obmjschd:54) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(obmjmsvr:47) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(obmjmsvr:48) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(ofrdmsvr:38) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(obmjmsvr:50) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(obmjhist:56) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(ofrsasvr:36) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(ofrcmsvr:40) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(obmjspbk:57) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(tmsvr:60) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(ofrpmsvr:58) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(obmtsmgr:59) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: CLL downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: CLH downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: TLM downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: TMM downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: TMAX is down
     ```

## <a name="install-prosort"></a>ProSort'u Yükle

ProSort, verileri sıralamak için toplu işlemlerde kullanılan bir yardımcı programdır.

**ProSort'u yüklemek için**

1. Toplu İşlem yüklemesinin başarılı olduğundan emin olun ve **prosort-bin-prosort\_2sp3-linux64-2123-opt.tar.gz** yükleyici dosyasının mevcut olduğunu doğrulayın.

2. Özellikler dosyasını kullanarak yükleyiciyi çalıştırın. Komut istemine şunları yazın:

     ```
     tar -zxvf prosort-bin-prosort\_2sp3-linux64-2123-opt.tar.gz
     ```

3. Prosort dizinini ev konumuna taşıyın. Komut istemine şunları yazın:

     ```
     mv prosort /opt/tmaxapp/prosort
     ```

4. Bir lisans alt dizini oluşturun ve lisans dosyasını burada kopyalayın. Örnek:

     ```
     cd /opt/tmaxapp/prosort 
     mkdir license 
     cp /opt/tmaxsw/oflicense/prosort/license.xml /opt/tmaxapp/prosort/license
     ```

5. Açık bash.profile içinde`vi .bash_profile`vi ( ) ve aşağıdaki gibi güncelleyin:

     ```bash
     #       PROSORT

     PROSORT_HOME=/opt/tmaxapp/prosort 
     PROSORT_SID=gbg 
     PATH=$PATH:$PROSORT_HOME/bin LD_LIBRARY_PATH=$PROSORT_HOME/lib:$LD_LIBRARY_PATH LIBPATH$PROSORT_HOME/lib:$LIBPATH 
     export PROSORT_HOME PROSORT_SID 
     PATH LD_LIBRARY_PATH LIBPATH 
     PATH=$PATH:$OPENFRAME_HOME/shbin 
     export PATH
     ```

6. Bash profilini yürütmek için komut istemine yazın:`. .bash_profile`

7. Yapılandırma dosyasını oluşturun. Örnek:

     ```
     oframe@oframe7: cd /opt/tmaxapp/prosort/config 
     oframe@oframe7: ./gen_tip.sh 
     Using PROSORT_SID "gbg"
      /home/oframe7/prosort/config/gbg.tip generated
     ```

8. Sembolik bağlantıyı oluşturun. Örnek:

     ```
     oframe@oframe7: cd /opt/tmaxapp/OpenFrame/util/ 
     oframe@oframe7home/oframe7/OpenFrame/util :  ln -s DFSORT SORT
     ```

9. Komutu çalıştırarak ProSort `prosort -h` yüklemesini doğrulayın. Örnek:

     ```
     oframe@oframe7: prosort -h

     Usage: prosort [options] [sort script files]
     options ------
     -h             Display this information 
     -v             Display version information 
     -s             Display state information 
     -j             Display profile information 
     -x             Use SyncSort compatible mode
     ```

## <a name="install-ofcobol"></a>OFCoBOL'u kurun

OFCOBOL, ana bilgisayarın COBOL programlarını yorumlayan OpenFrame derleyicisidir. 

**OFCOBOL yüklemek için**

1. Toplu İşlem/Çevrimiçi yüklemenin başarılı olduğundan emin olun,\_ardından\_OpenFrame COBOL3\_0 40\_Linux\_x86\_64.bin yükleyici dosyasının mevcut olduğunu doğrulayın.

2. OFCOBOL yükleyicisini çalıştırmak için komut isteminde yazın:

     ```
      ./OpenFrame\_COBOL3\_0\_40\_Linux\_x86\_64.bin
     ```

3. Lisans sözleşmesini okuyun ve devam etmek için Enter tuşuna basın.

4. Lisans sözleşmesini kabul edin. Yükleme tamamlandığında aşağıdakiler görüntülenir:

     ```
     Choose Install Folder 
     --------------------
     Where would you like to install?
     Default Install Folder: /home/oframe7/OFCOBOL

     ENTER AN ABSOLUTE PATH, OR PRESS <ENTER> TO ACCEPT THE DEFAULT : /opt/tmaxapp/OFCOBOL

     INSTALL FOLDER IS: /opt/tmaxapp/OFCOBOL 
     IS THIS CORRECT? (Y/N): Y[oframe7@ofdemo ~]$ vi .bash_profile

     ============================================================================ Installing... 
     ------------
     [==================|==================|==================|==================] 
     [------------------|------------------|------------------|------------------]

     =============================================================================== Installation Complete 
     --------------------
     Congratulations. OpenFrame_COBOL has been successfully installed
     PRESS <ENTER> TO EXIT THE INSTALLER
     ```

5. Bash profilini vi (`vi .bash_profile`) olarak açın ve OFCOBOL değişkenleriyle güncelleştirilmelerini doğrulayın.
6. Bash profilini yürütün. Komut istemine şunları yazın:

     ```
      source ~/.bash_profile
     ```

7. OFCOBOL lisansını yüklenen klasöre kopyalayın. Örnek:
     ```
     mv licofcob.dat $OFCOB_HOME/license
     ```
8. OpenFrame tjclrun.conf yapılandırma dosyasına gidin ve vi'de açın. Örnek:
     ```
     [oframe7@ofdemo ~]$ cd $OPENFRAME_HOME/config 
     [oframe7@ofdemo ~]$ vi tjclrun.conf
     ```

   Değişiklikten önceki SYSLIB bölümü aşağıda veda edebilirsiniz:
     ```
     [SYSLIB] BIN_PATH=${OPENFRAME_HOME}/bin:${OPENFRAME_HOME}/util:${COBDIR}/bin:/usr/local/bin:/bin LIB_PATH=${OPENFRAME_HOME}/lib:${OPENFRAME_HOME}/core/lib:${TB_HOME}/client/lib:${COBDIR}/lib:/ usr/lib:/lib:/lib/i686:/usr/local/lib:${PROSORT_HOME}/lib:/opt/FSUNbsort/lib
     ```
   İşte değişiklikten sonraki SYSLIB bölümü:
     ```
     [SYSLIB] BIN_PATH=${OPENFRAME_HOME}/bin:${OPENFRAME_HOME}/util:${COBDIR}/bin:/usr/local/bin:/bin LIB_PATH=${OPENFRAME_HOME}/lib:${OPENFRAME_HOME}/core/lib:${TB_HOME}/client/lib:${COBDIR}/lib:/ usr/lib:/lib:/lib/i686:/usr/local/lib:${PROSORT_HOME}/lib:/opt/FSUNbsort/lib :${ODBC_HOME}/lib 
     :${OFCOB_HOME}/lib
     ```
9. OpenFrame\_COBOL\_InstallLog.log dosyasını vi'de inceleyin ve hata olmadığını doğrulayın. Örnek:
     ```
     [oframe7@ofdemo ~]$ vi $OFCOB_HOME/UninstallerData/log/OpenFrame_COBOL_InstallLog.log 
     …….. 
     Summary 
     ------
     Installation: Successful. 
     131 Successes 
     0 Warnings 
     0 NonFatalErrors 
     0 FatalError
     ```
10. Yüklemeyi `ofcob --version` doğrulamak için komutu kullanın ve sürüm numarasını gözden geçirin. Örnek:

     ```
     [oframe7@ofdemo ~]$ ofcob --version 
     OpenFrame COBOL Compiler 3.0.54 
     CommitTag:: 645f3f6bf7fbe1c366a6557c55b96c48454f4bf
     ```

11. Komutu kullanarak OpenFrame'i yeniden başlatın. `tmdown/tmboot`

## <a name="install-ofasm"></a>OFASM'yi kurun

OFASM, ana bilgisayarın assembler programlarını yorumlayan OpenFrame derleyicisidir.

**OFASM'yi yüklemek için**

1. Toplu İşlem/Çevrimiçi yüklemenin başarılı olduğundan emin olun\_ve ardından\_\_OpenFrame ASM3 0 Linux\_x86\_64.bin yükleyici dosyasının mevcut olduğunu doğrulayın.

2. Yükleyiciyi çalıştırın. Örnek:

     ```
     [oframe7@ofdemo ~]$ ./OpenFrame_ASM3_0_Linux_x86_64.bin
     ```

3. Lisans sözleşmesini okuyun ve devam etmek için Enter tuşuna basın.
4. Lisans sözleşmesini kabul edin.
5. Bash profilinin OFASM değişkenleriyle güncelleştirileni doğrulayın. Örnek:

     ```
     [oframe7@ofdemo ~]$ source .bash_profile
     [oframe7@ofdemo ~]$ ofasm --version 
     # TmaxSoft OpenFrameAssembler v3 r328 
     (3ff35168d34f6e2046b96415bbe374160fcb3a34)

     [oframe7@ofdemo OFASM]$ vi .bash_profile

     # OFASM ENV 
     export OFASM_HOME=/opt/tmaxapp/OFASM 
     export OFASM_MACLIB=$OFASM_HOME/maclib/free_macro 
     export PATH="${PATH}:$OFASM_HOME/bin:" 
     export LD_LIBRARY_PATH="./:$OFASM_HOME/lib:$LD_LIBRARY_PATH"
     ```

6. OpenFrame tjclrun.conf yapılandırma dosyasını vi olarak açın ve aşağıdaki gibi düzenleme:

     ```
     [oframe7@ofdemo ~]$ cd $OPENFRAME_HOME/config 
     [oframe7@ofdemo ~]$ vi tjclrun.conf
     ```

     Değişiklikten *önceki* [SYSLIB] bölümü aşağıda veda edebilirsiniz:

     ```
     [SYSLIB] BIN_PATH=${OPENFRAME_HOME}/bin:${OPENFRAME_HOME}/util:${COBDIR}/bin:/usr/local/bin:/bi n:${OPENFRAME_HOME}/volume_default/SYS1.LOADLIB LIB_PATH=${OPENFRAME_HOME}/lib:${OPENFRAME_HOME}/core/lib:${TB_HOME}/client/lib:${CO BDIR}/lib:/usr/lib:/lib:/lib/i686:/usr/local/lib:${PROSORT_HOME}/lib:/opt/FSUNbsort/lib:${OFCOB_HOM E}/lib:${ODBC_HOME}/lib:${OFPLI_HOME}/lib
     ```

     Değişiklikten *sonraki* [SYSLIB] bölümü aşağıda veda edebilirsiniz:

     ```
     [SYSLIB] BIN_PATH=${OPENFRAME_HOME}/bin:${OPENFRAME_HOME}/util:${COBDIR}/bin:/usr/local/bin:/bi n:${OPENFRAME_HOME}/volume_default/SYS1.LOADLIB LIB_PATH=${OPENFRAME_HOME}/lib:${OPENFRAME_HOME}/core/lib:${TB_HOME}/client/lib:${CO BDIR}/lib:/usr/lib:/lib:/lib/i686:/usr/local/lib:${PROSORT_HOME}/lib:/opt/FSUNbsort/lib:${OFCOB_HOM E}/lib:${ODBC_HOME}/lib:${OFPLI_HOME}/lib:${OFASM_HOME}/lib
     ```

7. OpenFrame\_ASM\_InstallLog.log dosyasını vi'de açın ve hata olmadığını doğrulayın. Örnek:

     ```
     [oframe7@ofdemo ~]$ vi 
     $OFASM_HOME/UninstallerData/log/OpenFrame_ASM_InstallLog.log 
     …….. 
     Summary 
     ------

     Installation: Successful.

     55 Successes 
     0 Warnings 
     0 NonFatalErrors 
     0 FatalErrors
     ```

8. Aşağıdaki komutlardan birini vererek OpenFrame'i yeniden başlatın:

     ```
     tmdown / tmboot
     ```

     —veya—

     ```
     oscdown / oscboot
     ```

## <a name="install-osc"></a>OSC'yi yükleyin

OSC, yüksek hızlı OLTP işlemlerini ve diğer yönetim işlevlerini destekleyen IBM CICS'e benzer OpenFrame ortamıdır.

**OSC'yi yüklemek için**

1. Temel yüklemenin başarılı olduğundan emin olun,\_ardından OpenFrame OSC7\_\_0 Fix2\_Linux\_x86\_64.bin yükleyici dosyası nın ve osc.properties yapılandırma dosyasının mevcut olduğunu doğrulayın.
2. OSC.Properties dosyasında aşağıdaki parametreleri edin:
     ```
     OPENFRAME_HOME=/opt/tmaxapp/OpenFrame OSC_SYS_OSC_NCS_PATH=/opt/tmaxapp/OpenFrame/temp/OSC_NCS OSC_APP_OSC_TC_PATH=/opt/tmaxapp/OpenFrame/temp/OSC_TC
     ```

3. Gösterildiği gibi özellikleri dosyasını kullanarak yükleyici çalıştırın:

     ```
     [oframe7@ofdemo ~]$ chmod a+x OpenFrame_OSC7_0_Fix2_Linux_x86_64.bin [oframe7@ofdemo ~]$ ./OpenFrame_OSC7_0_Fix2_Linux_x86_64.bin -f osc.properties
     ```

     Tamamlandığında, "Yükleme Tamamlandı" iletisi görüntülenir.

4. Bash profilinin OSC değişkenleriyle güncelleştirileni doğrulayın.
5. OpenFrame\_OSC7\_0\_Fix2\_InstallLog.log dosyasını inceleyin. Şuna benzer şekilde görünecektir:

     ```
     Summary 
     ------ 
     Installation: Successful.

     233 Successes
     0 Warnings
     0 NonFatalErrors
     0 FatalError
     ```

6. ofsys.seq yapılandırma dosyasını açmak için vi'yi kullanın. Örnek:

     ```
     vi $OPENFRAME_HOME/config/ofsys.seq
     ```

7. \#BASE ve \#BATCH bölümlerinde, parametreleri gösterildiği gibi edin.

     ```
     Before changes
     #BASE
     ofrsasvr
     ofrlhsvr
     ofrdmsvr
     ofrdsedt
     ofrcmsvr
     ofruisvr
     ofrsmlog
     vtammgr
     TPFMAGENT

     #BATCH 
     #BATCH#obmtsmgr
     #BATCH#ofrpmsvr
     #BATCH#obmjmsvr
     #BATCH#obmjschd
     #BATCH#obmjinit
     #BATCH#obmjhist
     #BATCH#obmjspbk
     #TACF #TACF#tmsvr

     After changes  #BATCH
     #BASE          obmtsmgr 
     ofrsasvr       ofrpmsvr
     ofrlhsvr       obmjmsvr
     ofrdmsvr       obmjschd
     ofrdsedt       obmjinit
     ofrcmsvr       obmjhist
     ofruisvr       obmjspbk
     ofrsmlog
     vtammgr        #TACF
     TPFMAGENT      tmsvr
    ```

8. Lisans dosyasını kopyalayın. Örnek:

     ```
     [oframe7@ofdemo ~]$ cp /home/oframe7/oflicense/ofonline/licosc.dat 

     $OPENFRAME_HOME/license

     [oframe7@ofdemo ~]$ cd $OPENFRAME_HOME/license 
     oframe@oframe7/OpenFrame/license / ls -l 
     -rwxr-xr-x. 1 oframe mqm 80 Sep 12 01:37 licosc.dat 
     -rwxr-xr-x. 1 oframe mqm 80 Sep  8 09:40 lictacf.dat 
     -rwxrwxr-x. 1 oframe mqm 80 Sep  3 11:54 lictjes.da
     ```

9. OSC'yi başlatmak ve kapatmak için, komut istemine `osctdlinit OSCOIVP1` yazarak CICS paylaşılan belleği başlatın.

10. OSC'yi başlatmak için çalıştırın. `oscboot` Çıktı şuna benzer:

     ```
     OSCBOOT : pre-processing       [ OK ]

     TMBOOT for node(NODE1) is starting: 
     Welcome to Tmax demo system: it will expire 2016/11/4 
     Today: 2016/9/12 
          TMBOOT: TMM is starting: Mon Sep 12 01:40:25 2016 
          TMBOOT: CLL is starting: Mon Sep 12 01:40:25 2016 
          TMBOOT: CLH is starting: Mon Sep 12 01:40:25 2016 
          TMBOOT: TLM(tlm) is starting: Mon Sep 12 01:40:25 2016 
     ```

11. İşlem durumunun hazır olduğunu doğrulamak `tmadmin` için si'deki komutu kullanın. Tüm işlemler **durum** sütununda RDY görüntülenmelidir.

    ![RDY'yi görüntüleyen işlemler](media/tmadmin-02.png)

12. Komutu kullanarak OSC'yi `oscdown` kapatın.

## <a name="install-jeus"></a>JEUS'u yükle

JEUS (Java Enterprise User Solution) OpenFrame web uygulama sunucusunun sunum katmanını sağlar.

JEUS'u yüklemeden önce, JEUS'u yüklemek için gereken kütüphaneleri ve komut satırı araçlarını sağlayan Apache Ant paketini yükleyin.

**Apache Ant yüklemek için**

1. Komutunu kullanarak `wget` Karınca ikiliindir. Örnek:

     ```
     wget http://apache.mirror.cdnetworks.com/ant/binaries/apacheant-1.9.7-bin.tar.gz
     ```

2. İkili `tar` dosyayı ayıklamak ve uygun bir konuma taşımak için yardımcı programı kullanın. Örnek:

     ```
     tar -xvzf apache-ant-1.9.7-bin.tar.gz
     ```

3. Verimlilik için sembolik bir bağlantı oluşturun:

     ```
     ln -s apache-ant-1.9.7 ant
     ```

4. Bash profilini vi (`vi .bash_profile`) olarak açın ve aşağıdaki değişkenlerle güncelleyin:

     ```
     # Ant ENV
     export ANT_HOME=$HOME/ant 
     export PATH=$HOME/ant/bin:$PATH
     ```

5.  Değiştirilen ortam değişkenini uygulayın. Örnek:

     ```
     [oframe7\@ofdemo \~]\$ source \~/.bash\_profile
     ```

**JEUS'u yüklemek için**

1. Yardımcı programı kullanarak `tar` yükleyiciyi genişletin. Örnek:

     ```
     [oframe7@ofdemo ~]$ tar -zxvf jeus704.tar.gz
     ```

2. Bir **jeus** klasörü oluşturun (`mkdir jeus7`) ve ikili unzip.
3. **Kurulum** dizinini değiştirin (veya kendi ortamınız için JEUS parametresini kullanın). Örnek:

     ```
     [oframe7@ofdemo ~]$ cd jeus7/setup/
     ```

4. Yapıyı gerçekleştirmeden önce çalıştırın. `ant clean-all` Çıktı şuna benzer:

     ```
     Buildfile: /home/oframe7jeus7/setup/build.xml

     clean-bin:
     delete-domain:
     [echo] Deleting a domain configuration: domain = jeus_domain
     delete-nodesxml:
     clean-config:
     clean-all:
     BUILD SUCCESSFUL
     Total time: 0 seconds
     ```

5.  Etki alanı-config-template.properties dosyasının yedeğini yapın. Örnek:

     ```
     [oframe7@ofdemo ~]$ cp domain-config-template.properties domain-configtemplate.properties.bkp
     ```

6. Etki alanı-config-template.properties dosyasını vi'de açın:

     ```
     [oframe7\@ofdemo setup]\$ vi domain-config-template.properties
     ```

7. Değiştir `jeus.password=jeusadmin nodename=Tmaxsoft``jeus.password=tmax1234 nodename=ofdemo`

8. JEUS `ant install` oluşturmak için komutu uygulayın.
9.  .bash\_profil dosyasını JEUS değişkenleriyle birlikte gösterildiği gibi güncelleştirin:

     ```
     # JEUS ENV 
     export JEUS_HOME=/opt/tmaxui/jeus7 PATH="/opt/tmaxui/jeus7/bin:/opt/tmaxui/jeus7/lib/system:/opt/tmaxui/jeus7/webserver/bin:$ {PATH}" 
     export PATH
     ```

10. Bash profilini yürütün. Örnek:

     ```
     [oframe7@ofdemo setup]$ . .bash_profile
     ```

11. *İsteğe bağlı*. JETüs bileşenlerinin kolay kapatılması ve önyüklemesi için bir takma ad oluşturun:

     ```     
     # JEUS alias

     alias dsboot='startDomainAdminServer -domain jeus_domain -u administrator -p jeusadmin'
     alias msboot='startManagedServer -domain jeus_domain -server server1 -u administrator -p jeusadmin' 
     alias msdown=‘jeusadmin -u administrator -p tmax1234 "stop-server server1“’ 
     alias dsdown=‘jeusadmin -domain jeus_domain -u administrator -p tmax1234 "local-shutdown“’
     ```

12. Yüklemeyi doğrulamak için etki alanı yöneticisi sunucusunu gösterildiği gibi başlatın:

     ```
     [oframe7@ofdemo ~]$ startDomainAdminServer -domain jeus_domain -u administrator -p jeusadmin
     ```

13. Sözdizimini kullanarak web oturumunuzu doğrulayın:

     ```
     http://<IP>:<port>/webadmin/login
     ```

     Örneğin, <http://192.168.92.133:9736/webadmin/login.> oturum açma ekranı görüntülenir:
    
     ![JEUS WebAdmin oturum açma ekranı](media/jeus-01.png)

     > [!NOTE]
     > Bağlantı noktası güvenliğiyle ilgili herhangi bir sorunla karşılaşırsanız, 9736 portu açın veya güvenlik duvarını devre dışı sökün (`systemctl stop firewall`).

14. server1 için ana bilgisayar adını değiştirmek için, & **Düzenley'i**tıklatın, ardından **server1'i**tıklatın. Sunucu penceresinde, ana bilgisayar adını aşağıdaki gibi değiştirin:

    1.  **Düğüm adını** ofdemo olarak **değiştirin.**
    2.  Pencerenin sağ tarafında **Tamam'ı** tıklatın.
    3.  Pencerenin sol alt tarafındaki **değişiklikleri uygula'yı** tıklatın ve açıklama için *Hostname değişikliği'ni*girin.

    ![JEUS WebAdmin ekranı](media/jeus-02.png)

15. Yapılandırmanın onay ekranında başarılı olduğunu doğrulayın.

    ![jeus_domain Sunucu ekranı](media/jeus-03.png)

16. Yönetilen sunucu işlemini aşağıdaki komutu kullanarak "server1" başlatın:

     ```
     [oframe7@ofdemo ~]$ startManagedServer -domain jeus_domain -server server1 -u administrator -p jeusadmin
     ```

## <a name="install-ofgw"></a>OFGW'yi yükleyin

OFGW, 3270 terminal emülatörü ile OSI tabanı arasındaki iletişimi destekleyen ve terminal emülatörü ile OSI arasındaki oturumları yöneten OpenFrame ağ geçididir.

**OFGW yüklemek için**

1. JEUS'un başarıyla yüklendiğinden emin olun, ardından\_\_OFGW7 0 1\_Generic.bin yükleyici dosyasının mevcut olduğunu doğrulayın.
2. Yükleyiciyi çalıştırın. Örnek:

     ```
     [oframe7@ofdemo ~]$ ./OFGW7_0_1_Generic.bin
     ````

3. İlgili istemler için aşağıdaki konumları kullanın:
     -   JEUS Ev dizini
     -   JEUS Alan Adı
     -   JEUS Sunucu Adı
     -   Tibero Sürücü
     -   Tmax Düğümü ofdemo Kimliği

4. Varsayılanların geri kalanını kabul edin ve yükleyiciden çıkmak için Enter tuşuna basın.

5. OFGW URL'sinin beklendiği gibi çalıştığını doğrulayın:

     ```
     Type URL 
     http://192.168.92.133:8088/webterminal/ and press enter
      < IP >               :8088/webterminal/
     ```

     Aşağıdaki ekran görüntülenir:

    ![OpenFrame WebTerminal](media/ofgw-01.png)

## <a name="install-ofmanager"></a>OFManager'ı Yükleyin

OFManager, OpenFrame için web ortamında çalışma ve yönetim işlevleri sağlar.

**OFManager'ı yüklemek için**

1. OFManager7\_Generic.bin yükleyici dosyasının mevcut olduğunu doğrulayın.
2. Yükleyiciyi çalıştırın. Örnek:

     ```
     OFManager7_Generic.bin
     ```

3.  Devam etmek için Enter tuşuna basın, ardından lisans sözleşmesini kabul edin.
4.  Yükle klasörünü seçin.
5.  Varsayılanları kabul edin.
6.  Veritabanı olarak Tibero'yu seçin.
7.  Yükleyiciden çıkmak için Enter tuşuna basın.
8.  OFManager url'sinin beklendiği gibi çalıştığını doğrulayın:

     ```
     Type URL http://192.168.92.133:8088/ofmanager and press enter <  IP >  : < PORT >  ofmanager Enter ID:   ROOT 
     Password: SYS1
     ```

Başlangıç ekranı görüntülenir:

![Tmax OpenFrame Manager oturum açma ekranı](media/ofmanager-01.png)

Bu, OpenFrame bileşenlerinin yüklenmesini tamamlar.

## <a name="next-steps"></a>Sonraki adımlar

Bir ana bilgisayar göçü düşünüyorsanız, genişleyen iş ortağı ekosistemimiz size yardımcı olabilir. Ortak çözüm seçimi hakkında ayrıntılı rehberlik için [Platform Modernizasyon](https://datamigration.microsoft.com/)İttifakı'na bakın.

-   [Azure’u kullanmaya başlama](https://docs.microsoft.com/azure/)
-   [Ana Bilgisayar Tümleştirme Sunucusu (HIS) belgeleri](https://docs.microsoft.com/host-integration-server/)
-   [Azure Sanal Veri Merkezi Kaldırma ve Kaydırma Kılavuzu](https://blogs.msdn.microsoft.com/azurecat/2018/03/12/new-whitepaper-azure-virtual-datacenter-lift-and-shift-guide/)
