---
title: Oracle Golden Gate'i Azure Linux VM'de uygulayın | Microsoft Dokümanlar
description: Azure ortamınızda bir Oracle Golden Gate'i hızla çalışır hale getirin.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: mimckitt
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/02/2018
ms.author: mimckitt
ms.openlocfilehash: d7a52f09463d03fc6ecf0b57583d9eb7a5e65d45
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81263157"
---
# <a name="implement-oracle-golden-gate-on-an-azure-linux-vm"></a>Oracle Golden Gate'i Azure Linux VM'de uygulayın 

Azure CLI, komut satırından veya betik içindeki Azure kaynaklarını oluşturmak ve yönetmek için kullanılır. Bu kılavuz, Azure Marketi galerisi görüntüsünden bir Oracle 12c veritabanı dağıtmak için Azure CLI'nin nasıl kullanılacağını ayrıntılarıyla anlatır. 

Bu belge, Oracle Golden Gate'i Azure VM'de nasıl oluşturabileceğinizi, yükleyip yapılandırabileceğinizi adım adım gösterir. Bu öğreticide, iki sanal makine tek bir bölgede ayarlanmış bir kullanılabilirlik kümesinde ayarlanır. Aynı öğretici, tek bir Azure bölgesinde farklı Kullanılabilirlik Bölgelerindeki VM'ler için OracleGolden Gate'i veya iki farklı bölgedeki VM kurulumu için kullanılabilir.

