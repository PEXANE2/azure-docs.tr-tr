---
title: İş boyutu aşıldı hatası
description: İş boyutu veya şablon çok büyük olduğunda hata giderme işlemlerinin nasıl yapılacağını açıklar.
ms.topic: troubleshooting
ms.date: 09/25/2020
ms.openlocfilehash: 06645561964d9634d93061b3be4d100a578cc7e7
ms.sourcegitcommit: 5dbea4631b46d9dde345f14a9b601d980df84897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91373196"
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

## <a name="solution-2---use-serial-copy"></a>Çözüm 2-seri kopyalama kullanma

İkinci seçeneğiniz, kopyalama döngünüzü [paralel ve seri işlemeye](copy-resources.md#serial-or-parallel)değiştirmek. Bu seçeneği yalnızca hatanın, kopyalama yoluyla çok sayıda kaynak dağıtmaktan şüphelendiğiniz durumlarda kullanın. Bu değişiklik, kaynaklar paralel olarak dağıtılmadığından dağıtım süresini önemli ölçüde artırabilir.
