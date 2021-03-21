---
title: Tüm kurumsal sensörler tarafından bulunan cihazlar hakkında bilgi edinin
description: Bağlı sensörlerden cihaz bilgilerinin kapsamlı bir görünümünü almak için şirket içi yönetim konsolunda cihaz envanterini kullanın. Bu bilgileri yönetmek için içeri aktarma, dışarı aktarma ve filtreleme araçlarını kullanın.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 12/02/2020
ms.topic: how-to
ms.service: azure
ms.openlocfilehash: 9da5c8c89ee124e527584164b21b096ac815e5ca
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "100524034"
---
# <a name="investigate-all-enterprise-sensor-detections-in-the-device-inventory"></a>Cihaz envanterindeki tüm kurumsal algılayıcı algılamalarını araştırın

Şirket içi yönetim konsolunda *cihaz envanterini* kullanarak bağlı sensörlerden cihaz bilgilerini görüntüleyebilirsiniz. Bu özellik size tüm ağ bilgilerinin kapsamlı bir görünümünü sunar. Bu bilgileri yönetmek için içeri aktarma, dışarı aktarma ve filtreleme araçlarını kullanın. Bağlı algılayıcı sürümleri hakkındaki durum bilgileri de görüntülenir.

:::image type="content" source="media/how-to-work-with-asset-inventory-information/device-inventory-data-table.png" alt-text="Cihaz envanter verileri tablosunun ekran görüntüsü.":::

Aşağıdaki tabloda, cihaz envanterindeki tablo sütunları açıklanmaktadır.

| Parametre | Açıklama |
|--|--|
| **Kabul edilmemiş uyarılar** | Bu cihazla ilişkili işlenmemiş uyarıların sayısı. |
| **İş birimi** | Bu cihazı içeren iş birimi. |
| **Bölge** | Bu cihazı içeren bölge. |
| **Site** | Bu cihazı içeren site. |
| **Bölge** | Bu cihazı içeren bölge. |
| **Elektrikli** | Bu cihazı koruyan IoT algılayıcısı için Azure Defender. |
| **Ad** | IoT için Defender olarak bu cihazın adı bulundu. |
| **Tür** | PLC veya HMı gibi cihaz türü. |
| **Satıcı** | MAC adresinde tanımlandığı şekilde cihazın satıcısının adı. |
| **İşletim Sistemi** | Cihazın işletim sistemi. |
| **Üretici yazılımı** | Cihazın üretici yazılımı. |
| **IP Adresi** | Cihazın IP adresi. |
| **VLAN** | Cihazın VLAN 'ı. |
| **MAC adresi** | Cihazın MAC adresi. |
| **Protokoller** | Cihazın kullandığı protokoller. |
| **Kabul edilmemiş uyarılar** | Bu cihazla ilişkili işlenmemiş uyarıların sayısı. |
| **Yetkilendirildi** | Cihazın yetkilendirme durumu:<br />- **Doğru**: cihaz yetkilendirildi.<br />- **Yanlış**: cihaz yetkilendirilmedi. |
| **Tarayıcı olarak bilinir** | Bu cihazın ağda tarama benzeri etkinlikleri yapıp gerçekleştirmediğini belirtir. |
| **Programlama aygıtı** | Bu bir programlama aygıtı olup olmadığı.<br />- **Doğru**: cihaz, mühendislik istasyonlarıyla Ilgili olan PLCs, DTU 'lar ve denetleyiciler için programlama etkinlikleri gerçekleştirir.<br />- **Yanlış**: cihaz bir programlama aygıtı değildir. |
| **Gruplar** | Bu cihazın katıldığı gruplar. |
| **Son etkinlik** | Cihazın gerçekleştirdiği son etkinlik. |
| **Tespit** | Bu cihaz ağda ilk kez görüldüğünde. |

## <a name="integrate-data-into-the-enterprise-device-inventory"></a>Verileri kurumsal cihaz envanteriyle tümleştirme

Veri tümleştirme özellikleri, diğer kurumsal kaynaklardaki bilgilerle cihaz envanterindeki verileri geliştirmenize olanak sağlar. Bu kaynaklar CMDBs, DNS, güvenlik duvarları ve Web API 'Leri içerir.

Bu bilgileri öğrenmek için kullanabilirsiniz. Örnek:

- Cihaz satın alma tarihleri ve garanti sonu tarihleri

- Her cihazdan sorumlu olan kullanıcılar

- Cihazlar için açık anahtarlar

- Üretici yazılımının yükseltildiği son tarih

- İnternet erişimine izin verilen cihazlar

