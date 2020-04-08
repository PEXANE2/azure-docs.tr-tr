---
title: Azure'da müşteri verilerinin korunması
description: Bu makale, Azure'un müşteri verilerini nasıl koruduğuna değinin.
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2018
ms.author: terrylan
ms.openlocfilehash: 630304bec17dd34befab4e5bd9f1cfdfb6505645
ms.sourcegitcommit: 98e79b359c4c6df2d8f9a47e0dbe93f3158be629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80811418"
---
# <a name="azure-customer-data-protection"></a>Azure müşteri veri koruması   
Microsoft işlemleri ve destek personeli tarafından müşteri verilerine erişim varsayılan olarak reddedilir. Müşteri verilerine erişim verildiğinde, liderlik onayı gereklidir ve erişim dikkatle yönetilir ve günlüğe kaydedilir. Erişim denetimi gereksinimleri aşağıdaki Azure Güvenlik İlkesi tarafından belirlenir:

- Varsayılan olarak müşteri verilerine erişim yok.
- Müşteri sanal makinelerinde (VM) kullanıcı veya yönetici hesabı yok.
- Görevi tamamlamak için gereken en az ayrıcalığı tanıyın; erişim isteklerini denetler ve kaydedin.

Azure destek personeline Microsoft tarafından benzersiz kurumsal Active Directory hesapları atanır. Azure, anahtar bilgi sistemlerine erişimi denetlemek için Microsoft Bilgi Teknolojisi (MSIT) tarafından yönetilen Microsoft kurumsal Active Directory'ye güvenir. Çok faktörlü kimlik doğrulama gereklidir ve erişim yalnızca güvenli konsollardan verilir.

Tüm erişim girişimleri izlenir ve temel bir rapor kümesi aracılığıyla görüntülenebilir.

## <a name="data-protection"></a>Veri koruma
Azure, müşterilere hem varsayılan olarak hem de müşteri seçenekleri olarak güçlü veri güvenliği sağlar.

**Veri ayrımı**: Azure çok kiracılı bir hizmettir, bu da birden çok müşteri dağıtımı nın ve Sanal Etkin'in aynı fiziksel donanımda depolandığı anlamına gelir. Azure, her müşterinin verilerini başkalarının verilerinden ayırmak için mantıksal yalıtım kullanır. Ayrımcılık, çok kiracılı hizmetlerin ölçeğini ve ekonomik faydalarını sağlarken, müşterilerin birbirlerinin verilerine erişmesini titizlikle engeller.

**Istirahat tesbiti veri koruması**: Müşteriler, Azure'da depolanan verilerin kendi standartlarına uygun olarak şifrelenmesini sağlamaktan sorumludur. Azure, müşterilere gereksinimlerini en iyi şekilde karşılayan çözümü seçme esnekliği sağlayan çok çeşitli şifreleme özellikleri sunar. Azure Key Vault, müşterilerin verileri şifrelemek için bulut uygulamaları ve hizmetleri tarafından kullanılan anahtarların denetimini kolayca korumalarına yardımcı olur. Azure Disk Şifreleme, müşterilerin VM'leri şifrelemesini sağlar. Azure Depolama Hizmeti Şifrelemesi, müşterinin depolama hesabına yerleştirilen tüm verilerin şifresini şifrelemeyi mümkün kılar.

**Aktarım da veri koruması**: Müşteriler kendi Sanal M'leri ile son kullanıcılar arasındaki trafik için şifreleme sağlayabilir. Azure, aktarım sırasındaki verileri veya dış bileşenlerden gelen verileri ve iki sanal ağ arasında olduğu gibi dahili olarak aktarım daki verileri korur. Azure, CESG/NCSC tarafından önerilen 2.048 bit RSA/SHA256 şifreleme anahtarlarıyla endüstri standardı Aktarım Katmanı Güvenliği (TLS) 1,2 veya daha sonraki protokolünü aşağıdakiler arasındaki iletişimi şifrelemek için kullanır:

- Müşteri ve bulut.
- Azure sistemleri ve veri merkezleri arasında dahili olarak.

**Şifreleme**: Depolama ve aktarım sırasındaki verilerin şifrelemesi, verilerin gizliliğini ve bütünlüğünü sağlamak için en iyi uygulama olarak müşteriler tarafından dağıtılabilir. Müşterilerin Azure bulut hizmetlerini, Internet'ten ve hatta Azure tarafından barındırılan VM'leri arasında iletişimi korumak için TLS kullanacak şekilde yapılandırmaları kolaydır.

**Veri artıklığı**: Microsoft, bir veri merkezine siber saldırı veya fiziksel hasar olduğunda verilerin korunmasını sağlamaya yardımcı olur. Müşteriler şunları tercih edebilir:

- Uyumluluk veya gecikme göz önünde bulundurulması için ülke içi/bölge içi depolama.
- Güvenlik veya olağanüstü durum kurtarma amacıyla ülke dışı/bölge dışı depolama.

