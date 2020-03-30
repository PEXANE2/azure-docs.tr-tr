---
title: Azure Marketi Için Bulut İş Ortağı Portalında Sanal makine SUS sekmesi
description: Azure Marketi'nde sanal makine teklifi oluştururken kullanılan SNU sekmesini açıklar.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: dsindona
ms.openlocfilehash: 49f1de5128325b2884ea76b010727be45f1b195d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80288895"
---
# <a name="virtual-machine-skus-tab"></a>Sanal makine SKUs sekmesi

**Yeni Teklif** sayfasının **STU** sekmesi, bir veya daha fazla SNU oluşturmanıza ve bunları yeni teklifinize ilişkilendirmenize olanak tanır.  Farklı SCO'lar bir çözümü özellik kümelerine, VM görüntü türlerine, iş hacmine veya ölçeklenebilirliğe, faturalandırma modellerine veya başka bir özelliğe göre ayırt edebilir.


## <a name="create-a-sku"></a>Bir SKU oluşturma

Başlangıçta, yeni bir teklif herhangi bir ilişkili SKU olmayacak, bu yüzden **Yeni SKU**tıklayarak bir oluşturacak .

![Sanal makineler için Yeni Teklif sekmesinde yeni SKU düğmesi](./media/publishvm_005.png)

<br/>

**Yeni SKU** iletişim kutusu görüntülenir.  Yeni SKU için tanımlayıcıyı girin ve **ardından Tamam'ı**tıklatın. (Tanımlayıcı adlandırma kuralları için aşağıya bakın.)  **SK'ler** sekmesi artık düzenlenebilen alanları görüntüler.    Alan adına eklenen bir yıldız işareti (*) bunun gerekli olduğunu gösterir.

<!-- TD: This tab has been updated, now has "Old Pricing" and "Simplified Currency Pricing" sections"! -->

![Sanal makineler için Yeni Teklif formunda SKU sekmesi](./media/publishvm_006.png)

Aşağıdaki tabloda bu alanların amacı, içeriği ve biçimlendirmesi açıklanmaktadır.  Gerekli alanlar yıldız işareti (*) tarafından suçlanır.

<!-- TD: I took a new screenshot, and the fields differ somewhat from description in the VM Pub Guide.  Needs review. -->

