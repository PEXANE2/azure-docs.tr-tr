---
title: Sunucu Yönetme-Azure portal-MySQL için Azure veritabanı
description: Azure portal bir MySQL için Azure veritabanı sunucusunu yönetmeyi öğrenin.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: 553234410a7b8b798d26f0e2fee6132a2602b1cc
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74770518"
---
# <a name="manage-an-azure-database-for-mysql-server-using-the-azure-portal"></a>Azure portal kullanarak MySQL için Azure veritabanı sunucusunu yönetme
Bu makalede MySQL sunucuları için Afire veritabanınızı nasıl yöneteceğiniz gösterilmektedir. Yönetim görevleri işlem ve depolama ölçeklendirmesi, yönetici parola sıfırlama ve sunucu ayrıntılarını görüntüleme içerir.

## <a name="sign-in"></a>Oturum aç
[Azure Portal](https://portal.azure.com)’ında oturum açın.

## <a name="create-a-server"></a>Sunucu oluşturma
MySQL için Azure veritabanı sunucusu oluşturma ve kullanmaya başlama hakkında bilgi edinmek için [hızlı başlangıç](quickstart-create-mysql-server-database-using-azure-portal.md) sayfasını ziyaret edin.

## <a name="scale-compute-and-storage"></a>İşlem ve depolamayı ölçeklendirme

Sunucu oluşturulduktan sonra, gereksinimleriniz değiştikçe Genel Amaçlı ve bellek için Iyileştirilmiş katmanlar arasında ölçeklendirebilirsiniz. Ayrıca, sanal çekirdekleri artırarak veya azaltarak işlem ve belleği da ölçeklendirebilirsiniz. Depolama alanı yukarı ölçeklendirilebilir (ancak, depolamayı ölçeklendirmezsiniz).

### <a name="scale-between-general-purpose-and-memory-optimized-tiers"></a>Genel Amaçlı ve bellek için Iyileştirilmiş katmanlar arasında ölçeklendirme

Genel Amaçlı En Iyi duruma getirilmiş ve tam tersi yönde ölçeklendirebilirsiniz. Sunucu oluşturulduktan sonra temel katmandan ve arasında geçiş yapmak desteklenmez. 

1. Azure portal sunucunuzu seçin. **Ayarlar** bölümünde bulunan **fiyatlandırma katmanını**seçin.

2. Ölçeklendirdiğinize göre **genel amaçlı** veya **bellek için iyileştirilmiş**' u seçin. 

    ![değişiklik-Fiyatlandırma Katmanı](./media/howto-create-manage-server-portal/change-pricing-tier.png)

    > [!NOTE]
    > Katmanların değiştirilmesi, sunucunun yeniden başlatılmasına neden olur.

4. Değişiklikleri kaydetmek için **Tamam ' ı** seçin.


### <a name="scale-vcores-up-or-down"></a>Sanal çekirdekleri yukarı veya aşağı ölçeklendirme

1. Azure portal sunucunuzu seçin. **Ayarlar** bölümünde bulunan **fiyatlandırma katmanını**seçin.

2. Kaydırıcıyı istediğiniz değere taşıyarak **Vcore** ayarını değiştirin.

    ![ölçek-işlem](./media/howto-create-manage-server-portal/scaling-compute.png)

    > [!NOTE]
    > Sanal çekirdekleri ölçeklendirmek, sunucunun yeniden başlatılmasına neden olur.

3. Değişiklikleri kaydetmek için **Tamam ' ı** seçin.


### <a name="scale-storage-up"></a>Depolama ölçeğini artırma

1. Azure portal sunucunuzu seçin. **Ayarlar** bölümünde bulunan **fiyatlandırma katmanını**seçin.

2. Kaydırıcıyı istediğiniz değere taşıyarak **depolama** ayarını değiştirin.

    ![ölçek depolama](./media/howto-create-manage-server-portal/scaling-storage.png)

    > [!NOTE]
    > Depolama alanı aşağı ölçeklendirilmez.

3. Değişiklikleri kaydetmek için **Tamam ' ı** seçin.


## <a name="update-admin-password"></a>Yönetici parolasını Güncelleştir
Azure portal kullanarak yönetici rolü parolasını değiştirebilirsiniz.

1. Azure portal sunucunuzu seçin. **Genel bakış** penceresinde **Parolayı Sıfırla**' yı seçin.

   ![genel bakış](./media/howto-create-manage-server-portal/overview-reset-password.png)

2. Yeni bir parola girin ve parolayı onaylayın. Metin kutusu sizden parola karmaşıklığı gereksinimlerini ister.

   ![sıfırlama-parola](./media/howto-create-manage-server-portal/reset-password.png)

3. Yeni parolayı kaydetmek için **Tamam ' ı** seçin.


## <a name="delete-a-server"></a>Sunucu silme

Artık gerekmiyorsa, sunucunuzu silebilirsiniz. 

1. Azure portal sunucunuzu seçin. **Genel bakış** penceresinde **Sil**' i seçin.

    ![delete](./media/howto-create-manage-server-portal/overview-delete.png)

2. Bu sunucunun silmek istediğiniz sunucu olduğunu onaylamak için, giriş kutusuna sunucunun adını yazın.

    ![Onayla-Sil](./media/howto-create-manage-server-portal/confirm-delete.png)

    > [!NOTE]
    > Sunucu silindiğinde geri alınamaz.

3. **Sil**’i seçin.


## <a name="next-steps"></a>Sonraki adımlar
- [Yedeklemeler ve sunucu geri yükleme](howto-restore-server-portal.md) hakkında bilgi edinin
- [MySQL Için Azure veritabanı 'nda ayarlama ve izleme seçenekleri](concepts-monitoring.md) hakkında bilgi edinin