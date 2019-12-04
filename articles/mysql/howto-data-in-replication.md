---
title: Veri çoğaltma yapılandırma-MySQL için Azure veritabanı
description: Bu makalede MySQL için Azure veritabanı 'nın Gelen Verileri Çoğaltma nasıl ayarlanacağı açıklanır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: eaebcf50084223e1c1f4df30294bece96cffda6d
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74774305"
---
# <a name="how-to-configure-azure-database-for-mysql-data-in-replication"></a>MySQL için Azure veritabanı 'nı yapılandırma Gelen Verileri Çoğaltma

Bu makalede, ana ve çoğaltma sunucularını yapılandırarak MySQL için Azure veritabanı hizmetine Gelen Verileri Çoğaltma ayarlamayı öğreneceksiniz. Gelen Verileri Çoğaltma, şirket içinde, sanal makinelerde çalışan bir ana MySQL sunucusundan veya diğer bulut sağlayıcıları tarafından barındırılan veritabanı hizmetlerinde, MySQL için Azure veritabanı hizmeti 'ndeki bir çoğaltmada veri eşitlemenize olanak tanır. 

Bu makalede, MySQL sunucuları ve veritabanları ile ilgili en az bir önceki deneyim olduğunu varsaymaktadır.

## <a name="create-a-mysql-server-to-be-used-as-replica"></a>Çoğaltma olarak kullanılacak bir MySQL sunucusu oluşturma

1. MySQL için yeni bir Azure veritabanı sunucusu oluşturun

   Yeni bir MySQL sunucusu (örn.) oluşturun. "replica.mysql.database.azure.com"). Sunucu oluşturma için [Azure Portal kullanarak MySQL Için Azure veritabanı sunucusu oluşturma](quickstart-create-mysql-server-database-using-azure-portal.md) bölümüne bakın. Bu sunucu, Gelen Verileri Çoğaltma "çoğaltma" sunucusudur.

   > [!IMPORTANT]
   > MySQL için Azure veritabanı sunucusunun Genel Amaçlı veya bellek için Iyileştirilmiş fiyatlandırma katmanlarında oluşturulması gerekir.
   > 

2. Aynı kullanıcı hesaplarını ve ilgili ayrıcalıkları oluşturma

   Kullanıcı hesapları ana sunucudan Çoğaltma sunucusuna çoğaltılmaz. Kullanıcılara Çoğaltma sunucusuna erişimi sağlamayı planlıyorsanız, yeni oluşturulan MySQL için Azure veritabanı sunucusunda tüm hesapları ve ilgili ayrıcalıkları el ile oluşturmanız gerekir.

## <a name="configure-the-master-server"></a>Ana sunucuyu yapılandırma
Aşağıdaki adımlar, şirket içinde barındırılan MySQL sunucusunu, bir sanal makinede veya Gelen Verileri Çoğaltma için diğer bulut sağlayıcıları tarafından barındırılan veritabanı hizmetinde hazırlar ve yapılandırır. Bu sunucu, veri içindeki çoğaltmada "ana" dır. 

