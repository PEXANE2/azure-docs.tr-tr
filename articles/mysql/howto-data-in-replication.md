---
title: Veri çoğaltma yapılandırma - MySQL için Azure Veritabanı
description: Bu makalede, MySQL için Azure Veritabanı için Veri Çoğaltma nasıl ayarlanır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 3/27/2020
ms.openlocfilehash: 2148ce41267627d9d6e0437897a99a8dbdbe0746
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2020
ms.locfileid: "80382775"
---
# <a name="how-to-configure-azure-database-for-mysql-data-in-replication"></a>MySQL Data-in Replication için Azure Veritabanı nasıl yapılandırılır?

Bu makalede, ana ve çoğaltma sunucularını yapılandırarak MySQL hizmeti için Azure Veritabanı'nda Veri Çoğaltma'yı nasıl ayarlayabileceğinizi öğreneceksiniz. Data-in Replication, diğer bulut sağlayıcıları tarafından barındırılan çevrimiçi makinelerde veya diğer bulut sağlayıcıları tarafından barındırılan veritabanı hizmetlerinde şirket içinde çalışan ana MySQL sunucusundan gelen verileri MySQL hizmeti için Azure Veritabanı'nda bir yinelemeyle eşitlemenize olanak tanır. 

Bu makalede, MySQL sunucuları ve veritabanları ile en azından bazı önceki deneyime sahip olduğunu varsayar.

