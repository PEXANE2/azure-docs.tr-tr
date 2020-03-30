---
title: Yedekleme ve geri yükleme - Azure portalı - MariaDB için Azure Veritabanı
description: Bu makalede, Azure portalını kullanarak MariaDB için Azure Veritabanı'ndaki bir sunucunun nasıl geri yüklenir.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/27/2020
ms.openlocfilehash: fa8ead8daa202f5747c134a62fbd43bcdf2af0d7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80369253"
---
# <a name="how-to-backup-and-restore-a-server-in-azure-database-for-mariadb-using-the-azure-portal"></a>Azure portalını kullanarak MariaDB için Azure Veritabanı'ndaki bir sunucuyu yedekleme ve geri yükleme

## <a name="backup-happens-automatically"></a>Yedekleme otomatik olarak gerçekleşir
MariaDB sunucuları için Azure Veritabanı, Geri Yükleme özelliklerini etkinleştirmek için düzenli aralıklarla yedeklenir. Bu özelliği kullanarak, sunucuyu ve tüm veritabanlarını yeni bir sunucuda daha önceki bir zaman diliminde geri yükleyebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar
Bu nasıl yapılacağını kılavuzunu tamamlamak için şunları yapmanız gerekir:
- [MariaDB sunucusu ve veritabanı için](quickstart-create-mariadb-server-database-using-azure-portal.md) bir Azure Veritabanı

## <a name="set-backup-configuration"></a>Yedekleme yapılandırması ayarlama

**Sunucunuzu** yerel olarak yedekli yedeklemeler veya coğrafi olarak yedek li yedeklemeler için fiyatlandırma katmanı penceresinde sunucu oluşturma da yapılandırmak arasında seçim yaparsınız.

> [!NOTE]
> Bir sunucu oluşturulduktan sonra, sahip olduğu artıklık türü, coğrafi olarak gereksiz vs yerel gereksiz, değiştirilemez.
>

Azure portalı üzerinden bir sunucu oluştururken, **Fiyatlandırma Katmanı** penceresi sunucunuz için Yerel **Olarak Yedekli** veya Coğrafi Olarak **Yedekli** yedeklemeleri seçtiğiniz yerdir. Bu pencere aynı zamanda **Yedekleme Bekletme Dönemi'ni** seçtiğiniz yerdir - sunucu yedeklemelerinin ne kadar süreyle (gün sayısı olarak) depolanmasını istediğiniz.

   ![Fiyatlandırma Katmanı - Yedek Artıklığı Seçin](./media/howto-restore-server-portal/pricing-tier.png)

