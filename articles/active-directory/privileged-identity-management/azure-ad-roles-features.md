---
title: Ayrıcalıklı Kimlik Yönetiminde Azure REKLAM rollerini yönetme (PIM) | Microsoft Dokümanlar
description: Atama Ayrıcalıklı Kimlik Yönetimi (PIM) için Azure REKLAM rolleri nasıl yönetilir?
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.subservice: pim
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/06/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: f5d63577cbdbdf18cb17618bdb5c9e3b5de0e44a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79245986"
---
# <a name="management-capabilities-for-azure-ad-roles-in-privileged-identity-management"></a>Ayrıcalıklı Kimlik Yönetiminde Azure AD rolleri için yönetim yetenekleri

Azure AD rollerinin Ayrıcalıklı Kimlik Yönetimi'ndeki yönetim deneyimi, Azure AD rollerinin ve Azure kaynak rollerinin nasıl yönetildiğini birliştirmek için güncelleştirildi. Daha önce, Azure kaynak rolleri için Ayrıcalıklı Kimlik Yönetimi'nin Azure AD rolleri için kullanılamayan birkaç temel özelliği vardı.

Güncelleştirme şu anda kullanıma sunulduğundan, ikisini tek bir yönetim deneyiminde birleştiriyoruz ve bu güncelleştirmede Azure kaynak rolleriyle aynı işlevselliği elde elıyoruz. Bu makalede, güncelleştirilmiş özellikler ve gereksinimleri hakkında bilgi verir.


## <a name="time-bound-assignments"></a>Zamana bağlı atamalar

Azure AD rolleri için Ayrıcalıklı Kimlik Yönetimi'nde daha önce, *uygun* ve *kalıcı*olmak üzere iki olası durumla rol atamalarına aşinaydınız. Artık her atama türü için bir başlangıç ve bitiş saati ayarlayabilirsiniz. Bu ek, bir atama yerleştirebileceğiniz dört olası durum verir:

- Kalıcı olarak uygun
- Etkin kalıcı
- Uygun, atama için belirtilen başlangıç/bitiş tarihleri ile
- Atama için belirtilen başlangıç/bitiş tarihleriyle etkin

Çoğu durumda, kullanıcıların uygun atamaya sahip olmasını ve rolleri her seferinde etkinleştirmesini istemeseniz bile, atamalar için bir son kullanma süresi belirleyerek Azure REKLAM kuruluşunuzu korumaya devam edebilirsiniz. Örneğin, uygun olan bazı geçici kullanıcılarınız varsa, çalışmaları tamamlandığında onları otomatik olarak rol atamasından kaldırmak için bir son kullanma tarihi ayarlamayı düşünün.

## <a name="new-role-settings"></a>Yeni rol ayarları

Azure AD rolleri için yeni ayarlar da ekliyoruz. Daha önce, etkinleştirme ayarlarını yalnızca rol başına göre yapılandırabilirsiniz. Diğer bir noktada, çok faktörlü kimlik doğrulama gereksinimleri ve olay/istek bileti gereksinimleri gibi etkinleştirme ayarları, belirli bir rol için uygun olan tüm kullanıcılara uygulanmıştır. Artık, bir rolü etkinleştirebilmek için tek bir kullanıcının çok faktörlü kimlik doğrulaması gerçekleştirmesi gerekip gerekmediğini yapılandırabilirsiniz. Ayrıca, belirli rollerle ilgili Ayrıcalıklı Kimlik Yönetimi e-postalarınız üzerinde gelişmiş denetime sahip olabilirsiniz.

## <a name="extend-and-renew-assignments"></a>Atamaları genişletme ve yenileme

Zamana bağlı atamayı çözer çözmez sorabileceğiniz ilk soru, bir rolün süresi dolduğunda ne olacağıdır. Bu yeni sürümde, bu senaryo için iki seçenek salıyoruz:

- Genişletme – Bir rol ataması sona ermeye yaklaştığında, kullanıcı bu rol ataması için bir uzantı istemek için Ayrıcalıklı Kimlik Yönetimi'ni kullanabilir
- Yenileme – Bir rol atamasının süresi dolduğunda, kullanıcı bu rol ataması için yenileme istemek için Ayrıcalıklı Kimlik Yönetimi'ni kullanabilir

Her iki kullanıcı tarafından başlatılan eylemler, Global yönetici veya Ayrıcalıklı rol yöneticisinin onayını gerektirir. Yöneticilerin artık bu son kullanma gün lerini yönetme işinde olması gerekmez. Yalnızca uzatma veya yenileme isteklerini beklemeleri ve istek geçerliyse onaylamaları gerekir.

## <a name="api-changes"></a>API değişiklikleri

Müşteriler azure AD kuruluşlarına güncelleştirilmiş sürümü kullanıma aldığında, varolan grafik API'si çalışmayı durdurur. Azure kaynak rolleri [için Grafik API'sini](https://docs.microsoft.com/graph/api/resources/privilegedidentitymanagement-resources?view=graph-rest-beta)kullanmak için geçiş yapmak zorundasınız. Bu API'yi kullanarak Azure `/azureResources` REKLAM `/aadroles` rollerini yönetmek için imzayla `resourceId`değiştirin ve Dizin Kimliğini kullanın.

Önceki API'yi kullanan tüm müşterilere bu değişikliği önceden bildirmek için elimizden geleni yaptık. Azure REKLAM kuruluşunuz yeni sürüme geçtiyse ve hala eski API'ye güveniyorsanız, 'deki pim_preview@microsoft.comekibe ulaşın.

## <a name="powershell-change"></a>PowerShell değişikliği

Azure AD rolleri için Ayrıcalıklı Kimlik Yönetimi PowerShell modüllerini kullanan müşteriler için PowerShell güncelleştirmeyle çalışmayı durdurur. Önceki cmdletler yerine Azure AD Preview PowerShell modülü içindeki Ayrıcalıklı Kimlik Yönetimi cmdletlerini kullanmanız gerekir. [PowerShell Galerisi'nden](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.17)Azure AD PowerShell modüllerini yükleyin. Artık [bu PowerShell modülünde PIM işlemleri için dokümantasyon ve örnekleri okuyabilirsiniz.](powershell-for-azure-ad-roles.md)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD özel rolü atama](azure-ad-custom-roles-assign.md)
- [Azure AD özel rol ataması kaldırma veya güncelleştirme](azure-ad-custom-roles-update-remove.md)
- [Azure AD özel rol ataması yapılandırma](azure-ad-custom-roles-configure.md)
- [Azure AD'de rol tanımları](../users-groups-roles/directory-assign-admin-roles.md)
