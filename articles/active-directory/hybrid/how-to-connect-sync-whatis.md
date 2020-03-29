---
title: 'Azure AD Connect eşitlemesi: Eşitlemeyi anlama ve özelleştirme | Microsoft Dokümanlar'
description: Azure AD Connect eşitlemenin nasıl çalıştığını ve nasıl özelleştirilebildiğini açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: ee4bf802-045b-4da0-986e-90aba2de58d6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/08/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: e3b87f40d75d4045155e7dd953dc76ffd9de2b34
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60348748"
---
# <a name="azure-ad-connect-sync-understand-and-customize-synchronization"></a>Azure AD Connect eşitlemesi: Eşitlemeyi anlama ve özelleştirme
Azure Active Directory Connect eşitleme hizmetleri (Azure AD Connect eşitleme), Azure AD Connect'in ana bileşenidir. Şirket ortamınız ve Azure AD arasında kimlik verilerini eşitlemekle ilgili tüm işlemleri halleder. Azure AD Connect eşitlemi, Azure Active Directory Bağlayıcısı yapılandırılmış olan DirSync, Azure AD Sync ve Forefront Identity Manager'ın halefidir.

Bu **konu, Azure AD Connect eşitleme** **(eşitleme motoru**olarak da adlandırılır) için ana sayfadır ve bununla ilgili diğer tüm konulara bağlantılar listeler. Azure AD Connect'e bağlantılar için [bkz.](whatis-hybrid-identity.md)

Eşitleme hizmeti iki bileşenden oluşur: şirket içi **Azure AD Connect eşitleme** bileşeni ve Azure AD **Connect eşitleme hizmeti**olarak adlandırılan Azure AD'deki hizmet tarafı.

