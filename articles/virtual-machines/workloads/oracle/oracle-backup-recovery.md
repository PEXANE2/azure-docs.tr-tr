---
title: Bir Azure Linux sanal makinesinde Oracle Database 12c veritabanını yedekleyin ve kurtarın | Microsoft Dokümanlar
description: Azure ortamınızda bir Oracle Database 12c veritabanını nasıl yedekleyip kurtarılamayı öğrenin.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: romitgirdhar
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/02/2018
ms.author: rogirdh
ms.openlocfilehash: bae7e53a316fa6ca3158639cc551a0a3de5cb952
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79536930"
---
# <a name="back-up-and-recover-an-oracle-database-12c-database-on-an-azure-linux-virtual-machine"></a>Azure Linux sanal makinesinde Oracle Database 12c veritabanını yedekleyin ve kurtarın

Azure kaynaklarını komut isteminde oluşturmak ve yönetmek veya komut dosyalarını kullanmak için Azure CLI'yi kullanabilirsiniz. Bu makalede, Bir Azure Marketi galerisi görüntüsünden bir Oracle Database 12c veritabanı dağıtmak için Azure CLI komut dosyalarını kullanıyoruz.

Başlamadan önce Azure CLI'nin yüklü olduğundan emin olun. Daha fazla bilgi için [Azure CLI yükleme kılavuzuna](https://docs.microsoft.com/cli/azure/install-azure-cli)bakın.

## <a name="prepare-the-environment"></a>Ortamı hazırlama

### <a name="step-1-prerequisites"></a>Adım 1: Ön koşullar

