---
title: Azure yay bulutu 'nda uygulamayı ve dağıtımı anlama
description: Azure yay bulutu 'ndaki uygulama ve dağıtım arasındaki ayrım.
author: MikeDodaro
ms.author: brendm
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 07/23/2020
ms.custom: devx-track-java
ms.openlocfilehash: c053a7830e02eb7c460bd030ca3c6a10c00ea78a
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "91323646"
---
# <a name="understand-app-and-deployment-in-azure-spring-cloud"></a>Azure yay bulutu 'nda uygulamayı ve dağıtımı anlama

**Bu makale şu şekilde geçerlidir:** ✔️ Java ✔️ C #

**Uygulama** ve **dağıtım** , Azure Spring Cloud 'ın kaynak modelinde iki temel kavramlardır. Azure Spring Cloud 'da, bir *uygulama* bir iş uygulamasının veya bir mikro hizmetin soyutlamasıdır.  *Uygulama* olarak dağıtılan bir kod veya ikili dosya sürümü bir *dağıtımda* çalışır.  Uygulamalar, daha sonra gösterildiği gibi bir *Azure Spring Cloud Service örneğinde* veya yalnızca *hizmet örneğinde* çalışır.

 ![Uygulamalar ve dağıtımlar](./media/spring-cloud-app-and-deployment/app-deployment-rev.png)

Tek bir Azure aboneliğinde birden çok hizmet örneğiniz olabilir, ancak bir iş uygulaması veya mikro hizmeti oluşturan tüm uygulamalar tek bir hizmet örneği içinde yer alırken Azure Spring Cloud Service 'in kullanımı en kolay yoldur.

Azure yay bulutu standart katmanı, bir uygulamanın bir üretim dağıtımına ve tek bir hazırlık dağıtımına sahip olmasına olanak tanır. böylece kolayca mavi/yeşil bir dağıtım yapabilirsiniz.

## <a name="app"></a>Uygulama
Aşağıdaki özellikler/Özellikler uygulama düzeyinde tanımlanmıştır.

| Özellikler | Description |
|:--|:----------------|
| Genel</br>Uç Nokta | Uygulamaya erişmek için URL |
| Özel</br>Etki alanı | Özel etki alanının güvenliğini sağlayan CNAME kaydı |
| Hizmet</br>Bağlama | Diğer Azure hizmetleriyle kullanıma hazır bağlantı |
| Yönetilen</br>Kimlik | Azure Active Directory tarafından yönetilen kimlik, uygulamanızın Azure Key Vault gibi diğer Azure AD korumalı kaynaklara kolayca erişmesini sağlar |
| Kalıcı</br>Depolama | Verilerin uygulamanın yeniden başlatılmasının ötesinde kalıcı olmasını sağlayan ayar |

## <a name="deployment"></a>Dağıtım

Aşağıdaki özellikler/özellikler dağıtım düzeyinde tanımlanmıştır ve üretim/hazırlama dağıtımı takas edildiğinde değiş tokuş edilir.

| Özellikler | Açıklama |
|:--|:----------------|
| CPU | Uygulama örneği başına sanal çekirdek sayısı |
| Bellek | Uygulama örneği başına GB bellek|
| Örnek</br>Count | Uygulama örneklerinin sayısı, el ile veya otomatik olarak ayarlanır |
| Otomatik ölçeklendirme | Önceden tanımlanmış kurallara ve zamanlamaya göre örnek sayısını otomatik olarak Ölçeklendir |
| JVM</br>Seçenekler | JVM seçeneklerini ayarlama  |
| Ortam</br>Değişkenler | Ortam değişkenlerini ayarlama |
| Çalışma Zamanı</br>Sürüm | Java 8/Java 11|

## <a name="restrictions"></a>Kısıtlamalar

* **Uygulamanın bir üretim dağıtımı olması gerekir**: bir üretim DAĞıTıMıNıN silinmesi API tarafından engelleniyor. Silinmeden önce hazırlama olarak değiştirilmelidir.
* **Bir uygulama en fazla Iki dağıtıma sahip olabilir**: API tarafından ikiden fazla dağıtım oluşturma engellenmiş. Yeni ikilinizi mevcut üretime veya hazırlama dağıtımına dağıtın.
* **Dağıtım yönetimi temel katmanda kullanılamıyor**: Blue-Green dağıtım özelliği için standart katmanı kullanın.

## <a name="see-also"></a>Ayrıca bkz.
* [Azure yay bulutu 'nda hazırlama ortamı ayarlama](spring-cloud-howto-staging-environment.md)
