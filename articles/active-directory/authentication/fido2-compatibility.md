---
title: FIDO2 parolasız kimlik doğrulamasının tarayıcı desteği | Azure Active Directory
description: Tarayıcılar ve işletim sistemi birleşimleri Azure Active Directory kullanarak uygulamalar için FIDO2 parolasız kimlik doğrulamasını destekler
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 02/02/2021
ms.author: nichola
author: knicholasa
manager: martinco
ms.reviewer: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3e324ae0fc80bb5990f9cf15901080684086a549
ms.sourcegitcommit: 227b9a1c120cd01f7a39479f20f883e75d86f062
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2021
ms.locfileid: "100652250"
---
# <a name="browser-support-of-fido2-passwordless-authentication"></a>FIDO2 passwordless kimlik doğrulamasının tarayıcı desteği

Azure Active Directory, [FIDO2 güvenlik anahtarlarının](https://docs.microsoft.com/azure/active-directory/authentication/concept-authentication-passwordless#fido2-security-keys) parolasız bir cihaz olarak kullanılmasına izin verir. Microsoft hesapları için FIDO2 kimlik doğrulamasının kullanılabilirliği [2018 ' de duyurulmuştur](https://techcommunity.microsoft.com/t5/identity-standards-blog/all-about-fido2-ctap2-and-webauthn/ba-p/288910). Duyuruda anlatıldığı gibi, Microsoft ve Azure Active Directory hesaplarıyla güvenli kimlik doğrulamayı desteklemek için FIDO2 CTAP belirtimine yönelik bazı isteğe bağlı özellikler ve uzantılar uygulanmalıdır. Aşağıdaki diyagramda, Azure Active Directory ile FIDO2 kimlik doğrulama anahtarlarını kullanarak parolasız kimlik doğrulamasını destekleyen tarayıcıların ve işletim sistemi birleşimleri gösterilmektedir.

## <a name="supported-browsers"></a>Desteklenen tarayıcılar

Bu tabloda Azure Active Directory (Azure AD) ve Microsoft hesaplarının (MSA) kimlik doğrulaması desteği gösterilmektedir. Microsoft hesapları, müşteriler tarafından Xbox, Skype veya Outlook.com gibi hizmetler için oluşturulur. Desteklenen cihaz türleri arasında **USB**, yakın alan Iletişimi (**NFC**) ve Bluetooth düşük enerji (**ble**) bulunur.

|  | Chrome |  |  | Edge |  |  | Firefox |  |  |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| | USB | NFC | Aya | USB | NFC | Aya | USB | NFC | Aya |
| **Windows**  | ![Chrome, AAD için Windows için USB hesaplarını destekler.][y] | ![Chrome, AAD için Windows hesaplarında NFC 'yi destekler.][y] | ![Chrome, AAD hesaplarında Windows üzerinde BLE 'ı destekler.][y] | ![Edge, AAD için Windows hesaplarında USB 'yi destekler.][y] | ![Edge, AAD için Windows hesaplarında NFC 'yi destekler.][y] | ![Edge, AAD hesaplarında Windows üzerinde BLE 'ı destekler.][y] | ![Firefox, AAD için Windows için USB hesaplarını destekler.][y] | ![Firefox, AAD için Windows hesaplarında NFC 'yi destekler.][y] | ![Firefox, AAD hesaplarında Windows üzerinde BLE 'ı destekler.][y] |
| **macOS**  | ![Chrome, AAD hesapları için macOS 'ta USB 'yi destekler.][y] | ![Chrome, AAD hesapları için macOS 'ta NFC 'yi desteklemez.][n] | ![Chrome, AAD hesapları için macOS üzerinde BLE 'ı desteklemez.][n] | ![Edge, AAD hesapları için macOS 'ta USB 'yi destekler.][y] | ![Edge, AAD hesapları için macOS üzerinde NFC 'yi desteklemez.][n] | ![Edge, AAD hesapları için macOS üzerinde BLE 'ı desteklemiyor.][n] | ![Firefox, AAD hesapları için macOS üzerinde USB 'yi desteklemez.][n] | ![Firefox, AAD hesapları için macOS üzerinde NFC 'yi desteklemez.][n] | ![Firefox, AAD hesapları için macOS üzerinde BLE 'ı desteklemez.][n] |
| **Linux**  | ![Chrome, AAD hesapları için Linux üzerinde USB 'yi destekler.][y] | ![Chrome, AAD hesapları için Linux üzerinde NFC 'yi desteklemez.][n] | ![Chrome, AAD hesapları için Linux üzerinde BLE 'ı desteklemez.][n] | ![Edge, AAD hesapları için Linux üzerinde USB 'yi desteklemez.][n] | ![Edge, AAD hesapları için Linux üzerinde NFC 'yi desteklemez.][n] | ![Edge, AAD hesapları için Linux üzerinde BLE 'ı desteklemez.][n] | ![Firefox, AAD hesapları için Linux üzerinde USB 'yi desteklemez.][n] | ![Firefox, AAD hesapları için Linux üzerinde NFC 'yi desteklemez.][n] | ![Firefox, AAD hesapları için Linux üzerinde BLE 'ı desteklemez.][n] |

## <a name="operating-system-versions-tested"></a>Test edilen işletim sistemi sürümleri

Yukarıdaki tablodaki bilgiler, aşağıdaki işletim sistemi sürümleri için test edilmiştir.

| İşletim sistemi | En son sınanan sürüm |
| --- | --- |
| Windows | Windows 10 20H2 |
| Mac OS | OS X 11 Big Sur |
| Linux | Fedora 32 Iş Istasyonu |

## <a name="next-steps"></a>Sonraki adımlar
[Passwordless güvenlik anahtarı oturumunu etkinleştir (Önizleme)](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-passwordless-security-key)

<!--Image references-->
[y]: ./media/fido2-compatibility/yes.png
[n]: ./media/fido2-compatibility/no.png
