---
title: Tümce eşleştirme ve hizalama-özel çevirici
titleSuffix: Azure Cognitive Services
description: Eğitim yürütmesi sırasında, paralel belgelerde bulunan cümleler eşleştirilmiş veya hizalanır. Özel çevirici, bu tümcenin bir cümlesini okuyarak her seferinde bir cümleyi bir tümce öğrenir. Ardından, bu iki tümce içindeki kelimeleri ve tümceleri birbirlerine hizalar.
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: swmachan
ms.topic: conceptual
ms.openlocfilehash: e9bc5c876da6bd2be1b22b389b819e51330b2e50
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68595463"
---
# <a name="sentence-pairing-and-alignment-in-parallel-documents"></a>Paralel belgelerde tümce eşleştirme ve hizalama

Eğitim sırasında, paralel belgelerde bulunan cümleler eşleştirilmiş veya hizalanır. Özel çevirmen, veri kümelerinin her birinde hizalanmış cümleler olarak eşlenebildiği cümle sayısını raporlar.

## <a name="pairing-and-alignment-process"></a>Eşleştirme ve hizalama işlemi

Özel çevirici, her seferinde bir cümleden oluşan Tümcelerin çevirilerini öğrenir. Kaynaktan bir cümleyi ve sonra bu tümcenin hedeften çevirisini okumalar. Ardından, bu iki tümce içindeki kelimeleri ve tümceleri birbirlerine hizalar. Bu işlem, bir tümcedeki sözcüklerin ve deyimlerin, bu tümcenin çevirisi içindeki eşdeğer sözcüklere ve tümceciklere bir haritasını oluşturmasını sağlar. Hizalama, sistemin birbirleriyle çevirileri olan cümleler üzerinde hareket etmeye çalışır.

## <a name="pre-aligned-documents"></a>Önceden hizalanmış belgeler

Paralel belgeleriniz olduğunu biliyorsanız, önceden hizalanmış metin dosyaları sağlayarak cümle hizalamasını geçersiz kılabilirsiniz. Her iki belgedeki tüm tümceleri metin dosyasına çıkarabilir, her satır için bir cümleyi organize edebilir ve bir `.align` uzantıya sahip yükleyebilirsiniz. Uzantı `.align` , tümce hizalamasını atlayabilmesi için özel çeviriciyi işaret eder.

En iyi sonuçlar için, dosyalarınızda her satırda bir cümle olduğuna emin olun. Tümce içinde yeni satır karakterleri eklemeyin ve bu işlem kötü hizalamalara neden olur.

## <a name="suggested-minimum-number-of-extracted-and-aligned-sentences"></a>Önerilen en az ayıklanan ve hizalanmış tümceler sayısı

Bir eğitimin başarılı olması için aşağıdaki tabloda, her bir veri kümesinde bulunan ayıklanan Tümcelerin ve hizalanmış Tümcelerin en az sayısını gösterilmektedir. En az bir ayıklanan tümce sayısı, tümce hizalamasının ayıklanan tüm tümceleri başarıyla hizalanamayacak şekilde dikkate almanız için önerilen en düşük hizalı cümle sayısından çok daha yüksek.

| Veri kümesi   | Önerilen en az ayıklanan tümce sayısı | Önerilen en düşük hizalanmış tümce sayısı | Maksimum hizalanmış tümce sayısı |
|------------|--------------------------------------------|------------------------------------------|--------------------------------|
| Eğitim   | 10,000                                     | 2,000                                    | Üst sınır yok                 |
| Ayarlarını     | 2,000                                      | 500                                      | 2,500                          |
| Test Etme    | 2,000                                      | 500                                      | 2,500                          |
| Sözlük | 0                                          | 0                                        | Üst sınır yok                 |

## <a name="next-steps"></a>Sonraki adımlar

- Özel çevirmende bir [sözlüğü](what-is-dictionary.md) nasıl kullanacağınızı öğrenin.