*   Yedekleme ve kurtarma işlemini gerçekleştirmek için öncelikle Oracle Database 12c'nin yüklü bir örneğine sahip bir Linux VM oluşturmanız gerekir. VM oluşturmak için kullandığınız Market görüntüsünün adı *Oracle:Oracle-Database-Ee:12.1.0.2:latest*.

    Oracle veritabanı oluşturmayı öğrenmek için [Oracle veritabanını hızlı başlatma oluşturma](https://docs.microsoft.com/azure/virtual-machines/workloads/oracle/oracle-database-quick-create)bilgisini edinin.


### <a name="step-2-connect-to-the-vm"></a>Adım 2: VM'ye bağlanın

*   VM ile Güvenli Kabuk (SSH) oturumu oluşturmak için aşağıdaki komutu kullanın. IP adresini ve ana bilgisayar ad `publicIpAddress` birleşimini VM'nizin değeriyle değiştirin.

    ```bash
    ssh <publicIpAddress>
    ```

### <a name="step-3-prepare-the-database"></a>Adım 3: Veritabanını hazırlama

1.  Bu adım, *myVM*adlı bir VM üzerinde çalışan bir Oracle örneğinin (cdb1) olduğunu varsayar.

    *Oracle* superuser kökünü çalıştırın ve dinleyiciyi çalıştırın:

    ```bash
    $ sudo su - oracle
    $ lsnrctl start
    Copyright (c) 1991, 2014, Oracle.  All rights reserved.

    Starting /u01/app/oracle/product/12.1.0/dbhome_1/bin/tnslsnr: please wait...

    TNSLSNR for Linux: Version 12.1.0.2.0 - Production
    Log messages written to /u01/app/oracle/diag/tnslsnr/myVM/listener/alert/log.xml
    Listening on: (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=myVM.twltkue3xvsujaz1bvlrhfuiwf.dx.internal.cloudapp.net)(PORT=1521)))

    Connecting to (ADDRESS=(PROTOCOL=tcp)(HOST=)(PORT=1521))
    STATUS of the LISTENER
    ------------------------
    Alias                     LISTENER
    Version                   TNSLSNR for Linux: Version 12.1.0.2.0 - Production
    Start Date                23-MAR-2017 15:32:08
    Uptime                    0 days 0 hr. 0 min. 0 sec
    Trace Level               off
    Security                  ON: Local OS Authentication
    SNMP                      OFF
    Listener Log File         /u01/app/oracle/diag/tnslsnr/myVM/listener/alert/log.xml
    Listening Endpoints Summary...
    (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=myVM.twltkue3xvsujaz1bvlrhfuiwf.dx.internal.cloudapp.net)(PORT=1521)))
    The listener supports no services
    The command completed successfully
    ```

2.  (İsteğe bağlı) Veritabanının arşiv günlüğü modunda olduğundan emin olun:

    ```bash
    $ sqlplus / as sysdba
    SQL> SELECT log_mode FROM v$database;

    LOG_MODE
    ------------
    NOARCHIVELOG

    SQL> SHUTDOWN IMMEDIATE;
    SQL> STARTUP MOUNT;
    SQL> ALTER DATABASE ARCHIVELOG;
    SQL> ALTER DATABASE OPEN;
    SQL> ALTER SYSTEM SWITCH LOGFILE;
    ```

3.  (İsteğe bağlı) Commit test etmek için bir tablo oluşturun:

    ```bash
    SQL> alter session set "_ORACLE_SCRIPT"=true ;
    Session altered.
    SQL> create user scott identified by tiger;
    User created.
    SQL> grant create session to scott;
    Grant succeeded.
    SQL> grant create table to scott;
    Grant succeeded.
    SQL> alter user scott quota 100M on users;
    User altered.
    SQL> connect scott/tiger
    SQL> create table scott_table(col1 number, col2 varchar2(50));
    Table created.
    SQL> insert into scott_Table VALUES(1,'Line 1');
    1 row created.
    SQL> commit;
    Commit complete.
    ```

4.  Yedekleme dosyası nın konumunu ve boyutunu doğrulayın veya değiştirin:

    ```bash
    $ sqlplus / as sysdba
    SQL> show parameter db_recovery
    NAME                                 TYPE        VALUE
    ------------------------------------ ----------- ------------------------------
    db_recovery_file_dest                string      /u01/app/oracle/fast_recovery_area
    db_recovery_file_dest_size           big integer 4560M
    ```

5. Veritabanını yedeklemek için Oracle Recovery Manager'ı (RMAN) kullanın:

    ```bash
    $ rman target /
    RMAN> backup database plus archivelog;
    ```

### <a name="step-4-application-consistent-backup-for-linux-vms"></a>Adım 4: Linux VM'leri için uygulama tutarlı yedekleme

Uygulama tutarlı yedeklemeleri Azure Yedekleme'de yeni bir özelliktir. VM anlık görüntüsünden önce ve sonra yürütmek için komut dosyaları oluşturabilir ve seçebilirsiniz (anlık görüntü öncesi ve anlık görüntü sonrası).

1. JSON dosyasını indirin.

    Karşıdan yükleme VMSnapshotScriptPluginConfig.json . https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig Dosya içeriği aşağıdakilere benzer:

    ```output
    {
        "pluginName" : "ScriptRunner",
        "preScriptLocation" : "",
        "postScriptLocation" : "",
        "preScriptParams" : ["", ""],
        "postScriptParams" : ["", ""],
        "preScriptNoOfRetries" : 0,
        "postScriptNoOfRetries" : 0,
        "timeoutInSeconds" : 30,
        "continueBackupOnFailure" : true,
        "fsFreezeEnabled" : true
    }
    ```

2. VM'de /etc/azure klasörünü oluşturun:

    ```bash
    $ sudo su -
    # mkdir /etc/azure
    # cd /etc/azure
    ```

3. JSON dosyasını kopyalayın.

    VMSnapshotScriptPluginConfig.json'u /etc/azure klasörüne kopyalayın.

4. JSON dosyasını edin.

    VMSnapshotScriptPluginConfig.json dosyasını ve `PreScriptLocation` `PostScriptlocation` parametreleri içerecek şekilde edin. Örnek:

    ```output
    {
        "pluginName" : "ScriptRunner",
        "preScriptLocation" : "/etc/azure/pre_script.sh",
        "postScriptLocation" : "/etc/azure/post_script.sh",
        "preScriptParams" : ["", ""],
        "postScriptParams" : ["", ""],
        "preScriptNoOfRetries" : 0,
        "postScriptNoOfRetries" : 0,
        "timeoutInSeconds" : 30,
        "continueBackupOnFailure" : true,
        "fsFreezeEnabled" : true
    }
    ```

5. Anlık görüntü öncesi ve anlık görüntü sonrası komut dosyası dosyalarını oluşturun.

    Aşağıda, "soğuk yedekleme" (kapatma ve yeniden başlatma ile çevrimdışı yedekleme) için anlık görüntü öncesi ve anlık görüntü sonrası komut dosyalarının bir örneği verilmiştir:

    /etc/azure/pre_script.sh için:

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "$ORA_HOME/bin/dbshut $ORA_HOME" > /etc/azure/pre_script_$v_date.log
    ```

    /etc/azure/post_script.sh için:

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "$ORA_HOME/bin/dbstart $ORA_HOME" > /etc/azure/post_script_$v_date.log
    ```

    "Sıcak yedekleme" (çevrimiçi yedekleme) için anlık görüntü öncesi ve anlık görüntü sonrası komut dosyalarının bir örneği aşağıda verilmiştir:

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "sqlplus / as sysdba @/etc/azure/pre_script.sql" > /etc/azure/pre_script_$v_date.log
    ```

    /etc/azure/post_script.sh için:

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "sqlplus / as sysdba @/etc/azure/post_script.sql" > /etc/azure/pre_script_$v_date.log
    ```

    /etc/azure/pre_script.sql için, gereksinimlerinize göre dosyanın içeriğini değiştirin:

    ```bash
    alter tablespace SYSTEM begin backup;
    ...
    ...
    alter system switch logfile;
    alter system archive log stop;
    ```

    /etc/azure/post_script.sql için, gereksinimlerinize göre dosyanın içeriğini değiştirin:

    ```bash
    alter tablespace SYSTEM end backup;
    ...
    ...
    alter system archive log start;
    ```

6. Dosya izinlerini değiştirme:

    ```bash
    # chmod 600 /etc/azure/VMSnapshotScriptPluginConfig.json
    # chmod 700 /etc/azure/pre_script.sh
    # chmod 700 /etc/azure/post_script.sh
    ```

7. Komut dosyalarını test edin.

    Komut dosyalarını sınamak için önce kök olarak oturum açın. Ardından, hata olmadığından emin olun:

    ```bash
    # /etc/azure/pre_script.sh
    # /etc/azure/post_script.sh
    ```

Daha fazla bilgi için [Linux VM'leri için Uygulama tutarlı yedeklemesi'ne](https://azure.microsoft.com/blog/announcing-application-consistent-backup-for-linux-vms-using-azure-backup/)bakın.


### <a name="step-5-use-azure-recovery-services-vaults-to-back-up-the-vm"></a>Adım 5: VM'yi yedeklemek için Azure Kurtarma Hizmetleri kasalarını kullanın

1.  Azure portalında Kurtarma **Hizmetleri kasalarını**arayın.

    ![Kurtarma Hizmetleri kasaları sayfası](./media/oracle-backup-recovery/recovery_service_01.png)

2.  Kurtarma **Hizmetleri kasaları** bıçak, yeni bir kasa eklemek için, **Ekle'yi**tıklatın.

    ![Kurtarma Hizmetleri kasaları sayfa eklemek](./media/oracle-backup-recovery/recovery_service_02.png)

3.  Devam etmek için **myVault'u**tıklatın.

    ![Kurtarma Hizmetleri kasaları ayrıntı sayfası](./media/oracle-backup-recovery/recovery_service_03.png)

4.  **myVault** bıçağında **Yedekleme'yi**tıklatın.

    ![Kurtarma Hizmetleri vaults yedekleme sayfası](./media/oracle-backup-recovery/recovery_service_04.png)

5.  Yedekleme **Hedefi** bıçağında Azure **ve** Sanal **makinenin**varsayılan değerlerini kullanın. **Tamam**'a tıklayın.

    ![Kurtarma Hizmetleri kasaları ayrıntı sayfası](./media/oracle-backup-recovery/recovery_service_05.png)

6.  **Yedekleme ilkesi**için **DefaultPolicy'yi**kullanın veya **Yeni İlke Oluştur'u**seçin. **Tamam**'a tıklayın.

    ![Kurtarma Hizmetleri vaults yedekleme ilkesi ayrıntı sayfası](./media/oracle-backup-recovery/recovery_service_06.png)

7.  Sanal **makineler** seç bıçağında **myVM1** onay kutusunu seçin ve ardından **Tamam'ı**tıklatın. **Yedeklemeyi Etkinleştir** düğmesini tıklatın.

    ![Kurtarma Hizmetleri öğeleri yedek ayrıntı sayfasına tonozlar](./media/oracle-backup-recovery/recovery_service_07.png)

    > [!IMPORTANT]
    > **Yedeklemeyi Etkinleştir'i**tıklattıktan sonra, yedekleme işlemi zamanlanan süre dolana kadar başlamaz. Hemen yedekleme ayarlamak için bir sonraki adımı tamamlayın.

8.  **myVault - Backup items** blade'de, **BACKUP ITEM COUNT**altında, yedek madde sayısını seçin.

    ![Kurtarma Hizmetleri myVault detay sayfasını tonozlar](./media/oracle-backup-recovery/recovery_service_08.png)

9.  Yedek **Öğeler (Azure Sanal Makine)** bıçağında, sayfanın sağ tarafında, elips (**...**) düğmesini tıklatın ve sonra **şimdi Yedek'i**tıklatın.

    ![Kurtarma Hizmetleri vaults Yedekleme şimdi komut](./media/oracle-backup-recovery/recovery_service_09.png)

10. **Yedekleme** düğmesini tıklatın. Yedekleme işleminin tamamlanmasını bekleyin. Ardından, [Adım 6'ya gidin: Veritabanı dosyalarını kaldırın.](#step-6-remove-the-database-files)

    Yedekleme işinin durumunu görüntülemek için **İşler'i**tıklatın.

    ![Kurtarma Hizmetleri kasaları iş sayfası](./media/oracle-backup-recovery/recovery_service_10.png)

    Yedekleme işinin durumu aşağıdaki resimde görünür:

    ![Kurtarma Hizmetleri, durumla birlikte iş sayfasını kasaya](./media/oracle-backup-recovery/recovery_service_11.png)

11. Uygulama tutarlı yedekleme için, günlük dosyasındaki hataları giderin. Günlük dosyası /var/log/azure/Microsoft.Azure.RecoveryServices.VMSnapshotLinux/1.0.9114.0 adresinde bulunur.

### <a name="step-6-remove-the-database-files"></a>Adım 6: Veritabanı dosyalarını kaldırma 
Bu makalenin ilerleyen saatlerinde, kurtarma işlemini nasıl sınadığınızı öğreneceksiniz. Kurtarma işlemini sınamak için önce veritabanı dosyalarını kaldırmanız gerekir.

1.  Tablo alanı ve yedekleme dosyalarını kaldırın:

    ```bash
    $ sudo su - oracle
    $ cd /u01/app/oracle/oradata/cdb1
    $ rm -f *.dbf
    $ cd /u01/app/oracle/fast_recovery_area/CDB1/backupset
    $ rm -rf *
    ```
    
2.  (İsteğe bağlı) Oracle örneğini kapatın:

    ```bash
    $ sqlplus / as sysdba
    SQL> shutdown abort
    ORACLE instance shut down.
    ```

## <a name="restore-the-deleted-files-from-the-recovery-services-vaults"></a>Kurtarma Hizmetleri kasalarından silinen dosyaları geri yükleme
Silinen dosyaları geri yüklemek için aşağıdaki adımları tamamlayın:

1. Azure portalında *myVault* Kurtarma Hizmetleri kasaları öğesini arayın. Genel **Bakış** bıçağında, **Yedekleme öğelerinin**altında, öğe sayısını seçin.

    ![Kurtarma Hizmetleri myVault yedekleme öğeleritonolar](./media/oracle-backup-recovery/recovery_service_12.png)

2. **BACKUP ITEM COUNT**altında, öğe sayısını seçin.

    ![Kurtarma Hizmetleri kasaları Azure Virtual Machine yedek madde sayısı](./media/oracle-backup-recovery/recovery_service_13.png)

3. **myvm1** bıçağında **Dosya Kurtarma (Önizleme) seçeneğini**tıklatın.

    ![Kurtarma Hizmetleri kasaları dosya kurtarma sayfasının ekran görüntüsü](./media/oracle-backup-recovery/recovery_service_14.png)

4. Dosya **Kurtarma (Önizleme)** bölmesine, **Script'i İndir'i**tıklatın. Ardından, karşıdan yükleme (.sh) dosyasını istemci bilgisayardaki bir klasöre kaydedin.

    ![Komut dosyası dosyasını indirseçenekleri kaydeder](./media/oracle-backup-recovery/recovery_service_15.png)

5. .sh dosyasını VM'ye kopyalayın.

    Aşağıdaki örnek, dosyayı VM'ye taşımak için güvenli bir kopya (scp) komutunu nasıl kullanacağınızı gösterir. Ayrıca içindekileri panoya kopyalayabilir ve içeriği VM'de ayarlanmış yeni bir dosyaya yapıştırabilirsiniz.

    > [!IMPORTANT]
    > Aşağıdaki örnekte, IP adresi ve klasör değerlerini güncelleştirdiğinden emin olun. Değerler, dosyanın kaydedildiği klasöre eşlenmelidir.

    ```bash
    $ scp Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh <publicIpAddress>:/<folder>
    ```

6. Dosyayı değiştirin, böylece köke ait olur.

    Aşağıdaki örnekte, dosyayı köke ait olacak şekilde değiştirin. Ardından, izinleri değiştirin.

    ```bash 
    $ ssh <publicIpAddress>
    $ sudo su -
    # chown root:root /<folder>/Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh
    # chmod 755 /<folder>/Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh
    # /<folder>/Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh
    ```

    Aşağıdaki örnek, önceki komut dosyasını çalıştırdıktan sonra görmeniz gerekenleri gösterir. Devam etmek istendiğinde **Y**girin.

    ```output
    Microsoft Azure VM Backup - File Recovery
    ______________________________________________
    The script requires 'open-iscsi' and 'lshw' to run.
    Do you want us to install 'open-iscsi' and 'lshw' on this machine?
    Please press 'Y' to continue with installation, 'N' to abort the operation. : Y
    Installing 'open-iscsi'....
    Installing 'lshw'....

    Connecting to recovery point using ISCSI service...

    Connection succeeded!

    Please wait while we attach volumes of the recovery point to this machine...

    ************ Volumes of the recovery point and their mount paths on this machine ************

    Sr.No.  |  Disk  |  Volume  |  MountPath

    1)  | /dev/sde  |  /dev/sde1  |  /root/myVM-20170517093913/Volume1

    2)  | /dev/sde  |  /dev/sde2  |  /root/myVM-20170517093913/Volume2

    ************ Open File Explorer to browse for files. ************

    After recovery, to remove the disks and close the connection to the recovery point, please click 'Unmount Disks' in step 3 of the portal.

    Please enter 'q/Q' to exit...
    ```

7. Monte edilen birimlere erişim onaylanır.

    Çıkmak için **q**girin ve sonra monte edilen birimleri arayın. Eklenen birimlerin listesini oluşturmak için komut istemine **df -k**girin.

    ![df -k komutu](./media/oracle-backup-recovery/recovery_service_16.png)

8. Eksik dosyaları klasörlere kopyalamak için aşağıdaki komut dosyasını kullanın:

    ```bash
    # cd /root/myVM-2017XXXXXXX/Volume2/u01/app/oracle/fast_recovery_area/CDB1/backupset/2017_xx_xx
    # cp *.bkp /u01/app/oracle/fast_recovery_area/CDB1/backupset/2017_xx_xx
    # cd /u01/app/oracle/fast_recovery_area/CDB1/backupset/2017_xx_xx
    # chown oracle:oinstall *.bkp
    # cd /root/myVM-2017XXXXXXX/Volume2/u01/app/oracle/oradata/cdb1
    # cp *.dbf /u01/app/oracle/oradata/cdb1
    # cd /u01/app/oracle/oradata/cdb1
    # chown oracle:oinstall *.dbf
    ```

9. Aşağıdaki komut dosyasında, veritabanını kurtarmak için RMAN'ı kullanın:

    ```bash
    # sudo su - oracle
    $ rman target /
    RMAN> startup mount;
    RMAN> restore database;
    RMAN> recover database;
    RMAN> alter database open resetlogs;
    RMAN> SELECT * FROM scott.scott_table;
    ```

10. Diski çıkarın.

    Azure portalında, **Dosya Kurtarma (Önizleme)** bıçak üzerinde, **Diskleri Aşma'yı**tıklatın.

    ![Unmount diskler komutu](./media/oracle-backup-recovery/recovery_service_17.png)

## <a name="restore-the-entire-vm"></a>Tüm VM'yi geri yükleme

Silinen dosyaları Kurtarma Hizmetleri kasalarından geri yüklemek yerine, tüm VM'yi geri yükleyebilirsiniz.

### <a name="step-1-delete-myvm"></a>Adım 1: myVM silme

*   Azure portalında **myVM1** kasasına gidin ve sonra **Sil'i**seçin.

    ![Vault silme komutu](./media/oracle-backup-recovery/recover_vm_01.png)

### <a name="step-2-recover-the-vm"></a>Adım 2: VM'yi kurtarma

1.  Kurtarma **Hizmetleri kasalarına**gidin ve sonra **myVault'u**seçin.

    ![myVault girişi](./media/oracle-backup-recovery/recover_vm_02.png)

2.  Genel **Bakış** bıçağında, **Yedekleme öğelerinin**altında, öğe sayısını seçin.

    ![myVault yedekleme öğeleri](./media/oracle-backup-recovery/recover_vm_03.png)

3.  Yedekleme **Öğeleri (Azure Sanal Makine)** bıçağında **myvm1'i**seçin.

    ![Kurtarma VM sayfası](./media/oracle-backup-recovery/recover_vm_04.png)

4.  **Myvm1** bıçağında elips (**...**) düğmesini tıklatın ve ardından **VM'yi geri yükle'yi**tıklatın.

    ![VM komutunu geri yükle](./media/oracle-backup-recovery/recover_vm_05.png)

5.  Geri **Yükleme noktası** nı seç'te, geri yüklemek istediğiniz öğeyi seçin ve sonra **Tamam'ı**tıklatın.

    ![Geri yükleme noktasını seçin](./media/oracle-backup-recovery/recover_vm_06.png)

    Uygulama tutarlı yedeklemeyi etkinleştirdiyseniz, dikey mavi bir çubuk görüntülenir.

6.  **Yapılandırmayı Geri** Yükle'de, sanal makine adını seçin, kaynak grubunu seçin ve ardından **Tamam'ı**tıklatın.

    ![Yapılandırma değerlerini geri yükleme](./media/oracle-backup-recovery/recover_vm_07.png)

7.  VM'yi geri yüklemek için **Geri Yükle** düğmesini tıklatın.

8.  Geri yükleme işleminin durumunu görüntülemek için **İşler'i**tıklatın ve ardından **Yedek İşler'i**tıklatın.

    ![Yedekleme işleri durum komutu](./media/oracle-backup-recovery/recover_vm_08.png)

    Aşağıdaki şekilde geri yükleme işleminin durumunu gösterir:

    ![Geri yükleme işleminin durumu](./media/oracle-backup-recovery/recover_vm_09.png)

### <a name="step-3-set-the-public-ip-address"></a>Adım 3: Genel IP adresini ayarlama
VM geri yüklendikten sonra, genel IP adresini ayarlayın.

1.  Arama kutusuna **genel IP adresini**girin.

    ![Genel IP adresleri listesi](./media/oracle-backup-recovery/create_ip_00.png)

2.  Ortak **IP adresleri** çubuğunda **Ekle'yi**tıklatın. Ortak **IP adresi oluşturma** bıçağında, **Ad**için ortak IP adını seçin. **Kaynak grubu** olarak **Var olanı kullan**’ı seçin. Ardından **Oluştur'u**tıklatın.

    ![IP adresi oluşturma](./media/oracle-backup-recovery/create_ip_01.png)

3.  Ortak IP adresini VM'nin ağ arabirimiyle ilişkilendirmek için **myVMip'i**arayın ve seçin. Ardından, **Ilişkilendir'i**tıklatın.

    ![Ortak IP adresi](./media/oracle-backup-recovery/create_ip_02.png)

4.  **Kaynak türü için**Ağ **arabirimi'ni**seçin. myVM örneği tarafından kullanılan ağ arabirimini seçin ve ardından **Tamam'ı**tıklatın.

    ![Kaynak türü ve NIC değerlerini seçin](./media/oracle-backup-recovery/create_ip_03.png)

5.  Portaldan taşınan myVM örneğini arayın ve açın. VM ile ilişkili IP adresi myVM Genel **Bakış** bıçağında görünür.

    ![IP adresi değeri](./media/oracle-backup-recovery/create_ip_04.png)

### <a name="step-4-connect-to-the-vm"></a>Adım 4: VM'ye bağlanın

*   VM'ye bağlanmak için aşağıdaki komut dosyasını kullanın:

    ```bash
    ssh <publicIpAddress>
    ```

### <a name="step-5-test-whether-the-database-is-accessible"></a>Adım 5: Veritabanının erişilebilir olup olmadığını test edin
*   Erişilebilirliği sınamak için aşağıdaki komut dosyasını kullanın:

    ```bash
    $ sudo su - oracle
    $ sqlplus / as sysdba
    SQL> startup
    ```

    > [!IMPORTANT]
    > Veritabanı **başlangıç** komutu veritabanını kurtarmak için bir hata oluşturursa, [bkz.](#step-6-optional-use-rman-to-recover-the-database)

### <a name="step-6-optional-use-rman-to-recover-the-database"></a>Adım 6: (İsteğe bağlı) Veritabanını kurtarmak için RMAN'ı kullanın
*   Veritabanını kurtarmak için aşağıdaki komut dosyasını kullanın:

    ```bash
    # sudo su - oracle
    $ rman target /
    RMAN> startup mount;
    RMAN> restore database;
    RMAN> recover database;
    RMAN> alter database open resetlogs;
    RMAN> SELECT * FROM scott.scott_table;
    ```

Azure Linux VM'deki Oracle Database 12c veritabanının yedeklemesi ve kurtarılması tamamlandı.

## <a name="delete-the-vm"></a>VM’yi silin

VM'ye artık ihtiyacınız olmadığında, kaynak grubunu, VM'yi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu kullanabilirsiniz:

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

[Öğretici: Yüksek kullanılabilir VM'ler oluşturun](../../linux/create-cli-complete.md)

[VM dağıtım Azure CLI örneklerini keşfedin](../../linux/cli-samples.md)



