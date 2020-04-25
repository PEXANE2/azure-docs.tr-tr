---
title: Azure Marketi için Bulut İş Ortağı Portalı sanal makine marketi sekmesi
description: Azure Marketi VM teklifi oluşturma konusunda kullanılan Market sekmesini açıklar.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: dsindona
ms.openlocfilehash: 2c3d316d0d2810cb2a25ffd3bc4e34cf3454c10d
ms.sourcegitcommit: f7fb9e7867798f46c80fe052b5ee73b9151b0e0b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82146851"
---
# <a name="virtual-machine-marketplace-tab"></a>Sanal makine marketi sekmesi

> [!IMPORTANT]
> 13 Nisan 2020 ' den itibaren, Azure sanal makine tekliflerinizin yönetimini Iş Ortağı Merkezi 'ne taşımaya başlayacağız. Geçişten sonra, Iş Ortağı Merkezi 'nde tekliflerinizi oluşturup yönetirsiniz. Geçirilen tekliflerinizi yönetmek için [Azure sanal makine oluşturma teklifi](https://docs.microsoft.com/azure/marketplace/partner-center-portal/azure-vm-create-offer) ' nde bulunan yönergeleri izleyin.

**Yeni teklif** sayfasının **Market** sekmesi, olası müşterilerinize pazarlama, satış ve yasal bilgiler ve anlaşmalar sağlamanıza ve Market 'ten oluşturulan müşteri adaylarını yönetmenize olanak sağlar. Bu uzun biçim dört bölüme ayrılmıştır: **genel bakış**, **Pazarlama yapıtları**, **müşteri adayı yönetimi**ve **yasal**.


## <a name="overview-section"></a>Genel Bakış bölümü
Bu bölümde, Azure Market teklifiniz hakkında genel bilgileri girersiniz.  Alan adı üzerinde eklenen bir yıldız işareti (*) gerekli olduğunu gösterir.

![Sanal makineler için Market sekmesinin genel bakış bölümü](./media/publishvm_008.png)

Aşağıdaki tabloda bu alanların amacı ve içeriği açıklanmaktadır. Gerekli alanlar bir yıldız işareti (*) ile tanımlanır.

|  **Alan**                |     **Açıklama**                                                          |
|  ---------                |     ---------------                                                          |
| **Başlık\***                 | Teklifin başlığı, genellikle uzun, resmi adı. Bu başlık Market 'te göze çarpacak olarak görüntülenecektir.  En fazla 50 karakter uzunluğunda. |
| **Özet\***               | Çözüm için kısa bir amaç veya işlev.  En fazla 100 karakter uzunluğunda. |
| **Uzun Özet\***          | Çözüm amacı veya işlevi.  En fazla 256 karakter uzunluğunda. |
| **Açıklama\***           | Çözümün açıklaması.  En fazla 3000 karakter uzunluğunda basit HTML biçimlendirmesini destekler. |
| **Microsoft CSP satıcı kanalı\*** | Bulut çözümü sağlayıcıları (CSP) iş ortağı kanalı kabul etme artık kullanılabilir.  Teklifinizi Microsoft CSP iş ortağı kanalları aracılığıyla pazarlama hakkında daha fazla bilgi için lütfen bkz. [bulut çözümü sağlayıcıları](../../cloud-solution-providers.md) . |
| **Pazarlama tanımlayıcısı\***  | Bu teklifle ilişkilendirilecek benzersiz bir URL, genellikle kuruluşunuzun ve çözüm adı, maksimum uzunluk 50 karakter içerir.  Örneğin: <br/> `https://azuremarketplace.microsoft.com/marketplace/apps/contoso.sampleApp`  |
| **Abonelik kimliklerini Önizle\*** | Ön görüntüleyicilerin 100 abonelik tanımlayıcılarına bir tane ekleyin. Bu beyaz listeye alınan abonelikler, yayımlandıktan sonra, yayımlanmaya başladıktan sonra teklif erişimine sahip olur. |
| **Faydalı bağlantılar**          | Belgeleri, sürüm notlarını, SSS vb. belgelerine URL 'Ler ekleyin. |
| **Önerilen Kategoriler\*** | Birincil ve ikincil kategori dahil olmak üzere en fazla iki (2) kategori seçin (isteğe bağlı). Her birincil ve/veya ikincil kategori için en fazla iki (2) alt kategori seçin. Alt kategori seçilmezse, teklif yalnızca seçili kategori üzerinde bulunabilir olmaya devam eder. |
|  |  |


