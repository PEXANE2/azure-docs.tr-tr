---
title: Excel ile Azure Analysis Services bağlanma | Microsoft Docs
description: Excel kullanarak bir Azure Analysis Services sunucusuna nasıl bağlanacağınızı öğrenin. Bağlandıktan sonra kullanıcılar verileri araştırmak için PivotTable 'Lar oluşturabilir.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: b5c46c8b85af59efaf611300e2ab7129b3ef3cde
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80411378"
---
# <a name="connect-with-excel"></a>Excel ile bağlanma

Bir sunucu oluşturup bu sunucuya tablolu model dağıttıktan sonra, istemcileri bağlanabilir ve verileri keşfetmeye başlayabilir. 

## <a name="before-you-begin"></a>Başlamadan önce

Oturum açarken kullandığınız hesap, en azından okuma izinlerine sahip bir model veritabanı rolüne ait olmalıdır. Daha fazla bilgi edinmek için bkz. [Kimlik doğrulaması ve kullanıcı izinleri](analysis-services-manage-users.md). 

## <a name="connect-in-excel"></a>Excel 'de Bağlan

Excel 'deki bir sunucuya bağlanmak, Excel 2016 ve sonraki sürümlerde veri al kullanılarak desteklenir. Power Pivot içindeki tablo Içeri aktarma Sihirbazı 'Nı kullanarak bağlanma desteklenmez. 

1. Excel 'de, **veri** şeridinde,**Analysis Services****diğer kaynaklardan** >  **dış veri** > Al ' a tıklayın.

2. Veri Bağlantısı Sihirbazı ' nın **sunucu adı**' nda protokol ve URI dahil sunucu adını girin. Örneğin, asazure://westcentralus.asazure.windows.net/advworks. Ardından, **oturum açma kimlik bilgileri**' nde, **aşağıdaki Kullanıcı adı ve parolayı kullan**' ı seçin ve ardından kuruluş Kullanıcı adı nancy@adventureworks.com' nı ve parola ' yı yazın.

    > [!IMPORTANT]
    > Bir Microsoft hesabı, Live ID, Yahoo, Gmail, vb. ile oturum açarsanız veya Multi-Factor Authentication ile oturum açmanız gerekiyorsa, parola alanını boş bırakın. Ileri ' ye tıkladıktan sonra parola girmeniz istenir. 

    ![Excel oturum açmadan Bağlan](./media/analysis-services-connect-excel/aas-connect-excel-logon.png)

3. **Veritabanı ve tablo Seç**bölümünde, veritabanı ve model ya da perspektif ' i seçin ve ardından **son**' a tıklayın.
   
    ![Excel 'den Bağlan Model Seç](./media/analysis-services-connect-excel/aas-connect-excel-select.png)


## <a name="see-also"></a>Ayrıca bkz.

[İstemci kitaplıkları](analysis-services-data-providers.md)   
[Sunucunuzu Yönetin](analysis-services-manage.md)     


