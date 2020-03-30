---
title: Azure Veri Gölü Analizi Hesap İlkelerini Yönetme
description: Bir Data Lake Analytics hesabının kullanımını kontrol etmek için hesap ilkelerini en fazla AUs ve maksimum iş gibi nasıl kullanacağınızı öğrenin.
services: data-lake-analytics
ms.service: data-lake-analytics
author: guyhay
ms.author: guyhay
ms.reviewer: jasonwhowell
ms.assetid: 0a6102d1-7554-4df2-b487-4dae9a7287b6
ms.topic: conceptual
ms.date: 04/30/2018
ms.openlocfilehash: 4689714073047e383a53a04bd0069a8a27afdf9d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72966443"
---
# <a name="manage-azure-data-lake-analytics-using-account-policies"></a>Hesap İlkelerini Kullanarak Azure Veri Gölü Analizini Yönetme

Hesap ilkeleri, Azure Veri Gölü Analizi hesabının nasıl kullanıldığını denetlemenize yardımcı olur. Bu ilkeler, Azure Veri Gölü Analizi'ni kullanmanın maliyetini denetlemenize olanak tanır. Örneğin, bu ilkelerle, hesabın aynı anda kaç A'yi kullanabileceğini sınırlayarak beklenmeyen maliyet artışlarını önleyebilirsiniz.

## <a name="account-level-policies"></a>Hesap düzeyi ilkeleri

Bu ilkeler, Veri Gölü Analizi hesabındaki tüm işler için geçerlidir.

### <a name="maximum-number-of-aus-in-a-data-lake-analytics-account"></a>Veri Gölü Analytics hesabındaki maksimum AUs sayısı
Bir ilke, Data Lake Analytics hesabınızın kullanabileceği toplam Analiz Birimi (AUs) sayısını kontrol eder. Varsayılan olarak, değer 250 olarak ayarlanır. Örneğin, bu değer 250 US olarak ayarlanmışsa, kendisine atanan 250 U'su yla çalışan bir iş veya her biri 25 AUs ile çalışan 10 iş olabilir. Gönderilen ek işler, çalışan işler tamamlanana kadar sıraya alınır. Çalışan işler tamamlandığında, AUs'lar kuyrukta bekleyen işlerin çalıştırılaması için serbest bırakılır.

Data Lake Analytics hesabınızdaki AU sayısını değiştirmek için:

1. Azure portalında Data Lake Analytics hesabınıza gidin.
2. **Sınırlar ve ilkeler'i**tıklatın.
3. **Maksimum UYDU'lar**altında, bir değer seçmek için kaydırıcıyı taşıyın veya metin kutusuna değeri girin. 
4. **Kaydet**'e tıklayın.

> [!NOTE]
> Portaldaki varsayılan (250) MAY'lerden daha fazlasına ihtiyacınız varsa, destek isteği göndermek için **Yardım+Destek'i** tıklatın. Data Lake Analytics hesabınızda bulunan AUs sayısı artırılabilir.
>

