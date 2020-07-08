---
title: Azure Kubernetes hizmeti (AKS) tanılamaları genel bakış
description: Azure Kubernetes hizmetinde kendi kendine tanılama kümeleri hakkında bilgi edinin.
services: container-service
author: yunjchoi
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: yunjchoi
ms.openlocfilehash: 03bb7b786280dfcbb64190adac51b8d001d59c18
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "79126596"
---
# <a name="azure-kubernetes-service-diagnostics-preview-overview"></a>Azure Kubernetes hizmet tanılama (Önizleme) genel bakış

Azure Kubernetes Service (AKS) kümesi sorunlarını giderme, özellikle kümeniz iş açısından kritik iş yükleri çalıştırıyorsa kümenizin korunmasında önemli bir bölümüdür. AKS Tanılaması, kümenizdeki sorunları belirlemenize ve çözmenize yardımcı olan akıllı ve kendi kendine tanılama deneyimidir. AKS tanılaması bulutu yereldir ve bunu ek bir yapılandırma ya da faturalandırma maliyeti olmadan kullanabilirsiniz.

Bu özellik artık genel önizlemeye sunuldu.

## <a name="open-aks-diagnostics"></a>AKS tanılamayı aç

AKS tanılama 'ya erişmek için:

- [Azure Portal](https://portal.azure.com)Kubernetes kümenize gidin.
- Sol gezinti bölmesinde, AKS tanılamayı açan **sorunları Tanıla ve çöz** ' e tıklayın.
- Giriş sayfası kutucuğunda anahtar sözcükleri kullanarak kümenizin sorununu en iyi açıklayan bir kategori seçin veya arama çubuğunda sorununuzu en iyi açıklayan bir anahtar sözcük yazın, örneğin _küme düğümü sorunları_.

![Giriş sayfası](./media/concepts-diagnostics/aks-diagnostics-homepage.png)

## <a name="view-a-diagnostic-report"></a>Tanılama raporunu görüntüle

Bir kategoriye tıkladıktan sonra, kümenize özgü bir tanılama raporu görüntüleyebilirsiniz. Tanılama raporu, kümenizde durum simgeleriyle ilgili herhangi bir sorun varsa, bunu akıllıca çağırır. Sorunun ayrıntılı açıklamasını, önerilen eylemleri, yararlı belgeler, ilgili ölçümler ve günlüğe kaydetme verilerinin ayrıntılarını görmek için **daha fazla bilgi** ' ye tıklayarak her konuda ayrıntıya gidebilirsiniz. Tanılama raporları, çeşitli denetimler çalıştırıldıktan sonra kümenizin geçerli durumuna göre akıllıca oluşturulur. Tanılama raporları, kümenizin sorununu işaret etmek ve sorunu çözmek için sonraki adımları bulmak için yararlı bir araçtır.

![Tanılama raporu](./media/concepts-diagnostics/diagnostic-report.png)

![Genişletilmiş tanılama raporu](./media/concepts-diagnostics/node-issues.png)

## <a name="cluster-insights"></a>Küme öngörüleri

Aşağıdaki tanılama denetimleri, **küme öngörülerinde**kullanılabilir.

### <a name="cluster-node-issues"></a>Küme düğümü sorunları

Küme düğümü sorunları, kümenizin beklenmedik şekilde davranmasına neden olabilecek düğümle ilgili sorunları denetler.

- Düğüm hazırlığı sorunları
- Düğüm sorunları
- Yetersiz kaynak
- Düğüm eksik IP yapılandırması
- Düğüm CNı arızaları
- Düğüm bulunamadı
- Düğüm gücü kapalı
- Düğüm kimlik doğrulama hatası
- Node kuin-proxy eski

### <a name="create-read-update--delete-operations"></a>Oluşturma, okuma, güncelleştirme & silme işlemleri

CRUD Işlemleri, kümenizde sorunlara neden olabilecek CRUD işlemlerini denetler.

- Kullanımdaki alt ağ silme işlemi hatası
- Ağ güvenlik grubu silme işlemi hatası
- Kullanımdaki yol tablosu silme işlemi hatası
- Başvurulan kaynak sağlama hatası
- Genel IP adresi silme işlemi hatası
- Dağıtım kotası nedeniyle dağıtım hatası
- Kuruluş ilkesi nedeniyle işlem hatası
- Abonelik kaydı eksik
- VM Uzantısı sağlama hatası
- Alt ağ kapasitesi
- Kota aşıldı hatası

### <a name="identity-and-security-management"></a>Kimlik ve güvenlik yönetimi

Kimlik ve güvenlik yönetimi, kümenizin iletişimini engelleyebilecek kimlik doğrulama ve yetkilendirme hatalarını algılar.

- Düğüm yetkilendirme sorunları
- 401 hata
- 403 hataları

## <a name="next-steps"></a>Sonraki adımlar

[Aks Periscope](https://aka.ms/aksperiscope)kullanarak küme sorunlarınızı gidermeye yardımcı olmak için günlükleri toplayın.

Başlığa "[diag]" ekleyerek sorularınızı veya görüşlerinizi [UserVoice](https://feedback.azure.com/forums/914020-azure-kubernetes-service-aks) 'a gönderin.
