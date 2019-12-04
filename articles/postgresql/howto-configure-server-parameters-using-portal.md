---
title: Sunucu parametrelerini Yapılandırma-Azure portal-PostgreSQL için Azure veritabanı-tek sunucu
description: Bu makalede, Azure portal aracılığıyla PostgreSQL için Azure veritabanı 'nda Postgres parametrelerinin nasıl yapılandırılacağı açıklanır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 02/28/2018
ms.openlocfilehash: a9d078fe9aab12b9044733d17a1437801d5130a4
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74763688"
---
# <a name="configure-server-parameters-in-azure-database-for-postgresql---single-server-via-the-azure-portal"></a>PostgreSQL için Azure veritabanı 'nda sunucu parametrelerini Yapılandırma-Azure portal aracılığıyla tek sunucu 
Azure portal aracılığıyla bir PostgreSQL için Azure veritabanı sunucusu için yapılandırma parametrelerini listeleyebilir, gösterebilir ve güncelleştirebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar
Bu nasıl yapılır kılavuzunda adım adım yönergeler için şunlar gerekir:
- [PostgreSQL için Azure veritabanı sunucusu](quickstart-create-server-database-portal.md)

## <a name="viewing-and-editing-parameters"></a>Parametreleri görüntüleme ve görüntüleme
1. [Azure portalı](https://portal.azure.com) açın.

2. PostgreSQL için Azure Veritabanı sunucunuzu seçin.

3. **Ayarlar** bölümünde **sunucu parametreleri**' ni seçin. Sayfa, parametrelerin, değerlerinin ve açıklamalarının listesini gösterir.
Parametreler için ![genel bakış sayfası](./media/howto-configure-server-parameters-in-portal/3-overview-of-parameters.png)

4. Client_min_messages gibi numaralandırılmış tür parametrelerinin olası değerlerini görmek için **açılan** düğmeyi seçin.
açılan ![](./media/howto-configure-server-parameters-in-portal/4-enum-drop-down.png) Sırala

5. Cpu_index_tuple_cost gibi sayısal parametrelerin olası değerlerinin aralığını görmek için **ı** (bilgi) düğmesini seçin veya üzerine gelin.
![bilgi düğmesi](./media/howto-configure-server-parameters-in-portal/4-information-button.png)

6. Gerekirse, belirli bir parametreye daraltmak için **arama kutusunu** kullanın. Arama, parametrelerin adı ve açıklamasıdır.
![arama sonuçları](./media/howto-configure-server-parameters-in-portal/5-search.png)

7. Ayarlamak istediğiniz parametre değerlerini değiştirin. Bir oturumda yaptığınız tüm değişiklikler mor renkle vurgulanır. Değerleri değiştirdikten sonra **Kaydet**' i seçebilirsiniz. Ya da değişikliklerinizi **atabilirsiniz** .
değişiklikleri kaydetme veya atma ![](./media/howto-configure-server-parameters-in-portal/6-save-and-discard-buttons.png)

8. Parametreler için yeni değerler kaydettiyseniz, **Tümünü Sıfırla**' yı seçerek her şeyi varsayılan değerlere geri döndürebilirsiniz.
![tümünü Varsayılana sıfırla](./media/howto-configure-server-parameters-in-portal/7-reset-to-default-button.png)

## <a name="next-steps"></a>Sonraki adımlar
Hakkında bilgi edinin:
- [PostgreSQL için Azure veritabanı 'nda sunucu parametrelerine genel bakış](concepts-servers.md)
- [Azure CLı kullanarak parametreleri yapılandırma](howto-configure-server-parameters-using-cli.md)
