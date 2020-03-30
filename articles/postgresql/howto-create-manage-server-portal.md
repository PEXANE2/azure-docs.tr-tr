---
title: PostgreSQL için Azure Veritabanını Yönet - Azure portalı
description: Azure portalından PostgreSQL sunucusu için Bir Azure Veritabanını nasıl yönetebilirsiniz öğrenin.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 11/20/2019
ms.openlocfilehash: bcddd64afca29ac9fdd5d284fc8f809ff9e2477d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74535697"
---
# <a name="manage-an-azure-database-for-postgresql-server-using-the-azure-portal"></a>Azure portalını kullanarak PostgreSQL sunucusu için bir Azure Veritabanını yönetme
Bu makalede, PostgreSQL sunucuları için Azure Veritabanınızı nasıl yönetacağınızı gösterilmektedir. Yönetim görevleri arasında bilgi işlem ve depolama ölçekleme, yönetici parola sıfırlama ve sunucu ayrıntılarının görüntülenmesi yer almaktadır.

## <a name="sign-in"></a>Oturum aç
[Azure portalında](https://portal.azure.com)oturum açın.

## <a name="create-a-server"></a>Sunucu oluşturma
PostgreSQL sunucusu için bir Azure Veritabanı oluşturma yı ve nasıl başlayacağınızı öğrenmek için [hızlı başlat'ı](quickstart-create-server-database-portal.md) ziyaret edin.

## <a name="scale-compute-and-storage"></a>Hesaplama ve depolamayı ölçeklendirin

Sunucu oluşturulduktan sonra, gereksinimleriniz değiştikçe Genel Amaç ve Bellek Optimize Edilmiş katmanlar arasında ölçeklendirebilirsiniz. Ayrıca, vCore'ları artırarak veya azaltarak hesaplamayı ve belleği ölçeklendirebilirsiniz. Depolama ölçeklendirilebilir (ancak, depolama alanını küçültemezsiniz).

### <a name="scale-between-general-purpose-and-memory-optimized-tiers"></a>Genel Amaç ve Bellek Optimize Edilmiş katmanlar arasında ölçeklendirme

Genel Amaç'tan Bellek Optimize Edilip Tam tersi ölçeklendirebilirsiniz. Sunucu oluşturulduktan sonra Temel katmanına ve içinden değiştirme desteklenmez. 

1. Azure portalında sunucunuzu seçin. **Ayarlar** bölümünde bulunan **Fiyatlandırma katmanını**seçin.

2. Ölçeklediğiniz şeye bağlı olarak **Genel Amaç** veya **Bellek Optimize Edilmiştir'ı**seçin. 

    ![değişim-fiyatlandırma-katman](./media/howto-create-manage-server-portal/change-pricing-tier.png)

    > [!NOTE]
    > Katmanları değiştirmek sunucunun yeniden başlatılmasına neden olur.

4. Değişiklikleri kaydetmek için **Tamam'ı** seçin.


### <a name="scale-vcores-up-or-down"></a>VCores'u yukarı veya aşağı ölçeklendirin

1. Azure portalında sunucunuzu seçin. **Ayarlar** bölümünde bulunan **Fiyatlandırma katmanını**seçin.

2. Kaydırıcıyı istediğiniz değere taşıyarak **vCore** ayarını değiştirin.

    ![ölçek hesaplama](./media/howto-create-manage-server-portal/scaling-compute.png)

    > [!NOTE]
    > VCores ölçekleme bir sunucu yeniden neden olur.

3. Değişiklikleri kaydetmek için **Tamam'ı** seçin.


### <a name="scale-storage-up"></a>Depolamayı ölçeklendir

1. Azure portalında sunucunuzu seçin. **Ayarlar** bölümünde bulunan **Fiyatlandırma katmanını**seçin.

2. Kaydırıcıyı istediğiniz değere taşıyarak **Depolama** ayarını değiştirin.

    ![ölçek depolama](./media/howto-create-manage-server-portal/scaling-storage.png)

    > [!NOTE]
    > Depolama küçültülmüş olamaz.

3. Değişiklikleri kaydetmek için **Tamam'ı** seçin.


## <a name="update-admin-password"></a>Yönetici parolasını güncelleştir
Azure portalını kullanarak yönetici rolünün parolasını değiştirebilirsiniz.

1. Azure portalında sunucunuzu seçin. Genel **Bakış** penceresinde **Parolayı Sıfırla'yı**seçin.

   ![genel bakış](./media/howto-create-manage-server-portal/overview-reset-password.png)

2. Yeni bir parola girin ve parolayı onaylayın. Textbox parola karmaşıklığı gereksinimleri hakkında size istemir.

   ![sıfırlama-parola](./media/howto-create-manage-server-portal/reset-password.png)

3. Yeni parolayı kaydetmek için **Tamam'ı** seçin.


## <a name="delete-a-server"></a>Sunucu silme

Artık ihtiyacınız yoksa sunucunuzu silebilirsiniz. 

1. Azure portalında sunucunuzu seçin. Genel **Bakış** penceresinde **Sil'i**seçin.

    ![delete](./media/howto-create-manage-server-portal/overview-delete.png)

2. Silmek istediğiniz sunucunun bu olduğunu doğrulamak için giriş kutusuna sunucunun adını yazın.

    ![onaylamak-silme](./media/howto-create-manage-server-portal/confirm-delete.png)

    > [!NOTE]
    > Bir sunucunun silmesi geri alınamaz.

3. **Sil**’i seçin.


## <a name="next-steps"></a>Sonraki adımlar
- [Yedeklemeler ve sunucu geri yükleme](howto-restore-server-portal.md) si hakkında bilgi edinin
- [PostgreSQL için Azure Veritabanı'nda aladeğiştirme ve izleme seçenekleri](concepts-monitoring.md) hakkında bilgi edinin
