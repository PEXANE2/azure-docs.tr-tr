---
title: Import-Soru-Cevap Oluşturma için desteklenen URL türleri
description: URL türlerinin, QnA kümelerini içeri ve dışarı aktarmak için nasıl kullanılacağını anlayın.
ms.topic: reference
ms.date: 01/02/2020
ms.openlocfilehash: cf2a02f55d7c95ff9b3f0231ea07a783d18b73cf
ms.sourcegitcommit: 96dc60c7eb4f210cacc78de88c9527f302f141a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77651833"
---
# <a name="urls-supported-for-importing-documents"></a>Belgeleri içeri aktarmak için desteklenen URL 'Ler

URL türlerinin, QnA kümelerini içeri ve dışarı aktarmak için nasıl kullanılacağını anlayın.

## <a name="faq-urls"></a>SSS URL'leri

Soru-Cevap Oluşturma, 3 farklı formda SSS Web sayfalarını destekleyebilir:

* Düz SSS sayfaları
* Bağlantılar hakkında SSS sayfaları
* Konular giriş sayfası ile SSS sayfaları

### <a name="plain-faq-pages"></a>Düz SSS sayfaları

Bu SSS sayfası, yanıtları aynı sayfada sorular hemen izleyin, en yaygın türüdür.

Düz bir SSS sayfasının bir örneği aşağıda verilmiştir:

![Bilgi Bankası için düz SSS sayfası örneği](./media/qnamaker-concepts-datasources/plain-faq.png)


### <a name="faq-pages-with-links"></a>Bağlantılar hakkında SSS sayfaları

Bu tür bir SSS sayfasını, sorular birlikte toplanır ve aynı sayfa farklı bölümlerde veya farklı sayfalardaki olan yanıtları bağlanır.

SSS sayfası aynı sayfada bölümlerde bağlantılarla birlikte bir örnek aşağıda verilmiştir:

 ![Bilgi Bankası için bölüm bağlantı SSS sayfası örneği](./media/qnamaker-concepts-datasources/sectionlink-faq.png)


### <a name="parent-topics-page-links-to-child-answers-pages"></a>Üst konular sayfası alt yanıt sayfalarına bağlantılar

Bu tür bir SSS, her konunun farklı bir sayfada ilgili soru ve yanıt kümesiyle bağlantılı olduğu konular sayfasına sahiptir. Soru-Cevap Oluşturma, karşılık gelen soruların & yanıtları ayıklamak için tüm bağlantılı sayfalara gezinir.

Aşağıda, farklı sayfalardaki SSS bölümlerinin bağlantılarıyla ilgili konular sayfasına bir örnek verilmiştir.

 ![Bilgi Bankası için ayrıntılı bağlantı SSS sayfası örneği](./media/qnamaker-concepts-datasources/topics-faq.png)

## <a name="support-urls"></a>Destek URL 'Leri

Soru-Cevap Oluşturma, belirli bir görevin nasıl gerçekleştirileceğini, belirli bir sorunun nasıl tanılanacağı ve çözümleneceğini ve belirli bir işlem için en iyi uygulamaların hangileri olduğunu betimleyen web makaleleri gibi yarı yapılandırılmış destek Web sayfalarını işleyebilir. Ayıklama, hiyerarşik başlıkları olan açık bir yapıya sahip içerikte en iyi şekilde kullanılır.

> [!NOTE]
> Destek makaleleri için ayıklama yeni bir özelliktir ve erken aşamalarındayken. İyi yapılandırılmış olan ve karmaşık üstbilgiler/altbilgiler içermeyen basit sayfalar için en iyi şekilde kullanılır.

![Soru-Cevap Oluşturma, açık bir yapının hiyerarşik başlıklarla sunulduğu yarı yapılandırılmış Web sayfalarından ayıklanmasını destekler](./media/qnamaker-concepts-datasources/support-web-pages-with-heirarchical-structure.png)