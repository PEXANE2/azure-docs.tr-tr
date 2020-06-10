---
title: Okuma çoğaltmalarını yönetme-Azure portal-PostgreSQL için Azure veritabanı-tek sunucu
description: Bkz. Azure Database for PostgreSQL için Azure veritabanı-tek sunucu Azure portal.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 06/09/2020
ms.openlocfilehash: c7d55a7b10f0c874fd84f32db1dcf21fb60c231f
ms.sourcegitcommit: ce44069e729fce0cf67c8f3c0c932342c350d890
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84636671"
---
# <a name="create-and-manage-read-replicas-in-azure-database-for-postgresql---single-server-from-the-azure-portal"></a>PostgreSQL için Azure veritabanı 'nda okuma çoğaltmaları oluşturma ve yönetme-tek sunucu Azure portal

Bu makalede, Azure portal 'tan PostgreSQL için Azure veritabanı 'nda okuma çoğaltmaları oluşturmayı ve yönetmeyi öğreneceksiniz. Okuma çoğaltmaları hakkında daha fazla bilgi edinmek için bkz. [genel bakış](concepts-read-replicas.md).


## <a name="prerequisites"></a>Önkoşullar
[PostgreSQL Için Azure veritabanı sunucusunun](quickstart-create-server-database-portal.md) ana sunucu olması.

## <a name="azure-replication-support"></a>Azure çoğaltma desteği

[Okuma çoğaltmaları](concepts-read-replicas.md) ve [mantıksal kod çözme](concepts-logical.md) , bilgi Için doğrudan Postgres yazma günlüğüne (Wal) bağlıdır. Bu iki özellik, Postgres 'den farklı günlük düzeylerine sahip olmalıdır. Mantıksal kod çözme, okuma Çoğaltmalarından daha yüksek bir günlüğe kaydetme düzeyine sahip olmalıdır.

Doğru günlük kaydını yapılandırmak için Azure çoğaltma desteği parametresini kullanın. Azure çoğaltma desteğinin üç ayar seçeneği vardır:

* **Kapalı** -en az bilgiyi Wal 'e yerleştirir. Bu ayar, çoğu PostgreSQL için Azure veritabanı sunucuları üzerinde kullanılamaz.  
* **Çoğaltma** -daha ayrıntılı bir **şekilde.** Bu, [okuma çoğaltmalarının](concepts-read-replicas.md) çalışması için gereken en düşük günlüğe kaydetme düzeyidir. Bu ayar, çoğu sunucuda varsayılandır.
* **Çoğaltmadan**daha ayrıntılı **mantıksal** . Bu, mantıksal kod çözmenin çalışması için en düşük günlük kayıt düzeyidir. Okuma çoğaltmaları bu ayarda de çalışır.

Bu parametrenin bir değişikliğinden sonra sunucunun yeniden başlatılması gerekiyor. Dahili olarak, bu parametre Postgres parametrelerini, `wal_level` `max_replication_slots` ve ' ı ayarlar `max_wal_senders` .

## <a name="prepare-the-master-server"></a>Ana sunucuyu hazırlama

1. Azure portal, ana sunucu olarak kullanılacak PostgreSQL için mevcut bir Azure veritabanı sunucusu seçin.

2. Sunucu menüsünden **çoğaltma**' yı seçin. Azure çoğaltma desteği en az **çoğaltmaya**ayarlanmışsa, okuma çoğaltmaları oluşturabilirsiniz. 

3. Azure çoğaltma desteği en az **çoğaltma**olarak ayarlanmamışsa, ayarlayın. **Kaydet**'i seçin.

   ![PostgreSQL için Azure veritabanı-çoğaltma-çoğaltma ayarla ve Kaydet](./media/howto-read-replicas-portal/set-replica-save.png)

4. **Evet**' i seçerek değişikliği uygulamak için sunucuyu yeniden başlatın.

   ![PostgreSQL için Azure veritabanı-çoğaltma-yeniden başlatmayı Onayla](./media/howto-read-replicas-portal/confirm-restart.png)

5. İşlem tamamlandıktan sonra iki Azure portal bildirimi alacaksınız. Sunucu parametresini güncelleştirmek için bir bildirim vardır. Sunucu yeniden başlatması için hemen sonraki bir bildirim daha vardır.

   ![Başarı bildirimleri](./media/howto-read-replicas-portal/success-notifications.png)

