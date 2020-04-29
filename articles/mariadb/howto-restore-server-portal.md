---
title: Yedekleme ve geri yükleme-Azure portal-MariaDB için Azure veritabanı
description: Bu makalede, Azure portal kullanarak MariaDB için Azure veritabanı 'nda bir sunucunun nasıl geri yükleneceği açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/27/2020
ms.openlocfilehash: fa8ead8daa202f5747c134a62fbd43bcdf2af0d7
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80369253"
---
# <a name="how-to-backup-and-restore-a-server-in-azure-database-for-mariadb-using-the-azure-portal"></a>Azure portal kullanarak MariaDB için Azure veritabanı 'nda sunucu yedekleme ve geri yükleme

## <a name="backup-happens-automatically"></a>Yedekleme otomatik olarak gerçekleşir
MariaDB sunucuları için Azure veritabanı, geri yükleme özelliklerini etkinleştirmek üzere düzenli aralıklarla yedeklenir. Bu özelliği kullanarak, sunucuyu ve tüm veritabanlarını yeni bir sunucuda daha önceki bir zaman noktasına geri yükleyebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar
Bu nasıl yapılır kılavuzunu tamamlayabilmeniz için şunlar gerekir:
- [MariaDB sunucusu ve veritabanı Için Azure veritabanı](quickstart-create-mariadb-server-database-using-azure-portal.md)

## <a name="set-backup-configuration"></a>Yedekleme yapılandırmasını ayarla

Sunucu oluşturma sırasında, **fiyatlandırma katmanı** penceresinde yerel olarak yedekli yedeklemeler veya coğrafi olarak yedekli yedeklemeler için sunucunuzu yapılandırma arasında seçim yaparsınız.

> [!NOTE]
> Bir sunucu oluşturulduktan sonra, coğrafi olarak yedekli ve yerel olarak yedekli olan artıklık türü değiştirilemez.
>

Azure portal aracılığıyla bir sunucu oluştururken, **fiyatlandırma katmanı** penceresi sunucunuz Için **yerel olarak yedekli** veya **coğrafi olarak yedekli** yedeklemeleri seçtiğiniz yerdir. Bu pencere Ayrıca, **yedekleme bekletme dönemini** (gün sayısı cinsinden) sunucu yedeklemelerinin ne kadar süreyle depolandığını seçtiğiniz yerdir.

   ![Fiyatlandırma Katmanı-yedek yedeklilik seçin](./media/howto-restore-server-portal/pricing-tier.png)

Oluşturma sırasında bu değerleri ayarlama hakkında daha fazla bilgi için, bkz. [MariaDB sunucu hızlı başlangıç Için Azure veritabanı](quickstart-create-mariadb-server-database-using-azure-portal.md).

