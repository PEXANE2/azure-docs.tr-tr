---
title: Azure'da Cloudyn bütçelerini yönetme
description: Bu makale, Cloudyn'de hızlı bir şekilde bütçe oluşturmanıza ve bu bütçeleri yönetmeye başlamanıza yardımcı olur.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 01/24/2020
ms.topic: conceptual
ms.service: cost-management-billing
ms.reviewer: vitavor
ms.custom: seodec18
ms.openlocfilehash: 91a1027add615c71784b6be1261fab97aadd9f3a
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76769999"
---
# <a name="manage-azure-budgets-with-cloudyn"></a>Cloudyn ile Azure bütçelerini yönetme

Bütçeler ve bütçe tabanlı uyarılar oluşturmak, bulut idaresi ve sorumluluğu konusunda ilerleme sağlamanıza yardımcı olabilir. Bu makale, Cloudyn'de hızlı bir şekilde bütçe oluşturmanıza ve bu bütçeleri yönetmeye başlamanıza yardımcı olur.

Kurumsal veya MSP hesabınız varsa hiyerarşik maliyet varlığı yapınızı kullanarak farklı iş birimlerine, departmanlara veya diğer maliyet varlıklarına aylık bütçe kotaları atayabilirsiniz. Premium hesabınız varsa bulut harcamalarınızın tamamına uygulanan bütçe yönetimi işlevlerinden faydalanabilirsiniz. Tüm bütçeler el ile atanır.

Atanan bütçeleri temel alarak bütçenizin kullanılan bölümüne göre eşik uyarıları oluşturabilir ve her bir eşiğin önem derecesini tanımlayabilirsiniz.

Bütçe raporları atanan bütçeyi gösterir. Kullanıcılar, yaptıkları harcamaların zaman içindeki tüketimlerine göre fazla, az veya uygun olup olmadığını görüntüleyebilir. Raporun üstünde yer alan **Alanları Göster/Gizle**'yi seçerek maliyeti, bütçeyi, birikmiş maliyeti ve toplam bütçeyi görüntüleyebilirsiniz.

Azure Maliyet Yönetimi, Cloudyn'e benzer işlevler sunar. Azure Maliyet Yönetimi, yerel Azure maliyet yönetimi çözümüdür. Maliyet analizi yapmanıza, bütçe oluşturup yönetmenize, verileri dışarı aktarmanıza ve tasarruf önerilerini gözden geçirip gerekli eylemleri gerçekleştirmenize yardımcı olur. Maliyet Yönetimi bütçeleri hakkında daha fazla bilgi için bkz. [Bütçe oluşturma ve yönetme](../costs/tutorial-acm-create-budgets.md).

## <a name="create-budgets"></a>Bütçe oluşturma

Bütçeyi geçerli mali yıl için oluşturur ve belirli bir varlığa atarsınız.

Bütçe oluşturmak ve bir varlığa atamak için:

1. **Maliyetler** &gt; **Maliyet Yönetimi** &gt; **Bütçe** yolunu izleyin.
2. Bütçe Yönetimi sayfasının **Varlıklar** bölümünde bütçe oluşturmak istediğiniz varlığı seçin.
3. Bütçe yılında bütçeyi oluşturmak istediğiniz yılı seçin.
4. Her ay için bir bütçe değeri ayarlayın. İşiniz bittiğinde **Kaydet**'e tıklayın.
Bu örnekte, Haziran 2018 için aylık bütçe 135.000 ABD doları olarak ayarlanmıştır. Yılın toplam bütçesi 1.615.000,00 ABD dolarıdır.
![Aylık bütçeyi belirlediğiniz bir bütçe sayfası oluşturun](./media/manage-budgets/set-budget.png)


Yıllık bütçe dosyasını içeri aktarmak için:

1. **Eylemler** bölümünde **Dışarı aktar**'ı seçerek bütçe için temel olarak kullanılacak boş CSV şablonu indirin.
2. CSV dosyasına bütçe girişlerinizi ekleyip yerel olarak kaydedin.
3. **Eylemler** bölümünde **İçeri aktar**’ı seçin.
4. Kaydettiğiniz dosyayı seçin ve ardından **Tamam**'a tıklayın.

