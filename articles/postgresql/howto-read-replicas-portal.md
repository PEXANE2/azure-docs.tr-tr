---
title: Okuma yinelemelerini yönetme - Azure portalı - PostgreSQL için Azure Veritabanı - Tek Sunucu
description: Azure portalından PostgreSQL - Single Server için okuma yinelemelerini nasıl yönetebilirsiniz öğrenin.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 01/24/2020
ms.openlocfilehash: dd79618b8d9f016c92166edb9ecdb0bfb113947e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76768955"
---
# <a name="create-and-manage-read-replicas-in-azure-database-for-postgresql---single-server-from-the-azure-portal"></a>Azure portalından PostgreSQL - Tek Sunucu için Azure Veritabanı'nda okuma yinelemeleri oluşturma ve yönetme

Bu makalede, Azure portalından PostgreSQL için Azure Veritabanı'nda okuma yinelemelerinin nasıl oluşturulup yönetilenöğrenilir. Okuma yinelemeleri hakkında daha fazla bilgi edinmek için [genel bakışa](concepts-read-replicas.md)bakın.


## <a name="prerequisites"></a>Ön koşullar
[PostgreSQL sunucusunun](quickstart-create-server-database-portal.md) ana sunucu olması için bir Azure Veritabanı.

## <a name="prepare-the-master-server"></a>Ana sunucuyu hazırlama
Bu adımlar, Genel Amaç veya Bellek Optimize Edilmiş katmanlarda bir ana sunucu hazırlamak için kullanılmalıdır. Ana sunucu azure.replication_support parametresini ayarlayarak çoğaltma için hazırlanır. Çoğaltma parametresi değiştirildiğinde, değişikliğin etkili olması için sunucu yeniden başlatması gerekir. Azure portalında, bu iki adım tek bir düğme yle kapsüllenir: **Çoğaltma Desteğini Etkinleştir.**

1. Azure portalında, master olarak kullanmak üzere PostgreSQL sunucusu için varolan Azure Veritabanı'nı seçin.

2. Sunucu kenar çubuğunda, **SETTINGS**altında **Çoğaltma'yı**seçin.

> [!NOTE] 
> **Çoğaltma yıçöz'ün** gri renkte olduğunu görürseniz, çoğaltma ayarları varsayılan olarak sunucunuzda zaten ayarlanır. Aşağıdaki adımları atlayabilir ve bir okuma yinelemeoluşturmak için gidebilirsiniz. 

3. **Çoğaltma Desteğini Etkinleştir'i**seçin. 

   ![Çoğaltma desteğini etkinleştirme](./media/howto-read-replicas-portal/enable-replication-support.png)

4. Çoğaltma desteğini etkinleştirmek istediğinizi onaylayın. Bu işlem ana sunucuyu yeniden başlatacaktır. 

   ![Çoğaltma desteğini etkinleştirme onaylayın](./media/howto-read-replicas-portal/confirm-enable-replication.png)
   
5. İşlem tamamlandıktan sonra iki Azure portalı bildirimi alırsınız. Sunucu parametresini güncelleştirmek için bir bildirim vardır. Hemen izleyen sunucu yeniden başlatma için başka bir bildirim vardır.

   ![Başarı bildirimleri - etkinleştir](./media/howto-read-replicas-portal/success-notifications-enable.png)

6. Çoğaltma araç çubuğunu güncelleştirmek için Azure portal sayfasını yenileyin. Artık bu sunucu için okuma yinelemeleri oluşturabilirsiniz.

   ![Güncelleştirilmiş araç çubuğu](./media/howto-read-replicas-portal/updated-toolbar.png)
   
Çoğaltma desteğini etkinleştirmek, ana sunucu başına tek seferlik bir işlemdir. Kolaylık sağlamak için Çoğaltma **Desteğini Devre Dışı** Alabilme düğmesi sağlanır. Bu ana sunucuda hiçbir zaman çoğaltma oluşturamayacağınız dan emin değilseniz, çoğaltma desteğini devre dışı bırakmanızı önermiyoruz. Ana sunucunuzda varolan yinelemeler varken çoğaltma desteğini devre dışı kalamazsınız.