- Etkin virüsten koruma uygulamalarını çalıştıran cihazlar

- Cihazlarda oturum açan kullanıcılar

:::image type="content" source="media/how-to-work-with-asset-inventory-information/asset-inventory-screen-with-items-highlighted-v2.png" alt-text="Cihaz envanteri ekranındaki veri tablosu.":::

Verileri şu şekilde tümleştirebilirsiniz:

- El ile ekleme

- IoT için Defender 'ın sağladığı özelleştirilmiş betikleri çalıştırma

:::image type="content" source="media/how-to-work-with-asset-inventory-information/enterprise-data-integrator-graph.png" alt-text="Kurumsal veri tümleştirici diyagramı.":::

Sisteminizi Web API sorguları alacak şekilde ayarlamak için, IoT teknik desteği için Defender ile çalışabilirsiniz.

Verileri el ile eklemek için:

1. Yan menüde **cihaz envanteri** ' ni seçin ve ardından öğesini seçin :::image type="icon" source="media/how-to-work-with-asset-inventory-information/menu-icon.png" border="false"::: .

   :::image type="content" source="media/how-to-work-with-asset-inventory-information/asset-inventory-settings-v2.png" alt-text="Cihazınızın envanter ayarlarını düzenleyin.":::

2. **Cihaz envanteri ayarları** ILETIŞIM kutusunda **özel sütun Ekle**' yi seçin.

   :::image type="content" source="media/how-to-work-with-asset-inventory-information/add-custom-column.png" alt-text="Stoğunuzun özel bir sütununu ekleyin.":::

3. **Özel sütun Ekle** iletişim kutusunda, yeni sütun adını (en fazla 250 karakter utf) ekleyin, **el ile**' yi seçin ve **Kaydet**' i seçin. Yeni öğe, **cihaz envanteri ayarları** iletişim kutusunda görünür.

4. **Cihaz envanteri** penceresinin sağ üst köşesinde, :::image type="icon" source="media/how-to-work-with-asset-inventory-information/menu-icon-device-inventory.png" border="false"::: **tüm cihaz envanterini dışarı aktar**' ı seçin ve seçin. CSV dosyası oluşturulur.

   :::image type="content" source="media/how-to-work-with-asset-inventory-information/sample-exported-csv-file.png" alt-text="İçe aktarılmış CSV dosyası.":::

5. Bilgileri yeni sütuna el ile ekleyin ve dosyayı kaydedin.

6. **Cihaz envanteri** penceresinin sağ üst köşesinde, :::image type="icon" source="media/how-to-work-with-asset-inventory-information/menu-icon-device-inventory.png" border="false"::: öğesini seçin, **El Ile giriş sütunlarını içeri aktar**' ı seçin ve CSV dosyasına gidin. Yeni veriler, **cihaz envanter** tablosunda görüntülenir.

Diğer kurumsal varlıklardan verileri bütünleştirmek için:

1. **Cihaz envanteri** penceresinin sağ üst köşesinde, :::image type="icon" source="media/how-to-work-with-asset-inventory-information/menu-icon-device-inventory.png" border="false"::: **tüm cihaz envanterini dışarı aktar**' ı seçin ve seçin.

2. **Cihaz envanteri ayarları** ILETIŞIM kutusunda **özel sütun Ekle**' yi seçin.

   :::image type="content" source="media/how-to-work-with-asset-inventory-information/add-custom-column.png" alt-text="Stoğunuzun özel bir sütununu ekleyin.":::

3. **Özel sütun Ekle** iletişim kutusunda yeni sütun adını (en fazla 250 karakter utf) ekleyin ve **Otomatik**' i seçin. **Betiği yükle** ve **Test betiği** seçenekleri görüntülenir.

   :::image type="content" source="media/how-to-work-with-asset-inventory-information/add-custom-column-automatic.png" alt-text="Özel sütunları otomatik olarak ekle.":::

