---
title: Okuma çoğaltmalarını yönetme-Azure portal-PostgreSQL için Azure veritabanı-tek sunucu
description: Bkz. Azure Database for PostgreSQL için Azure veritabanı-tek sunucu Azure portal.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: how-to
ms.date: 07/10/2020
ms.openlocfilehash: 623b9c1eccefe5d7e6027ddbed61c89720d98e9a
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90884486"
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

3. Azure çoğaltma desteği en az **çoğaltma**olarak ayarlanmamışsa, ayarlayın. **Kaydet**’i seçin.

   :::image type="content" source="./media/howto-read-replicas-portal/set-replica-save.png" alt-text="PostgreSQL için Azure veritabanı-çoğaltma-çoğaltma ayarla ve Kaydet":::

4. **Evet**' i seçerek değişikliği uygulamak için sunucuyu yeniden başlatın.

   :::image type="content" source="./media/howto-read-replicas-portal/confirm-restart.png" alt-text="PostgreSQL için Azure veritabanı-çoğaltma-yeniden başlatmayı Onayla":::

5. İşlem tamamlandıktan sonra iki Azure portal bildirimi alacaksınız. Sunucu parametresini güncelleştirmek için bir bildirim vardır. Sunucu yeniden başlatması için hemen sonraki bir bildirim daha vardır.

   :::image type="content" source="./media/howto-read-replicas-portal/success-notifications.png" alt-text="Başarı bildirimleri":::

6. Çoğaltma araç çubuğunu güncelleştirmek için Azure portal sayfasını yenileyin. Artık bu sunucu için okuma çoğaltmaları oluşturabilirsiniz.
   

## <a name="create-a-read-replica"></a>Okuma amaçlı çoğaltma oluşturma
Bir okuma çoğaltması oluşturmak için aşağıdaki adımları izleyin:

1. Ana sunucu olarak kullanılacak PostgreSQL için mevcut bir Azure veritabanı sunucusu seçin. 

2. Sunucu kenar çubuğunda, **Ayarlar**' ın altında, **çoğaltma**' yı seçin.

3. **Çoğaltma ekle**' yi seçin.

   :::image type="content" source="./media/howto-read-replicas-portal/add-replica.png" alt-text="Çoğaltma ekleme":::

4. Okuma çoğaltması için bir ad girin. 

    :::image type="content" source="./media/howto-read-replicas-portal/name-replica.png" alt-text="Çoğaltmayı adlandırın":::

5. Çoğaltma için bir konum seçin. Varsayılan konum, ana sunucu ile aynıdır.

    :::image type="content" source="./media/howto-read-replicas-portal/location-replica.png" alt-text="Bir konum seçin":::

   > [!NOTE]
   > İçinde bir çoğaltma oluşturabileceğiniz bölgeler hakkında daha fazla bilgi edinmek için [çoğaltma kavramlarını oku makalesini](concepts-read-replicas.md)ziyaret edin. 

6. Çoğaltmanın oluşturulmasını onaylamak için **Tamam ' ı** seçin.

Okuma çoğaltması oluşturulduktan sonra **çoğaltma** penceresinden görüntülenebilir:

:::image type="content" source="./media/howto-read-replicas-portal/list-replica.png" alt-text="Çoğaltma penceresinde yeni çoğaltmayı görüntüleme":::
 

