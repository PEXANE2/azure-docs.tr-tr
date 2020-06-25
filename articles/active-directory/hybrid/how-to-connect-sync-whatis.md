---
title: 'Azure AD Connect eşitleme: eşitlemeyi anlama ve özelleştirme | Microsoft Docs'
description: Azure AD Connect eşitlemenin nasıl çalıştığını ve nasıl özelleştirileceğini açıklar.
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
ms.topic: how-to
ms.date: 11/08/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4dc1754eb8818df894e67c1748517b2e2f4356d6
ms.sourcegitcommit: f98ab5af0fa17a9bba575286c588af36ff075615
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2020
ms.locfileid: "85356874"
---
# <a name="azure-ad-connect-sync-understand-and-customize-synchronization"></a>Azure AD Connect eşitleme: eşitlemeyi anlama ve özelleştirme
Azure Active Directory Connect Eşitleme Hizmetleri (Azure AD Connect eşitleme) Azure AD Connect ana bileşenidir. Şirket içi ortamınız ile Azure AD arasında kimlik verilerini eşitlemeye ilişkin tüm işlemlerin ilgilendiğinden emin olur. Azure AD Connect eşitleme, Azure Active Directory Bağlayıcısı yapılandırılmış DirSync, Azure AD Eşitleme ve Forefront Identity Manager 'ın ardıllarıdır.

Bu konu, **Azure AD Connect Sync** ( **eşitleme altyapısı**da denir) için giriş ve onunla ilgili diğer tüm konuların bağlantılarını listeler. Azure AD Connect bağlantıları için bkz. Şirket [içi kimliklerinizi Azure Active Directory Ile tümleştirme](whatis-hybrid-identity.md).

Eşitleme hizmeti iki bileşenden oluşur, şirket içi **Azure AD Connect eşitleme** bileşeni ve Azure AD 'deki hizmet tarafı **Azure AD Connect eşitleme hizmeti**olarak adlandırılır.