4. [Microsoft desteği](https://support.serviceshub.microsoft.com/supportforbusiness/create?sapId=82c88f35-1b8e-f274-ec11-c6efdd6dd099)aldığınız betiği karşıya yükleyin ve test edin.

## <a name="retrieve-information-from-the-device-inventory"></a>Cihaz envanterinden bilgi alma

Yönetilen algılayıcılar tarafından algılanan kapsamlı cihaz bilgilerini alabilir ve bu bilgileri iş ortağı sistemleriyle tümleştirebilirsiniz. Örneğin, algılayıcı, bölge, site KIMLIĞI, IP adresi, MAC adresi, bellenim, protokol ve satıcı bilgilerini alabilirsiniz. Aşağıdakileri temel alarak aldığınız bilgileri filtreleyin:

- Yetkili ve yetkisiz cihazlar.

- Belirli sitelerle ilişkili cihazlar.

- Belirli bölgelerle ilişkili cihazlar.

- Belirli algılayıcılar ile ilişkili cihazlar.

Bu bilgileri almak ve bütünleştirmek için, IoT API komutlarına yönelik Defender ile çalışın. Daha fazla bilgi için bkz. [ıOT API algılayıcısı ve Yönetim Konsolu API 'leri Için Defender](references-work-with-defender-for-iot-apis.md).

## <a name="filter-the-device-inventory"></a>Cihaz envanterini filtreleme

İlgilendiğiniz sütunları göstermek için cihaz envanterine filtre uygulayabilirsiniz. Örneğin, PLC cihaz bilgilerini görüntüleyebilirsiniz.

:::image type="content" source="media/how-to-work-with-asset-inventory-information/asset-inventory-view-v2.png" alt-text="Cihaz envanterinin ekran görüntüsü.":::

Pencereyi bıraktığınızda filtre temizlenir.

Aynı filtreyi birden çok kez kullanmak için, bir filtre veya ihtiyacınız olan filtrelerin bir birleşimini kaydedebilirsiniz. Sol Bölmeyi açabilir ve kaydettiğiniz filtreleri görüntüleyebilirsiniz:

:::image type="content" source="media/how-to-work-with-asset-inventory-information/view-your-asset-inventories-v2.png" alt-text="Cihaz envanterleri ekranı.":::

Cihaz envanterini filtrelemek için:

1. Filtrelemek istediğiniz sütunda öğesini seçin :::image type="icon" source="media/how-to-work-with-asset-inventory-information/filter-a-column-icon.png" border="false"::: .

2. **Filtre** iletişim kutusunda filtre türünü seçin:

   - **Eşittir**: sütunu filtrelemek istediğiniz değere göre tam değer. Örneğin, protokol sütununu **Equals** ve şuna göre filtreleyerek `value=ICMP` , sütun yalnızca ICMP protokolünü kullanan cihazları sunar.

   - **Contains**: sütundaki diğer değerler arasında yer alan değer. Örneğin, protokol sütununu ve **içerir** öğesine göre filtreleyerek `value=ICMP` , sütun, ICMP protokolünü kullanan cihazları cihazın kullandığı protokoller listesinin bir parçası olarak sunar.

3. Sütun bilgilerini alfabetik sıraya göre düzenlemek için öğesini seçin :::image type="icon" source="media/how-to-work-with-asset-inventory-information/alphabetical-order-icon.png" border="false"::: . Ve oklarını seçerek sırayı düzenleyin :::image type="icon" source="media/how-to-work-with-asset-inventory-information/alphabetical-a-z-order-icon.png" border="false"::: :::image type="icon" source="media/how-to-work-with-asset-inventory-information/alphabetical-z-a-order-icon.png" border="false"::: .

4. Yeni bir filtre kaydetmek için filtreyi tanımlayın ve **farklı kaydet**' i seçin.

5. Filtre tanımlarını değiştirmek için tanımları değiştirin ve **Değişiklikleri Kaydet**' i seçin.

## <a name="view-device-information-per-zone"></a>Her bölge için cihaz bilgilerini görüntüleme

Bir bölgedeki cihazlarla ilgili olarak aşağıdaki bilgileri öğrenebilirsiniz.

### <a name="view-a-device-map"></a>Bir cihaz haritasını görüntüleme

Seçili bir bölge için bir algılayıcı cihaz haritasını görüntülemek için:

- **Site yönetimi** penceresinde, bölge adını Içeren çubuktan **bölge haritasını görüntüle** ' yi seçin.

  :::image type="content" source="media/how-to-work-with-asset-inventory-information/default-region-to-default-business-unit-v2.png" alt-text="Varsayılan bölge varsayılan iş birimidir.":::

**Cihaz Haritası** penceresi görüntülenir. Bu, algılayıcı, bunlara bağlı cihazlar ve diğer bilgiler dahil olmak üzere seçili bölge ile ilgili tüm ağ öğelerini gösterir.

:::image type="content" source="media/how-to-work-with-asset-inventory-information/zone-map-screenshot.png" alt-text="Bölge eşlemesinin ekran görüntüsü.":::

Haritalardan cihazları ve cihaz bilgilerini görüntülemek için aşağıdaki araçlar kullanılabilir. Bu özelliklerin her biri hakkında ayrıntılı bilgi için bkz. *IoT Platformu Için Defender Kullanıcı Kılavuzu*.

- **Harita yakınlaştırma görünümleri**: Basitleştirilmiş görünüm, bağlantılar görünümü ve ayrıntılı görünüm. Görüntülenmiş harita görünümü haritanın yakınlaştırma düzeyine bağlı olarak değişir. Yakınlaştırma düzeylerini ayarlayarak harita görünümleri arasında geçiş yapabilirsiniz.

  :::image type="icon" source="media/how-to-work-with-asset-inventory-information/zoom-icon.png" border="false":::

- **Harita arama ve Düzen araçları**: değişen ağ kesimlerini, cihazları, cihaz gruplarını veya katmanları göstermek için kullanılan araçlar.

  :::image type="content" source="media/how-to-work-with-asset-inventory-information/search-and-layout-tools.png" alt-text="Arama ve yerleşim araçları görünümünün ekran görüntüsü.":::

- **Cihazlarda Etiketler ve göstergeler:** Örneğin, bir BT ağındaki bir alt ağda gruplandırılan cihazların sayısı. Bu örnekte 8 ' dir.

  :::image type="content" source="media/how-to-work-with-asset-inventory-information/labels-and-indicators.png" alt-text="Etiketlerin ve göstergelerin ekran görüntüsü.":::

- **Cihaz özelliklerini görüntüle**: Örneğin, cihazı izleyen algılayıcı ve temel cihaz özellikleri. Cihaz özelliklerini görüntülemek için cihaza sağ tıklayın.

  :::image type="content" source="media/how-to-work-with-asset-inventory-information/asset-properties-v2.png" alt-text="Cihaz özellikleri görünümünün ekran görüntüsü.":::

- **Bir cihazla Ilişkili uyarı:** İlgili uyarıları görüntülemek için cihaza sağ tıklayın.

  :::image type="content" source="media/how-to-work-with-asset-inventory-information/show-alerts.png" alt-text="Uyarıları göster görünümünün ekran görüntüsü.":::

### <a name="view-alerts-associated-with-a-zone"></a>Bir bölgeyle ilişkili uyarıları görüntüleme

Belirli bir bölgeyle ilişkili uyarıları görüntülemek için:

- **Bölge** penceresini oluşturan uyarı simgesini seçin. 

  :::image type="content" source="media/how-to-work-with-asset-inventory-information/business-unit-view-v2.png" alt-text="Örneklerle varsayılan Iş birimi görünümü.":::

Daha fazla bilgi için bkz. [genel bakış: uyarılarla çalışma](how-to-work-with-alerts-on-premises-management-console.md).

### <a name="view-the-device-inventory-of-a-zone"></a>Bir bölgenin cihaz envanterini görüntüleme

Belirli bir bölgeyle ilişkili cihaz envanterini görüntülemek için:

- **Bölge** penceresinden **cihaz envanterini görüntüle** ' yi seçin.

  :::image type="content" source="media/how-to-work-with-asset-inventory-information/default-business-unit.png" alt-text="Cihaz envanteri ekranı görüntülenir.":::

Daha fazla bilgi için bkz. [cihaz envanterinde tüm kurumsal algılayıcı algılamalarını araştırın](how-to-investigate-all-enterprise-sensor-detections-in-a-device-inventory.md).

### <a name="view-additional-zone-information"></a>Ek bölge bilgilerini görüntüle

Aşağıdaki ek bölge bilgileri kullanılabilir:

- **Bölge ayrıntıları**: Bölgeyle ilişkili cihaz, uyarı ve algılayıcı sayısını görüntüleyin.

- **Algılayıcı ayrıntıları**: bölgeye atanan her algılayıcının adını, IP adresini ve sürümünü görüntüleyin.

- **Bağlantı durumu**: bir algılayıcı bağlantısı kesilirse sensörden bağlanın. Bkz. [Şirket içi yönetim konsoluna algılayıcıların bağlanmasını sağlar](how-to-activate-and-set-up-your-on-premises-management-console.md#connect-sensors-to-the-on-premises-management-console). 

- **Güncelleştirme ilerlemesi**: bağlı algılayıcı yükseltiliyorsa, yükseltme durumları görüntülenir. Yükseltme sırasında şirket içi yönetim konsolu, algılayıcıdan cihaz bilgilerini almaz.

## <a name="see-also"></a>Ayrıca bkz.

[Bir cihaz envanterinde algılayıcı algılamalarını araştırma](how-to-investigate-sensor-detections-in-a-device-inventory.md)
