---
layout: default
type: tech
date: 2023-11-30
category: story
title: Typescript에서 API에 대한 타입선언
subtitle: 안녕하세요. 리체에서 산업기능요원으로 복무를 하고 있는 개발자 김선진입니다. 병특을 진행하면서 겪을 수 있는 병특과 일들에 대해 소개 하려고 합니다.
member: 212
post-header: true
header-img: /assets/img/liche.png
---

프론트를 개발하면 필수적으로 백엔드와 API를 통해 데이터를 주고받는데, 이때 get이나 post 등 어떤 메소드를 써도 백엔드에 보내는 request, 받을때 response에 대한 타입을 선언해야합니다.

예:

```jsx
interface ConsultRequestType {
	category: string
	contents: string
}

interface ConsultResponseType {
	message: string
	status: number
}

export const useConsultsMutation = () => {
  return useMutation(async (data: ConsultRequestType) => {
    const res = await axios.request<ConsultResponseType>({
      url: `/api/enquiries/consults`,
      method: 'POST',
      data,
    });

    return res.data;
  });
};
```

하지만 위의 타입들은 이미 데이터를 가지고있는 백엔드에 이미 선언되어있는 경우가 많습니다. (~~루비나 자바스크립트로만 구성된 백엔드는 예외..~~)

그렇기 때문에 프론트 구성만 보면 중복된 코드가 아니지만 서비스 전체로보면 이것은 중복된 타입선언 코드이고, 불필요한 코드작성일 수 있습니다.

또한 API에 대한 명세가 바뀌었을때는 백엔드,프론트 모두 기능에 관한 코드를 수정하는것은 일반적이지만 타입에대한 부분도 모두 변경하는것은 유지보수성에도 좋지않기때문에 이와같은 중복 타입선언을 피하는 방법은 크게 두가지가 있습니다.

1. **Swagger를 이용하여 타입 추출**

https://medium.com/@youry.stancatte/generating-typescript-interfaces-from-swagger-1910cc7a726a

swagger에는 모든 전체 API명세에 대한 json을 제공하는 기능이있습니다.

(nest에서는 http://localhost:19401/docs로 swagger로 접근할 수 있다면 http://localhost:19401/docs-json 로 접근했을때 모든 API에 대한 명세를 얻습니다.)

이 기능을 이용하여 npm인 openapi-typescript을 설치하고

```jsx
npx openapi-typescript [swagger 전체 json 주소] --output [추출된 타입 파일명]
```

을 입력해주면, swagger에 명세에 대한 type선언 파일이 추출됩니다.

해당 파일의 일부:

```jsx
export interface paths {
  "/pets": {
    /** Returns all pets from the system that the user has access to */
    get: operations["findPets"];
    /** Creates a new pet in the store.  Duplicates are allowed */
    post: operations["addPet"];
  };
  "/pets/{id}": {
    /** Returns a user based on a single ID, if the user does not have access to the pet */
    get: operations["findPetById"];
    /** deletes a single pet based on the ID supplied */
    delete: operations["deletePet"];
  };
}

export interface definitions {
  Pet: definitions["NewPet"] & {
    /** Format: int64 */
    id: number;
  };
  NewPet: {
    name: string;
    tag?: string;
  };
  ErrorModel: {
    /** Format: int32 */
    code: number;
    message: string;
  };
}

....
```

이제 이것을 [위의 블로그](https://medium.com/@youry.stancatte/generating-typescript-interfaces-from-swagger-1910cc7a726a)와 같이 함수로 자동화하거나, import하여 하나하나 매칭하는 방법을 사용해도됩니다.

그리고 API명세가 바뀔때마다 openapi-typescript를 이용하여 다시 API에 대한 타입을 추출하면 프론트는 타입을 재작성할 필요가 없어집니다.

1. **백엔드와 프론트가 같은언어를 사용하고있다면 monorepo를 구성하여 타입공유**

이것은 백엔드와 프론트 모두 같은 언어(Typescript, Rust 등)로 개발된다면, 이것은 yarn이나 Lerna 등을 이용하여 백엔드에서 사용되는 API에 대한 타입을 프론트와 공유하는 방법이 있습니다.

1. **백엔드에서 타입코드를 모아 npm패키지 배포 혹은 프론트 프로젝트에 이동**

https://bit.dev/blog/sharing-types-between-your-frontend-and-backend-applications-l5qih48g/

https://andrejgajdos.com/typescript-type-sharing-between-react-and-node-js/
