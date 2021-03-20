---
title: İş boyutu aşıldı hatası
description: İş boyutu veya şablon çok büyük olduğunda hata giderme işlemlerinin nasıl yapılacağını açıklar.
ms.topic: troubleshooting
ms.date: 01/19/2021
ms.openlocfilehash: 1fde4918aff6e3bf494876f83c5b4313b3c5f3d2
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "98610412"
---
# <a name="resolve-errors-for-job-size-exceeded"></a>İş boyutu için hataları çözümleme aşıldı

Bu makalede, **jobsizeexceededexception** ve **deploymentjobsizeexceededexception** hatalarının nasıl çözümleneceği açıklanır.

## <a name="symptom"></a>Belirti

Bir şablonu dağıttığınızda, dağıtımın sınırları aştığını belirten bir hata alırsınız.

## <a name="cause"></a>Nedeni

Dağıtım izin verilen limitlerden birini aştığında bu hatayı alırsınız. Genellikle, şablonunuz veya dağıtımı çalıştıran iş çok büyük olduğunda bu hatayı görürsünüz.

Dağıtım işi 1 MB 'ı aşamaz. İş, istek hakkındaki meta verileri içerir. Büyük şablonlar söz konusu olduğunda, şablonla birlikte meta veriler iş için izin verilen boyutu aşabilir.


Şablon 4 MB'ı aşamaz. 4 MB sınırı, çok sayıda örnek oluşturmak için [kopyayı](copy-resources.md) kullanan kaynak tanımları için genişletildikten sonra şablonun son durumuna uygulanır. Son durum değişkenler ve parametreler için çözümlenen değerleri de içerir.

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

Kopyalama döngünüzü [paralel olarak seri işleme olarak](copy-resources.md#serial-or-parallel)değiştirmeyi düşünün. Bu seçeneği yalnızca hatanın, kopyalama yoluyla çok sayıda kaynak dağıtmaktan şüphelendiğiniz durumlarda kullanın. Bu değişiklik, kaynaklar paralel olarak dağıtılmadığından dağıtım süresini önemli ölçüde artırabilir.
