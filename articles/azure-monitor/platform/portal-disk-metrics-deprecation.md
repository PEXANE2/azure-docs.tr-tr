---
title: Azure portalında disk ölçümleri amortismanı | Microsoft Dokümanlar
description: Yeni ölçümleri kullanmak için hangi disk ölçümlerinin devre deprecated olduğunu ve metrik uyarılarınızı nasıl güncelleştirtini öğrenin.
services: azure-monitor
ms.subservice: metrics
ms.topic: conceptual
author: albecker1
ms.author: albecker
ms.date: 03/12/2020
ms.openlocfilehash: f2b960c2198800e04da77ad6b5be78d7b4762354
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79299809"
---
# <a name="disk-metrics-deprecation-in-the-azure-portal"></a>Azure portalında disk ölçümleri amortismanı

Deprecated disk ile ilgili ölçümler yakında Azure portalıkaldırılacaktır. Her amortismana uygun ölçümün yeni bir sürümünü kullanmanız için kullanılabilir. Bu makalede, hangi ölçümlerin yeni olduğu ve bunları kullanmak için metrik uyarılarınızın nasıl güncelleştirildiği gösterilmektedir.

## <a name="list-of-new-metrics"></a>Yeni ölçümler listesi

Bu tablo, amortismana uğran her ölçüğe karşılık gelen yeni metrikle eşlenir. 

|Amortismana ermiş metrik|Yeni (değiştirme) metrik|
|----|----|
|Veri Diski QD (Deprecated)|Veri DiskI Sıra Derinliği (Önizleme)|
|Veri Diski Okuma Bayt/Sn (Amortismana Ermiş)|Veri Diski Okuma Bayt/Sn (Önizleme)|
|Veri Diski Okuma İşlemleri/Sn (Amortismana Alındı)|Veri Diski Okuma İşlemleri/Sn (Önizleme)|
|Veri Diski Yazma Bayt/Sn (Amortismana Eritilmiş)|Veri Diski Yazma Bayt/Sn (Önizleme)|
|Veri Diski Yazma İşlemleri/Sn (Amortismana Alındı)|Veri Disk ihyası işlemleri/sn (Önizleme)|
|İşletim Sistemi QD (Amortismana Uğradı)|İşletim Sistemi Sıra Derinliği (Önizleme)|
|İşletim Sistemi Okuma Bayt/Sn (Amortismana Ermiş)|İşletim Sistemi Okuma Bayt/Sn (Önizleme)|
|İşletim Sistemi Okuma İşlemleri/Sn (Amortismana Uğradı)|İşletim Sistemi Okuma İşlemleri/Sn (Önizleme)|
|İşletim Sistemi Bayt/Sn (Amortismana Göre) Yaz|İşletim Sistemi Bayt/Sn Yaz (Önizleme)|
|İşletim Sistemi Yazma İşlemleri/Sn (Amortismana Uğradı)|İşletim Sistemi Yazma İşlemleri/Sn (Önizleme)|

<a id="update-metrics" />

## <a name="migrate-metrics-in-your-metric-alerts"></a>Metrik uyarılarınızdaki ölçümleri geçirin

Yeni ölçümleri kullanmak için metrik uyarılarınızı güncelleştirin.

1. Azure portalında **Uyarıları**arayın. Ardından, **Hizmetler** bölümünde **Uyarılar'ı**seçin.

   > [!div class="mx-imgBorder"]
   > ![Resim açıklaması](./media/portal-disk-metrics-deprecation/alert-service-azure-portal.png)

2. **Uyarılar** sayfasında, **Uyarı Kurallarını Yönet** düğmesini seçin. 

   > [!div class="mx-imgBorder"]
   > ![Resim açıklaması](./media/portal-disk-metrics-deprecation/manage-alert-rules-button.png)

3. Kaynak **grubu** açılır listesinde **Sanal Makineler** onay kutusunu seçin ve **Sinyal türü** açılır listesinde **Ölçümler** onay kutusunu seçin. 

   > [!div class="mx-imgBorder"]
   > ![Resim açıklaması](./media/portal-disk-metrics-deprecation/filter-alerts.png)

4. Ölçümler listesinde, disklerle ilgili koşulları tanımlayın. Kuralın adını tıklatın. 

   Ad, tablonun **Ad** sütununda bir köprü olarak görünür.

   > [!div class="mx-imgBorder"]
   > ![Resim açıklaması](./media/portal-disk-metrics-deprecation/find-disk-conditions.png)

5. **Kurallar yönetimi** sayfasının **Koşullar** bölümünde, uyarının durumunu tıklatın. 

   Durum bir köprü olarak görünür.  

   > [!div class="mx-imgBorder"]
   > ![Resim açıklaması](./media/portal-disk-metrics-deprecation/adjust-condition.png)

   **Yapılandırma sinyali mantığı** sayfası görüntülenir ve koşulun ayarları o sayfanın **Uyarı mantığı** bölümünde görünür.

6. Amortismana uğrama gelen metriği kaldırdığınızda kaybolacakları için bu ayarların kaydını yapın.

   > [!div class="mx-imgBorder"]
   > ![Resim açıklaması](./media/portal-disk-metrics-deprecation/condition-rules.png)

   > [!TIP] 
   > Bu ayarları ekran görüntüsünde veya metin dosyasında yakalamayı düşünün. 

7. Sinyal seçimi bağlantısını **yapmak için Geri'yi** tıklatın.

   > [!div class="mx-imgBorder"]
   > ![Resim açıklaması](./media/portal-disk-metrics-deprecation/back-to-signal-selection.png)

8. **Yapılı sinyal** mantık sayfasında uygun yedek metrik (yeni metrik) seçin. Yeni metnin adını kimlikle ilişkilendirmek için bu makalede daha önce görünen [tabloyu](#update-metrics) kullanın.

   > [!TIP] 
   > Metrik adlistesini daraltmak için arama çubuğuna yazmaya başlayın. 

   > [!div class="mx-imgBorder"]
   > ![Resim açıklaması](./media/portal-disk-metrics-deprecation/choose-new-metric.png)

9. **Bitti** düğmesini seçin. 

   > [!div class="mx-imgBorder"]
   > ![Resim açıklaması](./media/portal-disk-metrics-deprecation/set-new-metric.png)

10. **Kaydet** düğmesini seçerek değişikliklerinizi gerçekleştirin. 

    > [!div class="mx-imgBorder"]
    > ![Resim açıklaması](./media/portal-disk-metrics-deprecation/save-new-metric.png)






