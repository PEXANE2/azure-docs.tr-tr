---
title: Sunucusuz SQL havuzuyla işlenen veriler
description: Bu belge, Azure depolama 'da sunucusuz SQL havuzu kullanarak verileri sorgularken işlenen verilerin nasıl hesaplanacağını açıklar.
services: synapse analytics
author: filippopovic
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql
ms.date: 11/05/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.openlocfilehash: 06eb02aa3dd4d5fc8bd3605dac480d5afa52d5fa
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2020
ms.locfileid: "93424431"
---
# <a name="data-processed-with-serverless-sql-pool-in-azure-synapse-analytics"></a>Azure SYNAPSE Analytics 'te sunucusuz SQL havuzu ile işlenen veriler

İşlenen veri, bir sorgu yürütülürken geçici olarak sistemde depolanan veri miktarıdır ve şunları içerir:

- Depolama alanından okunan veri miktarı – şunları içerir:
  - Veri okunurken okunan veri miktarı
  - Meta veriler okunurken okunan veri miktarı (Parquet gibi meta veriler içeren dosya biçimleri için)
- Ara sonuçlarda veri miktarı – bitiş noktanıza veri aktarımı da dahil olmak üzere, sorgu yürütmesi sırasında düğümler arasında aktarılan veriler sıkıştırılmamış biçimde. 
- Depolamaya yazılan veri miktarı – sonuç kümesini depolamaya aktarmak için CETAS kullanırsanız, yazılan baytlar ve CETAS 'ın SEÇIM bölümü için işlenen veri miktarı üzerinden ücretlendirilirsiniz.

Dosyaları depolamadan okumak son derece iyileştirilmiştir ve kullanımları:

- Önceden getirme-okunan veri miktarına küçük bir ek yük eklenebilir. Bir sorgu bir dosyanın tamamını okuyorsa, ek yük kalmaz. Bir dosya kısmen okunmasından (örneğin, Ilk N sorgularda olduğu gibi), önceden getirme ile biraz daha fazla veri okunacaktır.
- İyileştirilmiş CSV ayrıştırıcısı – CSV dosyalarını okumak için PARSER_VERSION = ' 2.0 ' kullanırsanız, depolama alanından okunan verilerin biraz artmasıyla sonuçlanır.  İyileştirilmiş CSV ayrıştırıcısı, dosyaları eşit büyüklükte parçalar halinde paralel olarak okur. Öbeklerin tüm satırları içereceği garanti yoktur. Tüm satırların ayrıştırılacağından emin olmak için, bitişik öbeklerin küçük parçaları da okunabilir, bu da küçük miktarda ek yük ekliyor.

## <a name="statistics"></a>İstatistikler

Sunucusuz SQL havuzu sorgu iyileştiricisi, en iyi sorgu yürütme planlarını oluşturmak için istatistiklere bağımlıdır. İstatistikleri el ile oluşturabilirsiniz, aksi olarak sunucusuz SQL havuzu tarafından otomatik olarak oluşturulur. Her iki durumda da istatistikler, belirtilen örnek hızda belirli bir sütun döndüren ayrı bir sorgu yürütülerek oluşturulur. Bu sorguda, işlenen ilişkili bir veri miktarı vardır.

Oluşturulan istatistiklerden faydalanabilir aynı veya başka bir sorgu çalıştırırsanız, mümkünse istatistikler yeniden kullanılacaktır ve istatistik oluşturma için hiçbir ek veri işlenmeyecektir.

Bir Parquet sütunu için istatistik oluşturmak, dosyaların yalnızca ilgili sütununu okumasına neden olur. Bir CSV sütunu için istatistik oluşturmak, dosyaların tamamının okunmasına ve ayrıştırılmasından kaynaklanır.

## <a name="rounding"></a>Yuvarlama

İşlenen veri miktarı en yakın MB 'ye kadar, sorgu başına en az 10 MB veri işlenmiş olacak şekilde yuvarlanır.

## <a name="what-is-not-included-in-data-processed"></a>İşlenen verilere nelerin dahil olmadığı