Oluşturma sırasında bu değerleri ayarlama hakkında daha fazla bilgi için [MariaDB sunucusu için Azure Veritabanı'na bakın.](quickstart-create-mariadb-server-database-using-azure-portal.md)

Yedekleme bekletme süresi aşağıdaki adımlarla bir sunucuda değiştirilebilir:
1. [Azure portalında](https://portal.azure.com/)oturum açın.

2. MariaDB sunucusu için Azure Veritabanınızı seçin. Bu eylem **Genel Bakış** sayfasını açar.

3. **AYARLAR**altında menüden **Fiyatlandırma Katmanı'nı** seçin. Kaydırıcıyı kullanarak Yedekleme **Bekletme Süresini** 7 ile 35 gün arasında tercihinize göre değiştirebilirsiniz.
Aşağıdaki ekran görüntüsünde 35 güne çıkarılmıştır.
![Yedekleme bekletme süresi artırıldı](./media/howto-restore-server-portal/3-increase-backup-days.png)

4. Değişikliği onaylamak için **Tamam'ı** tıklatın.

Yedekleme bekletme süresi, kullanılabilir yedeklemelere dayandığı için zaman içinde bir geri yüklemenin ne kadar geri alınabileceğini yönetir. Zaman içinde geri yükleme aşağıdaki bölümde daha ayrıntılı olarak açıklanmıştır. 

## <a name="point-in-time-restore"></a>Belirli bir noktaya geri yükleme
MariaDB için Azure Veritabanı, sunucuyu bir noktada niçin geri yüklemenize ve sunucunun yeni bir kopyasına geri yüklemenize olanak tanır. Verilerinizi kurtarmak için bu yeni sunucuyu kullanabilir veya istemci uygulamalarınızın bu yeni sunucuyu işaret etmesini sağlayabilirsiniz.

Örneğin, bugün öğlen yanlışlıkla bir tablo düştüyse, öğlenden hemen önce zamana geri yükleyebilir ve eksik tabloyu ve verileri sunucunun yeni kopyasından alabilirsiniz. Zaman içinde geri yükleme, veritabanı düzeyinde değil, sunucu düzeyindedir.

Aşağıdaki adımlar örnek sunucuyu zamanında bir noktaya geri yüklenir:
1. Azure portalında, MariaDB sunucusu için Azure Veritabanınızı seçin. 

2. Sunucunun **Genel Bakış** sayfasının araç çubuğunda **Geri Yükle'yi**seçin.

   ![MariaDB için Azure Veritabanı - Genel Bakış - Geri Yükleme düğmesi](./media/howto-restore-server-portal/2-server.png)

3. Geri Yükleme formunu gerekli bilgilerle doldurun:

   ![MariaDB için Azure Veritabanı - Bilgileri geri yükleme](./media/howto-restore-server-portal/3-restore.png)
   - **Geri Yükleme noktası**: Geri yüklemek istediğiniz zamanı seçin.
   - **Hedef sunucu**: Yeni sunucu için bir ad sağlayın.
   - **Konum**: Bölgeyi seçemezsiniz. Varsayılan olarak kaynak sunucu ile aynıdır.
   - **Fiyatlandırma katmanı**: Bu parametreleri zamanında geri yükleme yaparken değiştiremezsiniz. Kaynak sunucuyla aynıdır. 

4. Sunucuyu geri yüklemek için **tamam'ı** tıklatın. 

5. Geri yükleme tamamlandığında, verilerin beklendiği gibi geri yüklenmiş olduğunu doğrulamak için oluşturulan yeni sunucuyu bulun.


Zaman içinde geri yükleme tarafından oluşturulan yeni sunucu, zaman içinde seçilen noktada varolan sunucu için geçerli olan sunucu yönetici giriş adı ve parolaya sahiptir. Parolayı yeni sunucunun **Genel Bakış** sayfasından değiştirebilirsiniz.

Geri yükleme sırasında oluşturulan yeni sunucuda özgün sunucuda var olan VNet hizmet bitiş noktaları yoktur. Bu yeni sunucu için bu kuralların ayrı olarak ayarlanılması gerekir. Orijinal sunucudan gelen güvenlik duvarı kuralları geri yüklenir.

## <a name="geo-restore"></a>Coğrafi geri yükleme

Sunucunuzu coğrafi olarak gereksiz yedeklemeler için yapılandıysanız, varolan sunucunun yedeklemesinden yeni bir sunucu oluşturulabilir. Bu yeni sunucu, MariaDB için Azure Veritabanı'nın kullanılabilen herhangi bir bölgesinde oluşturulabilir.  

1. MariaDB için **Veritabanları** > **Azure Veritabanı'nı**seçin. Ayrıca hizmeti bulmak için arama kutusuna **MariaDB** yazabilirsiniz.

   !["MariaDB için Azure Veritabanı" seçeneği](./media/howto-restore-server-portal/2_navigate-to-mariadb.png)

2. Formun **Kaynak** seç açılır düşüşünde **Yedekleme'yi**seçin. Bu eylem, coğrafi yedekli yedeklemeleri etkin leştirilmiş sunucuların listesini yükler. Yeni sunucunuzun kaynağı olması için bu yedeklemelerden birini seçin.
   ![Kaynak'ı seçin: Yedekleme ve coğrafi yedek li yedeklerin listesi](./media/howto-restore-server-portal/2-georestore.png)

   > [!NOTE]
   > Bir sunucu ilk oluşturulduğunda, coğrafi geri yükleme için hemen kullanılamayabilir. Gerekli meta verilerin doldurulması birkaç saat sürebilir.
   >

3. Formun geri kalanını tercihlerinizle doldurun. Herhangi bir **Konumu**seçebilirsiniz. Konumu seçtikten sonra Fiyatlandırma **Katmanı'nı**seçebilirsiniz. Varsayılan olarak, geri aldığınız varolan sunucunun parametreleri görüntülenir. Bu ayarları devralmak için herhangi bir değişiklik yapmadan **Tamam'ı** tıklatabilirsiniz. Veya İşlem **Oluşturma** (seçtiğiniz bölgede varsa), **vCore**sayısı, **Yedekleme Bekletme Süresi**ve **Yedek Artıklık Seçeneğini**değiştirebilirsiniz. Geri yükleme sırasında **Fiyatlandırma Katmanını** (Temel, Genel Amaç veya Bellek Optimize Edilen) veya **Depolama** boyutunu değiştirme desteklenmez.

Geo geri yükleme tarafından oluşturulan yeni sunucu, geri yükleme nin başlatıldığı anda varolan sunucu için geçerli olan sunucu yöneticisi giriş adı ve parolaya sahiptir. Parola, yeni sunucunun **Genel Bakış** sayfasından değiştirilebilir.

Geri yükleme sırasında oluşturulan yeni sunucuda özgün sunucuda var olan VNet hizmet bitiş noktaları yoktur. Bu yeni sunucu için bu kuralların ayrı olarak ayarlanılması gerekir. Orijinal sunucudan gelen güvenlik duvarı kuralları geri yüklenir.

## <a name="next-steps"></a>Sonraki adımlar
- Hizmetin [yedekleri](concepts-backup.md) hakkında daha fazla bilgi edinin
- [Yinelemeler](concepts-read-replicas.md) hakkında bilgi edinin
- [İş sürekliliği](concepts-business-continuity.md) seçenekleri hakkında daha fazla bilgi edinin
