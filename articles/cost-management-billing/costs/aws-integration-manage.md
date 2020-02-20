---
title: Azure Maliyet Yönetimi'nde AWS maliyetlerini ve kullanımını yönetme
description: Bu makale, Maliyet Yönetimi'ndeki maliyet analizlerini ve bütçeleri kullanarak AWS maliyetlerinizi ve kullanımınızı yönetme konusunda yardımcı olur.
author: bandersmsft
ms.author: banders
ms.date: 02/12/2020
ms.topic: conceptual
ms.service: cost-management-billing
ms.reviewer: matrive
ms.custom: ''
ms.openlocfilehash: fd4d4a16642e764276d49dd732a785be48197d01
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77200036"
---
# <a name="manage-aws-costs-and-usage-in-azure"></a>Azure'da AWS maliyetlerini ve kullanımını yönetme

Azure Maliyet Yönetimi için AWS Maliyet ve Kullanım raporu tümleştirmesini ayarlayıp yapılandırdıktan sonra AWS maliyetlerinizi ve kullanımınızı yönetmeye başlayabilirsiniz. Bu makale, Maliyet Yönetimi'ndeki maliyet analizlerini ve bütçeleri kullanarak AWS maliyetlerinizi ve kullanımınızı yönetme konusunda yardımcı olur.

Tümleştirmeyi henüz yapılandırmadıysanız bkz. [AWS Kullanım raporu tümleştirmesini ayarlama ve yapılandırma](aws-integration-set-up-configure.md).

_Başlamadan önce_: Maliyet analizi konusunda bilginiz yoksa [Maliyet analiziyle maliyetleri keşfetme ve analiz etme](quick-acm-cost-analysis.md) hızlı başlangıcına bakın. Azure'daki bütçeler konusunda bilginiz yoksa bkz. [Azure bütçesi oluşturma ve yönetme öğreticisi](tutorial-acm-create-budgets.md).

## <a name="view-aws-costs-in-cost-analysis"></a>Maliyet analizinde AWS maliyetlerini görüntüleme

AWS maliyetleri aşağıdaki kapsamlarda Maliyet Analizi'nde kullanılabilir:

- Yönetim grubu altındaki AWS bağlı hesapları
- AWS bağlı hesaplarıyla ilgili maliyetler
- AWS birleştirilmiş hesaplarla ilgili maliyetler

Aşağıdaki bölümlerde her birine ait olan maliyet ve kullanım verilerini görebilmeniz için kapsamları nasıl kullanacağınız açıklanmıştır.

### <a name="view-aws-linked-accounts-under-a-management-group"></a>Yönetim grubu altındaki AWS bağlı hesaplarını görüntüleme

Maliyetleri yönetim grubu kapsamı kullanarak görüntüleme, farklı aboneliklerden ve bağlı hesaplardan gelen toplu maliyetleri görmenin tek yoludur. Yönetim grubu kullanmak, bulutlar arası bir görünüm sunar.

Maliyet analizinde kapsam seçiciyi açın ve AWS bağlı hesaplarınızı barındıran yönetim grubunu seçin. Aşağıda Azure portalından örnek bir görüntü verilmiştir:

![Kapsam seç görünümü örneği](./media/aws-integration-manage/select-scope01.png)



Aşağıda maliyet analizinde yönetim grubunu Sağlayıcıya (Azure ve AWS) göre gruplanmış şekilde gösteren bir örnek verilmiştir.

![Maliyet analizinde üç aylık döneme ait Azure ve AWS maliyetlerini gösteren örnek](./media/aws-integration-manage/cost-analysis-aws-azure.png)

### <a name="view-aws-linked-account-costs"></a>AWS bağlı hesaplarıyla ilgili maliyetleri görüntüleme

AWS bağlı hesaplarıyla ilgili maliyetleri görüntülemek için kapsam seçiciyi açıp AWS bağlı hesabını seçin. Bağlı hesapların AWS bağlayıcısında tanımlandığı şekilde bir yönetim grubuyla ilişkilendirildiğini unutmayın.

Aşağıda AWS bağlı hesap kapsamını seçmeyi gösteren bir örnek verilmiştir.

![Kapsam seç görünümü örneği](./media/aws-integration-manage/select-scope02.png)



### <a name="view-aws-consolidated-account-costs"></a>AWS birleştirilmiş hesaplarla ilgili maliyetleri görüntüleme

AWS birleştirilmiş hesaplarla ilgili maliyetleri görüntülemek için kapsam seçiciyi açıp AWS birleştirilmiş hesabını seçin. Aşağıda AWS birleştirilmiş hesap kapsamını seçmeyi gösteren bir örnek verilmiştir.

![Kapsam seç görünümü örneği](./media/aws-integration-manage/select-scope03.png)



Bu kapsam, AWS birleştirilmiş hesabıyla ilişkili tüm AWS bağlı hesaplarının toplu bir görünümünü sağlar. Aşağıda bir AWS birleştirilmiş hesabının, hizmet adına göre gruplanan maliyetleri gösterdiği bir örnek verilmiştir.

