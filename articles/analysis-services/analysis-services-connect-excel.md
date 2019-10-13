---
title: Excel ile Azure Analysis Services bağlanma | Microsoft Docs
description: Excel kullanarak bir Azure Analysis Services sunucusuna nasıl bağlanacağınızı öğrenin.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 1ba6864d0bab460b270f6f27acced57d7943a17a
ms.sourcegitcommit: 8b44498b922f7d7d34e4de7189b3ad5a9ba1488b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2019
ms.locfileid: "72295323"
---
# <a name="connect-with-excel"></a>Excel ile bağlanma

Bir sunucu oluşturup bu sunucuya tablolu model dağıttıktan sonra, istemcileri bağlanabilir ve verileri keşfetmeye başlayabilir. 

## <a name="before-you-begin"></a>Başlamadan önce

Oturum açarken kullandığınız hesap, en azından okuma izinlerine sahip bir model veritabanı rolüne ait olmalıdır. Daha fazla bilgi edinmek için bkz. [Kimlik doğrulaması ve kullanıcı izinleri](analysis-services-manage-users.md). 

## <a name="connect-in-excel"></a>Excel 'de Bağlan

Excel 'deki bir sunucuya bağlanmak, Excel 2016 ve sonraki sürümlerde veri al kullanılarak desteklenir. Power Pivot içindeki tablo Içeri aktarma Sihirbazı 'Nı kullanarak bağlanma desteklenmez. 

1. Excel 'de, **veri** şeridinde **dış veri al** >  ' den**diğer kaynaklardan** >  ' e tıklayın**Analysis Services**.

2. Veri Bağlantısı Sihirbazı ' nın **sunucu adı**' nda protokol ve URI dahil sunucu adını girin. Örneğin, asazure://westcentralus.asazure.windows.net/advworks. Ardından, **oturum açma kimlik bilgileri**' nde, **aşağıdaki kullanıcı adını ve parolayı kullan**' ı seçin ve ardından kuruluş kullanıcı adını (örneğin nancy@adventureworks.com ve parola) yazın.

    > [!IMPORTANT]
    > Bir Microsoft hesabı, Live ID, Yahoo, Gmail, vb. ile oturum açarsanız veya Multi-Factor Authentication ile oturum açmanız gerekiyorsa, parola alanını boş bırakın. Ileri ' ye tıkladıktan sonra parola girmeniz istenir. 

    ![Excel oturum açmadan Bağlan](./media/analysis-services-connect-excel/aas-connect-excel-logon.png)

3. **Veritabanı ve tablo Seç**bölümünde, veritabanı ve model ya da perspektif ' i seçin ve ardından **son**' a tıklayın.
   
    ![Excel 'den Bağlan Model Seç](./media/analysis-services-connect-excel/aas-connect-excel-select.png)


## <a name="see-also"></a>Ayrıca bkz.

@No__t [istemci kitaplıkları](analysis-services-data-providers.md)-1  
[Sunucunuzu Yönetin](analysis-services-manage.md)     


