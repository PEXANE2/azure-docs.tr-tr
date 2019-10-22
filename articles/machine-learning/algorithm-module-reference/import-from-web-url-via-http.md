---
title: "HTTP: Module başvurusu aracılığıyla Web URL 'sinden içeri aktar"
titleSuffix: Azure Machine Learning service
description: Bir makine öğrenimi ardışık düzeninde kullanmak üzere genel bir Web sayfasından veri okumak için Azure Machine Learning hizmetindeki HTTP modülü aracılığıyla Web URL 'sinden Içeri aktarmayı nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ms.openlocfilehash: e2521dabdab8e9365019f35514f2d8d235c9c014
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72693130"
---
# <a name="import-from-web-url-via-http-module"></a>HTTP modülü aracılığıyla Web URL 'sinden içeri aktar

Bu makalede Azure Machine Learning hizmeti için görsel arabirim (Önizleme) modülü açıklanır.

Bir makine öğrenimi ardışık düzeninde kullanmak üzere genel bir Web sayfasından veri okumak için bu modülü kullanın.

Bir Web sayfasında yayımlanan veriler için aşağıdaki kısıtlamalar geçerlidir:

- Veriler desteklenen biçimlerden birinde olmalıdır: CSV, TSV, ARFF veya SvmLight. Diğer veriler hatalara neden olur.
- Kimlik doğrulaması gerekli değil veya desteklenmiyor. Verilerin genel kullanıma açık olması gerekir. 

Veri almanın iki yolu vardır: Sihirbazı kullanarak veri kaynağını ayarlama veya el ile yapılandırma.

## <a name="use-the-data-import-wizard"></a>Veri alma Sihirbazı 'nı kullanma

1. **Veri alım** modülünü işlem hattınızı ekleyin. Modülü, **veri girişi ve çıkış** kategorisinde arabiriminde bulabilirsiniz.

2. **Veri alma Sihirbazı 'Nı Başlat** ' a tıklayın ve http aracılığıyla Web URL 'si seçin.

3. URL 'ye yapıştırın ve bir veri biçimi seçin.

4. Yapılandırma tamamlandığında.

Var olan bir veri bağlantısını düzenlemek için Sihirbazı yeniden başlatın. Sihirbaz, sıfırdan tekrar başlamak zorunda kalmaması için önceki tüm yapılandırma ayrıntılarını yükler

## <a name="manually-set-properties-in-the-import-data-module"></a>Verileri Içeri aktarma modülündeki özellikleri el ile ayarlama

Aşağıdaki adımlarda, içeri aktarma kaynağının el ile nasıl yapılandırılacağı açıklanır.

1. [Veri alım](import-data.md) modülünü işlem hattınızı ekleyin. Modülü, **veri girişi ve çıkış** kategorisinde arabiriminde bulabilirsiniz.

2. **Veri kaynağı**için **http üzerinden Web URL 'si**seçeneğini belirleyin.

3. **URL**için, yüklemek istediğiniz verileri içeren SAYFANıN tam URL 'sini yazın veya yapıştırın.

    URL, yüklenecek verileri içeren sayfaya site URL 'sini ve tam yolu dosya adı ve uzantısıyla birlikte içermelidir.

    Örneğin, aşağıdaki sayfa, University of California, Irvine 'nın makine öğrenimi deposundaki Iris veri kümesini içerir:

    `https://archive.ics.uci.edu/ml/machine-learning-databases/iris/iris.data`

4. **Veri biçimi**için, listeden desteklenen veri biçimlerinden birini seçin.

    Biçimi belirlemede önce verileri her zaman denetlemeniz önerilir. UC Irvine sayfası CSV biçimini kullanır. Desteklenen diğer veri biçimleri, TSV, ARFF ve SvmLight.

5. Veriler CSV veya TSV biçimindeyse, kaynak verilerin bir başlık satırı içerip içermediğini göstermek için **dosyanın üst bilgi satırı** seçeneğini kullanın. Başlık satırı, sütun adlarını atamak için kullanılır.

6. Verilerin çok fazla değişmeyeceğini veya işlem hattını her çalıştırdığınızda verileri yeniden yüklemeyi önlemek istiyorsanız **önbelleğe alınmış sonuçları kullan** seçeneğini belirleyin.

    Bu seçenek belirlendiğinde, işlem hattı, modül ilk kez çalıştırıldığında verileri yükler ve bundan sonra veri kümesinin önbelleğe alınmış bir sürümünü kullanır.

    Veri kümesini ardışık düzen veri kümesinin her tekrarında yeniden yüklemek istiyorsanız, **önbelleğe alınmış sonuçları kullan** seçeneğinin işaretini kaldırın. [Içeri aktarma verileri](import-data.md)parametrelerinde herhangi bir değişiklik varsa sonuçlar da yeniden yüklenir.

7. İşlem hattını çalıştırma.

## <a name="results"></a>Sonuçlar

Tamamlandığında, çıktı veri kümesine tıklayın ve verilerin başarıyla içeri aktarılmadığını görmek için **Görselleştir** ' i seçin.


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning hizmeti için [kullanılabilen modül kümesine](module-reference.md) bakın. 