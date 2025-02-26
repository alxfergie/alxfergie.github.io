---
date: 2025-02-09-threejs-interaction
layout: post
title: Three.js와 React Three Fiber, Interaction
subtitle: 'Three.js & React Three Fiber'
description: >-
  InteractionTest.tsx 컴포넌트를 분석하며, React Three Fiber를 사용하여 3D 오브젝트와 상호작용하는 방법
image: 2025-02-09-threejs-interaction/thumb.png
optimized_image: 2025-02-09-threejs-interaction/thumb.png
category: Threejs
tags:
  - Interaction
  - 3D
author: dahyun
paginate: false
---

Three.js를 이용한 3D 플랫폼 개발 개념공부

# InteractionTest.tsx 분석: React Three Fiber에서 3D 오브젝트와 상호작용하기  

## 소개  
이 블로그에서는 `InteractionTest.tsx` 컴포넌트를 분석하며, **React Three Fiber**를 사용하여 3D 오브젝트와 상호작용하는 방법을 쉽게 설명하겠습니다. 사용자가 오브젝트를 클릭하면 어떤 일이 일어나는지, 그리고 **Raycasting**을 활용해 클릭된 오브젝트를 어떻게 감지하는지 알아보겠습니다.  

---

## 상호작용 이해하기  
React Three Fiber에서는 **클릭, 마우스 이동, 호버** 등의 이벤트를 감지할 수 있습니다. **Raycasting**을 사용하면 화면에서 특정 위치를 클릭했을 때 광선을 발사하여 해당 위치에 있는 3D 오브젝트를 찾을 수 있습니다.  

---

## Raycasting이란?  
**Raycasting**은 특정 지점에서 직선으로 빛(광선)을 발사하여 **오브젝트와 충돌하는지 확인하는 기술**입니다. 이를 활용하면 사용자가 마우스로 클릭한 오브젝트를 감지하고 **색상을 변경**하는 등의 기능을 만들 수 있습니다.  

예를 들어, 화면의 오른쪽을 클릭하면 **Raycaster**가 **카메라에서 해당 위치로 광선을 발사**하고, 충돌하는 오브젝트들을 찾습니다.  
- 가장 가까운 오브젝트만 선택할 수도 있고,  
- **모든 충돌한 오브젝트를 리스트로 반환**할 수도 있습니다.  

---

## App.tsx에서의 씬 설정  
`App.tsx` 파일에서는 `@react-three/fiber`의 **Canvas**를 설정하여 3D 씬을 만들고, `InteractionTest` 컴포넌트를 추가합니다.  

![]({{site.url}}/assets/img/2025-02-09-threejs-interaction/interactiontest.png)

## InteractionTest.tsx: 클릭 이벤트 처리

### 1. 클릭 이벤트에 Raycasting 적용하기

클릭 이벤트를 감지하는 `groupClickFunc` 함수:

#### 클릭 이벤트가 실행되는 과정

1. 클릭한 오브젝트의 색상을 초록색으로 변경
   ```tsx
   e.object.material.color = new THREE.Color('green');
   ```
2. Raycaster가 마우스 위치를 기반으로 광선을 발사
3. 광선이 충돌한 오브젝트들을 intersectObject를 통해 찾음.
4. 가장 가까운 오브젝트를 선택하여 색상을 빨간색으로 변경 `(mesh.material.color = new THREE.Color('red')).`

![]({{site.url}}/assets/img/2025-02-09-threejs-interaction/clickfunc.png)

## 2. 3D 오브젝트 그룹 만들기

![]({{site.url}}/assets/img/2025-02-09-threejs-interaction/3d.png)

### 클릭 이벤트에서 이벤트 오브젝트와 실제 클릭된 오브젝트의 차이

#### 이벤트 오브젝트란?
- 클릭 이벤트가 적용된 오브젝트 (onClick 이벤트를 가진 요소)
- `group`에 `onClick`을 적용하면 그룹 전체가 하나의 이벤트 오브젝트가 됨

#### 클릭된 오브젝트란?
- 실제 사용자가 클릭한 대상
- `event.object` 속성을 통해 클릭된 오브젝트를 찾을 수 있음
- `raycaster.intersectObject()`를 사용하여 광선이 감지한 오브젝트 리스트를 확인 가능

#### 클릭 이벤트 동작 방식
1. `group`에 `onClick`을 적용하면 그룹 자체가 이벤트 오브젝트가 됨.
2. Raycasting을 사용해 실제 클릭된 오브젝트를 찾음.
3. 가장 가까운 오브젝트를 선택하여 이벤트 적용 가능.

---

## 결론
`InteractionTest.tsx`는 React Three Fiber에서 Raycasting을 이용하여 3D 오브젝트와 상호작용하는 방법을 보여줍니다. 사용자가 오브젝트를 클릭하면 감지하여 색상을 변경하는 방식으로 동작합니다.

아래 이미지는 이 코드가 실제로 어떻게 동작하는지를 보여줍니다. 사용자가 3D 오브젝트를 클릭하면 Raycasting을 이용해 해당 오브젝트를 감지하고 색상을 변경하는 과정이 실행됩니다.

![]({{site.url}}/assets/img/2025-02-09-threejs-interaction/result.png)

이 기능을 활용하면 다양한 3D 인터랙티브 환경을 구축할 수 있습니다. 앞으로 마우스 오버 효과나 추가적인 인터랙션을 적용하여 더욱 발전시킬 수도 있습니다.

---

### 활용 예시

이 기술은 다양한 3D 애플리케이션에서 활용될 수 있습니다.

- **3D 제품 뷰어**: 클릭하면 색상을 변경하거나 정보를 표시할 수 있음.
- **게임**: 캐릭터나 오브젝트와 상호작용 가능.
- **시뮬레이션**: 물리적 실험이나 공학적 테스트에 활용 가능.

추후 마우스 오버 효과, 애니메이션, 고급 이벤트 핸들링을 추가하여 더욱 발전시킬 수 있습니다.

---


