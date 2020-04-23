---
title: Okuma yinelemelerini yönetme - Azure portalı - MariaDB için Azure Veritabanı
description: Bu makalede, portalı kullanarak MariaDB için Azure Veritabanı'nda okuma yinelemelerinin nasıl ayarlanıp yönetilen
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 4/21/2020
ms.openlocfilehash: 20d8e46d6fa6b031c809d629a6af41e8e682bcef
ms.sourcegitcommit: 75089113827229663afed75b8364ab5212d67323
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "82025093"
---
# <a name="how-to-create-and-manage-read-replicas-in-azure-database-for-mariadb-using-the-azure-portal"></a>Azure portalını kullanarak MariaDB için Azure Veritabanı'nda okuma yinelemeleri oluşturma ve yönetme

Bu makalede, Azure portalını kullanarak MariaDB hizmeti için Azure Veritabanı'nda okuma yinelemelerinin nasıl oluşturulacağını ve yöneteceğinizi öğreneceksiniz.

## <a name="prerequisites"></a>Önkoşullar

- Ana sunucu olarak kullanılacak [MariaDB sunucusu için](quickstart-create-mariadb-server-database-using-azure-portal.md) bir Azure Veritabanı.

> [!IMPORTANT]
> Okundu çoğaltma özelliği yalnızca Genel Amaç veya Bellek Optimize edilmiş fiyatlandırma katmanlarındaki MariaDB sunucuları için Azure Veritabanı için kullanılabilir. Ana sunucunun bu fiyatlandırma katmanlarından birinde olduğundan emin olun.

## <a name="create-a-read-replica"></a>Okuma yinelemesi oluşturma

Okuma çoğaltma sunucusu aşağıdaki adımları kullanarak oluşturulabilir:

1. [Azure portalında](https://portal.azure.com/)oturum açın.

2. Ana bilgisayar olarak kullanmak istediğiniz MariaDB sunucusu için varolan Azure Veritabanı'nı seçin. Bu eylem **Genel Bakış** sayfasını açar.

3. **AYARLAR**altında menüden **Çoğaltma'yı** seçin.

4. **Çoğaltma Ekle'yi**seçin.

   ![MariaDB için Azure Veritabanı - Çoğaltma](./media/howto-read-replica-portal/add-replica.png)

5. Çoğaltma sunucusu için bir ad girin.

    ![MariaDB için Azure Veritabanı - Çoğaltma adı](./media/howto-read-replica-portal/replica-name.png)

6. Yineleme sunucusunun konumunu seçin. Varsayılan konum, ana sunucununkiyle aynıdır.

    ![MariaDB için Azure Veritabanı - Yineleme konumu](./media/howto-read-replica-portal/replica-location.png)

7. Yinelemenin oluşturulmasını onaylamak için **Tamam'ı** seçin.

> [!NOTE]
> Okuma yinelemeleri ana sunucu yapılandırmasıyla oluşturulur. Çoğaltma sunucusu yapılandırması oluşturulduktan sonra değiştirilebilir. Yinelemenin ana değere ayak uydurabilmesi için yineleme sunucusunun yapılandırmasının ana değerden eşit veya daha büyük değerlerde tutulması önerilir.

Çoğaltma sunucusu oluşturulduktan sonra Çoğaltma **bıçağından** görüntülenebilir.

   ![MariaDB için Azure Veritabanı - Liste yinelemeleri](./media/howto-read-replica-portal/list-replica.png)

## <a name="stop-replication-to-a-replica-server"></a>Çoğaltma sunucusuna çoğaltmayı durdurma

> [!IMPORTANT]
> Bir sunucuya çoğaltmayı durdurmak geri alınamaz. Çoğaltma bir ana ve yineleme arasında durdurulduktan sonra geri alınamaz. Çoğaltma sunucusu daha sonra bağımsız bir sunucu haline gelir ve şimdi hem okuma ve yazma destekler. Bu sunucu yeniden çoğaltma içine yapılamaz.

Azure portalından bir ana ve çoğaltma sunucusu arasındaki çoğaltmayı durdurmak için aşağıdaki adımları kullanın:

1. Azure portalında, MariaDB sunucusu için ana Azure Veritabanınızı seçin. 

2. **AYARLAR**altında menüden **Çoğaltma'yı** seçin.

3. Çoğaltmayı durdurmak istediğiniz çoğaltma sunucusunu seçin.

   ![MariaDB için Azure Veritabanı - Çoğaltmayı durdurun select sunucusu](./media/howto-read-replica-portal/stop-replication-select.png)

4. **Çoğaltmayı Durdur'u**seçin.

   ![MariaDB için Azure Veritabanı - Çoğaltmayı durdur](./media/howto-read-replica-portal/stop-replication.png)

5. **Tamam'ı**tıklatarak çoğaltmayı durdurmak istediğinizi onaylayın.

   ![MariaDB için Azure Veritabanı - Çoğaltmayı durduronaylayın](./media/howto-read-replica-portal/stop-replication-confirm.png)

## <a name="delete-a-replica-server"></a>Çoğaltma sunucususilme

Azure portalından okuma yineleme sunucusunu silmek için aşağıdaki adımları kullanın:

1. Azure portalında, MariaDB sunucusu için ana Azure Veritabanınızı seçin.

2. **AYARLAR**altında menüden **Çoğaltma'yı** seçin.

3. Silmek istediğiniz çoğaltma sunucusunu seçin.

   ![MariaDB için Azure Veritabanı - Çoğaltma seç sunucususilme](./media/howto-read-replica-portal/delete-replica-select.png)

4. **Çoğaltmayı Sil'i** seçin

   ![MariaDB için Azure Veritabanı - Yinelemeyi sil](./media/howto-read-replica-portal/delete-replica.png)

5. Yinelemenin adını yazın ve yinelemenin silinmesini onaylamak için **Sil'i** tıklatın.  

   ![MariaDB için Azure Veritabanı - Yineleme yi onaylayın](./media/howto-read-replica-portal/delete-replica-confirm.png)

## <a name="delete-a-master-server"></a>Ana sunucusilme

> [!IMPORTANT]
> Bir ana sunucu durdurulduğunda, tüm çoğaltma sunucularına çoğaltma durdurulur ve ana sunucu silinir. Çoğaltma sunucuları artık hem okuma hem de yazma işlemlerini destekleyen tek başına sunucular haline gelir.

Azure portalından bir ana sunucuyu silmek için aşağıdaki adımları kullanın:

1. Azure portalında, MariaDB sunucusu için ana Azure Veritabanınızı seçin.

2. Genel **Bakış'tan** **Sil'i**seçin.

   ![MariaDB için Azure Veritabanı - Silme ana](./media/howto-read-replica-portal/delete-master-overview.png)

3. Ana sunucunun adını yazın ve ana sunucunun silinmesini onaylamak için **Sil'i** tıklatın.  

   ![MariaDB için Azure Veritabanı - Silme ana](./media/howto-read-replica-portal/delete-master-confirm.png)

## <a name="monitor-replication"></a>Çoğaltmayı izleme

1. Azure [portalında,](https://portal.azure.com/)izlemek istediğiniz MariaDB sunucusu için yineleme Azure Veritabanı'nı seçin.

2. Kenar çubuğunun **İzleme** bölümü **altında, Ölçümler'i**seçin:

3. Kullanılabilir ölçümlerin açılır listesinden **saniyeler içinde Çoğaltma gecikmesini** seçin.

   ![Çoğaltma gecikmesi'ni seçin](./media/howto-read-replica-portal/monitor-select-replication-lag.png)

4. Görüntülemek istediğiniz zaman aralığını seçin. Aşağıdaki resim 30 dakikalık bir zaman aralığı seçer.

   ![Zaman aralığını seçin](./media/howto-read-replica-portal/monitor-replication-lag-time-range.png)

5. Seçili zaman aralığıiçin çoğaltma gecikmesini görüntüleyin. Aşağıdaki resim, büyük bir iş yükü için son 30 dakikayı görüntüler.

   ![Zaman aralığını seçin](./media/howto-read-replica-portal/monitor-replication-lag-time-range-thirty-mins.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Okuma yinelemeleri](concepts-read-replicas.md) hakkında daha fazla bilgi edinin