![Maliyet analizinde AWS birleştirilmiş maliyetlerini gösteren örnek](./media/aws-integration-manage/cost-analysis-aws-consolidated.png)

### <a name="dimensions-available-for-filtering-and-grouping"></a>Filtreleme ve gruplama için kullanılabilen boyutlar

Aşağıdaki tabloda maliyet analizinde gruplama ve filtreleme için kullanılabilecek olan boyutlar verilmiştir.

| Boyut | Amazon CUR üst bilgisi | Kapsamlar | Yorumlar |
| --- | --- | --- | --- |
| Kullanılabilirlik alanı | lineitem/AvailabilityZone | Tümü |   |
| Konum | product/Region | Tümü |   |
| Ölçüm |   | Tümü |   |
| Ölçüm kategorisi | lineItem/ProductCode | Tümü |   |
| Ölçüm alt kategorisi | lineitem/UsageType | Tümü |   |
| İşlem | lineItem/Operation | Tümü |   |
| Kaynak | lineItem/ResourceId | Tümü |   |
| Kaynak türü | product/instanceType | Tümü | product/instanceType null ise lineItem/UsageType kullanılır. |
| ResourceGuid | Yok | Tümü | Azure ölçüm GUID değeri. |
| Hizmet adı | product/ProductName | Tümü | product/ProductName null ise lineItem/ProductCode kullanılır. |
| Hizmet katmanı |   |   |   |
| Abonelik Kimliği | lineItem/UsageAccountId | Birleştirilmiş hesap ve yönetim grubu |   |
| Abonelik adı | Yok | Birleştirilmiş hesap ve yönetim grubu | Hesap adları AWS Kuruluş API'si kullanılarak toplanır. |
| Etiket | resourceTags/\* | Tümü | _user:_ ön eki, bulutlar arası etiketlere izin vermek için kullanıcı tanımlı etiketlerden kaldırılmıştır. _aws:_ ön eki değiştirilmeden bırakılmıştır. |
| Faturalama hesabı kimliği | bill/PayerAccountId | Yönetim grubu |   |
| Faturalama hesabı adı | Yok | Yönetim grubu | Hesap adları AWS Kuruluş API'si kullanılarak toplanır. |
| Sağlayıcı | Yok | Yönetim grubu | AWS veya Azure. |

## <a name="set-budgets-on-aws-scopes"></a>AWS kapsamlarında bütçeleri ayarlama

Kuruluşunuzda maliyetleri önceden yönetmek ve sorumluluk bilinci sağlamak için bütçeleri kullanabilirsiniz. Bütçeler AWS birleştirilmiş hesabı ve AWS bağlı hesap kapsamlarında ayarlanır. Aşağıda Maliyet Yönetimi'ndeki bir AWS birleştirilmiş hesabına ait bütçe örnekleri gösterilmiştir:

![Birleştirilmiş AWS hesabı için bütçeleri gösteren örnek](./media/aws-integration-manage/budgets-aws-consolidated-account01.png)

## <a name="aws-data-collection-process"></a>AWS veri toplama işlemi

AWS bağlayıcısını ayarladıktan sonra veri toplama ve bulma işlemi başlar. Tüm kullanım verilerinin toplanması birkaç saat sürebilir. Süre şunlara bağlıdır:

- AWS S3 demeti içindeki CUR dosyalarını işlemek için gereken zaman.
- AWS birleştirilmiş hesabı ve AWS bağlı hesap kapsamlarını oluşturmak için gereken zaman.
- AWS'nin S3 demetindeki Maliyet ve Kullanım Raporu dosyalarına yazma süresi ve sıklığı

## <a name="aws-integration-pricing"></a>AWS tümleştirme fiyatlandırması

Her AWS bağlayıcısı 90 günlük ücretsiz deneme sunar. Genel Önizleme sırasında ücret alınmaz.

Liste fiyatı, aylık AWS maliyetlerinizin %1'inin altındadır. Her ay, önceki ay kullanılan tutarlara göre ödeme yaparsınız.

AWS API'lerine erişmek için ek ücret uygulanabilir.

## <a name="aws-integration-limitations"></a>AWS tümleştirme sınırlamaları

- Maliyet Yönetimi, birden fazla para birimi içeren maliyet raporlarını desteklemez. Birden fazla para birimine sahip bir kapsam seçtiğinizde hata iletisi görüntülenir.
- Bulut bağlayıcıları AWS GovCloud (US), AWS Gov veya AWS China desteği sunmaz.
- Maliyet Yönetimi yalnızca AWS _kullanım maliyetlerini_ gösterir. Vergiler, destek, para iadeleri, RI, krediler ve diğer ücret türleri desteklenmemektedir.

## <a name="troubleshooting-aws-integration"></a>AWS tümleştirmesiyle ilgili sorunları giderme

Sık karşılaşılan sorunları çözmek için aşağıdaki sorun giderme bilgilerini inceleyin.

### <a name="no-permission-to-aws-linked-accounts"></a>Bağlı AWS hesapları için izin yok

**Hata kodu:** _Yetkisiz_