## <a name="create-a-read-replica"></a>Okuma yinelemesi oluşturma
Okuma yinelemesi oluşturmak için aşağıdaki adımları izleyin:

1. Ana sunucu olarak kullanmak üzere PostgreSQL sunucusu için varolan Azure Veritabanı'nı seçin. 

2. Sunucu kenar çubuğunda, **SETTINGS**altında **Çoğaltma'yı**seçin.

3. **Çoğaltma Ekle'yi**seçin.

   ![Yineleme ekleme](./media/howto-read-replicas-portal/add-replica.png)

4. Okundu çoğaltma için bir ad girin. 

    ![Yinelemeyi adlandırın](./media/howto-read-replicas-portal/name-replica.png)

5. Yineleme için bir konum seçin. Varsayılan konum, ana sunucununkiyle aynıdır.

    ![Bir konum seçin](./media/howto-read-replicas-portal/location-replica.png)

   > [!NOTE]
   > Yineleme oluşturabileceğiniz bölgeler hakkında daha fazla bilgi edinmek [için, okuma yineleme kavramları makalesini](concepts-read-replicas.md)ziyaret edin. 

6. Yinelemenin oluşturulmasını onaylamak için **Tamam'ı** seçin.

Yineleme, ana bilgisayarla aynı bilgi işlem ve depolama ayarları kullanılarak oluşturulur. Yineleme oluşturulduktan sonra, ana sunucudan bağımsız olarak birkaç ayar değiştirilebilir: bilgi işlem oluşturma, vCores, depolama ve yedekleme bekletme süresi. Fiyatlandırma katmanı, Temel katman alabilen veya temel katmandan bağımsız olarak değiştirilebilir.

> [!IMPORTANT]
> Ana sunucu ayarı yeni bir değere güncelleştirilmeden önce, yineleme ayarını eşit veya daha büyük bir değerle güncelleştirin. Bu eylem, yinelemenin ana ustada yapılan değişiklikleri takip etmesini yardımcı olur.

Okuma yinelemesi oluşturulduktan sonra **Çoğaltma** penceresinden görüntülenebilir:

![Çoğaltma penceresinde yeni yinelemeyi görüntüleme](./media/howto-read-replicas-portal/list-replica.png)
 

## <a name="stop-replication"></a>Çoğaltmayı durdurma
Ana sunucu ile okuma yinelemesi arasındaki çoğaltmayı durdurabilirsiniz.

> [!IMPORTANT]
> Ana sunucuya ve okuma yinelemesine çoğaltmayı durdurduktan sonra geri alınamaz. Okuma yinelemesi hem okumaları hem de yazmaları destekleyen bağımsız bir sunucu haline gelir. Bağımsız sunucu yeniden çoğaltma yapılamaz.

Azure portalından bir ana sunucu ile okuma yinelemesi arasındaki çoğaltmayı durdurmak için aşağıdaki adımları izleyin:

1. Azure portalında, PostgreSQL sunucusu için ana Azure Veritabanınızı seçin.

2. Sunucu menüsünde, **AYARLAR**altında **Çoğaltma'yı**seçin.

3. Çoğaltmayı durdurmak için çoğaltma sunucusunu seçin.

   ![Yinelemeyi seçin](./media/howto-read-replicas-portal/select-replica.png)
 
