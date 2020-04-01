---
title: PCI-DSS v3.2.1 plan örnek kontrolleri
description: Ödeme Kartı Endüstrisi Veri Güvenliği Standardı v3.2.1 plan örneğinin Azure Politikası ve RBAC için kontrol eşlemi.
ms.date: 01/29/2020
ms.topic: sample
ms.openlocfilehash: db21ac9d628e777b6ff2cc86516cfb1497f5a62f
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76905639"
---
# <a name="control-mapping-of-the-pci-dss-v321-blueprint-sample"></a>PCI-DSS v3.2.1 plan örneğinin kontrol eşlemi

Aşağıdaki makalede, Azure Blueprints PCI-DSS v3.2.1 plan örnek haritalarının PCI-DSS v3.2.1 denetimleri ile nasıl açıklanmaktadır. Denetimler hakkında daha fazla bilgi için [PCI-DSS v3.2.1'e](https://www.pcisecuritystandards.org/documents/PCI_DSS_v3-2-1.pdf)bakın.

Aşağıdaki eşlemeler **PCI-DSS v3.2.1:2018** kontrollerine ait. Doğrudan belirli bir denetim eşleleme sine atlamak için sağdaki gezinmeyi kullanın. Eşlenen denetimlerin çoğu bir Azure [İlkesi](../../../policy/overview.md) girişimiyle uygulanır. Girişimin tamamını incelemek için Azure portalında **İlke'yi** açın ve **Tanımlar** sayfasını seçin. Ardından, ** \[Preview\] Audit PCI v3.2.1:2018 denetimlerini** bulun ve seçin ve yerleşik politika girişimini desteklemek için belirli VM Uzantılarıdağıtın.

> [!IMPORTANT]
> Aşağıdaki her denetim bir veya daha fazla [Azure İlkesi](../../../policy/overview.md) tanımıyla ilişkilidir. Bu ilkeler, denetime [uygunluğu değerlendirmenize](../../../policy/how-to/get-compliance-data.md) yardımcı olabilir; ancak, genellikle bir denetim ve bir veya daha fazla ilke arasında 1:1 veya tam bir eşleşme yoktur. Bu nedenle, Azure İlkesinde **Uyumlu olmak** yalnızca ilkelerin kendileri için başvurur; bu, bir denetimin tüm gereksinimlerine tam olarak uymanızı sağlamaz. Ayrıca, uyumluluk standardı şu anda herhangi bir Azure İlkesi tanımı tarafından ele alınmayan denetimleri içerir. Bu nedenle, Azure İlkesi'ndeki uyumluluk, genel uyumluluk durumunuza yalnızca kısmi bir görünümdür. Bu uyumluluk planı örneği için denetimler ve Azure İlkesi tanımları arasındaki ilişkilendirmeler zaman içinde değişebilir. Değişiklik geçmişini görüntülemek için [GitHub Commit History'ye](https://github.com/MicrosoftDocs/azure-docs/commits/master/articles/governance/blueprints/samples/pci-dss-3.2.1/control-mapping.md)bakın.

## <a name="132-and-134-boundary-protection"></a>1.3.2 ve 1.3.4 Sınır Koruması

Bu plan, ağ güvenlik gruplarını izin kurallarıyla izleyen [Azure İlkesi](../../../policy/overview.md) tanımları atayarak ağları yönetmenize ve denetlemenize yardımcı olur. Çok izin verilen kurallar istenmeyen ağ erişimine izin verebilir ve gözden geçirilmelidir. Bu plan, korumasız uç noktaları, uygulamaları ve depolama hesaplarını izleyen bir Azure İlkesi tanımı atar. Güvenlik duvarı tarafından korunmayan uç noktalar ve uygulamalar ve sınırsız erişime sahip depolama hesapları, bilgi sisteminde bulunan bilgilere istenmeyen erişime izin verebilir.

- Depolama hesaplarına sınırsız ağ erişimini denetleme
- Internet'ten erişim bitiş noktasına dönük olarak kısıtlanmalıdır

## <a name="34a-41-41g-41h-and-653-cryptographic-protection"></a>3.4.a, 4.1, 4.1.g, 4.1.h ve 6.5.3 Şifreleme Koruması

Bu plan, belirli şifreleme denetimlerini zorlayan [Azure İlkesi](../../../policy/overview.md) tanımları atayarak ve zayıf şifreleme ayarlarının denetim kullanımını kullanarak ilkenizi şifreleme denetimleri kullanarak uygulamanıza yardımcı olur. Azure kaynaklarınızın en iyi olmayan şifreleme yapılandırmalarına sahip olabileceğini anlamak, kaynakların bilgi güvenliği politikanıza uygun şekilde yapılandırıldığından emin olmak için düzeltici eylemlerde yardımcı olabilir. Özellikle, bu plan tarafından atanan ilkeler SQL veritabanlarında saydam veri şifreleme gerektirir; depolama hesaplarında eksik şifrelemeyi ve otomasyon hesabı değişkenlerini denetler. Depolama hesapları, İşlev Uygulamaları, WebApp, API Uygulamaları ve Redis Önbelleği'ne yönelik denetim güvensiz bağlantılarını ele alan ve şifrelenmemiş Hizmet Dokusu iletişimini denetleyen politikalar da vardır.

- Fonksiyon Uygulamasına yalnızca HTTPS üzerinden erişilebilir olmalıdır
- Web Uygulamasına yalnızca HTTPS üzerinden erişilebilir olmalıdır
- API Uygulamasına yalnızca HTTPS üzerinden erişilebilir olmalıdır
- SQL veritabanlarında Saydam Veri Şifrelemesi etkinleştirilmelidir
- Disk şifreleme sanal makinelerde uygulanmalıdır
- Otomasyon hesabı değişkenleri şifrelenmelidir
- Yalnızca Redis Önbelleğinize güvenli bağlantılar etkinleştirilmelidir
- Depolama hesaplarına güvenli aktarım etkinleştirilmelidir
- Service Fabric kümeleri ClusterProtectionLevel özelliğini EncryptAndSign olarak ayarlamalıdır
- SQL veritabanlarında Saydam Veri Şifrelemesi etkinleştirilmelidir
- SQL DB saydam veri şifrelemesini dağıtma

## <a name="51-62-66-and-1121-vulnerability-scanning-and-system-updates"></a>5.1, 6.2, 6.6 ve 11.2.1 Güvenlik Açığı Tarama ve Sistem Güncellemeleri

Bu plan, Eksik sistem güncelleştirmelerini, işletim sistemi güvenlik açıklarını, SQL güvenlik açıklarını ve Azure Güvenlik Merkezi'ndeki sanal makine güvenlik açıklarını izleyen [Azure İlkesi](../../../policy/overview.md) tanımlarını atayarak bilgi sistemi güvenlik açıklarını yönetmenize yardımcı olur. Azure Güvenlik Merkezi, dağıtılan Azure kaynaklarının güvenlik durumu hakkında gerçek zamanlı bilgi sahibi olmasını sağlayan raporlama özellikleri sağlar.

- Azure Güvenlik Merkezi'nde eksik Uç Nokta Koruması'nı izleme
- Windows Server için varsayılan Microsoft IaaaSAntimalware uzantısı dağıtma
- SQL Sunucularında Tehdit Algılamayı Dağıtma
- Makinelerinize sistem güncelleştirmeleri yüklenmelidir
- Makinelerinizdeki güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir
- SQL veritabanlarınızdaki güvenlik açıkları düzeltilmelidir
- Güvenlik açıkları, Güvenlik Açığı Değerlendirmesi çözümü yle düzeltilmelidir

## <a name="711-712-and-713-separation-of-duties"></a>7.1.1. 7.1.2 ve 7.1.3 Görevlerin Ayrılması

Yalnızca bir Azure abonelik sahibine sahip olmak, yönetim fazlalığına izin vermez. Tam tersine, çok fazla Azure abonelik sahibine sahip olmak, gizliliği ihlal edilen bir sahip hesabı aracılığıyla ihlal potansiyelini artırabilir. Bu plan, Azure aboneliklerinin sahip sayısını denetleyen [Azure İlkesi](../../../policy/overview.md) tanımları atayarak uygun sayıda Azure abonelik sahibini korumanıza yardımcı olur. Abonelik sahibi izinlerini yönetmek, uygun görev ayrımını uygulamanıza yardımcı olabilir.

- Aboneliğinize birden fazla sahip atanmalıdır
- Aboneliğiniz için en fazla 3 sahip belirlenmelidir 

## <a name="32-721-831a-and-831b-management-of-privileged-access-rights"></a>3.2, 7.2.1, 8.3.1.a ve 8.3.1.b Ayrıcalıklı Erişim Haklarının Yönetimi

Bu plan, çok faktörlü kimlik doğrulaması etkin olmayan sahibi, yazma ve/veya çalışma izinleri olan dış hesapları denetlemek için [Azure İlkesi](../../../policy/overview.md) tanımlarını atayarak ayrıcalıklı erişim haklarını kısıtlamanıza ve denetlemenize yardımcı olur. Azure, Azure kaynaklarına kimlerin erişebilenleri yönetmek için rol tabanlı erişim denetimi (RBAC) uygular. Özel RBAC kurallarının nerede uygulandığını anlamak, özel RBAC kuralları hataya açık olduğundan, gereksinimi ve doğru uygulamayı doğrulamanıza yardımcı olabilir. Bu plan ayrıca, SQL Sunucuları için Azure Active Directory kimlik doğrulamasını denetlemek için [Azure İlkesi](../../../policy/overview.md) tanımlarını da atar. Azure Etkin Dizin kimlik doğrulaması kullanmak, izin yönetimini basitleştirir ve veritabanı kullanıcılarının ve diğer Microsoft'un kimlik yönetimini merkezileştirir  
ağ geçidini kaydetmeniz gerekir.
 
- Sahip izinleri olan dış hesaplar aboneliğinizden kaldırılmalıdır
- Yazma izinleri olan dış hesaplar aboneliğinizden kaldırılmalıdır
- Okuma izinleri olan harici hesaplar aboneliğinizden kaldırılmalıdır
- MFA, aboneliğinizde sahip izni olan hesaplarda etkinleştirilmelidir
- MFA, aboneliğinizde yazma izinleri olan hesaplar etkinleştirilmelidir
- MFA, aboneliğinizde okuma izinleri olan hesaplarda etkinleştirilmelidir
- SQL sunucuları için bir Azure Active Directory yöneticisi sağlanmalıdır
- Özel RBAC kurallarının denetim kullanımı

## <a name="812-and-815-least-privilege-and-review-of-user-access-rights"></a>8.1.2 ve 8.1.5 Kullanıcı Erişim Haklarının En Az Ayrıcalık ve Gözden Geçirilmesi

Azure, Azure'daki kaynaklara kimlerin erişebildiğinizi yönetmenize yardımcı olmak için rol tabanlı erişim denetimi (RBAC) uygular. Azure portalını kullanarak, Azure kaynaklarına ve izinlerine kimlerin erişimi olduğunu inceleyebilirsiniz. Bu plan, amortismana tabi hesaplar ve yüksek izinlere sahip dış hesaplar da dahil olmak üzere gözden geçirilmesi için öncelik verilmesi gereken denetim hesaplarına [Azure İlkesi](../../../policy/overview.md) tanımlarını atar.

- Amortismana alınan hesaplar aboneliğinizden kaldırılmalıdır
- Sahip izinleri olan amortismana alınmış hesaplar aboneliğinizden kaldırılmalıdır
- Sahip izinleri olan dış hesaplar aboneliğinizden kaldırılmalıdır
- Yazma izinleri olan dış hesaplar aboneliğinizden kaldırılmalıdır
- Okuma izinleri olan harici hesaplar aboneliğinizden kaldırılmalıdır

## <a name="813-removal-or-adjustment-of-access-rights"></a>8.1.3 Erişim Haklarının Kaldırılması veya Ayarı

Azure, Azure'daki kaynaklara kimlerin erişebilenleri yönetmenize yardımcı olmak için rol tabanlı erişim denetimi (RBAC) uygular. Azure Etkin Dizini ve RBAC'ı kullanarak, kullanıcı rollerini kuruluş değişikliklerini yansıtacak şekilde güncelleştirebilirsiniz. Gerektiğinde, hesapların oturum açması (veya kaldırılması) engellenebilir ve bu da Azure kaynaklarına erişim haklarını hemen kaldırır. Bu plan, kaldırılması için düşünülmesi gereken amortismana tabi hesabı denetlemek için [Azure İlkesi](../../../policy/overview.md) tanımlarını atar.

- Amortismana alınan hesaplar aboneliğinizden kaldırılmalıdır
- Sahip izinleri olan amortismana alınmış hesaplar aboneliğinizden kaldırılmalıdır

## <a name="823ab-824ab-and-825-password-based-authentication"></a>8.2.3.a,b, 8.2.4.a,b ve 8.2.5 Şifre Tabanlı Kimlik Doğrulama

Bu plan, minimum güç ve diğer parola gereksinimlerini zorlamayan Windows VM'lerini denetleyen [Azure İlkesi](../../../policy/overview.md) tanımları atayarak güçlü parolalar uygulamanıza yardımcı olur. Parola gücü ilkesini ihlal eden VM'lerin farkında olmak, tüm VM kullanıcı hesaplarının parolalarının ilkeyle uyumlu olduğundan emin olmak için düzeltici eylemlerde yardımcı olur.

- \[Önizleme\]: En fazla 70 günlük parola yaşı olmayan Windows VM'lerini denetleyin
- \[Önizleme\]: En fazla 70 günlük parola yaşı olmayan Windows VM'lerini denetlemek için gereksinimleri dağıtma
- \[Önizleme\]: Minimum parola uzunluğunu 14 karakterle sınırlamayan Windows VM'lerini denetleme
- \[Önizleme\]: Minimum parola uzunluğunu 14 karakterle sınırlamayan Windows VM'lerini denetlemek için gereksinimleri dağıtma
- \[Önizleme\]: Önceki 24 parolanın yeniden kullanımına izin veren Windows VM'lerini denetleme
- \[Önizleme\]: Önceki 24 parolanın yeniden kullanılmasına izin veren Windows VM'lerini denetlemek için gereksinimleri dağıtma

## <a name="103-and-1054-audit-generation"></a>10.3 ve 10.5.4 Denetim Üretimi

Bu plan, Azure kaynaklarında günlük ayarlarını denetleyen [Azure İlkesi](../../../policy/overview.md) tanımları atayarak sistem olaylarının günlüğe kaydedilmesini sağlamanıza yardımcı olur.
Tanılama günlükleri, Azure kaynakları içinde gerçekleştirilen işlemlerhakkında bilgi sağlar. Azure günlükleri, kaynaklar arasında olayların zamanla ilişkili bir kaydını oluşturmak için senkronize iç saatlere güvenir.

- SQL Server'da gelişmiş veri güvenliği ayarlarında denetim etkinleştirilmelidir
- Tanılama ayarını denetle
- DENETIM SQL sunucu düzeyi Denetim ayarları
- DENETIMI SQL sunucularında dağıtma
- Depolama hesapları yeni Azure Kaynak Yöneticisi kaynaklarına geçirilmelidir
- Sanal makineler yeni Azure Kaynak Yöneticisi kaynaklarına geçirilmelidir

## <a name="1236-and-1237-information-security"></a>12.3.6 ve 12.3.7 Bilgi Güvenliği

Bu plan, kabul edilebilir ağ konumlarını ve ortam için izin verilen onaylı şirket ürünlerini denetleyen [Azure İlkesi](../../../policy/overview.md) tanımları atayarak ağınızı yönetmenize ve denetlemenize yardımcı olur. Bunlar, bu ilkelerin her biri içindeki ilke parametreleri aracılığıyla her şirket tarafından özelleştirilebilir.

- İzin verilen konumlar
- Kaynak grupları için izin verilen konumlar

## <a name="next-steps"></a>Sonraki adımlar

PCI-DSS v3.2.1 planının kontrol eşlemeyi gözden geçirdiğiniz için, genel bakış ve bu örneğin nasıl dağıtılabildiğini öğrenmek için aşağıdaki makaleleri ziyaret edin:

> [!div class="nextstepaction"]
> [PCI-DSS v3.2.1 planı - Genel Bakış](./index.md)
> [PCI-DSS v3.2.1 planı - Dağıtım adımları](./deploy.md)

Şemalar ve bunların kullanımı hakkındaki diğer makaleler:

- [Plan yaşam döngüsü](../../concepts/lifecycle.md)hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- Varolan atamaları nasıl [güncelleştirini](../../how-to/update-existing-assignments.md)öğrenin.