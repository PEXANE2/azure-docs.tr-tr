---
title: Azure Marketi için Bulut İş Ortağı Portalında Sanal makine Market sekmesi
description: Azure Marketi VM teklifi oluştururken kullanılan Market sekmesini açıklar.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: dsindona
ms.openlocfilehash: 156a405d93aeac27f27fe617e1a5485044309846
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81273826"
---
# <a name="virtual-machine-marketplace-tab"></a>Sanal makine Pazar yeri sekmesi

> [!IMPORTANT]
> 13 Nisan 2020'den itibaren Azure Sanal Makine tekliflerinizin yönetimini İş Ortağı Merkezi'ne taşımaya başlayacağız. Geçişten sonra, Tekliflerinizi İş Ortağı Merkezi'nde oluşturur ve yönetirsiniz. Geçirilen tekliflerinizi yönetmek için [Azure Sanal Makine Oluştur teklifindeki](https://aka.ms/CreateAzureVMoffer) yönergeleri izleyin.

**Yeni Teklif** sayfasının **Pazar sekmesi,** potansiyel müşterilerinize pazarlama, satış ve yasal bilgi ve anlaşmaları sağlamanızı ve pazardan oluşturulan müşteri adaylarını yönetmenize olanak tanır. Bu uzun form dört bölüme ayrılmıştır: **Genel Bakış**, **Pazarlama Eserler**, **Kurşun Yönetimi**, ve **Yasal**.


## <a name="overview-section"></a>Genel Bakış bölümü
Bu bölümde, Azure Market I' nizle ilgili genel bilgileri girersiniz.  Alan adına eklenen bir yıldız işareti (*) bunun gerekli olduğunu gösterir.

![Sanal makineler için Market sekmesine genel bakış bölümü](./media/publishvm_008.png)

Aşağıdaki tabloda bu alanların amacı ve içeriği açıklanmaktadır. Gerekli alanlar yıldız işareti (*) tarafından suçlanır.

