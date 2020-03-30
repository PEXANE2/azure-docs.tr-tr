---
title: ODBC ile Azure Veri Gezgini'ne bağlanma
description: Bu makalede, Azure Veri Gezgini'ne Açık Veritabanı Bağlantısı (ODBC) bağlantısı nın nasıl ayarlandığı nızı öğreneceksiniz.
author: orspod
ms.author: orspodek
ms.reviewer: gabil
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/30/2019
ms.openlocfilehash: 1b2e7a79eb932f5b971dda1d5d51b650789394db
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74034025"
---
# <a name="connect-to-azure-data-explorer-with-odbc"></a>ODBC ile Azure Veri Gezgini'ne bağlanma

Açık Veritabanı Bağlantısı[(ODBC),](/sql/odbc/reference/odbc-overview)veritabanı erişimi için yaygın olarak kabul edilen bir uygulama programlama arabirimidir . Özel bağlayıcısı olmayan uygulamalardan Azure Veri Gezgini'ne bağlanmak için ODBC'yi kullanın.

Arka planda, uygulamalar *odbc*arabiriminde sürücüleri denilen veritabanına özgü modüllerde uygulanan işlevleri çağırır. Azure Veri Gezgini, SQL Server iletişim protokolünün[(MS-TDS)](/azure/kusto/api/tds/)bir alt kümesini destekler, böylece SQL Server için ODBC sürücüsünü kullanabilir.

Aşağıdaki videoyu kullanarak, bir ODBC bağlantısı oluşturmayı öğrenebilirsiniz. 

> [!VIDEO https://www.youtube.com/embed/qA5wxhrOwog]

Alternatif olarak, [ODBC veri kaynağını](#configure-the-odbc-data-source) aşağıda ayrıntılı olarak yapılandırabilirsiniz. 

Makalede, SQL Server ODBC sürücüsünü nasıl kullanacağınızı öğrenirsiniz, böylece ODBC'yi destekleyen herhangi bir uygulamadan Azure Veri Gezgini'ne bağlanabilirsiniz. 

## <a name="prerequisites"></a>Ön koşullar

Aşağıdakileri yapmanız gerekir:

* [Sql Server sürüm 17.2.0.1 veya daha sonraki](/sql/connect/odbc/download-odbc-driver-for-sql-server) işletim sisteminiz için Microsoft ODBC Sürücüsü.

## <a name="configure-the-odbc-data-source"></a>ODBC veri kaynağını yapılandırma

SQL Server için ODBC sürücüsünü kullanarak bir ODBC veri kaynağını yapılandırmak için aşağıdaki adımları izleyin.

1. Windows'da, *ODBC Veri Kaynakları'nı*arayın ve ODBC Veri Kaynakları masaüstü uygulamasını açın.

1. **Ekle'yi**seçin.

    ![Veri kaynağı ekleme](media/connect-odbc/add-data-source.png)

1. **SQL Server için ODBC Driver 17'yi** seçin ve ardından **Bitirin.**

    ![Sürücüyü seçin](media/connect-odbc/select-driver.png)

1. Bağlantı ve bağlanmak istediğiniz küme için bir ad ve açıklama girin ve **ardından İleri'yi**seçin. Küme * \<URL'si ClusterName\>şeklinde\< olmalıdır. Bölge\>.kusto.windows.net*.

    ![Sunucu seçin](media/connect-odbc/select-server.png)

1. **Active Directory Integrated'ı** seçin ve **Next'i**seçin.

    ![Aktif Dizin Entegre](media/connect-odbc/active-directory-integrated.png)

1. Örnek verileri sonra **Sonraki**veritabanını seçin.

    ![Varsayılan veritabanını değiştirme](media/connect-odbc/change-default-database.png)

1. Bir sonraki ekranda, tüm seçenekleri varsayılan olarak bırakın ve **ardından Finish'i**seçin.

1. **Test Veri Kaynağını**seçin.

    ![Test veri kaynağı](media/connect-odbc/test-data-source.png)

1. Testin başarılı olduğunu doğrulayın ve **tamam'ı**seçin. Test başarılı olmadıysa, önceki adımlarda belirttiğiniz değerleri denetleyin ve kümeye bağlanmak için yeterli izinlere sahip olduğunuzu sağlayın.

    ![Test başarılı oldu](media/connect-odbc/test-succeeded.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Tableau'dan Azure Veri Gezgini'ne bağlan](tableau.md)