AWS bağlı hesap maliyetlerine erişim izni almanın iki yolu vardır:

- AWS bağlı hesaplarına sahip yönetim grubuna erişim sağlama.
- Birinin AWS bağlı hesabına erişim izni vermesini isteme.

AWS bağlayıcı oluşturucu varsayılan olarak bağlayıcının oluşturduğu tüm nesnelerin sahibidir. Buna AWS birleştirilmiş hesabı ve AWS bağlı hesapları da dahildir.

Bağlayıcı ayarlarını doğrulayabilmek için en azından katkıda bulunan rolüne ihtiyacınız olacaktır, okuyucu rolü bağlayıcı ayarlarını doğrulayamaz

### <a name="collection-failed-with-assumerole"></a>AssumeRole bağlantısı başarısız oldu

**Hata kodu:** _FailedToAssumeRole_

Bu hata, Maliyet Yönetimi'nin AWS AssumeRole API'sini çağıramadığı anlamına gelir. Bu sorunun nedeni rol tanımıyla ilgili bir sorun olması olabilir. Aşağıdaki koşulların doğru olduğundan emin olun:

- Dış kimlik, rol tanımındaki ve bağlayıcı tanımındaki değerle aynı.
- Rol türü **Size veya 3. taraflara ait olan başka bir Azure hesabı** olarak ayarlanmış.
- **MFA gerektirme** işaretlenmemiş.
- AWS rolündeki güvenilir AWS hesabı _432263259397_.

### <a name="collection-failed-with-access-denied---cur-report-definitions"></a>Koleksiyon, Erişim Reddedildi hatasıyla başarısız oldu - CUR rapor tanımları

**Hata kodu:** _AccessDeniedReportDefinitions_

Bu hata, Maliyet Yönetimi'nin Maliyet ve Kullanım raporu tanımlarını göremediği anlamına gelir. Bu izin, CUR'un Azure Maliyet Yönetimi tarafından tanımlanan ve beklenen şekilde olduğunu doğrulamak için kullanılır. Bkz. [AWS'de Maliyet ve Kullanım raporu oluşturma](aws-integration-set-up-configure.md#create-a-cost-and-usage-report-in-aws).

### <a name="collection-failed-with-access-denied---list-reports"></a>Koleksiyon, Erişim Reddedildi hatasıyla başarısız oldu - Raporları listeleme

**Hata kodu:** _AccessDeniedListReports_

Bu hata, Maliyet Yönetimi'nin CUR'un olduğu S3 demetindeki nesneyi listeleyemediği anlamına gelir. AWS IAM ilkesi demek ve demetteki nesneler için erişim ihtiyacı duyar. Bkz. [AWS'de rol ve ilke oluşturma](aws-integration-set-up-configure.md#create-a-role-and-policy-in-aws).

### <a name="collection-failed-with-access-denied---download-report"></a>Koleksiyon, Erişim Reddedildi hatasıyla başarısız oldu - Raporu indirme

**Hata kodu:** _AccessDeniedDownloadReport_

Bu hata, Maliyet Yönetimi'nin Amazon S3 demetinde depolanan CUR dosyalarına erişemediğini ve onları indiremediğini gösterir. Role eklenmiş olan AWS JSON ilkesinin [AWS'de rol ve ilke oluşturma](aws-integration-set-up-configure.md#create-a-role-and-policy-in-aws) bölümünün en altında gösterilen örneğe benzediğinden emin olun.

### <a name="collection-failed-since-we-did-not-find-the-cost-and-usage-report"></a>Maliyet ve Kullanım Raporunu bulamadığımız için koleksiyon başarısız oldu

**Hata kodu:** _FailedToFindReport_

Bu hata, Maliyet Yönetimi'nin bağlayıcıda tanımlanmış olan Maliyet ve Kullanım raporunu bulamadığı anlamına gelir. Silinmediğinden emin olun ve role atanmış olan AWS JSON ilkesinin [AWS'de rol ve ilke oluşturma](aws-integration-set-up-configure.md#create-a-role-and-policy-in-aws) bölümünün en altında gösterilen örneğe benzediğini unutmayın.

### <a name="unable-to-create-or-verify-connector-due-to-cost-and-usage-report-definitions-mismatch"></a>Maliyet ve Kullanım Raporu tanımlarının eşleşmemesi nedeniyle bağlayıcı oluşturulamıyor veya doğrulanamıyor

**Hata kodu:** _ReportIsNotValid_

Bu hata, AWS Maliyet ve Kullanım raporunun tanımıyla ilgilidir, bu rapor için belirli ayarlar gerekmektedir, gereksinimler için bkz. [AWS'de özel Maliyet ve Kullanım raporu oluşturma](aws-integration-set-up-configure.md#create-a-cost-and-usage-report-in-aws)

## <a name="next-steps"></a>Sonraki adımlar

- Azure ortamınızı yönetim gruplarıyla yapılandırmadıysanız bkz. [Yönetim gruplarını ilk kez ayarlama](../../governance/management-groups/overview.md#initial-setup-of-management-groups).
