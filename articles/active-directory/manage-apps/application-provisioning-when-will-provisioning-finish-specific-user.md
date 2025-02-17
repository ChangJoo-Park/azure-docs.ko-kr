---
title: 특정 사용자가 애플리케이션에 액세스할 수 있는 시점 파악 | Microsoft Docs
description: 매우 중요한 사용자가 Azure AD를 사용하여 사용자를 프로비저닝하도록 구성한 애플리케이션에 액세스할 수 있는 시기를 찾는 방법
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 08a78a311771395f7c2113f03d5137d59999d7a7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65783861"
---
# <a name="find-out-when-a-specific-user-will-be-able-to-access-an-application"></a>특정 사용자가 애플리케이션에 액세스할 수 있는 시점 파악
애플리케이션에서 자동 사용자 프로비저닝을 사용하는 경우 Azure AD는 정기적으로 예약된 시간 간격(일반적으로 10분 간격)으로 [사용자 및 그룹 할당](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)과 같은 항목을 기반으로 앱에서 사용자 계정을 자동으로 프로비저닝하고 업데이트합니다.

## <a name="how-long-does-it-take"></a>소요 시간

지정된 사용자를 프로비저닝하는 데 걸리는 시간은 주로 초기 "전체" 동기화가 이미 발생했는지 여부에 따라 달라집니다.

Azure AD와 앱 간의 첫 번째 동기화는 Azure AD 디렉터리의 크기와 프로비저닝 범위에 있는 사용자 수에 따라 20분에서 몇 시간이 걸릴 수 있습니다. 

프로비저닝 서비스는 초기 동기화 후에 두 시스템의 상태를 나타내는 워터마크를 저장하므로, 초기 동기화 이후의 후속 동기화가 빨라져(예: 10분 이내) 후속 동기화의 성능이 향상됩니다.

## <a name="how-to-check-the-status-of-a-user"></a>사용자의 상태를 확인하는 방법

선택한 사용자에 대한 프로비저닝 상태를 보려면 Azure AD의 감사 로그를 참조하세요.

프로비저 감사 로그는 Azure Portal의 **Azure Active Directory &gt; 엔터프라이즈 앱 &gt;\[애플리케이션 이름\]&gt; 감사 로그** 탭에서 액세스할 수 있습니다. **계정 프로비전** 범주의 로그를 필터링하여 해당 앱의 프로비전 이벤트만 볼 수 있습니다. 특성 매핑에서 사용자에 대해 구성된 “일치 ID”를 기반으로 사용자를 검색할 수 있습니다. 

예를 들어 Azure AD 측에서 "사용자 계정 이름" 또는 "메일 주소"를 일치하는 특성으로 구성하고 프로비저닝하지 않는 사용자의 값이 "audrey@contoso.com"인 경우 감사 로그에서 "audrey@contoso.com"을 검색한 후 반환된 항목을 검토합니다.

프로비저닝 감사 로그에는 다음을 포함하여 프로비저닝 서비스에서 수행한 모든 작업이 기록됩니다.

* 프로비저닝 범위에 있는 할당된 사용자에 대해 Azure AD 쿼리
* 해당 사용자의 존재에 대해 대상 앱 쿼리
* 시스템 간의 사용자 개체 비교
* 비교를 기반으로 대상 시스템에서 사용자 계정 추가, 업데이트 또는 비활성화

## <a name="next-steps"></a>다음 단계
[Azure Active Directory를 사용하여 SaaS 애플리케이션의 사용자를 자동으로 프로비저닝 및 프로비저닝 해제](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning)''