6. Çoğaltma araç çubuğunu güncelleştirmek için Azure portal sayfasını yenileyin. Artık bu sunucu için okuma çoğaltmaları oluşturabilirsiniz.
   

## <a name="create-a-read-replica"></a>Okuma çoğaltması oluşturma
Bir okuma çoğaltması oluşturmak için aşağıdaki adımları izleyin:

1. Ana sunucu olarak kullanılacak PostgreSQL için mevcut bir Azure veritabanı sunucusu seçin. 

2. Sunucu kenar çubuğunda, **Ayarlar**' ın altında, **çoğaltma**' yı seçin.

3. **Çoğaltma ekle**' yi seçin.

   ![Çoğaltma ekleme](./media/howto-read-replicas-portal/add-replica.png)

4. Okuma çoğaltması için bir ad girin. 

    ![Çoğaltmayı adlandırın](./media/howto-read-replicas-portal/name-replica.png)

5. Çoğaltma için bir konum seçin. Varsayılan konum, ana sunucu ile aynıdır.

    ![Bir konum seçin](./media/howto-read-replicas-portal/location-replica.png)

   > [!NOTE]
   > İçinde bir çoğaltma oluşturabileceğiniz bölgeler hakkında daha fazla bilgi edinmek için [çoğaltma kavramlarını oku makalesini](concepts-read-replicas.md)ziyaret edin. 

6. Çoğaltmanın oluşturulmasını onaylamak için **Tamam ' ı** seçin.

Bir çoğaltma, ana öğe ile aynı işlem ve depolama ayarları kullanılarak oluşturulur. Bir çoğaltma oluşturulduktan sonra, birden fazla ayar ana sunucudan bağımsız olarak değiştirilebilir: işlem oluşturma, sanal çekirdek, depolama ve yedekleme saklama süresi. Fiyatlandırma Katmanı, temel katmandan veya dışında bağımsız olarak da değiştirilebilir.

> [!IMPORTANT]
> Ana sunucu ayarı yeni bir değere güncellenmesinden önce, çoğaltma ayarını eşit veya daha büyük bir değere güncelleştirin. Bu eylem, çoğaltmanın ana üzerinde yapılan değişikliklerle devam etmesine yardımcı olur.

Okuma çoğaltması oluşturulduktan sonra **çoğaltma** penceresinden görüntülenebilir:

![Çoğaltma penceresinde yeni çoğaltmayı görüntüleme](./media/howto-read-replicas-portal/list-replica.png)
 

## <a name="stop-replication"></a>Çoğaltmayı durdurma
Bir ana sunucu ve bir okuma çoğaltması arasındaki çoğaltmayı durdurabilirsiniz.

> [!IMPORTANT]
> Bir ana sunucu ve bir okuma çoğaltması için çoğaltmayı durdurduktan sonra geri alınamaz. Okuma çoğaltması, hem okuma hem de yazma işlemlerini destekleyen tek başına bir sunucu haline gelir. Tek başına sunucu tekrar bir çoğaltmaya yapılamaz.

Azure portal bir ana sunucu ile okuma çoğaltması arasında çoğaltmayı durdurmak için şu adımları izleyin:

1. Azure portal, PostgreSQL için ana Azure veritabanı sunucunuzu seçin.

2. Sunucu menüsünde, **Ayarlar**' ın altında, **çoğaltma**' yı seçin.

3. Çoğaltmanın durdurulacağı çoğaltma sunucusunu seçin.

   ![Çoğaltmayı seçin](./media/howto-read-replicas-portal/select-replica.png)
 