1. İkili günlüğü aç

   Aşağıdaki komutu çalıştırarak, ana bilgisayarda ikili günlük özelliğinin etkinleştirilip etkinleştirilmediğini denetleyin: 

   ```sql
   SHOW VARIABLES LIKE 'log_bin';
   ```

   [`log_bin`](https://dev.mysql.com/doc/refman/8.0/en/replication-options-binary-log.html#sysvar_log_bin) DEĞIŞKENI "on" değeriyle döndürülürse, sunucunuzda ikili günlük etkin olur. 

   `log_bin` "kapalı" değeri ile döndürülürse, ". cn`log_bin=ON` f" dosyanızı düzenleyerek ikili günlüğü açın ve değişikliğin etkili olması için sunucunuzu yeniden başlatın.

2. Ana sunucu ayarları

   Gelen Verileri Çoğaltma, ana ve çoğaltma sunucuları arasında `lower_case_table_names` parametresinin tutarlı olmasını gerektirir. Bu parametre, MySQL için Azure veritabanı 'nda varsayılan olarak 1 ' dir. 

   ```sql
   SET GLOBAL lower_case_table_names = 1;
   ```

3. Yeni bir çoğaltma rolü oluşturma ve izin ayarlama

   Ana sunucuda, çoğaltma ayrıcalıklarıyla yapılandırılmış bir kullanıcı hesabı oluşturun. Bu, SQL komutları veya MySQL çalışma ekranı gibi bir araç aracılığıyla yapılabilir. Kullanıcı oluştururken belirtilmesi gereken SSL ile çoğaltmayı planlayıp planladığınızı düşünün. Ana sunucunuza [Kullanıcı hesaplarının nasıl ekleneceğini](https://dev.mysql.com/doc/refman/5.7/en/adding-users.html) anlamak için MySQL belgelerine bakın. 

   Aşağıdaki komutlarda, oluşturulan yeni çoğaltma rolü yalnızca ana bilgisayarın kendisini barındıran makineden değil, herhangi bir makineden ana bilgisayara erişebilir. Bu işlem, kullanıcı oluştur komutunda "syncuser@ '% '" belirtilerek yapılır. [Hesap adlarını belirtme](https://dev.mysql.com/doc/refman/5.7/en/account-names.html)hakkında daha fazla bilgi edinmek için MySQL belgelerine bakın.

   **SQL komutu**

   *SSL ile çoğaltma*

   Tüm kullanıcı bağlantıları için SSL istemek üzere, bir kullanıcı oluşturmak için aşağıdaki komutu kullanın: 

   ```sql
   CREATE USER 'syncuser'@'%' IDENTIFIED BY 'yourpassword';
   GRANT REPLICATION SLAVE ON *.* TO ' syncuser'@'%' REQUIRE SSL;
   ```

   *SSL olmadan çoğaltma*

   Tüm bağlantılar için SSL gerekmiyorsa, bir kullanıcı oluşturmak için aşağıdaki komutu kullanın:

   ```sql
   CREATE USER 'syncuser'@'%' IDENTIFIED BY 'yourpassword';
   GRANT REPLICATION SLAVE ON *.* TO ' syncuser'@'%';
   ```

   **MySQL çalışma ekranı**

   MySQL çalışma ekranı 'nda çoğaltma rolünü oluşturmak için, **Yönetim** panelinden **Kullanıcılar ve ayrıcalıklar** panelini açın. Ardından **Hesap Ekle**' ye tıklayın. 
 
   ![Kullanıcılar ve ayrıcalıklar](./media/howto-data-in-replication/users_privileges.png)

   Kullanıcı **adını oturum açma adı** alanına yazın. 

   ![Kullanıcıyı Eşitle](./media/howto-data-in-replication/syncuser.png)
 
   **Yönetim rolleri** paneline tıklayın ve sonra **genel ayrıcalıklar**listesinden **çoğaltma bağımlı** öğesini seçin. Ardından, çoğaltma rolünü oluşturmak için **Uygula** ' ya tıklayın.

   ![Çoğaltma bağımlı](./media/howto-data-in-replication/replicationslave.png)


4. Ana sunucuyu salt okunurdur moduna ayarlama

   Veritabanının dökümünü başlatmadan önce, sunucunun salt okunurdur modunda yerleştirilmesi gerekir. Salt okuma modundayken, ana öğe herhangi bir yazma işlemini işleyemez. İşletmenizin etkisini değerlendirin ve gerekirse, salt okuma penceresini yoğun olmayan bir zamanda zamanlayın.

   ```sql
   FLUSH TABLES WITH READ LOCK;
   SET GLOBAL read_only = ON;
   ```

5. İkili günlük dosyası adı ve sapmasını al

   Geçerli ikili günlük dosyası adını ve sapmasını öğrenmek için [`show master status`](https://dev.mysql.com/doc/refman/5.7/en/show-master-status.html) komutunu çalıştırın.
    
   ```sql
   show master status;
   ```
   Sonuçlar aşağıdaki gibi olmalıdır. Sonraki adımlarda kullanılacak ikili dosya adını unutmayın.

   ![Ana durum sonuçları](./media/howto-data-in-replication/masterstatus.png)
 
## <a name="dump-and-restore-master-server"></a>Ana sunucu dökümünü al ve geri yükle

1. Ana sunucudan tüm veritabanlarının dökümünü al

   Yöneticinizdeki veritabanlarının dökümünü yapmak için mysqldump kullanabilirsiniz. Ayrıntılar için, [döküm & geri yükleme](concepts-migrate-dump-restore.md)bölümüne bakın. MySQL kitaplığı ve test kitaplığının dökümünü almak gereksizdir.

2. Ana sunucuyu okuma/yazma moduna ayarla

   Veritabanı kaydedildikten sonra ana MySQL sunucusunu okuma/yazma moduna geri çevirin.

   ```sql
   SET GLOBAL read_only = OFF;
   UNLOCK TABLES;
   ```

3. Döküm dosyasını yeni sunucuya geri yükle

   Döküm dosyasını MySQL için Azure veritabanı hizmetinde oluşturulan sunucuya geri yükleyin. Bir MySQL sunucusuna bir döküm dosyası geri yüklemek için [döküm & geri yükleme](concepts-migrate-dump-restore.md) bölümüne bakın. Döküm dosyası büyükse, Çoğaltma sunucunuz ile aynı bölgedeki Azure 'daki bir sanal makineye yükleyin. Sanal makineden MySQL için Azure veritabanı sunucusuna geri yükleyin.

## <a name="link-master-and-replica-servers-to-start-data-in-replication"></a>Ana ve çoğaltma sunucularını başlatma Gelen Verileri Çoğaltma bağlama

1. Ana sunucu ayarla

   Tüm Gelen Verileri Çoğaltma işlevleri saklı yordamlar tarafından yapılır. Tüm yordamları [gelen verileri çoğaltma saklı yordamlar](reference-data-in-stored-procedures.md)' da bulabilirsiniz. Saklı yordamlar MySQL kabuğu veya MySQL çalışma ekranı 'nda çalıştırılabilir. 

   İki sunucuyu bağlamak ve çoğaltmaya başlamak için MySQL için Azure DB hizmetinde hedef çoğaltma sunucusunda oturum açın ve dış örneği ana sunucu olarak ayarlayın. Bu işlem, MySQL için Azure DB sunucusunda `mysql.az_replication_change_master` saklı yordamı kullanılarak yapılır.

   ```sql
   CALL mysql.az_replication_change_master('<master_host>', '<master_user>', '<master_password>', 3306, '<master_log_file>', <master_log_pos>, '<master_ssl_ca>');
   ```

   - master_host: ana sunucunun ana bilgisayar adı
   - master_user: Ana sunucu için Kullanıcı adı
   - master_password: Ana sunucu için parola
   - master_log_file: çalışan `show master status` ikili günlük dosyası adı
   - master_log_pos: çalışan `show master status` ikili günlük konumu
   - master_ssl_ca: CA sertifikasının bağlamı. SSL kullanmıyorsanız, boş bir dize geçirin.
       - Bu parametrenin bir değişken olarak iletilmesi önerilir. Daha fazla bilgi için aşağıdaki örneklere bakın.

> [!NOTE]
> Ana sunucu bir Azure VM 'de barındırılıyorsa, ana ve çoğaltma sunucularının birbirleriyle iletişim kurmasına izin vermek için "Azure hizmetlerine erişime Izin ver" seçeneğini "açık" olarak ayarlayın. Bu ayar, **bağlantı güvenliği** seçeneklerinden değiştirilebilir. Daha fazla bilgi için [Portal kullanarak güvenlik duvarı kurallarını yönetme](howto-manage-firewall-using-portal.md) bölümüne bakın.

   **Örnekler**

   *SSL ile çoğaltma*

   `@cert` değişkeni aşağıdaki MySQL komutları çalıştırılarak oluşturulur: 

   ```sql
   SET @cert = '-----BEGIN CERTIFICATE-----
   PLACE YOUR PUBLIC KEY CERTIFICATE’S CONTEXT HERE
   -----END CERTIFICATE-----'
   ```

   SSL ile çoğaltma, "companya.com" etki alanında barındırılan bir ana sunucu ve MySQL için Azure veritabanı 'nda barındırılan bir çoğaltma sunucusu arasında ayarlanır. Bu saklı yordam, çoğaltma üzerinde çalıştırılır. 

   ```sql
   CALL mysql.az_replication_change_master('master.companya.com', 'syncuser', 'P@ssword!', 3306, 'mysql-bin.000002', 120, @cert);
   ```
   *SSL olmadan çoğaltma*

   SSL olmadan çoğaltma, "companya.com" etki alanında barındırılan bir ana sunucu ve MySQL için Azure veritabanı 'nda barındırılan bir çoğaltma sunucusu arasında ayarlanır. Bu saklı yordam, çoğaltma üzerinde çalıştırılır.

   ```sql
   CALL mysql.az_replication_change_master('master.companya.com', 'syncuser', 'P@ssword!', 3306, 'mysql-bin.000002', 120, '');
   ```

1. Çoğaltmayı Başlat

   Çoğaltmayı başlatmak için `mysql.az_replication_start` saklı yordamını çağırın.

   ```sql
   CALL mysql.az_replication_start;
   ```

1. Çoğaltma durumunu denetleme

   Çoğaltma durumunu görüntülemek için Çoğaltma sunucusundaki [`show slave status`](https://dev.mysql.com/doc/refman/5.7/en/show-slave-status.html) komutunu çağırın.
    
   ```sql
   show slave status;
   ```

   `Slave_IO_Running` ve `Slave_SQL_Running` durumu "Yes" ise ve `Seconds_Behind_Master` değeri "0" ise, çoğaltma iyi çalışır. `Seconds_Behind_Master` çoğaltmanın ne kadar geç olduğunu gösterir. Değer "0" değilse, çoğaltmanın güncelleştirmeleri işlemesi anlamına gelir. 

## <a name="other-stored-procedures"></a>Diğer saklı yordamlar

### <a name="stop-replication"></a>Çoğaltmayı durdurma

Ana sunucu ve çoğaltma sunucusu arasında çoğaltmayı durdurmak için aşağıdaki saklı yordamı kullanın:

```sql
CALL mysql.az_replication_stop;
```

### <a name="remove-replication-relationship"></a>Çoğaltma ilişkisini kaldır

Ana ve çoğaltma sunucusu arasındaki ilişkiyi kaldırmak için aşağıdaki saklı yordamı kullanın:

```sql
CALL mysql.az_replication_remove_master;
```

### <a name="skip-replication-error"></a>Çoğaltma atlama hatası

Çoğaltma hatasını atlamak ve çoğaltmanın devam etmesini sağlamak için aşağıdaki saklı yordamı kullanın:
    
```sql
CALL mysql.az_replication_skip_counter;
```

## <a name="next-steps"></a>Sonraki adımlar
- MySQL için Azure veritabanı [gelen verileri çoğaltma](concepts-data-in-replication.md) hakkında daha fazla bilgi edinin. 
