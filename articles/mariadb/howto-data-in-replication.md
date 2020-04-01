---
title: Veri çoğaltma yapılandırma - MariaDB için Azure Veritabanı
description: Bu makalede, MariaDB için Azure Veritabanı'nda Veri Çoğaltma'nın nasıl ayarlanır.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/30/2020
ms.openlocfilehash: 332feffead74174ba0b9b278d8de1c5957d5b9e6
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80422463"
---
# <a name="configure-data-in-replication-in-azure-database-for-mariadb"></a>MariaDB için Azure Veritabanında Veri Çoğaltma'yı Yapılandırma

Bu makalede, ana ve çoğaltma sunucularını yapılandırarak MariaDB için Azure Veritabanı'nda Veri Çoğaltma'nın nasıl ayarlanır. Bu makalede, MariaDB sunucuları ve veritabanları ile bazı önceki deneyime sahip olduğunu varsayar.

MariaDB hizmeti için Azure Veritabanı'nda bir yineleme oluşturmak için, Veri Çoğaltma, ana MariaDB sunucusundan, sanal makinelerde (VM) veya bulut veritabanı hizmetlerinde verileri eşitler.

Bu makaledeki adımları gerçekleştirmeden önce Veri [çoğaltmasınırlamalarını ve gereksinimlerini](concepts-data-in-replication.md#limitations-and-considerations) gözden geçirin.

> [!NOTE]
> Ana sunucunuz sürüm 10.2 veya daha yeniyse, [Global İşlem Kimliği'ni](https://mariadb.com/kb/en/library/gtid/)kullanarak Veri Çoğaltma'yı ayarlamanızı öneririz.


## <a name="create-a-mariadb-server-to-use-as-a-replica"></a>Yineleme olarak kullanmak üzere bir MariaDB sunucusu oluşturma

1. MariaDB sunucusu için yeni bir Azure Veritabanı oluşturun (örneğin, replica.mariadb.database.azure.com). Sunucu, Data-in Replication'daki çoğaltma sunucusudur.

    Sunucu oluşturma hakkında bilgi edinmek için Azure [portalını kullanarak MariaDB sunucusu için Azure Veritabanı Oluştur'a](quickstart-create-mariadb-server-database-using-azure-portal.md)bakın.

   > [!IMPORTANT]
   > Genel Amaç veya Bellek Optimize edilmiş fiyatlandırma katmanlarında MariaDB sunucusu için Azure Veritabanı oluşturmanız gerekir.

2. Aynı kullanıcı hesapları ve ilgili ayrıcalıklar oluşturun.
    
    Kullanıcı hesapları ana sunucudan çoğaltma sunucusuna çoğaltılamaz. Yineleme sunucusuna kullanıcı erişimi sağlamak için, MariaDB sunucusu için yeni oluşturulan Azure Veritabanı'nda tüm hesapları ve ilgili ayrıcalıkları el ile oluşturmanız gerekir.

3. Ana sunucunun IP adresini yinelemenin güvenlik duvarı kurallarına ekleyin. 

   [Azure portalını](howto-manage-firewall-portal.md) veya [Azure CLI](howto-manage-firewall-cli.md)’yı kullanarak güvenlik duvarı kurallarını güncelleştirin.

## <a name="configure-the-master-server"></a>Ana sunucuyu yapılandırma

Aşağıdaki adımlar, şirket içinde barındırılan MariaDB sunucusunu, VM'de veya Veri Çoğaltma için bir bulut veritabanı hizmetinde hazırlar ve yapılandırır. MariaDB sunucusu, Veri Çoğaltma'nın yöneticisidir.

1. Devam etmeden önce [ana sunucu gereksinimlerini](concepts-data-in-replication.md#requirements) gözden geçirin. 

   Örneğin, ana sunucunun bağlantı noktası 3306'da hem gelen hem de giden trafiğe izin verdiğinden ve ana sunucunun **genel bir IP adresine**sahip olduğundan, DNS'nin herkese açık olduğundan veya tam nitelikli bir etki alanı adı (FQDN) olduğundan emin olun. 
   
   Başka bir makinede barındırılan MySQL komut satırı gibi bir araçtan veya Azure portalında bulunan [Azure Bulut BulutU'ndan](https://docs.microsoft.com/azure/cloud-shell/overview) bağlanmaya çalışarak ana sunucuya bağlantıyı test edin.

2. İkili günlüğe kaydetmeyi açın.
    
    Ana sitede ikili günlüğe kaydetmenin etkin olup olmadığını görmek için aşağıdaki komutu girin:

   ```sql
   SHOW VARIABLES LIKE 'log_bin';
   ```

   Değişken [`log_bin`](https://mariadb.com/kb/en/library/replication-and-binary-log-server-system-variables/#log_bin) değeri `ON`döndürürse, ikili günlük sunucunuzda etkindir.

   Değeri `log_bin` `OFF`döndürürse, **my.cnf** dosyasını ikili günlüğe kaydetmeyi `log_bin=ON` sağlayacak şekilde edin. Değişikliğin etkili olması için sunucuyu yeniden başlatın.

3. Ana sunucu ayarlarını yapılandırın.

    Veri Çoğaltma, parametrenin `lower_case_table_names` ana ve çoğaltma sunucuları arasında tutarlı olmasını gerektirir. `lower_case_table_names` Parametre, MariaDB için Azure Veritabanı'nda varsayılan olarak ayarlanır. `1`

   ```sql
   SET GLOBAL lower_case_table_names = 1;
   ```

4. Yeni bir çoğaltma rolü oluşturun ve izinler ayarlayın.

   Çoğaltma ayrıcalıklarıyla yapılandırılan ana sunucuda bir kullanıcı hesabı oluşturun. SQL komutlarını veya MySQL Workbench'i kullanarak hesap oluşturabilirsiniz. SSL ile çoğaltmayı planlıyorsanız, kullanıcı hesabını oluştururken bunu belirtmeniz gerekir.
   
   Ana sunucunuza kullanıcı hesapları eklemeyi öğrenmek için [MariaDB belgelerine](https://mariadb.com/kb/en/library/create-user/)bakın.

   Aşağıdaki komutları kullanarak, yeni çoğaltma rolü ana makineden erişebilir, sadece anayı barındıran makineye değil. Bu erişim için, bir kullanıcı oluşturmak için komutta **senkronize kullanıcı\@'%'** belirtin.
   
   MariaDB belgeleri hakkında daha fazla bilgi edinmek için [hesap adlarını belirtmeye](https://mariadb.com/kb/en/library/create-user/#account-names)bakın.

   **SQL komutu**

   - SSL ile çoğaltma

       Tüm kullanıcı bağlantıları için SSL'yi gerektirmek için, bir kullanıcı oluşturmak için aşağıdaki komutu girin:

       ```sql
       CREATE USER 'syncuser'@'%' IDENTIFIED BY 'yourpassword';
       GRANT REPLICATION SLAVE ON *.* TO ' syncuser'@'%' REQUIRE SSL;
       ```

   - SSL olmadan çoğaltma

       Tüm bağlantılar için SSL gerekli değilse, bir kullanıcı oluşturmak için aşağıdaki komutu girin:
    
       ```sql
       CREATE USER 'syncuser'@'%' IDENTIFIED BY 'yourpassword';
       GRANT REPLICATION SLAVE ON *.* TO ' syncuser'@'%';
       ```

   **MySQL Workbench**

   MySQL Workbench'te, **Yönetim** bölmesinde çoğaltma rolünü oluşturmak için **Kullanıcılar ve Ayrıcalıklar'ı**seçin. Ardından **Hesap Ekle'yi**seçin.
 
   ![Kullanıcılar ve Ayrıcalıklar](./media/howto-data-in-replication/users_privileges.png)

   **Giriş Adı** alanına bir kullanıcı adı girin.

   ![Kullanıcıyı senkronize edin](./media/howto-data-in-replication/syncuser.png)
 
   İdari **Görevler** panelini seçin ve ardından **Genel Ayrıcalıklar**listesinde **Çoğaltma Kölesi'ni**seçin. Çoğaltma rolünü oluşturmak için **Uygula'yı** seçin.

   ![Çoğaltma Kölesi](./media/howto-data-in-replication/replicationslave.png)


5. Ana sunucuyu salt okunur moduna ayarlayın.

   Veritabanını dökümüyapmadan önce, sunucunun salt okunur moduna alınması gerekir. Salt okunur modundayken, asıl kişi yazma hareketlerini işleyebilir. İş üzerindeki etkiyi önlemeye yardımcı olmak için, yalnızca okunur pencereyi yoğun olmayan bir zamanda zamanlayın.

   ```sql
   FLUSH TABLES WITH READ LOCK;
   SET GLOBAL read_only = ON;
   ```

6. Geçerli ikili günlük dosya adını ve ofset'i alın.

   Geçerli ikili günlük dosya adını ve ofset [`show master status`](https://mariadb.com/kb/en/library/show-master-status/)belirlemek için komutu çalıştırın.
    
   ```sql
   show master status;
   ```
   Sonuçlar aşağıdaki tabloya benzer olmalıdır:
   
   ![Yüksek Lisans Durum Sonuçları](./media/howto-data-in-replication/masterstatus.png)

   İkili dosya adını not edin, çünkü daha sonraki adımlarda kullanılacaktır.
   
7. GTID konumunu alın (isteğe bağlı, GTID ile çoğaltma için gerekli).

   İlgili binlog dosya adı ve ofset için GTID konumunu almak için işlevi [`BINLOG_GTID_POS`](https://mariadb.com/kb/en/library/binlog_gtid_pos/) çalıştırın.
  
    ```sql
    select BINLOG_GTID_POS('<binlog file name>', <binlog offset>);
    ```
 

## <a name="dump-and-restore-the-master-server"></a>Ana sunucuyu boşaltın ve geri yükleyin

1. Ana sunucudaki tüm veritabanlarını boşaltın.

   Ana sunucudaki tüm veritabanlarını boşaltmak için mysqldump'u kullanın. MySQL kitaplığını ve test kitaplığını dökümüne gerek yoktur.

    Daha fazla bilgi için [Dump ve geri yükleme'ye](howto-migrate-dump-restore.md)bakın.

2. Ana sunucuyu okuma/yazma moduna ayarlayın.

   Veritabanı atıldıktan sonra, ana MariaDB sunucusunu okuma/yazma moduna geri değiştirin.

   ```sql
   SET GLOBAL read_only = OFF;
   UNLOCK TABLES;
   ```

3. Döküm dosyasını yeni sunucuya geri yükleyin.

   Döküm dosyasını MariaDB hizmeti için Azure Veritabanı'nda oluşturulan sunucuya geri yükleyin. Bir döküm dosyasını MariaDB sunucusuna geri yüklemek için [Dökümü & Geri Yükleme'ye](howto-migrate-dump-restore.md) bakın.

   Döküm dosyası büyükse, çoğaltma sunucunuzla aynı bölgede Azure'daki bir VM'ye yükleyin. VM'den MariaDB sunucusu için Azure Veritabanına geri yükleyin.

## <a name="link-the-master-and-replica-servers-to-start-data-in-replication"></a>Veri çoğaltma başlatmak için ana ve çoğaltma sunucuları bağlantı

1. Ana sunucuyu ayarlayın.

   Tüm Data-in Çoğaltma işlevleri depolanan yordamlar tarafından yapılır. Tüm yordamları Veri [Çoğaltma Saklı Yordamları'nda](reference-data-in-stored-procedures.md)bulabilirsiniz. Depolanan yordamlar MySQL kabuğunda veya MySQL Workbench'te çalıştırılabilir.

   İki sunucuya bağlanmak ve çoğaltmayı başlatmak için, MariaDB hizmeti için Azure DB'deki hedef çoğaltma sunucusunda oturum açın. Ardından, MariaDB sunucusu için Azure `mysql.az_replication_change_master` DB'de `mysql.az_replication_change_master_with_gtid` depolanan yordamı kullanarak dış örneği ana sunucu olarak ayarlayın.

   ```sql
   CALL mysql.az_replication_change_master('<master_host>', '<master_user>', '<master_password>', 3306, '<master_log_file>', <master_log_pos>, '<master_ssl_ca>');
   ```
   
   or
   
   ```sql
   CALL mysql.az_replication_change_master_with_gtid('<master_host>', '<master_user>', '<master_password>', 3306, '<master_gtid_pos>', '<master_ssl_ca>');
   ```

   - master_host: ana sunucunun hostname
   - master_user: ana sunucu için kullanıcı adı
   - master_password: ana sunucu için parola
   - master_log_file: çalışan ikili günlük dosya adı`show master status`
   - master_log_pos: çalışan ikili günlük konumu`show master status`
   - master_gtid_pos: Çalışan GTID pozisyonu`select BINLOG_GTID_POS('<binlog file name>', <binlog offset>);`
   - master_ssl_ca: CA sertifikasının bağlamı. SSL kullanmıyorsanız, boş bir dize geçirin.*
    
    
    *master_ssl_ca parametresini değişken olarak geçirmenizi öneririz. Daha fazla bilgi için aşağıdaki örneklere bakın.

   **Örnekler**

   - SSL ile çoğaltma

       Aşağıdaki komutları çalıştırarak değişkeni `@cert` oluşturun:

       ```sql
       SET @cert = '-----BEGIN CERTIFICATE-----
       PLACE YOUR PUBLIC KEY CERTIFICATE\'S CONTEXT HERE
       -----END CERTIFICATE-----'
       ```

       SSL ile çoğaltma, companya.com etki alanında barındırılan bir ana sunucu ile MariaDB için Azure Veritabanı'nda barındırılan bir çoğaltma sunucusu arasında ayarlanır. Bu depolanan yordam çoğaltma üzerinde çalıştırılır.
    
       ```sql
       CALL mysql.az_replication_change_master('master.companya.com', 'syncuser', 'P@ssword!', 3306, 'mariadb-bin.000016', 475, @cert);
       ```
   - SSL olmadan çoğaltma

       SSL olmadan çoğaltma, companya.com etki alanında barındırılan bir ana sunucu ile MariaDB için Azure Veritabanı'nda barındırılan bir çoğaltma sunucusu arasında ayarlanır. Bu depolanan yordam çoğaltma üzerinde çalıştırılır.

       ```sql
       CALL mysql.az_replication_change_master('master.companya.com', 'syncuser', 'P@ssword!', 3306, 'mariadb-bin.000016', 475, '');
       ```

2. Çoğaltmayı başlatın.

   Çoğaltmayı `mysql.az_replication_start` başlatmak için depolanan yordamı arayın.

   ```sql
   CALL mysql.az_replication_start;
   ```

3. Çoğaltma durumunu denetleyin.

   Çoğaltma [`show slave status`](https://mariadb.com/kb/en/library/show-slave-status/) durumunu görüntülemek için çoğaltma sunucusundaki komutu arayın.
    
   ```sql
   show slave status;
   ```

   Durumda `Slave_IO_Running` `Slave_SQL_Running` ysa ve `yes`varsa ve değeri `Seconds_Behind_Master` `0`, çoğaltma çalışıyor. `Seconds_Behind_Master`yinelemenin ne kadar geç olduğunu gösterir. Değer `0`değilse, yineleme güncelleştirmeleri işliyor demektir.

4. Veri çoğaltmayı daha güvenli hale getirmek için ilgili sunucu değişkenlerini güncelleştirin (yalnızca GTID olmadan çoğaltma için gereklidir).
    
    MariaDB'deki yerel çoğaltma sınırlaması nedeniyle, [`sync_master_info`](https://mariadb.com/kb/en/library/replication-and-binary-log-system-variables/#sync_master_info) [`sync_relay_log_info`](https://mariadb.com/kb/en/library/replication-and-binary-log-system-variables/#sync_relay_log_info) GTID senaryosu olmadan çoğaltma üzerinde değişkenler ayarlamanız gerekir.

    Veri çoğaltmanın kararlı `sync_master_info` `sync_relay_log_info` olduğundan emin olmak için köle sunucunuzun ve değişkenlerinizi kontrol `1`edin ve değişkenleri ' ye ayarlayın.
    
## <a name="other-stored-procedures"></a>Diğer depolanan yordamlar

### <a name="stop-replication"></a>Çoğaltmayı durdurma

Ana ve çoğaltma sunucusu arasındaki çoğaltmayı durdurmak için aşağıdaki depolanan yordamı kullanın:

```sql
CALL mysql.az_replication_stop;
```

### <a name="remove-the-replication-relationship"></a>Çoğaltma ilişkisini kaldırma

Ana ve yineleme sunucusu arasındaki ilişkiyi kaldırmak için aşağıdaki saklı yordamı kullanın:

```sql
CALL mysql.az_replication_remove_master;
```

### <a name="skip-the-replication-error"></a>Çoğaltma hatasını atla

Çoğaltma hatasını atlamak ve çoğaltmaya izin vermek için aşağıdaki depolanan yordamı kullanın:
    
```sql
CALL mysql.az_replication_skip_counter;
```

## <a name="next-steps"></a>Sonraki adımlar
MariaDB için Azure Veritabanı için [Veri Çoğaltma](concepts-data-in-replication.md) hakkında daha fazla bilgi edinin.
