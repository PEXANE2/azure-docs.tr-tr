---
title: Azure haritalar Creator 'da ınkapılı haritalar ile çalışma
description: Bu makalede, Azure Maps Creator Hizmetleri için uygulanan kavramlar açıklanır.
author: anastasia-ms
ms.author: v-stharr
ms.date: 05/18/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 34a1495d1a14e35bc5a94bfc01f4034c6fd6de72
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83598398"
---
# <a name="creator-for-indoor-maps"></a>Inkapı haritaları için Oluşturucu

Bu makalede, Azure haritalar Oluşturucusu için uygulanan kavramlar ve araçlar açıklanır. Azure haritalar Oluşturucu API 'SI ve SDK 'Yı kullanmaya başlamadan önce bu makaleyi okumanızı öneririz.

Inkapımap verilerine dayalı harita özellikleriyle uygulama geliştirmek için oluşturucuyu kullanabilirsiniz. Bu makalede harita verilerinizi yükleme, dönüştürme, oluşturma ve kullanma işlemi açıklanmaktadır. Tüm iş akışı aşağıdaki diyagramda gösterilmiştir.

![Oluşturucu eşleme verileri iş akışı](./media/creator-indoor-maps/workflow.png)

## <a name="create-azure-maps-creator"></a>Azure haritalar Oluşturucu oluşturma

Creator hizmetlerini kullanmak için Azure haritalar Oluşturucu bir Azure Maps hesabında oluşturulmalıdır. Azure haritalar 'da Azure Maps Creator oluşturma hakkında bilgi için bkz. [Azure Maps Creator 'U yönetme](how-to-manage-creator.md).

## <a name="upload-a-drawing-package"></a>Bir çizim paketini karşıya yükle

Oluşturucu, karşıya yüklenen bir çizim paketini dönüştürerek ınkapısı eşleme verilerini toplar. Çizim paketi oluşturulmuş veya uzaktan kaldırma özelliğini temsil eder. Paket gereksinimlerini çizme hakkında daha fazla bilgi için bkz. [Çizim paketi gereksinimleri](drawing-requirements.md).

