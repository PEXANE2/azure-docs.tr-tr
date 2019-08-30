---
title: "CSV 'ye Dönüştür: Modül başvurusu"
titleSuffix: Azure Machine Learning service
description: Bir veri kümesini, R veya Python betik modülleriyle yüklenebilen, verilebileceğiniz veya paylaşılabilen CSV biçimine dönüştürmek için Azure Machine Learning hizmetinde CSV 'ye dönüştürme modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ms.openlocfilehash: 2b8f235014efe440e518f1f9c4d0d454f6918955
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70128906"
---
# <a name="convert-to-csv-module"></a>CSV modülüne Dönüştür

Bu makalede Azure Machine Learning hizmeti için görsel arabirim (Önizleme) modülü açıklanır.

Bir veri kümesini, R veya Python betik modülleriyle yüklenebilen, verilelebilecek veya paylaşılabilen bir CSV biçimine dönüştürmek için bu modülü kullanın.

### <a name="more-about-the-csv-format"></a>CSV biçimi hakkında daha fazla bilgi 

"Virgülle ayrılmış değerler" için temsil eden CSV biçimi, birçok harici makine öğrenimi aracı tarafından kullanılan bir dosya biçimidir. CSV, R veya Python gibi açık kaynak dillerle çalışırken ortak bir değişim biçimidir.

Azure Machine Learning ' de çalışmalarınızın çoğunu olsanız bile, veri kümenizi dış araçlar 'da kullanmak üzere CSV 'ye dönüştürmeyi yararlı bulabileceğiniz zamanlar olabilir. Örneğin:

+ Excel ile açmak veya ilişkisel bir veritabanına içeri aktarmak için CSV dosyasını indirin.  
+ CSV dosyasını bulut depolama alanına kaydedin ve görselleştirmeler oluşturmak için Power BI adresinden bağlanın.  
+ Verileri R ve Python 'da kullanmak üzere hazırlamak için CSV biçimini kullanın. Doğrudan Python veya Jupyter Not defteri 'nden verilere erişmek için gereken kodu oluşturmak üzere modülün çıktısına sağ tıklayın. 

Bir veri kümesini CSV 'ye dönüştürdüğünüzde, dosya Azure ML çalışma alanınıza kaydedilir. Azure Storage yardımcı programını kullanarak dosyayı doğrudan açabilir ve kullanabilir veya modül çıktısına sağ tıklayıp CSV dosyasını bilgisayarınıza indirebilir veya R ya da Python kodunda kullanabilirsiniz.  

## <a name="how-to-configure-convert-to-csv"></a>CSV 'ye dönüştürme yapılandırma

1.  [CSV 'ye dönüştürme](./convert-to-csv.md) modülünü denemenize ekleyin. Bu modülü, arabirimindeki **veri biçimi dönüştürmeleri** grubunda bulabilirsiniz. 

2. Veri kümesini çıkaran herhangi bir modüle bağlayın.   
  
3.  Denemeyi çalıştırın.

### <a name="results"></a>Sonuçlar
  

[CSV 'ye Dönüştür](./convert-to-csv.md)çıktısına çift tıklayın ve bu seçeneklerden birini belirleyin.  

 + **Sonuç veri kümesi-> indirme**: , Verilerin bir kopyasını, yerel bir klasöre kaydedebilmeniz için CSV biçiminde hemen açar. Bir klasör belirtmezseniz, varsayılan bir dosya adı uygulanır ve CSV dosyası yerel **indirmeler** kitaplığına kaydedilir.


 + **Sonuç veri kümesi-veri kümesi olarak > kaydet**: CSV dosyasını, ayrı bir veri kümesi olarak Azure ML çalışma alanına geri kaydeder.

 + **Veri erişim kodu oluştur**: Azure ML, Python kullanarak ya da R kullanarak verilere erişmeniz için iki kod kümesi oluşturur. Verilere erişmek için kod parçacığını uygulamanıza kopyalayın. (*Veri erişim kodu oluşturma yakında yakında sunulacaktır.* )

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning hizmeti için [kullanılabilen modül kümesine](module-reference.md) bakın. 