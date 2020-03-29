---
title: Azure panolarında özel işaretleme döşemesi kullanma
description: Statik içeriği görüntülemek için Azure panosuna işaretleme döşemesi eklemeyi öğrenin
services: azure-portal
keywords: ''
author: mgblythe
ms.author: mblythe
ms.date: 01/08/2020
ms.topic: conceptual
ms.service: azure-portal
manager: mtillman
ms.openlocfilehash: 74102423461a56bb6fc19c2eb9874f96a76e34e0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76310722"
---
# <a name="use-a-markdown-tile-on-azure-dashboards-to-show-custom-content"></a>Azure panolarında özel içerik göstermek için işaretleme döşemesi kullanma

Azure panolarınıza özel, statik içerik görüntülemek için bir işaretleme döşemesi ekleyebilirsiniz. Örneğin, bir işaretleme döşemesi üzerinde temel yönergeleri, görüntüyü veya bir köprü kümesini gösterebilirsiniz.

## <a name="add-a-markdown-tile-to-your-dashboard"></a>Panonuza işaretleme döşemesi ekleme

1. Azure portal kenar çubuğundan **Pano'yu** seçin.

   ![Portal kenar çubuğugösteren ekran görüntüsü](./media/azure-portal-markdown-tile/azure-portal-nav.png)

1. Pano görünümünde özel panolar oluşturduysanız, özel işaretleme döşemesinin görünmesi gereken panoyu seçmek için açılır paneli kullanın. **Döşeme Galerisi'ni**açmak için edit simgesini seçin.

   ![Pano edit görünümünü gösteren ekran görüntüsü](./media/azure-portal-markdown-tile/azure-portal-dashboard-edit.png)

1. Döşeme **Galerisi'nde,** **Markdown** adlı döşemeyi bulun ve **Ekle'yi**seçin. Döşeme panoya eklenir ve **İşaretlet** bölmesi edin açılır.

1. Başka bir alana geçtikten sonra döşemede görüntülenecek **Başlık** ve **Altyazı**için değerleri girin.

   ![Başlık ve altyazı girme sonuçlarını gösteren ekran görüntüsü](./media/azure-portal-markdown-tile/azure-portal-dashboard-enter-title.png)

1. Markdown içeriğini dahil etmek için seçeneklerden birini seçin: URL'yi kullanarak **satır içinde düzenleme** veya **Insert içeriği.**

   - Doğrudan işaretleme girmek istiyorsanız **Satır Çizgisi düzenlemesini** seçin.

      ![Satır içinde içerik girerken gösteren ekran görüntüsü](./media/azure-portal-markdown-tile/azure-portal-dashboard-markdown-inline-content.png)

   - Çevrimiçi olarak barındırılan mevcut işaretleme içeriğini kullanmak istiyorsanız **URL'yi kullanarak İçerik Ekle'yi** seçin.

      ![URL giriyi gösteren ekran görüntüsü](./media/azure-portal-markdown-tile/azure-portal-dashboard-markdown-url.png)

      > [!NOTE]
      > Daha fazla güvenlik için, bir işaretleme dosyası oluşturabilir ve şifrelemenin etkin olduğu bir [Azure depolama hesabı blob'unda](../storage/common/storage-service-encryption.md)saklayabilir , ardından URL seçeneğini kullanarak dosyayı işaret edebilirsiniz. İşaretiçeriği, depolama hesabının şifreleme seçenekleri aracılığıyla şifrelenir. Yalnızca dosya izni olan kullanıcılar panodaki işaretleme içeriğini görebilir.

1. **İşaretlet'i Edit** bölmesini kapatmak için **Bitti'yi** seçin. İçeriğiniz, sağ alt köşedeki tutamacı sürükleyerek yeniden boyutlandırabileceğiniz İşaretleişareti döşemesinde görünür.

   ![Özel işaretleme döşemesi gösteren ekran görüntüsü](./media/azure-portal-markdown-tile/azure-portal-custom-markdown-tile.png)

## <a name="markdown-content-capabilities-and-limitations"></a>Markdown içerik yetenekleri ve sınırlamaları

İşaretüstü döşemede düz metin, Markdown sözdizimi ve HTML içeriğinin herhangi bir birleşimini kullanabilirsiniz. Azure portalı, içeriğinizi döşemede gösterilen HTML'ye dönüştürmek için _işaretli_ bir açık kaynak kitaplığı kullanır. _İşaretli_ tarafından üretilen HTML, oluşturulmadan önce portal tarafından önceden işlenir. Bu adım, özelleştirmenizin portalın güvenliğini veya düzenini etkilemeyeceğinden emin olun. Bu ön işleme sırasında, HTML'nin olası bir tehdit oluşturan herhangi bir bölümü kaldırılır. Aşağıdaki içerik türlerine portal tarafından izin verilmez:

* JavaScript `<script>` – etiketleri ve satır satırlı JavaScript değerlendirmeleri kaldırılır.
* iframes `<iframe>` - etiketleri kaldırılacak.
* Stil `<style>` - etiketleri kaldırılır. HTML öğelerindeki satır satır stili öznitelikleri resmi olarak desteklenmez. Bazı satır lı stil öğelerinin sizin için çalıştığını görebilirsiniz, ancak portalın düzenini bozarlarsa, istedikleri zaman çalışmayı durdurabilirler. İşaretaşağı döşemesi, portalın varsayılan stillerini kullanan temel, statik içeriğe yöneliktir.

## <a name="next-steps"></a>Sonraki adımlar

* Özel bir pano oluşturmak için Azure [portalında pano oluştur ve paylaş'a](../azure-portal/azure-portal-dashboards.md) bakın
