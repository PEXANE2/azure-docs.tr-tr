---
title: Sunucu Yönetme-Azure portal-MySQL için Azure veritabanı esnek sunucu
description: MySQL için Azure veritabanı esnek sunucusunu Azure portal yönetme hakkında bilgi edinin.
author: mksuni
ms.author: sumuth
ms.service: mysql
ms.topic: how-to
ms.date: 9/21/2020
ms.openlocfilehash: 7a01863b3a0c29e94550be67ca957655cff32660
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "90941073"
---
# <a name="manage-an-azure-database-for-mysql---flexible-server-preview-using-azure-portal"></a>Azure portal kullanarak MySQL için Azure veritabanını yönetme-esnek sunucu (Önizleme)


> [!IMPORTANT]
> MySQL için Azure veritabanı-esnek sunucu şu anda genel önizlemededir.

Bu makalede, MySQL için Azure veritabanı esnek sunucularını (Önizleme) nasıl yöneteceğiniz gösterilmektedir. Yönetim görevleri işlem ve depolama ölçeklendirmesi, Rest sunucu yöneticisi parolası ve sunucunuzu silme işlemleri içerir.

## <a name="sign-in"></a>Oturum açın
[Azure portalında](https://portal.azure.com) oturum açın. Azure portal esnek sunucu kaynağına gidin.

## <a name="scale-compute-and-storage"></a>İşlem ve depolamayı ölçeklendirme

Sunucu oluşturulduktan sonra gereksinimlerinize göre çeşitli [fiyatlandırma katmanları](https://azure.microsoft.com/pricing/details/mysql/) arasında ölçeklendirebilirsiniz. Ayrıca sanal çekirdek sayısını artırıp azaltarak işlem ve bellek kapasitesinin ölçeğini artırabilir veya azaltabilirsiniz.

1. Azure portal sunucunuzu seçin. **Ayarlar** bölümünde bulunan **işlem + depolama**' yı seçin.

2. Daha yüksek işlem katmanını kullanarak sunucuyu ölçeklendirmek veya depolama ya da sanal çekirdekleri istediğiniz bir değere yükselterek aynı katmanda ölçeklendirmek için **Işlem katmanını**, **sanal çekirdeği**, **depolamayı** değiştirebilirsiniz.

   > [!div class="mx-imgBorder"]
   > :::image type="content" source="./media/howto-manage-server-portal/scale-server.png" alt-text="depolama esnek sunucusunu ölçeklendirme":::

   > [!Important]
   > - Depolama alanı aşağı ölçeklendirilmez.
   > - Sanal çekirdekleri ölçeklendirmek, sunucunun yeniden başlatılmasına neden olur.

3. Değişiklikleri kaydetmek için **Tamam ' ı** seçin.

## <a name="reset-admin-password"></a>Yönetici parolasını sıfırla

Azure portal kullanarak yönetici rolü parolasını değiştirebilirsiniz.

1. Azure portal sunucunuzu seçin. **Genel bakış** penceresinde **Parolayı Sıfırla**' yı seçin.

2. Yeni bir parola girin ve parolayı onaylayın. Metin kutusu sizden parola karmaşıklığı gereksinimlerini ister.

   > [!div class="mx-imgBorder"]
   > :::image type="content" source="./media/howto-manage-server-portal/reset-password.png" alt-text="esnek sunucu için parolanızı sıfırlayın":::

3. Yeni parolayı kaydetmek için **Kaydet** ' i seçin.

## <a name="delete-a-server"></a>Sunucu silme

Artık gerekmiyorsa, sunucunuzu silebilirsiniz.

1. Azure portal sunucunuzu seçin. **Genel bakış** penceresinde **Sil**' i seçin.

2. Sunucuyu silmek istediğinizi onaylamak için giriş kutusuna sunucunun adını yazın.

   > [!div class="mx-imgBorder"]
   > :::image type="content" source="./media/howto-manage-server-portal/delete-server.png" alt-text="esnek sunucuyu silme":::

   > [!NOTE]
   > Sunucu silindiğinde geri alınamaz.

3. **Sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar
- [Sunucu başlatmayı veya durdurmayı öğrenin](how-to-stop-start-server-portal.md)
- [Bir sunucuyu geri yüklemeyi öğrenin](how-to-restore-server-portal.md)
- [Bağlantı sorunlarını giderme](how-to-troubleshoot-common-connection-issues.md)

