---
title: MySQL için Azure veritabanı 'nda ana sürüm yükseltmesi-tek sunucu
description: Bu makalede, MySQL için Azure veritabanı-tek sunucu için ana sürümü nasıl yükselteceğiniz açıklanmaktadır
author: Bashar-MSFT
ms.author: bahusse
ms.service: mysql
ms.topic: how-to
ms.date: 1/28/2021
ms.openlocfilehash: 62faaed3672f721b26587d1bca3ddb0947f733e7
ms.sourcegitcommit: 54e1d4cdff28c2fd88eca949c2190da1b09dca91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2021
ms.locfileid: "99220845"
---
# <a name="major-version-upgrade-in-azure-database-for-mysql-single-server"></a>MySQL için Azure veritabanı tek sunucu 'da ana sürüm yükseltmesi

> [!NOTE]
> Bu makale, Microsoft 'un artık kullandığı bir terim olan _bağımlı_ dönem başvuruları içerir. Terim yazılımlardan kaldırıldığında, bu makaleyi kaldıracağız.
>

> [!IMPORTANT]
> MySQL için Azure veritabanı tek sunucu için ana sürüm yükseltmesi genel önizlemede.

Bu makalede MySQL ana sürümünüzü, MySQL için Azure veritabanı tek sunucu 'da nasıl yükseltebileceğinizi açıklamaktadır.

Bu özellik, müşterilerin MySQL 5,6 sunucularının herhangi bir veri hareketi olmadan veya herhangi bir uygulama bağlantı dizesi değişikliğine gerek kalmadan bir tıklama düğmesine sahip MySQL 5,7 ' e yerinde yükseltmelerini gerçekleştirmesini sağlayacaktır.

