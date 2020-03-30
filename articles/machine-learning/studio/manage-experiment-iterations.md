---
title: Deneme yinelemelerini yönetme
titleSuffix: ML Studio (classic) - Azure
description: Azure Machine Learning Studio'da (klasik) deneme yinelemelerini yönetme. Önceki varsayımları onaylamak veya iyileştirmek için önceki denemelerinizin önceki çalıştırmalarını istediğiniz zaman gözden geçirebilirsiniz.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.custom: seodec18
ms.date: 03/20/2017
ms.openlocfilehash: 5c8a278f09fdb3b605020e4c2fcf7aa2776906e7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79204316"
---
# <a name="manage-experiment-iterations-in-azure-machine-learning-studio-classic"></a>Azure Machine Learning Studio'da deneme yinelemelerini yönetme (klasik)

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

Öngörülü bir analiz modeli geliştirmek yinelemeli bir işlemdir - denemenizin çeşitli işlevlerini ve parametrelerini değiştirirken, eğitimli ve etkili bir modele sahip olduğunuzdan emin olana kadar sonuçlarınız birleşir. Bu işlemin anahtarı, deneme parametrelerinizin ve yapılandırmalarınızın çeşitli yinelemelerini izlemektir.



Önceki varsayımları onaylamak veya iyileştirmek için önceki denemelerinizin önceki çalıştırmalarını istediğiniz zaman gözden geçirebilirsiniz. Bir deneme çalıştırdığınızda, Machine Learning Studio (klasik) veri seti, modül ve bağlantı noktası bağlantıları ve parametreleri de dahil olmak üzere bir çalışma geçmişini tutar. Bu geçmiş aynı zamanda sonuçları, başlangıç ve durdurma saatleri, günlük iletileri ve yürütme durumu gibi çalışma zamanı bilgilerini de yakalar. Denemenizin ve ara sonuçlarınızın kronolojisini incelemek için istediğiniz zaman bu çalıştırmalardan herhangi birini geriye bakabilirsiniz. Hatta basit, karmaşık, hatta topluluk modelleme çözümleri oluşturma yolunda soruşturma ve keşif yeni bir aşamaya başlatmak için deneme önceki bir çalışma kullanabilirsiniz.

> [!NOTE]
> Bir denemenin önceki çalışmasını görüntülediğinizde, denemenin bu sürümü kilitlidir ve düzenlenemez. Ancak, **KAYDET'i** tıklatarak ve kopya için yeni bir ad sağlayarak bir kopyasını kaydedebilirsiniz. Machine Learning Studio (klasik) daha sonra edinebilir ve çalıştırabilirsiniz yeni kopyasını açar. Denemenizin bu kopyası, diğer tüm denemelerinizle birlikte **DENEYLER** listesinde bulunur.
> 
> 

## <a name="viewing-the-prior-run"></a>Önceki Çalıştırmayı Görüntüleme
En az bir kez çalıştırdığınız bir deneme açık olduğunda, özellikler bölmesinde Önceki **Çalıştır'ı** tıklatarak denemenin önceki çalışmasını görüntüleyebilirsiniz.

Örneğin, bir deneme oluşturduğunuzu ve sürümlerini 11:23, 11:42 ve 11:55'te çalıştırdığınızı varsayalım. Denemenin son çalışmasını açar sanız (11:55) ve **Önceki Çalıştır'ı**tıklattığınızda, 11:42'de çalıştırdığınız sürüm açılır.

## <a name="viewing-the-run-history"></a>Çalışma Geçmişini Görüntüleme
Açık bir denemede **Çalıştır Geçmişini Görüntüle'yi** tıklatarak bir denemenin önceki tüm çalıştırmalarını görüntüleyebilirsiniz.

Örneğin, [Doğrusal Regresyon][linear-regression] modülüyle bir deneme oluşturduğunuzu ve **Öğrenme oranının** değerini deneme sonuçlarınız üzerindeki değerini değiştirmenin etkisini gözlemlemek istediğinizi varsayalım. Denemeyi bu parametre için farklı değerlerle birden çok kez çalıştırın:

| Öğrenme Oranı değeri | Başlangıç saatini çalıştırın |
| --- | --- |
| 0.1 |11.09.2014 16:18:58 |
| 0.2 |11.09.2014 16:24:33 |
| 0,4 |11.09.2014 16:28:36 |
| 0,5 |11.09.2014 16:33:31 |

**VIEW RUN HISTORY'yi**tıklattığınızda, tüm bu çalıştırmaların bir listesini görürsünüz:

![Örnek çalışma geçmişi](./media/manage-experiment-iterations/viewrunhistory.jpg)

Bu çalıştırmalardan herhangi birini tıklattığınızda denemenin anlık görüntüsünü görüntüleyin. Yapılandırma, parametre değerleri, açıklamalar ve sonuçlar, denemenizin çalışmasını tam olarak kaydetmek için korunur.

> [!TIP]
> Denemenin yinelemelerinizi belgelemek için, başlığı her çalıştırdığınızda değiştirebilir, özellikler bölmesinde denemenin **Özetini** güncelleyebilir ve değişikliklerinizi kaydetmek için tek tek modüllere yorum ekleyebilir veya güncelleyebilirsiniz. Başlık, özet ve modül yorumları denemenin her çalışmasında kaydedilir.
> 
> 

Machine Learning Studio'daki **(klasik) DENEYLER** sekmesindeki deneylerin listesi her zaman bir deneyin en son sürümünü görüntüler. Denemenin önceki bir çalışmasını açarsanız **(Önceki Çalıştır** veya **GÖRÜNÜM KAÇIŞ Tarİhİ'ni**kullanarak), **VIEW RUN HISTORY'yi** tıklatarak **STATE** ve **Düzenleme durumu**olan yinelemeyi seçerek taslak sürüme dönebilirsiniz.

## <a name="iterating-on-a-previous-run"></a>Önceki Çalıştırmada Yinele
**Önceki Çalıştır** veya **RUN HISTORY'yi** tıklatTığınızda ve önceki bir çalıştırmayı açtığınızda, tamamlanmış bir denemeyi salt okunur modunda görüntüleyebilirsiniz.

Denemenizin yinelemesini önceki bir çalışma için yapılandırma şeklinizle başlamak istiyorsanız, çalıştırmayı açıp **KAYDET AS'yi**tıklatarak bunu yapabilirsiniz. Bu, yeni bir başlık, boş bir çalıştırma geçmişi ve önceki çalıştırmanın tüm bileşenleri ve parametre değerleriyle yeni bir deneme oluşturur. Bu yeni deneme, Machine Learning Studio (klasik) ana sayfasındaki **DENEYLER** sekmesinde listelenir ve denemenizin bu yinelemesi için yeni bir çalışma geçmişi başlatarak bunu değiştirebilir ve çalıştırabilirsiniz. 

Örneğin, önceki bölümde gösterilen deneme çalıştırma geçmişine sahip olduğunuzu varsayalım. **Öğrenme oranı** parametresini 0,4 olarak ayarladığınızda neler olduğunu gözlemlemek ve **eğitim çağı** sayısı parametresi için farklı değerler denemek istiyorsunuz.

1. **VIEW RUN HISTORY'yi** tıklatın ve saat 16:28:36'da çalıştırdığınız (parametre değerini 0,4 olarak ayarladığınız) denemenin yinelemesini açın.
2. **KAYDET'i**tıklatın.
3. Yeni bir başlık girin ve **Tamam** onay işaretini tıklatın. Denemenin yeni bir kopyası oluşturulur.
4. Eğitim **çağı parametresini değiştirin.**
5. **ÇALıŞTıR'ı**tıklatın.

Artık denemenizin bu sürümünü değiştirmeye ve çalıştırmaya devam ederek çalışmanızı kaydetmek için yeni bir çalışma geçmişi oluşturarak devam edebilirsiniz.

<!-- Module References -->
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
