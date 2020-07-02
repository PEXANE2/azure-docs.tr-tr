---
title: Yapay zeka
description: LUSıS, tanımladığınız şemaya göre verilerinize yönelik dil anlama sağlamak için yapay zeka kullanır.
ms.topic: conceptual
ms.date: 06/29/2020
ms.openlocfilehash: 4f145585e097a3cf6a2338dbab879f6fce07f71d
ms.sourcegitcommit: a989fb89cc5172ddd825556e45359bac15893ab7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85802663"
---
# <a name="artificial-intelligence-in-language-understanding-luis"></a>Language Understanding (LUSıS) içinde yapay zeka

LUSıS, tanımladığınız şemaya göre verilerinize doğal dil anlama (NLU) sağlamak için yapay zeka kullanır.

## <a name="natural-language-processing"></a>Doğal dil işleme

Doğal Language Understanding (NLU), doğal dil Işleme (NLP) için belirli bir alt konudur.

Doğal dil işleme, metin verilerinin her türlü işlemeyi gerçekleştiren daha geniş bir kavramdır; bu, şöyle şeyler içerir:

* Simgeleştirme
* Konuşma (POS) etiketleme bölümü
* Kesimleme
* Morphological Analizi
* Anlamsal benzerlik
* Discourse
* Çeviri

## <a name="natural-language-processing-in-luis"></a>LUSıS 'de doğal dil işleme

Doğal dil işleme, LUSıS uygulamanız için aşağıdaki yollarla kullanılabilir:
* [Doğal dil anlama](#natural-language-understanding) (lusıs)
* LUŞU 'da yapılandırılabilir NLP yönleri:
    * [Simgeleştirme](luis-language-support.md#tokenization)
    * Morphology-Aksanlar, noktalama işaretleri ve Word Forms [API ayarları](luis-reference-application-settings.md)
* Şu gibi diğer bilişsel [Hizmetler](../Welcome.md) tarafından sunulan sorgu utterliğini ön veya sonrası işleme:
    * [İde](../translator/translator-info-overview.md)

## <a name="natural-language-understanding"></a>Doğal dil anlama

NLU bir dil bildirimini, kullanıcılarınızı doğal olarak anlamanıza olanak tanıyan bir gösterimle _dönüştürebilme_ yeteneğidir. Doğal dil anlama çok zorlayıcı bir sorun olduğunu ve bir _AI-Hard_ sorunu olarak tanımlanır.

LUSıS, amaç ve ayıklama için odaklanmak üzere tasarlanmıştır, bu da şunları belirleyebilmelidir:
* Kullanıcının istediği
* Ne konuşuyor?

LUSıS, örneklerde açık tanımlama olmadan semantik benzerlik gibi daha geniş _NLP_ yönlerini çok az veya hiç bilgisine sahip değil. Örneğin, aşağıdaki belirteçler (kelimeler), girilen örneklerde benzer bağlamlarda kullanılana kadar üç farklı nesnelerdir:

* satıcıdan
* almanın
* satın

## <a name="next-steps"></a>Sonraki adımlar

* Bir LUO uygulamasının [geliştirme yaşam döngüsü](luis-concept-app-iteration.md) hakkında bilgi edinin