---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 02/12/2020
ms.author: dacoulte
ms.openlocfilehash: 05f9b653910dcbe39b937c711d0045fd3ee33d4e
ms.sourcegitcommit: b07964632879a077b10f988aa33fa3907cbaaf0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77193114"
---
|Ad |Açıklama |Efekt (ler) |Sürüm |
|---|---|---|---|
|[Kaynak gruplarına etiket ekleme](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Tags/AddTag_ResourceGroup_Modify.json) |Belirtilen etiketi ve değeri, bu etiketin eksik olduğu bir kaynak grubuna ekler veya güncellenir. Varolan kaynak grupları bir düzeltme görevi tetiklenerek düzeltilebilir. Etiket farklı bir değerle varsa, değiştirilmez. |Değiştirebilirler |1.0.0 |
|[Kaynaklara etiket ekleme](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Tags/AddTag_Modify.json) |Bu etiket eksik veya güncelleştirilmiş bir kaynak olduğunda belirtilen etiketi ve değeri ekler. Mevcut kaynaklar bir düzeltme görevi tetiklenerek düzeltilebilir. Etiket farklı bir değerle varsa, değiştirilmez. Kaynak gruplarındaki etiketleri değiştirmez. |Değiştirebilirler |1.0.0 |
|[Kaynak gruplarındaki bir etiketi ekleme veya değiştirme](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Tags/AddOrReplaceTag_ResourceGroup_Modify.json) |Herhangi bir kaynak grubu oluşturulduğunda veya güncelleştirilirken belirtilen etiketi ve değeri ekler veya değiştirir. Varolan kaynak grupları bir düzeltme görevi tetiklenerek düzeltilebilir. |Değiştirebilirler |1.0.0 |
|[Kaynaklardaki bir etiketi ekleme veya değiştirme](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Tags/AddOrReplaceTag_Modify.json) |Herhangi bir kaynak oluşturulduğunda veya güncelleştirilirken belirtilen etiketi ve değeri ekler veya değiştirir. Mevcut kaynaklar bir düzeltme görevi tetiklenerek düzeltilebilir. Kaynak gruplarındaki etiketleri değiştirmez. |Değiştirebilirler |1.0.0 |
|[Etiketi ve varsayılan değerini ekleyin](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Tags/ApplyTag_Append.json) |Bu etiketin eksik olduğu herhangi bir kaynak oluşturulduğu veya güncelleştirildiği zaman belirtilen etiketi ve değeri ekler. Bu ilke uygulanmadan önce oluşturulan kaynakların etiketlerini değiştirmez, bu kaynaklar değiştirilmez. Kaynak grupları için geçerlidir. Mevcut kaynaklardaki etiketlerin düzeltmesini destekleyen yeni ' Değiştir ' efekt ilkeleri mevcuttur (bkz. https://aka.ms/modifydoc). |ýna |1.0.0 |
|[Etiketi ve varsayılan değerini kaynak gruplarına ekleyin](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Tags/ResourceGroupApplyTag_Append.json) |Bu etiketin eksik olduğu herhangi bir kaynak grubunda oluşturulan veya güncellenen belirtilen etiketi ve değeri ekler. Bu ilke uygulanmadan önce oluşturulan kaynak gruplarının etiketlerini değiştirmez, bu kaynak grupları değiştirilmeyecektir. Mevcut kaynaklardaki etiketlerin düzeltmesini destekleyen yeni ' Değiştir ' efekt ilkeleri mevcuttur (bkz. https://aka.ms/modifydoc). |ýna |1.0.0 |
|[Kaynak grubundaki etiketi ve değerini ekleyin](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Tags/InheritTag_Append.json) |Bu etikete sahip olmayan herhangi bir kaynak oluşturulduğu veya güncelleştirildiği zaman, belirtilen etiketi kaynak grubundaki değeri ile ekler. Bu ilke uygulanmadan önce oluşturulan kaynakların etiketlerini değiştirmez, bu kaynaklar değiştirilmez. Mevcut kaynaklardaki etiketlerin düzeltmesini destekleyen yeni ' Değiştir ' efekt ilkeleri mevcuttur (bkz. https://aka.ms/modifydoc). |ýna |1.0.0 |
|[Kaynak grubundan bir etiketi devralma](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Tags/InheritTag_AddOrReplace_Modify.json) |Herhangi bir kaynak oluşturulduğunda veya güncelleştirilirken belirtilen etiketi ve değeri üst kaynak grubundan ekler veya değiştirir. Mevcut kaynaklar bir düzeltme görevi tetiklenerek düzeltilebilir. |Değiştirebilirler |1.0.0 |
|[Eksikse, kaynak grubundan bir etiketi devralma](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Tags/InheritTag_Add_Modify.json) |Bu etiketin oluşturulduğu veya güncelleştirildiği herhangi bir kaynakta, belirtilen etiketi üst kaynak grubundaki değeri ile ekler. Mevcut kaynaklar bir düzeltme görevi tetiklenerek düzeltilebilir. Etiket farklı bir değerle varsa, değiştirilmez. |Değiştirebilirler |1.0.0 |
|[Belirtilen etiketi gerektir](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Tags/RequireTag_Deny.json) |Bir etiketin varlığını zorlar. Kaynak grupları için geçerlidir. |reddedebilir |1.0.0 |
|[Kaynak gruplarında belirtilen etiketi gerektir](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Tags/ResourceGroupRequireTag_Deny.json) |Kaynak gruplarında bir etiketin varlığını zorlar. |reddedebilir |1.0.0 |
|[Etiket ve değerini iste](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Tags/RequireTagAndValue_Deny.json) |Gerekli bir etiketi ve değerini uygular. Kaynak grupları için geçerlidir. |reddedebilir |1.0.0 |
|[Kaynak gruplarında etiket ve değer iste](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Tags/ResourceGroupRequireTagAndValue_Deny.json) |Kaynak gruplarında gerekli bir etiketi ve değerini uygular. |reddedebilir |1.0.0 |
