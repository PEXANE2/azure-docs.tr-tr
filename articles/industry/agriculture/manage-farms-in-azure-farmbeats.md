---
title: Çiftlikleri Yönet
description: Çiftliklerin nasıl yönetilenini açıklar
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: 09144c4c35ab911b60931849807123608f2c3cdd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79271726"
---
# <a name="manage-farms"></a>Grupları yönetme

Çiftliklerinizi Azure FarmBeats'te yönetebilirsiniz. Bu makalede, çiftliklerinizi yönetmenize yardımcı olacak çiftlikler, aygıtlar, sensörler ve drone'lar nasıl oluşturulabileceğiniz hakkında bilgiler verilmektedir.

## <a name="create-farms"></a>Çiftlikler oluşturma

Aşağıdaki adımları kullanın:

1. Çiftlik Hızlandırıcı'ya giriş yapın, **Çiftlikler** sayfası görüntülenir.
    **Çiftlikler** sayfası, abonelikte zaten oluşturulmuş olmaları durumunda çiftliklerin listesini görüntüler.

    İşte örnek görüntü:

    ![Proje Çiftlik Beats](./media/create-farms-in-azure-farmbeats/create-farm-main-page-1.png)


2. **Çiftlik Oluştur'u** seçin ve **Ad,** **Ekinler** ve **Adres**sağlayın.
3. Farm **Boundary'i tanımla**(zorunlu alan) **Haritada İşaretle** veya **GeoJSON kodunu yapıştır'ı**seçin.

Bir çiftlik sınırını tanımlamanın iki yolu şunlardır:

1. **Haritadaki İşaret**: Çiftliğin sınırını çizmek ve işaretlemek için harita kontrol aracını kullanın. Sınırları işaretlemek için, ![Project](./media/create-farms-in-azure-farmbeats/pencil-icon-1.png) Farm Beats ve tam sınırları işaretlemek için.

    ![Proje Çiftlik Beats](./media/create-farms-in-azure-farmbeats/create-farm-mark-on-map-1.png)

2. **Paste GeoJson Kodu**: GeoJSON, JavaScript Object Notation (JSON) kullanarak coğrafi veri yapılarını kodlamabiçimidir. Bu seçenek, farm sınırlarını işaretlemek için bir GeoJSON dizesi girilebilen bir metin kutusu görüntüler. Ayrıca GeoJSON.io geoJSON kodu oluşturabilirsiniz.
Bilgileri doldurmaya yardımcı olmak için araç ipuçlarını kullanın.

    ![Proje Çiftlik Beats](./media/create-farms-in-azure-farmbeats/create-new-farm-1.png)

3.  Çiftlik oluşturmak için **Gönder'i** seçin. Yeni bir çiftlik oluşturulur ve **Çiftlikler** sayfasında görüntülenir.

## <a name="view-farm"></a>Çiftliği görüntüle

Çiftlik listesi sayfası oluşturulan çiftliklerin listesini görüntüler. Listesini görüntülemek için bir çiftlik seçin:

 - **Aygıt sayısı** — çiftlikte dağıtılan aygıtların sayısını ve durumunu görüntüler.
 - **Harita** - çiftlikte dağıtılan cihazlar ile çiftlik haritası.
 - **Telemetri** — çiftlikte konuşlandırılan sensörlerden telemetri görüntüler.
 - **En son Hassas Haritalar** - en son Uydu Endeksleri haritasını (EVI, NDWI), Toprak Nem Isı Haritası ve Sensör Yerleştirme haritasını görüntüler.

## <a name="edit-farm"></a>Çiftliği edin

**Çiftlikler** sayfası oluşturulan çiftliklerin listesini görüntüler.

1.  Çiftliği görüntülemek ve görüntülemek için bir çiftlik seçin.
2.  Çiftlik bilgilerini deletmek için **Çiftliği Edit'i** seçin. Çiftlik **Ayrıntıları** penceresinde, **Ad,** **Ekinler,** **Adres**ve **Çiftlik Sınırı** alanlarını atlayabilirsiniz.

    ![Proje Çiftlik Beats](./media/create-farms-in-azure-farmbeats/edit-farm-1.png)

3. Düzenlenen ayrıntıları kaydetmek için **Gönder'i** seçin.

## <a name="delete-farm"></a>Çiftliği silme

**Çiftlikler** sayfası oluşturulan çiftliklerin listesini görüntüler. Bir çiftliği silmek için aşağıdaki adımları kullanın:

1.  Çiftlik ayrıntılarını silmek için listeden bir çiftlik seçin.
2.  Çiftliği silmek için **Çiftliği Sil'i** seçin.

    ![Proje Çiftlik Beats](./media/create-farms-in-azure-farmbeats/delete-farm-1.png)

    > [!NOTE]
    > Bir çiftliği sildiğinizde, çiftlikle ilişkili aygıtlar ve haritalar silinmez. Aygıt ve haritalarla ilişkili herhangi bir çiftlik bilgisi ilgili olmayacaktır. FarmBeats hizmetinden cihazları, telemetrileri ve haritaları görüntülemeye devam edebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

Artık çiftliğinizi oluşturduğunuza göre, [sensör verilerinin](get-sensor-data-from-sensor-partner.md) çiftliğinize nasıl aktığını öğrenin.
