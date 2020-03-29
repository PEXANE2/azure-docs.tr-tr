---
title: Sunucu parametrelerini yapılandırma - Azure portalı - PostgreSQL için Azure Veritabanı - Tek Sunucu
description: Bu makalede, PostgreSQL için Azure Veritabanı'ndaki Postgres parametrelerinin Azure portalı üzerinden nasıl yapılandırılabildiğini açıklanmaktadır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 02/28/2018
ms.openlocfilehash: a9d078fe9aab12b9044733d17a1437801d5130a4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74763688"
---
# <a name="configure-server-parameters-in-azure-database-for-postgresql---single-server-via-the-azure-portal"></a>PostgreSQL - Single Server için Azure Veritabanı'ndaki sunucu parametrelerini Azure portalı üzerinden yapılandırın 
PostgreSQL sunucusu için bir Azure Veritabanı için yapılandırma parametrelerini Azure portalı üzerinden listeleyebilir, gösterebilir ve güncelleyebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar
İhtiyacınız olan bu nasıl yapılacağını kılavuzundan geçmek için:
- [PostgreSQL sunucusu için Azure Veritabanı](quickstart-create-server-database-portal.md)

## <a name="viewing-and-editing-parameters"></a>Parametreleri görüntüleme ve düzenleme
1. Azure [portalını](https://portal.azure.com)açın.

2. PostgreSQL için Azure Veritabanı sunucunuzu seçin.

3. **AYARLAR** bölümünde Sunucu **parametrelerini**seçin. Sayfa, parametrelerin, değerlerinin ve açıklamaların listesini gösterir.
![Parametrelere Genel Bakış Sayfası](./media/howto-configure-server-parameters-in-portal/3-overview-of-parameters.png)

4. client_min_messages gibi numaralandırılmış tür parametreleri için olası değerleri görmek için **açılır düğmeyi** seçin.
![Sayısal bırakma](./media/howto-configure-server-parameters-in-portal/4-enum-drop-down.png)

5. cpu_index_tuple_cost gibi sayısal parametreler için olası değerlerin aralığını görmek için **i** (bilgi) düğmesini seçin veya üzerine tıklayın.
![bilgi düğmesi](./media/howto-configure-server-parameters-in-portal/4-information-button.png)

6. Gerekirse, belirli bir parametreye daraltmak için **arama kutusunu** kullanın. Arama, parametrelerin adı ve açıklaması üzerindedir.
![Arama sonuçları](./media/howto-configure-server-parameters-in-portal/5-search.png)

7. Ayarlamak istediğiniz parametre değerlerini değiştirin. Oturumda yaptığınız tüm değişiklikler mor renkle vurgulanır. Değerleri değiştirdikten sonra **Kaydet'i**seçebilirsiniz. Ya da değişikliklerinizi **atabilirsiniz.**
![Değişiklikleri Kaydetme veya Atma](./media/howto-configure-server-parameters-in-portal/6-save-and-discard-buttons.png)

8. Parametreler için yeni değerler kaydettiyseniz, **tümünü varsayılana sıfırla'yı**seçerek her şeyi varsayılan değerlere geri döndürebilirsiniz.
![Tümlerini varsayılan olarak sıfırla](./media/howto-configure-server-parameters-in-portal/7-reset-to-default-button.png)

## <a name="next-steps"></a>Sonraki adımlar
Şunları öğrenin:
- [PostgreSQL için Azure Veritabanı'ndaki sunucu parametrelerine genel bakış](concepts-servers.md)
- [Azure CLI kullanarak parametreleri yapılandırma](howto-configure-server-parameters-using-cli.md)