## <a name="azure-ad-connect-sync-topics"></a>Azure AD Bağlama eşitleme konuları
| Konu başlığı | Neleri kapsıyor ve ne zaman okunacak |
| --- | --- |
| **Azure AD Connect eşitleme temelleri** | |
| [Mimariyi anlama](concept-azure-ad-connect-sync-architecture.md) |Senkronizasyon motoruna yeni gelen ve mimari ve kullanılan terimler hakkında bilgi edinmek isteyenler için. |
| [Teknik kavramlar](how-to-connect-sync-technical-concepts.md) |Mimari konunun kısa bir sürümü ve kısaca kullanılan terimleri açıklar. |
| [Azure AD Connect için topolojiler](plan-connect-topologies.md) |Eşitleme altyapısının desteklediği farklı topolojileri ve senaryoları açıklar. |
| **Özel yapılandırma** | |
| [Yükleme sihirbazını yeniden çalıştırma](how-to-connect-installation-wizard.md) |Azure AD Connect yükleme sihirbazını yeniden çalıştırdığınızda hangi seçeneklerin kullanılabildiğinizi açıklar. |
| [Bildirimsel Sağlamanın Anlaşılması](concept-azure-ad-connect-sync-declarative-provisioning.md) |Bildirimsel sağlama olarak adlandırılan yapılandırma modelini açıklar. |
| [Beyan Edici İfadeleri Anlama](concept-azure-ad-connect-sync-declarative-provisioning-expressions.md) |Bildirimsel sağlamada kullanılan ifade dilinin sözdizimini açıklar. |
| [Varsayılan yapılandırmayı anlama](concept-azure-ad-connect-sync-default-configuration.md) |Kutudan çıkma kurallarını ve varsayılan yapılandırmayı açıklar. Ayrıca, kutunun dışında senaryoların çalışması için kuralların birlikte nasıl çalıştığını da açıklar. |
| [Kullanıcıları ve Kişileri Anlama](concept-azure-ad-connect-sync-user-and-contacts.md) |Önceki konu üzerinde devam eder ve özellikle çok ormanlı bir ortamda, kullanıcılar ve ilgili kişiler için yapılandırmanın birlikte nasıl çalıştığını açıklar. |
| [Varsayılan yapılandırmada değişiklik yapma](how-to-connect-sync-change-the-configuration.md) |Akışları öznitelik için ortak bir yapılandırma değişikliği nasıl yapacağınız konusunda size yol gösterir. |
| [Varsayılan yapılandırmanın değiştirilmesine ilişkin önerilen yöntemler](how-to-connect-sync-best-practices-changing-default-configuration.md) |Destek sınırlamaları ve kutu dan sıcağkonfigürasyonda değişiklik yapmak için. |
| [Filtreleme yapılandırma](how-to-connect-sync-configure-filtering.md) |Hangi nesnelerin Azure AD ile senkronize edildiği ne kadar sınırlanılabilmek için farklı seçenekleri ve bu seçenekleri nasıl yapılandırılabilmek için adım adım açıklar. |
| **Özellikler ve senaryolar** | |
| [Yanlışlıkla silmeleri önleme](how-to-connect-sync-feature-prevent-accidental-deletes.md) |Önleme *yanlışlıkla siler* özelliğini ve nasıl yapılandırılabildiğini açıklar. |
| [Zamanlayıcı](how-to-connect-sync-feature-scheduler.md) |Verileri içe aktaran, eşitleyen ve dışa aktaran yerleşik zamanlayıcıyı açıklar. |
| [Parola karma senkronizasyonunu uygulama](how-to-connect-password-hash-synchronization.md) |Parola eşitlemenin nasıl çalıştığını, nasıl uygulanacağını ve nasıl çalıştırılabildiğini ve sorun gidermesini açıklar. |
| [Cihaz geri yazma](how-to-connect-device-writeback.md) |Azure AD Connect'te aygıt yazma nın nasıl çalıştığını açıklar. |
| [Dizin genişletmeleri](how-to-connect-sync-feature-directory-extensions.md) |Azure AD şemasını kendi özel özniteliklerinizle nasıl genişletirleştirin. |
| [Office 365 Tercih Edilen Veri Konumu](how-to-connect-sync-feature-preferreddatalocation.md) |Kullanıcının Office 365 kaynaklarını kullanıcıyla aynı bölgeye nasıl koyacağımı açıklar. |
| **Eşitleme Hizmeti** | |
| [Azure AD Connect eşitleme hizmeti özellikleri](how-to-connect-syncservice-features.md) |Azure AD'de eşitleme hizmeti tarafını ve eşitleme ayarlarını nasıl değiştireceğiniz açıklanır. |
| [Yinelenen öznitelik esnekliği](how-to-connect-syncservice-duplicate-attribute-resiliency.md) |**UserPrincipalName** ve **proxyAddresses** yinelenen öznitelik değerleri esnekliğini nasıl etkinleştireceği ve kullanılacağını açıklar. |
| **Operasyonlar ve UI** | |
| [Eşitleme Hizmeti Yöneticisi](how-to-connect-sync-service-manager-ui.md) |[Operations,](how-to-connect-sync-service-manager-ui-operations.md) [Connectors,](how-to-connect-sync-service-manager-ui-connectors.md) [Metaverse Designer](how-to-connect-sync-service-manager-ui-mvdesigner.md)ve [Metaverse Search](how-to-connect-sync-service-manager-ui-mvsearch.md) sekmeleri de dahil olmak üzere Eşitleme Servis Yöneticisi Kullanıcı Arabirimi'ni açıklar. |
| [Operasyonel görevler ve dikkat edilecek hususlar](how-to-connect-sync-operations.md) |Olağanüstü durum kurtarma gibi operasyonel kaygıları açıklar. |
| **Nasıl Yapılır...** | |
| [Azure REKLAM hesabını sıfırlama](how-to-connect-azureadaccount.md) |Azure AD Connect sync'inden Azure AD'ye bağlanmak için kullanılan hizmet hesabının kimlik bilgilerini sıfırlama. |
| **Daha fazla bilgi ve referanslar** | |
| [Bağlantı Noktaları](reference-connect-ports.md) |Eşitleme motoru ile şirket içi dizinleriniz ve Azure AD arasında açmanız gereken bağlantı noktalarını listeler. |
| [Azure Etkin Dizini'ne senkronize edilen öznitelikler](reference-connect-sync-attributes-synchronized.md) |Şirket içi AD ve Azure AD arasında senkronize edilen tüm öznitelikleri listeler. |
| [Fonksiyonlar Referans](reference-connect-sync-functions-reference.md) |Bildirimsağlamada kullanılabilen tüm işlevleri listeler. |

## <a name="additional-resources"></a>Ek Kaynaklar
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)
