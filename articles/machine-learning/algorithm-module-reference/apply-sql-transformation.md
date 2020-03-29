---
title: SQL Dönüşümü Uygulama
titleSuffix: Azure Machine Learning
description: Verileri dönüştürmek için giriş veri kümelerinde Bir SQLite sorgusu çalıştırmak için Azure Machine Learning'de SQL Dönüşümü Uygula modüllerini nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 09/09/2019
ms.openlocfilehash: 2e44a4861e2522b766aab9c7151d76c471dd2d8c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76314547"
---
# <a name="apply-sql-transformation"></a>SQL Dönüşümü Uygulama

Bu makalede, Azure Machine Learning tasarımcısının (önizleme) bir modülü açıklanmaktadır.

SQL Dönüşümü Uygula modüllerini kullanarak şunları yapabilirsiniz:
  
-   Sonuçlar için tablolar oluşturun ve veri kümelerini taşınabilir bir veritabanına kaydedin.  
  
-   Veri türlerinde özel dönüşümler gerçekleştirin veya agregalar oluşturun.  
  
-   Verileri filtrelemek veya değiştirmek ve sorgu sonuçlarını veri tablosu olarak döndürmek için SQL sorgu ekstrelerini çalıştırın.  

> [!IMPORTANT]
> Bu modülde kullanılan SQL motoru **SQLite'dir.** SQLite sözdizimi hakkında daha fazla bilgi için, daha fazla bilgi için [SQL'e SQLite tarafından anlaşıldı olarak](https://www.sqlite.org/index.html) bakın.  

## <a name="how-to-configure-apply-sql-transformation"></a>SQL Dönüşümü Uygula nasıl yapılandırılatır?  

Modül giriş olarak en fazla üç veri kümesi alabilir. Her giriş bağlantı noktasına bağlı veri kümelerine başvururken, `t1` `t2`adlarını `t3`, ve . Tablo numarası giriş bağlantı noktasının dizini gösterir.  
  
Kalan parametre, SQLite sözdizimini kullanan bir SQL sorgusudur. **SQL Script** metin kutusuna birden çok satır yazarken, her deyimi sonlandırmak için bir yarı-iki nokta üzerinde kullanın. Aksi takdirde, satır sonları boşluklara dönüştürülür.  

Bu modül, SQLite sözdiziminin tüm standart ifadelerini destekler. Desteklenmeyen deyimlerin listesi için [Teknik Notlar](#technical-notes) bölümüne bakın.

##  <a name="technical-notes"></a>Teknik notlar  

Bu bölümde uygulama ayrıntıları, ipuçları ve sık sorulan soruların yanıtları içerir.

-   Bağlantı noktası 1'de her zaman bir giriş gereklidir.  
  
-   Boşluk veya diğer özel karakterler içeren sütun tanımlayıcıları için, sütun tanımlayıcısını her zaman kare ayraçlara veya yan `SELECT` `WHERE` tümcedeki sütuna atıfta bulunurken çift tırnak işaretlerine bürün.  
  
### <a name="unsupported-statements"></a>Desteklenmeyen ifadeler  

SQLite, ANSI SQL standardının çoğunu desteklese de, ticari ilişkisel veritabanı sistemleri tarafından desteklenen birçok özellik içermez. Daha fazla bilgi için [SQL'e SQLite tarafından anlaşıldı olarak](http://www.sqlite.org/lang.html)bakın. Ayrıca, SQL deyimleri oluştururken aşağıdaki kısıtlamalara dikkat edin:  
  
- SQLite, çoğu ilişkisel veritabanı sisteminde olduğu gibi bir sütuna bir tür atamak yerine değerler için dinamik yazma kullanır. Zayıf bir şekilde yazılır ve örtülü tür dönüştürmesine izin verir.  
  
- `LEFT OUTER JOIN`uygulanır, ancak `RIGHT OUTER JOIN` uygulanmaz `FULL OUTER JOIN`veya .  

- `ALTER TABLE` Komutla `RENAME TABLE` birlikte `ADD COLUMN` ve deyimler kullanabilirsiniz, ancak diğer yan `DROP COLUMN`tümceler , , `ALTER COLUMN`ve `ADD CONSTRAINT`.  
  
- SQLite içinde bir GÖRÜNÜM oluşturabilirsiniz, ancak bundan sonra görünümler salt okunur. Bir görünümde `DELETE` `INSERT`bir `UPDATE` , veya deyimi yürütemezsiniz. Ancak, bir girişim üzerinde yangınları bir `DELETE` `INSERT`tetikleyici `UPDATE` oluşturabilirsiniz , , veya bir görünüm ve tetikleyici gövdesinde diğer işlemleri gerçekleştirmek.  
  

Resmi SQLite sitesinde sağlanan desteklenmeyen işlevler listesine ek olarak, aşağıdaki wiki diğer desteklenmeyen özelliklerin bir listesini sağlar: [SQLite - Desteklenmeyen SQL](http://www2.sqlite.org/cvstrac/wiki?p=UnsupportedSql)  
    
## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 
