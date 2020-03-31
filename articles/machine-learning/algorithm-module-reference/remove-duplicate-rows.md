---
title: 'Yinelenen Satırları Kaldırma: Modül Başvurusu'
titleSuffix: Azure Machine Learning
description: Bir veri kümesinden olası yinelenenleri kaldırmak için Azure Machine Learning'de Yinelenen Satırları Kaldır modüllerini nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/22/2019
ms.openlocfilehash: 490d3305abcbcd906a0f727d736db8cab7e4287e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79456030"
---
# <a name="remove-duplicate-rows-module"></a>Yinelenen Satırlar modüllerini kaldırma

Bu makalede, Azure Machine Learning tasarımcısı (önizleme) bir modül açıklanmaktadır.

Olası yinelenenleri bir veri kümesinden kaldırmak için bu modülü kullanın.

Örneğin, verilerinizin aşağıdaki gibi göründüğünü ve hastalar için birden çok kaydı temsil eder. 

| Hasta Kimliği | Baş harfler| Cinsiyet|Yaş|Kabul|
|----|----|----|----|----|
|1|F.M.| M| 53| Oca|
|2| F.A.M.| M| 53| Oca|
|3| F.A.M.| M| 24| Oca|
|3| F.M.| M| 24| Şub|
|4| F.M.| M| 23| Şub|
| | F.M.| M| 23| |
|5| F.A.M.| M| 53| |
|6| F.A.M.| M| NaN| |
|7| F.A.M.| M| NaN| |

Açıkçası, bu örnekte yinelenen verileri olan birden çok sütun vardır. Bunların gerçekten çoğaltılıp çoğaltılmadığı, veriler hakkındaki bilginize bağlıdır. 

+ Örneğin, birçok hastanın aynı ada sahip olduğunu biliyor olabilirsiniz. Herhangi bir ad sütununu kullanarak yinelenenleri ortadan kaldırmaz, yalnızca **kimlik** sütununu kullanırsınız. Bu şekilde, hastaların aynı ada sahip olup olmadıklarına bakılmaksızın yalnızca yinelenen kimlik değerlerine sahip satırlar filtrelenir.

+ Alternatif olarak, kimlik alanında yinelenenlere izin vermeye ve ad, soyad, yaş ve cinsiyet gibi benzersiz kayıtları bulmak için başka bir dosya bileşimi kullanmaya karar verebilirsiniz.  

Bir satırın yinelenen olup olmadığı ölçütlerini ayarlamak için, **anahtar**olarak kullanılacak tek bir sütun veya sütun kümesi belirtirsiniz. İki satır, yalnızca **tüm** anahtar sütunlarda değerler eşit olduğunda yinelenen olarak kabul edilir. Herhangi bir **satırın anahtarlar**için eksik değeri varsa, bunlar yinelenen satırlar olarak kabul edilmez. Örneğin, Cinsiyet ve Yaş yukarıdaki tabloda Anahtar olarak ayarlanırsa, 6 ve 7.

Modülü çalıştırdığınızda, bir aday veri kümesi oluşturur ve belirttiğiniz sütun kümesinde yinesi olmayan bir satır kümesi döndürür.

> [!IMPORTANT]
> Kaynak veri kümesi değiştirilmez; Bu modül, belirttiğiniz ölçütlere bağlı olarak yinelenenleri hariç tutmak üzere filtre uygulanmış yeni bir veri kümesi oluşturur.

## <a name="how-to-use-remove-duplicate-rows"></a>Yinelenen Satırları Kaldır nasıl kullanılır?

1. Modülü boru hattınıza ekleyin. **Veri Dönüştürme**, **Manipülasyon**altında Yinelenen **Satırları Kaldır** modüllerini bulabilirsiniz.  

2. Yinelenen satırlar için denetlemek istediğiniz veri kümesini bağlayın.

3. **Özellikler** bölmesinde, **Anahtar sütun seçim filtresi ifadesinin**altında, yinelenenleri tanımlamada kullanılacak sütunları seçmek için Başlat sütun **seçicisini**tıklatın.

    Bu bağlamda, **Anahtar** benzersiz bir tanımlayıcı anlamına gelmez. Sütun Seçici'yi kullanarak seçtiğiniz tüm **sütunlar anahtar sütunlar**olarak belirlenir. Seçili olmayan tüm sütunlar anahtar olmayan sütunlar olarak kabul edilir. Anahtar olarak seçtiğiniz sütunların birleşimi, kayıtların benzersizliğini belirler. (Bunu birden çok eşitlik birleşimleri kullanan bir SQL deyimi olarak düşünün.)

    Örnekler:

    + "Kimliklerin benzersiz olduğundan emin olmak istiyorum": Yalnızca kimlik sütununu seçin.
    + "Ad, soyad ve kimlik birleşiminin benzersiz olduğundan emin olmak istiyorum": Üç sütunu da seçin.

4. Yinelenenler bulunduğunda hangi satırın döndürüleceklerini belirtmek için **ilk yinelenen satırı Koru** onay kutusunu kullanın:

    + Seçilirse, ilk satır döndürülür ve diğerleri atılır. 
    + Bu seçeneğin işaretlerini geri alırsanız, son yinelenen satır sonuçlarda tutulur ve diğerleri atılır. 

5. Boru hattını gönderin.

6. Sonuçları gözden geçirmek için modülü sağ tıklatın ve **Visualize'ı**seçin. 

> [!TIP]
> Sonuçların anlaşılması zorsa veya bazı sütunları dikkate almak istemiyorsanız, [Dataset modülünde Sütunları Seç'i](./select-columns-in-dataset.md) kullanarak sütunları kaldırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 