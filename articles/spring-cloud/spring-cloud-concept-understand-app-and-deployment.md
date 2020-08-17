---
title: Azure yay bulutu 'nda uygulamayı ve dağıtımı anlama
description: Azure yay bulutu 'ndaki uygulama ve dağıtım arasındaki ayrım.
author: MikeDodaro
ms.author: brendm
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 07/23/2020
ms.custom: devx-track-java
ms.openlocfilehash: 81e1925810f374da6f02bf6c3a013b00b5bb9a2c
ms.sourcegitcommit: 64ad2c8effa70506591b88abaa8836d64621e166
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88264061"
---
# <a name="understand-app-and-deployment-in-azure-spring-cloud"></a>Azure yay bulutu 'nda uygulamayı ve dağıtımı anlama

**Uygulama** ve **dağıtım** , Azure Spring Cloud 'ın kaynak modelinde iki temel kavramlardır. Azure Spring Cloud 'da, bir *uygulama* bir iş uygulamasının veya bir mikro hizmetin soyutlamasıdır.  *Uygulama* olarak dağıtılan bir kod veya ikili dosya sürümü bir *dağıtımda*çalışır.

 ![Uygulamalar ve dağıtımlar](./media/spring-cloud-app-and-deployment/app-deployment-rev.png)

Azure yay bulutu standart katmanı, bir uygulamanın bir üretim dağıtımına ve tek bir hazırlık dağıtımına sahip olmasına olanak tanır. böylece kolayca mavi/yeşil bir dağıtım yapabilirsiniz.

## <a name="app"></a>Uygulama
Aşağıdaki özellikler/Özellikler uygulama düzeyinde tanımlanmıştır.

| Sabit listesi | Tanım |
|:--|:----------------|
| Genel</br>Uç Noktası | Uygulamaya erişmek için URL |
| Özel</br>Etki alanı | Özel etki alanının güvenliğini sağlayan CNAME kaydı |
| Hizmet</br>Bağlama | Dosyadaki function.jsve *Servicebustrigger* özniteliğinde ayarlanan bağlama yapılandırma özellikleri |
| Yönetilen</br>Kimlik | Azure Active Directory tarafından yönetilen kimlik, uygulamanızın Azure Key Vault gibi diğer Azure AD korumalı kaynaklara kolayca erişmesini sağlar |
| Kalıcı</br>Depolama | Verilerin uygulamanın yeniden başlatılmasının ötesinde kalıcı olmasını sağlayan ayar |

## <a name="deployment"></a>Dağıtım

Aşağıdaki özellikler/özellikler dağıtım düzeyinde tanımlanmıştır ve üretim/hazırlama dağıtımı takas edildiğinde değiş tokuş edilir.

| Sabit listesi | Tanım |
|:--|:----------------|
| CPU | Uygulama örneği başına sanal çekirdek sayısı |
| Bellek | Dağıtımları ölçeklendirmek veya genişletmek için bellek ayıran ayar |
| Örnek</br>Count | Uygulama örneklerinin sayısı, el ile veya otomatik olarak ayarlanır |
| Otomatik ölçeklendirme | Önceden tanımlanmış kurallara ve zamanlamaya göre örnek sayısını otomatik olarak Ölçeklendir |
| JVM</br>Seçenekler | ayar: JAVA_OPTS |
| Ortam</br>Değişkenler | Tüm Azure yay bulut ortamına uygulanan ayarlar |
| Çalışma Zamanı</br>Sürüm | Java 8/Java 11|

## <a name="restrictions"></a>Kısıtlamalar

* **Uygulamanın bir üretim dağıtımı olması gerekir**: bir üretim DAĞıTıMıNıN silinmesi API tarafından engelleniyor. Silinmeden önce hazırlama olarak değiştirilmelidir.
* **Bir uygulama en fazla Iki dağıtıma sahip olabilir**: API tarafından ikiden fazla dağıtım oluşturma engellenmiş. Yeni ikilinizi mevcut üretime veya hazırlama dağıtımına dağıtın.
* **Dağıtım yönetimi temel katmanda kullanılamıyor**: mavi yeşil dağıtım yeteneği için standart katmanı kullanın.

## <a name="see-also"></a>Ayrıca bkz.
* [Azure yay bulutu 'nda hazırlama ortamı ayarlama](spring-cloud-howto-staging-environment.md)