Başlamadan önce Azure CLI’nin yüklü olduğundan emin olun. Daha fazla bilgi için bkz. [Azure CLI yükleme kılavuzu](https://docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="prepare-the-environment"></a>Ortamı hazırlama

Oracle Golden Gate yüklemesini gerçekleştirmek için, aynı kullanılabilirlik kümesinde iki Azure VM oluşturmanız gerekir. VM'leri oluşturmak için kullandığınız Market görüntüsü **Oracle:Oracle-Database-Ee:12.1.0.2:latest**.

Ayrıca Unix editörü vi aşina olmak ve x11 (X Windows) temel bir anlayışa sahip olması gerekir.

Aşağıdaki ortam yapılandırmasının bir özeti veözeti:
> 
> |  | **Birincil site** | **Siteyi çoğaltma** |
> | --- | --- | --- |
> | **Oracle sürümü** |Oracle 12c Sürüm 2 – (12.1.0.2) |Oracle 12c Sürüm 2 – (12.1.0.2)|
> | **Makine adı** |myVM1 |myVM2 |
> | **İşletim sistemi** |Oracle Linux 6.x |Oracle Linux 6.x |
> | **Oracle SID** |CDB1 |CDB1 |
> | **Çoğaltma şeması** |TEST|TEST |
> | **Golden Gate sahibi/çoğaltma** |C##GGADMIN |REPUSER |
> | **Golden Gate süreci** |EXTORA |REPORA|


### <a name="sign-in-to-azure"></a>Azure'da oturum açma 

Az giriş komutuyla Azure aboneliğinizde oturum [açın.](/cli/azure/reference-index) Ardından ekrandaki yönergeleri izleyin.

```azurecli
az login
```

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetilebileceği mantıksal bir kapsayıcıdır. 

Aşağıdaki örnek `westus` konumunda `myResourceGroup` adlı bir kaynak grubu oluşturur.

```azurecli
az group create --name myResourceGroup --location westus
```

### <a name="create-an-availability-set"></a>Kullanılabilirlik kümesi oluşturma

Aşağıdaki adım isteğe bağlıdır, ancak önerilir. Daha fazla bilgi için [Azure kullanılabilirlik kümeleri kılavuzuna](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines)bakın.

```azurecli
az vm availability-set create \
    --resource-group myResourceGroup \
    --name myAvailabilitySet \
    --platform-fault-domain-count 2 \
    --platform-update-domain-count 2
```

### <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

[az vm create](/cli/azure/vm) komutuyla bir sanal makine oluşturun. 

Aşağıdaki örnek, iki VMs `myVM1` `myVM2`adlı oluşturur ve. Varsayılan bir anahtar konumunda zaten yoksa SSH anahtarlarını oluşturun. Belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın.

#### <a name="create-myvm1-primary"></a>myVM1 (birincil):

```azurecli
az vm create \
     --resource-group myResourceGroup \
     --name myVM1 \
     --availability-set myAvailabilitySet \
     --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
     --size Standard_DS1_v2  \
     --generate-ssh-keys \
```

VM oluşturulduktan sonra, Azure CLI aşağıdaki örneğe benzer bilgileri gösterir. (Dikkat edin `publicIpAddress`. Bu adres VM'ye erişmek için kullanılır.)

```output
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "westus",
  "macAddress": "00-0D-3A-36-2F-56",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "13.64.104.241",
  "resourceGroup": "myResourceGroup"
}
```

#### <a name="create-myvm2-replicate"></a>myVM2 oluşturma (çoğaltma):

```azurecli
az vm create \
     --resource-group myResourceGroup \
     --name myVM2 \
     --availability-set myAvailabilitySet \
     --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
     --size Standard_DS1_v2  \
     --generate-ssh-keys \
```

Oluşturulduktan sonra `publicIpAddress` da dikkat edin.

### <a name="open-the-tcp-port-for-connectivity"></a>Bağlantı için TCP bağlantı noktasını açma

Bir sonraki adım, Oracle veritabanına uzaktan erişmenizi sağlayan harici uç noktaları yapılandırmaktır. Dış uç noktaları yapılandırmak için aşağıdaki komutları çalıştırın.

#### <a name="open-the-port-for-myvm1"></a>myVM1 için bağlantı noktasını açın:

```azurecli
az network nsg rule create --resource-group myResourceGroup\
    --nsg-name myVm1NSG --name allow-oracle\
    --protocol tcp --direction inbound --priority 999 \
    --source-address-prefix '*' --source-port-range '*' \
    --destination-address-prefix '*' --destination-port-range 1521 --access allow
```

Sonuçlar aşağıdaki yanıta benzer olmalıdır:

```output
{
  "access": "Allow",
  "description": null,
  "destinationAddressPrefix": "*",
  "destinationPortRange": "1521",
  "direction": "Inbound",
  "etag": "W/\"bd77dcae-e5fd-4bd6-a632-26045b646414\"",
  "id": "/subscriptions/<subscription-id>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myVmNSG/securityRules/allow-oracle",
  "name": "allow-oracle",
  "priority": 999,
  "protocol": "Tcp",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sourceAddressPrefix": "*",
  "sourcePortRange": "*"
}
```

#### <a name="open-the-port-for-myvm2"></a>myVM2 için bağlantı noktasını açın:

```azurecli
az network nsg rule create --resource-group myResourceGroup\
    --nsg-name myVm2NSG --name allow-oracle\
    --protocol tcp --direction inbound --priority 999 \
    --source-address-prefix '*' --source-port-range '*' \
    --destination-address-prefix '*' --destination-port-range 1521 --access allow
```

### <a name="connect-to-the-virtual-machine"></a>Sanal makineye bağlanma

Sanal makine ile bir SSH oturumu oluşturmak için aşağıdaki komutu kullanın. IP adresini, sanal makinenizin `publicIpAddress` ile değiştirin.

```bash
ssh <publicIpAddress>
```

### <a name="create-the-database-on-myvm1-primary"></a>myVM1(birincil) veritabanını oluşturma

Oracle yazılımı zaten Marketplace görüntü yüklü, bu nedenle bir sonraki adım veritabanıyüklemektir. 

Yazılımı 'oracle' süper kullanıcısı olarak çalıştırın:

```bash
sudo su - oracle
```

Veritabanını oluşturun:

```bash
$ dbca -silent \
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

Çıktılar aşağıdaki yanıta benzer olmalıdır:

```output
Copying database files
1% complete
2% complete
8% complete
13% complete
19% complete
27% complete
Creating and starting Oracle instance
29% complete
32% complete
33% complete
34% complete
38% complete
42% complete
43% complete
45% complete
Completing Database Creation
48% complete
51% complete
53% complete
62% complete
70% complete
72% complete
Creating Pluggable Databases
78% complete
100% complete
Look at the log file "/u01/app/oracle/cfgtoollogs/dbca/cdb1/cdb1.log" for more details.
```

ORACLE_SID ve ORACLE_HOME değişkenlerini ayarlayın.

```bash
$ ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1; export ORACLE_HOME
$ ORACLE_SID=cdb1; export ORACLE_SID
$ LD_LIBRARY_PATH=ORACLE_HOME/lib; export LD_LIBRARY_PATH
```

İsteğe bağlı olarak, .bashrc dosyasına ORACLE_HOME ve ORACLE_SID ekleyebilirsiniz, böylece bu ayarlar gelecekteki oturum açma işlemleri için kaydedilir:

```bash
# add oracle home
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
# add oracle sid
export ORACLE_SID=cdb1
# add Oracle library path
export LD_LIBRARY_PATH=$ORACLE_HOME/lib
```

### <a name="start-oracle-listener"></a>Oracle dinleyicisini başlatın

```bash
$ lsnrctl start
```

### <a name="create-the-database-on-myvm2-replicate"></a>myVM2'de veritabanı oluşturma (çoğaltma)

```bash
sudo su - oracle
```

Veritabanını oluşturun:

```bash
$ dbca -silent \
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

ORACLE_SID ve ORACLE_HOME değişkenlerini ayarlayın.

```bash
$ ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1; export ORACLE_HOME
$ ORACLE_SID=cdb1; export ORACLE_SID
$ LD_LIBRARY_PATH=ORACLE_HOME/lib; export LD_LIBRARY_PATH
```

İsteğe bağlı olarak, .bashrc dosyasına ORACLE_HOME ve ORACLE_SID ekleyebilirsiniz, böylece bu ayarlar gelecekteki oturum açma işlemleri için kaydedilir.

```bash
# add oracle home
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
# add oracle sid
export ORACLE_SID=cdb1
# add Oracle library path
export LD_LIBRARY_PATH=$ORACLE_HOME/lib
```

### <a name="start-oracle-listener"></a>Oracle dinleyicisini başlatın

```bash
$ sudo su - oracle
$ lsnrctl start
```

## <a name="configure-golden-gate"></a>Golden Gate'i Yapılandır 
Golden Gate'i yapılandırmak için bu bölümdeki adımları izleyin.

### <a name="enable-archive-log-mode-on-myvm1-primary"></a>myVM1'de arşiv günlüğü modunu etkinleştirme (birincil)

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
```
Kuvvet günlüğe kaydetmeyi etkinleştirin ve en az bir günlük dosyasının bulunduğundan emin olun.

```bash
SQL> ALTER DATABASE FORCE LOGGING;
SQL> ALTER SYSTEM SWITCH LOGFILE;
SQL> ALTER SYSTEM set enable_goldengate_replication=true;
SQL> ALTER PLUGGABLE DATABASE PDB1 OPEN;
SQL> ALTER SESSION SET CONTAINER=PDB1;
SQL> ALTER DATABASE ADD SUPPLEMENTAL LOG DATA;
SQL> EXIT;
```

### <a name="download-golden-gate-software"></a>Golden Gate yazılımLarını indirin
Oracle Golden Gate yazılımını indirmek ve hazırlamak için aşağıdaki adımları tamamlayın:

1. Oracle Golden Gate indirme [sayfasından](https://www.oracle.com/technetwork/middleware/goldengate/downloads/index.html) **fbo_ggs_Linux_x64_shiphome.zip** dosyasını indirin. Oracle Linux **x86-64 için oracle GoldenGate 12.x.x.x**indirme başlığı altında, indirmek için .zip dosyaları bir dizi olmalıdır.

2. .zip dosyalarını istemci bilgisayarınıza indirdikten sonra, dosyaları VM'nize kopyalamak için Güvenli Kopyalama Protokolü'nü (SCP) kullanın:

   ```bash
   $ scp fbo_ggs_Linux_x64_shiphome.zip <publicIpAddress>:<folder>
   ```

3. .zip dosyalarını **/opt** klasörüne taşıyın. Ardından dosyaların sahibini aşağıdaki gibi değiştirin:

   ```bash
   $ sudo su -
   # mv <folder>/*.zip /opt
   ```

4. Dosyaları unzip (zaten yüklü değilse Linux unzip yardımcı programı yükleyin):

   ```bash
   # yum install unzip
   # cd /opt
   # unzip fbo_ggs_Linux_x64_shiphome.zip
   ```

5. İzni değiştir:

   ```bash
   # chown -R oracle:oinstall /opt/fbo_ggs_Linux_x64_shiphome
   ```

### <a name="prepare-the-client-and-vm-to-run-x11-for-windows-clients-only"></a>İstemciyi ve VM'yi x11'i çalıştıracak şekilde hazırlayın (yalnızca Windows istemcileri için)
Bu isteğe bağlı bir adımdır. Bir Linux istemcisi kullanıyorsanız veya zaten x11 kurulumunuz varsa bu adımı atlayabilirsiniz.

1. PuTTY ve Xming'i Windows bilgisayarınıza indirin:

   * [PutTY'yi Karşıdan Yükleyin](https://www.putty.org/)
   * [Xming'i İndir](https://xming.en.softonic.com/)

2. PuTTY'yi yükledikten sonra, PuTTY klasörüne (örneğin, C:\Program Files\PuTTY), puttygen.exe (PuTTY Anahtar Jeneratörü) çalıştırın.

3. PutTY anahtar jeneratörü olarak:

   - Bir anahtar oluşturmak için **Oluştur** düğmesini seçin.
   - Anahtarın içeriğini kopyalayın (**Ctrl+C**).
   - Özel **anahtarı kaydet düğmesini** seçin.
   - Görünen uyarıyı yoksayın ve ardından **Tamam'ı**seçin.

   ![PuTTY anahtar jeneratör sayfasının ekran görüntüsü](./media/oracle-golden-gate/puttykeygen.png)

4. VM'nizde şu komutları çalıştırın:

   ```bash
   # sudo su - oracle
   $ mkdir .ssh (if not already created)
   $ cd .ssh
   ```

5. **authorized_keys**adlı bir dosya oluşturun. Bu dosyadaki anahtarın içeriğini yapıştırın ve ardından dosyayı kaydedin.

   > [!NOTE]
   > Anahtar dize `ssh-rsa`içermelidir. Ayrıca, anahtarın içeriği tek bir metin satırı olmalıdır.
   >  

6. PuTTY’yi başlatın. **Kategori** bölmesinde **Bağlantı** > **SSH** > **Auth'u**seçin. Kimlik **doğrulama için Özel anahtar dosyasında,** daha önce oluşturduğunuz anahtara göz atın.

   ![Set Private Key sayfasının ekran görüntüsü](./media/oracle-golden-gate/setprivatekey.png)

7. **Kategori** bölmesinde **Bağlantı** > **SSH** > **X11'i**seçin. Ardından **X11 yönlendirme** kutusunu etkinleştir'i seçin.

   ![X11'i etkinleştir sayfasının ekran görüntüsü](./media/oracle-golden-gate/enablex11.png)

8. **Kategori** bölmesinde **Oturum'a**gidin. Ana bilgisayar bilgilerini girin ve sonra **Aç'ı**seçin.

   ![Oturum sayfasının ekran görüntüsü](./media/oracle-golden-gate/puttysession.png)

### <a name="install-golden-gate-software"></a>Golden Gate yazılımLarını yükleyin

Oracle Golden Gate'i yüklemek için aşağıdaki adımları tamamlayın:

1. Kahin olarak oturum açın. (Bir parola istenmeden oturum açabilmelisiniz.) Yüklemeye başlamadan önce Xming'in çalıştığını unutmayın.

   ```bash
   $ cd /opt/fbo_ggs_Linux_x64_shiphome/Disk1
   $ ./runInstaller
   ```

2. Oracle Database 12c için 'Oracle GoldenGate' seçeneğini belirleyin. Ardından devam etmek için **İleri'yi** seçin.

   ![Yükleyici Select Kurulum sayfasının ekran görüntüsü](./media/oracle-golden-gate/golden_gate_install_01.png)

3. Yazılım konumunu değiştirin. Ardından **Başlat Yöneticisi** kutusunu seçin ve veritabanı konumunu girin. Devam etmek için **İleri**’yi seçin.

   ![Yükleme seç sayfasının ekran görüntüsü](./media/oracle-golden-gate/golden_gate_install_02.png)

4. Stok dizinini değiştirin ve devam etmek için **İleri'yi** seçin.

   ![Yükleme seç sayfasının ekran görüntüsü](./media/oracle-golden-gate/golden_gate_install_03.png)

5. **Özet** ekranında devam etmek için **Yükle'yi** seçin.

   ![Yükleyici Select Kurulum sayfasının ekran görüntüsü](./media/oracle-golden-gate/golden_gate_install_04.png)

6. Bir komut dosyasını 'root' olarak çalıştırmanız isden edilebilir. Bu öyleyse, ayrı bir oturum açın, VM'ye ssh, köksudo ve sonra komut dosyası çalıştırın. **Devam Et'i** seç.

   ![Yükleme seç sayfasının ekran görüntüsü](./media/oracle-golden-gate/golden_gate_install_05.png)

7. Yükleme tamamlandığında, işlemi tamamlamak için **Kapat'ı** seçin.

   ![Yükleme seç sayfasının ekran görüntüsü](./media/oracle-golden-gate/golden_gate_install_06.png)

### <a name="set-up-service-on-myvm1-primary"></a>myVM1'de hizmet ayarlama (birincil)

1. Tnsnames.ora dosyasını oluşturun veya güncelleştirin:

   ```bash
   $ cd $ORACLE_HOME/network/admin
   $ vi tnsnames.ora

   cdb1=
    (DESCRIPTION=
      (ADDRESS=
        (PROTOCOL=TCP)
        (HOST=localhost)
        (PORT=1521)
      )
      (CONNECT_DATA=
        (SERVER=dedicated)
        (SERVICE_NAME=cdb1)
      )
    )

   pdb1=
    (DESCRIPTION=
      (ADDRESS=
        (PROTOCOL=TCP)
        (HOST=localhost)
        (PORT=1521)
      )
      (CONNECT_DATA=
        (SERVER=dedicated)
        (SERVICE_NAME=pdb1)
      )
    )
   ```

2. Golden Gate sahibini ve kullanıcı hesaplarını oluşturun.

   > [!NOTE]
   > Sahip hesabında C## öneki olmalıdır.
   >

    ```bash
    $ sqlplus / as sysdba
    SQL> CREATE USER C##GGADMIN identified by ggadmin;
    SQL> EXEC dbms_goldengate_auth.grant_admin_privilege('C##GGADMIN',container=>'ALL');
    SQL> GRANT DBA to C##GGADMIN container=all;
    SQL> connect C##GGADMIN/ggadmin
    SQL> ALTER SESSION SET CONTAINER=PDB1;
    SQL> EXIT;
    ```

3. Golden Gate test kullanıcı hesabını oluşturun:

   ```bash
   $ cd /u01/app/oracle/product/12.1.0/oggcore_1
   $ sqlplus system/OraPasswd1@pdb1
   SQL> CREATE USER test identified by test DEFAULT TABLESPACE USERS TEMPORARY TABLESPACE TEMP;
   SQL> GRANT connect, resource, dba TO test;
   SQL> ALTER USER test QUOTA 100M on USERS;
   SQL> connect test/test@pdb1
   SQL> @demo_ora_create
   SQL> @demo_ora_insert
   SQL> EXIT;
   ```

4. Ekstre parametresi dosyasını yapılandırın.

   Golden gate komut satırı arabirimini başlatın (ggsci):

   ```bash
   $ sudo su - oracle
   $ cd /u01/app/oracle/product/12.1.0/oggcore_1
   $ ./ggsci
   GGSCI> DBLOGIN USERID test@pdb1, PASSWORD test
   Successfully logged into database  pdb1
   GGSCI>  ADD SCHEMATRANDATA pdb1.test
   2017-05-23 15:44:25  INFO    OGG-01788  SCHEMATRANDATA has been added on schema test.
   2017-05-23 15:44:25  INFO    OGG-01976  SCHEMATRANDATA for scheduling columns has been added on schema test.

   GGSCI> EDIT PARAMS EXTORA
   ```

5. EXTRACT parametre dosyasına (vi komutlarını kullanarak) aşağıdakileri ekleyin. Esc tuşuna basın, ':wq!' dosyayı kaydetmek için. 

   ```bash
   EXTRACT EXTORA
   USERID C##GGADMIN, PASSWORD ggadmin
   RMTHOST 10.0.0.5, MGRPORT 7809
   RMTTRAIL ./dirdat/rt  
   DDL INCLUDE MAPPED
   DDLOPTIONS REPORT 
   LOGALLSUPCOLS
   UPDATERECORDFORMAT COMPACT
   TABLE pdb1.test.TCUSTMER;
   TABLE pdb1.test.TCUSTORD;
   ```

6. Kayıt özü--entegre özü:

   ```bash
   $ cd /u01/app/oracle/product/12.1.0/oggcore_1
   $ ./ggsci

   GGSCI> dblogin userid C##GGADMIN, password ggadmin
   Successfully logged into database CDB$ROOT.

   GGSCI> REGISTER EXTRACT EXTORA DATABASE CONTAINER(pdb1)

   2017-05-23 15:58:34  INFO    OGG-02003  Extract EXTORA successfully registered with database at SCN 1821260.

   GGSCI> exit
   ```

7. Ayıklama denetim noktalarını ayarlayın ve gerçek zamanlı ayıklama başlatın:

   ```bash
   $ ./ggsci
   GGSCI>  ADD EXTRACT EXTORA, INTEGRATED TRANLOG, BEGIN NOW
   EXTRACT (Integrated) added.

   GGSCI>  ADD RMTTRAIL ./dirdat/rt, EXTRACT EXTORA, MEGABYTES 10
   RMTTRAIL added.

   GGSCI>  START EXTRACT EXTORA

   Sending START request to MANAGER ...
   EXTRACT EXTORA starting

   GGSCI > info all

   Program     Status      Group       Lag at Chkpt  Time Since Chkpt

   MANAGER     RUNNING
   EXTRACT     RUNNING     EXTORA      00:00:11      00:00:04
   ```

   Bu adımda, daha sonra kullanılacak başlangıç SCN'sini farklı bir bölümde bulabilirsiniz:

   ```bash
   $ sqlplus / as sysdba
   SQL> alter session set container = pdb1;
   SQL> SELECT current_scn from v$database;
   CURRENT_SCN
   -----------
      1857887
   SQL> EXIT;
   ```

   ```bash
   $ ./ggsci
   GGSCI> EDIT PARAMS INITEXT
   ```

   ```bash
   EXTRACT INITEXT
   USERID C##GGADMIN, PASSWORD ggadmin
   RMTHOST 10.0.0.5, MGRPORT 7809
   RMTTASK REPLICAT, GROUP INITREP
   TABLE pdb1.test.*, SQLPREDICATE 'AS OF SCN 1857887'; 
   ```

   ```bash
   GGSCI> ADD EXTRACT INITEXT, SOURCEISTABLE
   ```

### <a name="set-up-service-on-myvm2-replicate"></a>myVM2'de hizmet ayarlama (çoğaltma)


1. Tnsnames.ora dosyasını oluşturun veya güncelleştirin:

   ```bash
   $ cd $ORACLE_HOME/network/admin
   $ vi tnsnames.ora

   cdb1=
    (DESCRIPTION=
      (ADDRESS=
        (PROTOCOL=TCP)
        (HOST=localhost)
        (PORT=1521)
      )
      (CONNECT_DATA=
        (SERVER=dedicated)
        (SERVICE_NAME=cdb1)
      )
    )

   pdb1=
    (DESCRIPTION=
      (ADDRESS=
        (PROTOCOL=TCP)
        (HOST=localhost)
        (PORT=1521)
      )
      (CONNECT_DATA=
        (SERVER=dedicated)
        (SERVICE_NAME=pdb1)
      )
    )
   ```

2. Çoğaltma hesabı oluşturun:

   ```bash
   $ sqlplus / as sysdba
   SQL> alter session set container = pdb1;
   SQL> create user repuser identified by rep_pass container=current;
   SQL> grant dba to repuser;
   SQL> exec dbms_goldengate_auth.grant_admin_privilege('REPUSER',container=>'PDB1');
   SQL> connect repuser/rep_pass@pdb1 
   SQL> EXIT;
   ```

3. Golden Gate test kullanıcı hesabı oluşturun:

   ```bash
   $ cd /u01/app/oracle/product/12.1.0/oggcore_1
   $ sqlplus system/OraPasswd1@pdb1
   SQL> CREATE USER test identified by test DEFAULT TABLESPACE USERS TEMPORARY TABLESPACE TEMP;
   SQL> GRANT connect, resource, dba TO test;
   SQL> ALTER USER test QUOTA 100M on USERS;
   SQL> connect test/test@pdb1
   SQL> @demo_ora_create
   SQL> EXIT;
   ```

4. Değişiklikleri çoğaltmak için REPLICAT parametre dosyası: 

   ```bash
   $ cd /u01/app/oracle/product/12.1.0/oggcore_1
   $ ./ggsci
   GGSCI> EDIT PARAMS REPORA  
   ```

   REPORA parametre dosyasının içeriği:

   ```bash
   REPLICAT REPORA
   ASSUMETARGETDEFS
   DISCARDFILE ./dirrpt/repora.dsc, PURGE, MEGABYTES 100
   DDL INCLUDE MAPPED
   DDLOPTIONS REPORT
   DBOPTIONS INTEGRATEDPARAMS(parallelism 6)
   USERID repuser@pdb1, PASSWORD rep_pass
   MAP pdb1.test.*, TARGET pdb1.test.*;
   ```

5. Çoğaltma denetim noktası ayarlayın:

   ```bash
   GGSCI> ADD REPLICAT REPORA, INTEGRATED, EXTTRAIL ./dirdat/rt
   GGSCI> EDIT PARAMS INITREP

   ```

   ```bash
   REPLICAT INITREP
   ASSUMETARGETDEFS
   DISCARDFILE ./dirrpt/tcustmer.dsc, APPEND
   USERID repuser@pdb1, PASSWORD rep_pass
   MAP pdb1.test.*, TARGET pdb1.test.*;   
   ```

   ```bash
   GGSCI> ADD REPLICAT INITREP, SPECIALRUN
   ```

### <a name="set-up-the-replication-myvm1-and-myvm2"></a>Çoğaltma (myVM1 ve myVM2) ayarlayın

#### <a name="1-set-up-the-replication-on-myvm2-replicate"></a>1. myVM2 çoğaltma ayarlayın (çoğaltma)

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci
  GGSCI> EDIT PARAMS MGR
  ```

Dosyayı aşağıdakilerle güncelleştirin:

  ```bash
  PORT 7809
  ACCESSRULE, PROG *, IPADDR *, ALLOW
  ```

Ardından Yönetici hizmetini yeniden başlatın:

  ```bash
  GGSCI> STOP MGR
  GGSCI> START MGR
  GGSCI> EXIT
  ```

#### <a name="2-set-up-the-replication-on-myvm1-primary"></a>2. myVM1 (birincil) üzerinde çoğaltma ayarlayın

İlk yükü başlatın ve hataları denetleyin:

```bash
$ cd /u01/app/oracle/product/12.1.0/oggcore_1
$ ./ggsci
GGSCI> START EXTRACT INITEXT
GGSCI> VIEW REPORT INITEXT
```

#### <a name="3-set-up-the-replication-on-myvm2-replicate"></a>3. myVM2 çoğaltma ayarlayın (çoğaltma)

SCN numarasını daha önce elde ettiğiniz numarayla değiştirin:

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci
  START REPLICAT REPORA, AFTERCSN 1857887
  ```

Çoğaltma başladı ve TEST tablolarına yeni kayıtlar ekleyerek bunu sınayabilirsiniz.


### <a name="view-job-status-and-troubleshooting"></a>İş durumunu ve sorun giderme durumunu görüntüleme

#### <a name="view-reports"></a>Raporları görüntüle
myVM1'deki raporları görüntülemek için aşağıdaki komutları çalıştırın:

  ```bash
  GGSCI> VIEW REPORT EXTORA 
  ```
 
myVM2'deki raporları görüntülemek için aşağıdaki komutları çalıştırın:

  ```bash
  GGSCI> VIEW REPORT REPORA
  ```

#### <a name="view-status-and-history"></a>Durumu ve geçmişi görüntüleme
myVM1'de durumu ve geçmişi görüntülemek için aşağıdaki komutları çalıştırın:

  ```bash
  GGSCI> dblogin userid c##ggadmin, password ggadmin 
  GGSCI> INFO EXTRACT EXTORA, DETAIL
  ```

myVM2'de durumu ve geçmişi görüntülemek için aşağıdaki komutları çalıştırın:

  ```bash
  GGSCI> dblogin userid repuser@pdb1 password rep_pass 
  GGSCI> INFO REP REPORA, DETAIL
  ```
Bu, Oracle linux'ta Golden Gate'in kurulumunu ve yapılandırmasını tamamlar.


## <a name="delete-the-virtual-machine"></a>Şu sanal makineyi silin:

Artık gerekolmadığında, kaynak grubunu, VM'i ve ilgili tüm kaynakları kaldırmak için aşağıdaki komut kullanılabilir.

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

[Yüksek oranda kullanılabilir sanal makine oluşturma öğreticisi](../../linux/create-cli-complete.md)

[VM dağıtımı CLI örneklerini keşfedin](../../linux/cli-samples.md)