## <a name="marketing-artifacts-section"></a>Pazarlama yapıtları bölümü

Bu ikinci bölüm üç alt bölüme bölünür: **logolar**, **ekran görüntüsü**ve **videolar**. Amblemler yalnızca gerekli olan pazarlama yapıtlarına yöneliktir, ancak en iyi müşteri için tüm müşteriler son derece önerilir. 

![Sanal makineler için yeni teklif formundaki Market sekmesinin pazarlama yapıları bölümü](./media/publishvm_009.png)

Aşağıdaki tabloda bu alanların amacı ve içeriği açıklanmaktadır. Gerekli alanlar bir yıldız işareti (*) ile tanımlanır.

|  **Alan**                |     **Açıklama**                                                          |
|  ---------                |     ---------------                                                          |
| *Ların*  |  |
| **Küçük\***                 | 40x40 pixel. ico bit eşlem                                                      |
| **Medium\***                | 90x90 pixel. ico bit eşlem                                                      |
| **Büyük\***                 | 115x115 pixel. ico bit eşlem                                                   |
| **Geniş\***                  | 255x115 pixel. ico bit eşlem                                                    |
| **Hero**                  | 815x290 bit eşlem.  İsteğe bağlı, ancak karşıya yüklendikten sonra Hero simgesi silinemez. |
| *Ekran görüntüleri*  | İsteğe bağlı, ancak SKU başına en fazla beş ekran görüntüsü. |
| **Adı**                  | Ad veya başlık <!-- TODO - max char length? none specified in UI -->                               |
| **Görüntü**                 | Ekran yakalama görüntüsü, 533x324 piksel                                         |
| *Videolar*  |  |
| **Adı**                  | Ad veya başlık  <!-- TODO - max char length? -->                              |
| **Bağlantısının**                  | YouTube veya Vimeo 'da barındırılan video URL 'SI                                        |
| **Resimler**             | 533x324 bit eşlem                                                               |
|   |   |

### <a name="logo-guidelines"></a>Logo yönergeleri

<!-- TD: It seems like this section could be better located in some common area, maybe an AMP Marketing/Design section 
+1 this should all be in a common area and referenced from here to that location.-->

Bulut İş Ortağı Portalı yüklenen tüm logolar, yönergeleri izlemelidir:

*  Azure tasarımının basit bir renk paleti vardır. Logonuz üzerinde birincil ve ikincil renk sayısını düşük tutun.
*  Azure portal Tema renkleri beyaz ve siyahtır. Bu nedenle, logolarınızın arka plan rengi olarak bu renkleri kullanmaktan kaçının. Logolarınızın Azure portal anlamlı hale getirmek için bir renk kullanın. Basit birincil renkleri öneririz. Saydam arka plan kullanıyorsanız, logoların/metnin beyaz veya siyah veya mavi olmadığından emin olun.
*  Logonuz üzerinde gradyan arka plan kullanmayın.
*  Logo üzerinde metin (şirketinizin veya marka adınız bile) yerleştirmekten kaçının. Logonuzun görünüm "düz" olmalıdır ve gradyanları kullanmaktan kaçınmalıdır.
*  Logoyu uzamayın.

#### <a name="hero-logo"></a>Kahraman logosu

Hero logosu isteğe bağlıdır; Ancak karşıya yüklendikten sonra Hero simgesi silinemez.  Hero logosu simgesi şu yönergeleri izlemelidir:

*  Hero simgeleri için siyah, beyaz ve saydam arka planlara izin verilmez.
*  Hero simgesinin arka planı olarak herhangi bir açık renk kullanmaktan kaçının.  Yayımcının görünen adı, plan başlığı ve teklif uzun Özeti beyaz yazı tipi renginde görüntülenir ve arka planda bir süre önce gelmelidir.
*  Hero logosunu tasarlarken çoğu metni kullanmaktan kaçının.  Yayımcı adı, plan başlığı, teklif uzun Özeti ve bir oluştur düğmesi, teklif listesi sırasında Hero simgesinin içine programlı bir şekilde katıştırılır. 
* Hero simgenin sağ tarafına, 415x100 piksel boyutunuzu ve soldan 370 piksel sapmasını dahil kullanılmamış bir dikdörtgen ekleyin.  

Örnek olarak, aşağıdaki Hero simgesi Azure Container Service içindir.  <!-- TD: It would be nice to have the raw bitmap, e.g.before and after embedding. -->

![Azure Container Service için örnek Hero simgesi](./media/publishvm_010.png)


### <a name="marketing-information-example"></a>Pazarlama bilgileri örneği 

Aşağıdaki görüntüde, pazarlama bilgilerinin Microsoft Windows Server ana ürün sayfasında nasıl görüntülendiği gösterilmektedir.

![Microsoft Windows Server için örnek ürün sayfası](./media/publishvm_011.png)


## <a name="lead-management-section"></a>Lider yönetimi bölümü

Üçüncü bölüm, Azure Market teklifinizden oluşturulan müşteri adaylarını toplamanıza olanak sağlar. Bu müşteri adayı bilgileri için aşağıdaki depolama seçeneklerini sunar (bir açılan listesinden).

* **Hiçbiri** -varsayılan, müşteri adayı bilgileri toplanmaz.
* Azure tablosu-bir bağlantı dizesi tarafından belirtilen Azure tablosuna yazıldı.
* Dynamics CRM Online-bir URL ve kimlik doğrulama kimlik bilgileriyle belirtilen [Microsoft Dynamics 365 online](https://dynamics.microsoft.com/) örneğine yazılır.
* HTTPS uç noktası-belirtilen HTTPS uç noktasına JSON yükü olarak yazılır.
* Marketo-belirtilen [Marketo](https://www.marketo.com/) örneğine yazılır, sunucu kimliği, bu kimlik ve form kimliği ile belirtilir.
* Salesforce-bir nesne tanımlayıcısıyla belirtilen [Salesforce](https://www.salesforce.com/) veritabanına yazılır.

Teklifinizi başarıyla yayımladıktan sonra, müşteri adayı bağlantısı onaylanır ve yapılandırılmış hedefe otomatik olarak bir test lideri gönderilir. Müşteri Yönetim mimarinizde her değişiklik yapıldığında, müşteri adayı bilgileri sürekli olarak yönetilmelidir ve bu ayarlar hemen güncel olmalıdır.

<!-- TD: For more info, see [Need a topic on lead information and processing that mimics the Appendix of the VM Pub Guide]. -->

## <a name="legal-section"></a>Yasal Bölüm

Bu son bölüm, her teklif için gereken yasal belgeleri sağlamanıza olanak sağlar.  

|  **Alan**                    |     **Açıklama**                                        |
|  ---------                    |     ---------------                                        |
| **Gizlilik Ilkesi URL 'SI\***      | Postalanan Gizlilik ilkenizin URL 'SI                          |
| **Standart Sözleşme mi kullanıyorsunuz?\***  |   |
| **Kullanım koşulları\***            | düz metin veya basit HTML olarak ilke.                       |
|  |  |


## <a name="next-steps"></a>Sonraki adımlar

Sonraki [destek](./cpp-support-tab.md) sekmesinde teklifiniz için teknik ve Kullanıcı Destek kaynakları sağlarsınız.
