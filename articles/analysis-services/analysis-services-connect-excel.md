---
title: Excel ile Azure Analiz Hizmetlerine Bağlanın | Microsoft Dokümanlar
description: Excel'i kullanarak Azure Çözümleme Hizmetleri sunucusuna nasıl bağlanabileceğinizi öğrenin. Bağlandıktan sonra, kullanıcılar verileri keşfetmek için PivotTable'lar oluşturabilir.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: bb76accfef94327386d0ff58d003782a9fe3b0d2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73573147"
---
# <a name="connect-with-excel"></a>Excel ile bağlanma

Bir sunucu oluşturduktan ve ona bir tabular modeli dağıttıktan sonra, istemciler bağlanabilir ve verileri keşfetmeye başlayabilir. 

## <a name="before-you-begin"></a>Başlamadan önce

Oturum açtırdığınız hesap, en az okuma izinleri olan bir model veritabanı rolüne ait olmalıdır. Daha fazla bilgi edinmek için bkz. [Kimlik doğrulaması ve kullanıcı izinleri](analysis-services-manage-users.md). 

## <a name="connect-in-excel"></a>Excel'de bağlan

Excel'de bir sunucuya bağlanma, Excel 2016 ve sonrası veriler alın kullanılarak desteklenir. Power Pivot'ta İçe Aktar Tablosu Sihirbazı kullanılarak bağlanma desteklenmez. 

1. Excel'de, **Veri** şeridinde, Analiz**Hizmetlerinden Diğer** > **Kaynaklardan**Dış Veri >  **Al'ı**tıklatın.

2. Veri Bağlantısı Sihirbazı'nda, **Sunucu adına,** protokol ve URI'yi içeren sunucu adını girin. Örneğin, asazure://westcentralus.asazure.windows.net/advworks. Ardından, **Logon kimlik bilgilerinde,** **aşağıdaki Kullanıcı Adı ve Parolayı**kullanın'ı nancy@adventureworks.comseçin ve ardından kuruluş kullanıcı adını, örneğin , ve parolayı yazın.

    > [!IMPORTANT]
    > Bir Microsoft Hesabı, Live ID, Yahoo, Gmail vb. ile oturum açtıysanız veya çok faktörlü kimlik doğrulamayla oturum açmanız gerekiyorsa, parola alanını boş bırakın. İleri'yi tıklattıktan sonra bir parola istenir. 

    ![Excel oturumaçlarından bağlan](./media/analysis-services-connect-excel/aas-connect-excel-logon.png)

3. **Veritabanı ve Tablo'yu seçin,** veritabanını ve modelveya perspektifi seçin ve ardından **Bitir'i**tıklatın.
   
    ![Excel select modelinden bağlan](./media/analysis-services-connect-excel/aas-connect-excel-select.png)


## <a name="see-also"></a>Ayrıca bkz.

[İstemci kitaplıkları](analysis-services-data-providers.md)   
[Sunucunuzu yönetme](analysis-services-manage.md)     