|  **Alan**       |     **Açıklama**                                                          |
|  ---------       |     ---------------                                                          |
|  *SKU Ayarları*   |    |
| **SKU KİmLİk\***       | Bu SKU için tanımlayıcı.  Bu ad, küçük alfasayısal karakterler veya tirelerden (-) oluşan en fazla 50 karaktere sahiptir, ancak tire ile son kullanamaz.  Teklif yayımlandıktan sonra değiştirilemez.  |
|  *SKU Detayları*   |  |
| **Başlık\***        | Pazarda görüntülemek için teklif için dostu adı. Maksimum uzunluğu 50 karakter. |
| **Özet\***      | Pazarda görüntülemek için teklif in özlü açıklaması. Maksimum uzunluğu 100 karakter. |
| **Açıklama\***  | Teklifin daha ayrıntılı bir açıklamasını sağlayan açıklama metni.  <!-- TD: max len/guidance? 3k characters -->  |
| **Bu SKU'yı sakla\*** | SKU'nun pazarda müşterilertarafından görülüp görülemeyeceğini gösterir.  SKU'yu tek tek satın almak için değil, yalnızca çözüm şablonları aracılığıyla kullanılabilir olmasını istiyorsanız gizlemek isteyebilirsiniz.  Ayrıca ilk test veya geçici veya mevsimlik teklifler için yararlı olabilir. |
| **Bulut Kullanılabilirliği\*** | SKU'nun hangi bulutlarda kullanılabileceğini belirler.  Varsayılan değer, Azure'un genel sürümüdür.  Microsoft Azure Hükümeti, ABD Federal, Eyalet, yerel veya kabile hükümetleri ve sertifikalı iş ortakları için kontrollü erişime sahip bir devlet topluluğu bulutudur.  Devlet bulutu hakkında daha fazla bilgi için azure [devlete hoş geldiniz](https://docs.microsoft.com/azure/azure-government/documentation-government-welcome)' e bakın. |
| **Bu özel bir SKU mu?\*** | SKU'nun özel mi yoksa herkese açık mı olduğunu gösterir. Varsayılan değer **Hayır** (ortak).  Daha fazla bilgi için [Genel ve Özel SK'lere](../../cloud-partner-portal-orig/cloud-partner-portal-azure-private-skus.md)bakın. |
| **Ülke/Bölge Durumu\*** | SKU'nuzun hangi ülkelerveya dünya bölgeleri satın alınacağını belirler. En az bir bölge/ülke seçin. <!-- TD: Is this parameter an AMP visibility control or a contractual one, or both? --> |  
|  *Fiyatlandırma*   |  |
| **Lisans Modeli\***| Kullanılacak standart faturalandırma modeli.  **Kullanım tabanlı aylık faturalı SKU'yı**seçerseniz, çekirdek başına fiyatlandırmanın ayrıntılarını ve ücretsiz deneme süresi sunmak isteyip istemediğinizi belirtmenizi sağlamak için bir akordeon bölümü açılır.  Bu bölüm ayrıca bu fiyatlandırma zamanlamasını Excel'e dışa aktarmanızı ve almanızı da sağlar. Daha fazla bilgi için [Azure Marketi'ndeki Faturalandırma seçeneklerine](../../billing-options-azure-marketplace.md)bakın. | 
|  *VM Görüntüler*   |  |
| **İşletim Sistemi Ailesi\*** | VM çözümmünün Windows veya Linux tabanlı olup olmadığını gösterir. |
| **İşletim Sistemi Türünü Seçin** | Belirtilen işletim sistemi belirli satıcı veya sürümü. |
| **İşletim Sistemi Dostu Adı\*** | İşletim sistemi adı müşterilere görüntülenecek.  |
| **Önerilen VM Boyutları\*** | Standart bir listeden önerilen altı VM boyutuna kadar seçim yapılmasını sağlar.  Bu liste Azure portalına ve Microsoft pazar yerlerine aktarılır.  Bu listede geçerli olan ilk VM boyutu (bu müşteri aboneliği, bölge, bölge, vb.) bu potansiyel müşteri için varsayılan olarak ayarlanır.  Kullanıcı bu boyutu çözüm görüntüsüyle uyumlu olarak değiştirebilir. | 
| **Açık Bağlantı Noktaları**| Limanlar açılacak ve SKU'yu destekleyecek protokol.  Bu yapılandırmalar, çözüm VM ağı için yapılandırdığınız sanal ağile eşleşmelidir. Bu ayarlar VM dağıtımı sırasında etkine gider. Ancak, Bir SKU yayımladıktan sonra Bağlantı Noktası ayarları değiştirilebilir. Daha fazla bilgi için bkz. [Azure portalıyla bir sanal makineye bağlantı noktaları açma](https://docs.microsoft.com/azure/virtual-machines/windows/nsg-quickstart-portal). <br/>Aşağıdaki varsayılan ağ eşlemeleri tüm VM'lere eklenir. &emsp;Windows: 3389 -> 3389 TCP, 5986 -> 5986 TCP; &emsp; Linux: 22 -> 22, TCP (SSH). |
| **Disk Sürümü**  | İlişkili çözüm VM, disk sürüm numarası ve disk URL tarafından belirtilir. Disk sürümü [anlamsal sürüm](https://semver.org/) biçiminde olmalıdır: `<major>.<minor>.<patch>`.  URL, işletim sistemi VHD için oluşturulan paylaşılan erişim imzası URI'dir.  SKU başına en fazla sekiz disk sürümü ekleyebilirsiniz, ancak Azure Marketi'nde yalnızca bir SKU için en yüksek disk sürüm numarası gösterilecektir. Diğer sürümler yalnızca API'ler aracılığıyla görünür olacaktır.  <!--TD: Add more specific link to API --> <br/> **Yeni veri diski** akordeon bölümü, VM'nize en fazla 15 veri diski eklemenizi sağlar.  Belirli bir VM sürümü ve ilişkili veri diskleri içeren bir SKU yayımladıktan sonra, bu yapılandırma değiştirilemez.  SKU'ya ek VM sürümleri eklenirse, aynı sayıda veri diskini de desteklemeleri gerekir. <br/> Azure tabanlı VM görüntü(leri) oluşturmadıysanız, bu alanı daha sonra güncelleştirme ekleyebilirsiniz.  İlişkili VM kaynağı oluşturma hakkında daha fazla bilgi için [VM teknik varlıkları oluştur](./cpp-create-technical-assets.md)bölümüne bakın.  
|  |  |

<!-- TD: The CPP UX warning msg indicates that underscores are also supported in these SKU IDs. I suspect this might be true for other identifiers. --> 

<br/> İlerlemenizi kaydetmek için **Kaydet'i** tıklatın. Bir sonraki sekmede, teklifinizin [Test Sürüşü'nu](./cpp-test-drive-tab.md)destekleyip desteklemediğinizi belirteceksiniz.


## <a name="additional-pricing-considerations"></a>Ek fiyatlandırma konuları

Yukarıda açıklanan fiyatlandırma modeli temel bir açıklamadır.  Değişiklikler eleniyor ve yerel veya bölgesel vergi yönetmeliklerinden ve Microsoft fiyatlandırma ilkelerinden etkilenebilir. 

### <a name="new-core-sizes-added-on-722019"></a>7/2/2019 tarihinde eklenen yeni çekirdek boyutları

VM yayıncılarına 2 Temmuz 2019'da yeni Azure sanal makine boyutları için yeni fiyatların eklenmesi (çekirdek sayısına bağlı olarak) bildirilir.  Yeni fiyatlar çekirdek boyutları 10, 44, 48, 60, 120, 208 ve 416 içindir.  Mevcut VM için bu çekirdek boyutları için yeni fiyatlar otomatik olarak cari fiyatlara göre hesaplanmıştır sunuyor.  Yayıncıların ek fiyatları gözden geçirmek ve istenen değişiklikleri yapmak için 1 Ağustos 2019'a kadar zamanları vardır.  Bu tarihten sonra, yayımcı tarafından yeniden yayımlanmazsa, bu yeni çekirdek boyutları için otomatik olarak hesaplanan fiyatlar geçerli olacaktır.


### <a name="simplified-currency-pricing"></a>Basitleştirilmiş Döviz Fiyatlandırması

1 Eylül 2018 tarihinden itibaren **basitleştirilmiş döviz fiyatlandırması** adlı yeni bir bölüm portala eklenecektir. Microsoft, dünyanın dört bir yanındaki müşterilerinizden daha öngörülebilir fiyatlandırma ve koleksiyonlar sağlayarak Azure Marketi işini kolaylaştırıyor. Bu düzene, müşterilerinize fatura verdiğimiz para birimi sayısını azaltmak da dahildir.  Daha fazla bilgi için Azure [Marketi'ndeki mevcut bir VM teklifini güncelleştir'e](./cpp-update-existing-offer.md)bakın.


### <a name="additional-information-on-taxes-and-prices"></a>Vergiler ve fiyatlar hakkında ek bilgiler

* Microsoft, bazı ülkeleri/bölgeleri *vergi olarak*kabul edilen ülkeler olarak sınıflandırMaktadır.  Bu tür ülkelerde/bölgelerde, Microsoft müşterilerden vergi toplar ve ardından hükümete vergi öder (para lar) öder.  Diğer ülkelerde/bölgelerde, ortaklar genellikle müşterilerinden vergi toplamak ve hükümete vergi ödemekle yükümlüdürler. İkinci ülkelerde/bölgelerde satış yapmayı seçerseniz, yerel vergileri hesaplama ve ödeme yeteneğine sahip olmalısınız.  <!-- TD: Find a good reference on taxing policies. The best I found was in the UWP section: https://docs.microsoft.com/windows/uwp/publish/tax-details-for-paid-apps -->
* Bir teklif yayına girdiğinde fiyatlar değiştirilemez. Ancak, desteklenen bölgeleri eklemeye veya kaldırmaya devam edebilirsiniz. 
* Microsoft, planlanan SKU ücretlerinize ek olarak müşteri standardı Azure VM kullanım ücretlerini de ücretlendirmez.
* Fiyatlar, fiyatları ayarlama sırasında mevcut döviz kurları üzerinde yerel para birimi cinsinden tüm bölgeler için ayarlanır.  <!-- TD: Meaning? - Offer created, published, other? -->
* Her bölgenin fiyatını ayrı ayrı ayarlamak için, lütfen fiyatlandırma tablosunu dışa aktarın, özel fiyatlandırma uygulayın ve sonra içe aktarın. 


## <a name="next-steps"></a>Sonraki adımlar

İsteğe bağlı olarak, bu özelliği destekliyorsanız [Test Sürüşü](./cpp-test-drive-tab.md) bilgilerini belirteceksiniz; aksi takdirde, teklifiniz için [pazar yeri](./cpp-marketplace-tab.md) verileri sağlarsınız.