Yedekleme saklama süresi bir sunucuda aşağıdaki adımlarla değiştirilebilir:
1. [Azure Portal](https://portal.azure.com/)oturum açın.

2. MariaDB sunucusu için Azure veritabanınızı seçin. Bu eylem **genel bakış** sayfasını açar.

3. Menüde, **Ayarlar**' ın altında **fiyatlandırma katmanı** ' nı seçin. Kaydırıcıyı kullanarak **yedekleme saklama süresini** 7 ila 35 gün süreyle tercihlerinize göre değiştirebilirsiniz.
Aşağıdaki ekran görüntüsünde 35 güne yükselmiştir.
![Yedekleme bekletme süresi arttı](./media/howto-restore-server-portal/3-increase-backup-days.png)

4. Değişikliği onaylamak için **Tamam** ' ı tıklatın.

Yedekleme bekletme süresi, kullanılabilir yedeklemeler temel aldığı için zaman içinde bir nokta geri yüklemesi alma süresinin ne kadar geri alınacağını yönetir. Bir noktadan sonra geri yükleme aşağıdaki bölümde daha ayrıntılı olarak açıklanmıştır. 

## <a name="point-in-time-restore"></a>Belirli bir noktaya geri yükleme
MariaDB için Azure veritabanı, sunucuyu bir zaman noktasına geri yüklemenize ve sunucunun yeni bir kopyasına geri yüklemenize olanak tanır. Verilerinizi kurtarmak için bu yeni sunucuyu kullanabilir veya istemci uygulamalarınızın bu yeni sunucuyu işaret edebilir.

Örneğin, bir tablo bugün öğleden sonra yanlışlıkla bırakıldıysa, daha önce geçen saate geri yükleyebilir ve eksik tablo ve verileri sunucunun bu yeni kopyasından elde edebilirsiniz. Zaman içinde geri yükleme, veritabanı düzeyinde değil, sunucu düzeyindedir.

Aşağıdaki adımlar örnek sunucuyu bir zaman noktasına geri yükler:
1. Azure portal, MariaDB sunucusu için Azure veritabanınızı seçin. 

2. Sunucunun **genel bakış** sayfasının araç çubuğunda **geri yükle**' yi seçin.

   ![MariaDB için Azure veritabanı-genel bakış-geri yükleme düğmesi](./media/howto-restore-server-portal/2-server.png)

3. Geri yükleme formunu gereken bilgilerle doldurun:

   ![MariaDB için Azure veritabanı-bilgileri geri yükleme](./media/howto-restore-server-portal/3-restore.png)
   - **Geri yükleme noktası**: geri yüklemek istediğiniz zaman noktasını seçin.
   - **Hedef sunucu**: yeni sunucu için bir ad sağlayın.
   - **Konum**: bölgeyi seçemezsiniz. Varsayılan olarak, kaynak sunucu ile aynıdır.
   - **Fiyatlandırma katmanı**: bir zaman içinde geri yükleme yaparken bu parametreleri değiştiremezsiniz. Kaynak sunucuyla aynıdır. 

4. Sunucuyu bir zaman noktasına geri yüklemek üzere geri yüklemek için **Tamam** ' a tıklayın. 

5. Geri yükleme tamamlandıktan sonra, verilerin beklendiği gibi geri yüklendiğini doğrulamak için oluşturulan yeni sunucuyu bulun.


Noktadan noktaya geri yükleme tarafından oluşturulan yeni sunucu, mevcut sunucu için geçerli olan Sunucu Yöneticisi oturum açma adı ve parolası ile aynı zamanda, Parolayı yeni sunucunun **genel bakış** sayfasından değiştirebilirsiniz.

Geri yükleme sırasında oluşturulan yeni sunucu, özgün sunucuda var olan VNet hizmeti uç noktalarına sahip değildir. Bu kuralların bu yeni sunucu için ayrı olarak ayarlanması gerekir. Özgün sunucudan gelen güvenlik duvarı kuralları geri yüklendi.

## <a name="geo-restore"></a>Coğrafi geri yükleme

Sunucunuzu coğrafi olarak yedekli yedeklemeler için yapılandırdıysanız, var olan sunucunun yedeklemesinden yeni bir sunucu oluşturulabilir. Bu yeni sunucu, MariaDB için Azure veritabanı 'nın kullanılabildiği herhangi bir bölgede oluşturulabilir.  

1. **Veritabanları** > ' nı**MariaDB için Azure veritabanı**' nı seçin. Hizmeti bulmak için arama kutusuna **MariaDB** de yazabilirsiniz.

   !["MariaDB için Azure veritabanı" seçeneği](./media/howto-restore-server-portal/2_navigate-to-mariadb.png)

2. Formun **Kaynak Seç** açılan menüsünde **Yedekle**' yi seçin. Bu eylem, coğrafi olarak yedekli yedeklemelerin etkinleştirildiği sunucuların listesini yükler. Yeni sunucunuzun kaynağı olacak şekilde bu Yedeklerden birini seçin.
   ![Kaynak seçin: coğrafi olarak yedekli yedeklemelerin yedeklenmesi ve listesi](./media/howto-restore-server-portal/2-georestore.png)

   > [!NOTE]
   > Sunucu ilk oluşturulduğunda coğrafi geri yükleme için hemen kullanılamayabilir. Gerekli meta verilerin doldurulması birkaç saat sürebilir.
   >

3. Formun geri kalanını tercihlerinize göre doldurun. Herhangi bir **konum**seçebilirsiniz. Konumu seçtikten sonra **fiyatlandırma katmanını**seçebilirsiniz. Varsayılan olarak, geri yüklemekte olduğunuz mevcut sunucunun parametreleri görüntülenir. Bu ayarları devralması için herhangi bir değişiklik yapmadan **Tamam** ' a tıklayabilirsiniz. Ya da **Işlem üretimini** (seçtiğiniz bölgede varsa), **sanal çekirdek**sayısı, **yedekleme saklama süresi**ve **yedekleme artıklığı seçeneğinin**sayısını değiştirebilirsiniz. Geri yükleme sırasında **fiyatlandırma katmanını** (temel, genel amaçlı veya bellek için iyileştirilmiş) veya **depolama** boyutunu değiştirme desteklenmiyor.

Coğrafi geri yükleme tarafından oluşturulan yeni sunucu, geri yükleme başlatıldığı sırada mevcut sunucu için geçerli olan Sunucu Yöneticisi oturum açma adı ve parolaya sahiptir. Parola, yeni sunucunun **genel bakış** sayfasından değiştirilebilir.

Geri yükleme sırasında oluşturulan yeni sunucu, özgün sunucuda var olan VNet hizmeti uç noktalarına sahip değildir. Bu kuralların bu yeni sunucu için ayrı olarak ayarlanması gerekir. Özgün sunucudan gelen güvenlik duvarı kuralları geri yüklendi.

## <a name="next-steps"></a>Sonraki adımlar
- Hizmetin [yedeklemeleri](concepts-backup.md) hakkında daha fazla bilgi edinin
- [Çoğaltmalar](concepts-read-replicas.md) hakkında bilgi edinin
- [İş sürekliliği](concepts-business-continuity.md) seçenekleri hakkında daha fazla bilgi edinin
