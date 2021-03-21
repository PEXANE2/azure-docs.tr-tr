---
title: Azure VM 'de Oracle veritabanı oluşturma | Microsoft Docs
description: Oracle Database 12c veritabanını hızlıca Azure ortamınızda çalışır duruma alın.
author: dbakevlar
ms.service: virtual-machines
ms.subservice: oracle
ms.collection: linux
ms.topic: quickstart
ms.date: 10/05/2020
ms.author: kegorman
ms.openlocfilehash: 8964248bb23b2b615c7e73e26d730fbd79b4e9e7
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102184466"
---
# <a name="create-an-oracle-database-in-an-azure-vm"></a>Azure VM 'de Oracle Database oluşturma

Bu kılavuzda, bir Oracle 19c veritabanı oluşturmak için [Oracle marketi Galeri görüntüsünden](https://azuremarketplace.microsoft.com/marketplace/apps/Oracle.OracleDatabase12102EnterpriseEdition?tab=Overview) bir Azure sanal makinesi dağıtmak üzere Azure CLI kullanılarak ayrıntıları yer alıyorsunuz. Sunucu dağıtıldıktan sonra, Oracle veritabanını yapılandırmak için SSH aracılığıyla bağlanırsınız. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.4 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. 

Aşağıdaki örnek *eastus* konumunda *RG-Oracle* adlı bir kaynak grubu oluşturur.

```azurecli-interactive
az group create --name rg-oracle --location eastus
```

## <a name="create-virtual-machine"></a>Sanal makine oluşturma

Bir sanal makine (VM) oluşturmak için [az VM Create](/cli/azure/vm) komutunu kullanın. 

Aşağıdaki örnekte `vmoracle19c` adlı bir VM oluşturulur. Varsayılan anahtar konumunda zaten mevcut değilse, SSH anahtarları da oluşturur. Belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın.  

```azurecli-interactive 
az vm create ^
    --resource-group rg-oracle ^
    --name vmoracle19c ^
    --image Oracle:oracle-database-19-3:oracle-database-19-0904:latest ^
    --size Standard_DS2_v2 ^
    --admin-username azureuser ^
    --generate-ssh-keys ^
    --public-ip-address-allocation static ^
    --public-ip-address-dns-name vmoracle19c

```

VM 'yi oluşturduktan sonra, Azure CLı aşağıdaki örneğe benzer bilgiler görüntüler. Değerini aklınızda yapın `publicIpAddress` . Bu adresi sanal makineye erişmek için kullanırsınız.

```output
{
  "fqdns": "",
  "id": "/subscriptions/{snip}/resourceGroups/rg-oracle/providers/Microsoft.Compute/virtualMachines/vmoracle19c",
  "location": "eastus",
  "macAddress": "00-0D-3A-36-2F-56",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "13.64.104.241",
  "resourceGroup": "rg-oracle"
}
```
## <a name="create-and-attach-a-new-disk-for-oracle-datafiles-and-fra"></a>Oracle datafiles ve FRA için yeni disk oluşturma ve iliştirme

```bash
az vm disk attach --name oradata01 --new --resource-group rg-oracle --size-gb 128 --sku StandardSSD_LRS --vm-name vmoracle19c
```

## <a name="open-ports-for-connectivity"></a>Bağlantı noktalarını bağlantı için açma
Bu görevde, VM 'yi koruyan Azure ağ güvenlik grubunu ayarlayarak veritabanı dinleyicisi ve EM Express için bazı dış uç noktaları yapılandırmanız gerekir. 

1. Oracle veritabanına uzaktan erişmek için kullandığınız uç noktayı açmak için aşağıdaki gibi bir ağ güvenlik grubu kuralı oluşturun:
   ```bash
   az network nsg rule create ^
       --resource-group rg-oracle ^
       --nsg-name vmoracle19cNSG ^
       --name allow-oracle ^
       --protocol tcp ^
       --priority 1001 ^
       --destination-port-range 1521
   ```
2. Oracle EM Express 'e uzaktan erişmek için kullandığınız uç noktayı açmak için az Network NSG Rule Create komutuyla aşağıdaki gibi bir ağ güvenlik grubu kuralı oluşturun:
   ```bash
   az network nsg rule create ^
       --resource-group rg-oracle ^
       --nsg-name vmoracle19cNSG ^
       --name allow-oracle-EM ^
       --protocol tcp ^
       --priority 1002 ^
       --destination-port-range 5502
   ```
3. Gerekirse, az Network public-IP Show komutuyla sanal makinenizin genel IP adresini aşağıda gösterildiği gibi edinin:

   ```bash
   az network public-ip show ^
       --resource-group rg-oracle ^
       --name vmoracle19cPublicIP ^
       --query [ipAddress] ^
       --output tsv
   ```

## <a name="prepare-the-vm-environment"></a>VM ortamını hazırlama

1. VM’ye bağlanma

   VM ile bir SSH oturumu oluşturmak için aşağıdaki komutu kullanın. IP adresini, `publicIpAddress` sanal makinenizin değeri ile değiştirin.

   ```bash
   ssh azureuser@<publicIpAddress>
   ```

2. Kök kullanıcıya geç

   ```bash
   sudo su -
   ```

3. Oracle datafiles 'ı tutmak için biçimlendirilecek en son oluşturulan disk cihazını denetle

   ```bash
   ls -alt /dev/sd*|head -1
   ```

   Çıktı şuna benzer olacaktır:
   ```output
   brw-rw----. 1 root disk 8, 16 Dec  8 22:57 /dev/sdc
   ```

4. Cihazı biçimlendirin. 
   Kök Kullanıcı cihazda yeniden çalıştırılmuş olarak 
   
   İlk olarak bir disk etiketi oluşturun:
   ```bash
   parted /dev/sdc mklabel gpt
   ```
   Ardından diskin tamamını kapsayan bir birincil bölüm oluşturun:
   ```bash
   parted -a optimal /dev/sdc mkpart primary 0GB 64GB   
   ```
   Son olarak, meta verilerini yazdırarak cihaz ayrıntılarını kontrol edin:
   ```bash
   parted /dev/sdc print
   ```
   Çıktı şuna benzer görünmelidir:
   ```bash
   # parted /dev/sdc print
   Model: Msft Virtual Disk (scsi)
   Disk /dev/sdc: 68.7GB
   Sector size (logical/physical): 512B/4096B
   Partition Table: gpt
   Disk Flags:
   Number  Start   End     Size    File system  Name     Flags
    1      1049kB  64.0GB  64.0GB  ext4         primary
   ```

5. Cihaz bölümünde bir dosya sistemi oluşturma

   ```bash
   mkfs -t ext4 /dev/sdc1
   ```

6. Bağlama noktası oluşturma
   ```bash
   mkdir /u02
   ```

7. Diski bağlama

   ```bash
   mount /dev/sdc1 /u02
   ```

8. Bağlama noktasındaki izinleri değiştirme

   ```bash
   chmod 777 /u02
   ```

9. /Etc/fstab dosyasına bağlama ekleyin. 

   ```bash
   echo "/dev/sdc1               /u02                    ext4    defaults        0 0" >> /etc/fstab
   ```
   
10. ***/Etc/hosts*** dosyasını genel IP ve ana bilgisayar adı ile güncelleştirin.

    ***Genel IP ve VMname*** değerlerini gerçek değerlerinizi yansıtacak şekilde değiştirin:
  
    ```bash
    echo "<Public IP> <VMname>.eastus.cloudapp.azure.com <VMname>" >> /etc/hosts
    ```
11. Konak adı dosyasını güncelleştirme
    
    Sanal makinenin etki alanı adını **/etc/hostname** dosyasına eklemek için aşağıdaki komutu kullanın. Bu, **eastus** bölgesinde kaynak grubunuzu ve sanal makineyi oluşturduğunuzu varsayar:
    
    ```bash
    sed -i 's/$/\.eastus\.cloudapp\.azure\.com &/' /etc/hostname
    ```

12. Güvenlik Duvarı bağlantı noktalarını aç
    
    SELinux, Market görüntüsünde varsayılan olarak etkinleştirildiğinden, veritabanı dinleme bağlantı noktası 1521 ' i ve Enterprise Manager Express bağlantı noktası 5502 ' e yönelik trafiğe yönelik güvenlik duvarını açmanız gerekir. Kök kullanıcı olarak aşağıdaki komutları çalıştırın:

    ```bash
    firewall-cmd --zone=public --add-port=1521/tcp --permanent
    firewall-cmd --zone=public --add-port=5502/tcp --permanent
    firewall-cmd --reload
    ```
   

## <a name="create-the-database"></a>Veritabanını oluşturma

Oracle yazılımı Market görüntüsüne zaten yüklenmiş. Örnek bir veritabanını aşağıda gösterildiği gibi oluşturun. 

1.  **Oracle** kullanıcısına geç:

    ```bash
    sudo su - oracle
    ```
2. Veritabanı dinleyicisini Başlat

   ```bash
   lsnrctl start
   ```
   Çıkış aşağıdakine benzer:
  
   ```output
   LSNRCTL for Linux: Version 19.0.0.0.0 - Production on 20-OCT-2020 01:58:18

   Copyright (c) 1991, 2019, Oracle.  All rights reserved.

   Starting /u01/app/oracle/product/19.0.0/dbhome_1/bin/tnslsnr: please wait...

   TNSLSNR for Linux: Version 19.0.0.0.0 - Production
   Log messages written to /u01/app/oracle/diag/tnslsnr/vmoracle19c/listener/alert/log.xml
   Listening on: (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=vmoracle19c.eastus.cloudapp.azure.com)(PORT=1521)))

   Connecting to (ADDRESS=(PROTOCOL=tcp)(HOST=)(PORT=1521))
   STATUS of the LISTENER
   ------------------------
   Alias                     LISTENER
   Version                   TNSLSNR for Linux: Version 19.0.0.0.0 - Production
   Start Date                20-OCT-2020 01:58:18
   Uptime                    0 days 0 hr. 0 min. 0 sec
   Trace Level               off
   Security                  ON: Local OS Authentication
   SNMP                      OFF
   Listener Log File         /u01/app/oracle/diag/tnslsnr/vmoracle19c/listener/alert/log.xml
   Listening Endpoints Summary...
     (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=vmoracle19c.eastus.cloudapp.azure.com)(PORT=1521)))
   The listener supports no services
   The command completed successfully
   ```
3. Oracle veri dosyaları için bir veri dizini oluşturun:

   ```bash
   mkdir /u02/oradata
   ```
    

3.  Veritabanı oluşturma yardımcısını çalıştırın:

    ```bash
    dbca -silent \
       -createDatabase \
       -templateName General_Purpose.dbc \
       -gdbname test \
       -sid test \
       -responseFile NO_VALUE \
       -characterSet AL32UTF8 \
       -sysPassword OraPasswd1 \
       -systemPassword OraPasswd1 \
       -createAsContainerDatabase false \
       -databaseType MULTIPURPOSE \
       -automaticMemoryManagement false \
       -storageType FS \
       -datafileDestination "/u02/oradata/" \
       -ignorePreReqs
    ```

    Veritabanının oluşturulması birkaç dakika sürer.

    Aşağıdakine benzer bir çıktı görürsünüz:

    ```output
        Prepare for db operation
       10% complete
       Copying database files
       40% complete
       Creating and starting Oracle instance
       42% complete
       46% complete
       50% complete
       54% complete
       60% complete
       Completing Database Creation
       66% complete
       69% complete
       70% complete
       Executing Post Configuration Actions
       100% complete
       Database creation complete. For details check the logfiles at: /u01/app/oracle/cfgtoollogs/dbca/test.
       Database Information:
       Global Database Name:test
       System Identifier(SID):test
       Look at the log file "/u01/app/oracle/cfgtoollogs/dbca/test/test.log" for further details.
    ```

4. Oracle değişkenlerini ayarlama

    Bağlanmadan önce, *ORACLE_SID* ortam değişkenini ayarlamanız gerekir:

    ```bash
        export ORACLE_SID=test
    ```

    Ayrıca, `oracle` `.bashrc` aşağıdaki komutu kullanarak gelecekteki oturum açma işlemleri için ORACLE_SID değişkenini Users dosyasına eklemeniz gerekir:

    ```bash
    echo "export ORACLE_SID=test" >> ~oracle/.bashrc
    ```

## <a name="oracle-em-express-connectivity"></a>Oracle EM Express bağlantısı

Veritabanını araştırmak için kullanabileceğiniz bir GUI yönetim aracı için Oracle EM Express 'i ayarlayın. Oracle EM Express 'e bağlanmak için önce Oracle 'da bağlantı noktasını ayarlamanız gerekir. 

1. SQLplus kullanarak veritabanınıza bağlanın:

    ```bash
    sqlplus sys as sysdba
    ```

2. Bağlandıktan sonra, EM Express için 5502 numaralı bağlantı noktasını ayarlayın

    ```bash
    exec DBMS_XDB_CONFIG.SETHTTPSPORT(5502);
    ```

3.  Tarayıcınızdan EM Express 'i bağlayın. Tarayıcınızın EM Express ile uyumlu olduğundan emin olun (Flash yüklemesi gereklidir): 

    ```https
    https://<VM ip address or hostname>:5502/em
    ```

    **Sys** hesabını kullanarak oturum açabilir ve **SYSDBA** onay kutusunu işaretleyebilirsiniz. Yükleme sırasında ayarladığınız Password **OraPasswd1** kullanın. 

    ![Oracle OEM Express oturum açma sayfasının ekran görüntüsü](./media/oracle-quick-start/oracle_oem_express_login.png)

## <a name="automate-database-startup-and-shutdown"></a>Veritabanı başlangıcını ve kapatılmasını otomatikleştirin

Varsayılan olarak Oracle veritabanı, sanal makineyi yeniden başlattığınızda otomatik olarak başlatılmaz. Oracle veritabanını otomatik olarak başlayacak şekilde ayarlamak için ilk olarak kök olarak oturum açın. Ardından, bazı sistem dosyalarını oluşturun ve güncelleştirin.

1. Kök olarak oturum aç

    ```bash
    sudo su -
    ```

2.  Otomatik başlangıç bayrağını dosyada olarak değiştirmek için aşağıdaki komutu çalıştırın `N` `Y` `/etc/oratab` :

    ```bash
    sed -i 's/:N/:Y/' /etc/oratab
    ```

3.  Adlı bir dosya oluşturun `/etc/init.d/dbora` ve aşağıdaki içeriği yapıştırın:

    ```bash
    #!/bin/sh
    # chkconfig: 345 99 10
    # Description: Oracle auto start-stop script.
    #
    # Set ORA_HOME to be equivalent to $ORACLE_HOME.
    ORA_HOME=/u01/app/oracle/product/19.0.0/dbhome_1
    ORA_OWNER=oracle

    case "$1" in
    'start')
        # Start the Oracle databases:
        # The following command assumes that the Oracle sign-in
        # will not prompt the user for any values.
        # Remove "&" if you don't want startup as a background process.
        su - $ORA_OWNER -c "$ORA_HOME/bin/dbstart $ORA_HOME" &
        touch /var/lock/subsys/dbora
        ;;

    'stop')
        # Stop the Oracle databases:
        # The following command assumes that the Oracle sign-in
        # will not prompt the user for any values.
        su - $ORA_OWNER -c "$ORA_HOME/bin/dbshut $ORA_HOME" &
        rm -f /var/lock/subsys/dbora
        ;;
    esac
    ```

4.  *Chmod* ile dosyalardaki izinleri aşağıdaki gibi değiştirin:

    ```bash
    chgrp dba /etc/init.d/dbora
    chmod 750 /etc/init.d/dbora
    ```

5.  Başlatma ve kapanmaya yönelik sembolik bağlantıları aşağıdaki gibi oluşturun:

    ```bash
    ln -s /etc/init.d/dbora /etc/rc.d/rc0.d/K01dbora
    ln -s /etc/init.d/dbora /etc/rc.d/rc3.d/S99dbora
    ln -s /etc/init.d/dbora /etc/rc.d/rc5.d/S99dbora
    ```

6.  Değişikliklerinizi test etmek için VM 'yi yeniden başlatın:

    ```bash
    reboot
    ```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Azure 'da ilk Oracle veritabanınızı araştırmayı tamamladıktan sonra VM artık gerekli değilse, [az Group Delete](/cli/azure/group) komutunu kullanarak kaynak grubunu, VM 'yi ve tüm ilgili kaynakları kaldırabilirsiniz.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

[Oracle Backup stratejileri](./oracle-database-backup-strategies.md) Ile veritabanınızı Azure 'da nasıl koruyacağınızı öğrenin

[Azure 'da diğer Oracle çözümleri](./oracle-overview.md)hakkında bilgi edinin. 

[Oracle otomatik depolama yönetimi öğreticisini yüklemeyi ve yapılandırmayı](configure-oracle-asm.md) deneyin.