> [!Note]
> * Ana sürüm yükseltmesi yalnızca MySQL 5,6 ' den MySQL 5,7 ' ye ana sürüm yükseltmesi için kullanılabilir.
> * Sunucu, yükseltme işlemi boyunca kullanılamaz olacaktır. Bu nedenle, planlı bakım pencereniz sırasında yükseltmeleri gerçekleştirmeniz önerilir. [Okuma çoğaltması kullanarak mysql 5,6 ' den mysql 5,7 ' e en düşük kapalı kalma sürümü yükseltmesi](#perform-minimal-downtime-major-version-upgrade-from-mysql-56-to-mysql-57-using-read-replicas) yapmayı düşünebilirsiniz.

## <a name="perform-major-version-upgrade-from-mysql-56-to-mysql-57-using-azure-portal"></a>Azure portal kullanarak MySQL 5,6 ' den MySQL 5,7 ' e büyük sürüm yükseltmesi gerçekleştirin

Azure portal kullanarak MySQL 5,6 Server için Azure veritabanınız için ana sürüm yükseltmesini gerçekleştirmek üzere bu adımları izleyin

> [!IMPORTANT]
> Üretimi doğrudan yükseltmek yerine, ilk olarak sunucunun geri yüklenen kopyasında yükseltmeyi gerçekleştirmenizi öneririz. Bkz. [Noktadan noktaya geri yükleme gerçekleştirme](howto-restore-server-portal.md#point-in-time-restore).

1. [Azure Portal](https://portal.azure.com/), var olan MySQL 5,6 Server Için Azure veritabanınızı seçin.

2. **Genel bakış** sayfasında, araç çubuğundaki **Yükselt** düğmesine tıklayın.

3. **Yükseltme** bölümünde MySQL 5,6 Server için Azure veritabanı 'nı 5,7 sunucusuna yükseltmek üzere **Tamam** ' ı seçin.

   :::image type="content" source="./media/how-to-major-version-upgrade-portal/upgrade.png" alt-text="MySQL için Azure veritabanı-genel bakış-yükseltme":::

4. Bir bildirim, yükseltmenin başarılı olduğunu doğrulayacaktır.


## <a name="perform-major-version-upgrade-from-mysql-56-to-mysql-57-using-azure-cli"></a>Azure CLı kullanarak MySQL 5,6 ' den MySQL 5,7 ' e büyük sürüm yükseltmesi gerçekleştirme

Azure CLı kullanarak MySQL 5,6 Server Azure veritabanınız için ana sürüm yükseltmesini gerçekleştirmek üzere bu adımları izleyin

> [!IMPORTANT]
> Üretimi doğrudan yükseltmek yerine, ilk olarak sunucunun geri yüklenen kopyasında yükseltmeyi gerçekleştirmenizi öneririz. Bkz. [Noktadan noktaya geri yükleme gerçekleştirme](howto-restore-server-cli.md#server-point-in-time-restore).

1. Yükseltme komutlarını çalıştırmak için [Windows Için Azure CLI](/cli/azure/install-azure-cli) 'yı veya [Azure Cloud Shell](../cloud-shell/overview.md) ' de Azure CLI 'yi kullanın. 
 
   Bu yükseltme, Azure CLı 'nin sürüm 2.16.0 veya üstünü gerektirir. Azure Cloud Shell kullanılıyorsa, en son sürüm zaten yüklüdür. Yüklü sürümü ve bağımlı kitaplıkları bulmak için az version komutunu çalıştırın. En son sürüme yükseltmek için az upgrade komutunu çalıştırın.

2. Oturum açtıktan sonra [az MySQL Server Upgrade](https://docs.microsoft.com/cli/azure/mysql/server?view=azure-cli-latest#az_mysql_server_upgrade&preserve-view=true) komutunu çalıştırın:

   ```azurecli
   az mysql server upgrade --name testsvr --resource-group testgroup --subscription MySubscription --target-server-version 5.7"
   ```
   
   Komut istemi "-çalışıyor" iletisini gösterir. Bu ileti artık gösterilmediğinde, sürüm yükseltmesi tamamlanmıştır.

## <a name="perform-major-version-upgrade-from-mysql-56-to-mysql-57-on-read-replica-using-azure-portal"></a>Azure portal kullanarak MySQL 5,6 ' den MySQL 5,7 ' ye ana sürüm yükseltmesini gerçekleştirin

1. [Azure Portal](https://portal.azure.com/), MySQL Için mevcut Azure veritabanı 5,6 okuma çoğaltma sunucusunu seçin.

2. **Genel bakış** sayfasında, araç çubuğundaki **Yükselt** düğmesine tıklayın.

3. **Yükseltme** bölümünde, MySQL için Azure veritabanı 5,6 okuma çoğaltması sunucusunu 5,7 sunucusuna yükseltmek için **Tamam** ' ı seçin.

   :::image type="content" source="./media/how-to-major-version-upgrade-portal/upgrade.png" alt-text="MySQL için Azure veritabanı-genel bakış-yükseltme":::

4. Bir bildirim, yükseltmenin başarılı olduğunu doğrulayacaktır.

5. **Genel bakış** sayfasında, MySQL için Azure veritabanı okuma çoğaltma sunucusu sürümünün 5,7 olduğunu doğrulayın.

6. Şimdi birincil sunucunuza gidin ve üzerinde [ana sürüm yükseltmesi gerçekleştirin](#perform-major-version-upgrade-from-mysql-56-to-mysql-57-using-azure-portal) .

## <a name="perform-minimal-downtime-major-version-upgrade-from-mysql-56-to-mysql-57-using-read-replicas"></a>MySQL 5,6 ' den MySQL 5,7 ' den, okuma çoğaltmaları kullanarak en düşük kapalı kalma süresi büyük sürümü

MySQL 5,6 ' den MySQL 5,7 ' den, okuma Çoğaltmalarından yararlanarak en düşük kesinti süresi ana sürüm yükseltmesi gerçekleştirebilirsiniz. Bu düşünce, ilk olarak sunucunuzun okuma çoğaltmasını 5,7 ve daha sonraki bir sürüme yükselterek uygulamanızı okumak ve yeni bir birincil hale getirmek için yük devretmeye yönelik bir uygulamadır.

1. [Azure Portal](https://portal.azure.com/), MySQL Için mevcut Azure veritabanı 5,6 ' i seçin.

2. Birincil sunucudan bir [okuma çoğaltması](https://docs.microsoft.com/azure/mysql/concepts-read-replicas#create-a-replica) oluşturun.

3. [Okuma çoğaltmalarınızı](#perform-major-version-upgrade-from-mysql-56-to-mysql-57-on-read-replica-using-azure-portal) 5,7 sürümüne yükseltin.

4. Çoğaltma sunucusunun 5,7. sürümde çalıştığını onaylamak için, uygulamanızın birincil sunucunuza bağlanmasını durdurun.
 
5. Çoğaltma durumunu denetleyin ve çoğaltmanın tümünün birinciyle yapıldığından emin olun; böylece tüm veriler eşitlenir ve birincil içinde yeni işlem gerçekleştirilmemesini sağlayın.

   [`show slave status`](https://dev.mysql.com/doc/refman/5.7/en/show-slave-status.html)Çoğaltma durumunu görüntülemek için Çoğaltma sunucusunda komutunu çağırın.

   ```sql
   SHOW SLAVE STATUS\G
   ```

   `Slave_IO_Running`Ve durumu `Slave_SQL_Running` "Yes" ise ve değeri `Seconds_Behind_Master` "0" ise, çoğaltma iyi çalışır. `Seconds_Behind_Master` çoğaltmanın ne kadar geç olduğunu gösterir. Değer "0" değilse, çoğaltmanın güncelleştirmeleri işlemesi anlamına gelir. `Seconds_Behind_Master`"0" olarak onaylandıktan sonra çoğaltmayı durdurmak güvenlidir.

6. [Çoğaltmayı durdurarak](https://docs.microsoft.com/azure/mysql/howto-read-replicas-portal#stop-replication-to-a-replica-server)okuma çoğaltmanıza birincili yükseltin.

7. Uygulamanızı Server 5,7 çalıştıran yeni birincil (Eski çoğaltma) olarak işaretleyin. Her sunucunun benzersiz bir bağlantı dizesi vardır. Uygulamanızı kaynak yerine (eski) çoğaltmaya işaret etmek üzere güncelleştirin.

> [!Note]
> Bu senaryonun yalnızca 4, 5 ve 6. adımlarda kapalı kalma süresi olacaktır.


## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="when-will-this-upgrade-feature-be-ga-as-we-have-mysql-v56-in-our-production-environment-that-we-need-to-upgrade"></a>Bu yükseltme özelliği, yükseltmemiz gereken üretim ortamımızda MySQL v 5.6 'ümüzü ne zaman GA olacak?

Bu özelliğin GA 'i MySQL v 5.6 kullanımdan kaldırmadan önce planlanmaktadır. Ancak, bu özellik üretim için hazırlayın ve Azure tarafından tam olarak desteklenerek ortamınızda güvenle çalıştırmalısınız. Önerilen en iyi yöntem olarak, yükseltme sırasında kapalı kalma süresini tahmin etmek ve üretim ortamında çalıştırmadan önce uygulama uyumluluk testi gerçekleştirmek için önce bunu bir sunucunun geri yüklenmiş kopyasında çalıştırıp test etmenize kesinlikle tavsiye ederiz. Daha fazla bilgi için, sunucunuzun zaman içindeki kopyasını oluşturmak üzere [Noktadan noktaya geri yükleme gerçekleştirme](howto-restore-server-portal.md#point-in-time-restore) konusuna bakın. 

### <a name="will-this-cause-downtime-of-the-server-and-if-so-how-long"></a>Bu, sunucu kapalı kalma süresine neden olur, ne kadar süreyle?

Evet, yükseltme işlemi sırasında sunucu kullanılamıyor olacaktır, bu sayede planlı bakım pencerenizde bu işlemi gerçekleştirmenizi öneririz. Tahmini kapalı kalma süresi, veritabanı boyutuna, sağlanan depolama boyutuna (IOPS 'nin sağlandığı) ve veritabanındaki tablo sayısına bağlıdır. Yükseltme süresi, sunucudaki tablo sayısıyla doğrudan orantılıdır. Temel SKU sunucularının yükseltmeleri, standart depolama platformunda olduğu için daha uzun sürme beklenmektedir. Sunucu ortamınız için kapalı kalma süresini tahmin etmek için, önce sunucunun geri yüklenen kopyasında yükseltmeyi gerçekleştirmenizi öneririz.  

### <a name="it-is-noted-that-it-is-not-supported-on-replica-server-yet-what-does-that-mean-concrete"></a>Henüz Çoğaltma sunucusunda desteklenmediğini not edin. Bu durum somut anlamı nedir?

Şu anda, birincil sürüm yükseltmesi çoğaltma sunucusu için desteklenmez. Bu, Çoğaltma sunucusunda (kaynak veya çoğaltma sunucusu) yer alan sunucular için çalıştırılmamalısınız. Yükseltme özelliği için çoğaltma desteğini eklemeden önce çoğaltmaya dahil olan sunucuların yükseltmesini test etmek isterseniz, aşağıdaki adımları öneriyoruz:

1. Planlı bakım sırasında, ad ve tüm yapılandırma bilgileri (kaynak sunucudan farklıysa sunucu parametre yapılandırması) yakalandıktan sonra [çoğaltmayı durdurun ve çoğaltma sunucusunu silin](howto-read-replicas-portal.md) .
2. Kaynak sunucunun yükseltmesini gerçekleştirin.
3. Adım 1 ' de yakalanan aynı ad ve yapılandırma ayarlarına sahip yeni bir okuma çoğaltması sunucusu sağlayın. Kaynak sunucu v 5.7 sürümüne yükseltildikten sonra yeni çoğaltma sunucusu v 5.7 'de otomatik olarak olacaktır.

### <a name="what-will-happen-if-we-do-not-choose-to-upgrade-our-mysql-v56-server-before-february-5-2021"></a>MySQL v 5.6 sunucusunu 5 Şubat 2021 tarihinden önce yükseltmeyi seçemiz olduğunda ne olur?

Daha önce olduğu gibi MySQL v 5.6 sunucusunu çalıştırmaya devam edebilirsiniz. Azure **, sunucunuzda hiçbir yükseltme zorlaması gerçekleştirmeyecektir** . Ancak, [MySQL Için Azure veritabanı sürüm oluşturma ilkesinde](concepts-version-policy.md) belgelenen kısıtlamalar uygulanır.

## <a name="next-steps"></a>Sonraki adımlar

[MySQL Için Azure veritabanı sürüm oluşturma ilkesi](concepts-version-policy.md)hakkında bilgi edinin.