4. **Çoğaltmayı Durdur'u**seçin.

   ![Çoğaltmayı durdur'u seçin](./media/howto-read-replicas-portal/select-stop-replication.png)
 
5. Çoğaltmayı durdurmak için **Tamam'ı** seçin.

   ![Çoğaltmayı durdurmak için onaylayın](./media/howto-read-replicas-portal/confirm-stop-replication.png)
 

## <a name="delete-a-master-server"></a>Ana sunucusilme
Bir ana sunucuyu silmek için, PostgreSQL sunucusu için bağımsız bir Azure Veritabanını silmek için aynı adımları kullanırsınız. 

> [!IMPORTANT]
> Bir ana sunucusildiğinizde, okunan tüm yinelemelere çoğaltma durdurulur. Okuma yinelemeleri, artık hem okumaları hem de yazmaları destekleyen bağımsız sunucular haline gelir.

Bir sunucuyu Azure portalından silmek için aşağıdaki adımları izleyin:

1. Azure portalında, PostgreSQL sunucusu için ana Azure Veritabanınızı seçin.

2. Sunucu için **Genel Bakış** sayfasını açın. **Sil**’i seçin.

   ![Sunucuya Genel Bakış sayfasında, ana sunucuyu silmek için seçin](./media/howto-read-replicas-portal/delete-server.png)
 
3. Silmek için ana sunucunun adını girin. Ana sunucunun silinmesini onaylamak için **Sil'i** seçin.

   ![Ana sunucuyu silmek için onaylayın](./media/howto-read-replicas-portal/confirm-delete.png)
 

## <a name="delete-a-replica"></a>Bir çoğaltmayı sil
Ana sunucuyı nasıl sildiğinize benzer bir okuma yinelemesini silebilirsiniz.

- Azure portalında, okundu çoğaltma için **Genel Bakış** sayfasını açın. **Sil**’i seçin.

   ![Yinelemeye Genel Bakış sayfasında, yinelemeyi silmek için](./media/howto-read-replicas-portal/delete-replica.png)
 
Ayrıca aşağıdaki adımları izleyerek **Çoğaltma** penceresinden okundu yinelemesini de silebilirsiniz:

1. Azure portalında, PostgreSQL sunucusu için ana Azure Veritabanınızı seçin.

2. Sunucu menüsünde, **AYARLAR**altında **Çoğaltma'yı**seçin.

3. Silmek için okundu yinelemesini seçin.

   ![Silmek için yinelemeyi seçin](./media/howto-read-replicas-portal/select-replica.png)
 
4. **Çoğaltmayı Sil'i**seçin.

   ![Silme çoğaltma'yı seçin](./media/howto-read-replicas-portal/select-delete-replica.png)
 
5. Silmek için yinelemenin adını girin. Yinelemenin silinmesini onaylamak için **Sil'i** seçin.

   ![Te çoğaltmasını silmek için onaylayın](./media/howto-read-replicas-portal/confirm-delete-replica.png)
 

## <a name="monitor-a-replica"></a>Yinelemeyi izleme
Okunan yinelemeleri izlemek için iki ölçüm kullanılabilir.

### <a name="max-lag-across-replicas-metric"></a>Çoğaltmalar metrik genelinde Max Lag
**Çoğaltmalar Arasında Max Lag** ölçümü, ana sunucu ile en geri kalan yineleme arasındaki baytgecikmesini gösterir. 

1.  Azure portalında, PostgreSQL sunucusu için ana Azure Veritabanı'nı seçin.

2.  **Ölçümler**’i seçin. **Ölçümler** penceresinde, **Çoğaltmalar Arasında Max Lag'ı**seçin.

    ![Yinelemeler arasında maksimum gecikmeyi izleme](./media/howto-read-replicas-portal/select-max-lag.png)
 
3.  **Toplama**için **Max'i**seçin.


### <a name="replica-lag-metric"></a>Çoğaltma Gecikmesi ölçümü
**Yineleme Gecikmesi** ölçümü, yinelemede son yeniden oynama işleminden bu yana geçen süreyi gösterir. Ananızda herhangi bir işlem meydana gelmezse, metrik bu süre gecikmesini yansıtır.

1. Azure portalında, PostgreSQL okuma yinelemesi için Azure Veritabanı'nı seçin.

2. **Ölçümler**’i seçin. **Ölçümler** **penceresinde, Çoğaltma Gecikmesi'ni**seçin.

   ![Yineleme gecikmesini izleme](./media/howto-read-replicas-portal/select-replica-lag.png)
 
3. **Toplama**için **Max'i**seçin. 
 
## <a name="next-steps"></a>Sonraki adımlar
* [PostgreSQL için Azure Veritabanı'nda okuma yinelemeleri](concepts-read-replicas.md)hakkında daha fazla bilgi edinin.
* [Azure CLI ve REST API'sında okuma yinelemelerini nasıl oluşturup yöneteceğimize](howto-read-replicas-cli.md)öğrenin.