|  **Alan**                |     **Açıklama**                                                          |
|  ---------                |     ---------------                                                          |
| **Başlık\***                 | Teklifin başlığı, genellikle uzun, resmi adı. Bu başlık pazarda belirgin bir şekilde görüntülenir.  Maksimum uzunluğu 50 karakter. |
| **Özet\***               | Çözümün kısa amacı veya işlevi.  Maksimum uzunluğu 100 karakter. |
| **Uzun Özet\***          | Çözümün amacı veya işlevi.  Maksimum uzunluğu 256 karakter. |
| **Açıklama\***           | Çözümün açıklaması.  Maksimum uzunluğu 3000 karakter, basit HTML biçimlendirme destekler. |
| **Microsoft CSP Bayi kanalı\*** | Bulut Çözüm Sağlayıcıları (CSP) iş ortağı kanal tercihi artık kullanılabilir.  Microsoft CSP iş ortağı kanalları aracılığıyla teklifinizi pazarlama hakkında daha fazla bilgi için lütfen [Bulut Çözüm Sağlayıcıları'na](../../cloud-solution-providers.md) bakın. |
| **Pazarlama Tanımlayıcısı\***  | Bu teklifle ilişkilendirilen benzersiz bir URL, genellikle kuruluş ve çözüm adınızı, maksimum uzunluk 50 karakteri içerir.  Örneğin: <br/> `https://azuremarketplace.microsoft.com/marketplace/apps/contoso.sampleApp`  |
| **Abonelik IYT'lerini Önizleme\*** | Bir ila 100 abonelik tanımlayıcıları previewers ekleyin. Bu beyaz listedeki abonelikler, yayınlanmadan önce, yayınlanmadan önce teklife erişebilir. |
| **Faydalı Linkler**          | Belgelere URL'ler, sürüm notları, SSS'ler vb. ekleyin. |
| **Önerilen Kategoriler\*** | Birincil ve ikincil kategori (isteğe bağlı) dahil olmak üzere en fazla iki (2) kategori seçin. Her birincil ve/veya ikincil kategori için en fazla iki (2) alt kategori seçin. Hiçbir alt kategori seçili değilse, sunduğunuz öneri yalnızca seçili kategoride bulunabilir olacaktır. |
|  |  |


## <a name="marketing-artifacts-section"></a>Pazarlama Eserleri bölümü

Bu ikinci bölüm üç alt bölüme ayrılmıştır: **Logolar**, **Ekran Görüntüsü**, ve **Videolar**. Logolar sadece gerekli pazarlama eserler, ancak tüm son derece en iyi müşteri itiraz için tavsiye edilir. 

![Sanal makineler için Yeni Teklif formunda Pazar sekmesinin Pazarlama Eserleri bölümü](./media/publishvm_009.png)

Aşağıdaki tabloda bu alanların amacı ve içeriği açıklanmaktadır. Gerekli alanlar yıldız işareti (*) tarafından suçlanır.

|  **Alan**                |     **Açıklama**                                                          |
|  ---------                |     ---------------                                                          |
| *Logo*  |  |
| **Küçük\***                 | 40x40 piksel .ico bit map                                                      |
| **Medium\***                | 90x90 piksel .ico bit map                                                      |
| **Büyük\***                 | 115x115 piksel .ico bit haritası                                                   |
| **Geniş\***                  | 255x115 piksel .ico bit haritası                                                    |
| **Kahraman**                  | 815x290 bit haritası.  İsteğe bağlı, ancak bir kez kahraman simgesi yüklendiğinde silinemez. |
| *Ekran görüntüleri*  | İsteğe bağlı, ancak SKU başına en fazla beş ekran görüntüsü. |
| **Adı**                  | Ad veya başlık <!-- TODO - max char length? none specified in UI -->                               |
| **Görüntü**                 | Ekran yakalama görüntüsü, 533x324 piksel                                         |
| *Videolar*  |  |
| **Adı**                  | Ad veya başlık  <!-- TODO - max char length? -->                              |
| **Bağlantı**                  | YouTube veya Vimeo'da barındırılan video URL'si                                        |
| **Küçük**             | 533x324 bit haritası                                                               |
|   |   |

### <a name="logo-guidelines"></a>Logo yönergeleri

<!-- TD: It seems like this section could be better located in some common area, maybe an AMP Marketing/Design section 
+1 this should all be in a common area and referenced from here to that location.-->

Bulut İş Ortağı Portalı'na yüklenen tüm logolar aşağıdaki kurallara uymalıdır:

*  Azure tasarımının basit bir renk paleti vardır. Logonuzdaki birincil ve ikincil renk sayısını düşük tutun.
*  Azure portalının tema renkleri beyaz ve siyahtır. Bu nedenle logolarınızın arka plan rengi olarak bu renkleri kullanmaktan kaçının. Logolarınızı Azure portalında öne çıkarabilecek bazı renkler kullanın. Basit birincil renkleri öneririz. Saydam arka plan kullanıyorsanız, logoların/metnin beyaz veya siyah veya mavi olmadığından emin olun.
*  Logonuzda degrade arka plan kullanmayın.
*  Logoya metin (hatta şirketiniz veya marka adınızı) yerleştirmekten kaçının. Görünüm ve logo hissediyorum "düz" olmalı ve degradeler kaçınmalısınız.
*  Logoyu esnetmayın.

#### <a name="hero-logo"></a>Kahraman logosu

Hero logosu isteğe bağlıdır; ancak yüklendikten sonra kahraman simgesi silinemez.  Hero logo simgesi yönergeleri izlemelidir:

*  Kahraman simgeleri için siyah, beyaz ve saydam arka planlara izin verilmez.
*  Kahraman simgesinin arka planı olarak herhangi bir açık renk kullanmaktan kaçının.  Publisher ekran adı, plan başlığı ve teklif uzun özeti beyaz yazı tipi renginde görüntülenir ve arka plana karşı öne çıkmalıdır.
*  Kahraman logosunu tasarlarken çoğu metni kullanmaktan kaçının.  Teklif listelendiğinde yayıncı adı, plan başlığı, teklif uzun özeti ve oluşturma düğmesi kahraman simgesinin içine programlı olarak gömülür. 
* Kahraman simgenizin sağ tarafına 415x100 piksel boyutunda ve soldan 370 px ofset olarak kullanılmayan bir dikdörtgen ekleyin.  

Örnek olarak, aşağıdaki kahraman simgesi Azure Kapsayıcı Hizmeti içindir.  <!-- TD: It would be nice to have the raw bitmap, e.g.before and after embedding. -->

![Azure Kapsayıcı Hizmeti için örnek kahraman simgesi](./media/publishvm_010.png)


### <a name="marketing-information-example"></a>Pazarlama bilgileri örneği 

Aşağıdaki resim, pazarlama bilgilerinin Microsoft Windows Server ana ürün sayfasında nasıl görüntülendiğini göstermektedir.

![Microsoft Windows Server için örnek ürün sayfası](./media/publishvm_011.png)


## <a name="lead-management-section"></a>Müşteri Yönetimi bölümü

Üçüncü bölüm, Azure Marketi tekliflerinizden oluşturulan müşteri müşteri adaylarını toplamanızı sağlar. Bu müşteri adayı bilgileri için aşağıdaki depolama seçeneklerini (açılır listeden) sunar.

* **Yok** - varsayılan, müşteri adayı bilgileri toplanmaz.
* Azure Tablosu - bir bağlantı dizesi tarafından belirtilen Azure tablosuna yazılır.
* Dynamics CRM Online - Bir URL ve kimlik doğrulama kimlik bilgileri tarafından belirtilen [Microsoft Dynamics 365 Çevrimiçi](https://dynamics.microsoft.com/) örneğine yazılır.
* HTTPS Endpoint - belirtilen HTTPS bitiş noktasına JSON yükü olarak yazılır.
* Marketo - sunucu kimliği, munchkin kimliği ve form kimliği tarafından belirtilen belirtilen [Marketo](https://www.marketo.com/) örneğine yazılır.
* Salesforce - bir nesne Tanımlayıcısı tarafından belirtilen [Salesforce](https://www.salesforce.com/) veritabanına yazılır.

Teklifinizi başarıyla yayımladıktan sonra, müşteri adayı bağlantısı doğrulanır ve test müşteri adayı otomatik olarak yapılandırılan hedefe gönderilir. Müşteri bilgileri sürekli olarak yönetilmeli ve müşteri yönetimi mimarinizde değişiklik yapıldığında bu ayarların hemen güncellenmesi gerekir.

<!-- TD: For more info, see [Need a topic on lead information and processing that mimics the Appendix of the VM Pub Guide]. -->

## <a name="legal-section"></a>Yasal bölüm

Bu son bölüm, her teklif için gerekli yasal belgeleri sağlamanızı sağlar.  

|  **Alan**                    |     **Açıklama**                                        |
|  ---------                    |     ---------------                                        |
| **Gizlilik Politikası URL'si\***      | Yayınlanan gizlilik politikanızın URL'si                          |
| **Standart Sözleşme'yi mi kullanıyorsun?\***  |   |
| **Kullanım koşulları\***            | düz metin veya basit HTML olarak politika.                       |
|  |  |


## <a name="next-steps"></a>Sonraki adımlar

Bir sonraki [Destek](./cpp-support-tab.md) sekmesinde, teklifiniz için teknik ve kullanıcı destek kaynakları sağlarsınız.
