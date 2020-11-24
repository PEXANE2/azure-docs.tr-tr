---
title: Ana sürüm yükseltme-Azure portal-MySQL için Azure veritabanı-tek sunucu
description: Bu makalede, Azure portal kullanarak MySQL için Azure veritabanı 'nın ana sürümünü nasıl yükselteceğiniz açıklanmaktadır
author: ambhatna
ms.author: ambhatna
ms.service: mysql
ms.topic: how-to
ms.date: 11/16/2020
ms.openlocfilehash: 080d7c09b180d5943216793119718eb6a2add79e
ms.sourcegitcommit: 1bf144dc5d7c496c4abeb95fc2f473cfa0bbed43
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2020
ms.locfileid: "95753060"
---
# <a name="major-version-upgrade-in-azure-database-for-mysql-single-server-using-the-azure-portal"></a>Azure portal kullanarak MySQL için Azure veritabanı 'nda ana sürüm yükseltmesi

> [!IMPORTANT]
> MySQL için Azure veritabanı tek sunucu için ana sürüm yükseltmesi genel önizlemede.

Bu makalede MySQL ana sürümünüzü, MySQL için Azure veritabanı tek sunucu 'da nasıl yükseltebileceğinizi açıklamaktadır.

Bu özellik, müşterilerin MySQL 5,6 sunucularının herhangi bir veri hareketi olmadan veya herhangi bir uygulama bağlantı dizesi değişikliğine gerek kalmadan bir tıklama düğmesine sahip MySQL 5,7 ' e yerinde yükseltmelerini gerçekleştirmesini sağlayacaktır.

> [!Note]
> * Ana sürüm yükseltmesi yalnızca MySQL 5,6 ' den MySQL 5,7 ' ye ana sürüm yükseltmesi için kullanılabilir<br>
> * Ana sürüm yükseltmesi henüz Çoğaltma sunucusunda desteklenmiyor.
> * Sunucu, yükseltme işlemi boyunca kullanılamaz olacaktır. Bu nedenle, planlı bakım pencereniz sırasında yükseltmeleri gerçekleştirmeniz önerilir.

## <a name="prerequisites"></a>Önkoşullar
Bu nasıl yapılır kılavuzunu tamamlayabilmeniz için şunlar gerekir:
- [MySQL Için Azure veritabanı tek sunucu](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="perform-major-version-upgrade-from-mysql-56-to-mysql-57"></a>MySQL 5,6 ' den MySQL 5,7 ' e büyük sürüm yükseltmesi gerçekleştirin

MySQL 5,6 Server Azure veritabanınız için ana sürüm yükseltmesini gerçekleştirmek üzere bu adımları izleyin

> [!IMPORTANT]
> Üretimi doğrudan yükseltmek yerine, ilk olarak sunucunun geri yüklenen kopyasında yükseltmeyi gerçekleştirmenizi öneririz. Bkz. [Noktadan noktaya geri yükleme gerçekleştirme](howto-restore-server-portal.md#point-in-time-restore). 

1. [Azure Portal](https://portal.azure.com/), var olan MySQL 5,6 Server Için Azure veritabanınızı seçin.

2. **Genel bakış** sayfasında, araç çubuğundaki **Yükselt** düğmesine tıklayın.

3. **Yükseltme** bölümünde MySQL 5,6 Server için Azure veritabanı 'nı 5,7 sunucusuna yükseltmek üzere **Tamam** ' ı seçin.

    :::image type="content" source="./media/how-to-major-version-upgrade-portal/upgrade.png" alt-text="MySQL için Azure veritabanı-genel bakış-yükseltme":::

4. Bir bildirim, yükseltmenin başarılı olduğunu doğrulayacaktır.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

**1. bu yükseltme özelliği, üretim ortamımızda yükseltilebilmemiz gereken MySQL v 5.6 ile ne zaman GA olacak?**

Bu özelliğin GA 'i MySQL v 5.6 kullanımdan kaldırmadan önce planlanmaktadır. Ancak, bu özellik üretim için hazırlayın ve Azure tarafından tam olarak desteklenerek ortamınızda güvenle çalıştırmalısınız. Önerilen en iyi yöntem olarak, yükseltme sırasında kapalı kalma süresini tahmin etmek ve üretim ortamında çalıştırmadan önce uygulama uyumluluk testi gerçekleştirmek için önce bunu bir sunucunun geri yüklenmiş kopyasında çalıştırıp test etmenize kesinlikle tavsiye ederiz. Daha fazla bilgi için, sunucunuzun zaman içindeki kopyasını oluşturmak üzere [Noktadan noktaya geri yükleme gerçekleştirme](howto-restore-server-portal.md#point-in-time-restore) konusuna bakın. 

**2. Bu, sunucu kapalı kalma süresine yol açacaktır, ne kadar süreyle?**

Evet, yükseltme işlemi sırasında sunucu kullanılamıyor olacaktır, bu sayede planlı bakım pencerenizde bu işlemi gerçekleştirmenizi öneririz. Tahmini kapalı kalma süresi, veritabanı boyutuna, sağlanan depolama boyutuna (IOPS 'nin sağlandığı) ve veritabanındaki tablo sayısına bağlıdır. Yükseltme süresi, sunucudaki tablo sayısıyla doğrudan orantılıdır. Temel SKU sunucularının yükseltmeleri, standart depolama platformunda olduğu için daha uzun sürme beklenmektedir. Sunucu ortamınız için kapalı kalma süresini tahmin etmek için, önce sunucunun geri yüklenen kopyasında yükseltmeyi gerçekleştirmenizi öneririz.  

**3. Çoğaltma sunucusunda henüz desteklenmediğini not edin. Bu durum somut anlamı nedir?**

Şu anda, birincil sürüm yükseltmesi çoğaltma sunucusu için desteklenmez. Bu, Çoğaltma sunucusunda (kaynak veya çoğaltma sunucusu) yer alan sunucular için çalıştırılmamalısınız. Yükseltme özelliği için çoğaltma desteğini eklemeden önce çoğaltmaya dahil olan sunucuların yükseltmesini test etmek isterseniz, aşağıdaki adımları öneriyoruz:

1. Planlı bakım sırasında, ad ve tüm yapılandırma bilgileri (kaynak sunucudan farklıysa sunucu parametre yapılandırması) yakalandıktan sonra [çoğaltmayı durdurun ve çoğaltma sunucusunu silin](howto-read-replicas-portal.md) .
2. Kaynak sunucunun yükseltmesini gerçekleştirin.
3. Adım 1 ' de yakalanan aynı ad ve yapılandırma ayarlarına sahip yeni bir okuma çoğaltması sunucusu sağlayın. Kaynak sunucu v 5.7 sürümüne yükseltildikten sonra yeni çoğaltma sunucusu v 5.7 'de otomatik olarak olacaktır.

**4. MySQL v 5.6 sunucusunu 5 Şubat 2021 tarihinden önce yükseltmeyi seçemiz, ne olur?**

Daha önce olduğu gibi MySQL v 5.6 sunucusunu çalıştırmaya devam edebilirsiniz. Azure **, sunucunuzda hiçbir yükseltme zorlaması gerçekleştirmeyecektir** . Ancak, [MySQL Için Azure veritabanı sürüm oluşturma ilkesinde](concepts-version-policy.md) belgelenen kısıtlamalar uygulanır.

## <a name="next-steps"></a>Sonraki adımlar

[MySQL Için Azure veritabanı sürüm oluşturma ilkesi](concepts-version-policy.md)hakkında bilgi edinin.
