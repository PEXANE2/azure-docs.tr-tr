---
title: Azure 'da Cloudyn bütçelerini yönetme
description: Bu makalede, hızlı bir şekilde bütçeleri oluşturun ve bunları Cloudyn'de yönetmeye başlama yardımcı olur.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 05/20/2019
ms.topic: conceptual
ms.service: cost-management-billing
manager: vitavor
ms.custom: seodec18
ms.openlocfilehash: 9d7d0e049d3c35aab56145beb94c8e41e56c5785
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74219091"
---
# <a name="manage-azure-budgets-with-cloudyn"></a>Cloudyn ile Azure bütçelerini yönetin

Ayar bütçelerini'kurmak ve bütçe tabanlı bulut idare ve Sorumluluk geliştirmek için Yardım uyarır. Bu makalede, hızlı bir şekilde bütçeleri oluşturun ve bunları Cloudyn'de yönetmeye başlama yardımcı olur.

Bir kuruluş ya da MSP hesabınız varsa, farklı iş birimleri, Departmanlar veya herhangi bir maliyet varlık aylık bütçe kotaları atamak için hiyerarşik maliyet varlık yapınızı kullanabilirsiniz. Premium hesabı varsa, daha sonra tüm bulut harcamalarını için uygulanan bütçe yönetimi işlevi kullanabilirsiniz. Tüm bütçelerini el ile atanır.

Atanan bütçeleri bağlı olarak, eşik uyarılarının tüketilen bütçenizi yüzdesine göre ayarlayabilir ve her eşiği önem derecesini tanımlayın.

Bütçe raporlar atanan bütçe gösterir. Kullanıcılar, kendi harcama üzerinden, altında veya par tüketimi için zamana sahip olduğunda görüntüleyebilir. Bütçe raporunun en üstünde **alanları göster/gizle** ' yi seçtiğinizde maliyet, bütçe, birikmiş maliyet veya toplam bütçeyi görüntüleyebilirsiniz.

Azure Maliyet Yönetimi, Cloudyn'e benzer işlevler sunar. Azure Maliyet Yönetimi, yerel Azure maliyet yönetimi çözümüdür. Maliyet analizi yapmanıza, bütçe oluşturup yönetmenize, verileri dışarı aktarmanıza ve tasarruf önerilerini gözden geçirip gerekli eylemleri gerçekleştirmenize yardımcı olur. Maliyet yönetiminin bütçeleri hakkında daha fazla bilgi için bkz. [bütçeleri oluşturma ve yönetme](tutorial-acm-create-budgets.md).

## <a name="create-budgets"></a>Bütçeleri oluşturun

Bütçe oluşturduğunuzda, mali yıl için ayarlayın ve belirli bir varlığa uygulanır.

Bütçe oluşturun ve bunu bir varlığa atamak için:

1. **Maliyetler** &gt; **maliyet yönetimi** &gt; **bütçesine**gidin.
2. Bütçe yönetimi sayfasında, **varlıklar**' ın altında, bütçeyi oluşturmak istediğiniz varlığı seçin.
3. Bütçe yılda bütçe oluşturmak istediğiniz yıl seçin.
4. Her ay için bir bütçe değere ayarlayın. İşiniz bittiğinde **Kaydet**' e tıklayın.
Bu örnekte, Haziran 2018 tarihinden itibaren aylık bütçe 135.000 için ayarlanır. Yıl için toplam bütçe $1,615,000.00 olacaktır.
her ay için bir bütçe ayarladığınız bir bütçe sayfası oluşturmak ![](./media/manage-budgets/set-budget.png)


Yıllık bütçenin bir dosyayı içe aktarmak için:

1. **Eylemler**' in altında, bütçe için temel olarak kullanmak üzere boş bir CSV şablonunu Indirmek Için **dışarı aktar** ' ı seçin.
2. Bütçe girişlerinizi içeren CSV dosyası doldurun ve yerel olarak kaydedin.
3. **Eylemler**altında **içeri aktar**' ı seçin.
4. Kayıtlı dosyanızı seçin ve ardından **Tamam**' a tıklayın.

