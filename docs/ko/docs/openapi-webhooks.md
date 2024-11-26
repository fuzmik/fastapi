# OpenAPI 웹훅(Webhooks)

API **사용자**에게 특정 **이벤트**가 발생할 때 *그들*의 앱(시스템)에 요청을 보내 **알림**을 전달할 수 있다는 것을 알리고 싶은 경우가 있습니다.

즉, 일반적으로 사용자가 API에 요청을 보내는 것과는 반대로, **API**(또는 앱)가 **사용자의 시스템**(그들의 API나 앱)으로 **요청을 보내는** 상황을 의미합니다.

이를 흔히 **웹훅(Webhook)**이라고 부릅니다.

## 웹훅 스텝

**코드에서** 웹훅으로 보낼 메시지, 즉 요청의 **바디(body)**를 정의하는 것이 일반적인 프로세스입니다.

앱에서 해당 요청이나 이벤트를 전송할 **시점**을 정의합니다.

**사용자**는 앱이 해당 요청을 보낼 **URL**을 정의합니다. (예: 웹 대시보드에서 설정)

웹훅의 URL을 등록하는 방법과 이러한 요청을 실제로 전송하는 코드에 대한 모든 로직은 여러분에게 달려 있습니다. 원하는대로 **고유의 코드**를 작성하면 됩니다.

## **FastAPI**와 OpenAPI로 웹훅 문서화하기

**FastAPI**를 사용하여 OpenAPI와 함께 웹훅의 이름, 앱이 보낼 수 있는 HTTP 작업 유형(예: `POST`, `PUT` 등), 그리고 보낼 요청의 **바디**를 정의할 수 있습니다.

이를 통해 사용자가 **웹훅** 요청을 수신할 **API 구현**을 훨씬 쉽게 할 수 있으며, 경우에 따라 사용자 API 코드의 일부를 자동 생성할 수도 있습니다.

/// info

웹훅은 OpenAPI 3.1.0 이상에서 지원되며, FastAPI `0.99.0` 이상 버전에서 사용할 수 있습니다.

///

## 웹훅이 포함된 앱 만들기

**FastAPI** 애플리케이션을 만들 때, `webhooks` 속성을 사용하여 *웹훅*을 정의할 수 있습니다. 이는 `@app.webhooks.post()`와 같은 방식으로 *경로(path) 작업*을 정의하는 것과 비슷합니다.

{* ../../docs_src/openapi_webhooks/tutorial001.py hl[9:13,36:53] *}

이렇게 정의한 웹훅은 **OpenAPI** 스키마와 자동 **문서화 UI**에 표시됩니다.

/// info

`app.webhooks` 객체는 사실 `APIRouter`일 뿐이며, 여러 파일로 앱을 구성할 때 사용하는 것과 동일한 타입입니다.

///

웹훅에서는 실제 **경로(path)** (예: `/items/`)를 선언하지 않는 점에 유의해야 합니다. 여기서 전달하는 텍스트는 **식별자**로, 웹훅의 이름(이벤트 이름)입니다. 예를 들어, `@app.webhooks.post("new-subscription")`에서 웹훅 이름은 `new-subscription`입니다.

이는 실제 **URL 경로**는 **사용자**가 다른 방법(예: 웹 대시보드)을 통해 지정하도록 기대되기 때문입니다.

### 문서 확인하기

이제 앱을 시작하고 <a href="http://127.0.0.1:8000/docs" class="external-link" target="_blank">http://127.0.0.1:8000/docs</a>로 이동해 봅시다.

문서에서 기존 *경로 작업*뿐만 아니라 **웹훅**도 표시된 것을 확인할 수 있습니다:

<img src="/img/tutorial/openapi-webhooks/image01.png">