> [!IMPORTANT]
> [Okuma çoğaltmasına genel bakış konusunun Konular bölümünü](concepts-read-replicas.md#considerations)gözden geçirin.
>
> Ana sunucu ayarı yeni bir değere güncellenmesinden önce, çoğaltma ayarını eşit veya daha büyük bir değere güncelleştirin. Bu eylem, çoğaltmanın ana üzerinde yapılan değişikliklerle devam etmesine yardımcı olur.

## <a name="stop-replication"></a>Çoğaltmayı durdurma
Bir ana sunucu ve bir okuma çoğaltması arasındaki çoğaltmayı durdurabilirsiniz.

> [!IMPORTANT]
> Bir ana sunucu ve bir okuma çoğaltması için çoğaltmayı durdurduktan sonra geri alınamaz. Okuma çoğaltması, hem okuma hem de yazma işlemlerini destekleyen tek başına bir sunucu haline gelir. Tek başına sunucu tekrar bir çoğaltmaya yapılamaz.

Azure portal bir ana sunucu ile okuma çoğaltması arasında çoğaltmayı durdurmak için şu adımları izleyin:

1. Azure portal, PostgreSQL için ana Azure veritabanı sunucunuzu seçin.

2. Sunucu menüsünde, **Ayarlar**' ın altında, **çoğaltma**' yı seçin.

3. Çoğaltmanın durdurulacağı çoğaltma sunucusunu seçin.

   :::image type="content" source="./media/howto-read-replicas-portal/select-replica.png" alt-text="Çoğaltmayı seçin":::
 
4. **Çoğaltmayı durdur**' u seçin.

   :::image type="content" source="./media/howto-read-replicas-portal/select-stop-replication.png" alt-text="Çoğaltmayı Durdur ' u seçin":::
 
5. Çoğaltmayı durdurmak için **Tamam ' ı** seçin.

   :::image type="content" source="./media/howto-read-replicas-portal/confirm-stop-replication.png" alt-text="Çoğaltmayı durdurmayı Onayla":::
 

## <a name="delete-a-master-server"></a>Ana sunucuyu silme
Ana Sunucuyu silmek için, bir PostgreSQL sunucusu için tek başına Azure veritabanını silmek üzere aynı adımları kullanın. 

> [!IMPORTANT]
> Bir ana sunucuyu sildiğinizde, tüm okuma çoğaltmalarına çoğaltma durdurulur. Okuma çoğaltmaları artık hem okuma hem de yazma işlemlerini destekleyen tek başına sunucular haline gelir.

Azure portal bir sunucuyu silmek için şu adımları izleyin:

1. Azure portal, PostgreSQL için ana Azure veritabanı sunucunuzu seçin.

2. Sunucunun **genel bakış** sayfasını açın. **Sil**’i seçin.

   :::image type="content" source="./media/howto-read-replicas-portal/delete-server.png" alt-text="Sunucu genel bakış sayfasında ana sunucuyu silmek için seçin":::
 
3. Silinecek ana sunucunun adını girin. Ana sunucuyu silmeyi onaylamak için **Sil** ' i seçin.

   :::image type="content" source="./media/howto-read-replicas-portal/confirm-delete.png" alt-text="Ana sunucuyu Silmeyi Onayla":::
 

## <a name="delete-a-replica"></a>Bir çoğaltmayı sil
Bir ana sunucuyu silme işlemine benzer bir okuma çoğaltmasını silebilirsiniz.

- Azure portal, okuma çoğaltması için **genel bakış** sayfasını açın. **Sil**’i seçin.

   :::image type="content" source="./media/howto-read-replicas-portal/delete-replica.png" alt-text="Çoğaltmaya genel bakış sayfasında, çoğaltmayı silmek için seçin":::
 
Ayrıca, aşağıdaki adımları izleyerek **çoğaltma** penceresinde okuma çoğaltmasını silebilirsiniz:

1. Azure portal, PostgreSQL için ana Azure veritabanı sunucunuzu seçin.

2. Sunucu menüsünde, **Ayarlar**' ın altında, **çoğaltma**' yı seçin.

3. Silinecek okuma çoğaltmasını seçin.

   :::image type="content" source="./media/howto-read-replicas-portal/select-replica.png" alt-text="Silinecek çoğaltmayı seçin":::
 
4. **Çoğaltmayı Sil**' i seçin.

   :::image type="content" source="./media/howto-read-replicas-portal/select-delete-replica.png" alt-text="Çoğaltmayı Sil ' i seçin":::
 
5. Silinecek çoğaltmanın adını girin. Çoğaltmanın silinmesini onaylamak için **Sil** ' i seçin.

   :::image type="content" source="./media/howto-read-replicas-portal/confirm-delete-replica.png" alt-text="Te çoğaltmasını Silmeyi Onayla":::
 

## <a name="monitor-a-replica"></a>Bir çoğaltmayı izleme
Okuma çoğaltmalarını izlemek için iki ölçüm mevcuttur.

### <a name="max-lag-across-replicas-metric"></a>Çoğaltmalar arasındaki en fazla gecikme ölçümü
**Çoğaltmalar genelinde en fazla gecikme** ölçümü, ana sunucu ile en fazla çoğaltma çoğaltması arasındaki gecikme sayısını gösterir. 

1.  Azure portal, PostgreSQL için ana Azure veritabanı sunucusu ' nu seçin.

2.  **Ölçümler**’i seçin. **Ölçümler** penceresinde **çoğaltmalar genelinde en fazla gecikme**' yı seçin.

    :::image type="content" source="./media/howto-read-replicas-portal/select-max-lag.png" alt-text="Çoğaltmalar genelinde en fazla gecikme 'yi izleme":::
 
3.  Toplamadır **,** **en fazla**' yı seçin.


### <a name="replica-lag-metric"></a>Çoğaltma gecikmesi ölçümü
**Çoğaltma gecikmesi** ölçümü, bir çoğaltmada son yeniden yürütülen işlemden bu yana geçen süreyi gösterir. Ana kuruluşunuzda gerçekleşen işlem yoksa, ölçüm bu gecikme süresini yansıtır.

1. Azure portal, PostgreSQL için Azure veritabanı okuma çoğaltması ' nı seçin.

2. **Ölçümler**’i seçin. **Ölçümler** penceresinde **çoğaltma gecikmesi**' nı seçin.

   :::image type="content" source="./media/howto-read-replicas-portal/select-replica-lag.png" alt-text="Çoğaltma gecikmesini izleme":::
 
3. Toplamadır **,** **en fazla**' yı seçin. 
 
## <a name="next-steps"></a>Sonraki adımlar
* [PostgreSQL Için Azure veritabanı 'nda okuma çoğaltmaları](concepts-read-replicas.md)hakkında daha fazla bilgi edinin.
* [Azure CLI ve REST API okuma çoğaltmaları oluşturma ve yönetme](howto-read-replicas-cli.md)hakkında bilgi edinin.