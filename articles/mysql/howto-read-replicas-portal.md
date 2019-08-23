---
title: Oluşturma ve MySQL için Azure veritabanı'nda salt okunur çoğaltmalar yönetme
description: Bu makalede, portalı kullanılarak MySQL için Azure veritabanı 'nda okuma çoğaltmalarının nasıl ayarlanacağı ve yönetileceği açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 05/21/2019
ms.openlocfilehash: 74f27f70c4a0752975a53b3889681d3910b1dd05
ms.sourcegitcommit: beb34addde46583b6d30c2872478872552af30a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69906433"
---
# <a name="how-to-create-and-manage-read-replicas-in-azure-database-for-mysql-using-the-azure-portal"></a>Azure portal kullanarak MySQL için Azure veritabanı 'nda okuma çoğaltmaları oluşturma ve yönetme

Bu makalede, Azure portal kullanarak MySQL için Azure veritabanı hizmetinde okuma çoğaltmaları oluşturmayı ve yönetmeyi öğreneceksiniz.

> [!IMPORTANT]
> Ana sunucunuz ile aynı bölgede veya seçtiğiniz diğer herhangi bir Azure bölgesinde bir okuma çoğaltması oluşturabilirsiniz. Bölgeler arası çoğaltma şu anda genel önizleme aşamasındadır.

## <a name="prerequisites"></a>Önkoşullar

- Bir [MySQL sunucusu için Azure veritabanı](quickstart-create-mysql-server-database-using-azure-portal.md) ana sunucu olarak kullanılır.

> [!IMPORTANT]
> Salt okunur çoğaltma özelliği yalnızca için Azure veritabanı genel amaçlı veya bellek için iyileştirilmiş fiyatlandırma katmanları MySQL sunucuları için kullanılabilir. Bu fiyatlandırma katmanlarından birini ana sunucusu olduğundan emin olun.

## <a name="create-a-read-replica"></a>Salt okunur bir çoğaltma oluşturma

Aşağıdaki adımlar kullanılarak, bir okuma çoğaltması sunucusu oluşturulabilir:

1. [Azure portal](https://portal.azure.com/) oturum açın.

2. Ana sunucu olarak kullanmak istediğiniz MySQL için Azure veritabanı sunucusunu seçin. Bu eylem açar **genel bakış** sayfası.

3. **Ayarlar**' ın altında, menüden **çoğaltma** ' yı seçin.

4. **Çoğaltma ekle**' yi seçin.

   ![MySQL için Azure veritabanı-çoğaltma](./media/howto-read-replica-portal/add-replica.png)

5. Çoğaltma sunucusu için bir ad girin.

    ![MySQL için Azure veritabanı-çoğaltma adı](./media/howto-read-replica-portal/replica-name.png)

6. Çoğaltma sunucusunun konumunu seçin. Varsayılan konum, ana sunucu ile aynıdır.

    ![MySQL için Azure veritabanı-çoğaltma konumu](./media/howto-read-replica-portal/replica-location.png)

   > [!NOTE]
   > İçinde bir çoğaltma oluşturabileceğiniz bölgeler hakkında daha fazla bilgi edinmek için [çoğaltma kavramlarını oku makalesini](concepts-read-replicas.md)ziyaret edin. 

7. Çoğaltmanın oluşturulmasını onaylamak için **Tamam ' ı** seçin.

> [!NOTE]
> Okuma çoğaltmaları aynı sunucu yapılandırma yöneticisi olarak oluşturulur. Çoğaltma sunucusu yapılandırması, oluşturulduktan sonra değiştirilebilir. Çoğaltma sunucusunun yapılandırmasını çoğaltma ana ayak olduğundan emin olmak için ana daha eşit veya daha fazla değerlerinde tutulması gereken önerilir.

Çoğaltma sunucusu oluşturulduktan sonra **çoğaltma** dikey penceresinden görüntülenebilir.

   ![MySQL için Azure veritabanı-liste çoğaltmaları](./media/howto-read-replica-portal/list-replica.png)

## <a name="stop-replication-to-a-replica-server"></a>Bir çoğaltma sunucusu için çoğaltma durdurma

> [!IMPORTANT]
> Bir sunucuya çoğaltma durdurma işlemi geri alınamaz. Bir ana ve çoğaltma arasında çoğaltmayı durdurdu sonra geri alınamaz. Çoğaltma sunucusu bir tek başına sunucu olur ve artık hem okuma hem de yazma işlemleri destekler. Bu sunucu bir yinelemeye yeniden yapılamıyor.

Azure portal ana ve çoğaltma sunucusu arasında çoğaltmayı durdurmak için aşağıdaki adımları kullanın:

1. Azure portal, MySQL için ana Azure veritabanı sunucusunu seçin. 

2. **Ayarlar**' ın altında, menüden **çoğaltma** ' yı seçin.

3. Çoğaltmasını durdurmak istediğiniz çoğaltma sunucusunu seçin.

   ![MySQL için Azure veritabanı-çoğaltmayı durdur Sunucu Seç](./media/howto-read-replica-portal/stop-replication-select.png)

4. **Çoğaltmayı durdur**' u seçin.

   ![MySQL için Azure veritabanı-çoğaltmayı durdur](./media/howto-read-replica-portal/stop-replication.png)

5. **Tamam**' a tıklayarak çoğaltmayı durdurmak istediğinizi onaylayın.

   ![MySQL için Azure veritabanı-çoğaltmayı durdur Onayla](./media/howto-read-replica-portal/stop-replication-confirm.png)

## <a name="delete-a-replica-server"></a>Çoğaltma sunucusunu Sil

Azure portal bir okuma çoğaltması sunucusunu silmek için aşağıdaki adımları kullanın:

1. Azure portal, MySQL için ana Azure veritabanı sunucusunu seçin.

2. **Ayarlar**' ın altında, menüden **çoğaltma** ' yı seçin.

3. Silmek istediğiniz çoğaltma sunucusunu seçin.

   ![MySQL için Azure veritabanı-çoğaltmayı silme sunucu seçme](./media/howto-read-replica-portal/delete-replica-select.png)

4. **Çoğaltmayı Sil** ' i seçin

   ![MySQL için Azure veritabanı-çoğaltmayı silme](./media/howto-read-replica-portal/delete-replica.png)

5. Çoğaltmanın adını yazın ve çoğaltmayı silmeyi onaylamak için **Sil** ' e tıklayın.  

   ![MySQL için Azure veritabanı-çoğaltmayı silme onaylama](./media/howto-read-replica-portal/delete-replica-confirm.png)

## <a name="delete-a-master-server"></a>Bir ana sunucu silme

> [!IMPORTANT]
> Ana sunucu silme tüm çoğaltma sunucuları için çoğaltma durdurulur ve ana sunucusunu siler. Artık hem okuma hem de yazma işlemleri destekleyen tek başına sunucular çoğaltma sunucusu olur.

Azure portal bir ana sunucuyu silmek için aşağıdaki adımları kullanın:

1. Azure portal, MySQL için ana Azure veritabanı sunucusunu seçin.

2. **Genel bakışta** **Sil**' i seçin.

   ![MySQL için Azure veritabanı-ana şablonu Sil](./media/howto-read-replica-portal/delete-master-overview.png)

3. Ana sunucunun adını yazın ve **Sil** ' e tıklayarak ana sunucuyu silmeyi onaylayın.  

   ![MySQL için Azure veritabanı-ana şablonu Sil](./media/howto-read-replica-portal/delete-master-confirm.png)

## <a name="monitor-replication"></a>Çoğaltmayı izleme

1. [Azure Portal](https://portal.azure.com/), Izlemek istediğiniz MySQL Için Azure veritabanı sunucusu çoğaltmasını seçin.

2. Kenar çubuğunun **izleme** bölümü altında **ölçümler**' i seçin:

3. Kullanılabilir ölçümler açılan listesinden **saniye cinsinden çoğaltma gecikmesi** seçin.

   ![Çoğaltma gecikmesini seçin](./media/howto-read-replica-portal/monitor-select-replication-lag.png)

4. Görüntülemek istediğiniz zaman aralığını seçin. Aşağıdaki görüntü 30 dakikalık bir zaman aralığı seçer.

   ![Zaman aralığını seçin](./media/howto-read-replica-portal/monitor-replication-lag-time-range.png)

5. Seçilen zaman aralığı için çoğaltma gecikmesini görüntüleyin. Aşağıdaki görüntüde son 30 dakika görüntülenir.

   ![Zaman aralığını seçin](./media/howto-read-replica-portal/monitor-replication-lag-time-range-thirty-mins.png)

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [çoğaltmaları okuyun](concepts-read-replicas.md)