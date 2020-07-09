---
title: Sunucu parametrelerini Yapılandırma-Azure portal-PostgreSQL için Azure veritabanı-tek sunucu
description: Bu makalede, Azure portal aracılığıyla PostgreSQL için Azure veritabanı 'nda Postgres parametrelerinin nasıl yapılandırılacağı açıklanır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: how-to
ms.date: 02/28/2018
ms.openlocfilehash: 7a7a98818ba565aad9f2404c77682171514c7573
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86117993"
---
# <a name="configure-server-parameters-in-azure-database-for-postgresql---single-server-via-the-azure-portal"></a>PostgreSQL için Azure veritabanı 'nda sunucu parametrelerini Yapılandırma-Azure portal aracılığıyla tek sunucu 
Azure portal aracılığıyla bir PostgreSQL için Azure veritabanı sunucusu için yapılandırma parametrelerini listeleyebilir, gösterebilir ve güncelleştirebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar
Bu nasıl yapılır kılavuzunda adım adım yönergeler için şunlar gerekir:
- [PostgreSQL için Azure veritabanı sunucusu](quickstart-create-server-database-portal.md)

## <a name="viewing-and-editing-parameters"></a>Parametreleri görüntüleme ve görüntüleme
1. [Azure Portal](https://portal.azure.com)açın.

2. PostgreSQL için Azure Veritabanı sunucunuzu seçin.

3. **Ayarlar** bölümünde **sunucu parametreleri**' ni seçin. Sayfa, parametrelerin, değerlerinin ve açıklamalarının listesini gösterir.
![Parametreler için genel bakış sayfası](./media/howto-configure-server-parameters-in-portal/3-overview-of-parameters.png)

4. Client_min_messages gibi numaralandırılmış tür parametrelerinin olası değerlerini görmek için **açılan** düğmeyi seçin.
![Açılan listesini numaralandır](./media/howto-configure-server-parameters-in-portal/4-enum-drop-down.png)

5. Cpu_index_tuple_cost gibi sayısal parametrelerin olası değerlerinin aralığını görmek için **ı** (bilgi) düğmesini seçin veya üzerine gelin.
![bilgi düğmesi](./media/howto-configure-server-parameters-in-portal/4-information-button.png)

6. Gerekirse, belirli bir parametreye daraltmak için **arama kutusunu** kullanın. Arama, parametrelerin adı ve açıklamasıdır.
![Arama sonuçları](./media/howto-configure-server-parameters-in-portal/5-search.png)

7. Ayarlamak istediğiniz parametre değerlerini değiştirin. Bir oturumda yaptığınız tüm değişiklikler mor renkle vurgulanır. Değerleri değiştirdikten sonra **Kaydet**' i seçebilirsiniz. Ya da değişikliklerinizi **atabilirsiniz** .
![Değişiklikleri kaydetme veya atma](./media/howto-configure-server-parameters-in-portal/6-save-and-discard-buttons.png)

8. Parametreler için yeni değerler kaydettiyseniz, **Tümünü Sıfırla**' yı seçerek her şeyi varsayılan değerlere geri döndürebilirsiniz.
![Tümünü Varsayılana sıfırla](./media/howto-configure-server-parameters-in-portal/7-reset-to-default-button.png)

## <a name="next-steps"></a>Sonraki adımlar
Şunları öğrenin:
- [PostgreSQL için Azure veritabanı 'nda sunucu parametrelerine genel bakış](concepts-servers.md)
- [Azure CLı kullanarak parametreleri yapılandırma](howto-configure-server-parameters-using-cli.md)
