---
title: Kullanıcı akışları için API Bağlayıcısı kod örnekleri-Azure AD
description: Azure Active Directory dış kimliklere yönelik self servis kaydolma akışlarında API bağlayıcıları için kod örnekleri.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: sample
ms.date: 06/16/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: a3995395cfd6256689bedc7a4a3c83effc65c0b4
ms.sourcegitcommit: 4e5560887b8f10539d7564eedaff4316adb27e2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87905999"
---
# <a name="samples-for-external-identities-self-service-sign-up"></a>Dış kimlikler self servis kaydolma örnekleri

Aşağıdaki tablolar, [API bağlayıcıları](api-connectors-overview.md)kullanarak Self Servis kaydolma Kullanıcı akışlarınızdaki Web API 'lerini kullanmak için kod örneklerine bağlantılar sağlar.

## <a name="api-connector-azure-function-quickstarts"></a>API Bağlayıcısı Azure Işlevi hızlı başlangıç

| Örnek                                                                                                                          | Açıklama                                                                                                                                               |
| ------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [.NET Core](https://github.com/Azure-Samples/active-directory-dotnet-external-identities-api-connector-azure-function-validate) | Bu .NET Core Azure Işlevi örneği, oturum açma işlemlerinin belirli kiracı etki alanlarına nasıl sınırlandırılacağını ve Kullanıcı tarafından belirtilen bilgilerin nasıl doğrulandığını gösterir. |
| [Node.js](https://github.com/Azure-Samples/active-directory-nodejs-external-identities-api-connector-azure-function-validate)   | Bu Node.js Azure Işlevi örneği, oturum açma işlemlerinin belirli kiracı etki alanlarına nasıl sınırlandırılacağını ve Kullanıcı tarafından belirtilen bilgilerin nasıl doğrulandığını gösterir.  |
| [Python](https://github.com/Azure-Samples/active-directory-python-external-identities-api-connector-azure-function-validate)    | Bu Python Azure Işlevi örneği, oturum açma işlemlerinin belirli kiracı etki alanlarına nasıl sınırlandırılacağını ve Kullanıcı tarafından belirtilen bilgilerin nasıl doğrulandığını gösterir.    |

<!-- \| [Java](../../azure-docs-pr/articles/active-directory/b2b/invite-internal-users.md#use-the-invitation-api-to-send-a-b2b-invitation) |  The sample below illustrates how to call the invitation API to invite an internal user as a B2B user. | -->

## <a name="custom-approval-workflows"></a>Özel onay iş akışları

| Örnek | Açıklama |
|--------| ----------- |
| [El ile onay iş akışı](https://github.com/Azure-Samples/active-directory-dotnet-external-identities-api-connectors-approvals) | Bu örnek, self servis kayıt sırasında Konuk Kullanıcı hesabı oluşturmayı yönetmek için uçtan uca onay iş akışını gösterir |

## <a name="identity-verification"></a>Kimlik doğrulama

| Örnek                                                                                                            | Açıklama                                                                                                                          |
| ----------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| [IDology](https://github.com/Azure-Samples/active-directory-dotnet-external-identities-idology-identity-verification) | Bu örnek, IDology ile tümleştirme için bir API Bağlayıcısı kullanarak Self Servis kaydolma 'nın bir parçası olarak bir kullanıcı kimliğini nasıl doğrulayacağınızı gösterir. |
| [Experian](https://github.com/Azure-Samples/active-directory-dotnet-external-identities-experian-identity-verification) | Bu örnek, Experian ile tümleştirme için bir API Bağlayıcısı kullanarak Self Servis kaydolma 'nın bir parçası olarak bir kullanıcı kimliğini nasıl doğrulayacağınızı gösterir. |
