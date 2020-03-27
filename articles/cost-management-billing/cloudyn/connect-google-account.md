---
title: Azure'da Cloudyn'e Google Cloud Platform hesabı bağlama
description: Maliyet ve kullanım verilerini Cloudyn raporlarında görüntülemek için bir Google Cloud Platform hesabı bağlayın.
author: bandersmsft
ms.author: banders
ms.date: 03/12/2020
ms.topic: conceptual
ms.service: cost-management-billing
ms.reviewer: benshy
ms.custom: seodec18
ROBOTS: NOINDEX
ms.openlocfilehash: 4e10d39f744b40e7eb9058841eed9ff0f0e55d44
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79474654"
---
# <a name="connect-a-google-cloud-platform-account"></a>Google Cloud Platform hesabına bağlanma

Mevcut Google Cloud Platform hesabınızı Cloudyn'e bağlayabilirsiniz. Hesabınızı Cloudyn'e bağladıktan sonra maliyet ve kullanım verilerini Cloudyn raporlarında görüntüleyebilirsiniz. Bu makale, Google hesabınızı yapılandırma ve Cloudyn'e bağlama konusunda yardımcı olur.

[!INCLUDE [cloudyn-note](../../../includes/cloudyn-note.md)]

## <a name="collect-project-information"></a>Proje bilgilerini toplama

Öncelikle projenizle ilgili bilgileri toplamanız gerekir.

1. [https://console.cloud.google.com](https://console.cloud.google.com) adresinden Google Cloud Platform konsolunda oturum açın.
2. Cloudyn'e eklemek istediğiniz proje bilgilerini gözden geçirin ve **Proje adı** ile **Proje kimliği** değerlerini not edin. Bilgileri sonraki adımlar için kaydedin.  
    ![Google Cloud Platform konsolunda Proje adı ve Proje kimliği](./media/connect-google-account/gcp-console01.png)
3. Faturalandırma etkin değilse ve projenize bağlı değilse bir ödeme hesabı oluşturun. Daha fazla bilgi için bkz. [Yeni ödeme hesabı oluşturma](https://cloud.google.com/billing/docs/how-to/manage-billing-account#create/_a/_new/_billing/_account).

## <a name="enable-storage-bucket-billing-export"></a>Depolama demeti faturasını dışarı aktarmayı etkinleştirme

Cloudyn, Google faturalama verilerinizi bir depolama demetinden alır. **Demet adı** ve **Rapor ön eki** bilgilerini daha sonra Cloudyn kaydı sırasında kullanmak üzere saklayın.

Kullanım raporlarının depolanması için Google Cloud Storage'ın kullanılması minimum düzeyde ücretlere neden olur. Daha fazla bilgi için bkz. [Cloud Storage Fiyatlandırması](https://cloud.google.com/storage/pricing).

1. Faturalama verilerinin dosyaya aktarılmasını etkinleştirmediyseniz [Faturalama verilerini dosyaya aktarma](https://cloud.google.com/billing/docs/how-to/export-data-file#how_to_enable_billing_export_to_a_file) sayfasındaki yönergeleri izleyin. JSON veya CSV faturalandırma dışarı aktarma biçimini kullanabilirsiniz.
2. Aksi halde Google Cloud Platform konsolunda **Faturalama** > **Faturalama dışarı aktarma**'yı seçin. Faturalama **Demet adı** ve **Rapor ön eki** bilgilerini not edin.  
    ![Fatura dışarı aktarma sayfasında gösterilen fatura dışarı aktarma bilgileri](./media/connect-google-account/billing-export.png)

## <a name="enable-google-cloud-platform-apis"></a>Google Cloud Platform API'lerini etkinleştirme

Cloudyn'in kullanım ve varlık bilgilerini toplaması için aşağıdaki Google Cloud Platform API'lerinin etkinleştirilmesi gerekir:

- BigQuery API
- Google Cloud SQL
- Google Cloud Datastore API
- Google Cloud Storage
- Google Cloud Storage JSON API
- Google Compute Engine API

### <a name="enable-or-verify-apis"></a>API'leri etkinleştirme veya doğrulama

1. Google Cloud Platform konsolunda Cloudyn'e kaydetmek istediğiniz projeyi seçin.
2. **API'ler ve Hizmetler** > **Kitaplık** yolunu izleyin.
3. Yukarıda listelenen API'leri bulmak için arama işlevini kullanın.
4. Har bir API için **API etkinleştirildi** ifadesinin görüntülendiğini doğrulayın. Aksi takdirde **ETKİNLEŞTİR**'e tıklayın.

## <a name="add-a-google-cloud-account-to-cloudyn"></a>Cloudyn'e Google Cloud hesabı ekleme

1. Cloudyn portalını Azure portalından açın veya [https://azure.cloudyn.com](https://azure.cloudyn.com/) sayfasına gidip oturum açın.
2. **Ayarlar**'a (dişli simgesi) tıklayın ve **Bulut Hesapları**'nı seçin.
3. **Hesap Yönetimi** bölümünde **Google Hesapları** sekmesini seçip **Yeni ekle +** öğesine tıklayın.
4. **Google Hesabı Adı** alanına ödeme hesabının e-posta adresini girip **İleri**'ye tıklayın.
5. Google kimlik doğrulaması iletişim kutusunda bir Google hesabı seçin veya girin ve cloudyn.com'un hesabınıza erişmesine **İZİN VERİN**.
6. Önceden not aldığınız istek projesi bilgilerini ekleyin. **Proje kimliği**, **Proje adı**, **faturalama** demeti adı ve **faturalama dosyası** rapor ön eki bilgilerini doldurup **Kaydet**'e tıklayın.  
    ![Cloudyn hesabına Google projesi ekleme](./media/connect-google-account/add-project.png)

Google hesabınız, hesap listesinde görünür ve **Kimliği doğrulandı** ifadesine sahip olmalıdır. Hesabın altında Google projenizin adı ve kimliği ile yeşil onay işareti simgesi bulunmalıdır. Hesap Durumu **Tamamlandı** olmalıdır.

Birkaç saat içinde Google maliyet ve kullanım bilgileri Cloudyn raporlarında görüntülenmeye başlar.

## <a name="next-steps"></a>Sonraki adımlar

- Cloudyn hakkında daha fazla bilgi edinmek isterseniz Cloudyn için [Kullanımı ve maliyetleri gözden geçirme](tutorial-review-usage.md) öğreticisine geçin.
