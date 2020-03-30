---
title: Azure VM'de Oracle veritabanı oluşturma | Microsoft Dokümanlar
description: Azure ortamınızda bir Oracle Database 12c veritabanını hızla çalışır duruma getirin.
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
ms.openlocfilehash: 9f4b9d53aaa1cac17fbaae4b638e144654fad4e5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79535638"
---
# <a name="create-an-oracle-database-in-an-azure-vm"></a>Azure VM'de Oracle Veritabanı Oluşturma

Bu kılavuz, Oracle 12c veritabanı oluşturmak için [Oracle pazar galerisi görüntüsünden](https://azuremarketplace.microsoft.com/marketplace/apps/Oracle.OracleDatabase12102EnterpriseEdition?tab=Overview) bir Azure sanal makine dağıtmak için Azure CLI'yi kullanır. Sunucu dağıtıldıktan sonra, Oracle veritabanını yapılandırmak için SSH üzerinden bağlanırsınız. 

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.4 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. 

Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-virtual-machine"></a>Sanal makine oluşturma

Sanal bir makine (VM) oluşturmak için [az vm create](/cli/azure/vm) komutunu kullanın. 

Aşağıdaki örnekte `myVM` adlı bir VM oluşturulur. Varsayılan anahtar konumunda zaten yoksa, SSH anahtarları da oluşturur. Belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın.  

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
    --size Standard_DS2_v2 \
    --admin-username azureuser \
    --generate-ssh-keys
```

VM'yi oluşturduktan sonra, Azure CLI aşağıdaki örneğe benzer bilgileri görüntüler. Değeri not `publicIpAddress`edin. VM'ye erişmek için bu adresi kullanırsınız.

```output
{
  "fqdns": "",
  "id": "/subscriptions/{snip}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "westus",
  "macAddress": "00-0D-3A-36-2F-56",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "13.64.104.241",
  "resourceGroup": "myResourceGroup"
}
```

## <a name="connect-to-the-vm"></a>VM’ye bağlanma

VM ile bir SSH oturumu oluşturmak için aşağıdaki komutu kullanın. IP adresini VM'nizin `publicIpAddress` değeriyle değiştirin.

```bash
ssh azureuser@<publicIpAddress>
```

## <a name="create-the-database"></a>Veritabanını oluşturma

Oracle yazılımı zaten Marketplace görüntü yüklü. Aşağıdaki gibi örnek bir veritabanı oluşturun. 

1.  *Oracle* superuser'a geçin ve ardından günlüğe kaydetme için dinleyiciyi çalıştırın:

    ```bash
    $ sudo su - oracle
    $ lsnrctl start
    ```

    Çıkış aşağıdakine benzer:

    ```output
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

2.  Veritabanını oluşturun:

    ```bash
    dbca -silent \
           -createDatabase \
           -templateName General_Purpose.dbc \
           -gdbname cdb1 \
           -sid cdb1 \
           -responseFile NO_VALUE \
           -characterSet AL32UTF8 \
           -sysPassword OraPasswd1 \
           -systemPassword OraPasswd1 \
           -createAsContainerDatabase true \
           -numberOfPDBs 1 \
           -pdbName pdb1 \
           -pdbAdminPassword OraPasswd1 \
           -databaseType MULTIPURPOSE \
           -automaticMemoryManagement false \
           -storageType FS \
           -ignorePreReqs
    ```

    Veritabanını oluşturmak birkaç dakika sürer.

3. Oracle değişkenlerini ayarlama

Bağlanmadan önce iki ortam değişkeni ayarlamanız gerekir: *ORACLE_HOME* ve *ORACLE_SID.*

```bash
ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1; export ORACLE_HOME
ORACLE_SID=cdb1; export ORACLE_SID
```

Ayrıca .bashrc dosyasına ORACLE_HOME ve ORACLE_SID değişkenler ekleyebilirsiniz. Bu, gelecekteki oturum açma işlemleri için ortam değişkenlerini kaydeder. `~/.bashrc`

```bash
# Add ORACLE_HOME. 
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1 
# Add ORACLE_SID. 
export ORACLE_SID=cdb1 
```

## <a name="oracle-em-express-connectivity"></a>Oracle EM Express bağlantısı

Veritabanını keşfetmek için kullanabileceğiniz bir GUI yönetim aracı için Oracle EM Express'i ayarlayın. Oracle EM Express'e bağlanmak için öncelikle Oracle'da bağlantı noktası kurmanız gerekir. 

1. sqlplus kullanarak veritabanınıza bağlanın:

    ```bash
    sqlplus / as sysdba
    ```

2. Bağlandıktan sonra, EM Express için 5502 bağlantı noktasını ayarlayın

    ```bash
    exec DBMS_XDB_CONFIG.SETHTTPSPORT(5502);
    ```

3. Zaten açılmamışsa PDB1 kapsayıcısını açın, ancak önce durumu kontrol edin:

    ```bash
    select con_id, name, open_mode from v$pdbs;
    ```

    Çıkış aşağıdakine benzer:

    ```output
      CON_ID NAME                           OPEN_MODE 
      ----------- ------------------------- ---------- 
      2           PDB$SEED                  READ ONLY 
      3           PDB1                      MOUNT
    ```

4. OPEN_MODE WRITE'ı OKUMA `PDB1` değilse, PDB1'i açmak için aşağıdaki komutları çalıştırın:

   ```bash
    alter session set container=pdb1;
    alter database open;
   ```

Sqlplus oturumunu sona erdirmek ve `exit` oracle kullanıcısının oturumunu açmak için yazmak için yazmanız `quit` gerekir.

## <a name="automate-database-startup-and-shutdown"></a>Veritabanı başlatma ve kapatmayı otomatikleştirin

Oracle veritabanı varsayılan olarak VM'yi yeniden başlattığınızda otomatik olarak başlamaz. Oracle veritabanını otomatik olarak başlatmak üzere ayarlamak için, önce kök olarak oturum açın. Ardından, bazı sistem dosyaları oluşturun ve güncelleştirin.

1. Kök olarak oturum aç

    ```bash
    sudo su -
    ```

2.  Sık kullanılan düzenleyicinizi kullanarak `/etc/oratab` dosyayı edin ve varsayılanı `N` şu şekilde `Y`değiştirin:

    ```bash
    cdb1:/u01/app/oracle/product/12.1.0/dbhome_1:Y
    ```

3.  Adlandırılmış `/etc/init.d/dbora` bir dosya oluşturun ve aşağıdaki içeriği yapıştırın:

    ```bash
    #!/bin/sh
    # chkconfig: 345 99 10
    # Description: Oracle auto start-stop script.
    #
    # Set ORA_HOME to be equivalent to $ORACLE_HOME.
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
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

4.  *Chmod'lu* dosyalarda izinleri aşağıdaki gibi değiştirin:

    ```bash
    chgrp dba /etc/init.d/dbora
    chmod 750 /etc/init.d/dbora
    ```

5.  Başlatma ve kapatma için sembolik bağlantılar aşağıdaki gibi oluşturun:

    ```bash
    ln -s /etc/init.d/dbora /etc/rc.d/rc0.d/K01dbora
    ln -s /etc/init.d/dbora /etc/rc.d/rc3.d/S99dbora
    ln -s /etc/init.d/dbora /etc/rc.d/rc5.d/S99dbora
    ```

6.  Değişikliklerinizi sınamak için VM'yi yeniden başlatın:

    ```bash
    reboot
    ```

## <a name="open-ports-for-connectivity"></a>Bağlantı için açık bağlantı noktaları

Son görev, bazı dış uç noktaları yapılandırmaktır. VM'yi koruyan Azure Ağ Güvenlik Grubu'nu kurmak için, önce VM'deki SSH oturumunuzdan çıkın (önceki adımda yeniden başlatılırken SSH'den atılmış olması gerekirdi). 

1.  Oracle veritabanına uzaktan erişmek için kullandığınız bitiş noktasını açmak için az [ağ nsg kuralını](/cli/azure/network/nsg/rule) içeren bir Ağ Güvenlik Grubu kuralı oluşturun: 

    ```azurecli-interactive
    az network nsg rule create \
        --resource-group myResourceGroup\
        --nsg-name myVmNSG \
        --name allow-oracle \
        --protocol tcp \
        --priority 1001 \
        --destination-port-range 1521
    ```

2.  Oracle EM Express'e uzaktan erişmek için kullandığınız bitiş noktasını açmak için [az ağ nsg kuralını](/cli/azure/network/nsg/rule) aşağıdaki gibi oluşturarak bir Ağ Güvenlik Grubu kuralı oluşturun:

    ```azurecli-interactive
    az network nsg rule create \
        --resource-group myResourceGroup \
        --nsg-name myVmNSG \
        --name allow-oracle-EM \
        --protocol tcp \
        --priority 1002 \
        --destination-port-range 5502
    ```

3. Gerekirse, [az network public-ip show](/cli/azure/network/public-ip) ile VM'nizin genel IP adresini aşağıdaki gibi elde edin:

    ```azurecli-interactive
    az network public-ip show \
        --resource-group myResourceGroup \
        --name myVMPublicIP \
        --query [ipAddress] \
        --output tsv
    ```

4.  EM Express'i tarayıcınızdan bağlayın. Tarayıcınızın EM Express ile uyumlu olduğundan emin olun (Flash yükleme gereklidir): 

    ```https
    https://<VM ip address or hostname>:5502/em
    ```

**SYS** hesabını kullanarak oturum açabilir ve **sysdba** onay kutusunu kontrol edebilirsiniz. Yükleme sırasında ayarladığınız **OraPasswd1** parolasını kullanın. 

![Oracle OEM Express giriş sayfasının ekran görüntüsü](./media/oracle-quick-start/oracle_oem_express_login.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Azure'daki ilk Oracle veritabanınızı keşfetmeyi bitirdikten ve VM'ye artık gerek kalmadıktan sonra, kaynak grubunu, VM'yi ve ilgili tüm kaynakları kaldırmak için [az grubu silme](/cli/azure/group) komutunu kullanabilirsiniz.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

[Azure'daki](oracle-considerations.md)diğer Oracle çözümleri hakkında bilgi edinin. 

Oracle [Otomatik Depolama Yönetimi öğreticisini yüklemeyi ve yapılandırmayı](configure-oracle-asm.md) deneyin.