### <a name="maximum-number-of-jobs-that-can-run-simultaneously"></a>Aynı anda çalıştırılabilen maksimum iş sayısı
Bu ilke, aynı anda kaç işin çalıştırılabildiğini sınırlar. Varsayılan olarak, bu değer 20 olarak ayarlanır. Data Lake Analytics'iniz kullanılabilir TU'lara sahipse, çalışan toplam iş sayısı bu politikanın değerine ulaşana kadar yeni işlerin hemen çalışması planlanır. Aynı anda çalıştırılabilen en fazla iş sayısına ulaştığınızda, bir veya daha fazla çalışan iş tamamlanana kadar (kullanılabilir AUs'a bağlı olarak) sonraki işler öncelik sırasına göre sıralanır.

Aynı anda çalıştırılabilen iş sayısını değiştirmek için:

1. Azure portalında Data Lake Analytics hesabınıza gidin.
2. **Sınırlar ve ilkeler'i**tıklatın.
3. **Maksimum Çalışan İş Sayısı**altında, bir değer seçmek için kaydırıcıyı taşıyın veya metin kutusuna değeri girin. 
4. **Kaydet**'e tıklayın.

> [!NOTE]
> Portalda varsayılan (20) iş sayısından daha fazla çalışmanız gerekiyorsa, destek isteği göndermek için **Yardım+Destek'i** tıklatın. Data Lake Analytics hesabınızda aynı anda çalıştırabilecek iş sayısı artırılabilir.
>

### <a name="how-long-to-keep-job-metadata-and-resources"></a>İş meta verilerini ve kaynaklarını ne kadar tutmak 
Kullanıcılarınız U-SQL işlerini çalıştırdığında, Data Lake Analytics hizmeti ilgili tüm dosyaları saklar. Bu dosyalar U-SQL komut dosyası, U-SQL komut dosyasında başvurulan DLL dosyaları, derlenmiş kaynaklar ve istatistikleri içerir. Dosyalar varsayılan Azure Veri Gölü Depolama hesabının /sistem/klasöründe dir. Bu ilke, bu kaynakların otomatik olarak silinmeden önce ne kadar süreyle depolanırlarını denetler (varsayılan değer 30 gündür). Bu dosyaları hata ayıklama ve gelecekte yeniden çalıştıracağınız işlerin performans ayarı için kullanabilirsiniz.

İş meta verilerini ve kaynaklarını ne kadar süreyle saklayın:

1. Azure portalında Data Lake Analytics hesabınıza gidin.
2. **Sınırlar ve ilkeler'i**tıklatın.
3. **İş Sorgularını Koruma Günleri'nin**altında, bir değer seçmek için kaydırıcıyı taşıyın veya metin kutusuna değeri girin.  
4. **Kaydet**'e tıklayın.

## <a name="job-level-policies"></a>İş düzeyi ilkeleri

İş düzeyi ilkeleri, en yüksek U'ları ve tek tek kullanıcıların (veya belirli güvenlik gruplarının üyelerinin) sundukları işlere ayarlayabildikleri maksimum önceliği denetlemenize olanak sağlar. Bu ilke, kullanıcıların neden olduğu maliyetleri denetlemenize olanak tanır. Ayrıca, zamanlanan işlerin aynı Data Lake Analytics hesabında çalışan yüksek öncelikli üretim işleri üzerindeki etkisini denetlemenize olanak tanır.

Data Lake Analytics'in iş düzeyinde belirleyebileceğiniz iki politikası vardır:

* **İş başına AU sınırı**: Kullanıcılar yalnızca bu aU sayısına kadar olan işleri gönderebilir. Varsayılan olarak, bu sınır hesap için maksimum AU sınırı yla aynıdır.
* **Öncelik**: Kullanıcılar yalnızca bu değerden daha düşük veya eşit önceliğe sahip işleri gönderebilir. Daha yüksek bir sayı daha düşük bir önceliği gösterir. Varsayılan olarak, bu sınır mümkün olan en yüksek öncelik olan 1 olarak ayarlanır.

Her hesapta ayarlanmış bir varsayılan ilke vardır. Varsayılan ilke, hesabın tüm kullanıcıları için geçerlidir. Belirli kullanıcılar ve gruplar için ek ilkeler oluşturabilirsiniz. 

> [!NOTE]
> Hesap düzeyi ilkeleri ve iş düzeyi ilkeleri aynı anda uygulanır.
>

### <a name="add-a-policy-for-a-specific-user-or-group"></a>Belirli bir kullanıcı veya grup için ilke ekleme

1. Azure portalında Data Lake Analytics hesabınıza gidin.
2. **Sınırlar ve ilkeler'i**tıklatın.
3. **İş Gönderme Sınırları**altında, Politika **Ekle** düğmesini tıklatın. Ardından aşağıdaki ayarları seçin veya girin:
    1. **İşlem İlkesi Adı**: İlkenin amacını hatırlatmak için bir ilke adı girin.
    2. **Kullanıcı veya Grup Seçin**: Bu politikanın geçerli olduğu kullanıcıyı veya grubu seçin.
    3. **İş AU Limitini Ayarlayın**: Seçili kullanıcı veya grup için geçerli olan AU sınırını ayarlayın.
    4. **Öncelik Sınırını Ayarla**: Seçili kullanıcı veya grup için geçerli olan öncelik sınırını ayarlayın.

4. **Tamam**'a tıklayın.

5. Yeni ilke, **İş Gönderme Sınırları**altında **Varsayılan** ilke tablosunda listelenir. 

### <a name="delete-or-edit-an-existing-policy"></a>Varolan bir ilkeyi silme veya düzenle

1. Azure portalında Data Lake Analytics hesabınıza gidin.
2. **Sınırlar ve ilkeler'i**tıklatın.
3. **İş Gönderme Sınırları**altında, yapmak istediğiniz ilkeyi bulun.
4.  **Sil** ve **Düzenle** seçeneklerini görmek için tablonun en sağ `...`sütunundaki .

## <a name="additional-resources-for-job-policies"></a>İş ilkeleri için ek kaynaklar
* [İlke genel bakış blog yazısı](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-overview/)
* [Hesap düzeyinde politikalar blog yazısı](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-account-level-policy/)
* [İş düzeyi politikaları blog yazısı](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-job-level-policy/)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Data Lake Analytics’e Genel Bakış](data-lake-analytics-overview.md)
* [Azure portalını kullanarak Data Lake Analytics'e başlayın](data-lake-analytics-get-started-portal.md)
* [Azure PowerShell'i kullanarak Azure Veri Gölü Analizini yönetin](data-lake-analytics-manage-use-powershell.md)