- Sunucu düzeyi meta verileri (oturum açma, roller, sunucu düzeyi kimlik bilgileri gibi)
- Uç noktanıza bu veritabanları olarak oluşturduğunuz veritabanları (kullanıcılar, roller, şemalar, görünümler, satır içi TVFs, saklı yordamlar, veritabanı kapsamlı kimlik bilgileri, dış veri kaynakları, dış dosya biçimleri, dış tablolar) gibi yalnızca meta veriler içerir.
  - Şema çıkarımı kullanırsanız, sütun adlarını ve veri türlerini çıkarması için dosya parçaları okunacaktır
- Belirtilen örnek yüzdesine göre verileri depolamadan işleyecek şekilde, ISTATISTIK oluştur haricinde DDL deyimleri
- Yalnızca meta veri sorguları

## <a name="reduce-amount-of-data-processed"></a>İşlenen veri miktarını azalt

Sorgu başına işlenen veri miktarını iyileştirebilir ve verilerinizi bölümleyerek ve Parquet gibi sıkıştırılmış bir sütunlu biçime dönüştürerek daha iyi performans sağlayabilirsiniz.

## <a name="examples"></a>Örnekler

İki tablo olduğunu varsayalım, her biri aynı veriye beş eşit ölçekli sütunda sahiptir:

- 5 TB 'lık CSV dosyası tarafından desteklenen population_csv tablo
- 1 TB 'lık Parquet dosyası tarafından desteklenen population_parquet tablosu-bu tablo, Parquet sıkıştırılmış verileri içerdiğinde öncekinden küçüktür
- 100 KB 'lık CSV dosyası tarafından desteklenen very_small_csv tablo

**Sorgu #1** : population_csv Sum (popülasyon) seçin

Bu sorgu, doldurma sütununun değerlerini almak için tüm dosyaları okur ve ayrıştırır. Düğümler bu tablodaki parçaları işleyecek, her parçanın popülasyon toplamı düğümler arasında aktarılır ve nihai toplam bitiş noktanıza aktarılır. Bu sorgu 5 TB veri işleyecek ve parçaların toplamlarını aktarmaya yönelik küçük ek yüke neden olur.

**Sorgu #2** : population_parquet Sum (popülasyon) seçin

Iquet gibi sıkıştırılmış ve sütun odaklı biçimlerin sorgulanmasıyla, sunucusuz SQL havuzu dosyanın tamamı yerine tek bir sıkıştırılmış sütun okuyabileceğinden, önceki sorguyla kıyasla daha az veri okumasına neden olur. Bu durumda, 0,2 TB okunacak (beş eşit boyutlu sütun, 0,2 TB). Düğümler bu tablodaki parçaları işleyecek, her parçanın popülasyon toplamı düğümler arasında aktarılır ve nihai toplam bitiş noktanıza aktarılır. Bu sorgu, 0,2 TB ve parçaların toplamlarını aktarmaya yönelik küçük bir ek yük işleyebilir.

**Sorgu #3** : select * from population_parquet

Bu sorgu tüm sütunları okuyacak ve tüm verileri sıkıştırılmamış biçimde aktaracaktır. Sıkıştırma biçimi 5:1 ise, 1 TB + sıkıştırılmamış verileri okumak için 6 TB 'yi işleyebilir.

**Sorgu #4** : very_small_csv Count (*) seçin

Bu sorgu tüm dosyaları okuyacaktır. Bu tablo için depolama alanındaki dosyaların toplam boyutu 100 KB 'dir. Düğümler bu tablonun parçalarını işler, her parçanın toplamı düğümler arasında aktarılır ve nihai toplam bitiş noktanıza aktarılır. Bu sorgu, 100 KB 'den fazla veri işleyecek. Bu sorgu için işlenen veri miktarı, [yuvarlama](#rounding)sırasında BELIRTILEN 10 MB 'a yuvarlanır.

## <a name="next-steps"></a>Sonraki adımlar

Sorguların performans için nasıl iyileştirileceği hakkında bilgi edinmek için [sunucusuz SQL havuzu Için en iyi yöntemleri](best-practices-sql-on-demand.md)inceleyin.
