---
title: Azure Kubernetes Hizmeti (AKS) Tanılama Genel Bakış
description: Azure Kubernetes Hizmeti'nde kümeleri kendi kendine tanılama hakkında bilgi edinin.
services: container-service
author: yunjchoi
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: yunjchoi
ms.openlocfilehash: 03bb7b786280dfcbb64190adac51b8d001d59c18
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79126596"
---
# <a name="azure-kubernetes-service-diagnostics-preview-overview"></a>Azure Kubernetes Hizmet Tanılama (önizleme) genel bakış

Azure Kubernetes Hizmeti (AKS) küme sorunlarını giderme, özellikle kümeniz görev açısından kritik iş yüklerini çalıştırıyorsa, kümenizi korumanın önemli bir parçasıdır. AKS Diagnostics, kümenizdeki sorunları belirlemenize ve çözmenize yardımcı olan akıllı, kendi kendine tanılama deneyimidir. AKS Diagnostics bulut ayarı dır ve ek yapılandırma veya faturalama maliyeti olmadan kullanabilirsiniz.

Bu özellik artık genel önizlemede.

## <a name="open-aks-diagnostics"></a>Açık AKS Tanılama

AKS Tanılama'ya erişmek için:

- [Azure portalındaki](https://portal.azure.com)Kubernetes kümenize gidin.
- AKS Diagnostics'i açan sol navigasyonda **Tanılama ve sorunları çözme** üzerine tıklayın.
- Ana sayfa döşemesinde anahtar kelimeleri kullanarak kümenizin sorununu en iyi açıklayan bir kategori seçin veya arama çubuğunda sorununuzu en iyi açıklayan bir anahtar kelime yazın (örneğin _Küme Düğüm Sorunları._

![Giriş sayfası](./media/concepts-diagnostics/aks-diagnostics-homepage.png)

## <a name="view-a-diagnostic-report"></a>Tanılama raporunu görüntüleme

Bir kategoriyi tıklattıktan sonra, kümenize özgü bir tanılama raporunu görüntüleyebilirsiniz. Tanılama raporu, kümenizde durum simgeleriyle ilgili herhangi bir sorun olup olmadığını akıllıca çağırır. Sorunun ayrıntılı açıklamasını, önerilen eylemleri, yararlı dokümanlara bağlantılar, ilgili ölçümler ve günlük verilerini görmek için **Daha Fazla Bilgi'ye** tıklayarak her konuya ayrıntılı bilgi verebilirsiniz. Tanılama raporları, çeşitli denetimleri çalıştırdıktan sonra kümenizin geçerli durumuna göre akıllıca oluşturulur. Tanılama raporları kümenizin sorununu saptamak ve sorunu çözmek için sonraki adımları bulmak için yararlı bir araç olabilir.

![Tanı Raporu](./media/concepts-diagnostics/diagnostic-report.png)

![Genişletilmiş Tanı Raporu](./media/concepts-diagnostics/node-issues.png)

## <a name="cluster-insights"></a>Küme İstatistikleri

Aşağıdaki tanılama **denetimleri Cluster Insights'ta**mevcuttur.

### <a name="cluster-node-issues"></a>Küme Düğümü Sorunları

Küme Düğümü Sorunları Kümenizin beklenmeyen bir şekilde gibi durmasına neden olabilecek düğümle ilgili sorunları denetler.

- Düğüm hazırlık sorunları
- Düğüm hataları
- Yetersiz kaynaklar
- Düğüm eksik IP yapılandırması
- Düğüm CNI hataları
- Düğüm bulunamadı
- Düğüm gücü kapalı
- Düğüm kimlik doğrulama hatası
- Düğüm kube-proxy bayat

### <a name="create-read-update--delete-operations"></a>Oluşturma, okuma, güncelleme & silme işlemleri

CRUD Operations, kümenizde sorunlara neden olabilecek tüm CRUD işlemlerini denetler.

- Kullanım da subnet silme işlemi hatası
- Ağ güvenlik grubu silme işlemi hatası
- Kullanım içi rota tablosu silme işlemi hatası
- Başvurulan kaynak sağlama hatası
- Genel IP adresi silme işlemi hatası
- Dağıtım kotası nedeniyle dağıtım hatası
- Kuruluş ilkesi nedeniyle işlem hatası
- Eksik abonelik kaydı
- VM uzantısı sağlama hatası
- Alt ağ kapasitesi
- Kota hatası aşıldı

### <a name="identity-and-security-management"></a>Kimlik ve güvenlik yönetimi

Kimlik ve Güvenlik Yönetimi, kümenizdeki iletişimi engelleyebilecek kimlik doğrulama ve yetkilendirme hatalarını algılar.

- Düğüm yetkilendirme hataları
- 401 hata
- 403 hataları

## <a name="next-steps"></a>Sonraki adımlar

[AKS Periskop'u](https://aka.ms/aksperiscope)kullanarak küme sorunlarınızı daha da gidermenize yardımcı olmak için günlükleri toplayın.

Başlığına "[Diag]" ekleyerek sorularınızı veya geri bildirimlerinizi [UserVoice'ta](https://feedback.azure.com/forums/914020-azure-kubernetes-service-aks) yayınlayın.
