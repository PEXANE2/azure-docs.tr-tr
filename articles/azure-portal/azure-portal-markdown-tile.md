---
title: Azure panoları üzerinde özel markaşağı kutucuğu kullanma
description: Statik içeriği göstermek için bir Azure panosuna markın kutucuğu ekleme hakkında bilgi edinin
services: azure-portal
keywords: ''
author: mblythe
ms.author: mblythe
ms.date: 01/08/2020
ms.topic: conceptual
ms.service: azure-portal
manager: mtillman
ms.openlocfilehash: 17d86b7c89ea5fb24c2adea22c5047c3e1ac3b6f
ms.sourcegitcommit: f53cd24ca41e878b411d7787bd8aa911da4bc4ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2020
ms.locfileid: "75832661"
---
# <a name="use-a-markdown-tile-on-azure-dashboards-to-show-custom-content"></a>Özel içerik göstermek için Azure panolarında markaşağı kutucuğu kullanın

Özel, statik içerik göstermek için Azure panolarınıza bir markı kutucuğu ekleyebilirsiniz. Örneğin, bir markı kutucuğunda temel yönergeleri, bir görüntüyü veya bir köprü kümesini gösterebilirsiniz.

## <a name="add-a-markdown-tile-to-your-dashboard"></a>Panonuza markın kutucuğu ekleme

1. Azure portal kenar çubuğundan **Pano** ' yı seçin.

   ![Portal kenar çubuğunu gösteren ekran görüntüsü](./media/azure-portal-markdown-tile/azure-portal-nav.png)

1. Herhangi bir özel Pano oluşturduysanız, pano görünümünde, özel markı kutucuğunun görünmesi gereken panoyu seçmek için açılan eklentiyi kullanın. **Kutucuk galerisini**açmak için Düzenle simgesini seçin.

   ![Pano düzenleme görünümünü gösteren ekran görüntüsü](./media/azure-portal-markdown-tile/azure-portal-dashboard-edit.png)

1. **Kutucuk galerisinde** **Marku** adlı kutucuğu bulun ve **Ekle**' yi seçin. Kutucuk panoya eklenir ve **Markaşağı Düzenle** bölmesi açılır.

1. Başka bir alana taşıdıktan sonra kutucukta görüntülenecek olan **başlık** ve alt **başlık**değerlerini girin.

   ![Başlık ve alt başlık girme sonuçlarını gösteren ekran görüntüsü](./media/azure-portal-markdown-tile/azure-portal-dashboard-enter-title.png)

1. Markaşağı içeriği dahil etme seçeneklerinden birini seçin: **satır içi Düzenle** veya **URL kullanarak içerik ekleme**.

   - Doğrudan markın girmek istiyorsanız **satır içi Düzenle** ' yi seçin.

      ![Satır içi içerik girmeyi gösteren ekran görüntüsü](./media/azure-portal-markdown-tile/azure-portal-dashboard-markdown-inline-content.png)

   - Çevrimiçi barındırılan mevcut markaşağı içeriğini kullanmak istiyorsanız **URL 'yi kullanarak Içerik Ekle** ' yi seçin.

      ![URL girmeyi gösteren ekran görüntüsü](./media/azure-portal-markdown-tile/azure-portal-dashboard-markdown-url.png)

      > [!NOTE]
      > Ek güvenlik için bir markaşağı dosyası oluşturabilir ve bu dosyayı [şifrelemenin etkin olduğu bir Azure Storage hesabı blobuna](../storage/common/storage-service-encryption.md)depolayıp URL seçeneğini kullanarak dosyayı işaret edebilirsiniz. Markaşağı içeriği, depolama hesabının şifreleme seçenekleri aracılığıyla şifrelenir. Yalnızca dosya izinlerine sahip kullanıcılar Pano üzerinde markaşağı içeriğini görebilir.

1. **Markı düzenleme** bölmesini kapatmak için **bitti** ' yi seçin. İçeriğiniz Markaşağı kutucuğunda görünür, bu da sağ alt köşedeki tutamacı sürükleyerek yeniden boyutlandırabilirsiniz.

   ![Özel markaşağı kutucuğunu gösteren ekran görüntüsü](./media/azure-portal-markdown-tile/azure-portal-custom-markdown-tile.png)

## <a name="markdown-content-capabilities-and-limitations"></a>Markaşağı içerik özellikleri ve sınırlamaları

Markaşağı kutucuğunda herhangi bir düz metin, markı sözdizimi ve HTML içeriği birleşimini kullanabilirsiniz. Azure portal, içeriğinizi kutucukta gösterilen HTML 'ye dönüştürmek için _işaretlenen_ adlı açık kaynaklı bir kitaplık kullanır. _İşaretlenen_ tarafından üretilen HTML, portal tarafından işlenmeden önce önceden işlenir. Bu adım, özelleştirmenin güvenlik veya portalın yerleşimini etkilememesini sağlamaya yardımcı olur. Bu ön işleme sırasında, olası bir tehdit oluşturan HTML 'nin herhangi bir bölümü kaldırılır. Portalın şu içerik türlerine izin verilmiyor:

* JavaScript – `<script>` Etiketler ve satır içi JavaScript değerlendirmeleri kaldırılır.
* iframe 'ler `<iframe>` Etiketler kaldırılacak.
* Stil `<style>` Etiketler kaldırılacak. HTML öğelerinde satır içi stil öznitelikleri resmi olarak desteklenmez. Bazı satır içi stil öğelerinin sizin için çalıştığını fark edebilirsiniz, ancak portalın düzenine müdahale ediyorsanız, çalışmayı durdurabilir. Markaşağı kutucuğu, portalın varsayılan stillerini kullanan temel ve statik içeriğe yöneliktir.

## <a name="next-steps"></a>Sonraki adımlar

* Özel bir pano oluşturmak için, bkz [. Azure Portal panoları oluşturma ve paylaşma](../azure-portal/azure-portal-dashboards.md)
