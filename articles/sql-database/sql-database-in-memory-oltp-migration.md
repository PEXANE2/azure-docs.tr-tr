---
title: Bellek IÇI OLTP SQL txn perf geliştirir
description: Varolan bir SQL veritabanında işlem performansını artırmak için Bellek IÇI OLTP'yi benimseyin.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: MightyPen
ms.date: 11/07/2018
ms.openlocfilehash: 653ed75341d5d56ecbe06cb59f0efafa1e68aa0f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80067279"
---
# <a name="use-in-memory-oltp-to-improve-your-application-performance-in-sql-database"></a>SQL Veritabanı'ndaki uygulama performansınızı artırmak için Bellek içi OLTP'yi kullanın

[In-Memory OLTP,](sql-database-in-memory.md) fiyatlandırma katmanını artırmadan [Premium ve Business Critical katman](sql-database-service-tiers-vcore.md) veritabanlarında işlem işleme, veri alma ve geçici veri senaryolarının performansını artırmak için kullanılabilir. 

> [!NOTE] 
> SQL [Veritabanı ile DTU'nun %70 oranında düşürülmesiyle Quorum'un anahtar veritabanının iş yükünü](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database) nasıl ikiye katettiğini öğrenin


Varolan veritabanınızda Bellek Içi OLTP'yi benimsemek için aşağıdaki adımları izleyin.

## <a name="step-1-ensure-you-are-using-a-premium-and-business-critical-tier-database"></a>Adım 1: Premium ve İş Açısından Kritik seviye veritabanı kullandığınızdan emin olun

Bellek içi OLTP yalnızca Premium ve İş Açısından Kritik seviye veritabanlarında desteklenir. Döndürülen sonuç 1 ise Bellek Içi desteklenir (0 değil):

```
SELECT DatabasePropertyEx(Db_Name(), 'IsXTPSupported');
```

*XTP,* *Ekstrem İşlem İşlemi* anlamına gelir



## <a name="step-2-identify-objects-to-migrate-to-in-memory-oltp"></a>Adım 2: Bellek OLTP'ye geçirilmek üzere nesneleri tanımlama
SSMS, etkin iş yükü olan bir veritabanına karşı çalıştırabileceğiniz bir **İşlem Performans Analizi Genel Bakış** raporu içerir. Rapor, Bellek IÇI OLTP'ye geçiş için aday olan tabloları ve depolanmış yordamları tanımlar.

SSMS olarak, rapor oluşturmak için:

* Object **Explorer'da**veritabanı düğümünüze sağ tıklayın.
* Tıklayın **Raporlar** > **Standart Raporlar** > **İşlem Performans Analizi Genel Bakış**.