Bu makaledeki adımları gerçekleştirmeden önce Veri [çoğaltmasınırlamalarını ve gereksinimlerini](concepts-data-in-replication.md#limitations-and-considerations) gözden geçirin.

## <a name="create-a-mysql-server-to-be-used-as-replica"></a>Çoğaltma olarak kullanılmak üzere bir MySQL sunucusu oluşturma

1. MySQL sunucusu için yeni bir Azure Veritabanı oluşturma

   Yeni bir MySQL sunucusu oluşturun (ör. "replica.mysql.database.azure.com"). Sunucu oluşturma [için Azure portalını kullanarak MySQL sunucusu için Azure Veritabanı Oluşturma'ya](quickstart-create-mysql-server-database-using-azure-portal.md) bakın. Bu sunucu, Data-in Replication'daki "çoğaltma" sunucusudur.

   > [!IMPORTANT]
   > MySQL sunucusu için Azure Veritabanı, Genel Amaç veya Bellek Optimize Edilmiş fiyatlandırma katmanlarında oluşturulmalıdır.
   > 

2. Aynı kullanıcı hesapları ve ilgili ayrıcalıklar oluşturma

   Kullanıcı hesapları ana sunucudan çoğaltma sunucusuna çoğaltılamaz. Kullanıcılara çoğaltma sunucusuna erişim sağlamayı planlıyorsanız, MySQL sunucusu için yeni oluşturulan bu Azure Veritabanı'nda tüm hesapları ve ilgili ayrıcalıkları el ile oluşturmanız gerekir.

3. Ana sunucunun IP adresini yinelemenin güvenlik duvarı kurallarına ekleyin. 

   [Azure portalını](howto-manage-firewall-using-portal.md) veya [Azure CLI](howto-manage-firewall-using-cli.md)’yı kullanarak güvenlik duvarı kurallarını güncelleştirin.

## <a name="configure-the-master-server"></a>Ana sunucuyu yapılandırma
Aşağıdaki adımlar, çevrimiçi olarak barındırılan MySQL sunucusunu sanal bir makinede veya Diğer Bulut Sağlayıcıları tarafından Veri Çoğaltma için barındırılan veritabanı hizmetinde hazırlar ve yapılandırır. Bu sunucu, Veri çoğaltma "ana" dır.


1. Devam etmeden önce [ana sunucu gereksinimlerini](concepts-data-in-replication.md#requirements) gözden geçirin. 

   Örneğin, ana sunucunun bağlantı noktası 3306'da hem gelen hem de giden trafiğe izin verdiğinden ve ana sunucunun **genel bir IP adresine**sahip olduğundan, DNS'nin herkese açık olduğundan veya tam nitelikli bir etki alanı adı (FQDN) olduğundan emin olun. 
   
   Başka bir makinede barındırılan MySQL komut satırı gibi bir araçtan veya Azure portalında bulunan [Azure Bulut BulutU'ndan](https://docs.microsoft.com/azure/cloud-shell/overview) bağlanmaya çalışarak ana sunucuya bağlantıyı test edin 

2. İkili günlüğe kaydetmeyi açma

   Aşağıdaki komutu çalıştırarak ana ustaüzerinde ikili günlüğe kaydetmenin etkin olup olmadığını denetleyin: 

   ```sql
   SHOW VARIABLES LIKE 'log_bin';
   ```

   Değişken [`log_bin`](https://dev.mysql.com/doc/refman/8.0/en/replication-options-binary-log.html#sysvar_log_bin) "A)" değeriyle döndürülürse, sunucunuzda ikili günlüğe kaydetme etkinleştirilir. 

   "KAPALI" değeriyle `log_bin` döndürülürse, my.cnf dosyanızı düzenleyerek `log_bin=ON` ikili günlüğe kaydetmeyi açın ve değişikliğin etkili olması için sunucunuzu yeniden başlatın.

3. Ana sunucu ayarları

   Veri Çoğaltma ana ve `lower_case_table_names` çoğaltma sunucuları arasında tutarlı olması için parametre gerektirir. Bu parametre MySQL için Azure Veritabanı'nda varsayılan olarak 1'dir. 

   ```sql
   SET GLOBAL lower_case_table_names = 1;
   ```

4. Yeni bir çoğaltma rolü oluşturma ve izin ayarlama

   Çoğaltma ayrıcalıklarıyla yapılandırılan ana sunucuda bir kullanıcı hesabı oluşturun. Bu, SQL komutları veya MySQL Workbench gibi bir araç aracılığıyla yapılabilir. Kullanıcı yı oluştururken bunun belirtilmesi gerekeceği için SSL ile çoğaltmayı planlayıp planlamadığınızı düşünün. Ana sunucunuza nasıl kullanıcı [hesapları](https://dev.mysql.com/doc/refman/5.7/en/adding-users.html) ekleyeceğinizhakkında bilgi almak için MySQL belgelerine bakın. 

   Aşağıdaki komutlarda, oluşturulan yeni çoğaltma rolü ana makineden erişebiliyor, sadece ana makineyi barındıran makineye değil. Bu, create user komutunda "syncuser@'"" belirtilerek yapılır. [Hesap adlarını belirtme](https://dev.mysql.com/doc/refman/5.7/en/account-names.html)hakkında daha fazla bilgi edinmek için MySQL belgelerine bakın.

   **SQL Komutu**

   *SSL ile çoğaltma*

   Tüm kullanıcı bağlantıları için SSL'yi kullanmak için bir kullanıcı oluşturmak için aşağıdaki komutu kullanın: 

   ```sql
   CREATE USER 'syncuser'@'%' IDENTIFIED BY 'yourpassword';
   GRANT REPLICATION SLAVE ON *.* TO ' syncuser'@'%' REQUIRE SSL;
   ```

   *SSL olmadan çoğaltma*

   Tüm bağlantılar için SSL gerekli değilse, bir kullanıcı oluşturmak için aşağıdaki komutu kullanın:

   ```sql
   CREATE USER 'syncuser'@'%' IDENTIFIED BY 'yourpassword';
   GRANT REPLICATION SLAVE ON *.* TO ' syncuser'@'%';
   ```

   **MySQL Workbench**

   MySQL Workbench'te çoğaltma rolünü oluşturmak için **Yönetim** panelinden **Kullanıcılar ve Ayrıcalıklar** panelini açın. Ardından **Hesap Ekle'ye**tıklayın. 
 
   ![Kullanıcılar ve Ayrıcalıklar](./media/howto-data-in-replication/users_privileges.png)

   Kullanıcı adını **Giriş Adı** alanına yazın. 

   ![Kullanıcıyı senkronize edin](./media/howto-data-in-replication/syncuser.png)
 
   **İdari Görevler** paneline tıklayın ve ardından **Genel Ayrıcalıklar**listesinden Çoğaltma Kölesi'ni seçin. **Replication Slave** Ardından çoğaltma rolünü oluşturmak için **Uygula'yı** tıklatın.

   ![Çoğaltma Kölesi](./media/howto-data-in-replication/replicationslave.png)


5. Ana sunucuyu salt okunur moduna ayarlama

   Veritabanını boşaltmaya başlamadan önce, sunucunun salt okunur moduna alınması gerekir. Salt okunur modundayken, asıl kişi yazma hareketlerini işleyemez. İşletmenizin etkisini değerlendirin ve gerekirse yalnızca okunan pencereyi yoğun olmayan bir zamanda zamanlayın.

   ```sql
   FLUSH TABLES WITH READ LOCK;
   SET GLOBAL read_only = ON;
   ```

6. İkili günlük dosya adı ve mahsup alma

   Geçerli [`show master status`](https://dev.mysql.com/doc/refman/5.7/en/show-master-status.html) ikili günlük dosya adını ve ofset belirlemek için komutu çalıştırın.
    
   ```sql
   show master status;
   ```
   Sonuçlar aşağıdaki gibi olmalıdır. Daha sonraki adımlarda kullanılacağı gibi ikili dosya adını not emin olun.

   ![Yüksek Lisans Durum Sonuçları](./media/howto-data-in-replication/masterstatus.png)
 
## <a name="dump-and-restore-master-server"></a>Ana sunucuyı boşaltma ve geri yükleme

1. Ana sunucudan tüm veritabanlarını dökümü

   Mysqldump'u kullanarak veritabanını üstten dökümü yapabilirsiniz. Ayrıntılar için [Dump & Geri Yükleme'ye](concepts-migrate-dump-restore.md)bakın. MySQL kitaplığını ve test kitaplığını dökümü gereksizdir.

2. Ana sunucunun okuma/yazma moduna ayarla

   Veritabanı boşaltıldıktan sonra, ana MySQL sunucusunu okuma/yazma moduna geri değiştirin.

   ```sql
   SET GLOBAL read_only = OFF;
   UNLOCK TABLES;
   ```

3. Döküm dosyasını yeni sunucuya geri yükleme

   Dökümü dosyasını MySQL hizmeti için Azure Veritabanı'nda oluşturulan sunucuya geri yükleyin. Bir döküm dosyasını MySQL sunucusuna nasıl geri yükleyin için [Dökümü & Geri Yükle'ye](concepts-migrate-dump-restore.md) bakın. Döküm dosyası büyükse, dosyayı çoğaltma sunucunuzla aynı bölgede Azure'daki sanal bir makineye yükleyin. Sanal makineden MySQL sunucusu için Azure Veritabanı'na geri yükleyin.

## <a name="link-master-and-replica-servers-to-start-data-in-replication"></a>Veri çoğaltma başlatmak için bağlantı ana ve çoğaltma sunucuları

1. Ana sunucu ayarlama

   Tüm Data-in Çoğaltma işlevleri depolanan yordamlar tarafından yapılır. Tüm yordamları Veri [Çoğaltma Saklı Yordamları'nda](reference-data-in-stored-procedures.md)bulabilirsiniz. Depolanan yordamlar MySQL kabuğunda veya MySQL Workbench'te çalıştırılabilir. 

   İki sunucuyu bağlamak ve çoğaltmayı başlatmak için MySQL hizmeti için Azure DB'deki hedef çoğaltma sunucusuna giriş yapın ve dış örneği ana sunucu olarak ayarlayın. Bu, MySQL `mysql.az_replication_change_master` sunucusu için Azure DB'de depolanan yordam kullanılarak yapılır.

   ```sql
   CALL mysql.az_replication_change_master('<master_host>', '<master_user>', '<master_password>', 3306, '<master_log_file>', <master_log_pos>, '<master_ssl_ca>');
   ```

   - master_host: ana sunucunun hostname
   - master_user: ana sunucu için kullanıcı adı
   - master_password: ana sunucu için parola
   - master_log_file: çalışan ikili günlük dosya adı`show master status`
   - master_log_pos: çalışan ikili günlük konumu`show master status`
   - master_ssl_ca: CA sertifikasının bağlamı. SSL kullanmıyorsanız, boş dize geçirin.
       - Bu parametrenin değişken olarak geçirilmesi önerilir. Daha fazla bilgi için aşağıdaki örneklere bakın.

> [!NOTE]
> Ana sunucu bir Azure VM'de barındırılıyorsa, ana sunucuve yineleme sunucuların birbirleriyle iletişim kurmasına izin vermek için "Azure hizmetlerine erişime izin verin" olarak "A)'yı ayarlayın. Bu ayar **Bağlantı güvenlik** seçeneklerinden değiştirilebilir. Daha fazla bilgi için [portalı kullanarak güvenlik duvarı kurallarını yönetmeye](howto-manage-firewall-using-portal.md) bakın.

   **Örnekler**

   *SSL ile çoğaltma*

   Değişken `@cert` aşağıdaki MySQL komutları çalıştırılarak oluşturulur: 

   ```sql
   SET @cert = '-----BEGIN CERTIFICATE-----
   PLACE YOUR PUBLIC KEY CERTIFICATE'`S CONTEXT HERE
   -----END CERTIFICATE-----'
   ```

   SSL ile çoğaltma, "companya.com" etki alanında barındırılan bir ana sunucu ile MySQL için Azure Veritabanı'nda barındırılan bir çoğaltma sunucusu arasında ayarlanır. Bu depolanan yordam çoğaltma üzerinde çalıştırılır. 

   ```sql
   CALL mysql.az_replication_change_master('master.companya.com', 'syncuser', 'P@ssword!', 3306, 'mysql-bin.000002', 120, @cert);
   ```
   *SSL olmadan çoğaltma*

   SSL olmadan çoğaltma, "companya.com" etki alanında barındırılan bir ana sunucu ile MySQL için Azure Veritabanı'nda barındırılan bir çoğaltma sunucusu arasında ayarlanır. Bu depolanan yordam çoğaltma üzerinde çalıştırılır.

   ```sql
   CALL mysql.az_replication_change_master('master.companya.com', 'syncuser', 'P@ssword!', 3306, 'mysql-bin.000002', 120, '');
   ```

1. Çoğaltmayı başlat

   Çoğaltmayı `mysql.az_replication_start` başlatmak için depolanan yordamı arayın.

   ```sql
   CALL mysql.az_replication_start;
   ```

1. Çoğaltma durumunu denetleme

   Çoğaltma [`show slave status`](https://dev.mysql.com/doc/refman/5.7/en/show-slave-status.html) durumunu görüntülemek için çoğaltma sunucusundaki komutu arayın.
    
   ```sql
   show slave status;
   ```

   "Evet" `Slave_IO_Running` durumu `Slave_SQL_Running` ve değeri "0" `Seconds_Behind_Master` ise, çoğaltma iyi çalışıyor. `Seconds_Behind_Master`yinelemenin ne kadar geç olduğunu gösterir. Değer "0" değilse, yineleme güncelleştirmeleri işliyor demektir. 

## <a name="other-stored-procedures"></a>Diğer depolanan yordamlar

### <a name="stop-replication"></a>Çoğaltmayı durdurma

Ana ve çoğaltma sunucusu arasındaki çoğaltmayı durdurmak için aşağıdaki depolanan yordamı kullanın:

```sql
CALL mysql.az_replication_stop;
```

### <a name="remove-replication-relationship"></a>Çoğaltma ilişkisini kaldırma

Ana ve yineleme sunucusu arasındaki ilişkiyi kaldırmak için aşağıdaki depolanmış yordamı kullanın:

```sql
CALL mysql.az_replication_remove_master;
```

### <a name="skip-replication-error"></a>Çoğaltma hataatlama

Çoğaltma hatasını atlamak ve çoğaltmaişleminin devam etmesine izin vermek için aşağıdaki depolanmış yordamı kullanın:
    
```sql
CALL mysql.az_replication_skip_counter;
```

## <a name="next-steps"></a>Sonraki adımlar
- MySQL için Azure Veritabanı için [Veri Çoğaltma](concepts-data-in-replication.md) hakkında daha fazla bilgi edinin. 