Bir çizim paketini karşıya yüklemek için [Azure Maps verilerini karşıya yükleme API](https://docs.microsoft.com/rest/api/maps/data/uploadpreview) 'sini kullanın.  Karşıya yükleme başarılı olduğunda, veri yükleme API 'SI bir kullanıcı veri tanımlayıcısı () döndürür `udid` . Bir `udid` sonraki adımda, karşıya yüklenen paketi ınkapımap verilerine dönüştürmek için kullanılacaktır.

## <a name="convert-a-drawing-package"></a>Bir çizim paketini dönüştürme

[Azure haritalar dönüştürme hizmeti](https://docs.microsoft.com/rest/api/maps/data/conversion) karşıya yüklenen bir çizim paketini ınkapılı harita verilerine dönüştürür. Dönüştürme hizmeti Ayrıca paketi de doğrular. Doğrulama sorunları iki türe sınıflandırılır: hatalar ve uyarılar. Herhangi bir hata algılanırsa, dönüştürme işlemi başarısız olur. Uyarı algılanmalı, dönüştürme başarılı olur. Ancak, tüm uyarıları gözden geçirmeniz ve çözmeniz önerilir. Uyarı, dönüştürmenin bir kısmının yoksayıldığı veya otomatik olarak düzeltilmediği anlamına gelir. Uyarıları çözememesi, ikinci süreçlerdeki hatalara neden olabilir. Daha fazla bilgi için bkz. [paket uyarılarını ve hatalarını çizme](drawing-conversion-error-codes.md).

Bir hata oluştuğunda, dönüştürme hizmeti [Azure Maps çizim hatası görselleştiricisi](drawing-error-visualizer.md) tek başına Web uygulamasına bir bağlantı sağlar. Çizim [paketi uyarılarını ve](drawing-conversion-error-codes.md) dönüştürme işlemi sırasında oluşan hataları Incelemek Için çizim hatası görselleştiricisi ' nı kullanabilirsiniz. Hataları düzelttikten sonra, paketi karşıya yüklemeyi ve dönüştürmeyi deneyebilirsiniz.

## <a name="create-indoor-map-data"></a>Inkapılı harita verileri oluşturma

Azure haritalar Oluşturucu üç hizmet sağlar:

* [Veri kümesi hizmeti](https://docs.microsoft.com/rest/api/maps/dataset/createpreview).
Dönüştürülmüş bir çizim paketi verilerinden bir veri kümesi oluşturmak için veri kümesi hizmetini kullanın.
* [Tileset hizmeti](https://docs.microsoft.com/rest/api/maps/tileset/createpreview).
Bir veri kümesinin vektör tabanlı temsilini oluşturmak için tileset hizmetini kullanın. Uygulamalar, veri kümesinin görsel kutucuk tabanlı görünümünü sunmak için bir tileset kullanabilir.
* [Özellik durumu hizmeti](https://docs.microsoft.com/rest/api/maps/featurestate). Dinamik harita stillendirilmesini desteklemek için özellik durumu hizmetini kullanın. Dinamik harita stili, uygulamaların IoT sistemi tarafından sunulan alanlara gerçek zamanlı olayları yansıtmaları sağlar.

### <a name="datasets"></a>Veri kümeleri

Veri kümesi, bir ınkapımap özellikleri koleksiyonudur. Inkapımap özellikleri, dönüştürülmüş bir çizim paketinde tanımlanan olanakları temsil eder. [Veri kümesi hizmeti](https://docs.microsoft.com/rest/api/maps/dataset/createpreview)ile bir veri kümesi oluşturduktan sonra, istediğiniz sayıda [tilesets](#tilesets) veya [özellik statesets](#feature-statesets)oluşturabilirsiniz.

[Veri kümesi hizmeti](https://docs.microsoft.com/rest/api/maps/dataset/createpreview) , geliştiricilerin mevcut bir veri kümesine tesis ekleme veya kaldırma olanağı sağlar. API kullanarak var olan bir veri kümesini güncelleştirme hakkında daha fazla bilgi için bkz. [DataSet Service](https://docs.microsoft.com/rest/api/maps/dataset/createpreview)içindeki ekleme seçenekleri. Bir veri kümesini güncelleştirme hakkında bir örnek için bkz. [veri Bakımı](#data-maintenance).

### <a name="tilesets"></a>Tilesets 'ler

Bir tileset, bir Tekdüzen kılavuz kutucukları kümesini temsil eden bir vektör verileri koleksiyonudur. Geliştiriciler bir veri kümesinden tilesets oluşturmak için [tileset hizmetini](https://docs.microsoft.com/rest/api/maps/tileset/createpreview) kullanabilir.

Farklı içerik aşamalarını yansıtmak için aynı veri kümesinden birden çok tilesets oluşturabilirsiniz. Örneğin, mobilya ve ekipmanla bir tileset, mobilya ve ekipman olmadan başka bir döşeme kümesi de yapabilirsiniz.  En son veri güncelleştirmeleriyle tek bir tilekümesi oluşturmayı seçebilirsiniz ve bunlardan en son veri güncelleştirmeleri olmadan bir tane oluşturabilirsiniz.

Vektör verilerine ek olarak, tileset, harita işleme iyileştirmesi için meta veriler sağlar. Örneğin, tileset meta verileri, tileset için bir min ve Max yakınlaştırma düzeyi içerir. Meta veriler ayrıca, tileset 'in coğrafi kapsamını tanımlayan bir sınırlayıcı kutu sağlar. Sınırlayıcı kutu, bir uygulamanın programlama yoluyla doğru merkez noktasını ayarlamasına olanak tanır. Tileset meta verileri hakkında daha fazla bilgi için bkz. [tileset LIST API](https://docs.microsoft.com/rest/api/maps/tileset/listpreview).

Bir tileset oluşturulduktan sonra, [oluşturma v2 hizmeti](#render-v2-service)tarafından alınabilir.

Bir tileset tarihi geçmiş hale gelirse ve artık yararlı değilse, tileset 'i silebilirsiniz. Tilesets 'in nasıl silineceği hakkında daha fazla bilgi için bkz. [veri Bakımı](#data-maintenance).

>[!NOTE]
>Bir tileset oluşturulduğu veri kümesinden bağımsızdır. Bir veri kümesinden tilesets 'ler oluşturup daha sonra bu veri kümesini güncelleştirirseniz, tilesets güncellenmeyecektir. Bir veri kümesindeki değişiklikleri yansıtmak için yeni bir tilesets oluşturmanız gerekir. Benzer şekilde, bir tileset silerseniz, veri kümesi etkilenmeyecektir.

### <a name="feature-statesets"></a>Özellik statesets 'ler

Özellik statesets 'ler, odalar veya ekipman gibi veri kümesi özelliklerine atanan dinamik Özellikler (*eyaletler*) koleksiyonlarıdır. Bir *durum* örneği sıcaklık veya doluluk olabilir. Her *durum* , özelliğin adını, değerini ve son güncelleştirmenin zaman damgasını içeren bir anahtar/değer çiftidir.

[Özellik durumu hizmeti](https://docs.microsoft.com/rest/api/maps/featurestate/createstatesetpreview) , bir veri kümesi için bir özellik stateset oluşturmanıza ve yönetmenize olanak sağlar. Stateset bir veya daha fazla *durum*tarafından tanımlanır. Oda gibi her bir özelliğin, kendisine bağlı bir *durum* olabilir.

Bir stateset içindeki her *durum* değeri IoT cihazları veya diğer uygulamalar tarafından güncelleştirilebilen veya alınabilir.  Örneğin, [özellik durumu GÜNCELLEŞTIRME API](https://docs.microsoft.com/rest/api/maps/featurestate/updatestatespreview)'sini kullanarak, alan doluluk ölçen cihazlar, bir odanın durum değişikliğini sistematik olarak gönderebilir.

Bir uygulama, bir tesis içindeki özellikleri geçerli durumlarına ve ilgili harita stiline göre dinamik olarak işlemek için bir özellik stateset kullanabilir. Özellik statesets 'i bir işleme eşlemesindeki stil özelliklerine kullanma hakkında daha fazla bilgi için bkz. [ınkapılı Web SDK modülü](#indoor-maps-module).

>[!NOTE]
>Tilesets gibi, bir veri kümesinin değiştirilmesi var olan durum kümesini etkilemez ve bir özellik stateset 'in eklendiği veri kümesi üzerinde hiçbir etkisi olmayacaktır.

## <a name="using-indoor-maps"></a>Inkapıharitaları kullanma

### <a name="render-v2-service"></a>Oluşturma v2 hizmeti

Azure haritalar [Işleme v2 hizmeti-Get Map kutucuğu API 'si](https://docs.microsoft.com/rest/api/maps/renderv2/getmaptilepreview) , Oluşturucu tilesets 'leri destekleyecek şekilde genişletildi.

[Oluşturma v2 hizmeti-harita durum kutucuğu API 'si](https://docs.microsoft.com/rest/api/maps/renderv2/getmaptilepreview) , uygulamaların tilesets istemesine izin verir. Daha sonra, tilesets 'ler bir harita denetimiyle veya SDK ile tümleştirilebilirler. Oluşturma v2 hizmetini kullanan bir harita denetimi örneği için bkz. [ınkapıeşlem modülü](#indoor-maps-module).

### <a name="web-feature-service-api"></a>Web özelliği hizmet API 'SI

Veri kümeleri, [Web özelliği hizmeti (WFS) API 'si](https://docs.microsoft.com/rest/api/maps/wfs)kullanılarak sorgulanabilir. WFS, [Open GEOSPATIAL CONSORTIUM API özelliklerini](http://docs.opengeospatial.org/DRAFTS/17-069r1.html)izler. WFS API 'SI, veri kümesinin içindeki özellikleri sorgulamanızı sağlar. Örneğin, belirli bir tesis ve kat düzeyi için tüm orta ölçekli toplantı odalarını bulmak üzere WFS 'yi kullanabilirsiniz.

### <a name="indoor-maps-module"></a>Inkapıharitaları modülü

[Azure Haritalar Web SDK 'sı](https://docs.microsoft.com/azure/azure-maps/) , ınkapıharitaları modülünü içerir. Bu modül, Azure Maps *harita denetimi* kitaplığı 'na genişletilmiş işlevler sunar. Inkapıeşlem modülü Oluşturucu 'da oluşturulan ınkapılı haritaları işler. , Kullanıcıların farklı katları görselleştirmesine yardımcı olan *zemin Seçicisi*gibi pencere öğelerini tümleştirir.

Inkapıharitaları modülü, ınkapısı eşleme verilerini diğer [Azure haritalar hizmetleriyle](https://docs.microsoft.com/azure/azure-maps/)tümleştiren Web uygulamaları oluşturmanıza olanak tanır. En yaygın uygulama kurulumları, yol, Imagery, hava durumu ve aktarım gibi diğer haritalardan gelen kapılara bilgi eklemeyi içerebilir.

Inkapıharitaları modülü ayrıca dinamik harita stilini de destekler. Bir uygulamada Özellik stateset dinamik stilini nasıl uygulayacağınızı gösteren adım adım yönergeler için bkz [. ınkapısı eşleme modülünü kullanma](how-to-use-indoor-module.md)

### <a name="azure-maps-integration"></a>Azure haritalar tümleştirmesi

Inkapıeşlemler için çözümler geliştirmeye başladığınızda, mevcut Azure Maps yeteneklerini tümleştirme yollarını bulabilirsiniz. Örneğin, varlık izleme veya güvenlik senaryoları, Oluşturucu ınkapaklı haritaları ile [Azure Maps geofence API 'si](https://docs.microsoft.com/rest/api/maps/spatial/postgeofence) kullanılarak uygulanabilir. Bölge, bir çalışanın belirli bir ınkapılı alanları girip girmediğini veya ayrılmadığını anlamak için kullanılabilir. Azure haritalar 'ı IoT telemetrisi ile bağlama hakkında daha fazla bilgi için [burada](tutorial-iot-hub-maps.md)bulunabilir.

### <a name="data-maintenance"></a>Veri Bakımı

 Azure haritalar Oluşturucu listesi, güncelleştirme ve silme API 'SI, veri kümelerini, tilekümelerinizi ve özellik statekümelerinizi listeledikten, güncelleştirmenize ve silmenizi sağlar.

>[!NOTE]
>Öğelerin bir listesini gözden geçirdikten ve bunları silmeye karar verirken, bu silmenin tüm bağımlı API veya uygulamalar üzerinde etkisini göz önünde bulundurmanız gerekir. Örneğin, [oluşturma v2-Get harita kutucuğu API 'si](https://docs.microsoft.com/rest/api/maps/renderv2/getmaptilepreview)aracılığıyla bir uygulama tarafından kullanılmakta olan bir kutucuk kümesini silmeniz gerekiyorsa, bu tileset 'in silinmesi, bir uygulama başarısızlığının bu kutucuk kümesini işlemesine neden olur.

### <a name="example-updating-a-dataset"></a>Örnek: bir veri kümesini güncelleştirme

Aşağıdaki örnek, bir veri kümesini güncelleştirme, yeni bir tileset oluşturma ve eski bir tileset 'i silme işlemlerinin nasıl yapılacağını gösterir.

1. Yeni çizim paketini karşıya yüklemek ve dönüştürmek için [bir çizim paketini karşıya yükleme](#upload-a-drawing-package) ve [bir çizim paketi bölümünü dönüştürme](#convert-a-drawing-package) bölümündeki adımları izleyin.

2. Dönüştürülen verileri varolan veri kümesine eklemek için [veri kümesi oluşturma API](https://docs.microsoft.com/rest/api/maps/dataset/createpreview) 'sini kullanın.

3. Güncelleştirilmiş veri kümesinden yeni bir tileset oluşturmak için [tileset Create API](https://docs.microsoft.com/rest/api/maps/tileset/createpreview) 'sini kullanın. 4. adım için yeni Tilesetıd 'yi kaydedin.

4. Güncelleştirilmiş kampüs veri kümesinin görselleştirmesini etkinleştirmek için uygulamanızdaki tileset tanımlayıcısını güncelleştirin. Eski tileset artık kullanımda değilse, bunu silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: bir Oluşturucu iç eşleme oluşturma](tutorial-creator-indoor-maps.md)
