---
title: Veri çoğaltma yapılandırma-MariaDB için Azure veritabanı
description: Bu makalede, MariaDB için Azure veritabanı 'nda Gelen Verileri Çoğaltma ayarlama açıklanır.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 6/11/2020
ms.openlocfilehash: 0b23b01faf1b6ba09f1c55db2ddabd1696e452be
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84738116"
---
# <a name="configure-data-in-replication-in-azure-database-for-mariadb"></a>MariaDB için Azure veritabanı 'nda Gelen Verileri Çoğaltma yapılandırma

Bu makalede, ana ve çoğaltma sunucularını yapılandırarak MariaDB için Azure veritabanı 'nda [gelen verileri çoğaltma](concepts-data-in-replication.md) nasıl ayarlanacağı açıklanır. Bu makalede, MariaDB sunucuları ve veritabanları ile ilgili bir önceki deneyim olduğunu varsaymaktadır.

MariaDB hizmeti için Azure veritabanı hizmetinde bir çoğaltma oluşturmak için, [gelen verileri çoğaltma](concepts-data-in-replication.md) verileri şirket içinde, sanal makinelerde (VM) veya bulut veritabanı hizmetlerinde bulunan ana MariaDB sunucusundan eşitler. Gelen Verileri Çoğaltma, MariaDB’de yerel olan ikili günlük (binlog) dosya konumuna dayalı çoğaltmayı temel alır. Binlog çoğaltma hakkında daha fazla bilgi edinmek için bkz. [binlog Çoğaltmaya genel bakış](https://mariadb.com/kb/en/library/replication-overview/).

Bu makaledeki adımları gerçekleştirmeden önce, verileri çoğaltmanın [sınırlamalarını ve gereksinimlerini](concepts-data-in-replication.md#limitations-and-considerations) gözden geçirin.

> [!NOTE]
> Ana sunucunuz sürüm 10,2 veya daha yeniyse, [genel Işlem kimliği](https://mariadb.com/kb/en/library/gtid/)kullanarak gelen verileri çoğaltma ayarlamanızı öneririz.


## <a name="create-a-mariadb-server-to-use-as-a-replica"></a>Çoğaltma olarak kullanmak için bir MariaDB sunucusu oluşturma

1. MariaDB sunucusu için yeni bir Azure veritabanı oluşturun (örneğin, replica.mariadb.database.azure.com). Sunucu, Gelen Verileri Çoğaltma içindeki çoğaltma sunucusudur.

    Sunucu oluşturma hakkında bilgi edinmek için, bkz. [Azure Portal kullanarak MariaDB Için Azure veritabanı sunucusu oluşturma](quickstart-create-mariadb-server-database-using-azure-portal.md).

   > [!IMPORTANT]
   > MariaDB sunucusu için Azure veritabanı 'nı Genel Amaçlı veya bellek için Iyileştirilmiş fiyatlandırma katmanlarında oluşturmanız gerekir.

2. Özdeş Kullanıcı hesapları ve ilgili ayrıcalıklar oluşturun.
    
    Kullanıcı hesapları ana sunucudan Çoğaltma sunucusuna çoğaltılmaz. Çoğaltma sunucusuna Kullanıcı erişimi sağlamak için, yeni oluşturulan MariaDB sunucusu için Azure veritabanı 'nda tüm hesapları ve ilgili ayrıcalıkları el ile oluşturmanız gerekir.

3. Ana sunucunun IP adresini çoğaltmanın güvenlik duvarı kurallarına ekleyin. 

   [Azure portalını](howto-manage-firewall-portal.md) veya [Azure CLI](howto-manage-firewall-cli.md)’yı kullanarak güvenlik duvarı kurallarını güncelleştirin.

> [!NOTE]
> Sapma ücretsiz iletişim
>
> Microsoft, farklı ve üçlü ortamları destekler. Bu makale, _İkincil_sözcüğe başvurular içerir. Kullanım açısından [ücretsiz iletişim Için Microsoft Stil Kılavuzu](https://github.com/MicrosoftDocs/microsoft-style-guide/blob/master/styleguide/bias-free-communication.md) bunu bir exclusionword olarak tanır. Bu makalede, şu anda yazılımda görüntülenen sözcük olduğundan, bu makalede tutarlılık için kullanılır. Yazılım, sözcüğü kaldıracak şekilde güncelleniyorsa, bu makale hizalamayla olacak şekilde güncelleştirilir.
>

## <a name="configure-the-master-server"></a>Ana sunucuyu yapılandırma

Aşağıdaki adımlar, şirket içinde barındırılan MariaDB sunucusunu, bir VM 'de veya Gelen Verileri Çoğaltma için bir bulut veritabanı hizmetinde hazırlar ve yapılandırır. MariaDB sunucusu Gelen Verileri Çoğaltma ana yöneticisidir.

1. Devam etmeden önce [ana sunucu gereksinimlerini](concepts-data-in-replication.md#requirements) gözden geçirin. 

   Örneğin, ana sunucunun bağlantı noktası 3306 ' de gelen ve giden trafiğe izin verdiğinden ve ana sunucunun **Genel BIR IP adresi**olduğundan, DNS genel olarak erişilebilir olduğundan veya tam etki alanı adı (FQDN) olduğundan emin olun. 
   
   Başka bir makinede barındırılan MySQL komut satırı veya Azure portal kullanılabilir [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) gibi bir araçtan bağlanmayı deneyerek ana sunucuya bağlantıyı test edin.

2. İkili günlüğü açın.
    
    Ana sunucuda ikili günlüğün etkin olup olmadığını görmek için aşağıdaki komutu girin:

   ```sql
   SHOW VARIABLES LIKE 'log_bin';
   ```

   Değişken [`log_bin`](https://mariadb.com/kb/en/library/replication-and-binary-log-server-system-variables/#log_bin) değeri döndürürse `ON` , sunucunuzda ikili günlük etkin olur.

   `log_bin`Değeri döndürürse `OFF` , ikili günlüğü açmak için **My. cnf** dosyasını düzenleyin `log_bin=ON` . Değişikliğin etkili olması için sunucuyu yeniden başlatın.

3. Ana sunucu ayarlarını yapılandırın.

    Gelen Verileri Çoğaltma, parametrenin `lower_case_table_names` ana ve çoğaltma sunucuları arasında tutarlı olmasını gerektirir. Bu `lower_case_table_names` parametre, `1` MariaDB Için Azure veritabanı 'nda varsayılan olarak olarak ayarlanır.

   ```sql
   SET GLOBAL lower_case_table_names = 1;
   ```

4. Yeni bir çoğaltma rolü oluşturun ve izinleri ayarlayın.

   Ana sunucuda, çoğaltma ayrıcalıklarıyla yapılandırılmış bir kullanıcı hesabı oluşturun. SQL komutları veya MySQL çalışma ekranı kullanarak bir hesap oluşturabilirsiniz. SSL ile çoğaltmayı planlıyorsanız, Kullanıcı hesabını oluştururken bunu belirtmeniz gerekir.
   
   Ana sunucunuza Kullanıcı hesaplarının nasıl ekleneceğini öğrenmek için, [MariaDB belgelerine](https://mariadb.com/kb/en/library/create-user/)bakın.

   Aşağıdaki komutları kullanarak, yeni çoğaltma rolü yalnızca ana bilgisayarın kendisini barındıran makineden değil, herhangi bir makineden ana bilgisayara erişebilir. Bu erişim için, komutta bir kullanıcı oluşturmak üzere **syncuser \@ '% '** belirtin.
   
   MariaDB belgeleri hakkında daha fazla bilgi edinmek için bkz. [hesap adlarını belirtme](https://mariadb.com/kb/en/library/create-user/#account-names).

   **SQL komutu**

   - SSL ile çoğaltma

       Tüm kullanıcı bağlantıları için SSL gerektirmek için, bir kullanıcı oluşturmak üzere aşağıdaki komutu girin:

       ```sql
       CREATE USER 'syncuser'@'%' IDENTIFIED BY 'yourpassword';
       GRANT REPLICATION SLAVE ON *.* TO ' syncuser'@'%' REQUIRE SSL;
       ```

   - SSL olmadan çoğaltma

       Tüm bağlantılar için SSL gerekmiyorsa, bir kullanıcı oluşturmak için aşağıdaki komutu girin:
    
       ```sql
       CREATE USER 'syncuser'@'%' IDENTIFIED BY 'yourpassword';
       GRANT REPLICATION SLAVE ON *.* TO ' syncuser'@'%';
       ```

   **MySQL Workbench**

   MySQL çalışma ekranı 'nda çoğaltma rolünü oluşturmak için, **Yönetim** bölmesinde, **Kullanıcılar ve ayrıcalıklar**' ı seçin. Ardından **Hesap Ekle**' yi seçin.
 
   ![Kullanıcılar ve ayrıcalıklar](./media/howto-data-in-replication/users_privileges.png)

   **Oturum açma adı** alanına bir Kullanıcı adı girin.

   ![Kullanıcıyı Eşitle](./media/howto-data-in-replication/syncuser.png)
 
   **Yönetim rolleri** panelini seçin ve ardından **genel ayrıcalıklar**listesinden **çoğaltma bağımlı**öğesini seçin. Çoğaltma rolünü oluşturmak için **Uygula** ' yı seçin.

   ![Çoğaltma bağımlı](./media/howto-data-in-replication/replicationslave.png)


5. Ana sunucuyu salt okunurdur moduna ayarlayın.

   Bir veritabanının dökümünü yapmadan önce, sunucunun salt okunurdur modunda yerleştirilmesi gerekir. Salt okuma modundayken, ana işlem herhangi bir yazma işlemini işleyemez. İş etkisini önlemeye yardımcı olmak için, salt okuma penceresini yoğun olmayan bir süre boyunca zamanlayın.

   ```sql
   FLUSH TABLES WITH READ LOCK;
   SET GLOBAL read_only = ON;
   ```

6. Geçerli ikili günlük dosyası adını ve sapmasını alın.

   Geçerli ikili günlük dosyası adını ve sapmasını öğrenmek için komutunu çalıştırın [`show master status`](https://mariadb.com/kb/en/library/show-master-status/) .
    
   ```sql
   show master status;
   ```
   Sonuçlar aşağıdaki tabloya benzer olmalıdır:
   
   ![Ana durum sonuçları](./media/howto-data-in-replication/masterstatus.png)

   Sonraki adımlarda kullanılacak olduğundan, ikili dosya adını aklınızda olun.
   
7. GTıD konumunu (isteğe bağlı, GTıD ile çoğaltma için gereklidir) alın.

   [`BINLOG_GTID_POS`](https://mariadb.com/kb/en/library/binlog_gtid_pos/)İlgili binlog dosyasının adı ve sapmasını IÇIN GTıD konumunu almak üzere işlevini çalıştırın.
  
    ```sql
    select BINLOG_GTID_POS('<binlog file name>', <binlog offset>);
    ```
 

## <a name="dump-and-restore-the-master-server"></a>Ana sunucuyu dökümünü al ve geri yükle

1. Ana sunucudan tüm veritabanlarının dökümünü alın.

   Ana sunucudan tüm veritabanlarının dökümünü yapmak için mysqldump kullanın. MySQL kitaplığı ve test kitaplığının dökümünü almak gerekli değildir.

    Daha fazla bilgi için bkz. [döküm ve geri yükleme](howto-migrate-dump-restore.md).

2. Ana sunucuyu okuma/yazma moduna ayarlayın.

   Veritabanı kaydedildikten sonra, Master MariaDB sunucusunu okuma/yazma moduna geri çevirin.

   ```sql
   SET GLOBAL read_only = OFF;
   UNLOCK TABLES;
   ```

3. Döküm dosyasını yeni sunucuya geri yükleyin.

   Döküm dosyasını MariaDB hizmeti için Azure veritabanı 'nda oluşturulan sunucuya geri yükleyin. Döküm dosyasını MariaDB sunucusuna geri yüklemek için bkz. [dump & restore](howto-migrate-dump-restore.md) .

   Döküm dosyası büyükse, Çoğaltma sunucunuz ile aynı bölgede yer alan Azure 'daki bir VM 'ye yükleyin. VM 'den MariaDB sunucusu için Azure veritabanı 'na geri yükleyin.

## <a name="link-the-master-and-replica-servers-to-start-data-in-replication"></a>Ana ve çoğaltma sunucularını başlayacak şekilde bağlama Gelen Verileri Çoğaltma

1. Ana sunucuyu ayarlayın.

   Tüm Gelen Verileri Çoğaltma işlevleri saklı yordamlar tarafından yapılır. Tüm yordamları [gelen verileri çoğaltma saklı yordamlar](reference-data-in-stored-procedures.md)' da bulabilirsiniz. Saklı yordamlar MySQL kabuğu veya MySQL çalışma ekranı 'nda çalıştırılabilir.

   İki sunucuyu bağlamak ve çoğaltmaya başlamak için, MariaDB hizmeti için Azure DB 'de hedef çoğaltma sunucusunda oturum açın. Ardından, dış örneği, `mysql.az_replication_change_master` `mysql.az_replication_change_master_with_gtid` MariaDB sunucusu IÇIN Azure DB 'deki veya saklı yordamını kullanarak ana sunucu olarak ayarlayın.

   ```sql
   CALL mysql.az_replication_change_master('<master_host>', '<master_user>', '<master_password>', 3306, '<master_log_file>', <master_log_pos>, '<master_ssl_ca>');
   ```
   
   veya
   
   ```sql
   CALL mysql.az_replication_change_master_with_gtid('<master_host>', '<master_user>', '<master_password>', 3306, '<master_gtid_pos>', '<master_ssl_ca>');
   ```

   - master_host: ana sunucunun ana bilgisayar adı
   - master_user: Ana sunucu için Kullanıcı adı
   - master_password: Ana sunucu için parola
   - master_log_file: çalışan ikili günlük dosyası adı`show master status`
   - master_log_pos: sıfırdan ikili günlük konumu`show master status`
   - master_gtid_pos: GTıD konumundan çalışıyor`select BINLOG_GTID_POS('<binlog file name>', <binlog offset>);`
   - master_ssl_ca: CA sertifikasının bağlamı. SSL kullanmıyorsanız boş bir dize geçirin. *
    
    
    * Master_ssl_ca parametresinde bir değişken olarak geçirilmesi önerilir. Daha fazla bilgi için aşağıdaki örneklere bakın.

   **Örnekler**

   - SSL ile çoğaltma

       `@cert`Aşağıdaki komutları çalıştırarak değişkeni oluşturun:

       ```sql
       SET @cert = '-----BEGIN CERTIFICATE-----
       PLACE YOUR PUBLIC KEY CERTIFICATE\'S CONTEXT HERE
       -----END CERTIFICATE-----'
       ```

       SSL ile çoğaltma, companya.com etki alanı üzerinde barındırılan bir ana sunucu ve MariaDB için Azure veritabanı 'nda barındırılan bir çoğaltma sunucusu arasında ayarlanır. Bu saklı yordam, çoğaltma üzerinde çalıştırılır.
    
       ```sql
       CALL mysql.az_replication_change_master('master.companya.com', 'syncuser', 'P@ssword!', 3306, 'mariadb-bin.000016', 475, @cert);
       ```
   - SSL olmadan çoğaltma

       SSL olmadan çoğaltma, companya.com etki alanı üzerinde barındırılan bir ana sunucu ve MariaDB için Azure veritabanı 'nda barındırılan bir çoğaltma sunucusu arasında ayarlanır. Bu saklı yordam, çoğaltma üzerinde çalıştırılır.

       ```sql
       CALL mysql.az_replication_change_master('master.companya.com', 'syncuser', 'P@ssword!', 3306, 'mariadb-bin.000016', 475, '');
       ```

2. Çoğaltmayı başlatın.

   `mysql.az_replication_start`Çoğaltmaya başlamak için saklı yordamı çağırın.

   ```sql
   CALL mysql.az_replication_start;
   ```

3. Çoğaltma durumunu denetleyin.

   [`show slave status`](https://mariadb.com/kb/en/library/show-slave-status/)Çoğaltma durumunu görüntülemek için Çoğaltma sunucusunda komutunu çağırın.
    
   ```sql
   show slave status;
   ```

   `Slave_IO_Running`Ve `Slave_SQL_Running` durumundaysa `yes` ve değeri `Seconds_Behind_Master` ise, `0` çoğaltma çalışır. `Seconds_Behind_Master`çoğaltmanın ne kadar geç olduğunu gösterir. Değer değilse `0` , çoğaltma güncelleştirmeleri işliyor demektir.

4. Verilerin çoğaltılmasını güvenli hale getirmek için ilgili sunucu değişkenlerini güncelleştirin (yalnızca GTıD olmadan çoğaltma için gereklidir).
    
    MariaDB içindeki yerel bir çoğaltma sınırlaması nedeniyle, [`sync_master_info`](https://mariadb.com/kb/en/library/replication-and-binary-log-system-variables/#sync_master_info) [`sync_relay_log_info`](https://mariadb.com/kb/en/library/replication-and-binary-log-system-variables/#sync_relay_log_info) gtıd senaryosu olmadan çoğaltma üzerinde ve değişkenlerini ayarlamanız gerekir.

    `sync_master_info` `sync_relay_log_info` Veri çoğaltma çoğaltmasının kararlı olduğundan emin olmak için bağımlı sunucunuzun ve değişkenlerini denetleyin ve değişkenlerini olarak ayarlayın `1` .
    
## <a name="other-stored-procedures"></a>Diğer saklı yordamlar

### <a name="stop-replication"></a>Çoğaltmayı durdurma

Ana sunucu ve çoğaltma sunucusu arasında çoğaltmayı durdurmak için aşağıdaki saklı yordamı kullanın:

```sql
CALL mysql.az_replication_stop;
```

### <a name="remove-the-replication-relationship"></a>Çoğaltma ilişkisini kaldır

Ana sunucu ve çoğaltma sunucusu arasındaki ilişkiyi kaldırmak için aşağıdaki saklı yordamı kullanın:

```sql
CALL mysql.az_replication_remove_master;
```

### <a name="skip-the-replication-error"></a>Çoğaltma hatasını atla

Çoğaltma hatasını atlamak ve çoğaltmaya izin vermek için aşağıdaki saklı yordamı kullanın:
    
```sql
CALL mysql.az_replication_skip_counter;
```

## <a name="next-steps"></a>Sonraki adımlar
MariaDB için Azure veritabanı [gelen verileri çoğaltma](concepts-data-in-replication.md) hakkında daha fazla bilgi edinin.
