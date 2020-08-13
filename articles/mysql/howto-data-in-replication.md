---
title: Veri çoğaltma yapılandırma-MySQL için Azure veritabanı
description: Bu makalede MySQL için Azure veritabanı 'nın Gelen Verileri Çoğaltma nasıl ayarlanacağı açıklanır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: how-to
ms.date: 8/7/2020
ms.openlocfilehash: f8dbdf87eef193540fd5c1bf9d9e7f3794ae46ce
ms.sourcegitcommit: c28fc1ec7d90f7e8b2e8775f5a250dd14a1622a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2020
ms.locfileid: "88168227"
---
# <a name="how-to-configure-azure-database-for-mysql-data-in-replication"></a>MySQL için Azure veritabanı 'nı yapılandırma Gelen Verileri Çoğaltma

Bu makalede, ana ve çoğaltma sunucularını yapılandırarak MySQL için Azure veritabanı 'nda [gelen verileri çoğaltma](concepts-data-in-replication.md) nasıl ayarlanacağı açıklanır. Bu makalede MySQL sunucuları ve veritabanları ile ilgili bir önceki deneyim olduğunu varsaymaktadır.

> [!NOTE]
> Sapma ücretsiz iletişim
>
> Microsoft, farklı ve üçlü ortamları destekler. Bu makale, _İkincil_sözcüğe başvurular içerir. Kullanım açısından [ücretsiz iletişim Için Microsoft Stil Kılavuzu](https://github.com/MicrosoftDocs/microsoft-style-guide/blob/master/styleguide/bias-free-communication.md) bunu bir exclusionword olarak tanır. Bu makalede, şu anda yazılımda görüntülenen sözcük olduğundan, bu makalede tutarlılık için kullanılır. Yazılım, sözcüğü kaldıracak şekilde güncelleniyorsa, bu makale hizalamayla olacak şekilde güncelleştirilir.
>

MySQL için Azure veritabanı hizmetinde bir çoğaltma oluşturmak için, [gelen verileri çoğaltma](concepts-data-in-replication.md) verileri Şirket içindeki bir ana MySQL sunucusundan, sanal makinelerde (VM) veya bulut veritabanı hizmetlerinde eşitler. Gelen Verileri Çoğaltma, MySQL’de yerel olan ikili günlük (binlog) dosya konumuna dayalı çoğaltmayı temel alır. Binlog çoğaltma hakkında daha fazla bilgi edinmek için [MySQL binlog çoğaltmasına genel bakış](https://dev.mysql.com/doc/refman/5.7/en/binlog-replication-configuration-overview.html)bölümüne bakın.

Bu makaledeki adımları gerçekleştirmeden önce, verileri çoğaltmanın [sınırlamalarını ve gereksinimlerini](concepts-data-in-replication.md#limitations-and-considerations) gözden geçirin.

## <a name="create-a-mysql-server-to-be-used-as-replica"></a>Çoğaltma olarak kullanılacak bir MySQL sunucusu oluşturma

1. MySQL için yeni bir Azure veritabanı sunucusu oluşturun

   Yeni bir MySQL sunucusu (örn.) oluşturun. "replica.mysql.database.azure.com"). Sunucu oluşturma için [Azure Portal kullanarak MySQL Için Azure veritabanı sunucusu oluşturma](quickstart-create-mysql-server-database-using-azure-portal.md) bölümüne bakın. Bu sunucu, Gelen Verileri Çoğaltma "çoğaltma" sunucusudur.

   > [!IMPORTANT]
   > MySQL için Azure veritabanı sunucusunun Genel Amaçlı veya bellek için Iyileştirilmiş fiyatlandırma katmanlarında oluşturulması gerekir.
   > 

1. Aynı kullanıcı hesaplarını ve ilgili ayrıcalıkları oluşturma

   Kullanıcı hesapları ana sunucudan Çoğaltma sunucusuna çoğaltılmaz. Kullanıcılara Çoğaltma sunucusuna erişimi sağlamayı planlıyorsanız, yeni oluşturulan MySQL için Azure veritabanı sunucusunda tüm hesapları ve ilgili ayrıcalıkları el ile oluşturmanız gerekir.

1. Ana sunucunun IP adresini çoğaltmanın güvenlik duvarı kurallarına ekleyin. 

   [Azure portalını](howto-manage-firewall-using-portal.md) veya [Azure CLI](howto-manage-firewall-using-cli.md)’yı kullanarak güvenlik duvarı kurallarını güncelleştirin.

## <a name="configure-the-master-server"></a>Ana sunucuyu yapılandırma
Aşağıdaki adımlar, şirket içinde barındırılan MySQL sunucusunu, bir sanal makinede veya Gelen Verileri Çoğaltma için diğer bulut sağlayıcıları tarafından barındırılan veritabanı hizmetinde hazırlar ve yapılandırır. Bu sunucu, veri içindeki çoğaltmada "ana" dır.


1. Devam etmeden önce [ana sunucu gereksinimlerini](concepts-data-in-replication.md#requirements) gözden geçirin. 

   Örneğin, ana sunucunun bağlantı noktası 3306 ' de gelen ve giden trafiğe izin verdiğinden ve ana sunucunun **Genel BIR IP adresi**olduğundan, DNS genel olarak erişilebilir olduğundan veya tam etki alanı adı (FQDN) olduğundan emin olun. 
   
   Başka bir makinede barındırılan MySQL komut satırı veya Azure portal kullanılabilir [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) gibi bir araçtan bağlanmayı deneyerek ana sunucuya bağlantıyı test edin.

1. İkili günlüğü aç

   Aşağıdaki komutu çalıştırarak, ana bilgisayarda ikili günlük özelliğinin etkinleştirilip etkinleştirilmediğini denetleyin: 

   ```sql
   SHOW VARIABLES LIKE 'log_bin';
   ```

   Değişken [`log_bin`](https://dev.mysql.com/doc/refman/8.0/en/replication-options-binary-log.html#sysvar_log_bin) "on" değeriyle döndürülürse, sunucunuzda ikili günlük etkin olur. 

   `log_bin`"Off" değeri ile döndürülürse, My. CNF Dosyanızı düzenleyerek ikili günlüğü açın `log_bin=ON` ve değişikliğin etkili olması için sunucunuzu yeniden başlatın.

1. Ana sunucu ayarları

   Gelen Verileri Çoğaltma, parametrenin `lower_case_table_names` ana ve çoğaltma sunucuları arasında tutarlı olmasını gerektirir. Bu parametre, MySQL için Azure veritabanı 'nda varsayılan olarak 1 ' dir. 

   ```sql
   SET GLOBAL lower_case_table_names = 1;
   ```

1. Yeni bir çoğaltma rolü oluşturma ve izin ayarlama

   Ana sunucuda, çoğaltma ayrıcalıklarıyla yapılandırılmış bir kullanıcı hesabı oluşturun. Bu, SQL komutları veya MySQL çalışma ekranı gibi bir araç aracılığıyla yapılabilir. Kullanıcı oluştururken belirtilmesi gereken SSL ile çoğaltmayı planlayıp planladığınızı düşünün. Ana sunucunuza [Kullanıcı hesaplarının nasıl ekleneceğini](https://dev.mysql.com/doc/refman/5.7/en/user-names.html) anlamak için MySQL belgelerine bakın. 

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

   **MySQL Workbench**

   MySQL çalışma ekranı 'nda çoğaltma rolünü oluşturmak için, **Yönetim** panelinden **Kullanıcılar ve ayrıcalıklar** panelini açın. Ardından **Hesap Ekle**' ye tıklayın. 
 
   ![Kullanıcılar ve ayrıcalıklar](./media/howto-data-in-replication/users_privileges.png)

   Kullanıcı **adını oturum açma adı** alanına yazın. 

   ![Kullanıcıyı Eşitle](./media/howto-data-in-replication/syncuser.png)
 
   **Yönetim rolleri** paneline tıklayın ve sonra **genel ayrıcalıklar**listesinden **çoğaltma bağımlı** öğesini seçin. Ardından, çoğaltma rolünü oluşturmak için **Uygula** ' ya tıklayın.

   ![Çoğaltma bağımlı](./media/howto-data-in-replication/replicationslave.png)

1. Ana sunucuyu salt okunurdur moduna ayarlama

   Veritabanının dökümünü başlatmadan önce, sunucunun salt okunurdur modunda yerleştirilmesi gerekir. Salt okuma modundayken, ana öğe herhangi bir yazma işlemini işleyemez. İşletmenizin etkisini değerlendirin ve gerekirse, salt okuma penceresini yoğun olmayan bir zamanda zamanlayın.

   ```sql
   FLUSH TABLES WITH READ LOCK;
   SET GLOBAL read_only = ON;
   ```

1. İkili günlük dosyası adı ve sapmasını al

   [`show master status`](https://dev.mysql.com/doc/refman/5.7/en/show-master-status.html)Geçerli ikili günlük dosyası adını ve sapmasını öğrenmek için komutunu çalıştırın.
    
   ```sql
   show master status;
   ```
   Sonuçlar aşağıdaki gibi olmalıdır. Sonraki adımlarda kullanılacak ikili dosya adını unutmayın.

   ![Ana durum sonuçları](./media/howto-data-in-replication/masterstatus.png)
 
## <a name="dump-and-restore-master-server"></a>Ana sunucu dökümünü al ve geri yükle

1. MySQL için Azure veritabanı 'na çoğaltmak istediğiniz veritabanlarını ve tabloları belirleme ve ana sunucudan döküm gerçekleştirme.
 
    Yöneticinizdeki veritabanlarının dökümünü yapmak için mysqldump kullanabilirsiniz. Ayrıntılar için, [döküm & geri yükleme](concepts-migrate-dump-restore.md)bölümüne bakın. MySQL kitaplığı ve test kitaplığının dökümünü almak gereksizdir.

1. Ana sunucuyu okuma/yazma moduna ayarla

   Veritabanı kaydedildikten sonra ana MySQL sunucusunu okuma/yazma moduna geri çevirin.

   ```sql
   SET GLOBAL read_only = OFF;
   UNLOCK TABLES;
   ```

1. Döküm dosyasını yeni sunucuya geri yükle

   Döküm dosyasını MySQL için Azure veritabanı hizmetinde oluşturulan sunucuya geri yükleyin. Bir MySQL sunucusuna bir döküm dosyası geri yüklemek için [döküm & geri yükleme](concepts-migrate-dump-restore.md) bölümüne bakın. Döküm dosyası büyükse, Çoğaltma sunucunuz ile aynı bölgedeki Azure 'daki bir sanal makineye yükleyin. Sanal makineden MySQL için Azure veritabanı sunucusuna geri yükleyin.

## <a name="link-master-and-replica-servers-to-start-data-in-replication"></a>Ana ve çoğaltma sunucularını başlatma Gelen Verileri Çoğaltma bağlama

1. Ana sunucu ayarla

   Tüm Gelen Verileri Çoğaltma işlevleri saklı yordamlar tarafından yapılır. Tüm yordamları [gelen verileri çoğaltma saklı yordamlar](reference-data-in-stored-procedures.md)' da bulabilirsiniz. Saklı yordamlar MySQL kabuğu veya MySQL çalışma ekranı 'nda çalıştırılabilir. 

   İki sunucuyu bağlamak ve çoğaltmaya başlamak için MySQL için Azure DB hizmetinde hedef çoğaltma sunucusunda oturum açın ve dış örneği ana sunucu olarak ayarlayın. Bu işlem, `mysql.az_replication_change_master` MySQL Için Azure DB sunucusunda saklı yordam kullanılarak yapılır.

   ```sql
   CALL mysql.az_replication_change_master('<master_host>', '<master_user>', '<master_password>', 3306, '<master_log_file>', <master_log_pos>, '<master_ssl_ca>');
   ```

   - master_host: ana sunucunun ana bilgisayar adı
   - master_user: Ana sunucu için Kullanıcı adı
   - master_password: Ana sunucu için parola
   - master_log_file: çalışan ikili günlük dosyası adı`show master status`
   - master_log_pos: sıfırdan ikili günlük konumu`show master status`
   - master_ssl_ca: CA sertifikasının bağlamı. SSL kullanmıyorsanız, boş bir dize geçirin.
       - Bu parametrenin bir değişken olarak iletilmesi önerilir. Daha fazla bilgi için aşağıdaki örneklere bakın.

   > [!NOTE]
   > Ana sunucu bir Azure VM 'de barındırılıyorsa, ana ve çoğaltma sunucularının birbirleriyle iletişim kurmasına izin vermek için "Azure hizmetlerine erişime Izin ver" seçeneğini "açık" olarak ayarlayın. Bu ayar, **bağlantı güvenliği** seçeneklerinden değiştirilebilir. Daha fazla bilgi için [Portal kullanarak güvenlik duvarı kurallarını yönetme](howto-manage-firewall-using-portal.md) bölümüne bakın.
      
   **Örnekler**
   
   *SSL ile çoğaltma*
   
   Değişkeni `@cert` aşağıdaki MySQL komutları çalıştırılarak oluşturulur: 
   
      ```sql
      SET @cert = '-----BEGIN CERTIFICATE-----
      PLACE YOUR PUBLIC KEY CERTIFICATE'`S CONTEXT HERE
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

1. Filtreleme 
 
   Ana bilgisayarınızdan bazı tabloların çoğaltılmasını atlamak istiyorsanız, `replicate_wild_ignore_table` çoğaltma sunucunuzdaki sunucu parametresini güncelleştirin. Virgülle ayrılmış bir liste kullanarak birden fazla tablo kalıbı sağlayabilirsiniz.

   Bu parametre hakkında daha fazla bilgi edinmek için [MySQL belgelerini](https://dev.mysql.com/doc/refman/8.0/en/replication-options-replica.html#option_mysqld_replicate-wild-ignore-table) gözden geçirin. 
    
   Parametreyi güncelleştirmek için [Azure Portal](howto-server-parameters.md) veya [Azure CLI](howto-configure-server-parameters-using-cli.md)kullanabilirsiniz.

1. Çoğaltmayı Başlat

   `mysql.az_replication_start`Çoğaltmayı başlatmak için saklı yordamı çağırın.

   ```sql
   CALL mysql.az_replication_start;
   ```

1. Çoğaltma durumunu denetleme

   [`show slave status`](https://dev.mysql.com/doc/refman/5.7/en/show-slave-status.html)Çoğaltma durumunu görüntülemek için Çoğaltma sunucusunda komutunu çağırın.
    
   ```sql
   show slave status;
   ```

   `Slave_IO_Running`Ve durumu `Slave_SQL_Running` "Yes" ise ve değeri `Seconds_Behind_Master` "0" ise, çoğaltma iyi çalışır. `Seconds_Behind_Master`çoğaltmanın ne kadar geç olduğunu gösterir. Değer "0" değilse, çoğaltmanın güncelleştirmeleri işlemesi anlamına gelir. 

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
