---
title: Google Cloud Platform hesabı Azure Cloudyn bağlanın | Microsoft Docs
description: Cloudyn raporlarında maliyet ve kullanım verilerini görüntülemek için bir Google Cloud Platform hesap bağlanın.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 05/21/2019
ms.topic: conceptual
ms.service: cost-management-billing
manager: benshy
ms.custom: seodec18
ms.openlocfilehash: 937d1b6e0bc9ece0507821538fafb0f5d8c0ef99
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74230127"
---
# <a name="connect-a-google-cloud-platform-account"></a>Google Cloud Platform hesaba Bağlan

Var olan Google Cloud Platform hesabınızı Cloudyn'e bağlanabilirsiniz. Hesabınızı Cloudyn'e bağladıktan sonra maliyet ve kullanım verileri, Cloudyn raporlarında kullanılabilir. Bu makalede, yapılandırmak ve Google hesabınızı Cloudyn'e bağlanmak için yardımcı olur.


## <a name="collect-project-information"></a>Proje bilgileri toplayın

Projeniz hakkında bilgi toplamak yoluyla başlattığınız.

1. [https://console.cloud.google.com](https://console.cloud.google.com)Google Cloud Platform konsolunda oturum açın.
2. Cloudyn 'e eklemek istediğiniz proje bilgilerini gözden geçirin ve **proje adını** ve **Proje kimliğini**unutmayın. Sonraki adımlar için kullanışlı bilgiler tutun.  
    Google Cloud Platform konsolunda gösterilen ![proje adı ve proje KIMLIĞI](./media/connect-google-account/gcp-console01.png)
3. Faturalandırma etkin değil ve projenize bağlı, bir faturalama hesabı oluşturun. Daha fazla bilgi için, bkz. [yeni faturalandırma hesabı oluşturma](https://cloud.google.com/billing/docs/how-to/manage-billing-account#create/_a/_new/_billing/_account).

## <a name="enable-storage-bucket-billing-export"></a>Depolama demetine fatura vermeyi etkinleştir

Cloudyn, bir depolama alanı Google fatura verilerinizi alır. Cloudyn kaydı sırasında daha sonra kullanılmak üzere **demet adı** ve **rapor öneki** bilgilerini daha sonra kullanmak için kullanışlı tutun.

Kullanım raporları depolamak için Google bulut depolama kullanarak en az ücretler doğurur. Daha fazla bilgi için bkz. [bulut depolama fiyatlandırması](https://cloud.google.com/storage/pricing).

1. Bir dosyaya faturalandırma vermeyi etkinleştirmediyseniz, [bir dosyaya faturalandırma verme özelliğini etkinleştirme](https://cloud.google.com/billing/docs/how-to/export-data-file#how_to_enable_billing_export_to_a_file)konusundaki yönergeleri izleyin. JSON veya CSV fatura dışarı aktarma biçimini kullanabilirsiniz.
2. Aksi takdirde, Google Cloud Platform konsolunda **faturalandırma** > **faturalandırma dışarı aktarma**' ya gidin. Fatura **kovaya adı** ve **rapor ön ekine**göz önüne alın.  
    Faturalandırma dışa aktarma sayfasında gösterilen faturalandırma dışa aktarma bilgilerini ![](./media/connect-google-account/billing-export.png)

## <a name="enable-google-cloud-platform-apis"></a>Google Cloud Platform API'leri etkinleştirin

Kullanım ve varlık bilgilerini toplamak için aşağıdaki Google Cloud Platform etkin API'ler Cloudyn gerekir:

- BigQuery API
- Google Cloud SQL
- Google Cloud Datastore API
- Google bulut depolama
- Google bulut depolama JSON API
- Google Compute Engine API

### <a name="enable-or-verify-apis"></a>Etkinleştirmek veya API'leri doğrulayın

1. Google Cloud Platform konsolunda Cloudyn'e kaydetmek istediğiniz projeyi seçin.
2. **API 'ler & hizmetleri** > **kitaplığına**gidin.
3. Daha önce listelenen her API bulmak için arama özelliğini kullanın.
4. Her API için, API 'nin **etkin** olduğunu doğrulayın. Aksi takdirde **Etkinleştir**' e tıklayın.

## <a name="add-a-google-cloud-account-to-cloudyn"></a>Google Cloud hesabı Cloudyn'e Ekle

1. Azure portal Cloudyn portalını açın veya [https://azure.cloudyn.com](https://azure.cloudyn.com/) ve oturum açın.
2. **Ayarlar** ' a tıklayın (COG simgesi) ve ardından **bulut hesapları**' nı seçin.
3. **Hesap yönetimi**'Nde **Google hesapları** sekmesini seçin ve ardından **Yeni + Ekle**' ye tıklayın.
4. **Google hesap adı**alanına faturalandırma hesabının e-posta adresini girin ve ardından **İleri**' ye tıklayın.
5. Google kimlik doğrulaması iletişim kutusunda bir Google hesabı seçin veya girin ve sonra hesabınıza cloudyn.com erişimine **Izin verin** .
6. İstek proje bilgileri eklemek önceki olduğunu not. Bunlar **proje kimliği**, **Proje** adı, **Fatura** demeti adı ve **Faturalama dosyası** rapor ön ekini içerir ve **Kaydet**' e tıklayın.  
    Cloudyn hesabına Google projesi eklemek ![](./media/connect-google-account/add-project.png)

Google Hesabınız Hesap listesinde görünür ve **kimliği doğrulanmalıdır**. Bunun altında Google proje adı ve kimliği görünür ve yeşil onay işareti simgesi vardır. Hesap durumu **tamamlandı**olmalıdır.

Birkaç saat içinde Google maliyet ve kullanım bilgilerini Cloudyn raporlarında gösterilir.

## <a name="next-steps"></a>Sonraki adımlar

- Cloudyn hakkında daha fazla bilgi edinmek için Cloudyn için [kullanımı ve maliyetleri gözden geçirme](./tutorial-review-usage.md) öğreticisine geçin.