Daha fazla bilgi için bkz: [Tablo veya Depolanan Yordamın Bellek içi OLTP'ye taşınabilir olup olmaması gerektiğini belirleme.](https://msdn.microsoft.com/library/dn205133.aspx)

## <a name="step-3-create-a-comparable-test-database"></a>Adım 3: Karşılaştırılabilir bir test veritabanı oluşturma
Raporun veritabanınızın bellek için en iyi duruma getirilmiş bir tabloya dönüştürülmesinden yararlanacak bir tabloya sahip olduğunu gösterdiğini varsayalım. Endikasyonu test ederek onaylamak için ilk testi yapmanızı öneririz.

Üretim veritabanınızın test kopyasına ihtiyacınız var. Test veritabanı, üretim veritabanınızla aynı hizmet katmanı düzeyinde olmalıdır.

Sınama kolaylaştırmak için, test veritabanınızı aşağıdaki gibi ayarlayın:

1. SSMS kullanarak test veritabanına bağlanın.
2. Sorgularda WITH (SNAPSHOT) seçeneğine ihtiyaç duymamak için veritabanı seçeneğini aşağıdaki T-SQL deyiminde gösterildiği gibi ayarlayın:
   
   ```
   ALTER DATABASE CURRENT
    SET
        MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT = ON;
   ```

## <a name="step-4-migrate-tables"></a>Adım 4: Tabloları geçir
Sınamak istediğiniz tablonun bellek için en iyi duruma getirilmiş bir kopyasını oluşturmalı ve doldurmanız gerekir. Aşağıdakileri kullanarak oluşturabilirsiniz:

* SSMS kullanışlı Bellek Optimizasyonu Sihirbazı.
* Manuel T-SQL.

#### <a name="memory-optimization-wizard-in-ssms"></a>SSMS'te Bellek Optimizasyonu Sihirbazı
Bu geçiş seçeneğini kullanmak için:

1. SSMS ile test veritabanına bağlanın.
2. Nesne **Gezgini'nde,** tabloya sağ tıklayın ve ardından **Bellek Optimizasyon Danışmanı'nı**tıklatın.
   
   * **Tablo Bellek Optimize Edici Danışmanı** sihirbazı görüntülenir.
3. Sihirbazda, tablonun bellek için en iyi duruma getirilmiş tablolarda desteklenmeyen özellikleri olup olmadığını görmek için **Geçiş doğrulamasını** (veya **Sonraki** düğmeyi) tıklatın. Daha fazla bilgi için bkz.
   
   * [Bellek Optimizasyondanışmanı](https://msdn.microsoft.com/library/dn284308.aspx)bellek *optimizasyonu denetim listesi.*
   * [Transact-SQL Yapılar In-Memory OLTP tarafından desteklenmeyen](https://msdn.microsoft.com/library/dn246937.aspx).
   * [Bellek IÇI OLTP'ye geçiş.](https://msdn.microsoft.com/library/dn247639.aspx)
4. Tabloda desteklenmeyen özellikler yoksa, danışman sizin için gerçek şema ve veri geçişini gerçekleştirebilir.

#### <a name="manual-t-sql"></a>Manuel T-SQL
Bu geçiş seçeneğini kullanmak için:

1. SSMS (veya benzer bir yardımcı program) kullanarak test veritabanınıza bağlanın.
2. Tablonuz ve dizinleri için tam T-SQL komut dosyası edinin.
   
   * SSMS'te, tablo düğümünüze sağ tıklayın.
   * Yeni Sorgu**Penceresine** > CREATE **olarak** > Komut Dosyası**Tablosu'nu**tıklatın.
3. Komut dosyası penceresinde CREATE TABLE deyimine WITH (MEMORY_OPTIMIZED = AÇIK) ekleyin.
4. CLUSTERED dizini varsa, CLUSTERED olarak değiştirin.
5. SP_RENAME kullanarak varolan tabloyu yeniden adlandırın.
6. Düzenlenen CREATE TABLE komut dosyanızı çalıştırarak tablonun bellek için optimize edilmiş yeni kopyasını oluşturun.
7. INSERT'i kullanarak verileri bellek için optimize edilmiş tablonuza kopyalayın... SELECT * Içine:

```
INSERT INTO <new_memory_optimized_table>
        SELECT * FROM <old_disk_based_table>;
```


## <a name="step-5-optional-migrate-stored-procedures"></a>Adım 5 (isteğe bağlı): Depolanan yordamları geçirin
Bellek içi özellik, geliştirilmiş performans için depolanan yordamı da değiştirebilir.

### <a name="considerations-with-natively-compiled-stored-procedures"></a>Yerel olarak derlenmiş depolanmış yordamlarla ilgili hususlar
Yerel olarak derlenmiş bir saklı yordam, T-SQL WITH yan tümcesi üzerinde aşağıdaki seçeneklere sahip olmalıdır:

* NATIVE_COMPILATION
* SCHEMABINDING: depolanan yordamı bırakmadığınız sürece, depolanan yordamı etkileyebilecek şekilde sütun tanımlarının değiştirilemeyeceği tabloları anlamına gelir.

Yerel bir modül işlem yönetimi için büyük bir [ATOM blokları](https://msdn.microsoft.com/library/dn452281.aspx) kullanmalıdır. Açık bir BEGIN İşlemi veya ROLLBACK HAREKETI için bir rol yoktur. Kodunuz bir iş kuralının ihlalini algılarsa, atomik bloğu [THROW](https://msdn.microsoft.com/library/ee677615.aspx) deyimiyle sonlandırabilir.

### <a name="typical-create-procedure-for-natively-compiled"></a>Yerel olarak derlenen ler için tipik CREATE YORDAMı
Yerel olarak derlenmiş bir yordam oluşturmak için genellikle T-SQL aşağıdaki şablona benzer:

```
CREATE PROCEDURE schemaname.procedurename
    @param1 type1, …
    WITH NATIVE_COMPILATION, SCHEMABINDING
    AS
        BEGIN ATOMIC WITH
            (TRANSACTION ISOLATION LEVEL = SNAPSHOT,
            LANGUAGE = N'your_language__see_sys.languages'
            )
        …
        END;
```

* TRANSACTION_ISOLATION_LEVEL için, SNAPSHOT yerel olarak derlenen depolanan yordam için en yaygın değerdir. Ancak, diğer değerlerin bir alt kümesi de desteklenir:
  
  * TEKRARLANABILIR OKUMA
  * Serileştirilebilir
* Language değeri sys.languages görünümünde bulunmalıdır.

### <a name="how-to-migrate-a-stored-procedure"></a>Depolanan yordam ı nasıl geçirilir?
Geçiş adımları şunlardır:

1. Düzenli yorumlanan depolanan yordamiçin CREATE PROCEDURE komut dosyası edinin.
2. Üstbilgisini önceki şablonla eşleşecek şekilde yeniden yazın.
3. Depolanan yordam T-SQL kodunun yerel olarak derlenen saklı yordamlar için desteklenmeyen özellikleri kullanıp kullanmadığını sapta. Gerekirse geçici çözüm uygulayın.
   
   * Ayrıntılar [için, Yerel Olarak Derlenen Saklı Yordamlar için Geçiş Sorunları'na](https://msdn.microsoft.com/library/dn296678.aspx)bakın.
4. SP_RENAME kullanarak eski depolanan yordamı yeniden adlandırın. Ya da sadece DROP.
5. Düzenlenen CREATE PROCEDURE T-SQL komut dosyanızı çalıştırın.

## <a name="step-6-run-your-workload-in-test"></a>Adım 6: Testte iş yükünüzü çalıştırın
Test veritabanınızda üretim veritabanınızda çalışan iş yüküne benzer bir iş yükü çalıştırın. Bu, tablolar ve depolanan yordamlar için Bellek İçi özelliğini kullanmanızdan elde edilen performans kazancını ortaya çıkarmalıdır.

İş yükünün başlıca özellikleri şunlardır:

* Eşzamanlı bağlantı sayısı.
* Okuma/yazma oranı.

Test iş yükünü uyarlamak ve çalıştırmak için, [burada](sql-database-in-memory.md)gösterilen kullanışlı ostress.exe aracını kullanmayı düşünün.

Ağ gecikmesini en aza indirmek için testinizi veritabanının bulunduğu aynı Azure coğrafi bölgesinde çalıştırın.

## <a name="step-7-post-implementation-monitoring"></a>Adım 7: Uygulama sonrası izleme
Bellek içi uygulamalarınızın üretimdeki performans etkilerini izlemeyi düşünün:

* [Bellek İçi Depolamayı İzle](sql-database-in-memory-oltp-monitoring.md).
* [Dinamik yönetim görünümlerini kullanarak Azure SQL Veritabanı’nı izleme](sql-database-monitoring-with-dmvs.md)

## <a name="related-links"></a>İlgili bağlantılar
* [Bellek Içi OLTP (Bellek Içi Optimizasyon)](https://msdn.microsoft.com/library/dn133186.aspx)
* [Yerel Olarak Derlenmiş Depolanmış Yordamlara Giriş](https://msdn.microsoft.com/library/dn133184.aspx)
* [Bellek Optimizasyondanışmanı](https://msdn.microsoft.com/library/dn284308.aspx)