Tamamlanan bütçenizi bir CSV dosyası olarak dışa aktarmak için, **Eylemler**altında, dosyayı Indirmek Için **dışarı aktar** ' ı seçin.

## <a name="view-budget-in-reports"></a>Rapor görünümü bütçede

Bu tamamlandığında, bütçeniz **maliyetler** &gt; **Maliyet Analizi** kapsamında ve zaman içinde maliyet ile bütçeyi raporlamak için maliyet raporlarının çoğunda yer aldığı maliyet raporlarında gösterilir. Ayrıca, **eylemleri**kullanarak bütçe eşiklerine göre raporlar zamanlayabilirsiniz.

Maliyet analizi raporu örneği aşağıda verilmiştir. Bu yılın başlangıcından bu yana toplam bütçe ve iş yükü ve kullanım türleri maliyeti gösterir.

![Bütçe ile örnek maliyet analizi raporu](./media/manage-budgets/cost-analysis-budget-example.png)

Bu örnekte, geçerli tarihi 22 Temmuz olduğunu varsayalım. Haziran 2018 için $ $135.000 aylık bütçeye kıyasla 71,611.28 maliyetidir. Sekiz gün ve ay sonuna kadar önce harcadığınız hala olduğundan maliyet aylık bütçe daha çok düşüktür.

Raporu görüntülemek için başka bir yolu, bütçenizi birikmiş maliyeti veya aramaktır. Birikmiş maliyetleri görmek için **alanları göster/gizle**bölümünde **Birikmiş maliyet** ve **Toplam bütçe**' ı seçin. Yılın başlangıcından bu yana birikmiş maliyetini gösteren bir örnek aşağıda verilmiştir.

![Örnek, birikmiş maliyet ve zaman Içinde Bütçe karşılaştırması raporunda gösterilen toplam bütçe](./media/manage-budgets/accumulated-budget.png)

Süre gelecekte birikmiş maliyetini bütçenizi aştığında. Grafik görünümünü _satır_ türü olarak değiştirirseniz daha kolay görebilirsiniz.

![Ay bazında maliyeti çizgi grafikte gösterilen bütçe](./media/manage-budgets/budget-line.png)

## <a name="create-budget-alerts-for-a-filter"></a>Bütçe uyarılar için bir filtre oluşturun

Önceki örnekte birikmiş maliyeti bütçesi yaklaşıldığında görebilirsiniz. Böylece yaklaşım harcama olduğunda bildirim alırsınız veya bütçenizi aştığında otomatik bütçe uyarılar oluşturabilirsiniz. Aslında, uyarı, zamanlanmış bir raporu bir eşik ile belirtir. Bütçe uyarısı eşiği ölçümler şunları içerir:

- Kalan Maliyet ve bütçe, bir para birimi değeri eşiği belirtmek için – karşılaştırması
- -Bir yüzde değeri eşiği belirtmek için bütçe – maliyet yüzdesi

Bir örneğe göz atalım.

Maliyet ve zaman Içinde bütçe raporunda, **Eylemler** ' i ve ardından **rapor zamanla**' yı seçin. Eşik sekmesinde, bir eşiği ölçümünü seçin. Örneğin, **Maliyet yüzdesi vs bütçe**. Bir uyarı türünü seçin ve bütçe yüzdesi değeri girin. Yalnızca bir kez bildirim almak istiyorsanız **ardışık uyarı sayısı** ' nı seçin ve _1_yazın. **Save (Kaydet)** düğmesine tıklayın.

![Bütçe uyarısı kaydetme ya da bu raporu kutusunda zamanlama oluşturma](./media/manage-budgets/budget-alert.png)

## <a name="next-steps"></a>Sonraki adımlar

- Cloudyn için ilk öğreticiyi tamamlamadıysanız [kullanımı ve maliyetleri gözden geçirin](tutorial-review-usage.md).
- [Cloudyn 'de bulunan raporlar](use-reports.md)hakkında daha fazla bilgi edinin.
