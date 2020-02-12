---
title: Deneme yinelemelerini yönetme
titleSuffix: ML Studio (classic) - Azure
description: Azure Machine Learning Studio (klasik) ' de deneme yinelemelerini yönetme. Önceki çalıştırmaları, denemelerinizi meydan okuyun, yeniden ziyaret, sonuçta onaylayın veya önceki varsayımlar daraltmak için herhangi bir zamanda gözden geçirebilirsiniz.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: zhanxia
ms.custom: seodec18
ms.date: 03/20/2017
ms.openlocfilehash: 71a20f0de9907b04b9dcfd63c0003f2540a2e370
ms.sourcegitcommit: 812bc3c318f513cefc5b767de8754a6da888befc
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77153392"
---
# <a name="manage-experiment-iterations-in-azure-machine-learning-studio-classic"></a>Azure Machine Learning Studio deneme yinelemelerini yönetme (klasik)
Tahmine dayalı analiz modeli geliştirmek olduğu bir süreçtir - çeşitli işlevleri ve denemenizi parametrelerini değiştirirken, memnun ana kadar sonuçlarınız yakınsanır eğitilmiş ve verimli bir model vardır. Bu işlem için anahtar deneme parametreleri ve yapılandırmaları çeşitli yinelemeler izlemektir.



Önceki çalıştırmaları, denemelerinizi meydan okuyun, yeniden ziyaret, sonuçta onaylayın veya önceki varsayımlar daraltmak için herhangi bir zamanda gözden geçirebilirsiniz. Bir deneme çalıştırdığınızda Machine Learning Studio (klasik), veri kümesi, modül ve bağlantı noktası bağlantıları ve parametreleri de dahil olmak üzere, çalıştırmanın bir geçmişini tutar. Bu geçmiş, ayrıca sonuçları, başlatma ve durdurma zamanları, günlük iletilerini ve yürütme durumu gibi çalışma zamanı bilgileri yakalar. Bu çalıştırmaları birini deneyin ve Ara sonuçlar uygulanmasına geçmişi gözden geçirmek istediğiniz zaman geri bakabilirsiniz. Yoldaki basit, karmaşık veya hatta topluluğu modelleme çözümleri oluşturmak için yeni bir sorgu ve bulma aşaması başlatmak için denemenizi önceki bir çalıştırmada bile kullanabilirsiniz.

> [!NOTE]
> Bir deney önceki bir çalıştırmada görüntülediğinizde, deneme sürümünün kilitli ve düzenlenemez. Bununla birlikte, **farklı kaydet** ' i tıklayarak ve kopya için yeni bir ad sağlayarak bir kopyasını kaydedebilirsiniz. Machine Learning Studio (klasik), daha sonra düzenleyebileceğiniz ve çalıştırabileceğiniz yeni kopyayı açar. Denemenizin bu kopyası, **denemeleri** listesinde tüm diğer denemeleri ile birlikte kullanılabilir.
> 
> 

## <a name="viewing-the-prior-run"></a>Önceki çalıştırma görüntüleme
En az bir kez çalıştırdığınız denemenizin bulunduğu bir deneyiniz varsa, Özellikler bölmesinde **önceki Çalıştır** ' a tıklayarak deneme sürümünün önceki çalıştırmasını görüntüleyebilirsiniz.

Örneğin, bir deneme oluşturmak ve 11: 23'te bu sürümlerini çalıştıran varsayalım 11:42 ve 11:55. Deneme sürümü (11:55) öğesini açıp **önceki çalıştırma**' ya tıkladığınızda, 11:42 ' de çalıştırdığınız sürüm açılır.

## <a name="viewing-the-run-history"></a>Çalıştırma geçmişini görüntüleme
Bir deneyin önceki çalıştırmalarını, açık bir deneyime göre **Çalıştır geçmişini görüntüle** ' ye tıklayarak görüntüleyebilirsiniz.

Örneğin, [Doğrusal regresyon][linear-regression] modülüyle bir deneme oluşturduğunuzu ve deneme sonuçlarınızda **öğrenme oranının** değerini değiştirmenin etkisini gözlemlemek istediğinizi varsayalım. Denemeyi birden çok kez bu parametre için farklı değerlerle şu şekilde çalıştırın:

