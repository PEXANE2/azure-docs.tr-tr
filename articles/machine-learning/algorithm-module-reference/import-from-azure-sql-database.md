---
title: Azure SQL veritabanından içeri aktar
titleSuffix: Azure Machine Learning service
description: Azure SQL veritabanından veri almak için Azure Machine Learning görsel arabirimindeki verileri Içeri aktarma modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 09/09/2019
ms.openlocfilehash: 374776088d93813f39122f2018b00466c55d2b6e
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72694616"
---
# <a name="import-from-azure-sql-database"></a>Azure SQL veritabanından içeri aktar

Bu makalede, Azure SQL veritabanından veri almak için Azure Machine Learning görsel arabirimindeki [verileri Içeri aktarma](import-data.md) modülünün nasıl kullanılacağı açıklanır.  

Veritabanından veri aktarmak için, hem sunucu adını hem de veritabanı adını ve tabloyu, görünümü veya sorguyu tanımlayan bir SQL ifadesini belirtmeniz gerekir.  

Genel olarak, Azure veritabanlarında veri depolama, Azure 'daki tabloları veya Blobları kullanmaktan daha pahalıdır. Abonelik türüne bağlı olarak bir veritabanında depoladığınız veri miktarına yönelik sınırlar da olabilir. Ancak, SQL Azure veritabanına karşı hiçbir işlem ücreti yoktur, bu seçenek, veri arama tabloları veya veri sözlükleri gibi daha küçük miktarlarda sık kullanılan bilgilere hızlı erişim için idealdir.

Verileri okumadan önce filtreleyebilmeniz gerekiyorsa veya raporlama için tahmine dayalı veya ölçümleri veritabanına geri kaydetmek istiyorsanız, verilerin bir Azure veritabanında depolanması da tercih edilir.

## <a name="how-to-import-data-from-azure-sql-database"></a>Azure SQL veritabanından verileri içeri aktarma

1. [Veri alım](import-data.md) modülünü işlem hattınızı ekleyin. Bu modülü, veri girişi ve çıkış kategorisinde görsel arabirimde bulabilirsiniz.

1. **Veri kaynağı**IÇIN **Azure SQL veritabanı**' nı seçin.

1. Azure SQL veritabanı 'na özel aşağıdaki seçenekleri ayarlayın.

    **Veritabanı sunucusu adı**: Azure tarafından oluşturulan sunucu adını yazın. Genellikle form `<generated_identifier>.database.windows.net` olur.

    **Veritabanı adı**: belirttiğiniz sunucuda var olan bir veritabanının adını yazın.

    **Sunucu Kullanıcı hesabı adı**: veritabanı için erişim izinlerine sahip olan bir hesabın kullanıcı adını yazın.

    **Sunucu Kullanıcı hesabı parolası**: belirtilen kullanıcı hesabı için parola belirtin.

    **Veritabanı sorgusu**: okumak istediğiniz verileri açıklayan bir SQL ifadesini yazın veya yapıştırın. Visual Studio Sunucu Gezgini veya SQL Server Veri Araçları gibi bir araç kullanarak SQL ifadesini her zaman doğrulayın ve sorgu sonuçlarını önceden doğrulayın.

1. Azure Machine Learning ' a okunan veri kümesinin, işlem hattının çalıştırmaları arasında değiştirilmesi beklenmiyorsa, **önbelleğe alınmış sonuçları kullan** seçeneğini belirleyin.

    Seçildiğinde, modül parametrelerinde başka değişiklik yoksa, işlem hattı modül ilk kez çalıştırıldığında verileri yükler ve bundan sonra veri kümesinin önbelleğe alınmış bir sürümünü kullanır.

    Veri kümesini her işlem hattının tekrarında yeniden yüklemek istiyorsanız bu seçeneğin seçimini kaldırın. Veri kümesi, [Içeri aktarma verilerinde](import-data.md)herhangi bir parametre değiştirildiğinde kaynaktan yeniden yüklenir.

1. İşlem hattını çalıştırma.

    [Içeri aktarma verileri](import-data.md) verileri görsel arabirime yüklediğine göre, kaynak veritabanında kullanılan veri türlerine bağlı olarak bazı örtük tür dönüştürme da gerçekleştirilebilir.

## <a name="results"></a>Sonuçlar

İçeri aktarma tamamlandığında, çıktı veri kümesine tıklayın ve verilerin başarıyla içeri aktarılmadığını görmek için **Görselleştir** ' i seçin.

İsteğe bağlı olarak, görsel arabirimdeki araçları kullanarak veri kümesini ve meta verilerini değiştirebilirsiniz:

- Sütun adlarını değiştirmek, bir sütunu farklı bir veri türüne dönüştürmek veya hangi sütunların Etiketler ya da Özellikler olduğunu belirtmek için [meta verileri Düzenle](edit-metadata.md) ' yi kullanın.

- Sütunların bir alt kümesini seçmek için [veri kümesindeki sütunları seç '](select-columns-in-dataset.md) i kullanın.

- Veri kümesini ölçütlere göre ayırmak veya ilk n satırı almak için [bölüm ve örnek](partition-and-sample.md) kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning hizmeti için [kullanılabilen modül kümesine](module-reference.md) bakın. 