## <a name="azure-ad-connect-sync-topics"></a>Azure AD Connect eşitleme konuları
| Konu başlığı | Ne kapsıyorsa ve ne zaman okunacaktır |
| --- | --- |
| **Azure AD Connect eşitleme temelleri** | |
| [Mimariyi anlama](concept-azure-ad-connect-sync-architecture.md) |Eşitleme motoruna yeni olan ve kullanılan mimari ve kullanım koşulları hakkında bilgi edinmek isteyen kişiler için. |
| [Teknik kavramlar](how-to-connect-sync-technical-concepts.md) |Mimari konusunun kısa bir sürümü ve kullanılan terimleri kısaca açıklar. |
| [Azure AD Connect için topolojiler](plan-connect-topologies.md) |Eşitleme altyapısının desteklediği farklı topolojileri ve senaryoları açıklar. |
| **Özel yapılandırma** | |
| [Yükleme Sihirbazı 'nı yeniden çalıştırma](how-to-connect-installation-wizard.md) |Azure AD Connect Yükleme Sihirbazı 'nı yeniden çalıştırdığınızda kullanabileceğiniz seçenekleri açıklar. |
| [Bildirim Temelli Sağlamayı Anlama](concept-azure-ad-connect-sync-declarative-provisioning.md) |Bildirim temelli sağlama adlı yapılandırma modelini açıklar. |
| [Bildirim Temelli Sağlama İfadelerini Anlama](concept-azure-ad-connect-sync-declarative-provisioning-expressions.md) |Bildirime dayalı sağlama içinde kullanılan ifade dili için sözdizimi açıklar. |
| [Varsayılan yapılandırmayı anlama](concept-azure-ad-connect-sync-default-configuration.md) |Hazır olmayan kuralları ve varsayılan yapılandırmayı açıklar. Ayrıca kuralların çalışma dışı senaryolar için birlikte nasıl çalıştığını açıklar. |
| [Kullanıcıları ve kişileri anlama](concept-azure-ad-connect-sync-user-and-contacts.md) |Önceki konu başlığında devam eder ve özellikle çok ormanlı bir ortamda Kullanıcı ve kişilerin yapılandırmasının birlikte nasıl çalıştığını açıklar. |
| [Varsayılan yapılandırmada değişiklik yapma](how-to-connect-sync-change-the-configuration.md) |Öznitelik akışlarda ortak bir yapılandırma değişikliği yapma konusunda size kılavuzluk eder. |
| [Varsayılan yapılandırmanın değiştirilmesine ilişkin en iyi yöntemler](how-to-connect-sync-best-practices-changing-default-configuration.md) |Destek sınırlamaları ve kullanıma hazır yapılandırmada değişiklik yapmak için. |
| [Filtrelemeyi yapılandırma](how-to-connect-sync-configure-filtering.md) |Hangi nesnelerin Azure AD ile eşitlendiğini sınırlandırma ve adım adım bu seçenekleri nasıl yapılandıracağınızı belirleyen farklı seçenekleri açıklar. |
| **Özellikler ve senaryolar** | |
| [Yanlışlıkla silmeleri engelleme](how-to-connect-sync-feature-prevent-accidental-deletes.md) |*Yanlışlıkla silmeleri engelleme* özelliğini ve nasıl yapılandırılacağını açıklar. |
| [Scheduler](how-to-connect-sync-feature-scheduler.md) |Verilerin içeri aktarılması, eşitlenmesi ve dışarı aktarılması için yerleşik Scheduler 'ı açıklar. |
| [Parola karması eşitlemeyi uygulama](how-to-connect-password-hash-synchronization.md) |Parola eşitlemesinin nasıl çalıştığını, nasıl uygulanacağını ve nasıl çalışılacağını ve sorun giderileceğini açıklar. |
| [Cihaz geri yazma](how-to-connect-device-writeback.md) |Azure AD Connect içinde cihaz geri yazma özelliğinin nasıl çalıştığını açıklar. |
| [Dizin genişletmeleri](how-to-connect-sync-feature-directory-extensions.md) |Azure AD şemasının kendi özel nitelikleriyle nasıl genişletileceğini açıklar. |
| [Office 365 PreferredDataLocation](how-to-connect-sync-feature-preferreddatalocation.md) |Kullanıcının Office 365 kaynaklarını kullanıcıyla aynı bölgeye nasıl koyabileceğinizi açıklar. |
| **Eşitleme hizmeti** | |
| [Azure AD Connect eşitleme hizmeti özellikleri](how-to-connect-syncservice-features.md) |Eşitleme hizmeti tarafını ve Azure AD 'de eşitleme ayarlarının nasıl değiştirileceğini açıklar. |
| [Yinelenen öznitelik dayanıklılığı](how-to-connect-syncservice-duplicate-attribute-resiliency.md) |**UserPrincipalName** ve **proxyAddresses** yinelenen öznitelik değerleri dayanıklılığı 'nin nasıl etkinleştirileceğini ve kullanılacağını açıklar. |
| **İşlemler ve Kullanıcı arabirimi** | |
| [Eşitleme Hizmeti Yöneticisi](how-to-connect-sync-service-manager-ui.md) |[İşlemler](how-to-connect-sync-service-manager-ui-operations.md), [Bağlayıcılar](how-to-connect-sync-service-manager-ui-connectors.md), [meta](how-to-connect-sync-service-manager-ui-mvdesigner.md)veri deposu Tasarımcısı ve [metadize arama](how-to-connect-sync-service-manager-ui-mvsearch.md) sekmeleri dahil Synchronization Service Manager Kullanıcı arabirimini açıklar. |
| [İşletimsel görevler ve önemli noktalar](how-to-connect-sync-operations.md) |Olağanüstü durum kurtarma gibi işlem sorunlarını açıklar. |
| **Nasıl Yapılır...** | |
| [Azure AD hesabını sıfırlayın](how-to-connect-azureadaccount.md) |Azure AD Connect eşitlemeden Azure AD 'ye bağlanmak için kullanılan hizmet hesabının kimlik bilgilerini sıfırlama. |
| **Daha fazla bilgi ve başvuru** | |
| [Bağlantı noktaları](reference-connect-ports.md) |Eşitleme altyapısı ve şirket içi dizinleriniz ile Azure AD arasında açmanız gereken bağlantı noktalarını listeler. |
| [Azure Active Directory eşitlenen öznitelikler](reference-connect-sync-attributes-synchronized.md) |Şirket içi AD ve Azure AD arasında eşitlenen tüm öznitelikleri listeler. |
| [İşlevler Başvurusu](reference-connect-sync-functions-reference.md) |Bildirim temelli sağlama içinde kullanılabilen tüm işlevleri listeler. |

## <a name="additional-resources"></a>Ek Kaynaklar
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)