| Öğrenme oranı değeri | Çalıştırma başlangıç zamanı |
| --- | --- |
| 0.1 |9/11/2014 4:18:58 pm |
| 0.2 |9/11/2014 4:24:33 pm |
| 0,4 |9/11/2014 4:28:36 pm |
| 0,5 |9/11/2014 4:33:31 pm |

**Çalışma geçmişini görüntüle**' ye tıklarsanız, tüm bu çalıştırmaların bir listesini görürsünüz:

![Örneği çalıştırma geçmişi](./media/manage-experiment-iterations/viewrunhistory.jpg)

Herhangi bir anlık görüntü deneyde, çalıştırdığınız zaman görüntülemek için bu çalışır birine tıklayın. Yapılandırma, parametre değerleri, açıklamaları ve sonuçları tüm bu çalıştırmanın denemenizin eksiksiz bir kaydı size korunur.

> [!TIP]
> Deneme yinelemelerinizi belgelemek için, her çalıştırdığınızda başlığı değiştirebilirsiniz, Özellikler bölmesinde deneyinin **özetini** güncelleştirebilir ve yaptığınız değişiklikleri kaydetmek için bağımsız modüllerde yorum ekleyebilir veya güncelleştirebilirsiniz. Başlık, Özet, modül açıklamaları deneme ile her bir çalıştırmanın kaydedilir.
> 
> 

Machine Learning Studio (klasik) içindeki **denemeleri** sekmesinde bulunan denemeleri listesi her zaman bir denemenin en son sürümünü görüntüler. Deneme sürümü ' nü (önceki **çalıştırma** veya **Görünüm çalıştırma geçmişi**kullanarak) açarsanız, **çalışma geçmişini görüntüle** ' ye tıklayarak ve **düzenlenebilir** **durumuna** sahip yinelemeyi seçerek Taslak sürümüne dönebilirsiniz.

## <a name="iterating-on-a-previous-run"></a>Önceki bir çalıştırmada yineleme
Önceki **çalıştırma** veya **Görünüm çalıştırma geçmişi** ' ne tıkladığınızda ve önceki bir çalıştırmayı açtığınızda, tamamlanmış bir denemeyi salt okuma modunda görüntüleyebilirsiniz.

Denemenizin bir yinelemesini önceki bir çalıştırma için yapılandırdığınız şekilde kullanmaya başlamak istiyorsanız, çalıştırmayı açıp **farklı kaydet**' e tıklayarak bunu yapabilirsiniz. Bu yeni bir deneme çalıştırma geçmişi, boş bir yeni başlık oluşturur ve tüm bileşenleri ve parametre değerlerini önceki çalıştırın. Bu yeni deneme Machine Learning Studio (klasik) giriş sayfasındaki **denemeleri** sekmesinde listelenir ve bu denemenizin bu yinelemesi için yeni bir çalıştırma geçmişi başlatarak onu değiştirebilir ve çalıştırabilirsiniz. 

Örneğin, denemeyi çalıştırma geçmişi önceki bölümde gösterilen olduğunu varsayalım. **Öğrenme oranı** parametresini 0,4 olarak ayarladığınızda ne olacağını gözlemlemek ve **eğitim dönemleri sayısı** için farklı değerler denemeniz gerekir.

1. **Çalışma geçmişini görüntüle** ' ye tıklayın ve 4:28:36 PM 'de çalıştırdığınız denemenin yinelemesini açın (içinde parametre değerini 0,4 olarak ayarlarsınız).
2. **Farklı kaydet**' e tıklayın.
3. Yeni bir başlık girin ve **Tamam** onay işaretine tıklayın. Denemeyi yeni bir kopyası oluşturulur.
4. **Eğitim dönemleri numarasını** değiştirin.
5. **Çalıştır**' a tıklayın.

Artık bu sürümünü denemenizi, çalıştırmak ve değiştirmek çalışmanızı kaydetmek için yeni bir çalıştırma geçmişi oluşturmaya devam edebilirsiniz.

<!-- Module References -->
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
