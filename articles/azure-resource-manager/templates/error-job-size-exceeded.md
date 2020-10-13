---
title: İş boyutu aşıldı hatası
description: İş boyutu veya şablon çok büyük olduğunda hata giderme işlemlerinin nasıl yapılacağını açıklar.
ms.topic: troubleshooting
ms.date: 10/07/2020
ms.openlocfilehash: 638bdef246fc908ab997bfb99e7526febdb3792e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91822148"
---
# <a name="resolve-errors-for-job-size-exceeded"></a>İş boyutu için hataları çözümleme aşıldı

Bu makalede, **jobsizeexceededexception** ve **deploymentsizeexceededexception** hatalarının nasıl çözümleneceği açıklanır.

## <a name="symptom"></a>Belirti

Bir şablonu dağıttığınızda, dağıtımın sınırları aştığını belirten bir hata alırsınız.

## <a name="cause"></a>Nedeni

Şablonunuzun boyutu 4 MB 'ı aştığında bu hatayı alabilirsiniz. 4 MB sınırı, çok sayıda örnek oluşturmak için [kopyayı](copy-resources.md) kullanan kaynak tanımları için genişletildikten sonra şablonun son durumuna uygulanır. Son durum ayrıca değişkenler ve parametreler için çözümlenen değerleri de içerir.

Dağıtım işi, istek hakkındaki meta verileri de içerir. Büyük şablonlar için, şablonla birleştirilmiş meta veriler, bir iş için izin verilen boyutu aşabilir.

Şablon için diğer sınırlar şunlardır:

* 256 parametreleri
* 256 değişkenleri
* 800 kaynak (kopya sayısı dahil)
* 64 çıkış değeri
* Şablon ifadesinde 24.576 karakter

## <a name="solution-1---simplify-template"></a>Çözüm 1-şablonu basitleştirme

İlk seçeneğiniz, şablonu basitleştirmektir. Bu seçenek, şablonunuz çok sayıda farklı kaynak türü dağıttığında işe yarar. Şablonu [bağlantılı şablonlara](linked-templates.md)bölüye göz önünde bulundurun. Kaynak türlerinizi mantıksal gruplara bölün ve her grup için bir bağlı şablon ekleyin. Örneğin, çok sayıda ağ kaynağı dağıtmanız gerekiyorsa, bu kaynakları bağlı bir şablona taşıyabilirsiniz.

Bağlı şablona bağlı olarak diğer kaynakları ayarlayabilir ve [bağlı şablonun çıktısından değerler alabilirsiniz](linked-templates.md#get-values-from-linked-template).

## <a name="solution-2---reduce-name-size"></a>Çözüm 2-ad boyutunu azalt

[Parametreler](template-parameters.md), [değişkenler](template-variables.md)ve [çıktılar](template-outputs.md)için kullandığınız adların uzunluğunu kısaltmak için deneyin. Bu değerler kopyalama döngüleriyle yinelendiğinde, büyük bir ad birçok kez çarpılarak çarpılır.

## <a name="solution-3---use-serial-copy"></a>Çözüm 3-seri kopyalama kullan

İkinci seçeneğiniz, kopyalama döngünüzü [paralel ve seri işlemeye](copy-resources.md#serial-or-parallel)değiştirmek. Bu seçeneği yalnızca hatanın, kopyalama yoluyla çok sayıda kaynak dağıtmaktan şüphelendiğiniz durumlarda kullanın. Bu değişiklik, kaynaklar paralel olarak dağıtılmadığından dağıtım süresini önemli ölçüde artırabilir.