4. **Çoğaltmayı durdur**' u seçin.

   ![Çoğaltmayı Durdur ' u seçin](./media/howto-read-replicas-portal/select-stop-replication.png)
 
5. Çoğaltmayı durdurmak için **Tamam ' ı** seçin.

   ![Çoğaltmayı durdurmayı Onayla](./media/howto-read-replicas-portal/confirm-stop-replication.png)
 

## <a name="delete-a-master-server"></a>Ana sunucuyu silme
Ana Sunucuyu silmek için, bir PostgreSQL sunucusu için tek başına Azure veritabanını silmek üzere aynı adımları kullanın. 

> [!IMPORTANT]
> Bir ana sunucuyu sildiğinizde, tüm okuma çoğaltmalarına çoğaltma durdurulur. Okuma çoğaltmaları artık hem okuma hem de yazma işlemlerini destekleyen tek başına sunucular haline gelir.

Azure portal bir sunucuyu silmek için şu adımları izleyin:

1. Azure portal, PostgreSQL için ana Azure veritabanı sunucunuzu seçin.

2. Sunucunun **genel bakış** sayfasını açın. **Sil**’i seçin.

   ![Sunucu genel bakış sayfasında ana sunucuyu silmek için seçin](./media/howto-read-replicas-portal/delete-server.png)
 
3. Silinecek ana sunucunun adını girin. Ana sunucuyu silmeyi onaylamak için **Sil** ' i seçin.

   ![Ana sunucuyu Silmeyi Onayla](./media/howto-read-replicas-portal/confirm-delete.png)
 

## <a name="delete-a-replica"></a>Bir çoğaltmayı sil
Bir ana sunucuyu silme işlemine benzer bir okuma çoğaltmasını silebilirsiniz.

- Azure portal, okuma çoğaltması için **genel bakış** sayfasını açın. **Sil**’i seçin.

   ![Çoğaltmaya genel bakış sayfasında, çoğaltmayı silmek için seçin](./media/howto-read-replicas-portal/delete-replica.png)
 
Ayrıca, aşağıdaki adımları izleyerek **çoğaltma** penceresinde okuma çoğaltmasını silebilirsiniz:

1. Azure portal, PostgreSQL için ana Azure veritabanı sunucunuzu seçin.

2. Sunucu menüsünde, **Ayarlar**' ın altında, **çoğaltma**' yı seçin.

3. Silinecek okuma çoğaltmasını seçin.

   ![Silinecek çoğaltmayı seçin](./media/howto-read-replicas-portal/select-replica.png)
 
4. **Çoğaltmayı Sil**' i seçin.

   ![Çoğaltmayı Sil ' i seçin](./media/howto-read-replicas-portal/select-delete-replica.png)
 
5. Silinecek çoğaltmanın adını girin. Çoğaltmanın silinmesini onaylamak için **Sil** ' i seçin.

   ![Te çoğaltmasını Silmeyi Onayla](./media/howto-read-replicas-portal/confirm-delete-replica.png)
 

## <a name="monitor-a-replica"></a>Bir çoğaltmayı izleme
Okuma çoğaltmalarını izlemek için iki ölçüm mevcuttur.

### <a name="max-lag-across-replicas-metric"></a>Çoğaltmalar arasındaki en fazla gecikme ölçümü
**Çoğaltmalar genelinde en fazla gecikme** ölçümü, ana sunucu ile en fazla çoğaltma çoğaltması arasındaki gecikme sayısını gösterir. 

1.  Azure portal, PostgreSQL için ana Azure veritabanı sunucusu ' nu seçin.

2.  **Ölçümler**’i seçin. **Ölçümler** penceresinde **çoğaltmalar genelinde en fazla gecikme**' yı seçin.

    ![Çoğaltmalar genelinde en fazla gecikme 'yi izleme](./media/howto-read-replicas-portal/select-max-lag.png)
 
3.  Toplamadır **,** **en fazla**' yı seçin.


### <a name="replica-lag-metric"></a>Çoğaltma gecikmesi ölçümü
**Çoğaltma gecikmesi** ölçümü, bir çoğaltmada son yeniden yürütülen işlemden bu yana geçen süreyi gösterir. Ana kuruluşunuzda gerçekleşen işlem yoksa, ölçüm bu gecikme süresini yansıtır.

1. Azure portal, PostgreSQL için Azure veritabanı okuma çoğaltması ' nı seçin.

2. **Ölçümler**’i seçin. **Ölçümler** penceresinde **çoğaltma gecikmesi**' nı seçin.

   ![Çoğaltma gecikmesini izleme](./media/howto-read-replicas-portal/select-replica-lag.png)
 
3. Toplamadır **,** **en fazla**' yı seçin. 
 
## <a name="next-steps"></a>Sonraki adımlar
* [PostgreSQL Için Azure veritabanı 'nda okuma çoğaltmaları](concepts-read-replicas.md)hakkında daha fazla bilgi edinin.
* [Azure CLI ve REST API okuma çoğaltmaları oluşturma ve yönetme](howto-read-replicas-cli.md)hakkında bilgi edinin.