Tamamlanmış bütçenizi CSV dosyası olarak dışarı aktarmak için **Eylemler** bölümünde **Dışarı aktar**'ı seçip dosyayı indirin.

## <a name="view-budget-in-reports"></a>Bütçeyi raporlarda görüntüleme

İşlem tamamlandığında bütçeniz **Maliyetler** &gt; **Maliyet Analizi** altındaki çoğu Maliyet raporunda ve Zaman İçinde Maliyet ve Bütçe raporunda gösterilir. İsterseniz **Eylemler**'i kullanarak raporları bütçe eşiklerine göre zamanlayabilirsiniz.

Aşağıda örnek bir Maliyet Analizi raporu verilmiştir. Yılın başlangıcından bu yana iş yüküne ve kullanım türlerine göre toplam bütçeyi ve maliyeti göstermektedir.

![Bütçeye sahip Maliyet Analizi raporu örneği](./media/manage-budgets/cost-analysis-budget-example.png)

Bu örnekte geçerli tarihin 22 Haziran olduğunu kabul edelim. Haziran 2018'in maliyeti 71.611,28 ABD doları olmuştur ve aylık bütçe 135.000 ABD dolarıdır. Ay sonuna kadar daha sekiz günlük harcama olduğundan maliyet, aylık bütçenin oldukça altındadır.

Raporu görüntülemenin bir başka yolu da bütçenize göre birikmiş maliyete göz atmaktır. Birikmiş maliyetleri görmek için **Alanları Göster/Gizle**'nin altında **Birikmiş Maliyet**'i ve **Toplam Bütçe**'yi seçin. Yılın başlangıcından bu yana birikmiş maliyeti gösteren örnek aşağıda verilmiştir.

![Zaman İçinde Maliyet ve Bütçe raporunda gösterilen birikmiş maliyet ve toplam bütçe örneği](./media/manage-budgets/accumulated-budget.png)

İleride bir noktada birikmiş maliyetiniz, bütçenizi aşabilir. Grafik görünümünü _çizgi_ olarak değiştirirseniz bunu daha kolay görebilirsiniz.

![Aya göre maliyet raporunda çizgi grafik olarak gösterilen bütçe](./media/manage-budgets/budget-line.png)

## <a name="create-budget-alerts-for-a-filter"></a>Filtre için bütçe uyarıları oluşturma

Yukarıdaki örnekte birikmiş maliyetin bütçeye yaklaştığını görebilirsiniz. Harcama tutarı bütçenize yaklaştığında veya onu geçtiğinde bildirim almak için otomatik bütçe uyarıları oluşturabilirsiniz. Uyarı temel olarak eşik değerine sahip olan bir zamanlanmış rapordur. Bütçe uyarı eşiği ölçümleri şunları içerir:

- Kalan maliyet-bütçe: Para birimi değeriyle bir eşik belirtmek için
- Maliyet yüzdesi-bütçe: Yüzde değeriyle bir eşik belirtmek için

Şimdi örneği inceleyelim.

Zaman İçinde Maliyet ve Bütçe raporunda **Eylemler**'e tıklayıp **Rapor zamanla**'yı seçin. Eşik sekmesinde bir eşik ölçümü seçin. Örneğin, **Maliyet yüzdesi-bütçe**. Bir uyarı türü seçin ve bütçenin yüzde değerini girin. Yalnızca bir kez bildirim almak istiyorsanız **Ardışık uyarı sayısı**'nı seçin ve _1_ yazın. **Kaydet**’e tıklayın.

![Bu raporu kaydet veya zamanla kutusunda bütçe uyarısı oluşturma](./media/manage-budgets/budget-alert.png)

## <a name="next-steps"></a>Sonraki adımlar

- Cloudyn için ilk öğreticiyi önceden tamamlamadıysanız, [Kullanım ve maliyetleri gözden geçirme](tutorial-review-usage.md) bölümünden bilgi edinin.
- [Cloudyn'de bulunan raporlar](use-reports.md) hakkında daha fazla bilgi edinin.
