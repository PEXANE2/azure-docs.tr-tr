---
title: Üçüncü taraf uygulamalarla uyumluluk sorunları ve Azure SYNAPSE Analytics
description: Üçüncü taraf uygulamaların Azure SYNAPSE ile bulabileceği bilinen sorunları açıklar
services: synapse-analytics
author: periclesrocha
ms.service: synapse-analytics
ms.topic: troubleshooting
ms.subservice: ''
ms.date: 11/18/2020
ms.author: procha
ms.reviewer: jrasnick
ms.openlocfilehash: a1031656eaa5125d07ae078773379270b26625e7
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "98121388"
---
# <a name="compatibility-issues-with-third-party-applications-and-azure-synapse-analytics"></a>Üçüncü taraf uygulamalarla uyumluluk sorunları ve Azure SYNAPSE Analytics

SQL Server için oluşturulan uygulamalar, Azure SYNAPSE adanmış SQL havuzları ile sorunsuz bir şekilde çalışır. Ancak, bazı durumlarda SQL Server ' de yaygın olarak kullanılan özellikler ve dil öğeleri Azure SYNAPSE 'de kullanılamayabilir veya farklı şekilde davranmayabilir.

Bu makalede, Azure SYNAPSE Analytics ile üçüncü taraf uygulamaları kullanırken karşılaşabileceğiniz bilinen sorunlar listelenmektedir. 

## <a name="tableau-error-an-attempt-to-complete-a-transaction-has-failed-no-corresponding-transaction-found"></a>Tableau hatası: "bir işlemi tamamlamaya yönelik bir girişim başarısız oldu. Karşılık gelen işlem bulunamadı "

Azure SYNAPSE adanmış SQL havuzu sürüm 10.0.11038.0 'den başlayarak, saklı yordam çağrılarını yapan bazı Tableau sorguları şu hata iletisiyle başarısız olabilir: "**[Microsoft] [SQL Server IÇIN ODBC sürücü 17] [SQL Server] 111214; Bir işlemi tamamlamaya yönelik bir girişim başarısız oldu. Karşılık gelen işlem bulunamadı.**"

### <a name="cause"></a>Nedeni

Bu, Azure SYNAPSE adanmış SQL havuzunda, ODBC ve JDBC sürücüleri tarafından otomatik olarak çağrılan yeni sistem saklı yordamlarının sunumundan kaynaklanan bir sorundur. Bu sistem saklı yordamlarından biri, yürütme başarısız olursa açık işlemlerin sonlandırılmasına neden olabilir. Bu sorun, istemci uygulama mantığına bağlı olarak gerçekleşebilir.

### <a name="solution"></a>Çözüm
Azure SYNAPSE adanmış SQL havuzlarına bağlı Tableau kullanılırken bu belirli sorunu görmekte olan müşteriler, SQL bağlantısı 'nda FMTONLY 'ı Evet olarak ayarlamış olmalıdır. Tableau Desktop ve Tableau Server için, Tableau 'in bu parametreyi sürücüye geçirdiğinden emin olmak için bir Tableau veri kaynağı özelleştirmesi (TDC) dosyası kullanmanız gerekir.  

> [!NOTE] 
> Microsoft üçüncü taraf araçlar için destek sağlamaz. Bu çözümün Tableau Desktop 2020.3.2 ile birlikte çalışıp çalışmadığını test ettiğimiz sürece, bu geçici çözümü kendi kapasiteniz üzerinde kullanmanız gerekir.
>

* [Tableau Desktop 'ta bir TDC dosyası ile genel özelleştirmeler yapmayı öğrenmek için Tableau masaüstü belgelerine bakın.](https://help.tableau.com/current/pro/desktop/en-us/odbc_customize.htm)
* [Tableau Server 'da bir TDC dosyası ile genel özelleştirmeler yapmayı öğrenmek için, bkz.. Tableau Server ile TDC dosyası.](https://kb.tableau.com/articles/howto/using-a-tdc-file-with-tableau-server)

Aşağıdaki örnekte, FMTONLY = YES parametresini SQL bağlantı dizesine geçiren bir Tableau TDC dosyası gösterilmektedir:

```json
<connection-customization class='azure_sql_dw' enabled='true' version='18.1'>
    <vendor name='azure_sql_dw' />
    <driver name='azure_sql_dw' />
    <customizations>        
        <customization name='odbc-connect-string-extras' value='UseFMTONLY=yes' />
    </customizations>
</connection-customization>
```
TDC dosyalarını kullanma hakkında daha fazla bilgi için Tableau desteğe başvurun. 

## <a name="see-also"></a>Ayrıca bkz.

* [Azure SYNAPSE Analytics 'te adanmış SQL havuzu için T-SQL dil öğeleri.](./sql-data-warehouse-reference-tsql-language-elements.md?bc=%2fazure%2fsynapse-analytics%2fbreadcrumb%2ftoc.json&toc=%2fazure%2fsynapse-analytics%2ftoc.json)
* [T-SQL deyimleri, Azure SYNAPSE Analytics 'te adanmış SQL havuzu için desteklenir.](./sql-data-warehouse-reference-tsql-statements.md)