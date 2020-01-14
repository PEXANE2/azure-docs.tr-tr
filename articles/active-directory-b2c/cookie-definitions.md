---
title: Tanımlama bilgisi tanımları-Azure Active Directory B2C | Microsoft Docs
description: Azure Active Directory B2C ' de kullanılan tanımlama bilgileri için tanımlar sağlar.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/18/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 66de4559ed006735f53ff993cce29370428b9998
ms.sourcegitcommit: 014e916305e0225512f040543366711e466a9495
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75930902"
---
# <a name="cookies-definitions-for-azure-active-directory-b2c"></a>Azure Active Directory B2C için tanımlama bilgisi tanımları

Aşağıdaki tabloda Azure Active Directory B2C ' de kullanılan tanımlama bilgileri listelenmektedir.

| Ad | Domain | Süre sonu | Amaç |
| ----------- | ------ | -------------------------- | --------- |
| x-MS-cpım-yönetici | main.b2cadmin.ext.azure.com | [Tarayıcı oturumunun](session-behavior.md) sonu | Kiracılar genelinde Kullanıcı üyeliği verilerini tutar. Bir kullanıcının üyesi ve üyelik düzeyi (yönetici veya Kullanıcı) olan kiracılar. |
| x-MS-cpım-dilim | login.microsoftonline.com, b2clogin.com, markalı etki alanı | [Tarayıcı oturumunun](session-behavior.md) sonu | İstekleri uygun üretim örneğine yönlendirmek için kullanılır. |
| x-MS-cpım-Trans | login.microsoftonline.com, b2clogin.com, markalı etki alanı | [Tarayıcı oturumunun](session-behavior.md) sonu | İşlemleri (Azure AD B2C kimlik doğrulama isteklerinin sayısı) ve geçerli işlemi izlemek için kullanılır. |
| x-MS-cpım-SSO: {ID} | login.microsoftonline.com, b2clogin.com, markalı etki alanı | [Tarayıcı oturumunun](session-behavior.md) sonu | SSO oturumunun sürdürülmesi için kullanılır. |
| x-MS-cpim-önbellek: {id} _n | login.microsoftonline.com, b2clogin.com, markalı etki alanı | [Tarayıcı oturumu](session-behavior.md)sonu, başarılı kimlik doğrulama | İstek durumunu korumak için kullanılır. |
| x-MS-cpım-CSRF | login.microsoftonline.com, b2clogin.com, markalı etki alanı | [Tarayıcı oturumunun](session-behavior.md) sonu | CRSF koruması için kullanılan siteler arası Istek forgery belirteci. |
| x-MS-cpım-DC | login.microsoftonline.com, b2clogin.com, markalı etki alanı | [Tarayıcı oturumunun](session-behavior.md) sonu | Azure AD B2C ağ yönlendirme için kullanılır. |
| x-MS-cpım-CTX | login.microsoftonline.com, b2clogin.com, markalı etki alanı | [Tarayıcı oturumunun](session-behavior.md) sonu | Bağlam |
| x-MS-cpım-RP | login.microsoftonline.com, b2clogin.com, markalı etki alanı | [Tarayıcı oturumunun](session-behavior.md) sonu | Kaynak sağlayıcısı kiracının üyelik verilerini depolamak için kullanılır. |
| x-MS-cpım-RC | login.microsoftonline.com, b2clogin.com, markalı etki alanı | [Tarayıcı oturumunun](session-behavior.md) sonu | Geçiş tanımlama bilgisini depolamak için kullanılır. |
