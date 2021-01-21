---
title: MySQL için Azure veritabanı 'nda ana sürüm yükseltmesi-tek sunucu
description: Bu makalede, MySQL için Azure veritabanı-tek sunucu için ana sürümü nasıl yükselteceğiniz açıklanmaktadır
author: ambhatna
ms.author: ambhatna
ms.service: mysql
ms.topic: how-to
ms.date: 1/13/2021
ms.openlocfilehash: b62f4ebc61ac27478788d8b2bae5e4145f87ac8b
ms.sourcegitcommit: 484f510bbb093e9cfca694b56622b5860ca317f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/21/2021
ms.locfileid: "98630209"
---
# <a name="major-version-upgrade-in-azure-database-for-mysql-single-server"></a>MySQL için Azure veritabanı tek sunucu 'da ana sürüm yükseltmesi

> [!IMPORTANT]
> MySQL için Azure veritabanı tek sunucu için ana sürüm yükseltmesi genel önizlemede.

Bu makalede MySQL ana sürümünüzü, MySQL için Azure veritabanı tek sunucu 'da nasıl yükseltebileceğinizi açıklamaktadır.

Bu özellik, müşterilerin MySQL 5,6 sunucularının herhangi bir veri hareketi olmadan veya herhangi bir uygulama bağlantı dizesi değişikliğine gerek kalmadan bir tıklama düğmesine sahip MySQL 5,7 ' e yerinde yükseltmelerini gerçekleştirmesini sağlayacaktır.

> [!Note]
> * Ana sürüm yükseltmesi yalnızca MySQL 5,6 ' den MySQL 5,7 ' ye ana sürüm yükseltmesi için kullanılabilir<br>
> * Ana sürüm yükseltmesi henüz Çoğaltma sunucusunda desteklenmiyor.
> * Sunucu, yükseltme işlemi boyunca kullanılamaz olacaktır. Bu nedenle, planlı bakım pencereniz sırasında yükseltmeleri gerçekleştirmeniz önerilir.

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