Veriler artıklık için seçili bir coğrafi bölgede çoğaltılabilir, ancak bunun dışında aktarılamaz. Müşterilerin, kopya sayısı ve çoğaltma veri merkezlerinin sayısı ve konumu da dahil olmak üzere verileri çoğaltmak için birden çok seçeneği vardır.

Depolama hesabınızı oluştururken, aşağıdaki çoğaltma seçeneklerinden birini seçin:

- **Yerel olarak yedekli depolama (LRS)**: Yerel olarak yedekli depolama, verilerinizin üç kopyasını saklar. LRS, tek bir bölgedeki tek bir tesis içinde üç kez çoğaltılır. LRS verilerinizi normal donanım hatalarından korur, ancak tek bir tesisin arızalanmasından korur.
- **Bölge yedekli depolama (ZRS)**: Bölge yedekli depolama, verilerinizin üç kopyasını saklar. ZRS, LRS'den daha yüksek dayanıklılık sağlamak için iki ila üç tesis arasında üç kez çoğaltılır. Çoğaltma, tek bir bölge içinde veya iki bölge boyunca gerçekleşir. ZRS, verilerinizin tek bir bölgede dayanıklı olmasını sağlamaya yardımcı olur.
- **Coğrafi yedekli depolama (GRS)**: Oluşturduğunuzda varsayılan olarak depolama hesabınız için coğrafi yedekli depolama etkinleştirilir. GRS verilerinizin altı kopyasını tutar. GRS ile verileriniz birincil bölge içinde üç kez çoğaltılır. Verileriniz ayrıca birincil bölgeden yüzlerce mil uzaktaki ikincil bir bölgede üç kez çoğaltılarak en yüksek dayanıklılık düzeyini sağlar. Birincil bölgede bir hata olması durumunda, Azure Depolama ikincil bölge üzerinde başarısız olur. GRS, verilerinizin iki ayrı bölgede dayanıklı olmasını sağlamaya yardımcı olur.

**Veri imhası**: Müşteriler verileri sildiğinde veya Azure'dan ayrıldıklarında, Microsoft yeniden kullanılmadan önce depolama kaynaklarını n için aşırı yazma nın yanı sıra kullanımdan kaldırılan donanımın fiziksel olarak yok edilmesi için katı standartlar izler. Microsoft, müşteri isteği ve sözleşmenin feshi ile ilgili verilerin tamamen silinmesini yürütür.

## <a name="customer-data-ownership"></a>Müşteri veri sahipliği
Microsoft, müşterilerin Azure'a dağıtdığı uygulamaları denetlemez, onaylamaz veya denetlemez. Ayrıca, Microsoft müşterilerin Azure'da ne tür verileri depolamayı seçtiklerini bilmiyor. Microsoft, Azure'a girilen müşteri bilgileri üzerinde veri sahipliği talep etmez.

## <a name="records-management"></a>Kayıt yönetimi
Azure, arka uç verileri için dahili kayıt tutma gereksinimleri oluşturmuştur. Müşteriler kendi kayıt tutma gereksinimlerini tanımlamaktan sorumludur. Azure'da depolanan kayıtlar için müşteriler, verilerini ayıklamave içeriklerini müşteri tarafından belirtilen bekletme süresi boyunca Azure dışında saklamaktan sorumludur.

Azure, müşterilerin üründen veri ve denetim raporları dışa aktarmasına olanak tanır. Dışa aktarma, müşteri tarafından tanımlanan bekletme süresine ait bilgileri saklamak için yerel olarak kaydedilir.

## <a name="electronic-discovery-e-discovery"></a>Elektronik keşif (e-keşif)
Azure müşterileri, Azure hizmetlerini kullanırken e-keşif gereksinimlerine uymaktan sorumludur. Azure müşterilerinin müşteri verilerini koruması gerekiyorsa, verileri yerel olarak dışa aktarıp kaydedebilirler. Ayrıca, müşteriler verilerinin dışa aktarılmasını Azure Müşteri Desteği departmanından talep edebilir. Azure, müşterilerin verilerini dışa aktarmalarına izin vermenin yanı sıra, kapsamlı bir günlüğe kaydetme ve izleme sağlar.

## <a name="next-steps"></a>Sonraki adımlar
Microsoft'un Azure altyapısını korumak için ne yaptığı hakkında daha fazla bilgi edinmek için bkz:

- [Azure tesisleri, tesisler ve fiziksel güvenlik](physical-security.md)
- [Azure altyapısı kullanılabilirliği](infrastructure-availability.md)
- [Azure bilgi sistemi bileşenleri ve sınırları](infrastructure-components.md)
- [Azure ağ mimarisi](infrastructure-network.md)
- [Azure üretim ağı](production-network.md)
- [Azure SQL Veritabanı güvenlik özellikleri](infrastructure-sql.md)
- [Azure üretim işlemleri ve yönetimi](infrastructure-operations.md)
- [Azure altyapı izleme](infrastructure-monitoring.md)
- [Azure altyapı bütünlüğü](infrastructure-integrity.md)
